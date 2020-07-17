# amq-docker-compose-ha

## To Use

    $ docker-compose up

    # One of these will work; the other will not:
    $ open http://localhost:11180
    $ open http://localhost:22280


##  What to Expect

* One node will start up and be active
* The other will start up and be passive, logging the following:

    Database /opt/activemq/data/kahadb/lock is locked by another server. This broker is now in slave mode waiting a lock to be acquired


## Force a Failover

    # Depending on which one is active, run the following command to force a fail-over
    $ docker-compose stop amq-primary-broker
    $ docker-compose stop amq-secondary-broker


## Step-by-step Demo

    $ docker-compose up -d amq-primary-broker
    $ sleep 3
    $ docker-compose up -d amq-secondary-broker

    # First curl should return normally; second should not
    $ curl -v http://localhost:11180
    $ curl -v http://localhost:22280

    $ docker-compose stop amq-primary-broker
    $ sleep 3
    $ docker-compose up -d amq-primary-broker

    # Second curl should return normally; first should not
    $ curl -v http://localhost:11180
    $ curl -v http://localhost:22280
