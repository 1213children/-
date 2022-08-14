```PYTHON
user_agent = Faker('zh-CN').user_agent()
data_list_queue = queue.Queue()

def get_header():
    return {
        'User-Agent': user_agent
    }
```