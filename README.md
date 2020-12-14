# Let's Do Yoga 
By Sarah Zaheer and Chloe Kim

## P5.js editor Link
https://editor.p5js.org/sarahzaheer999/sketches/Kz9HjDRql


## Code

```
let timer = 400
let timer2 = 300
// var counter=0;
// var timeleft = 60;

// function convertSeconds(s) {
// var min = floor(s / 60);
//   var sec = s % 60;
//   return min + ':' + sec;
// }

images = ['B.png', 'S.jpg', 'N.jpg']
imgIndex = 0
// More API functions here:
// https://github.com/googlecreativelab/teachablemachine-community/tree/master/libraries/pose

// the link to your model provided by Teachable Machine export panel
// const URL = "./my_model/";
let model, webcam, ctx, labelContainer, maxPredictions;

async function init() {
  const modelURL = "https://storage.googleapis.com/tm-model/TCZdXgIU4/model.json";
  const metadataURL = "https://storage.googleapis.com/tm-model/TCZdXgIU4/metadata.json";




  // load the model and metadata
  // Refer to tmImage.loadFromFiles() in the API to support files from a file picker
  // Note: the pose library adds a tmPose object to your window (window.tmPose)
  model = await tmPose.load(modelURL, metadataURL);
  maxPredictions = model.getTotalClasses();

  // Convenience function to setup a webcam
  const size = 400;
  const flip = true; // whether to flip the webcam
  webcam = new tmPose.Webcam(size, size, flip); // width, height, flip
  await webcam.setup(); // request access to the webcam
  await webcam.play();
  window.requestAnimationFrame(loop_);

  // append/get elements to the DOM
  const img = document.getElementById("Asana");

  img.width = size + 100;
  img.height = size;
  const canvas = document.getElementById("canvas");
  canvas.width = size + 100;
  canvas.height = size;
  ctx = canvas.getContext("2d");
  labelContainer = document.getElementById("label-container");
  for (let i = 0; i < maxPredictions; i++) { // and class labels
    labelContainer.appendChild(document.createElement("div"));
    console.log(maxPredictions)
  }
}

async function loop_(timestamp) {
  webcam.update(); // update the webcam frame
  await predict();
  window.requestAnimationFrame(loop_);
  document.getElementById('timer').innerHTML = timer
  document.getElementById('Asana').src = images[imgIndex]
}

async function predict() {
  // Prediction #1: run input through posenet
  // estimatePose can take in an image, video or canvas html element
  const {
    pose,
    posenetOutput
  } = await model.estimatePose(webcam.canvas);
  // Prediction 2: run input through teachable machine classification model
  const prediction = await model.predict(posenetOutput);

  //split 
  //const array = str.split(" ");
  
  /* for (let i = 0; i < maxPredictions; i++) {
    const classPrediction =
      prediction[i].className + ": " + prediction[i].probability.toFixed(2);
    labelContainer.childNodes[i].innerHTML = classPrediction;

  }*/
  
  const classPrediction =
      prediction[imgIndex].className + ": " + prediction[imgIndex].probability.toFixed(2);
    labelContainer.childNodes[0].innerHTML = classPrediction;
  if (timer != 0) {
    timer--;
  } else {
    timer = 400;
    
    if (imgIndex >= 0 && imgIndex < 3) {
      imgIndex++;
    } else {
      imgIndex = 0;
    }
    
    if (timer<=0) {
      if(timer2 !=0){
      timer2--;
    } else {
      timer2=300
    }
    }
  } 

  // finally draw the poses
  drawPose(pose);
}
  

function drawPose(pose) {
  for (i = 300; i > 0; i--) {
    document.getElementById('timer').innerHTML = i
  }

  if (webcam.canvas) {
    ctx.drawImage(webcam.canvas, 0, 0);
    // draw the keypoints and skeleton
    if (pose) {
      const minPartConfidence = 0.5;
      //tmPose.drawKeypoints(pose.keypoints, minPartConfidence, ctx);
      //tmPose.drawSkeleton(pose.keypoints, minPartConfidence, ctx);
    }

  }
}


let bgm;

function preload() {
  print("preload?");
  bgm = loadSound("cp2_final_bgm.mp3")
}


function setup() {
  bgm.play();
  
//   var timer = select('#timer')
//   timer.html(convertSeconds(timeleft - counter));
  
//   function timeIt() {
//     counter++;
//     timer.html(convertSeconds(timeleft - counter));
//   }
//   setInterval(timeIt,1000)
}

```





