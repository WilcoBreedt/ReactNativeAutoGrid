# ReactNativeAutoGrid
Generate a grid with non conflicting colours as a home screen in react native


```
// In App.js in a new project

import React from "react";
import PropTypes from 'prop-types';
import { View, Text } from "react-native";
import { createStackNavigator, createAppContainer, SafeAreaView } from "react-navigation";
import { TouchableOpacity } from "react-native-gesture-handler";

const TileContainer = ({ tiles, itemsPerRow, navigation }) => {
  let colors = [];
  for (let i = 0; i < 360; i += 360/tiles.length) {
    colors.push({ color: `hsl(${i+90}, 100%, 50%)`, fontColor: `hsl(${i+180+90}, 100%, 50%)`});
  };
  colors = colors.map((color, index) => ({ ...color, ...tiles[index] }));
  const rows = Array(Math.ceil(tiles.length/itemsPerRow)).fill(0).map((_, index) => colors.slice(index*itemsPerRow, (index*itemsPerRow)+itemsPerRow));
  return (
    <View style={{ flex: 1, marginTop: 5, marginBottom: 5 }}>
      { rows.map(items => (
        <View style={{ flex: 1, flexDirection: 'row', marginLeft: 5, marginRight: 5 }}>
            { items.map(item => <Tile navigation={navigation} name={item.title} color={item.color} to={item.to} fontColor={item.fontColor} />) }
        </View>))
      }
    </View>
  );
};

TileContainer.propTypes = {
  title: PropTypes.arrayOf(Tile).isRequired,
  itemsPerRow: PropTypes.number,
};

TileContainer.defaultProps = {
  itemsPerRow: 2,
}

const Tile = ({ name, to, color, fontColor = 'white', navigation }) => (
  <View style={{ backgroundColor: color, flex: 1, margin: 5, borderRadius: 5 }}>
    <TouchableOpacity onPress={() => navigation.push(to)} style={{ padding: 10, width: '100%', height: '100%', alignItems: 'center', justifyContent: 'center' }}>
      <Text style={{ textTransform: 'uppercase', fontSize: 30, color: fontColor, fontWeight: '600', textAlign: 'center' }}>{name}</Text>
    </TouchableOpacity>
  </View>
);

const Home = ({ navigation }) => (
  <SafeAreaView style={{ flex: 1, backgroundColor: 'white' }}>
    <TileContainer navigation={navigation} itemsPerRow={2} tiles={[{ title: 'Screen 1', to: 'Screen1' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 2', to: 'Screen2' }, { title: 'Screen 3', to: 'Screen3' }, { title: 'Screen 4', to: 'Screen1' }, { title: 'Screen 5', to: 'Screen1' }]} />
  </SafeAreaView> 
);

const Screen1 = () => (<View style={{ flex: 1, backgroundColor: '#ababab', alignItems: 'center', justifyContent: 'center' }}><Text>Screen 1</Text></View>);
const Screen2 = () => (<View style={{ flex: 1, backgroundColor: '#ababab', alignItems: 'center', justifyContent: 'center' }}><Text>Screen 2</Text></View>);
const Screen3 = () => (<View style={{ flex: 1, backgroundColor: '#ababab', alignItems: 'center', justifyContent: 'center' }}><Text>Screen 3</Text></View>);

const AppNavigator = createStackNavigator({
  Home,
  Screen1,
  Screen2,
  Screen3,
}, {
  initialRouteName: 'Home',
});

export default createAppContainer(AppNavigator);

```
