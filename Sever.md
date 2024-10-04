```shell
Https://docs.conda.io/en/latest/miniconda.html

download: Miniconda3-latest-Linux-x86_64.sh

bash Miniconda3-latest-Linux-x86_64.sh
Yes, Enter, Yes
source ~/.bashrc

conda info --envs
conda create --name ChatGLM3
conda activate ChatGLM3

conda config --show
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --remove channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/

conda install python=3.10
conda install pytorch==2.3.0 pytorch-cuda=12.1 -c pytorch -c nvidia
pip install transformers>=4.40.0
pip install numpy==2.0.2

pip install cpm_kernels>=1.0.11 1.0.11
pip install vllm>=0.4.2 0.4.3
pip install gradio>=4.26.0 4.44.0
pip install sentencepiece>=0.2.0 0.2.0
pip install sentence_transformers>=3.0.1 3.0.1

accelerate>=0.29.2 0.34.2
streamlit>=1.33.0 1.38.0
fastapi>=0.110.0 0.114.1
loguru~=0.7.2 0.7.2
mdtex2html>=1.3.0 1.3.0
latex2mathml>=3.77.0 3.77.0
jupyter_client>=8.6.1 8.6.2

sudo apt-get install git-lfs
git init
git lfs install

```



### gym-4-flash

```python
# conda install python=3.10
# pip install zhipuai # 2.1.5.20230904

from zhipuai import ZhipuAI

client = ZhipuAI(api_key="your api key")

response = client.chat.completions.create(
    model="glm-4-flash",
    messages=[
        {
            "role": "system",
            "content": "你是一个乐于解答各种问题的助手，你的任务是为用户提供专业、准确、有见地的建议。" 
        },
        {
            "role": "user",
            "content": "你好"
        }
    ],
    top_p= 0.7,
    temperature= 0.95,
    max_tokens=1024,
    tools = [{"type":"web_search","web_search":{"search_result":True}}],
    stream=True
)

for trunk in response:
    print(trunk)
    
ans = ''
for trunk in response:
    ans += trunk.choices[0].delta.content
print(ans)
```



```
You are a personality analyst, and your job is to summarize the personality of the speaker based on the content of the conversation.
Given this conversation about two speakers:

{speaker_0: I was also a key figure in the company's transition from KL-5 to GR-6 systems.
speaker_1: You must be busy.
speaker_0: I did.
speaker_1: So let me talk about your responsibilities.}.

Please summarize the features of speaker_0. Note: The summary is enough, do not give details of the analysis, the summary can not exceed 500 words.
Example: He is kind, childlike, and loves food and sports. He is loyal to friendship, but sometimes he relies too much on others. He has a great sense of humor and loves to joke around, but he's also prone to being emotional. He has low self-esteem, but his heart desires success. In the face of friendship and love, he shows a strong sense of responsibility, even if he sometimes makes mistakes. Overall, he is a charming and compassionate character.
```







