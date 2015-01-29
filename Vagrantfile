$ports=ENV['PORT_MAPPING'].split(',').map { |mapping|
  host, guest = mapping.split(':')
  {:host => host, :guest => guest}
}

VAGRANTFILE_API_VERSION = "2"  
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "base" do |h|
    h.vm.box = "hashicorp/precise64"
    h.vm.provider "virtualbox" do |v|
      v.check_guest_additions = false
      v.functional_vboxsf = false
    end
    h.vm.provision "docker"

    # https://github.com/mitchellh/vagrant/issues/3998#issuecomment-60359659
    #
    # The following line terminates all ssh connections. Therefore
    # Vagrant will be forced to reconnect.
    # That's a workaround to have the docker command in the PATH
    #
    h.vm.provision "shell", inline:
      "ps aux | grep 'sshd:' | awk '{print $2}' | xargs kill"

    $ports.each do |p|
      h.vm.network :forwarded_port, guest: p[:host], host: p[:host]
    end
  end

  $ports.each do |p|
    config.vm.define "postgres-#{p[:host]}" do |v|
      v.vm.provider "docker" do |d|
        d.image = "postgres:9.2"
        d.ports = ["#{p[:host]}:#{p[:guest]}"]
        d.env = {
          :POSTGRES_USER => "admin",
          :POSTGRES_PASSWORD => "pw",
        }
        d.vagrant_vagrantfile = "./Vagrantfile"
        d.vagrant_machine = "base"
      end
    end
  end
end

