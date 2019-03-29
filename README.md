Reference (Minimal) Brick Server
================================

Brick Server is an implementation of Brick API on top of a timeseries database (TimescaleDB) and a metadata database (Virtuoso). Data in buildings are represented as streams of timestamped-values generated from [Point](http://brickschema.org/schema/1.0.3/Brick#Point)s which may record a physical property or actuate a device. Metadata helps to find the right data streams that an application needs. Data and metadata are in different formats, and thus we have to store them in different databases. Brick Server provides a single API endpoint to access those two different data.

1. It would help you understand the general method of integrating your system with Brick.
2. It is an emulator of a building for Brick applications development.
3. It is a testbed for new functions in Brick.

If you want to learn more about Brick, please visit [brickschema.org](https://brickschema.org) for materials.

### Design Goals
- Functional correctness
- Usable performance

### What we HAVE
1. Implementation of Brick API endpoint.
2. Example backend databases.
3. Interface to different databases.

### What we WILL have
1. Will add a general app authorization mechanism with Brick.
2. Will gradually add support for other databases.
3. Will integrate geometry of buildings.
4. Will integrate a semantic for actuation function.

### What we DON'T have
1. We do not have exact data of full buildings. For the access to example dataset, please visit XXX, YYY.


## System Requirements
- OS: We have tested this only on Ubuntu 18.04.
- Python 3.5> is required.


## Getting Started
1. Install and run Virtuoso (Note: Please use only released versions in Virtuoso repository. [link](https://github.com/openlink/virtuoso-opensource/releases))
2. Install and run TimescaleDB/PostgreSQL. [link](https://docs.timescale.com/getting-started/installation)
3. Install pip packages by ``pip install -r requirements.txt``
4. Configure the databases and configure ``configs/configs.json``
    1. Create a database with a proper name (e.g., ``brick``,
    2. Create a role/user of the database.
    3. Add them inside ``configs/configs.json``
5. Run ``python entry.py`` to run the server.
6. Run ``python examples/test_api.py`` in another console.

### Alternative: Use Ansible

Ansible is a piece of provisioning software that simplifies the process of installing software on local or remote machines. Ansible is launched from a **control machine** (usually your laptop or personal computer) and targets a set of **hosts**, which consists of a list of IP addresses (this can contain your local machine if you use `localhost`). If you are unfamiliar with Ansible, we recommend reading the [Getting Started Documentation](https://docs.ansible.com/ansible/2.7/user_guide/intro_getting_started.html#getting-started)

1. Instal dependencies:
    - Make sure Python >=3.6 is installed on the deployment server.
    - Enable Filesystem ACL  in the host. ([for Ubuntu](https://help.ubuntu.com/community/FilePermissionsACLs<Paste>))
2. Install ansible on your local machine
    - follow instructions for the [control machine](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-the-control-machine)
3. Edit your `/etc/ansible/hosts` file to include the remote host and any SSH key you need; for example

    ```ini
    [ec2test]
    52.91.98.98 ansible_ssh_private_key_file=/path/to/my/ssh/key
    ```
    Ansible documentation for this step can be found [here](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)

4. Edit the `ansible/brick-server-playbook.yml` file:

    ```yaml
    - hosts:
      - ec2test
      remote_user: ubuntu
      # ...
    ```

    Edit the remote user name and the hosts designator appropriately. Note that the `ec2test` here is
    the same as in the `/etc/ansible/hosts` file.

5. Run `ansible-playbook brick-server-playbook.yml` from the `ansible` directory.

# References
- Brick (BuildSys 2016)
- Brick (EnergyPlus 2018)
