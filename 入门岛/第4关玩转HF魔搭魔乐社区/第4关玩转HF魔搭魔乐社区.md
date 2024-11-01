# 闯关任务
### 模型下载，使用Hugging Face平台、魔搭社区平台（可选）和魔乐社区平台（可选）下载文档中提到的模型（至少需要下载config.json文件、model.safetensors.index.json文件），请在必要的步骤以及结果当中截图。、、
### 模型上传(可选)
### Space上传(可选)

### 模型下载
#### 1首先进行huggingface平台的注册
简单介绍一下huggingface：
Hugging Face 最初专注于开发聊天机器人服务。尽管他们的聊天机器人项目并未取得预期的成功，但他们在GitHub上开源的Transformers库却意外地在机器学习领域引起了巨大轰动。如今，Hugging Face已经发展成为一个拥有超过100,000个预训练模型和10,000个数据集的平台，被誉为机器学习界的GitHub。
网址为：https://huggingface.co/

#### 2InternLM模型下载
在这里根据官方的文档介绍使用了CodeSpace
在终端中安装如下依赖
```bash
# 安装transformers
pip install transformers==4.38
pip install sentencepiece==0.1.99
pip install einops==0.8.0
pip install protobuf==5.27.2
pip install accelerate==0.33.0
```
#### 3下载internlm2_5-7b-chat的配置文件
先新建一个hf_download_josn.py 文件
```bash
touch hf_download_josn.py
```
在该文件中粘贴如下的代码
```bash
import os
from huggingface_hub import hf_hub_download

# 指定模型标识符
repo_id = "internlm/internlm2_5-7b"

# 指定要下载的文件列表
files_to_download = [
    {"filename": "config.json"},
    {"filename": "model.safetensors.index.json"}
]

# 创建一个目录来存放下载的文件
local_dir = f"{repo_id.split('/')[1]}"
os.makedirs(local_dir, exist_ok=True)

# 遍历文件列表并下载每个文件
for file_info in files_to_download:
    file_path = hf_hub_download(
        repo_id=repo_id,
        filename=file_info["filename"],
        local_dir=local_dir
    )
    print(f"{file_info['filename']} file downloaded to: {file_path}")
```
![image1](codespace.png)

#### 4下载internlm2_5-chat-1_8b并打印示例输出
```bash
touch hf_download_1_8_demo.py
```
代码如下
```bash
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM

tokenizer = AutoTokenizer.from_pretrained("internlm/internlm2_5-1_8b", trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained("internlm/internlm2_5-1_8b", torch_dtype=torch.float16, trust_remote_code=True)
model = model.eval()

inputs = tokenizer(["A beautiful flower"], return_tensors="pt")
gen_kwargs = {
    "max_length": 128,
    "top_p": 0.8,
    "temperature": 0.8,
    "do_sample": True,
    "repetition_penalty": 1.0
}

# 以下内容可选，如果解除注释等待一段时间后可以看到模型输出
# output = model.generate(**inputs, **gen_kwargs)
# output = tokenizer.decode(output[0].tolist(), skip_special_tokens=True)
# print(output)
```
如下图所示为下载成功展示
![image3](xiazai.png)

#### 5Hugging Face Spaces的使用
网址链接为：https://huggingface.co/spaces
![image55](space.png)
在这个页面中创建一个项目名字为intern_cobuild，并选择static应用进行创建
之后再回到codespace进行刚刚创建的项目的clone
```bash
cd /workspaces/codespaces-jupyter
git clone https://huggingface.co/spaces/Jwjyt/intern_cobuild
cd /workspaces/codespaces-jupyter/intern_cobuild
```
找到index.html修改为如下的代码
```bash
<!doctype html>
<html>
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width" />
  <title>My static Space</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
      height: 100%;
    }
    body {
      display: flex;
      justify-content: center;
      align-items: center;
    }
    iframe {
      width: 430px;
      height: 932px;
      border: none;
    }
  </style>
</head>
<body>
  <iframe src="https://colearn.intern-ai.org.cn/cobuild" title="description"></iframe>
</body>
</html>
```
在此之后就可以push到远程仓库上了
```bash
git add .
git commit -m "update: colearn page"
git push
```
但是到了git push这一步会有一个小的报错问题
![image323](gitpush.png)
在这里我们就需要用到HuggingFace的access tokens了
```bash
git remote set-url origin https://<user_name>:<token>@huggingface.co/<repo_path>
例如：
git remote set-url origin https://jack:hf_xxxxx@huggingface.co/spaces/jack/intern_cobuild/
然后再次git push即可
```
![image1212](tt.png)
tip:笔者在这一步一开始获取这个密钥的时候没有给予适当的权限导致即便获得了也没办法成功上传，所以需要赋予可write的权限
之后就可以成功上传了
![image121212](shangchuan.png)
之后再回到spaces的界面就可以看到如下的界面
tip:我这里没登录微信
![image7](success.png)