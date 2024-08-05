# Conventions and Configurations
Space data at the EPA developed in many disconnected silos among the various EPA regions and programs.  There is also a temporal aspect to this problem as space data formats have evolved through several stages including hard copy drawings, 2D digital CAD files, and 3D BIM files.  These later two can can be directly imported saving the effort of digital authoring.  However, it is likely that the data uploaded from stakeholders will have its own local conventions for numbering rooms, naming spaces, and assigning uses to those spaces.  In addition, there is always an amount of dirty data.  It is important that AGI be flexible to allow for local input while also being able to link multiple systems that contribute to an integrated view.  For this reason, the standard AGI fields will be supplemented with fields useful to users at multiple levels at the EPA.  Domains will be used to constrain data to predetermined options to maintain clean records and make data entry easier.  

Where possible, existing standards will be adopted and or adapted when defining data domains.  By default,  AGI has a hierarchical data model that allows the application to efficiently filter and search data.  Sites, facilities, levels, and units have IDs based on a nesting structure.  Each unit is on a level, each level is in a facility, and each facility is at a site.  This structure will act as a backbone for the application but may require modifications for local use.

**Projection**  Web Mercator is the default projection as our maps will be accessed through web services for most users.  While Mercator comes with its issues, it will be the fastest to perform in the browser and we will have many features loading as users pan and zoom around the map. 

**Versioning**  We are using a branched version configuration for the geodatabase.  In this workflow, an SDE connection is published as a branched version feature service with the feature service management capability enabled.  This allows users with the correct permissions to create a branch and make their edits in the branch.  The edits are then reviewed and merged back into the default branch.  Note, any changes to the schema will require republishing the service to pick up the changes.  The republishing process can be difficult as it requires shutting down any services and disconnecting all users that have locks in the database.

Data Sources:

* **Sites**: (Property Boundaries) Sites have not currently been integrated (ca. 3/24) but may come from parcel records.  
* **Facility**: Building footprints have come from Sunflower.
* **Units** (rooms) have come from CAD drawings
* **Details** (walls, doors, and furniture) have come from CAD drawings
* **Occupants** Have come from the eBusiness system.  They are synced on a nightly basis.


## Layer Conventions
When working in ArcPro, default configurations are often applied to new maps and layers.  It is easy to notice that precise settings may have been changed or lost until they have been put into a production map.  It is important to maintain consistent settings and have a known good reference to check against.  The following section outlines the settings for each layer.


### Units (Offices, Conference Rooms, etc)

Units are one of the most important spatial features in AGI.  A unit is roughly equivalent to a room.  However, AGI must manage all spaces in a building including those that are typically not assigned room numbers such as hallways and mechanical spaces.  To be flexible, AGI divides entire building footprints into units instead of just rooms.  Using symbology and filters, these additional spaces can be filtered out of the maps but still retained for calculating space usage for reports.

#### Units Feature Class Fields
The Units feature class comes with default fields provided by Esri.  EPA has added fields for internal uses.  These are some of the important fields in the Units feature class.  

|Field						| Example 	| Description  					|
|-							|-			|								-|
|Area ID 					| 			|Areas are used by AGI to group together units for a particular use.  |
|Building and Facility ID  |  			|This field is a concatenation of the  |
|Facility ID				|MD0317| The Facility ID is a unique ID that is used to match the unit back to the facility for reporting.  It is not in the default model but was added to make reporting easier. |
| Unit ID 					| HQ.DC0459.1.9251|  The unitID is created by appending the site ID, the facility ID, the level, and the 
|Name 						|9251 				|The Name is sourced from Sunflower??  ??this is the field used in the web app??
|Long Name 					|19251, G  |The Long Name is sourced from Sunflower???
|Use_Type 					| Stairway |The use type is used for symbology and is restricted by a domain. |
|Use Type Local 			| Stairway |The use type local is for local definitions of use type.  These can be used for reports and maps created for local consumption. |
|Level ID 					|HQ.DC0459.1  |The Level ID is a concatenation of .  It is important because it allows the filtering of levels in the web apps.
| Gross Area 				| 123		|WHERE DOES THIS COME FROM?
| Capacity 					| 123		|WHERE DOES THIS COME FREOM?  IS THIS THE LEGAL NUMBER?
| Assignable 				|Yes\No\U		 |Assignable means that a person is there full time.  It is used in the reports for assignable space. 
|IDF 						| 		| An intermediate distribution frame (IDF) is a satellite wiring closet used to distribute telco connections to individual wall jacks with trunk lines to the main distribution frame (MDF).


#### Unit Numbering:  
The Units number is used for local signage.  Units such as conference rooms and offices often already have a local set of numbering and can very across buildings.  These numbering systems are tied to signage, documents, and procedures.

AGI has a default internal unit ID system based on a spatial hierarch; Facility+Level+Local Room.  This column is x

AGI also has a column for local numbering that can be used for local versions of online and static maps.  This column is x.

#### Calculating space:
{Have to follow up with David on this.  Not sure how space was calculated} There is an attribute in the units FC for space measurement in square feet.  It appears that this comes from the CAD drawings.  The CAD drawings often do not have this information so there are many blank rows for space.  Space can be calculated using the feature geometry.  This needs more documentation.

%[  Attention should be paid to how this is calculated.  Our data is in Web Mercator.  Web Mercator is good for preserving angles but creates area distortion.  The process for calculating space should take into account the distortion caused by the Mercator projection when calculating area.]%


%[Freqency Table of Current entries]


#### Unit Domains
**Use_Type Assignment:** Space usage defines the activities that normally occur in a space i.e. conference room, mechanical room, restroom, etc.  Space usage is important because it  is used to symbolize and filter layers in the AGI applications.  It is also used in many reports to describe the amount of space devoted to each activity.

Use_type data is an attribute that will vary wildly in the incoming data.  In addition to all of the local use types, there are many units that are imported using labels, measurements of square feet, and even no data at all.  Problems with map symbolization and reporting accuracy will occur if the use_type data is not consistent.

In order to constrain data in the use_type field, A domain was constructed to consolidate the use_types.  This domain was based on the FICM standard.  The National Center for Education Statistics has a Facilities Inventory and Classification Manual (FICM). [1]  The FICM defines a space use hierarchy that is used by postsecondary institutions to report their space use to the Department of Education. [2] 

#### Unit Symbology
In the Indoors viewer app, units are symbolized by groupings of use_types.  Therefore, it is important that the data in this field is current.  Inaccurate data will prevent units from showing up or will put them in the wrong category.

In the floor plan editor, the use_types are used natively without grouping.



| Application       | Symbology Field | Scale |Style File |Notes |
| -------------     | ----------------| --    | --------  | -- |
|Viewer             |use_type         |       |
|Space Planner		|
|Floor Plan Editor  |


#### Unit Labels

the following table describes the unit label details for each application.

| Application       | Label Field | Font and Size | Visability Scale | Expression |
| -------------     | ------------| --            | --               | --         |
|Viewer             |use_type     |               |         			|			 |
|Space Planner      |use_type     |               |         			|			 |
|Floor Plan Editor  |use_type     |               |         			|			 |



#### Unit Popups

[TBD]

### Details (Walls, Windows, Furniture, Fixtures)

The details feature class is one of the most important.  The details FC shows walls, furniture, equipment etc.  The details FC is also used to create the network routing dataset.  The walls in the details FC dictate how the routing geoprocessing tools create the initial pathways.  When they run, the routing geoprocessing tools are filtered by the field “Use Type”.  For instance, the tools will ignore features with a use type of “furniture” but honor one that is “external wall.”

Since the routing tools use the details attributes to create routes (normally you would choose all of the values that represent barriers to travel such as walls and cubicles.), it is important that the data be clean when it enters the system.  If a wall has an attribute that is not included in the filter it will be ignored when creating routes and could lead to inaccurate routing.
When the incoming data arrives from the regions, it will likely have many different entries for “Use Type”.  They are mostly based on whatever was put in the CAD drawing.  It will be very important to clean this data as it come in so that it does not break the routing tools.
The other issue is with the number of details features.  The import process brings in each line segment from the CAD files as a separate feature.  For instance, every step in a stairway is a feature.  This means that many features have to be sent and drawn when displaying the layer.


#### Details Domains

The Details use_type domain is in the table below.  We are in the process of converting all current data into this domain.
Currently there is not a domain for the Details FC.  There should be one to constrain the “Use Type” field.  We are in the process of building one and writing the code that will clean up the current data that came in from the 

| Domain | Description | Description |
| ----------- | ----------- | --|
| _NotCat | Not Categorized | This category is used for data that has been imported but has not had its use type determinded yet.
| _Not Cat | Text |          |



| Code	| Description |
| ----------- | ----------- |
Door |	Door
Building Structure |	Building Structure
Fixtures and Furniture	|Fixtures and Furniture
Elevator/Escalator |	Elevator/Escalator
Not Categorized |	Not Categorized
Other |	Other
Stairs |	Stairs
Utility/Mechanical	Utility/Mechanical
Wall-Temp	Wall-Temp
Wall-Partial	Wall-Partial
Wall	Wall
Window	Window

#### Details Symbology

| Application       | Symbology Field | Scale |Style File |Notes |
| -------------     | ----------------| --    | --------  | -- |
|Viewer             |use_type         |       |
|Space Planner		|
|Floor Plan Editor  |



Zoom in:
Zoom out: 1:500
Style file:
#### Details Labels


•	field .  
•	Font  at  size.  
•	Not viewed beyond: 
•	Expression:

Not all feature in the details layer will by symbolized.
#### Details Popups


### Facilities (Buildings)

In AGI parlance, buildings are called facilities.  The facilities layer contains the building outlines on the site parcels.  EPA gets the facilities data from a program called Sunflower.  In addition to the standard AGI fields, the facilities FC will have EPA specific data that comes from Sunflower.  The table below details some of the important fields that are in the EPA AGI deployment.

One important field from the facilites FC is the x.  It is used the in the viewer application to populate the dropdown list of building shortcut.


			
		
Not all structures on a site may be tracked in AGI.  For instance, storage structures, car ports, or temporary buildings used for construction may not need to be tracked.  The determination of which structures on a site will be inventoried in AGI may be determined by the following criteria.
*	Does the structure have utilities?
*	Does the structure have an address?
*	Is the structure temporary?

#### Facilities Domains
| Field Name                        | Use                                           | ESRI | Notes |
| --------------------------- | ----------------------------------------------------- | ---------- | ----- |

Field Name	Use	Esri Standard	Application
#### Facilities Symbology

The facilities FC is mostly used to give reference to other layers.  It acts mainly as a basemap.  Therefore it is usually symbolized as a light colored polygon with and outline.  The outline shows how the building footprint varies from the floor plates.


#### Facilities Labels
Facility labels are not on by default.

•	field .  
•	Font  at  size.  
•	Not viewed beyond: 
•	Expression:

#### Facilities Popups

### Sites

The sites FC contains the property outlines for each building.  In most cases, this will be the parcel boundary.  The sites FC will have EPA specific data that comes from other applications.  The table below details some of the important fields that are in the EPA AGI deployment.


			
			
			

#### Site Domains
| Field Name                        | Use                                           | ESRI | Notes |
| --------------------------- | ----------------------------------------------------- | ---------- | ----- |
			
	
#### Site Symbology
Sites are like the facilities layer in that they should mostly provided context for other layers.  As such the sites FC is usually symbolized in a light color with and outline.


#### Site Labels
•	field
•	Font  at  size.  
•	Not viewed beyond: 
•	Expression:

#### Site Popups

### Occupants

### Points of Interest

<!-- ### 3D Scenes

3D scenes are used in both the viewer app and in ArcPro to help verify connectivity of network data sets.  3D scenes take a lot of CPU\GPU resources to make and use.
Wayfinding and Network Routing

The wayfinding functions of AGI rely on a network data set that is derived from several tools.  The first tool creates a lattice of all possible routes.  The second tool thins out these routes and the third tool creates the final origin/destination (OD) matrix that does all the routing.
The first two steps are very intensive operations.  For example, doing just the first step on the 8 floors of the north headquarters building took over 16 hours when working in the SDE.  It produced 1,315,260 features in the PrelimPathways FC on the SDE.  That is a lot of writing to the database.  It is more efficient to do the calculations local and then update the feature classes.
As edits are made to the floorplans, the network data sets will have to be recalculated.  This will require careful planning to make the process as efficient as possible.
I Asked Dave about the network routing GP tool performance at our weekly meeting and he said that they do the network routing on a local host using a file GDB.  It took some time to do the initial paths but after they had thinned out, the updated network data set only took about 15 minutes to resolve.
https://desktop.arcgis.com/en/arcmap/latest/extensions/network-analyst/editing-network-datasets-ways-to-edit-the-network-dataset.htm


### Wayfinding and Network Routing

The wayfinding functions of AGI rely on a network data set that is derived from several tools.  The first tool creates a lattice of all possible routes.  The second tool thins out these routes and the third tool creates the final origin/destination (OD) matrix that does all the routing.

The first two steps are very intensive operations.  For example, doing just the first step on the 8 floors of the north headquarters building took over 16 hours when working in the SDE.  It produced 1,315,260 features in the PrelimPathways FC on the SDE.  That is a lot of writing to the database.  It is more efficient to do the calculations local and then update the feature classes.

As edits are made to the floorplans, the network data sets will have to be recalculated.  This will require careful planning to make the process as efficient as possible.

I Asked Dave about the network routing GP tool performance at our weekly meeting and he said that they do the network routing on a local host using a file GDB.  It took some time to do the initial paths but after they had thinned out, the updated network data set only took about 15 minutes to resolve.
https://desktop.arcgis.com/en/arcmap/latest/extensions/network-analyst/editing-network-datasets-ways-to-edit-the-network-dataset.htm -->


# Data Operations

## Archive Cleanup
**Draft**
When branch managed editing is enabled, ArcGIS also enables archiving of edits.  We do not need these archives but we cannot disable them.  The archived edits clutter the tables and slow down the database operations.  If you open the GDB tables with a pure sql tool, you can see these extra records.  According to Esri, there is no way to turn this off.

The only way to clear the extra records is to stop the branch edit service, disable versioning on the dataset, export the data to purge the archive, and then reimport the clean tables from a file GDB to the SDE.



### Archive Cleanup Process Checklist
-	Delete all branches after merging all floor plans and space plans.
-	Shut down services on geosecureprd2 that are making connections to the SDE and locking the schema. https://geosecureprd2.epa.gov:6443/arcgis/manager/ This can only be reached from the host itself.  RDP to jumphost and then to Geosecureprd  There is an Indoors folder that has many of the services in it. - see list
 -	Use the sde connection to kick everyone out of the indoors SDE and break all the locks.  You can't do this with the arcgis account.  This may take a long time.  There may be hundreds of connections with thousands of locks.  Start by disconnecting all users.  If you select a large number of connections and select disconnect it may look like it has frozen but it is working.  Be patient.
 -	Use the sde connection to turn off Versioning on the Indoors dataset.
 -  (Test using the truncate archive tool)
 -	Export SDE feature class data to file GDB.  Use the feature class to geodatabase tool and export those classes and tables that are dirty.
 -  Rename the orginal feature classes by appending them with the export date.
 -  Import the feature classes back into the SDE from the file GDb
 -  Enable versioning on the dataset
-	Start branch editing services 
-	Test the maps and apps


## Changing the Schema

The schema is locked by the multiple services that access the SDE.  (Remember, the Viewer app uses a file GDB so it does not affect the production SDE).  The services below all have to be stopped in order to break the locks on the schema.  After the services are stopped, there will be other locks that must be manually broken.  If the locks can't be broken, you can disable logins using the SDE account.  However, the SDE account does not have permissions on the geodatabase tables so you have to turn logins back on and then switch to the arcgis account and make the schema changes before any reconnections are made that will lock the schema.

After the schema is changed, the services will not pickup the new config until they are republished.  You must update the map document and republish to push the changes.

connect with the sde account and disconnect all locks.
### Checklist 

-  Stop services on [GeoSecure ArcGIS Manager on GeoSecurePRD2](https://geosecureprd2.epa.gov:6443/arcgis/manager/)
	* Floor Plan Editor 
	* Space Planner
- Clear all locks
- If necessary, disable/enable logins to kick all connections off
- Make schema changes
- If necessary, update maps and republish services
## Units Use_Type Cleanup

## Details Use_Type Cleanup



## Exporting Data to FGDB

## Building Data Processes

  There are several general processes for data loading and editing.  We will also need processes to update data from applications that AGI is connected to.  For example,  facility managers will be updating floor plans by updating with changes and corrections.  Also, occupant updates will be coming from ebiz on a routine schedule as new employees are entered into the ebiz system on a daily basis.

**Scenario: Adding a building, floor, or other large data set by Innovate**

During the ArcGIS Indoors implementation process we will upload large amounts of data as we import CAD files.  These data will be done by a team of specialists at Innovate and will be mass loaded by entire floor and building.  These proceses will take hours to complete.  After the mass updates, the network will have to be recalculated.

Steps: 
1.	Innovate will get CAD files from clients.  The appropriate layers will be extracted from the CADs to a local GDB.
2.	A branch in the production database will be created for the new building.
3.	The data from the CAD created GDB will be loaded into the branch using ArcPro.
4.	The Branch will be merged back into the default SDE
5.	Network routing will be recalculated??

**Scenario: Adding a building, floor, or other large data set by Regional staff***

Some regional staff have expressed an interest in loading their own data.  In this case the CAD files will be loaded into a template file GDB by these regional staff.  The GDB will then be examined and loaded by Inovate staff using the QA\QC processes they have created.

**Scenario: Ad-hoc small geometry updates from facilities staff**
In this scenario facilities staff will be doing minor updates such as correcting room geometry or adding and updating walls, windows, and fixtures.  In addition, they may be changing attributes on features that already exist.  In this case, they will use the floor plan editor application.  This application creates a branch for a set of updates.  The user makes geometry and attribute changes and then requests for the changes to be merged back into the default branch.  The change administrators review the changes, merge them, and delete the   
Operating Processes

**Scenario: Hoteling/Hot desk: Hoteling will allow users to temporary reserve spaces to themselves for work.**
This application uses data about people that exists in the occupants table in Indoors.  It will be required to upadate the occupants table on a regular basis as employees join and leave the Agency.
Scenario: Updating occupant data from Ebusiness or other HR systems: Lorem Ipsom 
Scenario: Updating Service now information: 

	

**Quality Control Processes**

Esri has some scripts it has published to do basic quality control when importing data into AGI.  These functions do .  
Quality Control Requirements
1.	All units must have an AGI unique ID.  They may or may not have local numbering.  For instance, a janitor’s closet is space that needs to be reported on for AGI but may not need a local number or routing capability.
2.	There should be no voids in usable space.
3.	No overlapping of units.
4.	All units must have levels.  Levels will conform to the above pattern.
5.	Units use type conform to domain
a.	Naming conventions defined ab
Documentation
ESRI attribute rules [4]

***

# GIS Environments and Assets



## Production Environment

The ArcGIS Indoors production environment is hosted at https://geosecure.epa.gov. Geosecure is a standard ArcGIS portal environment that hosts other applications along with AGI.  It uses standard EPA SSO login as well as ArcGIS logins.  GeoSecure can be reached outside the VPN.

### ArcPro Projects
Day to day GIS work performed in ArcPro is being done on the Windows server Winkel (v18ovhrtay724.aa.ad.epa.gov - 137.67.224.154)  Users RDP across the VPN and login to the desktop.  The ArcPro documents are on the data drive in the folder **D:\Public\Data\ArcGISIndoors**

Projects are usually created for a particular task.  There may be other ArcPro projects but these are the ones that are used to execute the most important tasks.

<figure align="center">
<img  src="./images/IndoorsDataLocation.jpg" alt="image" width="700" height="auto">
<figcaption>Indoors Files
</figcaption>
</figure>

**Publishing to the production environment**

**Loading data into the GDB**

**Cleaning the Use_Type fields in new data**


## Staging Environment

### ArcPro Projects

GeoSecureSTG

	Map: Indoors_BV - Indoors Branch Version

	Map: 


DomainsAndCleaning

	**D:\Public\Data\ArcGISIndoors\Staging\Domains\DomainsAndCleaning**
