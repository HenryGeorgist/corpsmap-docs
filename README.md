# corpsmap-docs
A place to store Documents to support leveraging corpsmap

## About @corpsmap
@corpsmap is a project developed by Will B. to facilitate an enterprise ecosystem of geospatial applications that are loosely coupled but maintain a common set of frequently used tools. 
### GIS capabilities
The foundational gis capabilities are provided through openlayers for visualization and display and turf for the more some of the spatial analysis.

### development stack
@corpsmap utilizes the react-redux stack for state management and dispatches - it leverages react-redux-bundler to reduce the boiler-plate with rapidly developing components for the framework.

### Plugin Architecture
In order to best integrate with @corpsmap, the developer will likely need access to the internal states and dispatches of @corpsmap. To facilitate this @corpsmap provides the developer the capability of adding a plugin to the framework.

The most basic components of a plugin are the index.js file;
```
import bundle from './<your plugin directory>';

const plugin = {
    pluginName: '<your plugin name>',
    enabled: true,
    bundle: bundle,
    components:[]
  }

export default plugin
```
and the bundle.
```
export default {
    name:'<nameofplugininstore>',
    getReducer() {
      const initialData = {
        _shouldInitialize: false,
      };
      return (state = initialData, { type, payload }) => {
        switch(type){
          case <PLUGIN_INITIALIZE_START_DISPATCH_NAME>:
          case <PLUGIN_INITIALIZE_END_DISPATCH_NAME>:
            return Object.assign({}, state, payload);
          case MAP_INITIALIZED:
            return Object.assign({}, state, {
              _shouldInitialize: true
            })
          default:
            return state;
        }
      }
    },
    do<PLUGINSTORENAME>Initialize: () => ({ dispatch, store, anonGet }) => {
      dispatch({
        type: <PLUGIN_INITIALIZE_START_DISPATCH_NAME>,
        payload: {
          _shouldInitialize: false,
        }
      })
      //do cool stuff here!     
    },
    react<PLUGINSTORENAME>ShouldInitialize: (state) => {
      if(state.<nameofplugininstore>._shouldInitialize) return { actionCreator: "do<PLUGINSTORENAME>Initialize" };
    }
}
}
```

Finally, the plugin needs to get added into a map element of @corpsmap

```
import React from 'react';
import { connect } from 'redux-bundler-react';
import {
  Map,
  addData,
  basemapSwitcher,
  bookmarks,
  coordDisplay,
  draw,
  treeView,
  geocoder,
  identify,
  measureTools,
  rotateNorth,
  zoomInOut,
  zoomHome,
  zoomToBox
} from "@corpsmap/corpsmap";
import "@corpsmap/corpsmap/css/corpsmap.css";
import <plugin> from '../../cm-plugins/<your plugin directory>/index'
class MapPage extends React.Component {
  render(){
    return (
        <div className="container-fluid" style={{ padding: "0" }}>
            <Map
              theme="grey"
              plugins={[
                addData,
                basemapSwitcher,
                bookmarks,
                coordDisplay,
                draw,
                treeView(),
                geocoder,
                identify,
                measureTools,
                rotateNorth,
                zoomInOut,
                zoomHome,
                zoomToBox,
                <plugin>
              ]}
            />        
        </div>
    )
  }
}
export default connect(
  MapPage
  );
  ```


## Getting Started

If you are wanting to build a basic map with corpsmap, the first step would be to start by creating a new react app.

Open visual studio code in the directory where you want the new react application, and run the following three commands in order

```
$ npx create-react-app <your-app-name-HERE>
$ cd <your-app-name-here>
$ npm install @corpsmap/corpsmap redux-bundler redux-bundler-react
```

You will now be able to get started building your web page.
