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

print ("Field names")
for field in desc.fields:
    print ("\t{}".format(field.name))

descGDB = arcpy.Describe(arcpy.env.workspace)
print ("GDB Type: {} Release: {} Path: {}".format(descGDB.workspaceType, descGDB.release, descGDB.path))

print ("Script completed")
```
##### Lesson 03: Automating Scripts with Lists
```python
import arcpy
arcpy.env.workspace = r"C:/EsriTraining/PYTH/Automate/SanDiego.gdb"

fc_list = arcpy.ListFeatureClasses()

for featClass in fc_list:
    desc = arcpy.Describe(featClass)
    print("Name: {} Shape: {} Type: {}".format(desc.name, desc.shapeType, desc.datasetType))

print ("Script completed")
```
