IMAGE_NAME = "bento/ubuntu-16.04"
N = 2

Vagrant.configure("2") do |config|
    config.ssh.insert_key = false

    config.vm.provider "virtualbox" do |v|
        v.memory = 4096
        v.cpus = 2
    end
      
    config.vm.define "k8s-main" do |main|
        main.vm.box = IMAGE_NAME
        main.vm.network "private_network", ip: "192.168.56.10"
        main.vm.hostname = "k8s-main"
        main.vm.provision "ansible" do |ansible|
            ansible.verbose  = "v"
            ansible.playbook = "kubernetes-setup/main-playbook.yml"
            ansible.extra_vars = {
                node_ip: "192.168.56.10",
            }
        end
    end

    (1..N).each do |i|
        config.vm.define "node-#{i}" do |node|
            node.vm.box = IMAGE_NAME
            node.vm.network "private_network", ip: "192.168.56.#{i + 10}"
            node.vm.hostname = "node-#{i}"
            node.vm.provision "ansible" do |ansible|
                ansible.verbose  = "v"
                ansible.playbook = "kubernetes-setup/node-playbook.yml"
                ansible.extra_vars = {
                    node_ip: "192.168.56.#{i + 10}",
                }
            end
        end
    end
end