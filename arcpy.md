#### Introduction to Geoprocessing Scripts Using Python

##### Lesson 01: 
* function vs object
*

##### Lesson 02: Describing Data
```python
import arcpy
arcpy.env.workspace = r"C:\EsriTraining\PYTH\Describe\Corvallis.gdb"

# Describe function returns a Describe object, with multiple properties, such as data type, fields, indexes, and many others.
# Describe object: desc
# https://desktop.arcgis.com/en/arcmap/latest/analyze/arcpy-functions/describe.htm
# https://pro.arcgis.com/en/pro-app/arcpy/functions/describe.htm
desc = arcpy.Describe('Schools')
print("Name: {}".format(desc.name))
print("Shape: {}".format(desc.shapeType))
print("Type: {}".format(desc.datasetType))
print("Name: {} Shape: {} Type: {}".format(desc.name, desc.shapeType, desc.datasetType))

# desc.fields = [<geoprocessing describe field object object at 0x069231D0>, 
# <geoprocessing describe field object object at 0x069233C8>, 
# <geoprocessing describe field object object at 0x06923CB0>]
# https://desktop.arcgis.com/en/arcmap/latest/analyze/arcpy-classes/field.htm

print ("Field names")
for field in desc.fields:
    print ("\t{}".format(field.name))

# https://desktop.arcgis.com/en/arcmap/latest/analyze/arcpy-functions/workspace-properties.htm
# https://pro.arcgis.com/en/pro-app/arcpy/functions/workspace-properties.htm
descGDB = arcpy.Describe(arcpy.env.workspace)
print ("GDB Type: {} Release: {} Path: {}".format(descGDB.workspaceType, descGDB.release, descGDB.path))

print ("Script completed")
```
##### Lesson 03: Automating Scripts with Lists
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
##### Lesson 04: Working with Selections
```python
import arcpy
arcpy.env.workspace = "C:\EsriTraining\PYTH\Selections\SanDiego.gdb"

newField1 = arcpy.AddFieldDelimiters(arcpy.env.workspace,"TYPE")
newField2 = arcpy.AddFieldDelimiters(arcpy.env.workspace,"ESTAB")
# TYPE = 'Maritime'
maritimeSQLExp = newField1 + " = " + "'Maritime'"
historicSQLExp = newField2 + " > 0 and " + newField2 + " < 1956"

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
##### Lesson 05 Working with Cursors
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
with arcpy.da.UpdateCursor("Parcel", ["SHAPE@AREA", "ACRES"]) as cursor:
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
rowValues = [
["Benton", (-123.40, 44.49)], 
["Linn", (-122.49, 44.48)],
["Polk", (-123.38, 44.89)], 
["Tillamook", (-123.65, 45.45)]]
 
with arcpy.da.InsertCursor("CountyPNT", ["NAME", "SHAPE@XY"]) as cursor:
    for row in rowValues:
        cursor.insertRow(row)

cursor = arcpy.da.InsertCursor("CountyPNT", ["NAME", "SHAPE@XY"])
for row in rowValues
    cursor.insertRow(row)
del cursor
        
print ("Script completed")
```
