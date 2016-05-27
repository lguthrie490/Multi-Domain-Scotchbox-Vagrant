# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox"
    # config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]
    
    # Optional NFS. Make sure to remove other synced_folder line too
    config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }
    
    config.vm.provision "shell", inline: <<-SHELL
	
	# Put the local domains you want here
	# A folder with the domain's name will be automatically generated! Neat!
	DOMAINS=("")

	## Loop through all sites on provision
	for ((i=0; i < ${#DOMAINS[@]}; i++)); do

		## Current Domain
		DOMAIN=${DOMAINS[$i]}

		echo "Creating directory for $DOMAIN..."
		mkdir -p /var/www/$DOMAIN

	        echo "Creating vhost config for $DOMAIN..."
        	sudo cp /etc/apache2/sites-available/scotchbox.local.conf /etc/apache2/sites-available/$DOMAIN.conf

        	echo "Updating vhost config for $DOMAIN..."
      		sudo sed -i s,scotchbox.local,$DOMAIN,g /etc/apache2/sites-available/$DOMAIN.conf
        	sudo sed -i s,/var/www/public,/var/www/$DOMAIN/public,g /etc/apache2/sites-available/$DOMAIN.conf

        	echo "Enabling $DOMAIN. Will probably tell you to restart Apache..."
        	sudo a2ensite $DOMAIN.conf

        	echo "So let's restart apache..."
        	sudo service apache2 restart

         done
	
	SHELL
end
