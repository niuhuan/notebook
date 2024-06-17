
## 如何使用pip依赖

```
pip install -r requirements.txt -t ./code/python
用这个命令，把依赖安装到code路径，这样会被打包到代码包里面
然后云上的环境变量加个PYTHONPATH: /opt/python:/code/python:/code
让python可以识别到这个路径的依赖就好
```
