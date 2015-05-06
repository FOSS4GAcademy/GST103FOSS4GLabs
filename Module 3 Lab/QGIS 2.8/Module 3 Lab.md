# GST 103: Data Acquisition and Management 
## Lab 3 - Vector Data Quality
### Objective – Learn to  Verify the Quality of Vector Data with Topology Rules

Document Version: 4/26/2015

**FOSS4G Lab Author:**
Kurt Menke, GISP
Bird's Eye View GIS

**Original Lab Content Author:**
Richard Smith, Ph.D., GISP
Texas A&M University - Corpus Christi

---

The development of the original document was funded by the Department of Labor (DOL) Trade Adjustment Assistance Community College and Career Training (TAACCCT) Grant No.  TC-22525-11-60-A-48; The National Information Security, Geospatial Technologies Consortium (NISGTC) is an entity of Collin College of Texas, Bellevue College of Washington, Bunker Hill Community College of Massachusetts, Del Mar College of Texas, Moraine Valley Community College of Illinois, Rio Salado College of Arizona, and Salt Lake Community College of Utah.  This work is licensed under the Creative Commons Attribution 3.0 Unported License.  To view a copy of this license, visit http://creativecommons.org/licenses/by/3.0/ or send a letter to Creative Commons, 444 Castro Street, Suite 900, Mountain View, California, 94041, USA.

This document continues to be modified and improved by generous public contributions.

---

### 1. Introduction

GIS data are referred to as models because they are simplified representations of actual real world objects. As the real world is represented, this often results in data with thousands of records and complex spatial relationships. With so many records, it can be challenging to verify the quality of such large data by visual means alone. additionally, to verify that spatial relationships have been maintained, topology rules we can test the data and ensure that it is well constructed.

This lab includes the following tasks:

+ Task 1 Topology Rules - Part 1
+ Task 2 Topology Rules - Part 2
+ Task 3 Fixing Topology Errors

### 2 Objective: Learn To Verify the Quality of Vector Data with Topology Rules

In this lab you will be explore the spatial relationship between points, lines, and polygons. You will build topology rules and validate them to identify data errors.

###Task 1 Topology Rules - Part 1

In this task, you will use the Topology Checker plugin to investigate the quality of two datasets: bus routes and bus stops.

2. Open QGIS Desktop and add the parcels.shp, Bus_stops.shp and Bus_routes.shp layers from the Lab Data folder to the map (shown in figure below).

![Data Layers in QGIS Desktop](figures/Data_Layers_in_QGIS_Desktop.png "Data Layers in QGIS Desktop")

3. Save your project as Lab_3.qgs in the Lab Folder.
4. From the menu bar, choose Plugins | Manage and Install Plugins.
5. Select the Installed tab and enable the Topology Checker plugin.
5. Click Vector | Topology Checker| TopologyChecker, or, click on the Topology Checker ![Topology Checker button](figures/Topology_Checker_button.png "Topology Checker button") button to open the Topology Checker panel (figure below).

![Topology Checker Panel](figures/Topology_Checker_Panel.png "Topology Checker Panel")

First, you will investigate the integrity of the Bus Stops layer.

6. Click the Configure button at the bottom of the Topology Checker panel. This opens the Topology Rules Settings window. Here you can set up a variety of topology rules.
7. Under Current rules choose Bus_stops as the layer. Click the second drop down to see what topology rules are available for point layers. Choose ‘must not have duplicates’. This rule will check to make sure there are no stacked points, in other words, a bus stop situated directly over another. This type of error is difficult to identify without a topology rule.
8. Click the Add Rule button to have the rule established (shown in figure below).

![Topology Rule Setting](figures/Topology_Rule_Setting.png "Topology Rule Setting")

8. Click OK to close the Topology Rule Settings.
9. In the Topology Checker panel, click the Validate All button.

*Note*: You can also choose to zoom into a particular area and just validate the topology rule within the current extent by clicking instead the Validate Extent button.

9. The topology checker finds three duplicate geometries which are listed in the Topology Checker panel with their Feature IDs and the rule they are violating. (shown in figure below).

![Topology Errors Found](figures/Topology_Errors_Found.png "Topology Errors Found")

Additionally the duplicated points are highlighted in red on the map. Again, these errors would be difficult to find any other way. However, once identified, they are easy to fix. Simply toggle on editing, select the duplicates and delete them. 

Now you will examine the topology of the Bus_routes layer.

10. In the Topology Checker panel, click the Configure button again.
11. Select and delete the Topology rule for the Bus Stops. 
12. Create a new rule for the Bus_routes, using 'must not have dangles'. This means the endpoint of a dangling line will be identified. You might expect that since these data are only a portion of an urban area, and there are bus routes heading off the map that those dangling endpoints will be identified. However, there should not be any in the middle of the network.
13. Click the Add Rule button and click OK (topology rule shown in figure below). 

![Bus Route Topology Rule](figures/Bus_Route_Topology_Rule.png "Bus Route Topology Rule")

12. Validate the topology. The Topology Checker finds 25 errors of this type. Many are the expected ones, for example lines heading off the map edge. However, there are several in the middle of the network. Zoom in to some of these errors and investigate.

13. Save your project.

###Task 2 Topology Rules - Part 2

Now you will implement topology rules to check the integrity of the parcels layer.

1. Open your Lab3.qgs project in QGIS Desktop if it is not already open.
2. Open the Topology Checker panel.
3. Click the Configure button.
4. Select any existing rules and click the Delete Rule button to remove them.
5. Configure three rules for the parcels layer: 'must not have gaps', 'must not overlap', and 'must not have duplicates' (rules shown in figure below).

![Parcel Topology Rules](figures/Parcel_Topology_Rules.png "Parcel Topology Rules")

6. Click OK to set the topology rules.
7. In the Topology Checker panel, click Validate All.
7. The Topology Checker will report violations for each rule, seventeen errors in all.
8. Save your project.

###Task 3 Fixing Topology Errors

Now you will edit the parcel layer to eliminate these 17 topology errors.

1. Open your Lab3.qgs project in QGIS Desktop if it is not already open.
2. Re-validate the topology rules if they are not appearing.
3. First, you will work on the duplicate geometries. Right-click on the parcels layer in the Layers panel and choose Toggle Editing.
4. Double-click on the first **duplicate geometry** error in the Topology Checker panel to zoom into that location.
5. Use the Select Feature by Rectangle tool ![Select Feature by Rectangle tool](figures/Select_Feature_by_Rectangle_tool.png "Select Feature by Rectangle tool") *and drag a small rectangle* to select the duplicate parcels on the map.
6. Open the parcels layer attribute table. 
7. Change the display filter in the lower-left corner to Show Selected Features. 
8. Notice that for the two selected features, all of the attributes are identical.
9. Select the feature with the higher row number by clicking on the row number. This leaves just one selected record.
10. Click the Delete selected features ![Delete Selected Features Button](figures/Delete_selected_features_button.png "Delete Selected Features Button") button to remove the duplicate parcel.
10. Repeat these steps to delete the remaining duplicate geometry topology errors. The attribute table should now show a total of 6,968 records.

Now that the duplicate geometry errors are fixed, we will turn our attention to fixing the overlaps and gaps errors. To fix these errors, we will first set our snapping tolerances to make editing significantly easier and more precise.

11. From the menu bar choose Settings | Snapping Options.
12. Set the Snapping mode to Advanced.
12. In the left-most column, uncheck Bus_routes and Bus_stops and check parcels.
13. Set the Mode for parcels to 'to vertex' and the Tolerance to 10 map units.
14. Check Enable topological editing.
15. Check Enable snapping on intersection.

Topological editing maintains common boundaries in polygon mosaics. With this option checked, QGIS detects a shared boundary in a polygon mosaic and you only have to move the vertex once, and QGIS will take care of updating the other boundary.

The selected snapping options are shown in the figure below.

![Snapping Options](figures/Snapping_Options.png "Snapping Options")

16. Click OK to set and close the Snapping options.
12. From the menu bar choose Settings | Options and click on the Digitizing tab.
13. Set the Search radius for vertex edits to 10 pixels (shown in figure below). Setting this to something other than zero ensures that QGIS finds the correct vertex when editing. 

![Digitizing Options](figures/Digitizing_Options.png "Digitizing Options")

14. Click OK to set the digitizing options.
13. Open the Layer properties for the parcels layer and from the Style tab set the transparency to 50.
14. Click OK to close the Layer properties.
14. On the Topology Checker panel, uncheck Show errors.
15. Double-click on the error for Feature ID 624. The map will zoom to the location of the error. With errors turned off and the transparency set, you can see the overlap issue (shown in figure below).

![Overlap Area for Feature ID 624](figures/Overlap_Area_for_Feature_ID_624.png "Overlap Area for Feature ID 624")

There are two parcels involved in the overlap. Here, the western (left) overlapping parcel boundary needs to be moved west (left) so that it does not overlap with the eastern parcel (parcel on the right).

16. Use the Select Feature by Rectangle tool ![Select Feature by Rectangle tool](figures/Select_Feature_by_rectangle_tool.png "Select Feature by Rectangle tool") to select the western (left) overlapping parcel. The vertices of the selected feature will appear as red Xs.
17. Enable the Digitizing toolbar by clicking View | Toolbars | Digitizing.
17. On the Digitizing toolbar, click on the Node tool ![Node tool](figures/Node_tool.png "Node tool"). This tool allows you to move individual feature vertices.
18. Click on a vertex of the selected feature and the vertices will show as red boxes (shown in figure below). If the vertices for a different feature appear, try clicking on a vertex, then clicking between that vertex and another vertex of the same feature along the connecting line.

![Selected Verticies](figures/Selected_Vertices.png "Selected Verticies")

18. Click on the upper right vertex and the selected vertex will turn blue (figure below).

![Selected Vertex with the Node tool](figures/Selected_vertex_with_the_Node_tool.png "Selected Vertex with the Node tool") 

19. Drag that selected vertex west (left) until it snaps to the boundary of the parcel it is overlapping.
20. Repeat for the lower right vertex.
22. In the Topology Checker panel, click Validate All and the overlap topology error will no longer be listed and the overlap will be resolved (shown in figure below).

![Overlap Resolved](figures/Overlap_Resolved.png "Overlap Resolved")

There is one other issue with this parcel. There is a tiny overlap in the northwest corner (figure below). This very small overlap cannot be seen at this scale.

![Smaller Overlap Issue](figures/Smaller_Overlap_Issue.png "Smaller Overlap Issue")

23. To resolve this, simply select the affected vertices with the Node tool, and snap them back into place. The overlap is so small you won’t see them move, however, the overlap will be resolved due to the Topological editing setting.
21. The remaining overlaps can be fixed in the same fashion.
22. From the Topology Checker panel click on the first Gap error in the list (for Feature ID 0). Again you will be zoomed to the location of the error (shown in figure below).

![Gap Error](figures/Gap_Error.png "Gap Error")

23. In the Topology Checker panel, uncheck Show errors.
24. There is a small sliver between the parcels. Select the parcel to the north and then click on the Node tool.
24. Select the vertex in the southwestern corner of the selected parcel. Drag it until it snaps with the parcel vertex to the south, closing the gap.

![Fixing the Gap Error](figures/Fixing_the_Gap_Error.png "Fixing the Gap Error")

25. Click Validate extent to ensure that the issue has been resolved. 
26. You can repair other gap errors the same way.
27. When you are done repairing the topology errors, save your edits.
28. Save your project.

### 3 Conclusion

In this lab, you learned how to test the integrity of your vector data with topology rules. These rules can involve features in two different layers or can be set to test the features in a single layer. There are different rules for points, lines and polygon features. You also learned how to use Topological editing to resolve the issues found. 

### 4 Discussion Questions

1. What are the steps involved creating and testing a topology rule?
2. Explain the use of topology in industry.
3. How would topology be useful for property data?

### 5 Challenge Assignment (optional)

See if you can think of other topology rules that could be implemented against these data sets.  Use topological editing to fix all the errors found.
