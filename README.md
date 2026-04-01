# Enhanced Earthquake Catalog for the December 6, 2025 Mw 7.0 Yukon, Canada Earthquake Sequence

## Dataset Description

This dataset provides an enhanced earthquake catalog for the aftershock sequence following the December 6, 2025 Mw 7.0 earthquake in southwestern Yukon, Canada. The earthquake occurred beneath the glaciated terrain of the St. Elias Mountains, near Mt. King George and Hubbard Glacier, along the hypothesized Fairweather–Totschunda Connector fault (CF). The catalog was constructed using automated machine-learning-based detection, association, and location methods applied to continuous seismic waveform data recorded by 39 broadband stations from the Alaska Regional Network (AK), Alaska Volcano Observatory (AV), and Canadian National Seismograph Network (CN), located within approximately 500 km of the mainshock epicentre.

Seismic phase arrivals (P and S) were detected using the Earthquake Transformer (EQTransformer) deep-learning model, implemented via the SeisBench framework (Woollam et al., 2022; Mousavi et al., 2020), with detection thresholds for P and S waves set at 0.1. Phase association was performed using the PyOcto algorithm (Münchmeyer, 2024), guided by the AK135 1D velocity model, requiring a minimum of 5 P-phases and 5 S-phases per event, with at least 5 stations recording both P and S arrivals. Earthquake locations were refined using the NonLinLoc probabilistic location framework (Lomax et al., 2000) with the CN01 1D velocity model and Equal Differential Time (EDT) likelihood formulation. The catalog spans December 6, 2025 through January 6, 2026 and contains 8,243 earthquakes with magnitudes ranging from approximately 1.2 to 6.8 and depths between approximately 5 and 19 km. A total of 187,025 individual phase picks (103,074 P-wave and 83,951 S-wave arrivals) are included. Full methodological details are provided in the accompanying publication (Brillon et al., submitted to Seismica).

## Citation

If you use this dataset, please cite:

Brillon, C., Gosselin, J. M., Dokht, R. M. H., and Schaeffer, A. J. (submitted). The December 6, 2025 Mw 7.0 earthquake in Yukon, Canada: Assessing routine event data in a remote northern setting. *Seismica*.

## License

This dataset is licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](https://creativecommons.org/licenses/by/4.0/).

You are free to share and adapt this dataset for any purpose, provided appropriate credit is given. This dataset is provided as-is, with no warranties or guarantees from the authors, and no support is implied.

## File Descriptions

The dataset consists of three CSV files described below. The files are linked through shared identifiers: the `id` field in `event.csv` corresponds to the `eventId` field in `phase.csv`, and the `networkCode`/`stationCode` fields in `phase.csv` correspond to the same fields in `station.csv`.

### event.csv

Each row represents a single detected and located earthquake in the enhanced catalog. The file contains 8,243 events.

| Field | Type | Description |
|-------|------|-------------|
| `id` | Integer | Unique event identifier. Links to the `eventId` field in `phase.csv`. |
| `isotime` | String (ISO 8601) | Origin time of the earthquake in UTC (e.g., `2025-12-06T00:56:23.883Z`). |
| `latitude` | Float | Epicentral latitude in decimal degrees (WGS84). |
| `longitude` | Float | Epicentral longitude in decimal degrees (WGS84). |
| `depth` | Float | Hypocentral depth in kilometres below the surface. |
| `magnitude` | Float | Estimated earthquake magnitude. |

### station.csv

Each row represents one of the 39 seismic stations used to construct the catalog.

| Field | Type | Description |
|-------|------|-------------|
| `latitude` | Float | Station latitude in decimal degrees (WGS84). |
| `longitude` | Float | Station longitude in decimal degrees (WGS84). |
| `elevation` | Float | Station elevation in metres above sea level. |
| `networkCode` | String | FDSN network code (e.g., `AK`, `AV`, `CN`). Together with `stationCode`, uniquely identifies a station. |
| `stationCode` | String | FDSN station code (e.g., `BRWY`, `WHY`, `BAGL`). |
| `locationCode` | String | FDSN location code. Empty for most stations in this dataset. |

### phase.csv

Each row represents a single seismic phase pick (P-wave or S-wave arrival) associated with an event. The file contains 187,025 picks.

| Field | Type | Description |
|-------|------|-------------|
| `eventId` | Integer | Event identifier linking this pick to the corresponding event in `event.csv`. |
| `isotime` | String (ISO 8601) | Arrival time of the phase pick in UTC. |
| `lowerUncertainty` | Float | Lower uncertainty bound on the pick time, in seconds. |
| `upperUncertainty` | Float | Upper uncertainty bound on the pick time, in seconds. |
| `type` | String | Phase type: `P` for P-wave arrival or `S` for S-wave arrival. |
| `networkCode` | String | FDSN network code of the recording station. Links to `station.csv`. |
| `stationCode` | String | FDSN station code of the recording station. Links to `station.csv`. |
| `locationCode` | String | FDSN location code of the recording station. |
| `channelCode` | String | FDSN channel code indicating the instrument and component (e.g., `BHZ` for broadband high-gain vertical, `BHE` for broadband high-gain east, `HHZ` for high broadband vertical, `HHE` for high broadband east). P-wave picks are typically on vertical components (`BHZ`, `HHZ`) and S-wave picks on horizontal components (`BHE`, `HHE`). |
| `evalMode` | String | Evaluation mode of the pick. All picks in this catalog are `automatic`, generated by the EQTransformer model. |

## File Relationships

```
station.csv                    event.csv
(networkCode, stationCode) <-- (id)
         |                        |
         |                        |
         +-------> phase.csv <----+
          (networkCode,     (eventId)
           stationCode)
```

Each phase pick in `phase.csv` is linked to exactly one event via the `eventId` field and to exactly one station via the combination of `networkCode` and `stationCode`. A typical event has picks from multiple stations, and a typical station has picks associated with many events.

## References

- Lomax, A., Virieux, J., Volant, P., and Berge-Thierry, C. (2000). Probabilistic earthquake location in 3D and layered models. In *Advances in seismic event location*, pp. 101–134. Springer.
- Mousavi, S. M., Ellsworth, W. L., Zhu, W., Chuang, L. Y., and Beroza, G. C. (2020). Earthquake transformer—an attentive deep-learning model for simultaneous earthquake detection and phase picking. *Nature Communications*, 11(1), 3952.
- Münchmeyer, J. (2024). PyOcto: A high-throughput seismic phase associator. *Seismica*, 3(1).
- Woollam, J., et al. (2022). SeisBench—A toolbox for machine learning in seismology. *Seismological Society of America*, 93(3), 1695–1709.
