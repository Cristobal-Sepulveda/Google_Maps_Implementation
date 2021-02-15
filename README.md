________________________________________________________________________________

                            1. Looking Forward
________________________________________________________________________________

Capstone Component

  The first word in mobile development is “mobile.” One of the most immersive
  features within mobile applications is location. Location awareness allows
  your application to provide functionality that can react to context and
  physical space in a way that other formats cannot. While not required, the
  location feature could satisfy the hardware integration for your capstone
  project. Think about what features in your application either require
  location or could be enhanced by providing location integration.

In This Lesson

  Location is more than just a place. It can be distance, it can be an
  area, a destination, or it can be movement. As such, apps use location
  in many different ways. Locations can be used to fill out a form, tag
  a photo, track your workouts, draw a map, restrict usage to certain
  areas, or relate to others. How do the apps you use utilize location
  to enhance their feature list? Start by looking through your system
  permissions and which apps request Location permissions.

    * How do these apps use location?

    * Does that enhance the experience for the user?

    * Is it required for the feature?

    * What type of location level do they use?

    * When are you asked for permissions? (disable permissions and restart the
      app if you don’t   remember)

    * What happens if you don’t grant them?
________________________________________________________________________________

                            2. Introduction
________________________________________________________________________________

Reference Documentation

    * Google Maps API

    * https://developers.google.com/maps/documentation

What You'll Learn

    * Obtain an API key.

    * Integrate a Google Map in your app.

    * Display different map types.

    * Style the Google Map.

    * Add markers to your map.

    * Enable the user to place a marker on a point of interest (POI).

    * Enable location tracking.

What You'll Do

    * Get an API key from the Google API Console and register the key
      to your app.

    * Create the Wander app, which has an embedded Google Map.

    * Add custom features to your app such as markers and styling.

    * Enable location tracking in your app.
________________________________________________________________________________

                            3. Create a Google Maps Project
________________________________________________________________________________

Reference Documentation

  OnMapReadyCallback
  https://developers.google.com/android/reference/com/google/android/gms/maps/OnMapReadyCallback
  getMapAsync
  https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#getMapAsync%28com.google.android.gms.maps.OnMapReadyCallback%29
  SupportMapFragment
  https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment

Create the Wander project with the Maps template

  1. Create a new Android Studio project.
  2. Select the Google Maps Activity template.

https://video.udacity-data.com/topher/2019/October/5da7721a_image15/image15.png

  3. Name the project Wander.
  4. Make sure the language is Kotlin.
  5. Set the minimum API level to API 19.
  6. Click finish.

  Android Studio creates several maps-related additional files:

MapsActivity.java

  The MapsActivity.kt file instantiates the SupportMapFragment in the
  onCreate() method and uses the class's getMapAsync() to automatically
  initialize the maps system and the view. The activity that contains
  the SupportMapFragment must implement the OnMapReadyCallback interface
  and that interface's onMapReady() method. The onMapReady() method
  is called when the map is loaded.

activity_maps.xml

  This layout file contains a single fragment that fills the entire screen.
  The SupportMapFragment class is a subclass of the Fragment class.
  A Support Map Fragment is the simplest way to place a map in
  an application. It’s a wrapper around a view of a map to
  automatically handle the necessary life cycle needs.
  You can include SupportMapFragment in a layout file using a <fragment>
  tag in any ViewGroup, with an additional attribute:

    android:name="com.google.android.gms.maps.SupportMapFragment"

google_maps_api.xml

  You use this configuration file to hold your API key. The template
  generates two google_maps_api.xml files: one for debug and one for release.
  The file for the API key for the debug certificate is located in
  src/debug/res/values. The file for the API key for the release certificate
  is located in src/release/res/values. In this practical you only use the
  debug certificate.
________________________________________________________________________________

                            4. Getting an API Key
________________________________________________________________________________

Reference Documentation

    * Get an API Key

    * Sign your app

Get an API Key

    1. Open the debug version of the google_maps_api.xml file. The file
       includes a comment with a long URL. The URL's parameters include specific
       information about your app.

    2. Copy and paste the URL into a browser.

    3. Follow the prompts to create a project in the Google API Console.
       Because of the parameters in the provided URL, the API Console knows
       to automatically enable the Google Maps Android API.

    4. Create an API key and click Restrict Key to restrict the key's use
       to Android apps. The generated API key should start with AIza.

`//TODO 1.1`
    5. In the google_maps_api.xml file, paste the key into the google_maps_key
       string where it says YOUR_KEY_HERE.

    6. Run your app. You have an embedded map in your activity, with a marker
       set in Sydney, Australia. (The Sydney marker is part of the template,
       and you change it later.)

Rename mMap

  MapsActivity has a private lateinit var called mMap which is of type
  GoogleMap. By Kotlin naming conventions you will want to change it
  to be called map instead.

`//TODO 1.2`
    1. In MapsActivity, right click on mMap and click Refactor/Rename...

        https://video.udacity-data.com/topher/2019/October/5da77c36_a03-1/a03-1.png
        private lateinit var mMap: GoogleMap

    2. Change the variable name to map. Notice how all the references to
       mMap in the onMapReady() function also change to map.

       private lateinit var map: GoogleMap


________________________________________________________________________________

                            5. Add map types
________________________________________________________________________________

Add map types
    * onCreateOptionsMenu()
    https://developer.android.com/reference/android/app/Activity.html#onCreateOptionsMenu%28android.view.Menu%29
    * onOptionsItemSelected()
    https://developer.android.com/reference/android/app/Activity.html#onOptionsItemSelected%28android.view.MenuItem%29

  Google Maps include several map types: normal, hybrid, satellite,
  terrain, and "none." In this task you add an app bar with an options
  menu that allows the user to change the map type. You move the map's
  starting location to your own home location. Then you add support
  for markers, which indicate single locations on a map and can
  include a label.

  https://video.udacity-data.com/topher/2019/October/5da8e4c7_a04-1-v2/a04-1-v2.png

Add menu for map types

  The type of map that your user wants depends on the kind of
  information they need. When using maps for navigation in your car,
  it's helpful to see street names clearly - in this case you would
  use the normal option. When you are hiking, you probably care more
  about how much you have to climb to get to the top of the
  mountain - in this case you would use the terrain option. In the Add
  app bar step, you add an app bar with an options menu that allows
  the user to change the map type.

`//TODO 1.3`
    1. To create a new menu XML file, right-click on your res directory
       and select New > Android Resource File.

    2. In the dialog, name the file map_options. Choose Menu for the resource
       type. Click OK.

    https://video.udacity-data.com/topher/2019/October/5da77da6_a04-2/a04-2.png
`//TODO 1.4`
    3. In the text tab, replace the code in the new file with the following
       code to create the map options. The "none" map type is omitted, because
       "none" results in the lack of any map at all.

        <?xml version="1.0" encoding="utf-8"?>
        <menu xmlns:android="http://schemas.android.com/apk/res/android"
           xmlns:app="http://schemas.android.com/apk/res-auto">
           <item
               android:id="@+id/normal_map"
               android:title="@string/normal_map"
               app:showAsAction="never" />
           <item
               android:id="@+id/hybrid_map"
               android:title="@string/hybrid_map"
               app:showAsAction="never" />
           <item
               android:id="@+id/satellite_map"
               android:title="@string/satellite_map"
               app:showAsAction="never" />
           <item
               android:id="@+id/terrain_map"
               android:title="@string/terrain_map"
               app:showAsAction="never" />
        </menu>
`//TODO 1.5`
    4. In strings.xml, add resources for the title attributes in order to resolve the errors.

        <resources>
           …
           <string name="normal_map">Normal Map</string>
           <string name="hybrid_map">Hybrid Map</string>
           <string name="satellite_map">Satellite Map</string>
           <string name="terrain_map">Terrain Map</string>
           <string name="lat_long_snippet">Lat: %1$.5f, Long: %2$.5f</string>
           <string name="dropped_pin">Dropped Pin</string>
           <string name="poi">poi</string>
        </resources>

`//TODO 1.6`
    5. In MapsActivity, override the onCreateOptionsMenu() method and inflate
       the menu from the map_options resource file:

        override fun onCreateOptionsMenu(menu: Menu?): Boolean {
           val inflater = menuInflater
           inflater.inflate(R.menu.map_options, menu)
           return true
        }

`//TODO 1.7`
    6. To change the map type, use the setMapType() method on the GoogleMap
       object, passing in one of the map-type constants.
       In MapsActivity.kt, override the onOptionsItemSelected() method.
       Use the following code to change the map type when the user
       selects one of the menu options:

        override fun onOptionsItemSelected(item: MenuItem) = when (item.itemId) {
           // Change the map type based on the user's selection.
           R.id.normal_map -> {
               map.mapType = GoogleMap.MAP_TYPE_NORMAL
               true
           }
           R.id.hybrid_map -> {
               map.mapType = GoogleMap.MAP_TYPE_HYBRID
               true
           }
           R.id.satellite_map -> {
               map.mapType = GoogleMap.MAP_TYPE_SATELLITE
               true
           }
           R.id.terrain_map -> {
               map.mapType = GoogleMap.MAP_TYPE_TERRAIN
               true
           }
           else -> super.onOptionsItemSelected(item)
        }

    7. Run the app. Use the menu in the app bar to change the map type.
       Notice how the map's appearance changes between the different modes.

https://video.udacity-data.com/topher/2019/October/5da77f3a_a04-3/a04-3.png

________________________________________________________________________________

                            6. Adding Markers
________________________________________________________________________________

Reference Documentation
    * Markers
    https://developers.google.com/maps/documentation/android-sdk/marker

    * CameraUpdate
    https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate

    * LatLng
    https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng

  By default, the onMapReady() callback includes code that places a marker
  in Sydney, Australia, where Google Maps was created. The default callback
  also animates the map to pan to Sydney. In this step, you make the map’s
  camera move to your home, zoom to a level you specify and place a marker
  there.

`//TODO 1.8`
    1. In the onMapReady() method, remove the code that places the marker
       in Sydney and moves the camera. This is what your method should
       look like now.

        override fun onMapReady(googleMap: GoogleMap) {
           map = googleMap
        }

    2. Find the latitude and longitude of your home by following these instructions.
    https://support.google.com/maps/answer/18539?co=GENIE.Platform%3DDesktop&hl=en

    3. Create a value for the latitude and a value for the longitude and
       input their float values.

        val latitude = 37.422160
        val longitude = -122.084270

    4. Create a new LatLng object called homeLatLng. In the LatLng object, use
       the values you just created.

        val homeLatLng = LatLng(latitude, longitude)

    5. Move the camera to home by calling the moveCamera() function on the
       GoogleMap object and pass in a CameraUpdate object using CameraUpdateFactory.newLatLngZoom(). Create a val for how zoomed in
       you want to be on the map.

  The zoom level controls how zoomed in you are on the map. The following
  list gives you an idea of what level of detail each level of zoom shows:

    * 1: World
    * 5: Landmass/continent
    * 10: City
    * 15: Streets
    * 20: Buildings

  Pass in the home object and a float for how zoomed in you want it to be.

        val zoomLevel = 15f
        map.moveCamera(CameraUpdateFactory.newLatLngZoom(homeLatLng, zoomLevel))

    6. Add a marker to the map at your home.

        map.addMarker(MarkerOptions().position(homeLatLng))

  The method should look like this.

        override fun onMapReady(googleMap: GoogleMap) {
           map = googleMap

           //These coordinates represent the latitude and longitude of the Googleplex.
           val latitude = 37.422160
           val longitude = -122.084270
           val zoomLevel = 15f

           val homeLatLng = LatLng(latitude, longitude)
           map.moveCamera(CameraUpdateFactory.newLatLngZoom(homeLatLng, zoomLevel))
           map.addMarker(MarkerOptions().position(homeLatLng))
        }

    7. Run the app. The map should pan to your home, zoom into the desired
       level and place a marker on your home.

________________________________________________________________________________

                            7. Adding markers on long click
________________________________________________________________________________

Reference Documentation
  * Add markers on long click
  https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.OnMapLongClickListener
  * InfoWindow
  https://developers.google.com/maps/documentation/android-sdk/infowindows
  * MarkerOptions
  https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions

Add markers on long click

  In this step, you add a marker when the user touches and holds a location
  on the map. You will then add an InfoWindow that displays the coordinates
  of the marker when the marker is tapped.
`//TODO 1.9`
    1. Create a method stub in MapsActivity called setMapLongClick() that
       takes a GoogleMap as an argument. Attach a long click listener
       to the map object.

        private fun setMapLongClick(map:GoogleMap) {
           map.setOnMapLongClickListener { }
        }

    2. Inside onMapLongClick(), call the addMarker() method. Pass in a new
       MarkerOptions object with the position set to the passed-in LatLng:

        private fun setMapLongClick(map: GoogleMap) {
           map.setOnMapLongClickListener { latLng ->
               map.addMarker(
                   MarkerOptions()
                       .position(latLng)
               )
           }
        }
`//TODO 1.10`
    3. At the end of the onMapReady() method, call etMapLongClick(). Pass in map.

        override fun onMapReady(googleMap: GoogleMap) {
           …

           setMapLongClick(map)
        }

    4. Run the app. Touch and hold on the map to place a marker at a location.
       Tap the marker, which centers it on the screen. When a marker is tapped,
       navigation buttons appear at the bottom-left side of the screen,
       allowing the user to use the Google Maps app to navigate to the
       marked position.
https://video.udacity-data.com/topher/2019/October/5da8e8f9_a06-1-v2/a06-1-v2.png

Add an info window for the marker:

    1. In setMapLongClick()’s setOnMapLongClickListener() lambda, create a
       snippet. A snippet is additional text that is displayed below the
       title. In your case the snippet displays the latitude and longitude
       of a marker.

`//TODO 1.11`
        private fun setMapLongClick(map: GoogleMap) {
           map.setOnMapLongClickListener { latLng ->
               // A Snippet is Additional text that's displayed below the title.
               val snippet = String.format(
                   Locale.getDefault(),
                   "Lat: %1$.5f, Long: %2$.5f",
                   latLng.latitude,
                   latLng.longitude
               )
               map.addMarker(
                   MarkerOptions()
                       .position(latLng)
               )
           }
        }

`//TODO 1.12`
    2. Set the title of the marker to “Dropped Pin” and set the marker’s
       snippet to the snippet you just created.

        private fun setMapLongClick(map: GoogleMap) {
           map.setOnMapLongClickListener { latLng ->
               // A Snippet is Additional text that's displayed below the title.
               val snippet = String.format(
                   Locale.getDefault(),
                   "Lat: %1$.5f, Long: %2$.5f",
                   latLng.latitude,
                   latLng.longitude
               )
               map.addMarker(
                   MarkerOptions()
                       .position(latLng)
                       .title(getString(R.string.dropped_pin))
                       .snippet(snippet)

               )
           }
        }

    3. Run the app. Touch and hold on the map to drop a location marker.
       Tap the marker to show the info window.

https://video.udacity-data.com/topher/2019/October/5da8e9ac_a06-2-v2/a06-2-v2.png
________________________________________________________________________________

                            8. Adding markers on POI click
________________________________________________________________________________

Reference Documentation

    * OnPoiClickListener
https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.OnPoiClickListener

Add POI listener

  By default, points of interest (POIs) appear on the map along with their
  corresponding icons. POIs include parks, schools, government buildings,
  and more. When the map type is set to normal, business POIs also appear
  on the map. Business POIs represent businesses such as shops,
  restaurants, and hotels.

  In this step, you add an OnPoiClickListener to the map.
  This click-listener places a marker on the map immediately when
  the user clicks on a POI. The click-listener also displays the
  info window that contains the POI name.

`//TODO 1.13`
    1. Create a method stub in MapsActivity called setPoiClick() that takes
       a GoogleMap as an argument. In the setPoiClick() method, set an
       OnPoiClickListener on the passed-in GoogleMap:

        private fun setPoiClick(map: GoogleMap) {
           map.setOnPoiClickListener { poi ->

           }
        }
`//TODO 1.14`
    2. In the onPoiClick() method, place a marker at the POI location.
       Set the title to the name of the POI. Save the result to a variable
       called poiMarker.

        private fun setPoiClick(map: GoogleMap) {
           map.setOnPoiClickListener { poi ->
`//TODO 1.14`
               val poiMarker = map.addMarker(
                   MarkerOptions()
                       .position(poi.latLng)
                       .title(poi.name)
               )
           }
        }
`//TODO 1.15`
    3. In setOnPoiClickListener function, call showInfoWindow() on poiMarker
       to immediately show the info window.

          poiMarker.showInfoWindow()

  The function should look like this:

          private fun setPoiClick(map: GoogleMap) {
             map.setOnPoiClickListener { poi ->
                 val poiMarker = map.addMarker(
                     MarkerOptions()
                         .position(poi.latLng)
                         .title(poi.name)
                 )
                 poiMarker.showInfoWindow()
             }
          }
`//TODO 1.16`
    4. Call setPoiClick() at the end of onMapReady(). Pass in map.

          override fun onMapReady(googleMap: GoogleMap) {
             …

             setPoiClick(map)
          }

    5. Run your app and find a POI such as a park or a coffee shop. Tap on
       the POI to place a marker on it and display the POI's name in
       an info window.

https://video.udacity-data.com/topher/2019/October/5da8ea6c_a06-3-v2/a06-3-v2.png
________________________________________________________________________________

                            9. Style your map
________________________________________________________________________________

Reference Documentation

Styling a Google Map
https://developers.google.com/maps/documentation/android-sdk/styling
Styling Wizard
https://developers.google.com/maps/documentation/javascript/styling#styled_map_wizard

  You can customize Google Maps in many ways, giving your map a unique
  look and feel.

  You can customize a MapFragment object using the available XML attributes,
  as you would customize any other fragment. However, in this step you
  customize the look and feel of the content of the MapFragment, using
  methods on the GoogleMap object.

Create a style for your map

  To create a customized style for your map, you generate a JSON file that
  specifies how features in the map are displayed. You don't have to create
  this JSON file manually: Google provides the Styling Wizard, which generates
  the JSON for you after you visually style your map. In this lesson, you
  style the map for "retro," meaning that the map uses vintage colors and you
  will be adding colored roads.

  Note: Styling only applies to maps that use the normal map type.

    1. Navigate to https://mapstyle.withgoogle.com/ in your browser.

    2. Select Create a Style.

    3. Select the Retro theme.

https://video.udacity-data.com/topher/2019/October/5da8ec15_a08-1-v2/a08-1-v2.png

    4. Click More Options at the bottom of the menu.

    5. In the Feature type list, select Road > Fill. Change the color of the
       roads to any color you choose (such as pink).

    6. Click Finish. Copy the JSON code from the resulting pop-up window.

Add the style to your map

    1. In Android Studio, create a resource directory in the res
       directory and name it raw

https://video.udacity-data.com/topher/2019/October/5da8ed95_a08-2-v2/a08-2-v2.png

`//TODO 1.17`
    2. Create a file in res/raw called map_style.json.

    3. Paste the JSON code into the new resource file.

`//TODO 1.18`
    4. Create a TAG class variable above the onCreate() method. This will
       be used for logging purposes.

          private val TAG = MapsActivity::class.java.simpleName

`//TODO 1.19`
    5. In MapsActivity.kt, create a setMapStyle() function that takes in a GoogleMap.

          To set the JSON style to the map, call setMapStyle() on the GoogleMap
          object. Pass in a MapStyleOptions object, which loads the JSON file.
          The setMapStyle() method returns a boolean indicating the success
          of the styling.

            private fun setMapStyle(map: GoogleMap) {
               try {
                   // Customize the styling of the base map using a JSON object defined
                   // in a raw resource file.
                   val success = map.setMapStyle(
                       MapStyleOptions.loadRawResourceStyle(
                           this,
                           R.raw.map_style
                       )
                   )
               }
            }
`//TODO 1.20`
    6. If the styling is unsuccessful, print a log that the parsing has failed.

            private fun setMapStyle(map: GoogleMap) {
               try {
                   …
                   if (!success) {
                       Log.e(TAG, "Style parsing failed.")
                   }
               }
            }

    7. In the catch block if the file can't be loaded, the method throws
       a Resources.NotFoundException.

            private fun setMapStyle(map: GoogleMap) {
               try {
                   …
               } catch (e: Resources.NotFoundException) {
                   Log.e(TAG, "Can't find style. Error: ", e)
               }
            }

The full method should look like this:

            private fun setMapStyle(map: GoogleMap) {
               try {
                   // Customize the styling of the base map using a JSON object defined
                   // in a raw resource file.
                   val success = map.setMapStyle(
                       MapStyleOptions.loadRawResourceStyle(
                           this,
                           R.raw.map_style
                       )
                   )

                   if (!success) {
                       Log.e(TAG, "Style parsing failed.")
                   }
               } catch (e: Resources.NotFoundException) {
                   Log.e(TAG, "Can't find style. Error: ", e)
               }
            }

`//TODO 1.21`
    8. Call the setMapStyle() method in the onMapReady() method passing
       in your GoogleMap object.

            override fun onMapReady(googleMap: GoogleMap) {
               ...
               setMapStyle(map)
            }

    9. Run your app. The new styling should be visible with retro theming
       and roads of your chosen color when the map is in normal mode.

       Note: If you zoom out enough, the map will not show roads anymore so you will not see them.

https://video.udacity-data.com/topher/2019/October/5da8f01e_a08-3-v2/a08-3-v2.png

________________________________________________________________________________

                            10. Styling your marker
________________________________________________________________________________

Reference Documentation

MarkerOptions()
https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions

Style your marker

  You can personalize your map further by styling the map markers. In this
  step, you can change the default red markers into something more groovy.

`//TODO 1.22`
    1. In the onMapLongClick() method, add the following line of code to
       the MarkerOptions() of the constructor to use the default marker
       but change the color to blue:

            .icon(BitmapDescriptorFactory.defaultMarker(BitmapDescriptorFactory.HUE_BLUE))

  onMapLongClickListener() will look like this:

            map.setOnMapLongClickListener { latLng ->
               // A Snippet is Additional text that's displayed below the title.
               val snippet = String.format(
                   Locale.getDefault(),
                   "Lat: %1$.5f, Long: %2$.5f",
                   latLng.latitude,
                   latLng.longitude
               )
               map.addMarker(
                   MarkerOptions()
                       .position(latLng)
                       .title(getString(R.string.dropped_pin))
                       .snippet(snippet)
                     .icon(BitmapDescriptorFactory.defaultMarker(
                       BitmapDescriptorFactory.HUE_BLUE))
               )
            }

    2. Run the app. The markers that appear after you long click are now
       shaded blue. Note that POI markers are still red, because you didn't
       add styling to the onPoiClick() method.

https://video.udacity-data.com/topher/2019/October/5da8eef8_a09-1-v2/a09-1-v2.png

________________________________________________________________________________

                            11. Add an overlay
________________________________________________________________________________

Reference Documentation

  * Ground Overlay
  https://developers.google.com/maps/documentation/android-sdk/groundoverlay

  One way you can customize the Google Map is by drawing on top of it. This
  technique is useful if you want to highlight a particular type of
  location, such as popular fishing spots.

    * Shapes: You can add polylines, polygons, and circles to the map.

    * GroundOverlay objects: A ground overlay is an image that is fixed
      to a map. Unlike markers, ground overlays are oriented to the
      Earth's surface rather than to the screen. Rotating, tilting, or
      zooming the map changes the orientation of the image. Ground overlays
      are useful when you wish to fix a single image at one area on the map

  In this step, you add a ground overlay in the shape of an Android to
  your home location.

    1. Download this Android image and save it in your res/drawable folder.
      (Make sure the file name is android.png.)

https://video.udacity-data.com/topher/2019/October/5da8c987_image17/image17.png

`//TODO 1.23`
    2. In onMapReady(), after the call to move the camera to your
       home’s position, create a GroundOverlayOptions object. Assign the
       object to a variable called androidOverlay:

            val androidOverlay = GroundOverlayOptions()

    3. Use the BitmapDescriptorFactory.fromResource()method to create
       a BitmapDescriptor object from the above image. Pass the object
       into the image() method of the GroundOverlayOptions object:

            val androidOverlay = GroundOverlayOptions()
               .image(BitmapDescriptorFactory.fromResource(R.drawable.android))

`//TODO 1.24`
    4. Set the position property for the GroundOverlayOptions object by
       calling the position() method. Create a float for the width in
       meters of the desired overlay. For this example, a width
       of 100f works well. Pass in the home LatLng object and the size value.

            val overlaySize = 100f
            val androidOverlay = GroundOverlayOptions()
               .image(BitmapDescriptorFactory.fromResource(R.drawable.android))
`//TODO 1.25`
               .position(homeLatLng, overlaySize)

`//TODO 1.26`
    5. Call addGroundOverlay() on the GoogleMap object. Pass in your
       GroundOverlayOptions object:

            map.addGroundOverlay(androidOverlay)

    6. Run the app. Try changing the value of zoomLevel to 18f to see
       the Android image as an overlay.

https://video.udacity-data.com/topher/2019/October/5da8f153_a10-2-v2/a10-2-v2.png

________________________________________________________________________________

                            12. Enable Location Tracking
________________________________________________________________________________

Reference Documentation

  * Location
  https://developers.google.com/maps/documentation/android-sdk/location

  Users often use Google Maps to see their current location. To display
  the device location on your map, you can use the location-data layer.

  The location-data layer adds a My Location button to the top-right
  side of the map. When the user taps the button, the map centers on
  the device's location. The location is shown as a blue dot if the
  device is stationary, and as a blue chevron if the device is moving.

  In this task, you enable the location-data layer.

  Enabling location tracking in Google Maps requires a single line of code.
  However, you must make sure that the user has granted location permissions
  (using the runtime-permission model).

  In this step, you request location permissions and enable the location
  tracking.

`//TODO 1.27`
    1. In the AndroidManifest.xml file, verify that the FINE_LOCATION
       permission is already present. Android Studio inserted this
       permission when you selected the Google Maps template.

              <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

`//TODO 1.28`
    2. In MapsActivity.kt, create a REQUEST_LOCATION_PERMISSION variable.

              private val REQUEST_LOCATION_PERMISSION = 1

`//TODO 1.29`
    3. To check if permissions are granted, create a method in the
       MapsActivity.kt called isPermissionGranted(). In this method, check
       if the user has granted the permission.

            private fun isPermissionGranted() : Boolean {
              return ContextCompat.checkSelfPermission(
                   this,
                  Manifest.permission.ACCESS_FINE_LOCATION) === PackageManager.PERMISSION_GRANTED
            }
`//TODO 1.30`
    4. To enable location tracking in your app, create a method in the
       MapsActivity.kt called enableMyLocation() that takes no arguments
       and doesn't return anything.
       Check for the ACCESS_FINE_LOCATION permission. If the permission
       is granted, enable the location layer. Otherwise, request the permission:

            private fun enableMyLocation() {
               if (isPermissionGranted()) {
                   map.setMyLocationEnabled(true)
               }
               else {
                   ActivityCompat.requestPermissions(
                       this,
                       arrayOf<String>(Manifest.permission.ACCESS_FINE_LOCATION),
                       REQUEST_LOCATION_PERMISSION
                   )
               }
            }
`//TODO 1.31`
    5. Call enableMyLocation() from the onMapReady() callback to enable
       the location layer.

            override fun onMapReady(googleMap: GoogleMap) {
               …
               enableMyLocation()
            }

`//TODO 1.32`
    6. Override the onRequestPermissionsResult() method. If the requestCode
       is equal to REQUEST_LOCATION_PERMISSION permission is granted, and
       if the grantResults array is non empty with
       PackageManager.PERMISSION_GRANTED in its
       first slot, call enableMyLocation():

            override fun onRequestPermissionsResult(
               requestCode: Int,
               permissions: Array<String>,
               grantResults: IntArray) {
               // Check if location permissions are granted and if so enable the
               // location data layer.
               if (requestCode == REQUEST_LOCATION_PERMISSION) {
                   if (grantResults.size > 0 && (grantResults[0] == PackageManager.PERMISSION_GRANTED)) {
                       enableMyLocation()
                   }
               }
            }

    7. Run the app. There should be a pop up requesting access to the
       device's location. Allow the permission.

https://video.udacity-data.com/topher/2019/October/5da8d821_a11-2/a11-2.png

  Note that this pop up may look different if you are not running the
  recommended API 29.

  The map will now display the device's current location using a blue dot.
  Notice that there is a location button on the top right. If you move
  the map away from your location and click on this button it will
  center the map back to the device location.

https://video.udacity-data.com/topher/2019/October/5da8f196_a11-2-v2/a11-2-v2.png


________________________________________________________________________________

                            13. Final Summary
________________________________________________________________________________

In this lesson you created a map and added markers programmatically, on long
click, and on POI click. You also learned how to style your Google
map and enable location tracking.

To check out the solution for this entire lesson, you can visit this Github repo.
https://github.com/udacity/android-kotlin-geo-maps

With the FirebaseUI library you can also support other authentication methods
such as signing in with a phone number. To learn more about the capabilities
of the FirebaseUI library and how to utilize other functionalities
it provides, check out the following resources:

    * Google Maps API
    https://developers.google.com/maps/documentation
    * Maps SDK for Android
    https://developers.google.com/maps/documentation/android-sdk/overview

For more about maps, check out these other resources:

  Codelabs:

  Build your own current place picker
https://developers.google.com/codelabs/maps-platform/location-places-android?index=..%2F..index#0
  Going places with Android
https://codelabs.developers.google.com/?cat=maps-platform&index=..%2F..index#0
  Real time asset tracking
https://codelabs.developers.google.com/?cat=mapsplatform&index=..%2F..index#0
________________________________________________________________________________
