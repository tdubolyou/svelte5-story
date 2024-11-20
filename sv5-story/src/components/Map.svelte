<script>
    import { onMount, onDestroy, createEventDispatcher } from 'svelte';
    import maplibre from 'maplibre-gl';
    import cafeData from '../data/cafeTO.json';

    let map;
    const dispatch = createEventDispatcher();

    // Define a function to fly to the passed coordinates and zoom level
    const flyTo = (coordinates = [-79.3832, 43.6532], zoomLevel = 14) => {
        if (map) {
            map.flyTo({
                center: coordinates,
                zoom: zoomLevel,
                speed: 0.8,
                curve: 1
            });
        }
    };

    onMount(() => {
        map = new maplibre.Map({
            container: 'map',
            style: 'https://basemaps.cartocdn.com/gl/positron-gl-style/style.json',
            center: [-79.3832, 43.6532], // Toronto, for example
            zoom: 12
        });

        map.on('load', () => {
            try {
                // Add the cafeTO.geojson file to the map once it is fully loaded
                map.addSource('cafes', {
                    type: 'geojson',
                    data: cafeData
                });

                map.addLayer({
                    'id': 'cafes',
                    'type': 'circle',
                    'source': 'cafes',
                    'paint': {
                        'circle-radius': 6,
                        'circle-color': '#B42222'
                    }
                });

                // Emit the map instance to the parent for further usage
                dispatch('mapReady', map);
            } catch (error) {
                console.error('Error adding GeoJSON source to the map:', error);
            }
        });

        // Cleanup function to properly remove the map
        onDestroy(() => {
            if (map) {
                map.remove();
            }
        });
    });
</script>

<style>
    #map {
        height: 100vh;
        width: 100%;
    }
</style>

<div id="map"></div>
