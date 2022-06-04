# Wacayang, Indonesian Wayang App

## About
Wacayang is a mobile application that able to identify Indonesian wayang kulit characters. This app uses images uploaded by the user as input, and the information about the name, description, image, and related video about the identified wayang character will be displayed on the app.

## App Features
![Feature 1](https://github.com/Wacayang-Bangkit-2022/Wacayang-Documentation/blob/main/assets/feature%20(1).png)
![Feature 2](https://github.com/Wacayang-Bangkit-2022/Wacayang-Documentation/blob/main/assets/feature%20(2).png)
![Feature 3](https://github.com/Wacayang-Bangkit-2022/Wacayang-Documentation/blob/main/assets/feature%20(3).png)
![Feature 4](https://github.com/Wacayang-Bangkit-2022/Wacayang-Documentation/blob/main/assets/feature%20(4).png)
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
![Integration Method](https://github.com/Wacayang-Bangkit-2022/Wacayang-Documentation/blob/main/assets/integration.png)
Integration method explanation:
1. Android app send a network request using Retrofit library. This request has JWT token as Authorization header.
2. Cloud Run acts as service to serve request from the app. It will verify the token first before proceed.
3. After token is verified, Cloud Run services will access back-end app depending on the request. If it looking for user or wayang information, it will query to SQL database. If it looking for wayang image prediction, then it will post that image to ML model.
4. After back-end app finished processing, Cloud Run services will return the result as JSON literals to the Android app.
5. Android app will process the JSON literals and show relevant information to the user.

## Android Studio Project Installation
### Components
Wacayang Android app is developed using Android Studio IDE. Here are components that we used.
* Developed using [Kotlin](https://kotlinlang.org/) language.
* Composed by [Activity](https://developer.android.com/reference/android/app/Activity) and [Fragment](https://developer.android.com/guide/fragments).
* Using [RecycleView](https://developer.android.com/guide/topics/ui/layout/recyclerview) and its adapater for item listing.
* [CameraX](https://developer.android.com/training/camerax) to utilize mobile camera features including flash, front/back camera, and more.
* Using [Retrofit](https://square.github.io/retrofit/) library for network request.
* Using official [Youtube API](https://developers.google.com/youtube/android/player) for Youtube video player.
* [BottomNavigation](https://developer.android.com/reference/com/google/android/material/bottomnavigation/BottomNavigationView) to navigate between main menus (Home, Favorite, and Settings).
* Connected to [Firebase Auth](https://firebase.google.com/docs/auth) for Google and anonymous sign in.
* Utilize [LiveData](https://developer.android.com/topic/libraries/architecture/livedata), [ViewModel](https://developer.android.com/topic/libraries/architecture/viewmodel), and [Repository](https://developer.android.com/codelabs/basic-android-kotlin-training-repository-pattern) pattern for Single Source of Truth (SSOT)
### Workflow
#### 1. Clone The Project and Open It in Android Studio
```
git clone https://github.com/Wacayang-Bangkit-2022/Wacayang-MobileDev.git
```
#### 2. Connect the Project to Your Firebase Auth
* Head to your [Firebase Console](https://console.firebase.google.com/).
* Then create or use your existing Firebase project.
* Activate Firebase Authentication feature.
* Open `Project Settings -> General`, select `New App`, and choose Android app.
* Fill debug SHA1 fingerprint. You can find this by execute `Gradle -> signingReport` on Android Studio.
* After the new Firebase app added, you will see your debug SHA1 added to that Firebase app. You can add another SHA1 such as your signed app SHA1. You can find your signed app SHA1 by executing this command on terminal. But, you will need to create your [Keystore](https://developer.android.com/studio/publish/app-signing#generate-key) first
```
keytool -list -v -keystore <your keystore path> -alias <your alias>
```
* Install [Firebase SDK](https://developer.android.com/studio/write/firebase) to your Android Studio project.
* Download the `google-service.json` from your Firebase Console, and copy it to the `app` folder of your Android Studio project.
#### 3. Run or Build The App
After you open the project, wait for the Gradle to finish building first. Then you can choose to build debug app by using `Run -> Run'app'`. Or you can build signed App by head to `Build -> Generate Signed Bundle/APK`.
## Cloud Computing Project Installation
### Components
* SQL database running on Google Cloud Platform
* REST API developed using Node.JS, Express, and Flask.
* Deployed REST API running as service on Google Cloud Run.
### Workflow
#### 1. Clone The Project and Open It in Your Favorite IDE
```
git clone https://github.com/Wacayang-Bangkit-2022/Wacayang-CloudComputing.git
```
#### 2. Get Service Account Key from Firebase
* Open your last Firebase project used for Android Studio project installation
* Go to `Project Settings -> Service Accounts`
* Choose `Generate New Private Key`, and your private key JSON file will be downloaded.
* Rename your private key as `serviceAccountKey.json`
* Copy that file to your cloned project, both inside `wacayang_general_api` and `wacayang_ai_api` folder.
#### 3. Create SQL Instance on GCP
* Open your Cloud Console, head to `SQL -> MySQL -> Create New Instance`
* Create new database on your newly created SQL instance.
* Create necessary tables as showed on this schema below.
![Database Schema](https://github.com/Wacayang-Bangkit-2022/Wacayang-Documentation/blob/main/assets/schema.png)
* Setup your database connection such as `DB_USER`, `DB_PASS` on the `Connection` tab.
#### 4. Deploy REST API to Cloud Run
* There will be two service running on Cloud Run.
* First, go to `wacayang_general_api` folder via terminal. And run `gcloud run deploy` command. Fill the rest of required fields. When it finish, it should shows your deployed service URL.
* Do the same for `wacayang_ai_api` folder, run `gcloud run deploy` command. Fill the rest of required fields. When it finish, it should shows your deployed service URL.
* These URL will be used by Android app to send network request. Replace the URL on `ApiService.kt` inside your Android Studio project to make it works with your deployed API.
* Go to your Cloud Console then head to your Cloud Run tab.
* Open your deployed `wacayang_general_api` service and create new revision by choosing `Edit & Deploy New Revision`.
* On Variable and Secrets tab, add new variable for `DB_NAME`, `DB_USER`, `DB_PASS`, and `INSTANCE_CONNECTION_NAME`. This necessary so your service can connect to your database. Fill these information based on your SQL database instance, then choose `Deploy` to create new revision.
#### Our Deployed REST API URL
You can send an e-mail to **nauvalmfirdaus@gmail.com** to access our API. We won't disclose our API here to prevent unwanted GCP billing. But, here is a gist of our REST API URLs.
* Wacayang General API
```
https://wacayang-api-<encrypted-code>.a.run.app
```
* Wacayang AI API
```
https://wacayang-ai-api-<encrypted-code>.a.run.app
```
