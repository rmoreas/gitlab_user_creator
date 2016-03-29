# Gitlab external user creator

This small app allows *internal* users to create new [*external users*](https://about.gitlab.com/2016/03/22/gitlab-8-6-released/) (introduced in Gitlab 8.6). For example in our research group we want to invite external collaborators without the need to be a system administrator.

Gitlab itself does not (not yet) allow for fine-grained administration permissions, but thanks to the [REST API](http://doc.gitlab.com/ce/api/README.html) and the [Gitlab OAuth](http://doc.gitlab.com/ce/integration/oauth_provider.html) we can write small apps like this one to meet our needs.

## Setup and installation

### Dependencies
It is common to work in a Python virtualenv. There the basic steps to install all dependencies would be:
```bash
virtualenv venv
. ./venv/bin/activate
pip install -r requirements.txt
```

### Configuration
The app expects a file ```settings.cfg``` containing the following parameters.

| Parameter | Description |
| --------- | ----------- |
| GITLAB_BASE | Base URL to the Gitlab installation (without the /api/v3 part) |
| GITLAB_CONSUMER_KEY | *Application Id* provided by Gitlab |
| GITLAB_CONSUMER_SECRET | *Secret* provided by Gitlab |
| GITLAB_ADMIN_TOKEN | *Private token* of a Gitlab administrator |
| CSRF_SECRET | Some long string which will be used to secure the html forms |
| ... | More Flask parameters can also be provided in this configuration |

An example configuration is provided in [settings.example.cfg](settings.example.cfg).

### Gitlab configuration
1. Register the application for Gitlab OAuth.
   1. In the *Gitlab Admin area*, enter the *Applications* settings.
   1. Start a *New Application* with the corresponding button.
   1. Enter some name, e.g. "External User Creator".
   1. Enter the *Redirect URI*. This must be "DOMAIN/oauth-authorized", where DOMAIN depends on your installation. In development setups you can use "http://localhost:5000/oauth-authorized".
   1. Confirm and copy the *Application Id* and *Secret* generated by Gitlab in your ```settings.cfg```.
1. Obtain an administrator private token.
   1. Login as an admin and navigate to *Profile settings* > *Account*. There you will find the *Private token* which is needed to create users via the API.

### Launching the app
For development purposes one can use the built-in Flask/Werkzeug webserver:
```bash
python run.py
```
For this setup the *Redirect URI* in the Gitlab settings should contain something like ```http://localhost:5000/oauth-authorized```.

In production it is adviced to use some production WSGI server. Read more on the [Flask deployment documentation](http://flask.pocoo.org/docs/deploying/).

## Deployment
Example for a deployment on the same server running our local Gitlab installation is in [DEPLOY.md](DEPLOY.md).

## Copyright
Copyright on the application belongs to the Institute for Theoretical Physics, ETH Zurich.

This project ships copies of the Bootstrap framework and the jQuery library, which contain their own copyright.


## License
Distributed under the Apache License, Version 2.0. (See accompanying file [LICENSE.txt](LICENSE.txt) or copy at [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0))


