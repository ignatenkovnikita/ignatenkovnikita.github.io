---
layout: post
title: Leaflet arrow!
---

I find solution for draw arrow by Leaflet


I know, there are some libraries for drawing lines with arrows, 
like [Leaflet.TextPath](https://github.com/makinacorpus/Leaflet.TextPath) or 
[Leaflet.PolylineDecorator](https://github.com/bbecquet/Leaflet.PolylineDecorator). 
But this time I wanted to create something really simple, that could fit my defined objects.

The biggest problem with arrows is to calculate and manipulate with angles, because that’s the reason of arrows. 
The theory is quite simple, we just need to google the
 [trigonometric functions](http://en.wikipedia.org/wiki/Trigonometric_functions) and
  [Pythagorean theorem](http://en.wikipedia.org/wiki/Pythagorean_theorem). 
We will improvise a little and act like our coordinate system is cartesian. 
Here we can see each line as a hypotenuse and we are able to calculate its length with Pythagorean theorem and 
differences in latitude and longitude between two points that are defining our line.

From trigonometric functions we will need arctan ([Math.atan2](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/atan2) in javascript), that defines our angle from the 
ratio of our latitude and longitude differences. The javascript code should look something like (we also need to 
multiply out angle by 57.295779513082 to get grades) :


```js
var diffLat = points[p+1]["lat"] - points[p]["lat"]
var diffLng = points[p+1]["lng"] - points[p]["lng"]
var angle = 360 - (Math.atan2(diffLat, diffLng)*57.295779513082)
```
 

The next step is to define DivIcon class. This class is for setting custom icons for markers in leaflet map. One from the options here is “html” parameter. Here we are able to put custom html code (also with css) for displaying with our marker. I have found kind of a database with arrows in unicode and chose “&#10152;” (one problem here is, that at this point we need to choose an arrow heading right or rewrite our angle calculation to have zero somewhere else). Then our divIcon should look like this :


```javascript
new L.divIcon({ 
    className : "arrowIcon",
    iconSize: new L.Point(30,30), 
    iconAnchor: new L.Point(15,15), 
    html : "<div style = 'font-size: 20px; -webkit-transform: rotate("+ angle +"deg)'>&#10152;</div>"
    })
```
 

We could also extend Polyline class with new function and put everything inside:

```js

var arrowPolyline = L.Polyline.extend({
    addArrows: function(){
        var points = this.getLatLngs()
        for (var p = 0; p +1 < points.length; p++){ 
            var diffLat = points[p+1]["lat"] - points[p]["lat"]
            var diffLng = points[p+1]["lng"] - points[p]["lng"]
            var center = [points[p]["lat"] + diffLat/2,points[p]["lng"] + diffLng/2]
            var angle = 360 - (Math.atan2(diffLat, diffLng)*57.295779513082)
            var arrowM = new L.marker(center,{
               icon: new L.divIcon({ 
                   className : "arrowIcon",
                   iconSize: new L.Point(30,30), 
                   iconAnchor: new L.Point(15,15), 
                   html : "<div style = 'font-size: 20px; -webkit-transform: rotate("+ angle +"deg)'>&#10152;</div>"
               })
            }).addTo(map);
        }
    }
})
 
```

Working example is then on [jsfiddle](http://jsfiddle.net/eid/xP3aN/60/).

![Example](/images/arrows_sample.png "Example leaflet arrow")


[The original page](http://www.coffeegnome.net/arrows-in-leaflet/)


