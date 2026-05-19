---
title: 'SharpSim — Features'
description: 'What SharpSim can do today.'
pubDate: 2026-05-19
tag: 'Features'
navLabel: 'Features'
navOrder: 3
---

A growing list of components built on SharpSim's small core.

## Pluggable event calendars

Pick the `IEventList` implementation that fits your workload — see [EventCalendar](/projects/sharpsim/features/event-calendar/) for benchmarks and trade-offs.

## Strongly typed time

`SimTime` arithmetic prevents the usual "seconds or milliseconds?" bugs.

## Reusable simulation nodes

`SimNode<TSim, THistory>` gives entities a place to keep state and react to events without inheriting policy from the framework.

## Roadmap

- A first-class statistics module
- Built-in distributions
- Replay & checkpointing
