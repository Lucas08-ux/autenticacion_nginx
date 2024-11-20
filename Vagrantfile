# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.provision "shell", inline: <<-SHELL
     apt-get update
     apt-get install -y git
     apt-get install -y nginx
  SHELL

  config.vm.define "lucas" do |lucas|
    lucas.vm.box = "debian/bookworm64"
    lucas.vm.network "private_network", ip: "192.168.57.102"

    lucas.vm.provision "shell", inline: <<-SHELL

    mkdir -p /var/www/lucas
    cp -vr /vagrant/html /var/www/lucas/html
    chown -R www-data:www-data /var/www/lucas/html
    chmod -R 755 /var/www/lucas

    cp -v /vagrant/lucas /etc/nginx/sites-available/lucas
    ln -s /etc/nginx/sites-available/lucas /etc/nginx/sites-enabled/
    cp -v /vagrant/hosts /etc/hosts

    # Para el hosts del anfitrión Windows (hay posibilidad de que no funcione por temas de permisos. En caso de no funcioar, hay queintroducir manualmente en el archivo hosts de Windows los nombres y las IPs)
    powershell.exe -ExecutionPolicy Bypass -File add_hosts.ps1

    # Creación de usuarios y contraseñas para el acceso web

    sudo sh -c "echo 'lucas:$(openssl passwd -apr1 1234)' > /etc/nginx/.htpasswd"

    sudo sh -c "echo 'gomez:$(openssl passwd -apr1 1234)' >> /etc/nginx/.htpasswd"

    cat /etc/nginx/.htpasswd  

    # Segunda web
    mkdir -p /var/www/web2/html
    git clone https://github.com/Lucas08-ux/web2.git /var/www/web2/html
    chown -R www-data:www-data /var/www/web2/html
    chmod -R 755 /var/www/web2
    cp -v /vagrant/web2 /etc/nginx/sites-available/web2
    ln -s /etc/nginx/sites-available/web2 /etc/nginx/sites-enabled/

    systemctl restart nginx
    systemctl status nginx
    SHELL
  end # lucas
end
