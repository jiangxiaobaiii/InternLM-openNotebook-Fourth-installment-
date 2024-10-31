### 闯关任务1：完成Leetcode 383, 笔记中提交代码与leetcode提交通过截图
### 闯关任务2：Vscode连接InternStudio debug笔记

## 1完成leetcode题目383题
题目如下
```bash
给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。
 
如果可以，返回 true ；否则返回 false 。
 
magazine 中的每个字符只能在 ransomNote 中使用一次。
 
 
 
示例 1：
 
输入：ransomNote = "a", magazine = "b"
输出：false
示例 2：
 
输入：ransomNote = "aa", magazine = "ab"
输出：false
示例 3：
 
输入：ransomNote = "aa", magazine = "aab"
输出：true
 
 
提示：
 
1 <= ransomNote.length, magazine.length <= 105
ransomNote 和 magazine 由小写英文字母组成

```
提交完成结果如下图所示
![image111](https://github.com/jiangxiaobaiii/InternLM-openNotebook-Fourth-installmentnt/blob/main/%E5%85%A5%E9%97%A8%E5%B2%9B/%E7%AC%AC2%E5%85%B3Python%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/%E5%8A%9B%E6%89%A3%E9%A2%98%E7%9B%AE.png?raw=true)


## 2完成Vscode连接InternStudio debug笔记
### 2.1首先尝试了debug例子如下
![image22](https://github.com/jiangxiaobaiii/InternLM-openNotebook-Fourth-installmentnt/blob/main/%E5%85%A5%E9%97%A8%E5%B2%9B/%E7%AC%AC2%E5%85%B3Python%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/debug.png?raw=true)

经过一步一步debug之后的结果如下
![image33](https://github.com/jiangxiaobaiii/InternLM-openNotebook-Fourth-installmentnt/blob/main/%E5%85%A5%E9%97%A8%E5%B2%9B/%E7%AC%AC2%E5%85%B3Python%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/debug%E7%BB%93%E6%9E%9C.png?raw=true)

### 2.2之后在进行对如下调用书生浦语API实现将非结构化文本转化成结构化json的例子
官方提供的代码如下
```bash
from openai import OpenAI
import json
def internlm_gen(prompt,client):
    '''
    LLM生成函数
    Param prompt: prompt string
    Param client: OpenAI client 
    '''
    response = client.chat.completions.create(
        model="internlm2.5-latest",
        messages=[
            {"role": "user", "content": prompt},
      ],
        stream=False
    )
    return response.choices[0].message.content

api_key = ''
client = OpenAI(base_url="https://internlm-chat.intern-ai.org.cn/puyu/api/v1/",api_key=api_key)

content = """
书生浦语InternLM2.5是上海人工智能实验室于2024年7月推出的新一代大语言模型，提供1.8B、7B和20B三种参数版本，以适应不同需求。
该模型在复杂场景下的推理能力得到全面增强，支持1M超长上下文，能自主进行互联网搜索并整合信息。
"""
prompt = f"""
请帮我从以下``内的这段模型介绍文字中提取关于该模型的信息，要求包含模型名字、开发机构、提供参数版本、上下文长度四个内容，以json格式返回。
`{content}`
"""
res = internlm_gen(prompt,client)
res_json = json.loads(res)
print(res_json)
```

在尝试运行的时候发现程序报错于是进行断电debug
![image331](https://github.com/jiangxiaobaiii/InternLM-openNotebook-Fourth-installmentnt/blob/main/%E5%85%A5%E9%97%A8%E5%B2%9B/%E7%AC%AC2%E5%85%B3Python%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/%E6%96%AD%E7%82%B9%E6%B5%8B%E8%AF%95%E7%BB%93%E6%9E%9C.png?raw=true)
如上图所示报错在第30行处
JSONDecodeError 是因为 res 的内容不符合 JSON 格式
于是进行代码的调整与修改最终结果如下所示
![image4545](https://github.com/jiangxiaobaiii/InternLM-openNotebook-Fourth-installmentnt/blob/main/%E5%85%A5%E9%97%A8%E5%B2%9B/%E7%AC%AC2%E5%85%B3Python%E5%9F%BA%E7%A1%80%E7%9F%A5%E8%AF%86/%E8%BE%93%E5%87%BA%E7%BB%93%E6%9E%9C%E5%A6%82%E4%B8%8B.png?raw=true)
