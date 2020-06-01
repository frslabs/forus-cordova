##  FORUS - Cordova Plugin

### 1. Introduction

Forus SDK comes with a simple screen with multiple instructions to capture a perfect KYC compliant photograph. The SDK comes with compression, blur and exposure detection as standard.

#### 2. Adding plugin to the app

- Extract the plugin ZIP file `FORUS.zip`

- Update the maven credentials in `/path/to/FORUS/src/android/FORUS.gradle` file

- Add the plugin to your app
  
    `cordova plugin add /path/to/FORUS`
    
 ## FOR IOS 
 
 - Add the swift support to cordova app :  `cordova plugin add cordova-plugin-add-swift-support --save`
 
 - Add camera permissions to your app to capture picture
    `cordova plugin add cordova-plugin-ios-camera-permissions --variable CAMERA_USAGE_DESCRIPTION="To capture the face"`
    
 ###### Save/Edit Netrc settings to install custom pod

You will need a valid netrc credentials to install octus from maven, which can be obtained by contacting `support@frslabs.com`. 

1. Create or edit .netrc file under current user's home directory
2. Write the below lines into that file, replace <YOUR_USERNAME> and <YOUR_PASSWORD> with your credentials which is shared through email and save the file.
```ruby
machine octus-ios.repo.frslabs.space
login <YOUR_USERNAME>
password <YOUR_PASSOWRD>
```
3. In terminal enter below command to install the pod
pod install or pod update.
    
    
 - After adding the plugin you need to install the sdk through pod 
    1: first check with this command to see if all the details are added correctly
                `open podfile` 
    2: It should display the following details:

        `# DO NOT MODIFY -- auto-generated by Apache Cordova
        source 'https://gitlab.com/frslabs-public/ios/forus.git'
        source 'https://github.com/CocoaPods/Specs.git'
        platform :ios, '12.0'   //If changes are needed to platform ios version, you can change it here
        use_frameworks!
        target 'forus' do
            project 'forus.xcodeproj'
            pod 'Forus', '~> 1.0.1'
        end`
    
    3: Use this command to install :  `pod install`
    4: Then build the project to add the plugin  :  `cordova build ios`


#### 3. Usage

- Load the plugin
  
  ```javascript
  ForusPlugin = cordova.require("com.frslabs.cordova.plugin.forus.FORUS")
  ```

- Initialize configuration for Android 
  
  ```javascript
  var forusConfig = {
      "licence_key" : "<LICENCE_KEY>",
      "instruction_flag" : "TRUE", // TRUE or FALSE
      "camera" : "1", // 1 - Front, 2 - Back, 3 - Front or Back
      "face_capture_time_limit" : "8", // 8 to 60 sec
      "eye_blink_flag" : "TRUE", // TRUE or FALSE
      "auto_adjust_exposure_flag" : "TRUE", // TRUE or FALSE
      "check_image_quailty_flag" : "TRUE", // TRUE or FALSE
      "image_quality_threshold" : "2", // 1 - Low, 2 - Medium , 3 - High
      "eye_blink_message" : "1", // 1 to 4
      "detect_smile_flag" : "FALSE" // TRUE or FALSE
  }
  ```
- Initialize configuration for IOS 

- Passing the input parameters from index.js to native code
    ```Javascript
        var inputParamsDict = {};
        inputParamsDict['SHOW_INSTRUCTION'] = 'STATUS_NO';
        inputParamsDict['CAMERA_MODE'] = 'CAMERA_FRONT';
        inputParamsDict['SECURITY_LEVEL'] = 'SECURITY_MODE3';           //face and smile detection
  //        inputParamsDict['SECURITY_LEVEL''] = 'SECURITY_MODE1';       //face detection
  //        inputParamsDict['SECURITY_LEVEL'] = 'SECURITY_MODE2';       //face and eye blink detection
  //        inputParamsDict['SECURITY_LEVEL'] = 'SECURITY_MODE4';       //face and randomaly selected eyeblink and smile     detection.
        inputParamsDict['LICENCE_KEY'] = 'your licence key';
    ```

- Create callback methods
  
  ```javascript
  var successCallBack = function success(result){ 
      console.info(result);
  }
  ```
  
  ```javascript
  var failureCallBack = function error(result){
      console.info(result);
  }
  ```

- Invoke the Android plugin
  
  ```javascript
  ForusPlugin.invokeForus(forusConfig, successCallBack, failureCallBack);
  ```
- Invoke the IOS plugin 

  ```Javascript
        FORUS.invokeForusSDK(inputParamsDict,successCallback,errorCallback);
  ```

- Android Sample success reponse
  
  ```javascript
  data: "{"eyeBlinkDetected":true,"faceImagePath":"/data/data/io.cordova.hellocordova/files/facesdk_images/Image-4937.jpg","smileDetected":false,"log":"1,1,8,1,1,2,1,1,0,0|17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,17,0,0,0,0,18,0,0,0,0,0,18,0,0,0,0,0,18,0,0,7,0,18,8,8,11,18,11,11,18,12,12,18,12,12,18,12,13,18,12,14,17"}"
  ```

- Android Sample failure response
  
  ```javascript
  error: "{"message":"Invalid licence","code":"302"}"
  ```

        For all error codes please refer to the android SDK document.

- IOS Sample success reponse    
    ```Javascript
        "faceImagePath" =  "/var/mobile/Containers/Data/Application/43B20C4E-5087-4881-95B8-05FFD3DC6F21/Documents/faceImage.png"
    ```

- IOS Sample failure response
    ```Javascript
        000    Sucess
        901    Timeout
        902    No eyeblink/smile detected
        903    Licence expired
        904    Invalid licence
        905    Transaction failed
    ```
