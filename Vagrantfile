# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT

echo Fixing some problems with scotchbox...

echo Fetch repo updates
sudo add-apt-repository ppa:ondrej/php
sudo apt-get update

echo install ruby
sudo apt-get -y install ruby

echo Update sendmail path
echo 'sendmail_path = /usr/bin/env /home/vagrant/.rbenv/shims/catchmail -f dev@scotchbox.local' >> /etc/php/7.0/apache2/conf.d/20-mailcatcher.ini

echo install PHP7 and dependencies
sudo apt-get -y install php7.0
sudo apt-get -y install php7.0-xml
sudo apt-get -y install php-xml
sudo apt-get -y install libapache2-mod-php7.0 libphp7.0-embed libssl-dev openssl php7.0-cgi php7.0-cli php7.0-common php7.0-dev php7.0-fpm php7.0-phpdbg

echo update php settings
echo 'upload_max_filesize = 100M' >> /etc/php/7.0/apache2/conf.d/user.ini
echo 'post_max_size = 100M' >> /etc/php/7.0/apache2/conf.d/user.ini
echo 'max_execution_time = 120' >> /etc/php/7.0/apache2/conf.d/user.ini
echo 'memory_limit = 256M' >> /etc/php/7.0/apache2/conf.d/user.ini

echo Restart apache
sudo service apache2 restart
SCRIPT

# echo Start mailcatcher
# mailcatcher --http-ip=0.0.0.0

Vagrant.configure("2") do |config|
    
    # This is Scotch Box 3.0 (the free version)
    # _________________________________________
    # If you want PHP7, MySQL 5.7, Ubuntu 16.04, Build Scripts (customize your own boxes in minutes)...
    # ... an NGINX version, PHPUnit, Yarn, WebPack, improved email testing with MailHog...
    # ... generally Higher versions of things, Ruby (via RVM), Node (via NVM), WebPack ready, and more.
    
    # Just go to https://box.scotch.io/pro
    # Your support will help keep this project alive!

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.9"
    config.vm.hostname = "scotchbox"
    config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    
    # Optional NFS. Make sure to remove other synced_folder line too
    #config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }

    # run some tasks while setting up box for the first time, $script from above
    config.vm.provision "once", type: "shell" do |once|
        once.inline = $script
    end

    config.vm.provision "always", type: "shell", run: "always" do |always|
        always.inline = '/home/vagrant/.rbenv/shims/mailcatcher --http-ip=0.0.0.0'
    end
end