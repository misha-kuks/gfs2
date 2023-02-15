MACHINES = {
  :iscsi => {
        :box_name => "centos/7",
        :ip_addr => '192.168.56.150',
  :disks => {
    :sata1 => {
       :dfile => './iscsi1-sata1.vdi',
       :size => 1024,
       :port => 1
          },
          
      
        }
  },
  :pcm1 => {
        :box_name => "centos/7",
        :ip_addr => '192.168.56.101',
	:disks => {
		:sata1 => {
			:dfile => './pcm1-sata1.vdi',
			:size => 1024,
			:port => 1
		},
		

	}

		
  },
  :pcm2 => {
        :box_name => "centos/7",
        :ip_addr => '192.168.56.102',
	:disks => {
		:sata1 => {
			:dfile => './pcm2-sata1.vdi',
			:size => 1024,
			:port => 1
		},
	
	 }

		
  },
  :pcm3 => {
        :box_name => "centos/7",
        :ip_addr => '192.168.56.103',
	:disks => {
		:sata1 => {
			:dfile => './pcm3-sata1.vdi',
			:size => 1024,
			:port => 1
		},
	
	 }

		
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s

          #box.vm.network "forwarded_port", guest: 3260, host: 3260+offset

          box.vm.network "private_network", ip: boxconfig[:ip_addr]

          box.vm.provider :virtualbox do |vb|
                vb.name = boxname.to_s
            	  vb.customize ["modifyvm", :id, "--memory", "1024"]
                  needsController = false
		  boxconfig[:disks].each do |dname, dconf|
			  unless File.exist?(dconf[:dfile])
				vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
                                needsController =  true
                          end

		  end
                  if needsController == true
                     vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
                     boxconfig[:disks].each do |dname, dconf|
                         vb.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
                     end
                  end
          end
 	

      end
  end
end