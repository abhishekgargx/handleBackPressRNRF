# handleBackPressRNRF
# Navigation using RNRF library + Handle Back Press events 
```js
// below code works with Android + react native + react-native-router-flux
// this is final index.js file code from where control whole app navigation
import { BackHandler, ToastAndroid } from 'react-native';
import React,{Component} from 'react';
import { Router, Scene, Actions } from 'react-native-router-flux';
import { Provider } from 'react-redux';
// as per your compoenents import them accordingly
import Home from './home';
import OtherScreen from './OtherScreen';
//variable 
var backButtonPressedOnceToExit = false;

export default class App extends Component {
    
  componentWillMount(){
    BackHandler.addEventListener('hardwareBackPress', this.onBackPress.bind(this));
  }

  componentWillUnmount(){
    BackHandler.removeEventListener('hardwareBackPress', this.onBackPress.bind(this));
    }
    
  // you can call this method anywhere like this 
  //import App from "./index";
  // let performBackPress = () => App.onBackPress();
  //to perfrom custom ui navigation (eg. header btn for back press) and it handle hardware back btn press automatically  
  static onBackPress() {
        if (backButtonPressedOnceToExit) {
            BackAndroid.exitApp();
        } else {
            if (Actions.currentScene !== 'Home') {
                Actions.pop();
                return true;
            } else {
                backButtonPressedOnceToExit = true;
                ToastAndroid.show("Press Back Button again to exit",ToastAndroid.SHORT);
                //setting timeout is optional
                setTimeout( () => { backButtonPressedOnceToExit = false }, 2000);
                return true;
            }
        }
    }
    
    render() {
      return(
        <Provider store={store}>
          <Router backAndroidHandler={this.onBackPress} >
            <Scene key="root" }>
              <Scene key="Home" component={Home} initial={true}  />
              <Scene key="OtherScreen" component={OtherScreen}  />
            </Scene>
          </Router>
        </Provider>
      );
    }
}

AppRegistry.registerComponent('YourAppNameAccordingToPackageJSON', () => App);
```
