Vagrant.configure("2") do |config|

  ###################################################################################
  # define number of workers
  W = 1

  # provision W VMs as workers
  (1..W).each do |i|

    # name the VMs
    config.vm.define "rkeworker#{i}" do |node|

      # which image to use
      node.vm.box = "opensuse/Leap-15.2.x86_64"

      # sizing of the VMs
      node.vm.provider "libvirt" do |lv|
        lv.random_hostname = true
        lv.memory = 1024
        lv.cpus = 2
      end

      # set the hostname
      node.vm.hostname = "rkeworker#{i}"

      # set the IP address
      node.vm.network "private_network", ip: "192.168.123.2#{i}"

    end # config.vm.define workers

  end # each-loop workers

  ###############################################

  # define number of servers
  M = 3

  # provision N VMs as servers
  (1..M).each do |i|

    # name the VMs
    config.vm.define "rkeserver#{i}" do |node|

      # which image to use
      node.vm.box = "opensuse/Leap-15.2.x86_64"

      # sizing of the VMs
      node.vm.provider "libvirt" do |lv|
        lv.random_hostname = true
        lv.memory = 2048
        lv.cpus = 2
      end

      # set the hostname
      node.vm.hostname = "rkeserver#{i}"

      # set the IP address
      node.vm.network "private_network", ip: "192.168.123.1#{i}"

    end # config.vm.define servers

    # if this is the last machine
    if i == M

      config.vm.provision "shell" do |shell_provisioner|
        shell_provisioner.inline = <<-SHELL
          sudo zypper ref
          sudo zypper -n in docker
          sudo usermod -aG docker vagrant
          sudo systemctl start docker
          sudo systemctl enable docker
          cat /vagrant/ssh_key_rke.pub >> /home/vagrant/.ssh/authorized_keys
        SHELL
      end

    end # if-condition last machine

  end # each-loop servers

end # Vagrant.configure
