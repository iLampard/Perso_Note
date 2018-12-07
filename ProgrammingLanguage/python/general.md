# General Techniques
  
### Data Analysis  
#### 列表的差

```python
newList = list(set(a).difference(b))
list(set(a) - set(b))
```

### pip

#### 制作并上传pypi安装包
```python
python setup.py sdist path # setup.py 目录下
twine upload dist\\*
```

如果打包成wheel文件的话
```python
python setup.py bdist_wheel
```

#### pip安装包的本地路径

```python
C:\\Users\\xxxxx\\AppData\\Local\\pip
```
  