# Strayos Architecture: GCP

### Functional Specification

###### David Ayeke

Last Updated: December 8, 2017
2017 Strayos, All Rights Reserved

---

## Overview

GCP's aid ODM's accuracy by marking GPS locations of easily recognizable features in the images. 
A typical GCP file looks like the following. 

```
WGS84 UTM 10N
544256.7 5320919.9 5 3044 2622 IMG_0525.jpg
544157.7 5320899.2 5 4193 1552 IMG_0585.jpg
544033.4 5320876.0 5 1606 2763 IMG_0690.jpg
```

Line 1 is the projection. To aid the front end and other parts of our system, all GCP files processed by ODM will need to be in a UTM projection **NOT** an EPSG code. 

The subsequent lines are geographic locations of specific pixels in specific images in the following format: Easting Northing Height(in meters) PixelX PixelY Filename

This spec is not, by any stretch of the imagination, complete. All of the wording will neeed to be revisited several times before it is finalized. The graphics and layout of the screens is shown here merely to illustrate the underlying functionality. The actual look and feel will be developed over time in typical agile fashion. 

## Scenarios

In designing products, it helps to imagine a few real life stores of how actual (stereotypical) people would use them.

### Scenario 1: Greg the GCP nut

Greg is using the Strayos platform to process a flight. He uploads a GCP template in the format we specify as well as the images. During processing, a window pops up with GCP's we were able to detect. It prompts him to click the exact center of the gcp. Once he has done that for the gcps that were detected, he gets prompted again with gcp's he may have missed. He repeats the process he did for the initial list. Once everything looks good, he is prompted to finish the upload process. 

### Scenario 2: Tom the Tensor Tinkerer
Tom wants to improve our GCP detection algorithm. We've had hundreds of GCP's processed by the new system, and he wants to use that data to create new training and validation sets for the neural network. He should be able to download the GCPs generated from feedback from the users.

### Scenario 3: David the Debugger
An email came in from a user that couldn't upload images. David queries the api for all processes, and sees that one of the images don't have exif data (or something). He then restarts that process.