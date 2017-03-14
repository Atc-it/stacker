<!--
  Title: Stacker - Environment for local web development, ready for use. Run Symfony, Laravel, Yii, and other frameworks easy! You can also run native php.
  Description: Quickly start of developing locally with Nginx, PHP7, Mysql, Pgsql, Mailcatcher and Redis.
               No e-mail is send externally, everything is catched by mailcatcher.
  Author: maxlab
  Badge genrator: https://poser.pugx.org/
  -->
<p align="center">
<img alt="Frameworks a lot - he's one!" src="/logo.png">
</p>

[![Build Status](https://travis-ci.org/Maxlab/stacker.svg?branch=master)](https://travis-ci.org/Maxlab/stacker)
[![License](https://poser.pugx.org/maxlab/stacker/license)](https://packagist.org/packages/maxlab/stacker)
[![Gratipay User](https://img.shields.io/gratipay/user/maxlab.svg)](https://gratipay.com/~maxlab)
[![Bountysource](https://img.shields.io/bountysource/team/maxlabstacker/activity.svg)](https://www.bountysource.com/teams/maxlabstacker)
[![Donate](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=Q477VJVB9STGS)

#### Frameworks a lot - he's one!
Environment for local web development, ready for use. Run Symfony, Laravel, Yii, and other frameworks easy! You can also run native php.

- 1 video presentation - https://youtu.be/qVqzYMczuwM (RU)
- 2 video phpStorm + xDebug + Stacker = profit! - https://youtu.be/RYnRamdZJ-Q (RU)
- 3 video stacker console, composer, gem, npm and etc. - https://youtu.be/WBFMs35ucfk (RU)
- 4 video stacker Run Symfony, Laravel and native php scripts - https://youtu.be/TONMezpUqkc (RU)

#### General goals
- Frameworks a lot - he's one!
- Everything is easy, nothing to migrate
- Quickly start of developing locally
- No overhead on settings! Сopied project and run
- Zoo under a Docker, let the host mashine remains clean!


## Installation
- Install [docker](https://docs.docker.com/)
- Install docker [compose](https://docs.docker.com/compose/install/) > 1.8.0
- Clone this project 
```sh 
$ git clone git@github.com:Maxlab/stacker.git
```

## Start

#### Run in Stacker directory 
```sh 
# make ./workspace folder and make a symbolic link to your folder with your projects 
$ mkdir workspace && ln -s /your_path/to_your_own_projects ./workspace

$ docker-compose build && docker-compose up -d && docker-compose ps

$ \*.dev > 127.0.0.1 (if you use boot2docker, use that ip)
$ sudo apt-get update && sudo apt-get install dnsmasq
$ mv ./test ./workspace
```
- add a file `/etc/dnsmasq.d/dev.conf` with `address=/.dev/127.0.0.1`
- (if need) hosts
  - add `127.0.0.1 test.project.dev` to your hosts file `/etc/hosts`
  - add `127.0.0.1 mail.dev` to your hosts file `/etc/hosts`  
- Then open your browser and open http://test.php.dev/


#### For pure PHP
- add your project in workspace folder `./workspace/<customer>/<projectname>/htdocs` (no need to restart, this will work out of the box)
- open http://customer.project.dev/ in your browser (if you do not have dnsmasq, you have to add your hosts file manually).

#### For Symfony2
- add your Sf2 project in workspace folder `./workspace/<customer>/<projectname>` (no need to restart, this will work out of the box)
- open http://customer.project.dev/ in your browser (if you do not have dnsmasq, you have to add your hosts file manually).


## On the ship
- mailcatcher   -> schickling/mailcatcher:latest (all outgoing mail is sent to http://mail.dev/)
- nginx         -> nginx:1.10.1
- elasticsearch -> elasticsearch:5
- mysql         -> mysql:5.7
- pgsql         -> postgres:9.6   
- php7xdebug    -> php:7.0 + xdebug
- php5apache    -> php:5apache for legacy
- php7console   -> stacker console
- redis         -> redis:3.0

## Console
- *ZSH* + [oh-my-zsh](http://ohmyz.sh/)
- For frontend: nodejs, gem, npm, bower, gulp, uglify-js, uglifycss 
- For backend: composer, php, phpunit, symfony, symfony-autocomplete, Yii2 autocomplete

## FAQ

#### Which settings in the configs for my projects?
- Database
    - You can access the database in your app config use `db` for mysql and `pgsql` for posgrqsql
        (Files will be saved in the mysql directory so it will be saved after destroying or recreating the containers)
    ```yaml
      #Example mysql
      parameters:
        database_host: mysql
        database_port: 3306
        database_name: sf
        database_user: root
        database_password: root
    
      #Example pgsql
      parameters:
        database_host: pgsql
        database_port: 5433
        database_name: sf
        database_user: postgres
        database_password: postgres
      
      #Example redis
      parameters:
        database_host: redis
        database_port: 6379
    ```

#### What external ports are listening images?
- It's easy. For convenience, the external ports of the databases are offset by plus one. 
    For example, mysql listens to port 3306+1=3307 and so on...
- Check the file [docker-compose.yml](/docker-compose.yml) for more 

#### How to Configure local wildcard DNS server(for linux)
- Install Dnsmasq: sudo apt-get install dnsmasq
- Since Ubuntu's NetworkManager uses dnsmasq, and since that messes things up a little for us, open up /etc/NetworkManager/NetworkManager.conf and comment out (#) the line that reads dns=dnsmasq. Restart NetworkManager afterwards: sudo restart network-manager.
- Make sure Dnsmasq listens to local DNS queries by editing /etc/dnsmasq.conf, and adding the line listen-address=127.0.0.1.
- Create a new file in /etc/dnsmasq.d (eg. /etc/dnsmasq.d/dev.conf), and add the line address=/.dev/127.0.0.1 to have dnsmasq resolve requests for *.dev domains. Restart Dnsmasq: sudo /etc/init.d/dnsmasq restart.

#### xDebug + PhpStorm configuration
- Watch this video https://youtu.be/RdmcGAAQGfI
-dinclude_path=./:/usr/local/lib/php:/root/.composer/vendor/phpunit   

#### I have a lot of the Symphony project, is it possible to make a symbolic link to them? 
- Yes! It's much faster and easier, plus no need to move folders from the usual places.
- In the directory with your projects, create a folder and copy all the projects from the Symphony code. 
Now, make a link to your directory project in the directory with the Stacker, 
remove a directory ./workspace and rename your link to workspace - that's all! 
Now all your Symphony projects is available from the browser.

#### How to contact the any instances Staker in console?
You can do so
```sh 
$ /your_path/to_stacker_folder/bin/stacker console
```
But, it will be much better
```sh
# for bash
$ echo 'export PATH=/your_path/to_stacker_folder/bin:$PATH' >> ~/.bashrc && source ~/.bashrc 
# for ~/.zshrc
$ echo 'export PATH=/your_path/to_stacker_folder/bin:$PATH' >> ~/.zshrc && source ~/.zshrc
# then restart console and run
$ stacker console
```

#### Symfony completion
```sh
$ stacker console
$ cd to_symfony_folder
$ sf [tab*2] # for sf3 completion OR sf2 for sf2 completion
```

## Commands
```sh
$ stacker usage # for list available commands
$ stacker console # for enter to console
$ stacker logs <cont_name> -f # for logs stream container
$ stacker build && stacker down && stacker up && stacker ps #for full rebuild
```





