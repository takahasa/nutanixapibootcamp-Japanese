.. _api_delete_vm:

----------------------
API: Delete VM
----------------------

Overview
++++++++

In this exercise you will delete the VM you created in exercise 2.

.. note::

  Estimated time to complete: **5 MINUTES**

Exercise: Delete your VM
++++++++++++++++++++++++++++++

#. Click + in the main window to create a new tab-window

#. Confirm that **Delete** is selected in the dropdown

#. Enter the URL to for your VM Start with the URL from the previous exercise:

    - https://{{prism_central_ip}}:9440/api/nutanix/v3/vms
    - Append /{{uuid}} to the URL, using the uuid from the metadata section noted in the VM creation response in exercise 2.

#. Configure basic authentication for this API call

    - Follow the same steps from exercise 1

#. Click Send to submit the delete API call
