# Choropleth-Map
Choropleth Map

Visualize Data with a Choropleth Map

This is a freeCodeCamp Choropleth Map project, we import some JSON data with geographical and educational data about counties in the United States and use d3 to render a Choropleth Map, by converting topojson into geojson and then using the geoPath() method to draw paths.

Source code can be found in this repo.



Here's the [Visualize Data with a Choropleth Map](https://sarvade.github.io/Choropleth-Map/):
<iframe src="https://sarvade.github.io/Choropleth-Map/" frameborder="0" height="550" width="800" scrolling="no" title="3D New York City Demonstration"></iframe>


## Project Setup

Looking at what we need to create and talking through the structure of the task.

- Create Skeleton Page, set title
- Import D3, TopoJSON, and Test Suite
- Create and Link Script Page
- Create and Link Stylesheet
- Create SVG Canvas with id
- Set body display and svg bg

## Creating Variables and Functions

- countyURL points to the freeCodeCamp county map data JSON
- educationURL points to the freeCodeCamp education data JSON
- countyData will be used to store an array of data to give to d3 to draw the map
- educationData will be used to store an array of data to give to d3 to associate education information with the counties
- canvas is a d3 selection of the svg canvas to reference easily
- drawMap() is a function that will draw the map, once we have obtained the data


```jsx
let countyURL = 'https://cdn.freecodecamp.org/testable-projects-fcc/data/choropleth_map/counties.json'
let educationURL = 'https://cdn.freecodecamp.org/testable-projects-fcc/data/choropleth_map/for_user_education.json'

let countyData
let educationData

let canvas = d3.select('#canvas')

let drawMap = () => {
 
}
```

## Fetching the Data

- Create variables to store both data sets
- For each, call the d3 json method, giving as a string the url of the json
- This works asynchronously and returns a promise, so call the .then() method to give a function to run when the promise is completed
- This function takes in the data and error (if unsuccessful)
- If there is an error, log it, otherwise set the data to our variables
- d3 will automatically convert them into JavaScript Objects
- I've embedded one inside the other so that we can add some flow to these async functions
- Finally, we will run the drawMap() function to draw the map once everything is ready

```jsx
d3.json(countyURL).then(
    (data, error) => {
        if(error){
            console.log(error)
        }else{
            countyData = data
            console.log('County Data')
            console.log(countyData)

            d3.json(educationURL).then(
                (data, error) => {
                    if(error){
                        console.log(error)
                    }
                    else{
                        educationData = data
                        console.log('Education Data')
                        console.log(educationData)
                        drawMap()
                    }
                }
            )

        }
    }
)
```
## Understanding and Converting the Data we need

- The actual data we need is in the objects > counties
- The geometry field is an array with a set of objects for each county
- They have an id in the form of a number
- They also have an array of 'arcs' which will be used to create paths. These reference the arcs object which has an array of objects with coordinates to draw each arc

This is currently in a format called topoJSON, but d3 only supports geoJSON to draw paths with, so we need to convert it.

We can do this using the feature() method from topoJSON, giving it the data set as the first argument, and what we want to extract as the second argument (just the counties):

```jsx
countyData = data
countyData = topojson.feature(countyData, countyData.objects.counties)
console.log('County Data')
console.log(countyData)
```







