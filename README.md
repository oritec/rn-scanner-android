# React Native Document Scanner for Android (Using OpenCV)
This library is (almost completely) based on [this](https://github.com/Diastorm/rn-doc-scanner-android) one. 
It just fixes the rotation of images and OpenCV library dependency.

## Installation
```
yarn add https://github.com/oritec/rn-scanner-android.git
``` 

Then, open `android/build.gradle` and inside allprojects -> repositories:

```  
allprojects {
    repositories {
        ...
        mavenCentral()
        maven {
            url 'https://maven.google.com'
        }
        maven {
            url "http://dl.bintray.com/steveliles/maven"
        }
        maven {
            url "https://jitpack.io"
        }
    }
}
```

Open `android/app/src/main/AndroidManifest.xml`, and inside manifest tag include:

```
<manifest ...
    xmlns:tools="http://schemas.android.com/tools"
>
```

Inside application tag:

```
<application
    ...
    tools:replace="android:allowBackup"
>
```

Finally, open android/app/build.gradle and change compileSdkVersion xxx to:

```
compileSdkVersion 26
```

#### Bonus: install OpenCV library for Android

I found [this](https://medium.com/@sukritipaul005/a-beginners-guide-to-installing-opencv-android-in-android-studio-ea46a7b4f2d3) 
excellent guide to install OpenCV on your Android Projects that works just fine with this library. 
Just follow every step of the guide but using OpenCV SDK version [3.1.0](https://sourceforge.net/projects/opencvlibrary/files/opencv-android/3.1.0/OpenCV-3.1.0-android-sdk.zip/download).

## Usage

#### Simple Example
```
import React, {Component} from 'react';
import {
  View,
  Text,
  TouchableNativeFeedback,
  Image,
} from 'react-native';

import {RNDocScanner} from 'rn-scanner-android';

export default class App extends Component {
  constructor(props) {
    super(props);
    this.state = {
      foto: null,
    };
  }

  onPressScan = async () => {
    try {
      const image = await RNDocScanner.getDocumentCrop(true);
      this.setState({foto: image});
    } catch (err) {
      console.log(err);
    }
  };

  render() {
    return (
      <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
        <Text style={{marginBottom: 20}}>Oritec Document Scanner</Text>
        <TouchableNativeFeedback
          onPress={this.onPressScan}
          background={TouchableNativeFeedback.SelectableBackground()}>
          <View
            style={{
              width: 250,
              backgroundColor: '#F7B44C',
              flexDirection: 'row',
              justifyContent: 'center',
              elevation: 3,
            }}>
            <Text style={{margin: 10, color: 'white', fontSize: 16}}>Scan</Text>
          </View>
        </TouchableNativeFeedback>
        <View style={{height: 250, marginTop: 50}}>
          {this.state.foto != null && (
            <Image
              source={{uri: this.state.foto}}
              style={{width: 250, height: 250}}
            />
          )}
        </View>
      </View>
    );
  }
}
```