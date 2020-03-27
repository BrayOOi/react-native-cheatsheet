# React-Native Cheatsheet
A note on gotchas of React-Native coming from Web React

CONTENTS
----------
1. Components
  - Element Counterparts
  - Self-enclosing tags
2. Styles
3. Routing
  - Creating Routes
  - Navigations


COMPONENTS
----------

Element Counterparts
--------------------

React Native uses primitive elements instead of HTML elements to render components

- div === View
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
  - onPress=(() => {})


Self-enclosing tags
--------------------
In addition to some of their notable attributes:

- Button
  - title="I am a button"
  - onPress={() => {}}

- FlatList
  - keyExtractor={data => data.id} // any unique strings 
  - data={array} // array to be iterated over
  - renderItem={({ item }) => ...} // renderItem receives a prop object and each element are referenced by item, hence you will need to destructure item from the props object

- TextInput
  - autoCapitalize="characters/none/sentences/words"
  - autoCorrect={true/false}

- Image
  - source={require(STATIC_FOLDER)}




STYLES
------
- RN styles do not append any units behind eg. px and only Integers 

RN validates styling of components through another primitive component StyleSheet shown below.

const styles = StyleSheet.create({
  textStyle: {
    fontSize: 20
  }
});

The styles validated can then be referenced to components as such.

const TextDisplay = () => (
  <Text style={styles.textStyle}>I am text</Text>
);

ROUTING - react-navigation
--------------------------

Creating routes
---------------

- Routing with react-navigation is defined by a function createStackNavigator, which is then passed in 2 arguments 

- The first object contains the paths and their respective component
- The second object contains additional configurations

const navigator = createStackNavigator({
  Home: HomeScreen, // a path "Home" rendering a component "HomeScreen"
  About: AboutScreen, 
  ...
}, {
  initialRouteName: "Home",
  defaultNavigationOptions: {
    title: "Hello World!!!" // This will be shown at the header
  }
});

export default createAppContainer(navigator);


Navigations
-----------
Just like how React-Router passes its own props to all its immediate children components, React-Navigation also passes additional props to its children

The prop used for navigation is "navigation" and can be destructured from props as below

const HomeScreen = ({ navigation: { navigate } }) => (
  <View>
    <Button onPress={() => navigate("About")} /> // The string inside navigate corresponds to the keys passed as the first argument to createStackNavigator
  </View>
);