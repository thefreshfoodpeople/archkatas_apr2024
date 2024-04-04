# Detection of a new type of parasite

## Context

In order to support farmers with early warning of a potential new parasite infestation, a workflow will be put in place to send an alert when a new type of parasite is detected.

## Assumptions

 - the underwater cameras have a data collection mechanism that can be linked to a local processing unit.
 - the underwater cameras can independently detect parasites and indicate this to the local processing unit.
 - the local message queue that will be used to send alerts to the cloud has an implementation based on the [Priority Queue Pattern](https://learn.microsoft.com/en-us/azure/architecture/patterns/priority-queue) - see [002_ADR_Priority_Queue](../ADR/002_ADR_Priority_Queue.md)

## Sequence

```mermaid
sequenceDiagram
    participant cam as Under Water Camera
    participant mediaProc as Local Media Processor
    participant queue as Priority Based Queue
    participant cloud as Cloud Data Ingestion
    Note over cam,cloud: Camera unit can detect the presence of parasites
    cam->>+mediaProc: indicate parasite detected
    mediaProc->>mediaProc: check parasite log
    alt new type of parasite
	    mediaProc->>queue: Queue High Priority Alert
	else existing type of parasite
	    mediaProc->>mediaProc: Log detection
	    mediaProc->>mediaProc: Check detection threshold
	end
    alt threshold reached
	    mediaProc->>-queue: Queue High Priority Alert
    end
    queue->>+cloud: Send alerts