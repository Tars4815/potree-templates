===========================
Potree Standard Templates
===========================

This page will cover details on how to build a web page with a Potree viewer using already available functionalities.

Examples
---------

* :ref:`basic-viewer`
* :ref:`example2`
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
* :ref:`example32`
* :ref:`example33`
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

.. _example2:

CA13 (18 billion points)
++++++++++++++++++++++++

[TESTO]

..
    code block exampl

.. code-block:: html

  <b><a href="https://labmgf.dica.polimi.it/">link</a></b> - Example of code-block.

"""""""""""""""""""""""""""""""""""""""""""""""

.. _example3:

Retz (Potree + Cesium)
+++++++++++++++++++++++

[TESTO]

.. _example4:

Classifications
+++++++++++++++++++

[TESTO]

.. _example5:

Various Features
+++++++++++++++++

[TESTO]

.. _example6:

Toolbar
+++++++

[TESTO]

.. _example7:

Load Project
++++++++++++

[TESTO]

.. _example8:

Matcap
++++++

[TESTO]

.. _example9:

Virtual Reality
+++++++++++++++

[TESTO]

.. _example10:

Heidentor
+++++++++

[TESTO]

.. _example11:

Lion
+++++

[TESTO]

.. _example12:

Lion LAS
++++++++

[TESTO]

.. _example13:

Lion LAZ
++++++++

[TESTO]

.. _example14:

EPT
++++

[TESTO]

.. _example15:

EPT Binary
++++++++++

[TESTO]

.. _example16:

EPT zstandard
+++++++++++++

[TESTO]

.. _example17:

Clipping Volume
+++++++++++++++

[TESTO]

.. _example18:

Oriented Images
++++++++++++++++

[TESTO]

.. _example19:

Elevation Profile
+++++++++++++++++

[TESTO]

.. _example20:

Measurements
+++++++++++++++++

[TESTO]

.. _example21:

Meshes
++++++

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

.. _example32:

Custom Sidebar Section
++++++++++++++++++++++

`Working example <http://potree.org/potree/examples/custom_sidebar_section.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/custom_sidebar_section.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""


[TESTO]

.. _example33:

Embedded iframe
++++++++++++++++++++++

`Working example <http://potree.org/potree/examples/custom_sidebar_section.html>`__

..
    add centerd image

.. image:: https://github.com/potree/potree/blob/develop/examples/thumbnails/embedded_iframe.png?raw=true
  :align: center

"""""""""""""""""""""""""""""""""""""""""""""""

[TESTO]

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