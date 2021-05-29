Vagrant.configure("2") do |config|

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

  end # each-loop servers

end # Vagrant.configure
