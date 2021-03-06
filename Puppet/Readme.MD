## Puppet


### Puppet Types
  
      1. Master-Slave
      2. Stand Alone

### Puppet resource Types

      1. Built-In / Core  -> Puppet team will maintain and Develop.
      2. Defined          -> Edit the default resource and do customization on that.
      3. Custom           -> Write from scratch with Ruby language.

### Commands

  - Check the puppet code
  
    `puppet parser validate`
  
  - Test the puppet code
  
    `puppet apply --noop`
  
  - Run the puppet code
  
    `puppet apply`
    
    
 ## Puppet Resource
 
 ### Create a User ( create_user.pp )
 
       user { "anand":
        ensure => "present",
        uid    => "1011",
        shell  => "/bin/sh",
       }
       
### Create a File inside a new folder

        file { "/root/anand":
          ensure => "directory",
          }

        file { "/root/anand/test.txt":
          ensure   => "present",
          owner    => "anand",
          group    => "anand",
          mode     => "755",
          content  => "Hello Anand !! This is Puppet code learning",
         }
         
### Install a Package

        package { "httpd":
          ensure   => "present",
         }

        service { "httpd":
          ensure  => "running",
         }


## Puppet Classes

      Puppet classes are collection of puppet resources bundle to gether as a single unit.
      
### Application Deployment with Puppet Classes

          class httpddeploy {

            package { "httpd":
              ensure  => "present",
             }

            file { "/var/www/html/index.html":
              content  => "Hi all this is an Application Deployment with Puppet code",
             }

            service { "httpd":
              ensure  => "running",
            }
          }
          
         include httpddeploy
         
         
## Execute the Manifest files in Agent servers like below.

   Master : 

      - vi /etc/puppetlabs/code/environments/production/manifests/site.pp

              class httpddeploy {

                package { "httpd":
                  ensure  => "present",
                 }

                file { "/var/www/html/index.html":
                  content  => "Hi all this is an Application Deployment with Puppet code",
                 }

                service { "httpd":
                  ensure  => "running",
                }
              }

              include httpddeploy
  
   Slave :
 
        - For testing/validating the manifest file 

          `puppet agent -tv --noop`

        - Run the manifest file on Agent

          `puppet agent -tv`

  
  
## Node Defination

  To bind a Single/more agents with a  manifest file then go with Node Defination.
  
  - vi /etc/puppetlabs/code/environments/production/manifests/site.pp
  
          node "puppetslave.cisco.com", "puppetslave02.cisco.com" {
              include httpddeploy
            }
            
          Note : If you didn't specify other agent info in site.pp file then it will throw an error at checking time.So , do like below.
          
          node "default" {
          
          }
            
  - Manifest File to run the things( vi /etc/puppetlabs/code/environments/production/manifests/httpddeploy.pp )
  
        class httpddeploy {

            package { "httpd":
              ensure  => "present",
             }

            file { "/var/www/html/index.html":
              content  => "Hi all !! this is an Application Deployment with Puppet code",
             }

            service { "httpd":
              ensure  => "running",
            }
          }


    - Now , Run the manifest file/site.pp file from Agengt.
    
        `puppet agent -tv --noop`
        
        `puppet agent -tv`
   
      
  

## Puppet-Modules-Fogue


### For Installing the VIM Module in Slave servers

  There are 2 ways to download the Modules
    
   1. Manually
        
          - Download the required Module from forge.puppet.com
          
            `wget https://forge.puppet.com/v3/files/WhatsARanjit-vim-0.3.0.tar.gz`
            
          - Then Insatll Manually
          
            `puppet module install /root/WhatsARanjit-vim-0.3.0.tar.gz`
            
          - List Out the Avilable modules in Master
           
            `puppet module list`
            
          - Add the module in Master server in site.pp
          
             `
               node "puppetslave.cisco.com" {

                  include httpddeploy

                  class { "vim":

                   }
                }
           `
           - Finally Ran this from Agent ( Else by deafult puppet agent will take the latest changes for every 30 mins default )
             `puppet agent -tv --noop`  --> for Dry run mode
             
             `puppet agent -tv `
    
   2. Using Puppet Module Install
   
   
          - Search for puppet module directly here.
          
            `puppet module search jenkins`
            
          - Indtall Module Directly
          
            `puppet module install puppet-jenkins`
            
          - Edit the site.pp file directly.
          
            `
               node "puppetslave.cisco.com" {

                  include httpddeploy

                  class { "jenkins":

                   }
                }
           `
        
           - Finally Ran this from Agent ( Else by deafult puppet agent will take the latest changes for every 30 mins default )
                     
                     `puppet agent -tv --noop`  --> for Dry run mode
                     

                     `puppet agent -tv `
