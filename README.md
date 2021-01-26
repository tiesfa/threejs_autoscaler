# Three.js Autoscaler :balance_scale:
Created: Early 2019
<br><br>
Models come in different shapes and sizes. In Threejs this means that you need to manually adjust the camera to frame the model/scene correctly. Autoscaler makes sure that every model gets properly framed within their canvas.

## How to embed Autoscaler in your Three.js code
__In this example the autoscaler is used with a STL loader__
1. Add the autoscale code as the last child of the loader.
```var initBoundingBox = new THREE.Box3().setFromObject(mesh);

   var negX = initBoundingBox.min[Object.keys(initBoundingBox.min)[0]];
   var posX = initBoundingBox.max[Object.keys(initBoundingBox.max)[0]];
   var modelWidth = posX - (negX);

   var negY = initBoundingBox.min[Object.keys(initBoundingBox.min)[1]];
   var posY = initBoundingBox.max[Object.keys(initBoundingBox.max)[1]];
   var modelHeight = posY - (negY);

   if (modelWidth >= modelHeight) {
    // Scale based on the width
    var scaleFactor = 2 / modelWidth;
   } else {
    // Scale based on the height
    var scaleFactor = 1.5 / modelHeight;
   }
    mesh.scale.set(scaleFactor, scaleFactor, scaleFactor);

   // Adjust model position on Y axis
   var boundingBox = new THREE.Box3().setFromObject(mesh);

   var newNegY = boundingBox.min[Object.keys(boundingBox.min)[1]];
   var newPosY = boundingBox.max[Object.keys(boundingBox.max)[1]];
   var newModelHeight = newPosY - (newNegY);

   var heightAdjust = (newModelHeight / 2) - newPosY;
   mesh.position.set(0, heightAdjust, 0);
   scene.add(mesh);
```

## What happens under the hood
1. Autoscaler will get the vectors from the model with the help of the BoundingBox.
1. The X and Y values will be stored inside variables.
   1. The width of the model gets determined by subtracting the negative x vector from the positive x vector.
   1. The height of the model gets determined by substracting the negative y vector from the positive y vector.
1. Then the model gets scaled based on which side is bigger. For example, a tall model will have a height bigger than the width. In this case the height will be scaled to fit the canvas.
1. The y axis position of the model gets calculated and centered.

## What does Three.js autoscaler look like applied on a broad and a tall model
Broad model on the left and tall model on the right. Perfectly scaled to match the canvas: ![mainpage](https://github.com/tiesfa/threejs_autoscaler/blob/main/screenshot.png)
