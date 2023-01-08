===========================
Potree Standard Templates
===========================

This page will cover details on how to build a web page with a Potree viewer using already available functionalities.

Examples
---------

* :ref:`basic-viewer`
* :ref:`ca13`
* :ref:`example3`
* :ref:`example4`
* :ref:`example5`
* :ref:`example6`
* :ref:`example7`
* :ref:`example8`
* :ref:`example9`
* :ref:`example10`
* :ref:`example11`
* :ref:`example12`
* :ref:`example13`
* :ref:`example14`
* :ref:`example15`
* :ref:`example16`
* :ref:`example17`
* :ref:`example18`
* :ref:`example19`
* :ref:`example20`
* :ref:`example21`
* :ref:`example22`
* :ref:`example23`
* :ref:`example24`
* :ref:`example25`
* :ref:`example26`
* :ref:`example27`
* :ref:`example28`
* :ref:`example29`
* :ref:`example30`
* :ref:`example31`
* :ref:`custom-sidebar`
* :ref:`iframe`
* :ref:`gradient-colors`

.. _basic-viewer:

Basic Viewer
++++++++++++

`Working example <http://potree.org/potree/examples/viewer.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/viewer.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

After cloning the Potree develop repository as suggested in section [reference], navigate to the *examples* folder and search for the `viewer.html file <https://github.com/potree/potree/blob/develop/examples/viewer.html>`__.
This file template includes the basic settings for a functional Potree Viewer and represents the basis for all the other examples too.

In the **head** section you can find all the stilesheets' references for each library needed in the basic viewer, defining the rendering of the Potree navigation area as well as the sidebar appearance.
In this section, like every HTML page, it is possible to define the page title together with metadata information about the author and the content of the document.

..
    code block example for basic viewer

.. code-block:: html

  <head>
	  <meta charset="utf-8">
	  <meta name="description" content="">
	  <meta name="author" content="">
	  <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
	  <title>Potree Viewer</title>

	  <link rel="stylesheet" type="text/css" href="../build/potree/potree.css">
	  <link rel="stylesheet" type="text/css" href="../libs/jquery-ui/jquery-ui.min.css">
	  <link rel="stylesheet" type="text/css" href="../libs/openlayers3/ol.css">
	  <link rel="stylesheet" type="text/css" href="../libs/spectrum/spectrum.css">
	  <link rel="stylesheet" type="text/css" href="../libs/jstree/themes/mixed/style.css">
  </head>

"""""""""""""""""""""""""""""""""""""""""""""""

Diving in the **body** section, first used JS libraries and dependencies are included.

..
    js libraries included in the bbody section

.. code-block:: html

	<script src="../libs/jquery/jquery-3.1.1.min.js"></script>
	<script src="../libs/spectrum/spectrum.js"></script>
	<script src="../libs/jquery-ui/jquery-ui.min.js"></script>
	<script src="../libs/other/BinaryHeap.js"></script>
	<script src="../libs/tween/tween.min.js"></script>
	<script src="../libs/d3/d3.js"></script>
	<script src="../libs/proj4/proj4.js"></script>
	<script src="../libs/openlayers3/ol.js"></script>
	<script src="../libs/i18next/i18next.js"></script>
	<script src="../libs/jstree/jstree.js"></script>
	<script src="../build/potree/potree.js"></script>
	<script src="../libs/plasio/js/laslaz.js"></script>

"""""""""""""""""""""""""""""""""""""""""""""""

The Potree container class is then defined, settings also the renderer area and the sidebar elements too. By changing the *background.jpg* path in the renderer area, it is possible to change the background image that appears at the beginning of the rendering.

..
    Potree container class code

.. code-block:: html

	<div class="potree_container" style="position: absolute; width: 100%; height: 100%; left: 0px; top: 0px; ">
		<div id="potree_render_area" style="background-image: url('../build/potree/resources/images/background.jpg');"></div>
		<div id="potree_sidebar_container"> </div>
	</div>

"""""""""""""""""""""""""""""""""""""""""""""""

In the following lines, the script create a new viewer and define the scene settings. By calling different functions, it also defines appeareance options like:

* **Eye-Dome Lightning**: it could be enabled or disabled with *.setEDLEnabled()*;
* **Field of View**, defining the numerical value for the view angle with *.setFOV()*;
* **Point Budget** sets the default point population for point cloud rendering *.setPointBudget()*;
* **Size of Octree Cells** giving a numerical value as input to *.setMinNodeSize()*;
* **Background** appearance, choosing as parameter for *.setBackground()* one of the following 4 defaults options: skybox, gradient, black and white;
* **Description**, a text defined with *.setDescription()* that supports HTML and appears on the top of the renderer area;
* **Setting parameters**, loading with *.loadSettingsFromURL()* a file with the desired settings for appearance.

When applying *.loadGUI()*, it is possible to set the default style of the Potree sidebar by:

* Setting the display **language**: simply put the language code inside *.setLanguage()*;
* Choosing the **visibility of sidebar sections** at loading: this can be done by passing the section class name (e.g. #menu_tools) inside *$("#menu_tools").next().show()*. The other class name are: [UPDDATE];
* The **visibility of the entire sidebar**, set as true when including *.toggleSidebar()*.

..
    Script defining the Potree Viewer and scene

.. code-block:: html

	<script>
		window.viewer = new Potree.Viewer(document.getElementById("potree_render_area"));
		iewer.setEDLEnabled(false);
		viewer.setFOV(60);
		viewer.setPointBudget(1_000_000);
		viewer.loadSettingsFromURL();
		viewer.setBackground("skybox");
		viewer.setDescription("Point cloud courtesy of <a target='_blank' href='https://www.sigeom.ch/'>sigeom sa</a>");
		
		viewer.loadGUI(() => {
			viewer.setLanguage('en');
			$("#menu_tools").next().show();
			("#menu_clipping").next().show();
			viewer.toggleSidebar();
		});
	</script>

"""""""""""""""""""""""""""""""""""""""""""""""

After setting the viewer and scene parameter, it's time to include the point cloud. This can be done through the *.loadPointCloud()* function, including in the parenthesis:

* the path to the file of the point cloud;
* the name of the point cloud that will appear in the scene section of the sidebar (e.g. "sigeom.sa");

Then, a series of parameters is set inside the loading function:

* **material.size** defines the size of dots used for the cloud rendering;
* **material.pointSizeType** indicates the point sizing view mode to be adopted in the render, choosing between the following options: *FIXED*, *ATTENUATED* or *ADAPTIVE*;
* **material.shape** sets the shape used for point shape rendering. It could be *SQUARE*, *CIRCLE* or *PARABOLOID*;
* **material.pointColorType** makes possible to select the attribute to be used for cloud coloring in the renderer area.

All these parameters corresponds to the properties accessible after clicking on the point cloud from the objects list in the scene sidebar section.

The *.addPointCloud()* is then applied to the scene to which the pointcloud should be added.
Additionally, [Define ]

..
    Potree code for adding a point cloud to a scene

.. code-block:: html

	<script>
  		// Load and add point cloud to scene
		Potree.loadPointCloud("../pointclouds/vol_total/cloud.js", "sigeom.sa", e => {
			let scene = viewer.scene;
			let pointcloud = e.pointcloud;
			
			let material = pointcloud.material;
			material.size = 1;
			material.pointSizeType = Potree.PointSizeType.ADAPTIVE;
			material.shape = Potree.PointShape.SQUARE;
			material.pointColorType = Potree.PointColorType.RGB;

			scene.addPointCloud(pointcloud);
			
			viewer.fitToScreen();
			// scene.view.setView(
			// 	[589974.341, 231698.397, 986.146],
			// 	[589851.587, 231428.213, 715.634],
			// );
		});
	</script>

"""""""""""""""""""""""""""""""""""""""""""""""

.. _ca13:

CA13 (18 billion points)
++++++++++++++++++++++++

`Working example <http://potree.org/potree/examples/ca13.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/ca13.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

After cloning the Potree develop repository as suggested in section [reference], navigate to the *examples* folder and search for the `ca13.html file <https://github.com/potree/potree/blob/develop/examples/ca13.html>`__.
This file template includes the basic settings for a functional Potree Viewer that includes a point cloud with more than 18 billion points.

[TESTO]

..
    code block exampl

.. code-block:: html

  <b><a href="https://labmgf.dica.polimi.it/">link</a></b> - Example of code-block.

"""""""""""""""""""""""""""""""""""""""""""""""

.. _example3:

Retz (Potree + Cesium)
+++++++++++++++++++++++

`Working example <http://potree.org/potree/examples/cesium_retz.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/cesium_retz.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example4:

Classifications
+++++++++++++++++++

`Working example <potree.org/potree/examples/classifications.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/classifications.jpg?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example5:

Various Features
+++++++++++++++++

`Working example <http://potree.org/potree/examples/features_sorvilier.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/features_sorvilier.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example6:

Toolbar
+++++++

`Working example <http://potree.org/potree/examples/toolbar.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/toolbar.jpg?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example7:

Load Project
++++++++++++

`Working example <http://potree.org/potree/examples/load_project.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/load_project.jpg?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example8:

Matcap
++++++

`Working example <http://potree.org/potree/examples/matcap.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/matcap.jpg?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example9:

Virtual Reality
+++++++++++++++

`Working example <https://potree.org/potree/examples/vr_heidentor.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/heidentor.jpg?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example10:

Heidentor
+++++++++

`Working example <http://potree.org/potree/examples/heidentor.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/heidentor.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example11:

Lion
+++++

`Working example <http://potree.org/potree/examples/lion.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/lion.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example12:

Lion LAS
++++++++

`Working example <http://potree.org/potree/examples/lion_las.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/lion_las.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example13:

Lion LAZ
++++++++

`Working example <http://potree.org/potree/examples/lion_laz.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/lion_las.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example14:

EPT
++++

`Working example <http://potree.org/potree/examples/ept.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/lion.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example15:

EPT Binary
++++++++++

`Working example <http://potree.org/potree/examples/ept_binary.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/lion_las.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example16:

EPT zstandard
+++++++++++++

`Working example <http://potree.org/potree/examples/ept_zstandard.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/lion_las.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example17:

Clipping Volume
+++++++++++++++

`Working example <http://potree.org/potree/examples/clipping_volume.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/clipping_volume.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example18:

Oriented Images
++++++++++++++++

`Working example <http://potree.org/potree/examples/oriented_images.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/oriented_images.jpg?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example19:

Elevation Profile
+++++++++++++++++

`Working example <http://potree.org/potree/examples/elevation_profile.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/elevation_profile.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example20:

Measurements
+++++++++++++++++

`Working example <http://potree.org/potree/examples/measurements.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/measurements.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example21:

Meshes
++++++

`Working example <http://potree.org/potree/examples/meshes.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/meshes.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example22:

Multiple Point Clouds
+++++++++++++++++++++

`Working example <http://potree.org/potree/examples/multiple_pointclouds.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/multiple_point_clouds.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example23:

Camera Animation
++++++++++++++++

`Working example <http://potree.org/potree/examples/camera_animation.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/camera_animation.jpg?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example24:

Features (CA13)
+++++++++++++++

`Working example <http://potree.org/potree/examples/features_ca13.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/features_ca13.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example25:

Annotations
++++++++++++

`Working example <http://potree.org/potree/examples/annotations.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/annotations.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example26:

Hierarchical Annotations
++++++++++++++++++++++++

`Working example <http://potree.org/potree/examples/annotation_hierarchy.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/annotation_hierarchy.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example27:

Animation Paths
++++++++++++++++++++++++

`Working example <http://potree.org/potree/examples/animation_paths.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/animation_paths.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example28:

Shapefiles
++++++++++

`Working example <http://potree.org/potree/examples/shapefiles.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/shapefiles.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example29:

Cesium CA13
++++++++++++

`Working example <http://potree.org/potree/examples/cesium_ca13.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/cesium_ca13.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example30:

Geopackage
++++++++++++

`Working example <http://potree.org/potree/examples/geopackage.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/geopackage.jpg?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _example31:

Cesium Sorvilier
++++++++++++++++

`Working example <http://potree.org/potree/examples/cesium_sorvilier.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/cesium_sorvilier.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _custom-sidebar:

Custom Sidebar Section
++++++++++++++++++++++

`Working example <http://potree.org/potree/examples/custom_sidebar_section.html>`__

..
    Custom sidebar example screenshot

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/custom_sidebar_section.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

After cloning the Potree develop repository as suggested in section [reference], navigate to the *examples* folder and search for the `custom_sidebar_section.html file <https://github.com/potree/potree/blob/develop/examples/custom_sidebar_section.html>`__.
This file template includes the basic settings for a functional Potree Viewer (:ref:`basic-viewer`) equipped with examples of custom sidebar.

In the **body** section inside the script block, after defining the basic settings for initialising the viewer, the sidebar section are called inside the **.loadGUI()** function: 
First the new section name is declared with a variable (let section). To define its name and keeping the style coherent with the rest of the sidebar sections, simply substitute *Metadata* with the desired title. Advanced style settings can be performed as well accoring to CSS sintax.
Then the title is attached to the content variable (let content).
Finally the the section is populated with an HTML-compatible content (content.html()). This can include texts as well as other media.
The new section is then inserted in the standard sidebar by first setting the toggling functionaly on click (*.slideToggle()*) and then indicating its order position in relation to the other sections (*.insertBefore()*).
The visibility at first page loading is set by including the code the section variable name followed by *.hide()*.

..
    Potree custom sidebar example code

.. code-block:: html

  <script type="module">
    viewer.loadGUI(() => {
			viewer.toggleSidebar();
			
			let section = $(`
				<h3 id="menu_meta" class="accordion-header ui-widget"><span>Metadata</span></h3>
				<div class="accordion-content ui-widget pv-menu-list"></div>
			`);
			let content = section.last();
			content.html(`
			<div class="pv-menu-list">
				A custom Section in the sidebar!<br>
				<br>	
				Uncomment "content.hide();" to hide content by default.<br>
				<br>
				Take a look at src/viewer/sidebar.html and sidebar.js to 
				learn how the other sections were populated.
			</div>
			`);
			section.first().click(() => content.slideToggle());
			section.insertBefore($('#menu_about'));
			
		});
  </script>

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

.. _iframe:

Embedded iframe
++++++++++++++++++++++

`Working example <http://potree.org/potree/examples/embedded_iframe.html>`__

..
    Embedded iframe screenshot

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/embedded_iframe.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

This example simply illustrates how to set up a webpage with a Potree Viewer embedded in iframe HTML element. The example code is available in the *examples* folder in the `embedded_iframe.html file <https://github.com/potree/potree/blob/develop/examples/embedded_iframe.html>`__

The iframe implementation is possible once a proper viewer is set up in a dedicated html file. In the case of the code example, the iframe is included in the **body** section in a **div** element and refers to the Potree viewer set in the :ref:`basic-viewer` example.
In order to change the viewer page to be embedded, subtitute *viewer.html* in the code below with the name of the html page in which you defined a Potree viewer.

..
    Potree iframe example

.. code-block:: html

  <html>
    <head></head>
    <body>
      <div style="position: absolute; left: 20%; right: 20%; top: 20%; bottom: 20%">
        <iframe src="viewer.html" style="width: 100%; height: 100%"></iframe>
      </div>
    </body>
  </html>

"""""""""""""""""""""""""""""""""""""""""""""""

In the div element additional settings are defined, including the style and the position of the element within the page following CSS standards. 

.. _gradient-colors:

Gradient colors
+++++++++++++++

`Working example <http://potree.org/potree/examples/gradient_colors.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/gradient_colors.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""


[TESTO]

"""""""""""""""

// UNDER CONSTRUCTION //