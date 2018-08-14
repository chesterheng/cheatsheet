#### Introduction to Geoprocessing Scripts Using Python
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
