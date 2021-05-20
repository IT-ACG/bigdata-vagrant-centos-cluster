# bigdata-vagrant-centos-cluster
跨平台的灵活便捷的大数据分布式实验环境部署方案(支持windows,mac,linux平台)

- forward port ruls ✔️ 
- ssh-auto-login ✔️
- sudoers ✔️
- hadoop  ️✔️
- flink   ✔️
- spark   ✘
- hdfs    ✔️
- yarn    ✔️ 
- kafka   ✘ 
- zookeeper ✘
- docker √
- docker alias √ 

# software
Vagrant 2.2.12
VirtualBox 6.1.22

# Useage
1. download vagrant && virtualbox  
2. cd $project  
3. vagrant up  
4. 手动修改node01 host 将 127.0.0.1 node01注释掉(todo 自动化)  
5. 自行配置host，另外相应的端口访问可以参看Vagrantfile中的转发规则  

欢迎提问,邮件 feng-qichao@qq.com  
