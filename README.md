# TFLite_Heroku
Deploy a TensorFlow Lite model to Heroku

- [Overview](#overview)
- [Create Flask App](#create-flask-app)
- [Deploy To Heroku](#deploy-to-heroku)
- [Test](#test)
- [Links](#links)

## Overview
Author: James Downs  
Date: 1/21/2022  
Model: https://www.tensorflow.org/lite/examples/text_classification/overview  
App: https://tfjs-tflite-demo.herokuapp.com/  

In this project we will:
1) Create a basic Flask app to provide a GUI for our model
2) Integrate Github changes into Heroku
3) Deploy our model and app to Heroku
4) Test our model through our new Heroku app

### Using A Pretrained Model
Building your own machine learning model can be tricky and time consuming. Luckily, TensorFlow has several pre-trained models you can use to get your project up and running quicker.
TensorFlow Hub serves as a repository of pre-trained models for issues such as natural language processing, image classification, object detection, etc. 
It also includes helpful tutorials on how to import and fine-tune those models.

### What Is TensorFlow Lite?
Machine learning models can be fairly resource intensive in their execution, which limits their use on mobile and IoT devices. TensorFlow Lite aims to solve that problem by creating more resource efficient models.

We'll be using the TFLite model for movie review sentiment analysis:  
https://www.tensorflow.org/lite/examples/text_classification/overview

### TensorFlow.js
TensorFlow.js is a library for machine learning in JavaScript. It enables the deployment of machine learning models directly in the browser or in Node.js.

## Create Flask App
We'll be using Flask as our framework to serve up the model. Here's what our file structure will look like:

📦TFLite_Heroku  
 ┣ 📂static  
 ┃ ┣ 📜script.js  
 ┃ ┗ 📜style.css  
 ┣ 📂templates  
 ┃ ┗ 📜index.html  
 ┣ 📜Procfile  
 ┣ 📜README.md  
 ┣ 📜app.py  
 ┗ 📜requirements.txt  

### What Is Flask?
Flask is a micro web framework. It contains a simple but extensible core.  
In their words, "...Flask can be everything you need and nothing you don’t."  
In the spirit of efficiency, which is the point of TFLite, Flask meets our needs quite well.

### Creating Our HTML File
Our only real goal is to be able to pass some text to our a model and return its evaluation.  
So, we'll have a text box to accept some input, a button to send the input to our a model, and some text to output the result:  
```
<!DOCTYPE html>
<html lang="en" >
<head>
  <meta charset="UTF-8">
  <title>TFLite Movie Review Analysis</title>
  <script src="https://cdn.jsdelivr.net/npm/@tensorflow-models/tasks@0.0.1-alpha.8"></script>
  <link rel= "stylesheet" type= "text/css" href= "{{ url_for('static',filename='style.css') }}">
</head>
<body>
<!-- partial:index.partial.html -->
<div class="textarea-container">
  <textarea placeholder="Enter a movie review"></textarea>
  <div class="btn">
    Analyze
  </div>
</div>
<div class="result"></div>
<!-- partial -->
  <script src="{{ url_for('static',filename='script.js') }}"> </script>

</body>
</html>
```

### Static File Referencing
You may have noticed how our .css and .js files were referenced in the HTML file.  
We have to call them this way because of how Flask works.  
Flask automatically looks for static files within the `static` directory.  
You use the `url_for` function and `static` designation to generate the URLs for these files.  

### Flask Template Rendering
To generate HTML from within Python, Flask configures the Jinja2 template engine for you automatically.  
To render a template you can use the render_template() method.  

## Deploy To Heroku

### Integrating GitHub

## Test

## Links
- TensorFlow Hub: https://www.tensorflow.org/hub
- TensorFlow Lite: https://www.tensorflow.org/lite
- TensorFlow.js: https://www.tensorflow.org/js
- TFLite Movie Review Model: https://www.tensorflow.org/lite/examples/text_classification/overview
- Flask Quickstart Guide: https://flask.palletsprojects.com/en/2.0.x/quickstart/
- Heroku App: https://tfjs-tflite-demo.herokuapp.com/
