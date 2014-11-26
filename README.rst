Dokku-Alt Server Setup
======================

This is a set of `Ansible <http://docs.ansible.com/>`_ playbooks to provision a
`Dokku-Alt <https://github.com/dokku-alt/dokku-alt>`_ server running for my
own personal PAAS.

These won't work out of the box for provisioning your own server but you
are welcome to fork and adapt them to your own needs under the BSD License. See the
`LICENSE <https://github.com/mlavin/dokku-playbooks/blob/master/LICENSE>`_ file for more details.


Example Usage
---------------------

To get started you must install the requirements::

    $ mkvirtualenv -p /usr/bin/python2.7 dokku-playbooks
    (dokku-playbooks) $ pip install -r requirements.txt


The original bootstraping of a fresh server involves only creating a non-root
user with password-less sudo associated with my SSH key::

    (dokku-playbooks) $ ansible-playbook bootstrap.yml -i hosts.ini -u root --ask-pass

Once that initial user is created you will no longer need to connect as root. In fact
once you have run the playbook for the first time you will not longer be allowed
to connect as root::

    (dokku-playbooks) $ ansible-playbook -i hosts.ini site.yml

This will install and configure a few base packages as well as finally Docker
and Dokku-Alt. To add an admin user for Dokku you need to send the SSH key to
the ``dokku access:add`` command::

    (dokku-playbooks) $ cat ~/.ssh/id_rsa.pub | ssh 104.171.115.182 sudo dokku access:add

The tasks are tagged so that only parts of the deployment can be run such as::

    (dokku-playbooks) $ ansible-playbook -i hosts.ini site.yml --tags "packages,security"

To see the list of available tags you'll have to read through the playbooks.
