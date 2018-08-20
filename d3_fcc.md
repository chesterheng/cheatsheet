##### Add Document Elements with D3
```
<body>
  <script>
    const anchor = d3.select("body").append("h1").text("Learning D3");
  </script>
</body>
```

##### Select a Group of Elements with D3
```
<body>
  <ul>
    <li>Example</li>
    <li>Example</li>
    <li>Example</li>
  </ul>
  <script>
    const anchors = d3.selectAll("li").text("list item");
  </script>
</body>
```

##### Work with Data in D3
```
<body>
  <script>
    const dataset = [12, 31, 22, 17, 25, 18, 29, 14, 9];
    d3.select("body").selectAll("h2")
      .data(dataset)
      .enter()
      .append("h2")
      .text("New Title");
     </script>
</body>
```

##### Work with Dynamic Data in D3
##### Add Inline Styling to Elements
##### Change Styles Based on Data
##### Add Classes with D3##### Add Classes with D3
##### Update the Height of an Element Dynamically
##### Change the Presentation of a Bar Chart
##### Learn About SVG in D3
Not PassedDisplay Shapes with SVG
Not PassedCreate a Bar for Each Data Point in the Set
Not PassedDynamically Set the Coordinates for Each Bar
Not PassedDynamically Change the Height of Each Bar
Not PassedInvert SVG Elements
Not PassedChange the Color of an SVG Element
Not PassedAdd Labels to D3 Elements
Not PassedStyle D3 Labels
Not PassedAdd a Hover Effect to a D3 Element
Not PassedAdd a Tooltip to a D3 Element
Not PassedCreate a Scatterplot with SVG Circles
Not PassedAdd Attributes to the Circle Elements
Not PassedAdd Labels to Scatter Plot Circles
Not PassedCreate a Linear Scale with D3
Not PassedSet a Domain and a Range on a Scale
Not PassedUse the d3.max and d3.min Functions to Find Minimum and Maximum Values in a Dataset
Not PassedUse Dynamic Scales
Not PassedUse a Pre-Defined Scale to Place Elements
Not PassedAdd Axes to a Visualization
