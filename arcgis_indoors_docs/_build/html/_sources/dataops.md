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

