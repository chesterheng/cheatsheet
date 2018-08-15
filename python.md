#### Getting Started with the Python Scripting Language

##### The basics of Python
* Strings, numbers, and lists
```python
# Variables can hold strings
folder = "C:/Student"
whereClause = "[STREET_NAM] = 'CATALINA'"

# Strings surrounded in double (") or single (') quotes
# Use r method for one backslash (\)
# Strings can be combined together
gdbPath = r"C:\SouthAfrica.gdb"
fc = "Roads"
fullPath = gdbPath + "\" + fc

# Strings are indexed
fc = "Street.shp"
newFC = fc[:-4]   


```
* Line continuation
* Functions, modules, and statements
* Decision making and looping
* Case sensitivity rules

```python
a = 5
print(a)

b = 5 + 6
print(b)

c = "Hello"
print(c)

d = ["Jack", "Diane", "Lisa"]
print(d[0])
print(d[2])
print(d[-1])

e = "Railroads.shp"
print(e[3])
print(e[-2])
print(e[0:8])

f = round(3.4)
print(f)

print dir(__builtins__)

g = "Streets"
h = ".shp"
print(g + h)

i = 1
print(g + str(i) + h)

x = 5
if x < 5:
    print "The number is less than 5"
elif x > 5:
    print "The number is greater than 5"
else:
    print "The number is equal to 5"

y = 1
while y < 10:
    print(y)
    y = y + 1

fcList = ["City.shp", "Roads.shp", "Railroads.shp"]
for eachFC in fcList:
    print(eachFC)

for num in range(3, 7):
    print(num)
```

##### String manipulation and text files
```python
# Open input names/addresses text file
inFile = file(r"C:\Student\GSPL\Database\NamesAddresses.txt", "r")

# Open output labels file
outFile = file(r"C:\Student\GSPL\Exercise02\Labels.txt", "w")

# Read input text file and write labels
for line in inFile.readlines():
    nmAddrList = line.split(",")
    nm = nmAddrList[0]
    ad = nmAddrList[1]
    newName = nm.replace("&", "and")
    outFile.write(newName + "\n")
    outFile.write(ad)
    outFile.write("Redlands, CA 92374" + "\n" + "\n")

# Close files
inFile.close()
outFile.close()

print("Finished")
```
    
