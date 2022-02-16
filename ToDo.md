__Maritime Dashboard__<br /> <br /> 
Goal: To move geoJSON layers to geoServer and serve them from Geoserver.

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
The above assigns ID to each item in the Geojson, so javascript knows the id of the layer which is loaded. Thus, we are able to get the id in the in the javasript popup and know which layer is loaded.

Current Development:
``` 
  fetch('https://geoserver.pacificdata.org/gem_mb_dashboard/wfs?service=wfs&version=2.0.0&request=GetFeature&typeNames=gem_mb_dashboard:fj_eez&outputFormat=application/json&srsName=epsg:4326')
          .then(res => res.json())
          .then((data) => {
            if (data) {
            zonesGeoJson = data;
            window.setTimeout(function(){
              iframe.postMessage({interactiveLayer: true, type: 'zone.add', items: zonesGeoJson}, origin);

              zonesGeoJson.forEach(function(item){
                iframe.postMessage({interactiveLayer: true, type: 'zone.show', id: item.id}, origin);
              });

              iframe.postMessage({interactiveLayer: true, type: 'layer.enable'}, origin);
            }, 20000);
          }
          });        
          
 ```
  
Problem: 
Cannot seem to load the layer from geoserver via WFS request.

          
