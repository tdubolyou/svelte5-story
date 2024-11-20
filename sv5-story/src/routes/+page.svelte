<script>
	import Sidebar from '../components/Sidebar.svelte';
	import Map from '../components/Map.svelte';
	import SplashPage from '../components/SplashPage.svelte';
	//import { dev } from '$app/environment';

	let mapRef;
	let sidebarVisible = true;
	let showSplash = true;

	function handleMapRef(event) {
		mapRef = event.detail;
	}

	function toggleSidebar() {
   		sidebarVisible = !sidebarVisible;
  	}
	
  // Define a function to fly to the passed coordinates and zoom level
  function flyTo(coordinates = [-79.3832, 43.6532], zoomLevel = 14) {
      if (mapRef) {
          mapRef.flyTo({
              center: coordinates,
              zoom: zoomLevel,
              speed: 0.8,
              curve: 1
          });
      }
  };

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
		<Map bind:this={mapRef} on:mapReady={handleMapRef}/>
	  </div>
	{/if}
  </div>
  