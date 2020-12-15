## Creating a Custom Plugin based on an @corpsmap application-control
@corpsmap has a large set of application-controls that can be leveraged by application developers. This section attempts to illustrate a simple example.

First the developer must create a Create-React-App and import the @corpsmap module from npm. 
Next the developer must create a page for their application which contains an @corpsmap map

First a bundle needs to be developed that provides any actions for the activation and deactivation of the button

```

export default {
  name: "nsiDownload",
  getReducer: () => {
    const initialData = {
      fips: '15005',
    };
    return (state = initialData, { type, payload }) => {
      switch (type) {
        case "NSI_DOWNLOAD_STARTED":
          return Object.assign({}, state, payload);
        default:
          return state;
      }
    };
  },
  doNsiDownload:() =>({ dispatch, store }) => {
    var p = store.selectDrawPolygonsLayer() //from corpsmap's store
    dispatch({
      type: "NSI_DOWNLOAD_STARTED",
      payload: {
        fips: '15002',
      },
    });
    dispatch({
      type: "BASIC_TOOL_DEACTIVATE", //a corpsmap event
      payload: {
        activeTool: null, //a corpsmap value in the store
      },
    });
  },
  selectFips: (state) =>{
    return state.nsiDownload.fips
  }
};

```

```
import React from 'react';
import { connect } from 'redux-bundler-react';
import { BasicToolbarButton } from "@corpsmap/corpsmap";

const NSIDownloadButton = ({doNsiDownload}) =>{
  return (
    <BasicToolbarButton
      enabled={true}
      iconClass="mdi mdi-cloud-download"
      title="Download NSI Structures"
      onDeactivate={() => {}}
      onActivate={doNsiDownload}
    />
  );
};
export default connect(
    'doNsiDownload',
    NSIDownloadButton
);
```

then finally the index that binds them together and exprots the plugin

```
import nsiDlbundle from './cm3-nsi-download-bundle'
import dlButton from './cm3-nsi-download'
const cm3NsiDownloadPlugin={
    pluginName: 'NSIDownloadPlugin',
    enabled: true,
    bundle: nsiDlbundle,//this gives corpsmap access to the store created by our bundle
    components:[{
      component: dlButton,
      region: 'basic-toolbar',
      sortOrder: 201
    }]
  }

export {cm3NsiDownloadPlugin as default}
```


