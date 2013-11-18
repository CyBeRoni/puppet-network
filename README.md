#network

####Table of Contents

1. [Overview](#overview)
2. [Module Description](#module-description)
3. [Setup](#setup)
    * [Resources managed by network module](#resources-managed-by-network-module)
    * [Setup requirements](#setup-requirements)
    * [Beginning with module network](#beginning-with-module-network)
4. [Usage](#usage)
5. [Operating Systems Support](#operating-systems-support)
6. [Development](#development)

##Overview

This module configures network interfaces and parameters.

##Module Description

The module is based on **stdmod** naming standards version 0.9.0.

Refer to http://github.com/stdmod/ for complete documentation on the common parameters.


##Setup

###Resources managed by network module
* This module enables the network service
* Can manage any configuration file in the config_dir_path with network::conf
* Can manage interfaces with network::interfaces

###Setup Requirements
* PuppetLabs [stdlib module](https://github.com/puppetlabs/puppetlabs-stdlib)
* PuppetLabs [concat module](https://github.com/puppetlabs/puppetlabs-concat)
* StdMod [stdmod module](https://github.com/stdmod/stdmod)
* Puppet version >= 2.7.x
* Facter version >= 1.6.2

###Beginning with module network

The main class arguments can be provided either via Hiera (from Puppet 3.x) or direct parameters:

        class { 'network':
          parameter => value,
        }


The module provides a generic network::conf define to manage any file in the config_dir_path which is:
  On 'Debian' osfamily: '/etc/network',
  On 'Redhat' osfamily: '/etc/sysconfig/network-scripts',

        network::conf { 'if-up.d/my_script':
          template => 'site/network/my_script',
        }

The module provides as cross OS complaint define to manage single interfaces: network::interface
Note: On Debian if you use network::interface once you must provide ALL the network::interface
      defines for all your interfaces

To configure a dhcp interface on Debian

        network::interface { 'eth0':
          method => 'dhcp',
        }

To configure a dhcp interface on RedHat

        network::interface { 'eth0':
          bootproto => 'dhcp',
        }

To configure a static interface with basic parameters

        network::interface { 'eth1':
          ipaddress => '10.42.42.50',
          netmask   => '255.255.255.0',
        }



##Usage

You have different possibile approaches in the usage of this module. Use the one you prefer.

* Just use the network::interface defines:

        network::interface { 'eth0':
          method => 'dhcp',
        }

        network::interface { 'eth1':
          ipaddress => '10.42.42.50',
          netmask   => '255.255.255.0',
        }

* Use the main network class and the interfaces_hash to configure all the interfaces (ideal with Hiera, here the parameter is explicitely passed):

        class { 'network':
          interfaces_hash => {
            'eth0' => {
              method    => 'dhcp',
            },
            'eth1' => {
              ipaddress => '10.42.42.50',
              netmask   => '255.255.255.0',
            },
          },
        }

* Use the main network class and the usual stdmod parameters to manage the (main) network configuration file

        class { 'network':
          config_file_template => 'site/network/network.conf.erb',
        }

* Manage the whole configuration directory

        class { 'network':
          config_dir_source  => 'puppet:///modules/site/network/conf/',
        }



##Operating Systems Support

This is tested on these OS:
- RedHat osfamily 5 and 6
- Debian 6 and 7
- Ubuntu 10.04 and 12.04


##Development

Pull requests (PR) and bug reports via GitHub are welcomed.

When submitting PR please follow these quidelines:
- Provide puppet-lint compliant code
- If possible provide rspec tests
- Follow the module style and stdmod naming standards

When submitting bug report please include or link:
- The Puppet code that triggers the error
- The output of facter on the system where you try it
- All the relevant error logs
- Any other information useful to undestand the context