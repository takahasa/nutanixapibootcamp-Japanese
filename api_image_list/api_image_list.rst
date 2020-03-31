.. _api_image_list:

----------------------
API: List of Images
----------------------

Overview
++++++++

In this exercise you will list the images on the clusters.  Later exercises
will require the image uuid in the JSON body.  These images are stored in the AHV image service.

.. note::

  Estimated time to complete: **5 MINUTES**



Exercise: List the images on the cluster
+++++++++++++++++++++++++++++++++++++++++++

#. Click + in the main window to create a new tab-window

#. Click the dropdown and select POST

    - v3 standardizes on POST for listing to offer server-side filtering, grouping, and sorting

#. Enter the URL to list images

    - https://{{prism_central_ip}}:9440/api/nutanix/v3/images/list

#. Configure basic authentication for this API call

    - Follow the same steps from the first exercise
    - v3 conforms to HTTP as a stateless protocol such that each API call is authenticated

#. Set the media type to application/json

    - Follow the same steps from the first exercise

#. Fill out the body

    - Click the Body tab
    - Copy or type an empty dictionary in the json body as seen below

    .. code-block:: bash

      {}

    .. figure:: images/apimetajson.png

#. Click Send to submit the v3 API call

  - The intent response provides an array of image resources, similar to GET on one entity
  - Take note of the uuid for the CentOS7 disk image in the metadata section

    .. figure:: images/centosuuid.png


  - Take note of the uuid for the Windows2016 disk image in the metadata section

    .. figure:: images/windowsuuid.png
