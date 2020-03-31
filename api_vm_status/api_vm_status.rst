.. _api_vm_status:

----------------------
API: VM Status
----------------------

Overview
++++++++

.. note::

  Estimated time to complete: **5 MINUTES**


Exercise: Get the status of your VM
++++++++++++++++++++

#. Click + in the main window to create a new tab-window

#. Confirm that GET is selected in the dropdown

#. Enter the URL to get your VM

    - Start with the URL from the previous exercise: https://{{prism_central_ip}}:9440/api/nutanix/v3/vms
    - Append /{{uuid}} to the URL, using the uuid from the metadata section noted in the VM creation response from exercise 2

    .. figure:: images/appenduuid.png

#. Configure basic authentication for this API call

    - Follow the same steps from the previous exercise
    - v3 conforms to HTTP as a stateless protocol such that each API call is authenticated

#. Click Send to submit the v3 API call

    - The intent response contains two important sections:
        - The status exposes the current state of the entity
        - The spec describes the final state desired by the user
    - The state attribute in the status section allows a user to know if the system has transformed the entity from its current state to the final state
    - If the state is PENDING instead of COMPLETE, click Send to get the VM again

    .. figure:: images/getstatus.png
