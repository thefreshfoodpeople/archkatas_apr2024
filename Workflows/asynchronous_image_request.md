# Asynchronous Image Request 
  

## Context  
  
There are a number of reasons that a Fishy Watch user (farmer, support engineer, etc.) may wish to obtain a still image or images from the underwater cameras. For example, a treatment for a particular parasite may have been administered and the user wishes to obtain some images of the fish to determine how effective the treatment was.
In the low bandwidth, patchy environments we expect Fishy Watch to be deployed in, there may be times when a synchronous response to such a request is not possible. It may be also the case that we which to control the rate at which these types of requests are processed. To this end, we will provide an API that will allow image requests to me queued and processed asynchronously. When the image or images have been processed, the results will be placed on a message queue (as low to medium priority) - see [002_ADR_Priority_Queue](../ADR/002_ADR_Priority_Queue.md). 
  

## Assumptions  
  - there is sufficient local storage to cache a 'reasonable' number of images for a 'reasonable' amount of time.
  - the underwater camera's provide an API to request still image capture. 
  - there is some way for user to view the images once they have been sent from the local message queue to the cloud. 

## Sequence  
 1. User requests an image via the UI
 2. The Request is stored by the API in the local request queue and a 202 Accepted response returned
 3. The request is sent to the camera
 4. The camera captures the image
 5. The image is wrapped in a message and placed on the outgoing queue.
 6. The message is sent to the cloud and made available to the user
 7. The user is notified of the image availability along with an access link.


```mermaid  
sequenceDiagram  
	participant ui as Web User Interface
	participant api as Image Request API
    participant cam as Under Water Camera  
    participant lq as Local Request Queue 
    participant qProc as Request Queue Processor  
    participant queue as Priority Based Queue
    participant cloud as Cloud Data Ingestion
    
    ui->>+api: Image Capture Request
    api->>lq: queue the request
    api->>-ui: 202 (Accepted) 
    loop for each request  
	    qProc->>lq: pull the next request
	    qProc->>+cam: request image capture
	    cam->>-qProc: return image
	    qProc->>queue: queue image message
    end
    queue->>+cloud: Send Messages  