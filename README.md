# shell

写的一些shell脚本
##远程文件同步脚本
```
#!/bin/bash
file_name=$1  #获取的第一个参数为文件名
file_path=$2  #获取的第二个参数为文件路径,不包含文件名
echo $file_name
echo $file_path
while read line
do 
        ip=`echo $line | cut -d' ' -f 1`
	echo $ip
        user_name=`echo $line | cut -d' ' -f 2`
	echo $user_name
        password=`echo $line | cut -d' ' -f 3`
	echo $password
	sshpass -p $password ssh $user_name@$ip mkdir -p $file_path </dev/null
	./expect.sh $file_name $user_name $ip $file_path $password
done < machine_info
```
```
#!/usr/bin/expect -f
set timeout -1
set src_file [lindex $argv 0]
set user_name [lindex $argv 1]
set host_ip [lindex $argv 2]
set dest_file [lindex $argv 3]
set pwd [lindex $argv 4]
spawn scp $src_file $user_name@$host_ip:$dest_file
expect {
	"*(yes/no)?"
		{
			send "yes\r"
			expect "*assword:" {
				send "$pwd\r"
			}
		}
	"*assword:"
		{
			send "$pwd\r"
		}
}
expect eof
```
```
192.168.23.12 xll 123
192.168.23.13 xll 123
192.168.23.14 xll 123
```
