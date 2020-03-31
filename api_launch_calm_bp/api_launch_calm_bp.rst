.. _api_launch_calm_bp:

----------------------
(Optional) API: Launch Calm BP
----------------------

Overview
++++++++

This exercise is optional.  A Calm Blueprint must already be present on your PC instance.  This API is meant to demonstrate a simple Blueprint launch from the API.
Some of the information in the lab will be provided by the instructor.

.. note::

  Estimated time to complete: **5 MINUTES**



Exercise: Lanch Calm Blueprint
+++++++++++++++++++++++++++++++++++++++++++

#. Obtain the UUID of the Blueprint.

#. Click the dropdown and select POST

    - v3 standardizes on POST for listing to offer server-side filtering, grouping, and sorting

#. Enter the URL to of the Calm Blueprint.  The UUID of the Blueprint is required.

    - https://{{prism_central_ip}}:9440/api/nutanix/v3/blueprints/<blueprint-uuid>/simple_launch

#. Configure basic authentication for this API call

        - Click the **Authorization** tab and select **Basic Auth** from the Type dropdown
        - Enter Prism credentials of the cluster, and click **Update Request**:
            - **Username** - admin
            - **Password** - Use the “Prism login password” from handout

        .. figure:: images/basicauth.png

#. Set the media type to application/json

        - Click the Body tab
        - Select the radio button for raw
        - Click the Text dropdown and select JSON (application/json)

        .. figure:: images/jsonmediatype.png

#. Fill out the body

    - Click the Body tab
    - Copy or type json body as seen below.  Be sure to add your initials the app_name field.

    .. code-block:: bash


      {
      "spec": {
        "app_name": "LaunchDemoPostman-<Initials>",
        "app_description": "An example of a simple blueprint launch via the v3 REST API",
        "app_profile_reference": {
            "kind": "app_profile",
            "name": "AHV",
            "uuid": "<app_uuid>"
            }
        }
      }



#. Click Send to submit the v3 API call

#. Check the application status under Calm within Prism Central.
