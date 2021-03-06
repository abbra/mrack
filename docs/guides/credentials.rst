How to configure provider's credentials
=======================================

mrack supports several providers by using popular libraries without overriding
how credentials are handled. That means every provider must be configured
differently and according to its library.

This guide explains how you can configure those credentials including pointers
to original resources where you can find more info about that.

AWS
---
mrack uses ``boto3`` library to interact with AWS, thus you can follow the
instructions from Boto's `documentation <https://boto3.amazonaws.com/v1/documentation/api/latest/guide/credentials.html#guide-credentials>`__

The most straightforward way is to use a shared config file. Its default location
is ``~/.aws/config/credentials``. You can override it by setting the ``AWS_CONFIG_FILE``
environment variable.

.. code:: ini

   aws_access_key_id=ACCESS_KEY_ID
   aws_secret_access_key=SECRET_ACCESS_KEY
   region=eu-central-1
   retries=
       max_attempts=5
       mode=standard

OpenStack
---------
Before calling ``mrack`` you must define some environment variables or source the
OpenStack RC file provided by your instance.

.. code:: bash

   $ source myproject-openrc.sh
   $ mrack up

Instructions on where you can find the RC file are available `here <https://docs.openstack.org/mitaka/cli-reference/common/cli_set_environment_variables_using_openstack_rc.html>`__

Beaker
------

Under the ``beaker`` section of your ``provisioning-config.yaml`` file you must
set ``client_conf`` pointing to your Beaker client configuration. E.g:

.. code:: yaml

   beaker:
       distros:
           fedora-33: Fedora-33%
           fedora-32: Fedora-32%

       client_conf: /etc/beaker/client.conf
       keypair: id_rsa.pub

       reserve_duration: 86400
       server_role: idm_ci_bkr_server
       max_attempts: 240

More info about Beaker client configuration is available `here <https://beaker-project.org/docs/user-guide/bkr-client.html>`__

.. note::

    You will need to set the URL of your Beaker server without trailing slash and then configure how your Beaker client authenticates with the Beaker server.
    You can use either password authentication or Kerberos authentication.
