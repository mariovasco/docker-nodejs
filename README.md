<figure>
<img src="http://ww1.prweb.com/prfiles/2015/07/21/12907174/gI_146921_dchq-logo.png" alt="" />
</figure>

To run & manage this simple Docker Node.js "Hello World" application on 18 different clouds and virtualization platforms (including vSphere, OpenStack, AWS, Rackspace, Microsoft Azure, Google Compute Engine, DigitalOcean, IBM SoftLayer, etc.), make sure that you either:
-   **Sign Up for FREE on DCHQ.io** -- <http://dchq.io> (no credit card required), or
-   **Download DCHQ On-Premise Standard Edition for FREE** -- <http://dchq.co/dchq-on-premise-download.html>

[![Customize and Run](https://dl.dropboxusercontent.com/u/4090128/dchq-customize-and-run.png)](https://www.dchq.io/landing/products.html#/library?org=DCHQ)


Customize & Run all the published Docker Node.js application templates and many other templates (including multi-tier Java application stacks, Python, Ruby, PHP, Mongo Replica Set Cluster, Drupal, Wordpress, MEAN.JS, etc.)

### Node.js Hello World

[![Customize and Run](https://dl.dropboxusercontent.com/u/4090128/dchq-customize-and-run.png)](https://www.dchq.io/landing/products.html#/library?org=DCHQ&bl=2c91802d5244241d0152527faac31e62)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
node:
  image: dchq/nodejs:latest
  mem_min: 50m
  host: host1
  cpu_shares: 1
  publish_all: true
  cluster_size: 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


### Nginx and Node.js

[![Customize and Run](https://dl.dropboxusercontent.com/u/4090128/dchq-customize-and-run.png)](https://www.dchq.io/landing/products.html#/library?org=DCHQ&bl=2c91802d5244241d015247fbea0b5579)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
LB:
  image: nginx:latest
  publish_all: true
  host: host1
  mem_min: 50m
  plugins:
    - !plugin
      id: 0H1Nk
      restart: true
      lifecycle: on_create, post_scale_out:AppServer, post_scale_in:AppServer
      arguments:
        # Use container_private_ip if you're using Docker networking
        - servers=server {{node | container_private_ip}}:8080;
        # Use container_hostname if you're using Weave networking
        #- servers=server {{AppServer | container_hostname}}:8080;
node:
  image: dchq/nodejs:latest
  mem_min: 100m
  host: host1
  cpu_shares: 1
  publish_all: false
  cluster_size: 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

### Apache HTTP Server and Node.js

[![Customize and Run](https://dl.dropboxusercontent.com/u/4090128/dchq-customize-and-run.png)](https://www.dchq.io/landing/products.html#/library?org=DCHQ&bl=2c91802d5244241d0152480e5c605990)

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
HTTP-LB:
  image: httpd:latest
  publish_all: true
  mem_min: 50m
  host: host1
  plugins:
    - !plugin
      id: uazUi
      restart: true
      lifecycle: on_create, post_scale_out:AppServer, post_scale_in:AppServer
      arguments:
        # Use container_private_ip if you're using Docker networking
        - BalancerMembers=BalancerMember http://{{node | container_private_ip}}:8080
        # Use container_hostname if you're using Weave networking
        #- BalancerMembers=BalancerMember http://{{node | container_hostname}}:8080
node:
  image: dchq/nodejs:latest
  mem_min: 100m
  host: host1
  cpu_shares: 1
  publish_all: false
  cluster_size: 1
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

