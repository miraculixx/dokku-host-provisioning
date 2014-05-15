Dokku Vagrant Configuration
-----------------------------

Uses [Vagrant](http://www.vagrantup.com/) to create and update digital ocean droplets that runs a specific version of Dokku, Docker and various plug-ins.

To use the vagrant configurations, you need the vagrant digital ocean plugin:

    vagrant plugin install vagrant-digitalocean

Some general configuration variables are necessary for the configurations before provisioning the instances:

    export DIGITAL_OCEAN_CLIENT_ID="replaceme"
    export DIGITAL_OCEAN_API_KEY="replaceme"
    export DIGITAL_OCEAN_REGION="Amsterdam 2"
    export PAPERTRAIL_PORT="12345"

Set configuration that depends on DNS and performance requirements (Note: Dokku needs wildcard subdomain registration to be able to map virtual hosts based on sub-domains):

Example 1:

    export VHOST=foodev.com
    export SIZE=8GB

Example 2:

    export VHOST=foo.com
    export SIZE=4GB

To provision a dokku-enabled instance running in digital ocean:

    export HOSTNAME=dokku.$VHOST
    cd vagrant/dokku/
    mkdir -p build/$HOSTNAME
    cd build/$HOSTNAME
    erb ../../Vagrantfile.erb > Vagrantfile
    erb ../../provision.sh.erb > provision.sh
    cp -r ../../shell-scripts-to-include shell-scripts-to-include

First time, run:

    vagrant up --provider=digital_ocean

With an already running droplet:

    vagrant provision

To enter the virtual machine:

    vagrant ssh

## Shell scripts

docker-enter.sh - Uses nsenter to step into a running container
remove-cid-files-that-dont-have-active-containers.sh - Makes sure that all dokku container files contain ids to running docker containerslimit-dokku-deployments.sh - Use to limit commit-specific deployments
limit-dokku-deployments.sh - Use to limit commit-specific deployments
