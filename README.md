# TFLite_Heroku
Deploy a TensorFlow Lite model to Heroku

- [Overview](#overview)
- [Import Model](#import-model)
- [Save Model](#save-model)
- [Convert to TFLite](#convert-to-tflite)
- [Create Flask App](#creat-flask-app)
- [Integrate Github](#integrate-github)
- [Test](#test)

## Overview
Author: James Downs
Date: 1/21/2022

In this project we will:
1) Import a model from TensorFlow Hub
2) Save the model in the `SavedModel` format
3) Conver the model to a TensorFlow Lite model
4) Create a basic Flask app to prove a GUI for our model
5) Integrate Github changes into Heroku
6) Deploy our model and app to Heroku
7) Test our model through our new Heroku app

## Import Model
In this section, we will import a model from TensorFlow Hub
### TensorFlow Hub
Link: https://www.tensorflow.org/hub
Description: TensorFlow Hub is TensorFlow's official repository of pre-trained models.
You can find 
