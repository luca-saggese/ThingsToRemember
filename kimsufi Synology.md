sudo apt-get install virtualbox

VBoxManage createvm --name Synology --ostype Linux26_64 --register

VBoxManage showvminfo Synology

VBoxManage modifyvm Synology --cpus 2 --memory 2048 --vram 12

VBoxManage storagectl Synology --name "SATA Controller" --add sata --bootable on

VBoxManage createhd --filename /home/ubuntu/synology.vdi --size 1024000 --variant Standard

VBoxManage storageattach Synology --storagectl "SATA Controller" --port 0 --device 0 --type hdd --medium /home/ubuntu/synoboot.vdi
VBoxManage storageattach Synology --storagectl "SATA Controller" --port 1 --device 0 --type hdd --medium /home/ubuntu/synology.vdi

VBoxManage modifyvm Synology --nic1 bridged --bridgeadapter1 enp1s0 --macaddress1 0011322CA785

VBoxManage startvm Synology



VBoxManage createvm --name Synology2 --ostype Linux26_64 --register