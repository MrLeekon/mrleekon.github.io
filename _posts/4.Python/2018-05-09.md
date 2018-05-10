# <Python>关于删除Mac中『 ._ 』文件 脚本
```python
import os  
import re  
  
pattern = re.compile('^\._')  
  
for root, dirs, files in os.walk(os.getcwd()):  
    for name in files:  
        if(pattern.match(name)):  
            print 'Delete file: ' + os.path.join(root, name)  
            os.remove(os.path.join(root, name))  
```

