# Applications
The following section describes the default applications provided by AGI.  Links to the AGI applications are consolidated at the site [SOME Experience builder app here](https://).  This link requires a ArcGIS Portal login for GeoSecure.



## Indoors Viewer App  

The AGI viewer is the most heavily used application in AGI.  It displays uneditable building floor plans and has search and navigation features. It can integrate with other applications by providing a basemap to show locations for nonspatial applications.  For example, AGI can show where a printer from a Service Now ticket is located and focus the map on the printer’s location.  It can then route a technician to the location of the printer.

The Indoors viewer uses a file geodatabase for its data.  The file geodatabase is updated on a nightly schedule using data extracted from the SDE.  There are several reasons for using the file geodatabase.  The first is performance.  The file geodatabase is stored on the GIS server.  Data access is much faster because the GIS server doesn't have to reach across the network to the SDE server for data.  The file geodatabase also does not have any of the archived edits (see below) cluttering up the database.  These are purged when the data is exported from the SDE.  Another reason for using a file geodatabase is that it diverts the majority of the workload from the SDE.  This means that data loading and maintenance operations during business hours will not affect the experience of the application for most users.

The viewer app also shows reservations of conference rooms.  The reservations are made in Microsoft Office applications such as Outlook and the reservations flow to the AGI platform.  There is a service (see below) that publishes this data.


<img src="./images/FloorPlanViewer.jpg" alt="image" width="700" height="auto">

+++



### Indoors Viewer Assets

ArcGIS Manager Folder: https://geosecureprd2.epa.gov:6443/arcgis/manager/index.html#f=Indoors

|GeoSecure Asset Name 						|Asset Type 				|Details URL	|Asset URL	|Published From|Notes|
|-							|-							|-			  					|-			|-				|-	|
|Indoors Viewer Application		 	|Application								|[Details](https://geosecure.epa.gov/portal/home/item.html?id=9775e8a2ae704471bd77e8ae3a0e82ff)  |[App](https://geosecure.epa.gov/portal/apps/indoors/index.html?appid=9775e8a2ae704471bd77e8ae3a0e82ff)|[Project\Map] | 
|Indoors Viewer Web Map  			|Web Map									|[Details](https://geosecure.epa.gov/portal/home/item.html?id=883e01b48e5245b18242c21ce11dfb7b)  |[Map](https://geosecure.epa.gov/portal/apps/mapviewer/index.html?layers=0400c15d7d7e4828b06fb3189d244ecf)|[Project\Map] | 
|Indoors_Cached				 		|Map Service						|[Details](https://geosecure.epa.gov/portal/home/item.html?id=0400c15d7d7e4828b06fb3189d244ecf)|[REST](https://geosecure.epa.gov/arcgis/rest/services/Indoors/Indoors_Cached/MapServer)|[Project\Map]|
|Network_2406 						| Route/Closest Facility Service									|[Details](https://geosecure.epa.gov/portal/home/item.html?id=3ea3814d28b74df1a9254237099b463a)|[REST](https://geosecure.epa.gov/arcgis/rest/services/Indoors/Network_2406/NAServer)			|[Project\Map]| Configured in App
|Reservations  		 		|Meeting Room Reservation Service									|[Details](https://geosecure.epa.gov/portal/home/item.html?id=9e84fdaa9d384d028a31e87142155c59)  |[REST](https://geosecure.epa.gov/arcgis/rest/services/Indoors/Reservations/FeatureServer)|[Project\Map] | 





### Viewer ArcPro Projects and Maps

The viewer has assets published out of the ArcPro project x.  There is one map to publish  the read only service.  It used to be the case that the production web map was maintained in a Pro project.  However, we have begun to make changed directly to the map online.  The version in Pro might not be the cannonical version.  It has become convention that we download the current version from GeoSecure, make edits, and then publish the map overwriting the current map. 

### Regional Indoors Viewers

Regional AGI viewers have been made for several regions.  These apps limit the building list to the local region.  This makes navigation and searching easier for the end users.  The regional viewers use the same file GDB as the national viewer but have web maps with definition queries applied to the services.  


!!!!!!!!!!!!!!!!



## [Space Planner App](https://geosecure.epa.gov/portal/apps/spaceplanner/index.html?appid=56014bb0bc074a4487e84075641427b5)  

The space planner app is used to assign occupants to spaces such as offices or workstations.  In addition to long term planning, the space planner app can be used for hoteling, or hotdesking temporary staff.  Once a plan is proposed it can be submitted for comment and approval.  The space planner app symbology focuses on displaying occupant information and differs from the viewer app.  The app can be used to search for particular staff members or see the configuration of a team’s offices.

The space planner app uses a branch version feature service.  Each plan creates a branch where the edits for that plan are kept.  Users can create plans but must have admin rights to merge the plan back into the default branch.  usrs in the [ArcGIS Indoors Branch Version Admins](https://geosecure.epa.gov/portal/home/group.html?id=2e9083ec27d24219862efc661b1dc870#overview) can merge plans back into the default branch. 

https://geosecure.epa.gov/portal/apps/spaceplanner/index.html?appid=56014bb0bc074a4487e84075641427b5




<figure align="center">
<img  src="./images/SpacePlanner.jpg" alt="image" width="700" height="auto">
<figcaption>Space Planner App.</figcaption>
</figure>



### Space Planner Assets

|GeoSecure Asset Name 			| Asset Type	|Details URL |Asset URL|Published From|Notes|
|-						|-			  	|-		|-			|-	|-	|
|EPA Space Planner Application		 	|Application								|[Details](https://geosecure.epa.gov/portal/home/item.html?id=56014bb0bc074a4487e84075641427b5)  |[App](https://geosecure.epa.gov/portal/apps/spaceplanner/index.html?appid=56014bb0bc074a4487e84075641427b5)|[Project\Map] | 
| Planner_WM 			|Web Map									|[Details](https://geosecure.epa.gov/portal/home/item.html?id=3d73322b96e74ae9ba70f0b69aac509f)  |[Map](https://geosecure.epa.gov/portal/apps/mapviewer/index.html?webmap=3d73322b96e74ae9ba70f0b69aac509f)|[Project\Map] | 
|Indoors Feature Layer |Indoors FeatureLayer/Indoors_BV	|[Details](https://geosecure.epa.gov/portal/home/item.html?id=b41e9df0137c4bfe9ad6fcadd7a7be53)|[REST](https://geosecure.epa.gov/arcgis/rest/services/Indoors/Indoors_BV/MapServer)|[Project\Map]|1234
|r		| Space_Planner_Web_Map_MIL1 ?? - Broken				|[Details](https://geosecure.epa.gov/portal/home/item.html?id=cb0305e1b85e4fbfbb59e04cdd123768)|[REST]()|[Project\Map]|

### Regional Space Planner

Just like the floor plan editor, there are regional versions of the space planner app.  The regional 

## [Floor Plan Editor](https://geosecure.epa.gov/portal/apps/floorplaneditor/index.html?appid=db661657704f44839af736933a7e1a8c)

The floor plan editor allows local staff to maintain and update floor plans based on changes to the building.  It also allows staff to correct errors from the original CAD file imports if out of date files were used.  New units, details, and furniture can be added using a simplified web interface.  The floor plan editor creates a branch in the geodatabase and stores the changes proposed by the user in that branch.  Users with elevated privileges then merge the edits back to the default branch.

In addition to the 

<figure align="center">
<img  src="./images/FloorPlanEditor.jpg" alt="image" width="700" height="auto">
<figcaption>Floor Plan Editor App.</figcaption>
</figure>

### Floor Plan Editor Portal App
Details: https://geosecure.epa.gov/portal/apps/floorplaneditor/index.html?appid=45bd82cc155f4aae836c4e7c3d1d3aea

App: https://geosecure.epa.gov/portal/apps/floorplaneditor/index.html?appid=db661657704f44839af736933a7e1a8c
### Floor Plan Editor Maps



### Floor Plan Editor Services



|Asset 			| Asset Type|GeoSecure Name	|Details |Rest URL|Published From|Notes|
|-						| -|-			  	|-		|-			|-	|-	|
|Floor Plan Editor |Application |	|[Details]()|[REST]()|[Project\Map]|1234
|Floor Plan Editor Web Map |Web Map |	|[Details]()|[REST]()|[Project\Map]|1234
|Editable Branch Managed Feature Service |Service |	|[Details]()|[REST]()|[Project\Map]|1234
|Feature Service |Service |	|[Details]()|[REST]()|[Project\Map]|1234

+++



## Customized Apps

The above apps are the standard app templates provided by Esri and provide some of the most commonly requested uses of floor plan data.  These applications are build using the standard paradigmn of data, services, maps, and applications.  Using the same Indoors data, custom applications for specific needs of EPA staff can be created. These may include maps for evacuation, IT infrastructure, WiFi coverage, etc.  To utilize AGI data, users connect ArcPro to one or more of the web published feature services or web maps and utilize the data to make maps as if the data was on a local hard drive.  Filters, symbolization, and labeling can be applied without affecting the production applications.

## Status Dashboard
The status dashboard show the progress of the building import process.  The various states of the buildings are shown

App: https://geosecure.epa.gov/portal/apps/dashboards/1fa13a60a52b43ff99beeebae2a609d5

Details: https://geosecure.epa.gov/portal/home/item.html?id=1fa13a60a52b43ff99beeebae2a609d5





# External Application Integrations
## Space Reservations through Office 365 Integration
In the past, conference rooms were the only spaces that were able to be reserved.  Reservations were made through Outlook/Office 365.  However, Office365 does not have a map interface that you can use to select spaces based on location.  Also, indiviual offices were not available for reservation.  More recently, individual workspaces and offices have become assignable in a new process called hot desking.  Users may want to reserve workspaces and conference rooms that are adjacent so they can collaborate on a project easier.  Indoors can act as a map interface to the reservation system.  AGI can interface with O365 and show current reservations and accept new ones.  This is done through the Space Planner application and will be described below.
% This system works through a connector [future work]

% The contact for this integration is []


## Sunflower Integration

https://www.cgi.com/us/en-us/federal/cgi-sunflower-solutions

EPA currently uses the application Sunflower for facilities and asset management.  Sunflower keeps track of building envelopes and assets for maintenance tasks, capital planning, and operations reporting.  When the AGI project was started, an initial list of buildings and their footprints was needed.  Sunflower (Sunflower was actually the second package used.  There was a transition to Sunflower from ___ during the initial data collection for Indoors.) was used to get the list of buildings and their footprints to start the AGI project with.  The footprints from Sunflower are not believed to be extremely accurate.  As the CAD drawings are ingested it is intended that the CADs will more accurately represent the building footprints.  After getting the initial building details and footprints, there has been no other integration between Sunflower and AGI.  Future integration with Sunflower may be possible to map building assets to locations.

The latest Sunflower data was extracted on x.  

It was deposited in database x located at x.  

The ArcPro project __ was used to ingest the data into the AGI database.



## Service Now (SN):  

Test Site: https://USEPAtest.servicenowservices.com

EPA currently uses the application Service Now (SN) to manage IT helpdesk tickets.  AGI can be used to map the locations of tickets and communicate to staff how to quickly get to ticket locations.  This can be particularly useful when directing helpdesk staff to itinerate employees using hot desking.  A spatial perspective can also be used to group geographically clustered tickets to reduce the travel time of helpdesk staff.

The SN/AGI integration uses tools provided by [Esri](https://github.com/Esri/indoors-servicenow-feature-service).  The Esri tools are a set of templates and scripts that move data between the systems. Because of the large number of tickets, this integration did not work.  It was too slow and cumbersome.



Our current integration uses a syncronization process.[^sn]  This process AGI interacts with SN through an HTTP API.  SN has one table in its database that holds the locations of offices (Units).  This table is not spatial, it is just a list of offices without any coordinates or geometry.

[^sn]:We initially tried using a tool provided by Esri that integrates the data in realtime.  It uses Koop and Node to integrate the spatial data.  Because of the large number of buildings, it was too slow to use; even before we had many [^sn]

Our synchronization process hits the SN API and gets the list of offices/units.  It then to the AGI units table using attribute data.  It joins on field ??  The AGI units have geometry and can be mapped.   After joining the SN offices, it hands them back to SN with the AGI unit ID (and possibly Lat,Long).  With the unit ID, SN is able to make a call to AGI service endpoint and pop up  a map in a new window showing the AGI viewer map zoomed in on the SN location.
https://github.com/Esri/indoors-servicenow-feature-service

Frequency: The synchronization tools are run as needed; usually by Torrin.  – run as needed.  Python toolbox.  ServicenowloaderMark

The notes.txt file in the project folder on Winkel has info for the service now integration – contains example queries – save useful queries here.

SN API Documentation - https://docs.servicenow.com/en-US/bundle/vancouver-api-reference/page/build/applications/concept/api-rest.html
For future integration – get list of active tickets and map them.

There is a connection file called that is in the SN integration GIS project folder.  It uses credentials x to connect to the SN database.  They are documented at x.
Our contact for these credentials is x  
The credentials are database? Credentials.  
They expire/have to be rotated every x
The connection file to connect to the SN database is x

The SN project is in folder x
The SN integration tool is a script that resides in x



## eBusiness

eBusiness is used to manage staff in some regions at EPA (there may be other local applications).  Data about staff is syncronized from ebusiness to AGI.  In AGI, staff are called occupants.  Currently, occupants for AGI come out of the eBusiness system to be used for AGI applications like office occupant lookups and hot desking.  Not all regions use eBusiness.  As there are over 17,000 employees at the EPA, there is constant changes in the  staff directory so synchronization with eBusiness requires a quick cadence.

The eBusiness data is exported into format and deposited to .  

The ArcGIS Pro project  was used to integrate the eBusiness data using the tools .
vmoraeeprd1.rtpnc.epa.gov.sde
Database in folder ebusiness_vmoraeeprd2
Replicates 2 ebiz tables into SDE as standalone table and populates fields with concatenation of unit and building IDs.  
Geoprocessing tool licensed updateOccupantFeatures
	Give the uniqueID for occupants and units
	Updates the occupants in Indoors.

The ebusiness syncronization is running on weekly basis

% [Future work.  Describe any IPS equipment and integration.]
<!-- ## Indoor Positioning System (IPS)
  An IPS is a hardware system that provides location data inside a building.  It is usually separate from any particular application, much like WiFI hardware.  Indoor navigation can be a difficult task for several reasons.  Indoor navigation requires more precise location data than outdoor navigation.  Also, many of the signals used by applications to determine location outside are blocked indoors.  When available, they may be inaccurate due to reflection and interference.  AGI can use signals inside from IPS hardware to show location and direct users through buildings to their destinations.

  IPS is not currently on the road map but is here in the documentation for reference.  IPS requires particular system configuration, licensing and data ingestion steps outlined here. https://pro.arcgis.com/en/pro-app/latest/help/data/indoor-positioning/get-started-with-arcgis-ips.htm
   -->


```{tableofcontents}
```