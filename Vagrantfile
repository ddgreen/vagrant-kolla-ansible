script = <<SCRIPT
apt update && apt upgrade -y
apt install python3-dev libffi-dev gcc libssl-dev -y
apt install python3-venv
python3 -m venv kolla-ansible/venv
source kolla-ansible/venv
pip install -U pip
pip install 'ansible<5.0'
pip install git+https://opendev.org/openstack/kolla-ansible@stable/xena
mkdir -p /etc/kolla
chown $USER:$USER /etc/kolla
cp -r kolla-ansible/venv/share/kolla-ansible/etc_examples/kolla/* /etc/kolla
cp kolla-ansible/venv/share/kolla-ansible/ansible/inventory/* .
SCRIPT

Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2004"
  config.vm.provider :libvirt do |libvirt|
    libvirt.cpus = 4
    libvirt.memory = 10240
  end
  config.vm.define :node1 do |node|
    node.vm.hostname = "node1"
    node.vm.network :private_network, ip:"192.168.50.11"
  end
  config.vm.define :node2 do |node|
    node.vm.hostname = "node2"
    node.vm.network :private_network, ip:"192.168.50.12"
  end
  config.vm.define :node3 do |node|
    node.vm.hostname = "node3"
    node.vm.network :private_network, ip:"192.168.50.13"
  end
  config.vm.provision :shell, :inline => script
end
