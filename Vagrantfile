# Defines our Vagrant environment
#
# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANT_VM_PROVIDER = "virtualbox"
ANSIBLE_RAW_SSH_ARGS = []
Vagrant.configure("2") do |config|
    N = 3
    config.ssh.insert_key = false

    (1..N-1).each do |machine_id|
        ANSIBLE_RAW_SSH_ARGS << "-o IdentityFile=#{Dir.pwd}/.vagrant/machines/mongo#{machine_id}/virtualbox/private_key"
    end

    (1..N).each do |i|
        config.ssh.private_key_path = "/home/marcos/.vagrant.d/insecure_private_key"
        config.vm.define "mongo#{i}" do |mongo|
            mongo.vm.box = "ubuntu/xenial64"
            mongo.vm.hostname  = "mongo#{i}"
            mongo.vm.network :private_network, ip: "190.120.88.1#{i}"
            
            mongo.vm.provider "virtualbox" do |vb|
                vb.memory = "4096"
                vb.cpus = 2
            end

            # Boot all machines before provisioning
            if i == N
                mongo.vm.provision :ansible do |ansible|
                    ansible.limit           = "all"
                    ansible.playbook        = "provisioning/mongodb.yml"
                    ansible.inventory_path  = "provisioning/inventory.yml"
                    ansible.raw_ssh_args = ANSIBLE_RAW_SSH_ARGS
                    ansible.ask_become_pass = true
                end
            end
        end


    end

    # # Elliot machine
    # config.vm.define "elliot" do |master|
    #     master.vm.hostname  = "elliot"
    #     master.vm.network "private_network", ip: "192.168.50.10"
    #     master.vm.disk :disk, size: "2GB", primary: true
    # end

    # # Darlene machine
    # config.vm.define "darlene" do |master|
    #     master.vm.hostname  = "darlene"
    #     master.vm.network "private_network", ip: "170.154.32.13"
    #     master.vm.disk :disk, size: "2GB", primary: true
    # end

    # # Tyrell machine
    # config.vm.define "tyrell" do |master|
    #     master.vm.hostname  = "tyrell"
    #     master.vm.network "private_network", ip: "140.320.42.15"
    #     master.vm.disk :disk, size: "2GB", primary: true
    # end

    # Angela machine
    # config.vm.define "angela" do |angela|
    #     angela.vm.hostname  = "angela"
    #     angela.vm.network "private_network", ip: "165.120.88.2"
    #     angela.vm.disk :disk, size: "10GB", primary: true
    # end

    # config.vm.provision "ansible" do |ansible|
    #     ansible.playbook       = "provisioning/zookeeper.yml"
    #     ansible.inventory_path = "provisioning/inventory.yml"
    #     ansible.ask_become_pass = true
    # end

    # config.vm.provision "ansible" do |ansible|
    #     ansible.playbook       = "provisioning/kafka.yml"
    #     ansible.inventory_path = "provisioning/inventory.yml"
    #     ansible.ask_become_pass = true
    # end

    # config.vm.provision "ansible" do |ansible|
    #     ansible.playbook       = "provisioning/kubernetes.yml"
    #     ansible.inventory_path = "provisioning/inventory.yml"
    #     ansible.ask_become_pass = true
    # end


end