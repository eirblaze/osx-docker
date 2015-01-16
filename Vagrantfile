VAGRANTFILE_API_VERSION = "2"  
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "host" do |h|
    h.vm.box = "hashicorp/precise64"
    h.vm.provider "virtualbox" do |v|
      v.check_guest_additions = false
      v.functional_vboxsf = false
    end
    h.vm.provision "docker"

#    h.vm.provision "shell", :inline => <<EOS
#service docker stop
#sed -ie 's/${DOCKER_OPTS}/--storage-driver=devicemapper/' /etc/default/docker
#service docker start
#EOS
#    h.vm.provision "shell", inline:
#      "ps aux | grep 'sshd:' | awk '{print $2}' | xargs kill"

    h.vm.network :forwarded_port, guest:5432, host:5432
  end
 
  config.vm.define "postgres" do |v|
    v.vm.provider "docker" do |d|
      d.image = "postgres:9.2"
      d.ports = ["5432:5432"]
      d.env = {
        :POSTGRES_USER => "admin",
        :POSTGRES_PASSWORD => "password",
      }
      d.vagrant_vagrantfile = "./Vagrantfile"
      d.vagrant_machine = "host"
    end
  end
end

