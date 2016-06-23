# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.44.10"
    config.vm.hostname = "develop"
    #config.ssh.username = "vagrant"
    #config.ssh.password = "vagrant"
    
    config.vm.synced_folder ".", "/var/www", :smb => { :mount_options => ["username=vagrant","password=vagrant"] }
    #config.vm.synced_folder ".", "/var/www", type: "rsync", rsync__auto: "true", rsync__exclude: ".git/"
	
    # Optional NFS. Make sure to remove other synced_folder line too
    #config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }
    config.vm.provider "virtualbox" do |v|
      host = RbConfig::CONFIG['host_os']

      # Give VM 1/4 system memory 
      if host =~ /darwin/
        # sysctl returns Bytes and we need to convert to MB
        mem = `sysctl -n hw.memsize`.to_i / 1024
      elsif host =~ /linux/
        # meminfo shows KB and we need to convert to MB
        mem = `grep 'MemTotal' /proc/meminfo | sed -e 's/MemTotal://' -e 's/ kB//'`.to_i 
      elsif host =~ /mswin|mingw|cygwin/
        # Windows code via https://github.com/rdsubhas/vagrant-faster
        mem = `wmic computersystem Get TotalPhysicalMemory`.split[1].to_i / 1024
      end

      mem = mem / 1024 / 4
      v.customize ["modifyvm", :id, "--memory", mem]
    end
end
