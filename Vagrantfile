# -*- mode: ruby -*-
# vi: set ft=ruby :
#author: TonyFeng

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

	(1..3).each do |i|
		config.vm.define "node0#{i}" do |node|
	
		# 设置虚拟机的Box
		node.vm.box = "centos/7"
		# 设置虚拟机的主机名
		node.vm.hostname="node0#{i}"
		# 设置虚拟机本地(即宿主机)使用的IP
		node.vm.network "private_network", ip: "192.168.33.#{i}"

		if i==1
			# hadoop HDFS NameNode web管理端口
			node.vm.network "forwarded_port", guest: 50070, host: 50071, protocol: "tcp", id: "hdfs_secondary_name_node"
			# hadoop yarn resource manager web端口
			node.vm.network "forwarded_port", guest: 8088, host: 8089, protocol: "tcp", id: "yarn_rm_web"
			# flink web界面端口
			node.vm.network "forwarded_port", guest: 8081, host: 8082, protocol: "tcp", id: "flink_web"
			# nc 端口
			node.vm.network "forwarded_port", guest: 9998, host: 9999, protocol: "tcp", id: "nc"
		end

		if i==2
			node.vm.network "forwarded_port", guest: 50075, host: 50077, protocol: "tcp", id: "node02_datenode"
			node.vm.network "forwarded_port", guest: 8042, host: 8044, protocol: "tcp", id: "node02_nodemanager"
		end
		if i==3
			node.vm.network "forwarded_port", guest: 50075, host: 50078, protocol: "tcp", id: "node03_datenode"
			node.vm.network "forwarded_port", guest: 8042, host: 8045, protocol: "tcp", id: "node03_nodemanager"
		end
		
		# 宿主机与虚拟机22端口转发映
		# config.vm.network "forwarded_port", guest: 22, host: "220#{i}"
		node.vm.network "forwarded_port", guest: 22, host: "220#{i}"

		# 设置宿主机与虚拟机的共享目录 (默认当前目录会挂载到虚机主用户目录下/home/vagrant)
		#node.vm.synced_folder "~/Desktop/share", "/home/vagrant/share"

		# 拷贝相应的依赖文件
		config.vm.provision "file", source: "apps/jdk-8u202-linux-x64.tar.gz", destination: "/home/vagrant/apps/jdk-8u202-linux-x64.tar.gz"
		config.vm.provision "file", source: "apps/hadoop-2.9.2.tar.gz", destination: "/home/vagrant/apps/hadoop-2.9.2.tar.gz"
		config.vm.provision "file", source: "apps/flink-1.7.1-bin-hadoop28-scala_2.12.tgz", destination: "/home/vagrant/apps/flink-1.7.1-bin-hadoop28-scala_2.12.tgz"
		config.vm.provision "file", source: "apps/zookeeper-3.4.12.tar.gz", destination: "/home/vagrant/apps/zookeeper-3.4.12.tar.gz"
		config.vm.provision "file", source: "sshd_config", destination: "/home/vagrant/sshd_config"
		config.vm.provision "file", source: "test", destination: "/home/vagrant/test"
		config.vm.provision "file", source: "yum", destination: "/home/vagrant/yum"
		config.vm.provision "file", source: "scripts/sudoers", destination: "/home/vagrant/scripts"
		config.vm.provision "file", source: "ssh-auto-login", destination: "/home/vagrant/ssh-auto-login"
		if i==2 || i==3
			node.vm.provision "file", source: "hadoop-env-files-for-slaves", destination: "/home/vagrant/hadoop-env-files"
		end
		if i==1
			node.vm.provision "file", source: "hadoop-env-files-for-master", destination: "/home/vagrant/hadoop-env-files"
		end
		# VirtaulBox相关配置
		node.vm.provider "virtualbox" do |v|
			# 设置虚拟机的名称
			v.name = "node0#{i}"
			# 设置虚拟机的内存大小  
			v.memory = 1024
			# 设置虚拟机的CPU个数
			v.cpus = 1
		end
		# node.vm.provision "shell", inline: "echo ${i} >> /home/vagrant/apps/zookeeper/data/myid"
		# node.vm.provision "shell", inline: $clusters_script # 使用shell脚本进行软件安装和配置
		# 配置jdk hadoop flink大数据开发环境变量
		node.vm.provision "shell", path: "scripts/cluster-env.sh" 
		# 更换国内yum源
		node.vm.provision "shell", path: "scripts/replace-yum-repo.sh" 
		# 同步集群时间
		node.vm.provision "shell", path: "scripts/sync-time.sh" 
		# adduser usergroup sudoers
		node.vm.provision "shell", path: "scripts/add-user.sh" 
		# add docker alias
		#node.vm.provision "shell", path: "scripts/add-docker-alias.sh"
		# todo:  auto login without password 
		# node.vm.provision "shell", path: "scripts/ssh-group.sh" 

		end
	end
end
