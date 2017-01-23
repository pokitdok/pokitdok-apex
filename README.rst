PokitDok Platform API Client for Salesforce Apex
================================================

Resources
---------

Please see the documentation_ for detailed information on all of the PokitDok Platform APIs.  The documentation includes Apex client examples for each of the implemented APIs.

Report API client issues_ on GitHub


Quick start
-----------

.. code-block:: java

    // Instantiate a PokitDokAPIClient with your credentials
    PokitDokAPIClient pokitdok = new PokitDokAPIClient('<CLIENT_ID>', '<CLIENT_SECRET>');

    // Create an eligibility request payload
    Map<String,Object> eligibilityRequest = new Map<String,Object>();
    
    Map<String,String> member = new Map<String,String>();
    member.put('birth_date', '1970-01-01');
    member.put('first_name', 'John');
    member.put('last_name', 'Smith');
    member.put('id', '3141592653');
    eligibilityRequest.put('member',member);

    Map<String,String> provider = new Map<String,String>();
    provider.put('first_name', 'Jane');
    provider.put('last_name', 'Doe');
    provider.put('npi', '1467560003');
    eligibilityRequest.put('provider',provider);

    eligibilityRequest.put('trading_partner_id','MOCKPAYER');
        
    // Make the call to the eligibility API.  The response is serialized JSON.
    String eligibilityResponse = pokitdok.eligibility(JSON.serialize(eligibilityRequest));


Installation
------------

There is not presently an artifact for this codebase that can be installed either at the command line or from within the Salesforce AppExchange Marketplace.  The code in this repository should copied, class by class, into a Salesforce Sandbox environment and tested there, before deploying to a Production environment.


Configuration
-------------

In addition to bringing the source code into the Sandbox environment, the system needs to be configured appropriately to allow the HTTP calls to go through.

Navigate: ``Security Controls`` > ``Remote Site Settings`` > ``New``

Create a new ``Remote Site`` with these values:

:Remote Site Name: PokitDok_Platform
:Remote Site URL: https://platform.pokitdok.com
  
Reference the APEX Developer's Guide for additional information about adding a `Remote Site <https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_callouts_remote_site_settings.htm>`_.


Platform Cache
--------------

OAuth session access tokens can be persisted and used across the organization to eliminate the need to authenticate with the PokitDok Platform servers with each API request.  This is accomplished by utilizing the Salesforce Platform Cache.

The Salesforce Platform Cache lets you store and retrieve data that is shared across your organization. Put, retrieve, or remove cache values by using the Cache.Org and Org.Partition classes in the Cache namespace. Use the Platform Cache Partition tool to create or remove org partitions and allocate their cache capacities to balance performance across apps.

Unlike session cache, org cache is accessible across sessions, requests, and org users and profiles. Org cache expires when its specified time-to-live is reached, in this case, 60 minutes.

If your organization has not set up a default cache partition, you may do so by following these steps.

Navigate: ``Setup`` > ``Platform Cache`` > ``New Platform Cache Partition``

Create a default cache with these or similar configuration parameters.

*Details*

:Label: BaseOrgCachePartition
:Name: BaseOrgCachePartition
:Default Partition: X
:Description: Default Org level cache partition

*Org Cache Allocation*

:Organization: 5

Reference the APEX Developer's Guide for additional information about configuring `Platform Cache <https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_cache_namespace_overview.htm>`_.

Testing
-------

There is unit test coverage for the core API Client code. The tests exercise authentication and each of the implemented API endpoints.  Because tests are not allowed to go across the wire, a Mock Response Generator is supplied that will return mocked responses for the endpoints for which they have registered.

The tests has been verified to run successfully on Salesforce on Winter '17 and code coverage is over 93%.

The repository also contains an example reference implementation that will use the API Client to make actual calls against the PokitDok Platform APIs.  You will need to sign up for free at http://platform.pokitdok.com for your own App credentials.  See the example_ page for more information on the configuration necessary to fully test out the actual API calls.


License
-------

Copyright (c) 2017 PokitDok, Inc.  See LICENSE_ for details.

.. _documentation: https://platform.pokitdok.com/documentation/v4/?apex#
.. _issues: https://github.com/pokitdok/pokitdok-apex/issues
.. _example: https://github.com/pokitdok/pokitdok-apex/tree/dev/example
.. _LICENSE: LICENSE.txt
