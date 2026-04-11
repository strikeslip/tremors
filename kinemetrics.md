# Kinemetrics Structural Health Monitoring (SMH) Reference
## TROJAN TREMORS / SOS Archive

---

## Kinemetrics — Company & Platform

- **Kinemetrics homepage:** https://kinemetrics.com/
- **Founded:** 1969, Pasadena, CA — global market leader in earthquake monitoring instrumentation
- **Structural Health Monitoring (SHM) overview:** https://kinemetrics.com/application/structural-health-monitoring/
- **OASIS platform (pre-OasisPlus):** https://kinemetrics.com/post_products/oasis/
- **OasisPlus platform:** https://kinemetrics.com/application/oasis/ | https://oasisplus.kmi.com/
- **OasisPlus launch announcement (BusinessWire, 2019):** https://www.businesswire.com/news/home/20190515005267/en/Kinemetrics-Launches-OasisPlus-to-Provide-Preparedness-Situational-Awareness-Real-Time-Response-for-Hospitals-Before-During-After-Earthquakes
- **OasisPlus PBEE/SHM technical paper (PDF):** https://kinemetrics.com/wp-content/uploads/2018/04/Earthquake-Business-Continuity-Abstract.pdf
- **Kinemetrics webinars (instrumentation):** https://kinemetrics.com/taxonomy_webinars/instrumentation/

**OasisPlus** is an earthquake business continuity and SHM platform providing real-time structural data, SAFE Reports (post-event building assessment), occupant check-in via mobile app, and performance-based engineering analyses guided by ASCE 41 thresholds. Notable deployments: Burj Khalifa, Dubai World Trade Center, Seattle Children's Hospital.

---

## Kinemetrics Hardware

### EpiSensor Accelerometer

- **EpiSensor ES-T (triaxial):** https://kinemetrics.com/post_products/episensor-es-t/
- **EpiSensor ES-U2 (uniaxial):** https://kinemetrics.com/post_products/episensor-es-u2/
- **EarthScope PASSCAL instrument reference:** https://www.passcal.nmt.edu/content/instrumentation/sensors/accelerometers/kinemetrics-accelerometer

Key specs:
- Full-scale range: ±0.25g to ±4g (user selectable)
- Frequency response: DC to 200 Hz
- Dynamic range: 155 dB+
- Output: ±2.5V to ±20V differential

### Obsidian Datalogger

- **Obsidian product page:** https://kinemetrics.com/post_products/obsidian/
- **Obsidian datasheet (PDF):** https://kinemetrics.com/wp-content/uploads/2017/04/datasheet-obsidian-accelerograph-kinemetrics.pdf
- **ETNA 2 datasheet (PDF):** https://kinemetrics.com/wp-content/uploads/2017/04/datasheet-etna2-accelerograph-kinemetrics.pdf
- **EarthScope/NSF SAGE NRL entry:** https://ds.iris.edu/ds/nrl/datalogger/kinemetrics/obsidian/

Key specs:
- 3+1 channels (internal triaxial EpiSensor deck)
- ADC: 24-bit Delta-Sigma per channel, simultaneous sampling
- Anti-alias: Double Precision FIR (>140 dB attenuation at Nyquist)
- Sampling rates: 1, 10, 20, 50, 100, 200, 250, 500, 1000, 2000, 5000 sps
- Dynamic range: ~127–130 dB (RMS at 200/100 sps)
- Output formats: MiniSEED, EVT, ASCII
- Security: SSH, SSL, OpenVPN
- EEWS-ready: 1s and 0.1s data packets

---

## David Geffen Galleries — Seismic Profile

- **LACMA "Earthquakes, Sliders and Art" (Unframed blog):** https://unframed.lacma.org/2022/02/16/earthquakes-sliders-and-art
- **Clark Construction seismic technology article:** https://www.clarkconstruction.com/news/seismic-technology-helps-lacma-team-protect-people-and-art
- **Earthquake Protection Systems products page:** https://www.earthquakeprotection.com/products

**Building specs:**
- Architect: Atelier Peter Zumthor | Structural Engineer: SOM | GC: Clark Construction
- 56 triple-friction pendulum isolators (EPS, manufactured in Vallejo, CA)
  - 40 primary isolators (9+ ft diameter, 40,000 lb each; 50,000 lb with top plate)
  - 16 secondary isolators (10,000 lb each)
  - Maximum lateral displacement: ±5 feet in any direction
- Façade dampers: Seele (glass panel vibration damping)
- Isolation period designed across three "gears": moderate EQ (slider only) → M8.0 San Andreas (full dish engagement)
- Building code mandates seismic instrumentation for all base-isolated LA structures (CSMIP Network CE)

**Monitoring note:** Kinemetrics is the dominant CSMIP instrumentation provider in Southern California. The Geffen Galleries monitoring system has not been officially named in public project releases. OasisPlus is the probable candidate for SHM data layer.

---

## CSMIP — California Strong Motion Instrumentation Program

- **CSMIP program page (CA Dept. of Conservation):** https://www.conservation.ca.gov/cgs/smi/program
- **CESMD (Center for Engineering Strong Motion Data):** https://www.strongmotioncenter.org/
- **CESMD data attribution/use policy:** https://www.strongmotioncenter.org/NCESMD/reports/CESMD_DataPolicy.htm
- **CESMD FAQ:** https://www.strongmotioncenter.org/NCESMD/reports/FAQ.pdf
- **COSMOS data centers directory:** https://www.strongmotion.org/data-centers
- **USGS CESMD report (2022):** https://pubs.usgs.gov/publication/70239434
- **USGS CESMD new developments (2024):** https://pubs.usgs.gov/publication/70256974
- **CSMIP structure instrumentation overview (SpringerLink):** https://link.springer.com/chapter/10.1007/978-94-010-0696-5_2
- **SMIP22 Seminar Proceedings (PDF):** https://www.strongmotioncenter.org/smip_proceedings/smip22/SMIP22_Proceedings.pdf

**Program facts:**
- Established 1972 (post-San Fernando earthquake)
- Managed by California Geological Survey (CGS)
- ~1,400 stations, 10,000+ sensors statewide
- Funded via local building permit fees
- Network code: **CE**
- Data transmitted to CGS HQ within minutes of event
- Annual SMIP Seminars transfer research to engineering practice
- Base-isolated buildings: sensors placed above AND below isolators — quantitatively measures absorbed energy

---

## Kinemetrics Data Pipeline

### 1. Signal Capture
EpiSensor converts ground acceleration → analog voltage (microvolt-level). Signal conditioned/amplified before digitization.

### 2. Digitization (Delta-Sigma ADC)
- 24-bit Delta-Sigma converter per channel
- Internal oversampling at hundreds of kHz
- FIR anti-aliasing filter (>140 dB attenuation at output Nyquist)
- Dynamic range: 130–155 dB

### 3. Time-Tagging
- GPS / PTP (Precision Time Protocol) synchronization
- TCXO (Temperature Compensated Crystal Oscillator) phase-locked
- Sub-microsecond accuracy per sample

### 4. Data Packaging & Compression
- STEIM-1 or STEIM-2 applied (see below)
- Output: MiniSEED (archiving/streaming) or EVT (event-triggered, rich metadata)

### 5. SHM Metrics (OasisPlus)
- Peak Ground Acceleration (PGA)
- Inter-story Drift (floor-to-floor displacement)
- Spectral Response (frequency content, isolator detuning verification)
- Velocity and Displacement (integration of acceleration waveforms)

---

## Sampling Rates

| Application | Rate | Target | Key Metric |
|---|---|---|---|
| Structural (buildings) | 100–200 sps | High-frequency shaking | Inter-story drift, PGA |
| Global seismology | 1–40 sps | Low-frequency rolling | Arrival times, magnitude |
| Kinemetrics max (Obsidian) | 5,000 sps | Blast/mechanical | Specialized |

**Nyquist rule:** Sample rate must be >= 2x the highest frequency of interest. 100 sps reliable capture to 50 Hz.

---

## MiniSEED Format

### Specification & History

- **SEED Reference Manual v2.4 (FDSN, PDF):** http://www.fdsn.org/pdf/SEEDManual_V2.4.pdf
- **FDSN SEED format overview (EarthScope/IRIS):** https://ds.iris.edu/ds/nodes/dmc/data/formats/seed/
- **FDSN homepage:** https://www.fdsn.org/

**Timeline:**
- 1987: FDSN adopts SEED v2.0 (design by USGS/Ray Buland)
- 1992: SEED v2.2 splits into *Dataless SEED* (metadata) + *miniSEED* (waveforms only)
- 1984–present: STEIM compression becomes de facto waveform encoding
- 2016–present: miniSEED 3 development begins

### miniSEED 2.4 Structure

Each file = concatenation of fixed-length **Data Records** (512 or 4,096 bytes).

**Fixed Header (48 bytes):**
- Sequence number (6-digit, incrementing)
- Quality indicator: `D` (Data) / `R` (Raw) / `Q` (Quality controlled)
- Station ID (1–5 chars, e.g. `LACMA`)
- Location ID (2-char, e.g. `00`)
- Channel ID (3-char SEED convention, e.g. `HNZ` = High-gain accelerometer, Normal rate, Vertical)
- Network code (1–2 char, e.g. `CE` = CSMIP)
- Start time (precision: 0.0001s)

**Blockette 1000 (Data Only Blockette):**
- Data encoding (STEIM-1 = Type 10, STEIM-2 = Type 11)
- Byte order (Big/Little Endian)
- Record length (as power of 2)

**Data Payload:**
- Integer differences (counts), NOT physical units
- Physical conversion requires external **StationXML** metadata (sensitivity/gain)

### miniSEED vs. EVT

| Feature | Kinemetrics EVT | MiniSEED |
|---|---|---|
| Primary Use | Post-event engineering | Archive & streaming |
| Metadata | Built-in (sensors, building info) | Minimal (network, station, channel) |
| Compression | Uncompressed/proprietary | STEIM-1/2 |
| Mode | Event-triggered | Continuous or triggered |

### miniSEED 3

- **FDSN miniSEED 3 spec (overview):** https://docs.fdsn.org/projects/miniseed3
- **miniSEED 3 record definition:** https://docs.fdsn.org/projects/miniseed3/en/latest/definition.html
- **miniSEED 3 data encodings:** https://docs.fdsn.org/projects/miniseed3/en/latest/data-encodings.html
- **miniSEED 3 changelog:** https://docs.fdsn.org/projects/miniseed3/en/latest/changes.html
- **miniSEED 3 appendix (2.4 to 3 field mapping):** https://docs.fdsn.org/projects/miniseed3/en/latest/appendix.html

Key improvements over 2.4:
- **JSON-based Extra Headers** (replaces blockette system) — arbitrary metadata in-record
- **Nanosecond time resolution** (up from microseconds)
- **Variable record length** (no longer powers-of-2 restriction; up to 4 GB)
- **URI-based Source Identifiers** (replaces fixed 4-part NSLC codes)
- **CRC-32C integrity field** in every record header

---

## libmseed — MiniSEED Library

- **GitHub repo (EarthScope):** https://github.com/EarthScope/libmseed
- **Documentation:** https://earthscope.github.io/libmseed/
- **Capabilities overview:** https://earthscope.github.io/libmseed/page-capabilities.html
- **Releases:** https://github.com/earthscope/libmseed/releases

Maintained by Chad Trabant, EarthScope Data Services. Apache 2.0 licensed. Supports read/write of both miniSEED 2 and miniSEED 3; format auto-detected on read. The canonical C library for all miniSEED tooling.

---

## StationXML — Metadata Format

- **FDSN StationXML schema page:** https://www.fdsn.org/xml/station/
- **StationXML documentation (v1.2):** https://docs.fdsn.org/projects/stationxml/en/latest/index.html
- **StationXML overview:** https://docs.fdsn.org/projects/stationxml/en/latest/overview.html
- **GitHub schema repo:** https://github.com/FDSN/StationXML

StationXML is the modern replacement for Dataless SEED volumes. Provides sensitivity (gain), instrument response, geographic coordinates, and other metadata required to convert miniSEED integer counts into physical units. Retrieved separately from a data center (e.g. via `fdsnws-station` web service).

---

## EarthScope / FDSN Web Services (Data Access)

- **EarthScope homepage:** https://www.earthscope.org/
- **fdsnws-dataselect service documentation:** https://service.earthscope.org/fdsnws/dataselect/1/
- **fdsnws-dataselect migration notice (iris.edu to earthscope.org):** https://www.earthscope.org/news/earthscope-fdsnws-dataselect-service-has-moved-as-part-of-cloud-transition/
- **Query builder tool:** https://service.earthscope.org/fdsnws/dataselect/docs/1/builder/
- **USGS earthquake event API:** https://earthquake.usgs.gov/fdsnws/event/1/

**Primary SOS data endpoint pattern:**
```
https://service.earthscope.org/fdsnws/dataselect/1/query?net=CE&sta=[STATION]&loc=00&cha=HN[Z/1/2]&start=[ISO]&end=[ISO]
```
Network `CE` = CSMIP (California Strong Motion Instrumentation Program).
Note: `service.iris.edu` now redirects (HTTP 307) to `service.earthscope.org` — update all hardcoded URLs.

---

## SeedLink — Real-Time Streaming

- **EarthScope/IRIS SeedLink overview:** https://ds.iris.edu/ds/nodes/dmc/services/seedlink/
- **SeisComP SeedLink documentation:** https://www.seiscomp.de/doc/apps/seedlink.html
- **ringserver (SeedLink server config guide):** https://seiscode.iris.washington.edu/projects/ringserver/wiki/How_to_configure_ringserver_as_a_SeedLink_streaming_server

**Protocol summary:**
- TCP-based client-server protocol
- Handshake phase: client subscribes to NSLC-coded streams via ASCII commands
- Continuous FIFO data stream: 8-byte header (sequence number) + 512-byte miniSEED record per packet
- Robust: clients can disconnect/reconnect without data loss (within server ring buffer window)
- Used for real-time earthquake early warning (ShakeAlert) and continuous ambient monitoring
- Default TCP port: 18000

---

## STEIM Compression

Developed by **Dr. Joseph Steim** (1984). First-difference algorithm: stores delta between successive samples rather than full values. Efficient because seismic waveforms are relatively slowly changing signals.

| Version | Year | Notes |
|---|---|---|
| STEIM-1 | 1984 | Groups of 8/16/32-bit differences. Simpler, robust. SEED Encoding Type 10 |
| STEIM-2 | ~1990s | 5/6-bit packing options. Higher compression. SEED Encoding Type 11. Production standard |
| STEIM-3 | 1990 (proposed) | Bit-spanning across 32-bit words; rarely deployed commercially |

**Kinemetrics connection:**
- Dr. Steim founded Quanterra (1987) → acquired by Kinemetrics (1999)
- All Kinemetrics hardware (Q330, Obsidian, Granite, ETNA 2) defaults to STEIM-2 output
- STEIM-2 remains the FDSN de facto standard (Encoding Type 11)

**Important SOS note:** During large earthquakes, sample-to-sample differences grow dramatically → STEIM compression efficiency drops → data surges. Network buffers and ingest pipelines must be sized to handle burst traffic.

---

## SOS / TROJAN TREMORS Integration Notes

### Data Sourcing
- SOS pulls MiniSEED streams via EarthScope `fdsnws-dataselect`
- Kinemetrics Obsidian dataloggers (CSMIP network `CE`) are primary capture hardware for LA structures feeding FDSN
- Relevant channel codes for structural monitoring: `HN` prefix
  - `HNZ` = High-gain accelerometer, Normal rate, Vertical
  - `HN1` / `HN2` = Horizontal components (or `HNN`/`HNE` for North/East)

### Sonification Pipeline
- Earthquake waves: 0.1–50 Hz (mostly sub-audible)
- SOS playback acceleration transposes into audible range
- Base-isolated buildings produce characteristically **lower, filtered output** — isolators attenuate high-frequency jolts → deeper "thrum" character when sonified
- Non-isolated buildings: sharp, broadband transients across full frequency range

### Above/Below Isolator Differential — The Core of TROJAN TREMORS
The Geffen Galleries SHM system places sensors both below (foundation) and above (superstructure) the 56 EPS isolators. The **difference between these two streams is the sound of the isolation system working** — the building's continuous geological voice. TROJAN TREMORS proposes reframing this already-existing structural data as public sound.

### Multi-Channel Triaxial Mapping (Suggested)
- **Z (vertical)** → bass / sub-register
- **X (longitudinal)** → melodic/harmonic layer
- **Y (transverse)** → spatial counter-voice

### Event vs. Ambient — Compositional Structure
- Kinemetrics 155 dB dynamic range captures ambient urban hum (traffic, wind, footsteps) alongside seismic events
- EVT event triggers flag P-wave arrival (sharp, percussive; ~6 km/s) and S-wave arrival (longer, tonal; ~3.5 km/s) — natural compositional structure emerges from physics
- ShakeAlert / EEWS integration possible via OasisPlus for pre-event audio cue layer

---

## Key References — Quick Table

| Resource | URL |
|---|---|
| Kinemetrics | https://kinemetrics.com/ |
| OasisPlus | https://oasisplus.kmi.com/ |
| EpiSensor ES-T | https://kinemetrics.com/post_products/episensor-es-t/ |
| Obsidian datalogger | https://kinemetrics.com/post_products/obsidian/ |
| Obsidian datasheet (PDF) | https://kinemetrics.com/wp-content/uploads/2017/04/datasheet-obsidian-accelerograph-kinemetrics.pdf |
| LACMA isolators (Unframed) | https://unframed.lacma.org/2022/02/16/earthquakes-sliders-and-art |
| Clark Construction LACMA seismic | https://www.clarkconstruction.com/news/seismic-technology-helps-lacma-team-protect-people-and-art |
| EPS Triple Friction Pendulum | https://www.earthquakeprotection.com/products |
| CSMIP | https://www.conservation.ca.gov/cgs/smi/program |
| CESMD | https://www.strongmotioncenter.org/ |
| FDSN SEED Manual v2.4 (PDF) | http://www.fdsn.org/pdf/SEEDManual_V2.4.pdf |
| FDSN SEED format (EarthScope) | https://ds.iris.edu/ds/nodes/dmc/data/formats/seed/ |
| miniSEED 3 spec | https://docs.fdsn.org/projects/miniseed3 |
| miniSEED 3 record definition | https://docs.fdsn.org/projects/miniseed3/en/latest/definition.html |
| StationXML docs | https://docs.fdsn.org/projects/stationxml/en/latest/ |
| StationXML schema (FDSN) | https://www.fdsn.org/xml/station/ |
| libmseed (GitHub/EarthScope) | https://github.com/EarthScope/libmseed |
| libmseed documentation | https://earthscope.github.io/libmseed/ |
| EarthScope fdsnws-dataselect | https://service.earthscope.org/fdsnws/dataselect/1/ |
| EarthScope migration notice | https://www.earthscope.org/news/earthscope-fdsnws-dataselect-service-has-moved-as-part-of-cloud-transition/ |
| SeedLink (EarthScope/IRIS) | https://ds.iris.edu/ds/nodes/dmc/services/seedlink/ |
| SeisComP SeedLink docs | https://www.seiscomp.de/doc/apps/seedlink.html |
| USGS earthquake event API | https://earthquake.usgs.gov/fdsnws/event/1/ |
| SOS / Sounds of Seismic | https://sos.allshookup.org/ |

---

*Reference compiled for TROJAN TREMORS / SOS GitHub archive.*
*Sources: Kinemetrics technical documentation, CSMIP/CGS program pages, FDSN/EarthScope specifications, LACMA project documentation, Clark Construction.*
*Last updated: April 2026*
