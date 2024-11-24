# NAIR - Wildfire Detection and Visualization using Sentinel Hub

This project interacts with Sentinel Hub services to retrieve and visualize satellite imagery data, specifically focusing on detecting and visualizing wildfires.
<pre>
[Fully Rendered JupytherNotebook: https://nbviewer.org/github/ksanda5/NAIR/blob/main/WildfireIdentification.ipynb]
</pre>
[Fully Rendered JupytherNotebook: https://nbviewer.org/github/ksanda5/NAIR/blob/main/WildfireIdentification.ipynb]
## Key Components

### 1. Configuration Setup
Sentinel Hub configuration is set up using `SHConfig`, where the user inputs their client ID and secret. This configuration is necessary for authenticating and making requests to the Sentinel Hub API.

### 2. Defining the Area of Interest (AOI)
The AOI is defined using GeoJSON format and read into a GeoDataFrame. This specifies the geographic region for which satellite data will be requested.

### 3. Geometry Conversion
The AOI is converted to a specific coordinate reference system (CRS) and stored as a Geometry object. This ensures the geographic data is in the correct format for further processing.

### 4. Evalscripts for Data Processing
Two evalscripts are defined:
- **Fire Detection Evalscript**: Processes Sentinel-3 data to detect active fire points.
- **Wildfire Visualization Evalscript**: Processes Sentinel-2 data to visualize wildfires, enhance natural colors, and optionally show burn scars and water highlights.

### 5. Creating and Executing Requests
Requests are created using the `SentinelHubRequest` class, specifying the evalscripts, input data parameters, response format, geometry, and configuration. The requests are executed to retrieve the processed satellite imagery data.

### 6. Data Visualization
The retrieved images are visualized using a custom plotting function. This function scales and clips the image values for better visualization.

## Example of a Key Code Block

Here’s an example of a key code block that sets up and executes a request for wildfire visualization:

```python
# Create a SentinelHubRequest object for wildfire visualization using the wildfire visualization evalscript
request_wildfire_visualisation = SentinelHubRequest(
    evalscript=evalscript_wildfire_visualisation,
    input_data=[
        SentinelHubRequest.input_data(
            data_collection=DataCollection.SENTINEL2_L2A.define_from(
                name="s2",
                service_url="https://sh.dataspace.copernicus.eu"
            ),
            time_interval=("2021-07-31", "2021-08-31"),
            other_args={"dataFilter": {"mosaickingOrder": "leastRecent"}},
        )
    ],
    responses=[SentinelHubRequest.output_response("default", MimeType.PNG)],
    geometry=full_geometry,
    size=[1000, 920],
    config=config,
)

# Execute the request to retrieve the wildfire visualization images
imgs = request_wildfire_visualisation.get_data()
```
### First step approach
- Google Orbit Hearth
- Wildfire dataset
- Cloud removal algorithm
- NVDI Index and/or burned/area surface
- FFT/Inverse FFT
- Dry soil detecting


### Selling point
- Potentially can be used by insurance company for risk evaluation and insurance
- S&R Operation in case of natural disaster
- sell to insurance explaining the scenario with a farmer that pretends that his field has been burnt by a wildfire

- find city close to hepl evacuation
- cospas-sarst->finds early warning about wildfires
- 
