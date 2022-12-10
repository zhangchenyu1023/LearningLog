在Jupyter Notebook中使用新的kernel

如何使用新的kernel？

使用命令conda install ipykernel在maskrcnn环境中安装ipykernel
执行命令python -m ipykernel install --user --name 环境名称 --display-name "你希望看见的环境名"在jupyter notebook中添加需要的环境
比如：python -m ipykernel install --user --name maskrcnn --display-name maskrcnn在jupyter notebook中添加名为maskrcnn的环境
删除kernel环境的命令：jupyter kernelspec remove 环境名称