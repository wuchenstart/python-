1. 用户输入
   * input()接收用户输入字符串，python2.7用raw_input()
   * int():数值字符串转换为数值
   * 求模：%
2. while循环
```python
while 条件:
    ...
```
3. 退出
   - (1) 条件测试:
   ```python
   message = ''
   # 指定条件运行程序
   while message != 'quit':
       ...
   ```
   - (2) 标记：
   ```python
   active = True
   # 满足很多条件才运行或不运行，判断整个程序是否处于活动状态
   while active:
       ...
   ```
   - (3) break:
   ```python
   # 满足if条件，立即退出循环，不再运行循环余下代码
   while True:
       if 条件:
           break
   ```
   - (4) continue:结束本次循环
**注：for循环中不应该修改列表，否则python无法跟踪其中元素**
**在遍历列表的同时对其修改，可使用while循环**
4. 使用while循环来处理列表和字典
   - (1) 列表之间移动元素pop()
```python
"""
假设一个列表包含新注册但还未验证的网站用户；验证这些用户后，将此列表中的用户移到另一个已验证用户列表中
一种方法while循环，在验证用户的同时将其从未验证用户列表中提取出来，再将其加入到另一个已验证用户列表中
"""
# 创建一个待验证用户列表和一个用于存储已验证用户的空列表
unconfirmed_users = ['alice', 'brian', 'candace']
confirmed_users = []

# 验证每个用户，直到没有未验证用户为止
# 将每个经过验证的列表都移到已验证用户列表中
while unconfirmed_users:
    current_user = unconfirmed_users.pop()
    print("Verifying user: " + current_user.title())
    confirmed_users.append(current_user)

# 显示所有已验证的用户
print("\nThe following users have been confirmed:")
for confirmed_user in confirmed_users:
    print(confirmed_user.title())
```
   - (2) 删除列表中包含的所有特定值
```python
"""
假设有一个宠物列表，包含多个值为'cat'的元素。
删除所有这些元素，可不断运行一个while循环，直到列表中不再包含值'cat'。
"""
pets = ['dog', 'cat', 'dog', 'goldfish', 'cat', 'rabbit', 'cat']
print(pets)

while 'cat' in pets:
    pets.remove('cat')

print(pets)
```
   - (3) 用户输入填充字典
```python
"""
使用while循环提示用户输入任意数量的信息
"""
responses = {}

# 设置一个标志，指出调查是否继续
polling_active = True
while polling_active:
    # 提示输入被调查者的名字和回答
    prompt_1 = "What is your name?"
    name = input(prompt_1)
    prompt_2 = "Which mountain would you like to climb someday?"
    response = input(prompt_2)

    # 将答案存储在字典中
    responses[name] = response

    # 看看是否还有人要参与调查
    prompt_3 = "Would you like to let another persono respond? (yes/no)"
    repeat = input(prompt_3)

    if repeat == 'no':
        polling_active = False

# 调查结束，显示结果
print("\n--- Poll Results ---")
for name, response in responses.items():
    print(name + " would like to climb " + response + ".")
```
   
