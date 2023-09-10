<script>
   import { wc } from './geojson/wc.js';
   import { airbnb } from './geojson/airbnb.js';
    import { onMount, onDestroy } from 'svelte';
    import { browser } from '$app/environment';
    
    let mapContainer;
    let map;
    
    onMount(async () => {
       if(browser) {
          const leaflet = await import('leaflet');
          map = leaflet.map(mapContainer).setView([-33.930703940326545, 18.579752112319], 10);
    
    // Add the tile layer      
    leaflet.tileLayer('https://tiles.stadiamaps.com/tiles/alidade_smooth/{z}/{x}/{y}{r}.{ext}', {
	minZoom: 0,
	maxZoom: 20,
	attribution: '&copy; <a href="https://www.stadiamaps.com/" target="_blank">Stadia Maps</a> &copy; <a href="https://openmaptiles.org/" target="_blank">OpenMapTiles</a> &copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a> contributors',
	ext: 'png'
}).addTo(map);
    
    // Add a marker to the map
    leaflet.marker([-26.1925013,28.0100383]).addTo(map)

    // Add WC geojson
      leaflet.geoJSON(wc)

      // Add points
      var geojsonMarkerOptions = {
         radius: 1,
         fillColor: "#ff7800",
         color: "none",
         weight: 1,
         opacity: 1,
         fillOpacity: 0.8
      };

      leaflet.geoJSON(airbnb, {
    pointToLayer: function (feature, latlng) {
        return L.circleMarker(latlng, geojsonMarkerOptions);
    }
})
                 
       }
    });
    
    onDestroy(async () => {
       if(map) {
          map.remove();
       }
    });
    </script>
    
    
    <main>
       <div bind:this={mapContainer}></div>
    </main>
    
    <style>
       @import 'leaflet/dist/leaflet.css';
       div {
       height: 90vh;
       }
    </style>