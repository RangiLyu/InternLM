# Lagnet

[English](lagent.md) | 简体中文

## 简介

[Lagent](https://github.com/InternLM/lagent) 是一个轻量级、开源的基于大语言模型的智能体（agent）框架，支持用户快速地将一个大语言模型转变为多种类型的智能体，并提供了一些典型工具为大语言模型赋能。它的整个框架图如下:

![image](https://github.com/InternLM/lagent/assets/24351120/cefc4145-2ad8-4f80-b88b-97c05d1b9d3e)

本文主要介绍 Lagent 的基本用法。更全面的介绍请参考 Lagent 中提供的 [例子](https://github.com/InternLM/lagent/tree/main/examples)。

## 安装

通过 pip 进行安装 (推荐)。

```bash
pip install lagent
```

同时，如果你想修改这部分的代码，也可以通过以下命令从源码编译 Lagent:

```bash
git clone https://github.com/InternLM/lagent.git
cd lagent
pip install -e .
```

## 运行一个 ReAct 智能体的网页样例

```bash
# 需要确保已经安装 streamlit 包
# pip install streamlit
streamlit run examples/react_web_demo.py
```

然后你就可以在网页端和智能体进行对话了，效果如下图所示

![image](https://github.com/InternLM/lagent/assets/24622904/3aebb8b4-07d1-42a2-9da3-46080c556f68)

## 用 InternLM-Chat 构建一个 ReAct 智能体

**注意：**如果你想要启动一个 HuggingFace 的模型，请先运行 pip install -e .[all]。

```python
# Import necessary modules and classes from the "lagent" library.
from lagent.agents import ReAct
from lagent.actions import ActionExecutor, GoogleSearch, PythonInterpreter
from lagent.llms import HFTransformer

# Initialize the HFTransformer-based Language Model (llm) and provide the model name.
llm = HFTransformer('internlm/internlm-chat-7b-v1_1')

# Initialize the Google Search tool and provide your API key.
search_tool = GoogleSearch(api_key='Your SERPER_API_KEY')

# Initialize the Python Interpreter tool.
python_interpreter = PythonInterpreter()

# Create a chatbot by configuring the ReAct agent.
chatbot = ReAct(
    llm=llm,  # Provide the Language Model instance.
    action_executor=ActionExecutor(
        actions=[search_tool, python_interpreter]  # Specify the actions the chatbot can perform.
    ),
)
# Ask the chatbot a mathematical question in LaTeX format.
response = chatbot.chat('若$z=-1+\sqrt{3}i$,则$\frac{z}{{z\overline{z}-1}}=\left(\ \ \right)$')

# Print the chatbot's response.
print(response.response)  # Output the response generated by the chatbot.
>>> $-\\frac{1}{3}+\\frac{{\\sqrt{3}}}{3}i$
```
