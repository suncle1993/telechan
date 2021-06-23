python调用示例

```
import json

import requests


def call_api(url: str, payload: dict):
    payload = {k: v for k, v in payload.items() if v not in (None, "")}
    body = json.dumps(payload, ensure_ascii=False)
    headers = {"Content-Type": "application/json"}
    data = body.encode()
    response = requests.post(url=url, data=data, headers=headers, timeout=60)
    return response.status_code, response.json()


def send_text_message(send_key: str, text: str):
    """ 发送普通文本 """
    url = "https://suncletelegrambot.vercel.app/api/send"
    payload = {
        "sendkey": send_key,
        "text": text,
    }
    call_api(url, payload)


def send_markdown_message(send_key: str, markdown_text: str):
    """
    发送markdown文本
    仅支持部分 Markdown 语法，任何不兼容以下语法的的提交，都会导致 400 错误。注意不支持图片，注意不支持图片，注意不支持图片：

    *bold \*text*
    _italic \*text_
    __underline__
    ~strikethrough~
    *bold _italic bold ~italic bold strikethrough~ __underline italic bold___ bold*
    [inline URL](http://www.example.com/)
    [inline mention of a user](tg://user?id=123456789)
    `inline fixed-width code`
    ```
    pre-formatted fixed-width code block
    ```
    ```python
    pre-formatted fixed-width code block written in the Python programming language
    ```
    :param send_key: 用户从SuncleBot获取到的sendkey
    :param markdown_text: markdown文本
    :return: (status_code, res_body)
    """
    url = "https://suncletelegrambot.vercel.app/api/send"
    payload = {
        "sendkey": send_key,
        "text": f"Hi, {send_key}",
        "markdown": markdown_text,
    }
    return call_api(url, payload)


if __name__ == "__main__":
    suncle1993_send_key = "xxx"
    # 测试普通文本
    send_text_message(suncle1993_send_key, "I LOVE U")
    # 测试不带图片的markdown文本
    markdown_text = """
**这是加粗的文字**
*这是倾斜的文字*
***这是斜体加粗的文字***
~~这是加删除线的文字~~

[简书](http://jianshu.com)
[百度](http://baidu.com)
    """
    send_markdown_message(suncle1993_send_key, markdown_text)
```
