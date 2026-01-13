# cloud-sovereignty-demo
Cloud Sovereignty Demonstration with Skupper

This demonstration requires Skupper v2.0.0 or later.

To allow access to Podman for Skupper:

    $ systemctl --user enable podman.socket --now

=====
To setup and populate a local postgresql database:
Note that the database setup (i.e. db-name, user, password, etc.) can be seen in the below code.

    $ components/db-populate

=====
To interact with the database:
    $ psql -d demo-db -U demo -w

=====
Bringing up the network on the following clusters:
Hub-cluster (namespaces npdemo-hub-a, npdemo-hub-b)

    $ ./hub-setup

Onprem cluster (current namespace)

    $ ./onprem-setup hq

Podman site (for access to the database)

    $ ./podman-setup

Processing-site (namespace npdemo-site-${SITE_NAME})

    $ ./kubesite-setup <site-name>
    $ ./np-deploy <site-name> <country-code-lower-case>

Setting up the console on a site

    $ helm install skupper-network-observer oci://quay.io/skupper/helm/network-observer --version 2.1.3
    $ oc create route passthrough skupper-console --service=skupper-network-observer --port=https
