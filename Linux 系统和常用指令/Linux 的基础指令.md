# Linux 的基础指令
* ls
* pwd
* cd
* mkdir
    * -p 一步到创建出文件夹
* touch
* cp
    * -r 将文件夹下的内容一块复制
* mv    
* rm
    * -f 强制删除（不会再单个文件确认）
    * -r 删除文件夹
    * ls |grep -v pem |xargs -i rm {} #筛选出列表中没有 pem 内容的文件删掉
* _>_ 将内容覆盖到后者
* _>>_  将内容追加到后者
* cat
* head 查看文件前十行
* tail 查看文件后十行
* clear
* |
