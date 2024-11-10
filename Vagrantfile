Vagrant.configure("2") do |config|
    # Configuración de la VM para el primer servidor Apache
    config.vm.define "web1" do |web1|
      web1.vm.box = "ubuntu/bionic64"  # Puedes usar cualquier distribución compatible
      web1.vm.hostname = "web1"
      web1.vm.network "private_network", ip: "192.168.33.10"
      web1.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web1.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
        echo "<h1>Servidor Web 1</h1>" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración de la VM para el segundo servidor Apache
    config.vm.define "web2" do |web2|
      web2.vm.box = "ubuntu/bionic64"
      web2.vm.hostname = "web2"
      web2.vm.network "private_network", ip: "192.168.33.11"
      web2.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      web2.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y apache2
        systemctl enable apache2
        echo "<h1>Servidor Web 2</h1>" > /var/www/html/index.html
      SHELL
    end
  
    # Configuración de la VM para el servidor NGINX que actúa como balanceador de carga
    config.vm.define "loadbalancer" do |loadbalancer|
      loadbalancer.vm.box = "ubuntu/bionic64"
      loadbalancer.vm.hostname = "loadbalancer"
      loadbalancer.vm.network "private_network", ip: "192.168.33.12"
      loadbalancer.vm.provider "virtualbox" do |vb|
        vb.memory = "512"
        vb.cpus = 1
      end
      loadbalancer.vm.provision "shell", inline: <<-SHELL
        apt-get update
        apt-get install -y nginx
        systemctl enable nginx
  
        # Configuración de balanceo de carga
        cat <<EOF > /etc/nginx/conf.d/load_balancer.conf
        upstream backend {
            server 192.168.33.10;
            server 192.168.33.11;
        }
  
        server {
            listen 80;
            location / {
                proxy_pass http://backend;
            }
        }
        EOF
  
        systemctl restart nginx
      SHELL
    end
  end
  