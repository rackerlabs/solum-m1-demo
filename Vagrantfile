#############################################################################
# This VagrantFile runs the Solum M1 Demo
#############################################################################

MEM = ENV['SOLUM_MEM'] ||= '4096'
IP  = ENV['SOLUM_IP']  ||= '192.168.76.11'

Vagrant.configure("2") do |config|

  config.vm.box      = 'solum-m1-demo'
  config.vm.box_url  = 'http://e83ea3bf5684a2dd541e-86585e01c603f42ab62a7136a9dd2acc.r76.cf1.rackcdn.com/solum-m1-demo.box'

  # DevStack with Nova that may have Docker driver and/or Solum.
  config.vm.define :devstack do |devstack|
    devstack.vm.hostname = 'devstack'
    devstack.vm.network :private_network, ip: IP

    devstack.vm.provider "virtualbox" do |v|
      v.customize ["modifyvm", :id, "--memory", MEM]
      v.customize ["modifyvm", :id, "--cpus", 2]
      v.customize ["modifyvm", :id, "--nicpromisc1", "allow-all"]
    end

    devstack.vm.provision :shell, :inline => <<-SCRIPT
      su vagrant -c "/home/vagrant/devstack/stack.sh"
      docker tag solum/slugrunner 127.0.0.1:5042/slugrunner
      docker push 127.0.0.1:5042/slugrunner
    SCRIPT


  end

end
