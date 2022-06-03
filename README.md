# Wacayang, Indonesian Wayang App

## About
Wacayang is a mobile application that able to identify Indonesian wayang kulit characters. This app uses images uploaded by the user as input, and the information about the name, description, image, and related video about the identified wayang character will be displayed on the app.

## Features
* Search various Indonesia wayang by typing on search bar.
* Upload image from camera or gallery to predict its wayang character using machine learning.
* Information about Indonesian wayangs inclunding name, story, images, and related wayang shows from local Indonesian puppeter.
* Personalization to add and remove wayang to/from favorite library.
* Share thoughts and knowledge through posting comments.
* Secured account sign in using Google Firebase JWT token.

## Technology
* Android Studio IDE to develop the Android application.
* Google Cloud Platform as deployment platform for REST API, database, and ML model.
* Tensorflow and Keras to develop, train, and deploy ML model.

## Integration Method
Wacayang consists of three components, Android, Cloud Computing, and Machine Learning. Basically, to integrate these, Cloud Computing acts as service to bridge communication between Android and Machine Learning. Here is a simple illustration on how our integration method works.
![Integration Method](https://github.com/Wacayang-Bangkit-2022/Wacayang-Documentation/blob/main/assets/_integration_method.png)
<br /><br />
Integration method explanation:
1. Android app send a network request using Retrofit library. This request has JWT token as Authorization header.
2. Cloud Run acts as service to serve request from the app. It will verify the token first before proceed.
3. After token is verified, Cloud Run services will access back-end app depending on the request. If it looking for user or wayang information, it will query to SQL database. If it looking for wayang image prediction, then it will post that image to ML model.
4. After back-end app finished processing, Cloud Run services will return the result as JSON literals to the Android app.
5. Android app will process the JSON literals and show relevant information to the user.

## How to Replicate
This section shows how to replicate our project. There are three sub-section, 1) Android App, 2) Cloud Computing, 3) Machine Learning. Each sub-section explains how to replicate that particular section. If you are looking for the integration method, please refer to **Integration Method** section.
### 1. Android App

### 2. Cloud Computing

### 3. Machine Learning
