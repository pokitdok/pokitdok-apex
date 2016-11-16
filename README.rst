PokitDok Platform API Client for Salesforce APEX 
=======================================

Resources
---------

Please see the documentation_ for detailed information on all of the PokitDok Platform APIs.
The documentation includes Python client examples for each API.

Report API client issues_ on GitHub


Quick start
-----------

.. code-block:: python

    import pokitdok

    pd = pokitdok.api.connect('<your client id>', '<your client secret>')

    #submit an eligibility request
    pd.eligibility({
        "member": {
            "birth_date": "1970-01-01",
            "first_name": "Jane",
            "last_name": "Doe",
            "id": "W000000000"
        },
        "trading_partner_id": "MOCKPAYER"
    })

Making Requests
---------------

The client offers a few options for making API requests.
High level functions are available for each of the APIs for convenience.
If your application would prefer to interact with the APIs at a lower level,
you may elect to use the general purpose request method or one of the http method aliases built around it.

.. code-block:: python

    # a low level "request" method is available that allows you to have more control over the construction of the API request
    pd.request('/activities', method='get')

    pd.request('/eligibility/', method='post', data={
        "member": {
            "birth_date": "1970-01-01",
            "first_name": "Jane",
            "last_name": "Doe",
            "id": "W000000000"
        },
        "trading_partner_id": "MOCKPAYER"
    })

    # Convenience methods are available for the commonly used http methods built around the request method
    pd.get('/activities')

    pd.post('/eligibility/', data={
        "member": {
            "birth_date": "1970-01-01",
            "first_name": "Jane",
            "last_name": "Doe",
            "id": "W000000000"
        },
        "trading_partner_id": "MOCKPAYER"
    })

    # higher level functions are also available to access the APIs
    pd.activities()

    pd.eligibility({
        "member": {
            "birth_date": "1970-01-01",
            "first_name": "Jane",
            "last_name": "Doe",
            "id": "W000000000"
        },
        "trading_partner_id": "MOCKPAYER"
    })


Authentication and Authorization
--------------------------------

Access to PokitDok APIs is controlled via OAuth2.  Most APIs are accessible with an
access token acquired via a client credentials grant type since scope and account context
are not required for their use.  If you're just interested in using APIs that don't
require a specific scope and account context, you simply supply your app credentials
and you're ready to go:


.. code-block:: python

    import pokitdok

    pd = pokitdok.api.connect('<your client id>', '<your client secret>')



if you'd like your access token to automatically refresh when using the authorization flow, you can connect like this:

.. code-block:: python

    pd = pokitdok.api.connect('<your client id>', '<your client secret>', auto_refresh=True)


That instructs the Python client to use your refresh token to request a new access token
when the access token expires after 1 hour.

For APIs that require a specific scope/account context in order to execute,  you'll need to request
authorization from a user prior to requesting an access token.

.. code-block:: python

    def new_token_handler(token):
        print('new token received: {0}'.format(token))
        # persist token information for later use

    pd = pokitdok.api.connect('<your client id>', '<your client secret>', redirect_uri='https://yourapplication.com/redirect_uri', scope=['user_schedule'], auto_refresh=True, token_refresh_callback=new_token_handler)

    authorization_url, state = pd.authorization_url()
    #redirect the user to authorization_url


You may set your application's redirect uri value via the PokitDok Platform Dashboard (https://platform.pokitdok.com)
The redirect uri specified for authorization must match your registered redirect uri exactly.

After a user has authorized the requested scope, the PokitDok Platform will redirect back to your application's
Redirect URI along with a code and the state value that was included in the authorization url.
If the state matches the original value, you may use the code to fetch an access token:

.. code-block:: python

    pd.fetch_access_token(code='<code value received via redirect>')


Your application may now access scope protected APIs on behalf of the user that authorized the request.
Be sure to retain the token information to ensure you can easily request an access token when you need it
without going back through the authorization code grant redirect flow.   If you don't retain the token
information or the user revokes your authorization, you'll need to go back through the authorization process
to get a new access token for scope protected APIs.

Check SSL protocol and cipher
-----------------------------

.. code-block:: python

    pd.request('/ssl/', method='get')

Supported Python Versions
-------------------------

This library aims to support and is tested against these Python versions:

* 2.6.9
* 2.7.6
* 3.4.0
* 3.5.0
* PyPy

You may have luck with other interpreters - let us know how it goes.

License
-------

Copyright (c) 2016 PokitDok, Inc.  See LICENSE_ for details.

.. _documentation: https://platform.pokitdok.com/documentation/v4/?apex#
.. _issues: https://github.com/pokitdok/pokitdok-apex/issues
.. _LICENSE: LICENSE.txt

