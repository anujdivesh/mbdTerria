__Maritime Dashboard__<br /> <br /> 
Goal: to move geoJSON layers to geoServer and serve them as WMS.

Current Setup of one Layer:
```
  fetch('https://pacificdata.org/sites/default/files/mbd/boundaries-FJ.json')
            .then(res => res.json())
            .then((data) => {      
              if (data) {
                  console.log(data)
                borersGeoJson = data;
                window.setTimeout(function(){
                  iframe.postMessage({interactiveLayer: true, type: 'zone.add', items: borersGeoJson}, origin);

                  borersGeoJson.forEach(function(item){
                    iframe.postMessage({interactiveLayer: true, type: 'zone.show', id: item.id}, origin);
                  });

                  iframe.postMessage({interactiveLayer: true, type: 'layer.enable'}, origin);
                }, 15000);
              }
          });
```
The above assigns ID to each item in the Geojson, so javascript knows the id

Development:
``` 
var mydataset = [
                    {
                        "workbench": ["1"],
                        "catalog": [
                        {
                            "type": "group",
                            "name": "Crews Datasets",
                            "members": [
                                {
                                    "id": "1",
                                    "name": "Fiji",
                                    "layers": "gem_mb_dashboard:fj_eez",
                                    "url": "https://geoserver.pacificdata.org/ows?service=wms&version=1.3.0&request=GetCapabilities",               
                                    "type": "wms"
                                }
                            ]
                        }
                        ],
                        "homeCamera": {
                        "west": 176.72849572223166,
                        "south": 19.331499922789956,
                        "east": 178.87050330120584,
                        "north": -17.729500901184654
                        }
                    }
                    ];
          iframe.postMessage({ id:928982, initSources: mydataset}, origin);
 ```
  
 Problem: is there a way we can pass ID in the above postmessage, so we are able to do a callback from Javascript, this will help populate Drupal Popup. 
 As of know, Javascript does not know what layer has been passed to TerriaJS
          
