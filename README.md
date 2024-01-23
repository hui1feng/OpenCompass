# OpenCompass
## OpenCompass 大模型评测
### 1.关于评测的三个问题

* 为什么需要评测：模型选型、能力提升、应用场景效果测评。
* 测什么：知识、推理、语言；长文本、智能体、多轮对话、情感、认知、价值观。
* 怎样测：自动化客观测评、人机交互测评、基于大模型的大模型测评。

大模型评测分为主观评测和客观评测
![1](https://github.com/hui1feng/OpenCompass/assets/126125104/766ed4a0-6353-482a-89a8-e57eefe541bf)


客观评测评测不了时使用主观评测
![2](https://github.com/hui1feng/OpenCompass/assets/126125104/00e2ac9f-d19a-46a0-ad83-f5caf375dd35)


模型是否对提示词敏感
![3](https://github.com/hui1feng/OpenCompass/assets/126125104/fb880da3-7ab7-4d0e-af30-9294b177367b)


OpenCompass测评平台
![4](https://github.com/hui1feng/OpenCompass/assets/126125104/0cf77367-a9ca-4b2e-ad89-a470750047e6)

平台架构
![5](https://github.com/hui1feng/OpenCompass/assets/126125104/a185fe2e-8159-49df-83c5-ff9be746fa91)

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

### 2.动手实践：
面向GPU的环境安装
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
数据准备
```python
# 解压评测数据集到 data/ 处
cp /share/temp/datasets/OpenCompassData-core-20231110.zip /root/opencompass/
unzip OpenCompassData-core-20231110.zip
# 将会在opencompass下看到data文件夹
```
启动客观评测
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
主观测评
需要对另一个模型对结果进行测评，因此需要两个.py文件
