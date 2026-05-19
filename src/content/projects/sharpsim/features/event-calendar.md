---
title: 'SharpSim — EventCalendar'
description: 'Pluggable IEventList implementations for the pending event set.'
pubDate: 2026-05-19
tag: 'Feature'
navLabel: 'EventCalendar'
navOrder: 0
---

The pending event set is the hot loop of any discrete-event simulator. SharpSim exposes `IEventList` so you can pick the implementation that fits your workload — or plug in your own.

| Implementation | Insert | Pop min | Best for |
| --- | --- | --- | --- |
| `EventList` | O(n) | O(1) | Small queues, reference / debugging |
| `MListEventList` | O(1) amortized | O(bucket size) | General-purpose DES |
| `MultiTierMListEventList` | O(1) amortized | O(bucket size) | Large queues with wide or multimodal time scales |

`MListEventList` implements the two-tier pending-event-set from Goh & Thng (2003): `N` unsorted time-bucket sub-lists plus a sorted *current list* that drains one bucket at a time. The bucket width is set from a sample-based estimate of the average inter-event gap,

```
W = α · (t_max − t_min) / (n − 1),    α = 2
```

`MultiTierMListEventList` extends this with a Ladder-Queue-style hierarchy: when a coarse bucket would contain more than a threshold of events, it is recursively subdivided into a finer tier on demand.

## Benchmarks

Apple Silicon, .NET 8, Release. Times include both inserts and pops.

### Fill-then-Drain

Insert N random events, then drain all:

| N | `EventList` | `MListEventList` | `MultiTierMListEventList` |
| ---: | ---: | ---: | ---: |
| 1,000   |     0.44 ms |   0.26 ms |   0.32 ms |
| 10,000  |    14.22 ms |   5.92 ms |   **3.46 ms** |
| 100,000 | 1,485.65 ms | 125.64 ms | **112.79 ms** |

`EventList` is effectively O(n²) overall — each sorted insert shifts the list tail. The MList variants stay roughly linear: **~13× faster** at 100k events.

### Hold-model (DES-style)

Keep `hold` in-flight events, run `steps` pop-then-schedule cycles:

| steps   | hold   | `EventList` | `MListEventList` | `MultiTierMListEventList` |
| ---:    | ---:   | ---:        | ---:             | ---: |
| 100,000 |    100 |    17.77 ms |  **9.33 ms**     | 12.70 ms |
| 100,000 |  1,000 |    41.04 ms | **17.83 ms**     | 24.35 ms |
| 100,000 | 10,000 |   259.93 ms |    29.77 ms      | **21.30 ms** |

For small in-flight populations the two-tier `MListEventList` wins outright. Once the working set is large enough that individual buckets balloon (here around 10k), the multi-tier version's on-demand subdivision pays off.

## Picking one

```csharp
// Reference / debugging — deterministic and easy to inspect
var sim = new Simulation(new EventList(), history);

// General-purpose default for larger simulations
var sim = new Simulation(new MListEventList(), history);

// Wide or multimodal time scales, large working sets
var sim = new Simulation(new MultiTierMListEventList(), history);
```

All three are drop-in `IEventList` implementations; the rest of the simulator is unchanged.
