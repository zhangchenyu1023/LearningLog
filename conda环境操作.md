## conda环境操作

查看所有环境：conda info -e

创建虚拟环境：conda create -n xxx python=3.6

激活(或切换不同python版本)的虚拟环境：
							source activate your_env_name(虚拟环境名称)
							Windows: activate your_env_name(虚拟环境名称)

导出当前环境：conda env export > requirement.yaml

导入环境操作：conda env create -f 2.txt  .xml

pip导出安装的库到txt：pip freeze > requirement.txt

pip导入txt中列出的库到新机：pip install -r 1.txt



克隆环境 conda create -n newname -clone oldname
