# NAIR - Wildfire Detection and Visualization using Sentinel Hub

This project interacts with Sentinel Hub services to retrieve and visualize satellite imagery data, specifically focusing on detecting and visualizing wildfires.

[Fully Rendered JupytherNotebook: https://nbviewer.org/github/ksanda5/NAIR/blob/main/WildfireIdentification.ipynb]


https://github.com/user-attachments/assets/d18cb62e-ec52-4f75-877e-e7a360176ddb


![Screenshot from 2024-11-24 15-56-07](https://github.com/user-attachments/assets/baf677c2-6c1c-486c-a7ce-e295d676d5d2)
![fire](https://github.com/user-attachments/assets/4ef80f4a-06f0-4741-ae4a-18fa33926499)
![Screenshot from 2024-11-24 15-57-33](https://github.com/user-attachments/assets/63cfb33b-adb5-4b64-bf42-27776c38a6ad)
![Screenshot from 2024-11-24 15-58-10](https://github.com/user-attachments/assets/ee072c91-84b7-46a9-a539-ab791d476fdc)

# The problem

Europe is currently facing an increasingly complex set of security challenges that significantly threaten public safety and societal stability. Among these, the escalating frequency and intensity of wildfires have emerged as particularly urgent issues. These catastrophic events pose immediate dangers to lives and property while also inflicting profound and lasting impacts on the economy, environment, and cultural heritage. 

In recent years, wildfires have become alarmingly prevalent, with 2023 marking one of the most devastating seasons in Europe’s history. Reports indicate that over 504,000 hectares were scorched across the continent, making it one of the worst years for wildfires since the turn of the millennium. The economic losses attributed to wildfires in 2023 alone exceeded €2.5 billion, underscoring the severe financial repercussions on affected communities. This devastation has resulted in significant population displacement and irreversible damage to ecosystems. 

The trend of increasing wildfire incidents shows no signs of abating. Factors such as climate change, inadequate land management practices, and urban expansion contribute to a landscape that is increasingly vulnerable to these disasters. The initial phase of a wildfire crisis is critical for effective response efforts; swift action is essential to mitigate damage and protect lives. However, traditional monitoring and response methods often fall short in providing timely and accurate information about the extent of the threat. Emergency services frequently operate with limited situational awareness, hindering their ability to make informed decisions quickly. 

Moreover, a significant challenge lies in the reliance on single-source connectivity for communication between drones and control stations. This dependence on satellite-based networking or GPS communication can create bottlenecks that delay information exchange and impede prompt communication with emergency service command centers. To address these multifaceted challenges, there is an urgent need for innovative solutions that facilitate rapid detection and assessment of wildfires in real time. Effective monitoring systems must integrate advanced technologies capable of gathering comprehensive data about impacted areas, enabling emergency responders to act decisively during critical moments. 

# The solution

Our proposed solution harnesses the capabilities of unmanned aerial vehicles (UAVs) and mid-sized drones combined with geospatial intelligence to bridge this gap. By implementing a self-sustainable networking solution among drone fleets, we aim to overcome communication challenges and enhance the effectiveness of wildfire monitoring and response efforts across Europe.

<br />
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

## Satellite Imagery for Initial Assessment

Utilizing images from the SENTINEL-2 satellite, we can accurately identify affected areas and extract initial coordinates of wildfire events. This satellite data is crucial for understanding the extent of the threat and initiating timely responses. Leveraging our proprietary technology, we deploy a fleet of drones—ranging from Tactical UAVs to quadcopters—to gather detailed information about the hazard. This process involves delineating the impacted zone and automatically coordinating the drone swarm to enhance our understanding of the area.

## Advanced Onboard Sensors and Smart Agents

Each drone is equipped with advanced onboard sensors that work in conjunction with smart agents. These technologies enable the drones to identify critical infrastructure such as buildings and people, as well as monitor environmental conditions to assess and forecast further developments of the wildfire. This capability allows for real-time data collection that informs emergency responders about evolving threats.

## Customization and Contextual Adaptation

Our technology allows agents to be customized according to specific scenarios, adapting based on available information from the environment, the nature of the emergency, and the connectivity status among drones in the fleet. This adaptability facilitates the exchange of contextual knowledge through a Distributed Reinforcement Learning model designed for embedded devices. As a result, we can identify and prioritize critical Points of Interest (POIs), providing granular information that leads to a deeper understanding of the affected zone. The agents perform asymmetrical sensor fusion, integrating data from individual units to create a comprehensive overview of the situation.

## Robust Communication Framework

The adaptive drone swarm features a robust communication framework among the fleet. By utilizing Low-Power Wide-Area Network (LoRaWAN) and long-distance Wi-Fi technology, our system eliminates reliance on a single point of failure. This architecture ensures full autonomy and redundancy through unsupervised coordination, establishing a resilient communication bridge capable of withstanding disruptions in satellite communication. This capability is essential for maintaining connectivity over long distances and in hostile environmental conditions that may impede communication between the fleet and the Control Ground Station (CGS).

## Use of EU Space Technologies

Sentinel-2 is designed to capture multi-spectral imagery across 13 spectral bands, ranging from visible light to shortwave infrared. The spatial resolutions of these bands vary, with four bands at 10 meters, six bands at 20 meters, and three bands at 60 meters. This configuration enables detailed observations of vegetation, soil, water bodies, and coastal areas across a wide swath width of 290 kilometers. 

Our solution relies on bands 02, 03, 04, 08, 11, and 12, crucially for effective burned area detection. Each of these bands serves a distinct purpose in analyzing the spectral characteristics of the Earth's surface, particularly in identifying areas impacted by fire by computing:

Normalized Difference Vegetation Index (NDVI): Calculated using Bands 08 (NIR) and 04 (Red), NDVI helps identify healthy vegetation versus burned areas by measuring the difference between the reflectance in these bands. Healthy vegetation has high NDVI values, while burned areas typically show lower values.

Normalized Difference Water Index (NDWI): This index is calculated using Bands 03 (Green) and 08 (NIR) to assess water content in vegetation. After a fire, changes in moisture levels can indicate whether an area has been affected.

Fire Area Index (FAI): The script introduces an index calculated from Bands 11 and 12 along with Band 08. This index leverages the unique spectral responses of these bands to enhance the detection of burned areas by combining information about moisture content and vegetation health.
