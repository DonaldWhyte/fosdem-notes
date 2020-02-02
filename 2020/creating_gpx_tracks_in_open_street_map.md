# Creating GPX tracks from cycle routes in OpenStreetMap

https://fosdem.org/2020/schedule/event/creating_gpx_tracks_from_cycle_routes_in_openstreetmap/

https://fosdem.org/2020/schedule/event/creating_gpx_tracks_from_cycle_routes_in_openstreetmap/attachments/slides/3886/export/events/attachments/creating_gpx_tracks_from_cycle_routes_in_openstreetmap/slides/3886/Creating_GPX_tracks_from_cycle_routes_in_OpenStreetMaps.pdf

## OpenStreetMap Data Model

Node -- coordinate in GPS space
Way -- ordered seqeunce of nodes
Relation -- relation between ways to describe of route

Many problems with the data when combining multiple ways to form a single, continuous route.

## Solution: OpenCycleRoute

Find all places ways intersect (waypoint)
Build cost matrix
Use ways that minimise cost matrix

```
cost = distance * coefficient
```

coefficient depends on type of road -- country no car path, segargated cycle way, normal road, opposite direction road

The end goal is choose non-opposite roads (e.g. on roundabouts), fill in missing connections in an OpenStreetMap route

## Challenges

Elevation data is tricky to get a hold off, which is important for cycle routes.

Discontinuity issues in the underlying OpenStreetMap data. How to resolve?

https://github.com/hpgmiskin/OpenCycleExport
