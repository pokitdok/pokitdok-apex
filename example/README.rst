Salesforce Apex Reference Implementation
========================================

Scenario
--------

Imagine there is a ``Patient`` custom object that stores basic PHI of individual patients at a medical facility.  In order to verify the patient has active insurance coverage, an eligibility request needs to be performed.  

The reference implementation assumes a small ``Patient`` custom object exists that has a custom Javascript button that will execute the code necessary to utilize the PokitDok Platform API Client to make the eligibility request.

It is necessary to have many administrative privileges in order to set up the system to execute the reference implementation.

It is also assumed that the source code has been installed in the sandbox environment.


Patient Object
--------------

Navigate: ``Setup`` > ``Create`` > ``Objects`` > ``New``

Create a new custom object to capture patient data.  Configure the field-level security options for each field as appropriate, and add to the ``Patient Layout``.

+-----------------+--------------------+------------+
| Field Label     | API Name           | Data Type  |
+=================+====================+============+
| Patient ID      | Name               | Text(80)   |
+-----------------+--------------------+------------+ 
| Active Coverage | Active_Coverage__c | Checkbox   |
+-----------------+--------------------+------------+ 
| Birth Date      | Birth_Date__c      | Date       |
+-----------------+--------------------+------------+ 
| First Name      | First_Name__c      | Text(32)   |
+-----------------+--------------------+------------+ 
| Last Name       | Last_Name__c       | Text(32)   |
+-----------------+--------------------+------------+ 
| Member ID       | Member_ID__c       | Text(32)   |
+-----------------+--------------------+------------+ 
| Payer           | Payer__c           | Picklist   |
+-----------------+--------------------+------------+ 
| Plan Begin Date | Plan_Begin_Date__c | Date       |
+-----------------+--------------------+------------+

For the ``Payer`` dropdown, add a single value of *MOCKPAYER*.


Patients Tab
------------

Navigate: ``Setup`` > ``Create`` > ``Tabs`` > ``New``

Create a new ``Patients`` Tab for the ``Patient`` object.  There was a **Caduceus** tab style that seemed appropriate.  You should now be allowed to traverse to the Patients views to create records.


Javascript Button
-----------------

Navigate: ``Setup`` > ``Create`` > ``Objects`` > ``Patient`` > ``New Action``

Create a custom button to call into the APEX code

:Label: Run Eligibility Check
:Name: Run_Eligibility_Check
:Behavior: Execute JavaScript
:OnClick Javascript: See below


.. code-block:: javascript

  {!REQUIRESCRIPT("/soap/ajax/25.0/connection.js")} 
  {!REQUIRESCRIPT("/soap/ajax/25.0/apex.js")} 
  var result = sforce.apex.execute("Eligibility","runEligibilityCheck",{}); 
  alert(result); 
  window.location.reload(); 
  sforce.debug.trace=true;


Navigate: ``Setup`` > ``Create`` > ``Objects`` > ``Patient`` > ``[Edit] Patient Layout`` > ``Buttons``

Add the ``Run Eligibility Check`` button to the ``Custom Buttons`` section on the layout.  While in the Layout, feel free to arrange the fields.


Create A Patient Record
-----------------------

Navigate: ``Patients`` > ``New``

:Patient ID: 1234567890
:First Name: John
:Last Name: Doe 
:Birth Date:  1/1/1970
:Payer: MOCKPAYER
:Member ID: 0987654321


Make An Eligibility Request
---------------------------

After creating the patient, click the ``Run Eligibility Request`` button.


License
-------

Copyright (c) 2016 PokitDok, Inc.  See LICENSE_ for details.

.. _documentation: https://platform.pokitdok.com/documentation/v4/?apex#
.. _issues: https://github.com/pokitdok/pokitdok-apex/issues
.. _example: https://github.com/pokitdok/pokitdok-apex/tree/dev/example
.. _LICENSE: LICENSE.txt

