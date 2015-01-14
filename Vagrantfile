VAGRANTFILE_API_VERSION = "2"  
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 
  config.vm.define "postgres" do |v|
    v.vm.provider "docker" do |d|
      d.image = "paintedfox/postgresql"
      d.volumes = ["/var/docker/postgresql:/data"]
      d.ports = ["5432:5432"]
      d.env = {
        USER: "root",
        PASS: "pw",
        DB: "root"
      }
      d.vagrant_vagrantfile = "./Vagrantfile.proxy"
    end
  
  end
 
end

