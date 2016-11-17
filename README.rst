PokitDok Platform API Client for Salesforce APEX 
================================================

Resources
---------

Please see the documentation_ for detailed information on all of the PokitDok Platform APIs.  The documentation includes APEX client examples for each of the implemented APIs.

Report API client issues_ on GitHub


Quick start
-----------

.. code-block:: java

    // Instantiate a PokitDokAPIClient with your credentials
    PokitDokAPIClient pokitdok = new PokitDokAPIClient('<CLIENT_ID>', '<CLIENT_SECRET>');

    // Create an eligibility request payload
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

In addition to bringing the source code into the Sandbox environment, the system need to be configured appropriately to allow the HTTP calls to go through.  Create a ``Remote Site`` by traversing ``Security Controls`` > ``Remote Site Settings`` and use these values:

:Remote Site Name: PokitDok_Platform
:Remote Site URL: https://platform.pokitdok.com
  

Testing
-------

There is unit test coverage for the core API Client code. The tests exercise authentication and each of the implemented API endpoints.  Because tests are not allowed to go across the wire, a Mock Response Generator is supplied that will return mocked responses for the endpoints for which they have registered.

The tests has been verified to run successfully on Salesforce on Winter '17.

The repository also contains an example reference implementation that will use the API Client to make actual calls against the PokitDok Platform APIs.  You will need to sign up for free at http://platform.pokitdok.com for your own App credentials.  See the example_ page for more information on the configuration necessary to fully test out the actual API calls.


License
-------

Copyright (c) 2016 PokitDok, Inc.  See LICENSE_ for details.

.. _documentation: https://platform.pokitdok.com/documentation/v4/?apex#
.. _issues: https://github.com/pokitdok/pokitdok-apex/issues
.. _example: https://github.com/pokitdok/pokitdok-apex/tree/dev/example
.. _LICENSE: LICENSE.txt

