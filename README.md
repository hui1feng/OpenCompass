# OpenCompass
## OpenCompass 大模型评测

### 关于评测的三个问题

* 为什么需要评测：模型选型、能力提升、应用场景效果测评。
* 测什么：知识、推理、语言；长文本、智能体、多轮对话、情感、认知、价值观。
* 怎样测：自动化客观测评、人机交互测评、基于大模型的大模型测评。

### 评测对象：
* 基座模型：一般是经过海量的文本数据以自监督学习的方式进行训练获得的模型（如OpenAI的GPT-3，Meta的LLaMA），往往具有强大的文字续写能力。

* 对话模型：一般是在的基座模型的基础上，经过指令微调或人类偏好对齐获得的模型（如OpenAI的ChatGPT、上海人工智能实验室的书生·浦语），能理解人类指令，具有较强的对话能力。

### 评测方法：
OpenCompass采取客观评测与主观评测相结合的方法。针对具有确定性答案的能力维度和场景，通过构造丰富完善的评测集，对模型能力进行综合评价。针对体现模型能力的开放式或半开放式的问题、模型安全问题等，采用主客观相结合的评测方式。

大模型评测分为主观评测和客观评测
#### 客观评测
针对具有标准答案的客观问题，我们可以我们可以通过使用定量指标比较模型的输出与标准答案的差异，并根据结果衡量模型的性能。同时，由于大语言模型输出自由度较高，在评测阶段，我们需要对其输入和输出作一定的规范和设计，尽可能减少噪声输出在评测阶段的影响，才能对模型的能力有更加完整和客观的评价。

为了更好地激发出模型在题目测试领域的能力，并引导模型按照一定的模板输出答案，OpenCompass采用提示词工程 （prompt engineering）和语境学习（in-context learning）进行客观评测。

在客观评测的具体实践中，我们通常采用下列两种方式进行模型输出结果的评测：
* 判别式评测：该评测方式基于将问题与候选答案组合在一起，计算模型在所有组合上的困惑度（perplexity），并选择困惑度最小的答案作为模型的最终输出。例如，若模型在 问题? 答案1 上的困惑度为 0.1，在 问题? 答案2 上的困惑度为 0.2，最终我们会选择 答案1 作为模型的输出。

* 生成式评测：该评测方式主要用于生成类任务，如语言翻译、程序生成、逻辑分析题等。具体实践时，使用问题作为模型的原始输入，并留白答案区域待模型进行后续补全。我们通常还需要对其输出进行后处理，以保证输出满足数据集的要求。

![1](https://github.com/hui1feng/OpenCompass/assets/126125104/766ed4a0-6353-482a-89a8-e57eefe541bf)

#### 主观测评
语言表达生动精彩，变化丰富，大量的场景和能力无法凭借客观指标进行评测。针对如模型安全和模型语言能力的评测，以人的主观感受为主的评测更能体现模型的真实能力，并更符合大模型的实际使用场景。

OpenCompass采取的主观评测方案是指借助受试者的主观判断对具有对话能力的大语言模型进行能力评测。在具体实践中，我们提前基于模型的能力维度构建主观测试问题集合，并将不同模型对于同一问题的不同回复展现给受试者，收集受试者基于主观感受的评分。由于主观测试成本高昂，本方案同时也采用使用性能优异的大语言模拟人类进行主观打分。在实际评测中，本文将采用真实人类专家的主观评测与基于模型打分的主观评测相结合的方式开展模型能力评估。

在具体开展主观评测时，OpenComapss采用单模型回复满意度统计和多模型满意度比较两种方式开展具体的评测工作。
客观评测评测不了时使用主观评测
![2](https://github.com/hui1feng/OpenCompass/assets/126125104/00e2ac9f-d19a-46a0-ad83-f5caf375dd35)


模型是否对提示词敏感
![3](https://github.com/hui1feng/OpenCompass/assets/126125104/fb880da3-7ab7-4d0e-af30-9294b177367b)


OpenCompass测评平台
![4](https://github.com/hui1feng/OpenCompass/assets/126125104/0cf77367-a9ca-4b2e-ad89-a470750047e6)

平台架构
![5](https://github.com/hui1feng/OpenCompass/assets/126125104/a185fe2e-8159-49df-83c5-ff9be746fa91)

* 模型层：大模型评测所涉及的主要模型种类，OpenCompass以基座模型和对话模型作为重点评测对象。
* 能力层：OpenCompass从本方案从通用能力和特色能力两个方面来进行评测维度设计。在模型通用能力方面，从语言、知识、理解、推理、安全等多个能力维度进行评测。在特色能力方面，从长文本、代码、工具、知识增强等维度进行评测。
* 方法层：OpenCompass采用客观评测与主观评测两种评测方式。客观评测能便捷地评估模型在具有确定答案（如选择，填空，封闭式问答等）的任务上的能力，主观评测能评估用户对模型回复的真实满意度，OpenCompass采用基于模型辅助的主观评测和基于人类反馈的主观评测两种方式。
* 工具层：OpenCompass提供丰富的功能支持自动化地开展大语言模型的高效评测。包括分布式评测技术，提示词工程，对接评测数据库，评测榜单发布，评测报告生成等诸多功

评测流水线设计
![6](https://github.com/hui1feng/OpenCompass/assets/126125104/6e76a791-c5e7-4d66-a0e8-b65031b0741b)


前沿探索（多模态）
![7](https://github.com/hui1feng/OpenCompass/assets/126125104/c0ed51ea-5ff0-46c8-93e8-be426e077cd3)


前沿探索（法律领域）
![8](https://github.com/hui1feng/OpenCompass/assets/126125104/3eb4ba23-e269-4b19-8585-8ce1779642c3)


前沿探索（医疗领域）

![9](https://github.com/hui1feng/OpenCompass/assets/126125104/523e54f2-88d5-46c8-b63f-cc7c20ee425b)

大模型测评领域的挑战

![10](https://github.com/hui1feng/OpenCompass/assets/126125104/3aad2441-ee53-4afe-8dd7-14f46cf45545)

### 动手实践：
#### 面向GPU的环境安装
```python
conda create --name opencompass --clone=/root/share/conda_envs/internlm-base
source activate opencompass
git clone https://github.com/open-compass/opencompass
cd opencompass
pip install -e .
```
注意：这里会报错
Cloning into 'opencompass'...
fatal: unable to access 'https://github.com/open-compass/opencompass/': Received HTTP code 503 from proxy after CONNEC
使用gitee
```python
 git clone https://gitee.com/open-compass/opencompass.git
```
#### 数据准备
```python
# 解压评测数据集到 data/ 处
cp /share/temp/datasets/OpenCompassData-core-20231110.zip /root/opencompass/
unzip OpenCompassData-core-20231110.zip
# 将会在opencompass下看到data文件夹
```
#### 启动客观评测
确保按照上述步骤正确安装 OpenCompass 并准备好数据集后，可以通过以下命令评测 InternLM-Chat-7B 模型在 C-Eval 数据集上的性能。由于 OpenCompass 默认并行启动评估过程，我们可以在第一次运行时以 --debug 模式启动评估，并检查是否存在问题。在 --debug 模式下，任务将按顺序执行，并实时打印输出。
```python
python run.py --datasets ceval_gen --hf-path /share/temp/model_repos/internlm-chat-7b/ --tokenizer-path /share/temp/model_repos/internlm-chat-7b/ --tokenizer-kwargs padding_side='left' truncation='left' trust_remote_code=True --model-kwargs trust_remote_code=True device_map='auto' --max-seq-len 2048 --max-out-len 16 --batch-size 4 --num-gpus 1 --debug
```
命令解析
```python
--datasets ceval_gen \
--hf-path /share/temp/model_repos/internlm-chat-7b/ \  # HuggingFace 模型路径
--tokenizer-path /share/temp/model_repos/internlm-chat-7b/ \  # HuggingFace tokenizer 路径（如果与模型路径相同，可以省略）
--tokenizer-kwargs padding_side='left' truncation='left' trust_remote_code=True \  # 构建 tokenizer 的参数
--model-kwargs device_map='auto' trust_remote_code=True \  # 构建模型的参数
--max-seq-len 2048 \  # 模型可以接受的最大序列长度
--max-out-len 16 \  # 生成的最大 token 数
--batch-size 4  \  # 批量大小
--num-gpus 1  # 运行模型所需的 GPU 数量
--debug
```
#### 主观测评
需要对另一个模型对结果进行测评，因此需要两个.py文件
