# Docker packaging for davrods

This repository contains docker packaging for [davrods](https://github.com/UtrechtUniversity/davrods). 
Davrods is an Apache WebDAV interface for [iRODS](https://irods.org), allowing cross platform access to
iRODS files and collections using a standard interface. Packaging davrods in a container allows you to 
the freedom to deploy and upgrade davrods separately from the iRODS servers, even if the two are running
on the same physical server. 

This particular packaging is designed to ease the deployment of a properly configured webdav server 
over HTTPS. Directories are provided in the build environment for you to place certificates and 
keys for your https server, as well as any trust anchors required to secure SSL communications 
to the iRODS server on the backend.

# Installation

## Summary 

Installation can be broken into four conceptual steps: 

1. Select which version of the software to run by cloning the repository and checking out the appropriate branch.
1. Securing SSL keys, certificates, and CA-roots and placing them in the correct build environment directories.
1. Copying template files to the config directory and editing to suit your environment.
1. Build the image and run the container.

When this process is finished, you should have a container which runs davrods. At a minimum, you'll need to map 
ports 80 and 443 to those same ports on your machine. You'll have to decide how you want to manage when you want
the container to start and stop, and what you're going to use to manage that process. These brief instructions
are a tutorial for none of these things.

## Populating the build environment

The repository is structured as follows: 

* templates/
  * _various templates for you to edit and copy to ../buildenv/config_
* buildenv/
  * Dockerfile
  * anchors/
  * certs/
  * config/
  * private/
  
## SSL

The contents of the SSL related directories in the build environment are copied directly to specific directories in 
the `/etc/pki` tree. After the trust anchors are copied, `update-ca-trust` is run. This arrangement is intended to 
make the process of configuring the container for SSL as easy, transparent and painless as possible. If there are 
any questions, a quick inspection of the Dockerfile should resolve any uncertainty as to what is intended. Specifically: 

* `buildenv/anchors` maps to `/etc/pki/ca-trust/source/anchors`
* `buildenv/certs` maps to `/etc/pki/tls/certs`
* `buildenv/private` maps to `/etc/pki/tls/private`

How you might use this: 

1. If you are planning to verify the iRODS server's SSL certificate, put the CA's self-signed certificate in `buildenv/anchors`.
Likewise for any other CA certificates you intend for the davrods server process to recognize.
1. If you have an SSL key and certificate which you intend to have Apache use, place the key in the `buildenv/private` directory
and the cert in the `buildenv/certs` directory.
1. If you have a "ca-bundle" from a CA like GoDaddy, place it in the `buildenv/certs` directory. 

Later, when editing the `davrods-vhost.conf` file, you can simply specify the full path to the certificates, bundle files,
and/or keys that you want the Apache server to use.

## Configuration

In the `templates/` directory, there are a number of files that end in `.templ`. You must copy them to the `buildenv/config` 
directory, stripping off the `.templ` extension in the process. You will need to edit the files so that the server does what 
you want. See the davrods documentation for details.

* `config/irods_environment.json` will be copied to `/etc/httpd/irods`
* `config/davrods-*` will be copied to `/etc/httpd/conf.d`
