PokitDok Platform API Client for Salesforce APEX 
=======================================

Resources
---------

Please see the documentation_ for detailed information on all of the PokitDok Platform APIs.
The documentation includes APEX client examples for each of the implemented APIs.

Report API client issues_ on GitHub


Quick start
-----------

.. code-block:: apex

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


License
-------

Copyright (c) 2016 PokitDok, Inc.  See LICENSE_ for details.

.. _documentation: https://platform.pokitdok.com/documentation/v4/?apex#
.. _issues: https://github.com/pokitdok/pokitdok-apex/issues
.. _LICENSE: LICENSE.txt

