# Priority alerts for threshold breaches

## Context

Farmers are interested when telemetry measurements (pH, temperature, salinity, oxygen levels etc.) go outside predefined bounds (thresholds). The thresholds are set by the farmers and may depend on location and the specifics of the geographical location of the farm.
The system will generate alerts when these readings are outside of the pre-defined thresholds (subject to a configurable 'debouncing' approach) and send these as high priority to the farmer (and other subscribers).

## Sequence

```mermaid
sequenceDiagram
    participant tele as Under Water Sensor
    participant teleProc as Local Telemetry Processor
    participant queue as Priority Based Queue
    participant cloud as Cloud Data Ingestion
    Note over tele, cloud:  This approach is generally applicable to telemetry readings
    tele->>+teleProc: send telemetry reading
    teleProc->>teleProc: Debounce the reading 
    Note right of teleProc: configurable filtering of repeated identical readings 
    teleProc->>teleProc: check reading against thresholds
    alt reading is outside of thresholds
        teleProc->>-queue: Queue High Priority Alert
    end
    queue->>+cloud: Send alerts