# React-Native Cheatsheet
A note on gotchas of React-Native coming from Web React

CONTENTS
----------
1. Components
  - Element Counterparts
  - Special tags
  - Self-enclosing tags
2. Styles
  - Box Model
  - Flex-box
  - Positions
3. Routing
  - Creating Routes
  - Navigations


COMPONENTS
----------

Element Counterparts
--------------------

React Native uses primitive elements instead of HTML elements to render components

- div === View (see ScrollView and div frags)
- span === Text
- img === Image
- input === TextInput
- button === Button/TouchableOpacity
- ul/ol === FlatList

All primitive components can be imported from react-native

Special tags
------------
1. TouchableOpacity
  - A wrapper that can catch any onPress events
  - All of its children are open to onPress events and will be affected by the changing opacity
  - event handler
    - onPress=(() => {})

2. Using icons
  - Expo CLI provides a library of icons to use
  - Just import the respective component and insert the name as argument as such.

  import { FontAwesome } from "@expo/vector-icons";

  \<FontAwesome name="search" size={50}> // size attribute shorthand

3. ScrollView
  - just like a View component, but will automatically enable scrolling if the children components do not fit the screen

Self-enclosing tags
--------------------
In addition to some of their notable attributes:

- Button
  - title="I am a button"
  - onPress={() => {}}

- FlatList
  - Horizontal // render data horizontally
  - showsHorizontalScrollIndicator={false} // hide scroll bar
  - keyExtractor={data => data.id} // any unique strings 
  - data={array} // array to be iterated over
  - renderItem={({ item }) => ...} // renderItem receives a prop object and each element are referenced by item, hence you will need to destructure item from the props object

- TextInput
  - autoCapitalize="characters/none/sentences/words"
  - autoCorrect={true/false}
  - onChangeText={textHandler or (curVal) => handles text.....} // the value will be passed to the function, not event object
  - onEndEditing(() => {})

  - recommended to add border because you will not see it on screen

- Image
  - source={require(STATIC_FOLDER) or { uri: IMG_URL }}
  - Image components need an explicit height and widths

<br>


STYLES
------
- RN styles do not append any units behind eg. px and only Integers 

RN validates styling of components through another primitive component StyleSheet shown below.

const styles = StyleSheet.create({
  textStyle: {
    fontSize: 20 // default font-size is 14
  }
});

The styles validated can then be referenced to components as such.

Box Model
---------
- padding === padding, 
&nbsp;&nbsp;paddingVertical, paddingHorizontal,
&nbsp;&nbsp;paddingTop, paddingBottom, paddingLeft, paddingRight
- margin === margin,
&nbsp;&nbsp;marginVertical, marginHorizontal,
&nbsp;&nbsp;marginTop, marginBottom, marginLeft, marginRight
- border-width === borderWidth,
&nbsp;&nbsp;borderTopWidth, borderBottomWidth, borderLeftWidth, borderRightWidth

const TextDisplay = () => (<br>
&nbsp;&nbsp;\<Text style={styles.textStyle}>I am text\</Text><br>
);

Flex-box
--------
- Unlike CSS, Flexbox for RN has a default value of column for flex-direction
- Unlike CSS, you don't need to declare display: flex at parent

- flex-grow === flex

- a component directly under App component with a flex: 1 will fill the entire visible screen (minus the navbar at the bottom of Android)
- However, the same can be achieved by using a div frag because flex: 1 will create unpredictable situations

<br>

Positions
---------
position: "absolute",<br>
top: 0,<br>
right: 0,<br>
left: 0,<br>
bottom: 0<br>

or

...StyleSheet.absoluteFillObject

will cause the element to fill up the parent component

ROUTING - react-navigation
--------------------------

Creating routes
---------------

- Routing with react-navigation is defined by navigators, which is then passed in 2 arguments 

- The first object contains the routes and their respective component
- The second object contains additional configurations

const navigator = createStackNavigator({ <br>
&nbsp;&nbsp;Home: HomeScreen, // a path "Home" rendering a component "HomeScreen" <br>
&nbsp;&nbsp;About: AboutScreen, <br>
&nbsp;&nbsp;... <br>
}, { <br>
&nbsp;&nbsp;initialRouteName: "Home", <br>
&nbsp;&nbsp;defaultNavigationOptions: { <br>
&nbsp;&nbsp;&nbsp;&nbsp;title: "Hello World!!!" // This will be shown at the header <br>
&nbsp;&nbsp;}<br>
}); <br>

export default createAppContainer(navigator);

<br>

Navigations
-----------
Just like how React-Router passes its own props to all its immediate children components, React-Navigation also passes additional props to its children

The prop used for navigation is "navigation" and can be destructured from props as below

const HomeScreen = ({ navigation: { navigate } }) => (<br>
&nbsp;&nbsp;\<View><br>
&nbsp;&nbsp;&nbsp;&nbsp;\<Button onPress={() => navigate("About")} /> // The string inside navigate corresponds to the keys passed as the first argument to createStackNavigator <br>
&nbsp;&nbsp;\</View> <br>
);

Unlike react-router and RectDOM, when RN and React-Navigation does not unmount a component when users visit away from the component. Therefore an useEffect hook as below will not work when users visit back the page

useEffect(() => {
&nbsp;&nbsp;// Do something when component first mounts
}, []);

In order to simulate the effect, we need to add a listener

useEffect(() => {
&nbsp;&nbsp;// Do something when component first mounts

&nbsp;&nbsp;const listener = navigation.addListener("didFocus", () => {
&nbsp;&nbsp;&nbsp;&nbsp;// Do something again when the user visit back the screen
&nbsp;&nbsp;});

&nbsp;&nbsp;return () => {
&nbsp;&nbsp;&nbsp;&nbsp;listener.remove();
&nbsp;&nbsp;};
}, []);

However, the listener will not be removed after unmounting the component and therefore the remove function is returned for cleanup

You can also import { NavigationEvents } from "react-navigation"

\<NavigationEvents
&nbsp;&nbsp;onWillFocus={// Do something when RN begins navigate into the component}
&nbsp;&nbsp;onWillBlur={// Do something when component begins navigate away}
/>