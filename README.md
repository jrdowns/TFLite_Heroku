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

### Creating Our CSS File
Here we'll style our text box, button, and result output:  
```
body {
  font-family: Arial, Helvetica, sans-serif;
  padding: 20px;
}

.textarea-container {
  display: flex;
}

textarea {
  border: 1px solid #999;
  border-radius: 4px;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 18px;
  height: 60px;
  padding: 8px;
  width: 500px;
}

.btn {
  align-items: center;
  /* background: linear-gradient(90deg, #ff6f00, #ff9100); */
  background: blue;
  border: 1px solid #ccc;
  border-radius: 6px;
  color: white;
  cursor: pointer;
  display: flex;
  font-size: 14px;
  height: 64px;
  justify-content: cetner;
  margin-left: 4px;
  opacity: .9;
  padding: 2px 18px;
}

.btn:hover {
  opacity: 1;
}

.result {
  margin-top: 6px;
  color: #888;
}
```

### Creating Our JavaScript File
Here we'll use TensorFlow.js to load our model and use it to make a prediction of whether the movie review is possitive or negative based on the text input:  
```
async function start() {
  const textarea = document.querySelector("textarea");
  const resultDiv = document.querySelector(".result");

  // Load the TFLite model.
  const model = await tfTask.NLClassification.CustomModel.TFLite.load({
    model:
      "https://storage.googleapis.com/tfweb/models/movie_review_sentiment_classification.tflite"
  });

  document.querySelector(".btn").addEventListener("click", async () => {
    // Get the classification result for the entered text
    const result = await model.predict(textarea.value);

    // Show the results.
    resultDiv.textContent = result.classes
      .map((c) => `${c.className}: ${c.score.toFixed(3)}`)
      .join(", ");
  });
}

start();
```
It's that easy to load a TFLite model into our web app.

### Creating Our App File
Since we're keeping it simple, all we need to do is tell Flask where to route its root page too.  
In this case, it's `index.html`.  
```
from flask import Flask, render_template

app = Flask(__name__)

@app.route("/")
def index():
    return render_template("index.html")

if __name__ == "__main__":
    app.run(debug=True)
```

### Flask Template Rendering
To generate HTML from within Python, Flask configures the Jinja2 template engine for you automatically.  
To render a template you can use the render_template() method. Here we use it to render our `index.html` page.

### Creating Our Requirements File
The requirements file will make sure all of the dependencies we need to run our app get automatically installed.  
```
Flask
gunicorn==19.9.0
itsdangerous==1.1.0
Jinja2==2.10.1
MarkupSafe==1.1.1
Werkzeug==0.15.5
```

### Creating Our Procfile
Since we're deploying our app to Heroku, we'll need a Procfile to tell Heroku what to do.
In this case, it's pretty simple:  
```
web: gunicorn app:app
```

## Deploy To Heroku
Heroku is a cloud application platform. Depending our your resource needs, you can deploy small applications completely free.  
So, log into https://dashboard.heroku.com/apps or create an account if you don't have one.  

### Create Our Heroku App
At the top right of your app dashboard, click `New` then `Create new app`.
Give your application a name and then click `Create App`.  
Now, we'll need to add our code to our app...

### Integrating GitHub
We can easily push code to our Heroku application be enabling integration with our GitHub account and respository.  
Under your application's `Deploy` tab, scroll down to `Deployment Method` and select GitHub.  
All Heroku access to your GitHub, select the repository you've pushed your code to, and enabled automatic deployment.  
This will allow any code changes you make to the GitHub repo to be automatically pulled into the Heroku app where it will be redeployed.  
Now click `Deploy Branch` and manually deploy your build for the first time.  

## Test
Now that our app is deploy, we should be able to use it.  
Click the `Open App` button to go to you app's web page.  

You should see a text box to input a movie review and a button to send the review to our model for evaluation.  
Enter a movie review, click `Analyze`, and voila! We should see our model's classification of wether the review is negative or positive.  
![image](https://user-images.githubusercontent.com/13874365/151497824-72ad7ebf-ea9b-4cb5-866f-2717f24e52b8.png)



## Links
- TensorFlow Hub: https://www.tensorflow.org/hub
- TensorFlow Lite: https://www.tensorflow.org/lite
- TensorFlow.js: https://www.tensorflow.org/js
- TFLite Movie Review Model: https://www.tensorflow.org/lite/examples/text_classification/overview
- Flask Quickstart Guide: https://flask.palletsprojects.com/en/2.0.x/quickstart/
- Heroku Dashboard: https://dashboard.heroku.com/apps
