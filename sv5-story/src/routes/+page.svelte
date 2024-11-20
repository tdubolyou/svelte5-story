<script>
	import Sidebar from '../components/Sidebar.svelte';
	import Map from '../components/Map.svelte';
	import SplashPage from '../components/SplashPage.svelte';
  
	let mapRef;
	let sidebarVisible = true;
	let showSplash = true;
  
	// Define a function to fly to the passed coordinates and zoom level
	function flyTo(coordinates = [-79.3832, 43.6532], zoomLevel = 14) {
	  if (mapRef && typeof mapRef.flyTo === 'function') {
		// Ensure that mapRef is an actual Maplibre instance
		mapRef.flyTo({
		  center: coordinates,
		  zoom: zoomLevel,
		  speed: 0.8,
		  curve: 1,
		});
	  } else {
		console.error('Map reference is not set properly or does not have the flyTo method');
	  }
	}
  
	function handleMapRef(event) {
	  // Set the mapRef to the actual map instance from the 'mapReady' event
	  mapRef = event.detail; // Assign the map instance received from Map.svelte
	}
  
	function toggleSidebar() {
	  sidebarVisible = !sidebarVisible;
	}
  
	// Function to close the splash page
	function closeSplash() {
	  showSplash = false;
	}
  </script>
  
  <style>
	.app {
	  display: flex;
	  position: relative;
	}
  
	.map-container {
	  flex: 1;
	  height: 100vh;
	  overflow: hidden;
	}
  </style>
  
  <div class="app">
	<!-- Splash Page (Visible on Load) -->
	{#if showSplash}
	  <SplashPage onClose={closeSplash} />
	{/if}
  
	<!-- Main Content (Visible when Splash is hidden) -->
	{#if !showSplash}
	  <Sidebar {flyTo} />
	  <div class="map-container">
		<Map on:mapReady={handleMapRef} />
	  </div>
	{/if}
  </div>
  