# Django Techniques

### 安装
最好是创建虚拟环境

### 运行
#### 在局域网发布
- 假设端口为8000
```bash
python manage.py runserver 0.0.0.0:8000 --insecure
```

#### 在PyCharm中快捷启动设置
Run/Debug Configurations
在PyCharm Professional版本中选择‘Django server’然后设置
- Additional options: --insecure

### 语法问题
#### Iterate through dict in django template

参考 [python - how to iterate through dictionary in a dictionary in django template? - Stack Overflow](https://stackoverflow.com/questions/8018973/how-to-iterate-through-dictionary-in-a-dictionary-in-django-template)
  


```python
  def query_alpha_bottom_up(request):
      context = dict()
      return render(request, 
                    "some.html", 
                    {"signal": df_signal.T.to_dict()})
  ```
  
  
  
  ```html
  <table class="pure-table" align="center">
          <thead>
              <tr>
                  {% for header in headers %}
                      <th><p style="text-align: center;">{{ header }}</p></th>
                  {%  endfor %}
              </tr>
          </thead>
  
          <tbody>
          {% for key, value in signal.items %}
          <tr >
              {% for k, v in value.items %}
                  <td>{{v}}</td>
              {% endfor %}
          </tr>
          {% endfor %}
          </tbody>
      </table>
  ```

#### 传递数据给HChart
可参见 [How to Integrate Highcharts.js with Django](https://simpleisbetterthancomplex.com/tutorial/2018/04/03/how-to-integrate-highcharts-js-with-django.html)