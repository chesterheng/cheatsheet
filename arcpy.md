#### Introduction to Geoprocessing Scripts Using Python

##### Running Scripts in Python 
* ArcPy functions: https://desktop.arcgis.com/en/arcmap/latest/analyze/arcpy-functions/alphabetical-list-of-arcpy-functions.htm
* ArcPy classes: https://desktop.arcgis.com/en/arcmap/latest/analyze/arcpy-classes/alphabetical-list-of-arcpy-classes.htm

##### Describing Data
```python
import arcpy
arcpy.env.workspace = r"C:\EsriTraining\PYTH\Describe\Corvallis.gdb"

desc = arcpy.Describe('Schools')
print ("Name: {}".format(desc.name))
print ("Name: {} Shape: {} Type: {}".format(desc.name, desc.shapeType,
                                            desc.datasetType))
print ("Name: {} Shape: {} Type: {}".format(desc.name, desc.shapeType,
                                            desc.datasetType))

print ("Field names")
for field in desc.fields:
    print ("\t{}".format(field.name))

descGDB = arcpy.Describe(arcpy.env.workspace)
print ("GDB Type: {} Release: {} Path: {}".format(
    descGDB.workspaceType, descGDB.release, descGDB.path))

print ("Script completed")
```

##### Automating Scripts with Lists
```python
import arcpy
arcpy.env.workspace = r"C:\EsriTraining\PYTH\Automate\SanDiego.gdb"

fc_list = arcpy.ListFeatureClasses()
# fc_list = [u'Climate', u'Freeways', u'MajorAttractions', u'Ocean', u'Railroads', u'Streams', u'Zipcodes', u'Rail100']

for featClass in fc_list:
    desc = arcpy.Describe(featClass)
    print("Name: {} Shape: {} Type: {}".format(desc.name, desc.shapeType, desc.datasetType))

print ("Script completed")
```

##### Working with Selections
```python
import arcpy
arcpy.env.workspace = "C:\EsriTraining\PYTH\Selections\SanDiego.gdb"

maritimeSQLExp = "TYPE = 'Maritime'"
historicSQLExp = "ESTAB > 0 and ESTAB < 1956"

# Make Feature Layer in Memory
arcpy.MakeFeatureLayer_management("Climate", "MaritimeLyr", maritimeSQLExp)
arcpy.MakeFeatureLayer_management("MajorAttractions", "HistoricLyr", historicSQLExp)

# Select Layer By Location
arcpy.SelectLayerByLocation_management("HistoricLyr", "COMPLETELY_WITHIN", "MaritimeLyr", "", "NEW_SELECTION")

# Count number of features
featCount = arcpy.GetCount_management("HistoricLyr")
print ("Number of historic features selected: {}".format(featCount))

print ("Script completed")
```

##### Working with Cursors
###### Search Cursor
```python
import arcpy
arcpy.env.workspace = "C:\EsriTraining\PYTH\Cursors\SanDiego.gdb"

fc = "MajorAttractions"
fields = ["NAME", "ADDR", "CITYNM", "ZIP"]
with arcpy.da.SearchCursor(fc, fields) as cursor:
    for row in cursor:
        print ("{0}\n{1}\n{2}, CA {3}\n".format(row[0], row[1], row[2], row[3]))

print ("Script completed")
```
###### Update Cursor
```python
import arcpy
arcpy.env.workspace = "C:\EsriTraining\PYTH\Cursors\Corvallis.gdb"

fc = "Parcel"
fields = ["SHAPE@AREA", "ACRES"]
with arcpy.da.UpdateCursor(fc, fields) as cursor:
    for row in cursor:
        # read data from column 0
        geom = row[0]
        # update changes to column 1
        row[1] = geom / 43560
        # update current row to GDB
        cursor.updateRow(row)

print ("Script completed")
```
###### Insert Cursor
```python
import arcpy
arcpy.env.workspace = "C:\EsriTraining\PYTH\Cursors\SanDiego.gdb"

fc = "CountyPNT"
fields = ["NAME", "SHAPE@XY"]
rowValues = [["Benton", (-123.40, 44.49)], ["Linn", (-122.49, 44.48)],["Polk", (-123.38, 44.89)], ["Tillamook", (-123.65, 45.45)]]
 
with arcpy.da.InsertCursor(fc, fields) as cursor:
    for row in rowValues:
        cursor.insertRow(row)
        
print ("Script completed")
```

##### Sharing scripts
```python
import arcpy
arcpy.env.workspace = "C:\EsriTraining\PYTH\Sharing_scripts\Corvallis.gdb"
arcpy.env.overwriteOutput = True

### Obtain script parameter values
distance = 500
output_FC = "output"
#distance = arcpy.GetParameterAsText(0)
#output_FC = arcpy.GetParameterAsText(1)

SQLExp = "PARK_NAME = 'Central Park'"

### Create Feature Layers
# Create Parks feature layer for Central Park
arcpy.MakeFeatureLayer_management("Parks", "CentralPark", SQLExp)

# Create Parking Meters feature layer for selection
arcpy.MakeFeatureLayer_management("ParkingMeters", "Meters")

### Perform spatial selection
# Select all Meters that are within specified distance of Central Park
arcpy.SelectLayerByLocation_management("Meters", "WITHIN_A_DISTANCE",
                                       "CentralPark", distance,
                                       "NEW_SELECTION")
### Update Flag field
with arcpy.da.UpdateCursor("Meters", ["FLAG"]) as cursor:
    for row in cursor:
        row[0] = "Y"
        cursor.updateRow(row)

### Copy selected meters to new feature class
arcpy.CopyFeatures_management("Meters", output_FC)

### Report selected meter count
count = arcpy.GetCount_management(output_FC)
print ("Number of meters to program: {0}".format(count))

print ("Script completed")
```
##### Automating map production
```python
## Step 1
import arcpy.mapping as MAP

mxd = MAP.MapDocument(r"C:\EsriTraining\PYTH\Map_production\CorvallisMeters.mxd")
df = MAP.ListDataFrames(mxd)[0]

## Step 2
updateLayer = MAP.ListLayers(df, "ParkingMeters")[0]
sourceLayer = MAP.Layer(r"C:\EsriTraining\PYTH\Map_production\ParkingMeters.lyr")
MAP.UpdateLayer(df, updateLayer, sourceLayer, True)

addLayer = MAP.Layer(r"C:\EsriTraining\PYTH\Map_production\Schools.lyr")
MAP.AddLayer(df, addLayer)

refLayer = MAP.ListLayers(df, "Schools")[0]

## This is the tricky step.  The order of the arguments appears to be backwards.
MAP.MoveLayer(df, refLayer, updateLayer, "BEFORE")

## Step 3
mxd.title = "Corvallis Meters Map"
elemList = MAP.ListLayoutElements(mxd, "TEXT_ELEMENT")

for elem in elemList:
    if elem.name == "Corvallis Meters":
        elem.text = "Corvallis Parking Meters Inventory Report"

#mxd.saveACopy(r"C:\EsriTraining\PYTH\Map_production\CorvallisMeters_ks.mxd")
del mxd
```
