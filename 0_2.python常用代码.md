<h2>python常用代--文件操作

#### 一:读取文件并按行切分
```python
import sys                #导入sys包,获取命令行参数
from itertools import islice

taskList=sys.argv[1]      #获取第一个参数,即task.txt路径,任务列表
outFile=sys.argv[2]       #获取第二个参数,即输出文件路径
task_id=sys.argv[3]       #获取第三个参数,即task_id,任务ID

inpath = open(taskList)   #打开task.txt
outpath = outFile         #打开输出文件
oridata = {}

def getAttr():
	for line in islice(inpath, 0, None):     #一行一行读取输入文件内容   
		info = line.split("\t")              #使用分隔符分割数据
		job_number = info[0].split("@")[0]   #获取任务ID,job_number=11

		if oridata.has_key(job_number):      
			oridata[job_number] = oridata[job_number] + "#" + info[1]
		else:
			oridata[job_number] = info[1]    #以job_number为key，以\t后面的内容为value，存入oridata字典中

	for key, value in oridata.items():      #遍历字典key,value
		if key == task_id:              #调用python脚本时,命令行输入的第三个参数,task_id=11 
			out = open(outpath, "w+")       #如果key值和task_id(11)相同,则打开人群编码表
			str = ""
			for y in value.split("#"):      #遍历value，以#号分割(添加进字典时,如果key已经存在,添加了#)
				p_id = "null"                
				sa_id = "null"
				for i in y.split("&"):      
					state = "T"
					if (i.startswith("elec_id")):
						p_id = i.split(":")[1]
						if len(p_id.strip()) == 0:
							state = "F"
					if (i.startswith("area_id")):
						sa_id = i.split(":")[1]
						if len(sa_id.strip()) == 0:
							state = "F"
					if (i.startswith("attrib_one")):
						mctype = i.split(":")[1]
						if len(mctype.strip()) == 0:
							state = "F"
					if state == "T": 
						str = p_id + "," + mctype + "," + sa_id + "\n"      #str="GHBH20181101160734607,4,010005"
						out.write(str)     #在人群编码表中写入数据
	out.close()

if __name__ == '__main__':
    getAttr()                      #调用主函数
```

二:读取文件，按字段排序后，写入文件
```python
# -*- coding:utf-8 -*-
import sys
import io
import sys
from itertools import islice

#reload(sys)
#sys.setdefaultencoding( "utf-8" )

file="E://python//untitled2//python学习//python基本语法//123.txt"
#file=sys.argv[1]
#outfile=sys.argv[2]
outfile="E://python//untitled2//python学习//python基本语法//234.txt"

#inpath= open(file,'r', encoding='UTF-8')
inpath = io.open(file, "r", encoding='utf-8')
before={}
for line in islice(inpath, 0, None):     #一行一行读取输入文件内容
    info = line.split("|",-1)             #python中 | 就是竖线分割 不用加双斜杠
    gongsi=info[0]
    number=int(info[1])       #将字符串转换成int
    before[gongsi]=number

after = dict(sorted(before.items(), key=lambda e: e[1],reverse=True))
#遍历输出字典的
cnt=0
out = open(outfile, "w+")
for key, value in after.items():
    cnt += 1
    if cnt >= 100:
        break
    #print("{}:{}".format(key, value))
    str1=key+":"+str(value)+"\n"
    #print(str1)
    out.write(str1)
out.close()
```