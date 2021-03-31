# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = 'digital_ocean'
  config.vm.box_url = "https://github.com/devopsgroup-io/vagrant-digitalocean/raw/master/box/digital_ocean.box"
  config.ssh.private_key_path = 'ssh_keys/do_ssh_key'

  config.vm.synced_folder "remote_files", "/vagrant", type: "rsync"
  
  config.vm.define "minitwit-log-server", primary: true do |server|

    server.vm.provider :digital_ocean do |provider|
      provider.ssh_key_name = "do_ssh_key"
      provider.token = ENV["DIGITAL_OCEAN_TOKEN"]
      provider.image = 'docker-18-04'
      provider.region = 'fra1'
      provider.size = 's-2vcpu-2gb'
      provider.privatenetworking = true
    end

    server.vm.hostname = "minitwit-log-server"
    server.vm.provision "shell", inline: <<-SHELL

    ufw allow 80
    ufw allow 3000
    sysctl -w vm.max_map_count=262144

    echo -e "\nOpening port for minitwit ...\n"
    echo ". $HOME/.bashrc" >> $HOME/.bash_profile

    echo -e "\nConfiguring credentials as environment variables...\n"
    echo "export DOCKER_USERNAME='daaa@itu.dk'" >> $HOME/.bash_profile
    echo "export DOCKER_PASSWORD='1S8&YfL3ycT8'" >> $HOME/.bash_profile
    source $HOME/.bash_profile

    echo -e "\nVagrant setup done ..."
    echo -e "minitwit will later be accessible at http://$(hostname -I | awk '{print $1}'):80"
    SHELL
  end
end
