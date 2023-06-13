###  修改储存数据库的位置
1. 停止MySQL服务：在"服务"里找到MySQL服务，停止
2. 在 ```ProgramData/MySQL/MySQL Server 8.0``` 下，找到 ```my.ini``` 文件
3. 修改文件权限（或将文件拖出到桌面进行修改）
4. 将 ```datadir=C:/ProgramData/MySQL/MySQL Server 8.0/Data```注释掉，并新增行 ```datadir=....``` 为自己的路径
5. 将原路径下Data文件夹的内容复制到自己修改后的路径下
6. 重新启动MySQL
7. 进入mysql，输入 ```show variables like '%datadir';```，查看是否修改成功