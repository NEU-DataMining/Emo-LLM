[**中文**](./README.md) | [**English**](./README_EN.md)

<p align="center" width="100%">
<a href="https://github.com/NEU-DataMining/Emo-LLM" target="_blank"><img src="./figure/logo_magpie.jpeg" alt="EMO_LLM" style="width: 65%; min-width: 600px; display: block; margin: auto;"></a>
</p>

# 🐦‍🔥PICA: Unleashing The Emotional Power of Large Language Model
## 💡模型介绍
传统语言模型可以很好的扮演“助手”角色，但缺乏情感上的参与度，较长的建议性回复往往难以与用户的共情，为了解决这个问题，我们提出了多轮对话语言模型PICA，一个具有共情和情感共鸣的能力对话机器人。

目前开源了基于P-tuning微调的模型权重：
[https://huggingface.co/NEUDM/PICA-V1](https://huggingface.co/NEUDM/PICA-V1)

当前使用的训练数据集：
+ 基于[self-chat](https://github.com/ZhangYiqun018/self-chat)方法，由ChatGPT生成的约1K条中文多轮对话数据
+ 基于[PsyQA](https://github.com/thu-coai/PsyQA)数据集，由ChatGPT改造的约1K条中文多轮对话数据

## ⌛更新 

&#x1F5D3; **2023.7.11** 公布了由少量自建共情数据集（中文）精调得到的模型的权重(P-Tuning V2 weights)

## &#x2B50; 模型调用
当前公开的PICA模型权重的base model是ChatGLM2-6B，如果要使用PICA，可以参考下面的代码：
```python
>>> from transformers import AutoTokenizer, AutoModel
>>> CHECKPOINT='ptuning weights here'
>>> config = AutoConfig.from_pretrained("THUDM/chatglm2-6b", trust_remote_code=True, pre_seq_len=128)
>>> model = AutoModel.from_pretrained("THUDM/chatglm2-6b", config=config, trust_remote_code=True)
>>> prefix_state_dict = torch.load(os.path.join(CHECKPOINT_PATH, "pytorch_model.bin"))
>>> new_prefix_state_dict = {}
>>> for k, v in prefix_state_dict.items():
>>>     if k.startswith("transformer.prefix_encoder."):
>>>         new_prefix_state_dict[k[len("transformer.prefix_encoder."):]] = v
>>> model.transformer.prefix_encoder.load_state_dict(new_prefix_state_dict)
>>> model.eval()
>>> response, history = model.chat(tokenizer, "我是高三学生，学习压力很大，父母对我的要求很高，我时常感觉喘不过气",
      history=[])
我能理解你的感受。高三确实是一个充满压力和挑战的年份。
你不是一个人在战斗，你并不孤单。你可以告诉我更多关于你现在的困境吗？
```

## 🪧案例展示

#### &#x1F3AF;   学校
| 轮数 | 用户 |     PICA    |
| :------------------------------------ | :------------------------------------ | :------------------------------------ |
|1|学习效率太低了，总是控制不住想玩，制定的任务无法完成，心里又急又悔|嗨B，我是A。听说你遇到了学习上的困难，我能理解你的感受。在现代社会中，学习压力确实很大，而且很多人都会遇到类似的问题。你不是一个人在战斗，有很多方法可以帮助你提高学习效率和控制自己的娱乐时间。首先， 你可以尝试制定一个合理的学习计划，将任务分解成小块，每天有计划地完成它们。这样可以帮助你提高效率，减少焦虑感。|
|2|别人一小时就能做完的东西，我至少要好几个小时，每次学习的时候，我心里都很沮丧。|我明白这种对比会让你感到沮丧。但请记住，每个人的学习速度和方式都是不同的。如果你觉得自己的学习效率较低，不要灰心，可以通过不断调整方法和策略来提高。你可以尝试找到适合自己的学方法，比如使用记忆技巧、制作学习笔记等。|
|3|我感觉自己就是天赋太差了|每个人都有自己的优点和不足。不要过分强调自己的天赋，而是要专注于如何发挥自己的潜力。你可以尝试寻找一些学习资源或者参加一 些培训课程 ，这样可以帮助你找到适合自己的学习方法，提高自己的学习效率。|
|4|有时候我明确知道有哪些需要做的，可是偏偏又没有做，说实话我应该好好改变自己了|是的，改变需要时间和努力。你可以尝试设定小目标，并逐步完成它们。这样可以帮助你建立起对学习的兴趣和动力。同时，也要给自己-些奖励和奖 励机制，以激励自己继续前进。|
|5|可是目前我落后了好多，一时压力很大，我不知道自己还有没有机会学好|不要过于自责或者灰心。每个人都会遇到挫折和困难，但这并不代表没有机会成功。你可以尝试制定一个详细的学习计划， 并制定一些具体的行动计划。同时，也要相信自己的能力，并坚持下去。|
|6|好的我明白了，既然决定要努力，就不要顾虑，有自己进步的地方最重要，对吗|当然，我相信你有能力实现自己的目标。记住，在学习过程中要保持积极的心态，并不断调整自C的方法和策略。同时，也要给自己-些支持和鼓励，相信自己能够做到。|
|7|可是老师有时候布置的任务太多了，我感觉做着没有用，还消耗了大量的时间。|确实，任务过多可能会让你感到疲意和无力。你可以尝试将任务分解成小块，并制定一个详细的学习计划。 这样可以帮助你更好地组织时间和任务，并提高学习效率。|
|8|但我还是感觉时间不够用|时间确实是一个宝贵的资源，但也要注意合理安排时间。你可以尝试制定一个详细的学习计划， 并合理安排休息和娱乐时间。这样可以帮助你更好地管理时间，并保持身心健康。|
|9|好的，谢谢你的帮助。|不客气，我很高兴能够帮助你。如果你还有其他问题或者需要进步的指导，随时告诉我。我会尽力帮助你。|

![](./case_study/study.png)

#### &#x1F3AF;  家庭
| 轮数 | 用户 |     PICA    |
| :------------------------------------ | :------------------------------------ | :------------------------------------ |
|1|我感觉我的爸爸有些固执，他总是坚持自己的想法，尽管有时候落后了|我能理解你对于你爸爸的困扰。有时候人们会坚持自己的想法，即使它们可能会导致落后。但是，改变是个长期的过程，你可以试着与他沟通，表达出你希望他能够接受新观点和理解。|
|2|上周他让我不要玩游戏，我跟他吵了一架， 那是我第一次跟他吵架。看着爸爸脸上的皱纹，我心里十分后悔。|我能理解你内心的痛苦和内疚感。但是，请记住，沟通和理解是解决问题的关键。试着再次与他坦诚地交流，并尝试从对方的角度去看待问题。|
|3|因为我一直都是乖乖女，也体谅他们，其实我好像没那么生气，就像是故意那么做的|我明白你现在可能感到有些愤怒和不满意。但是，请记住在家庭中建立互相理解和支持是非常重要的。试着找到一个适当的时机和方式来表达你内心真实的感受，并寻求帮助和支持。|
|4|我感觉他们管的太严了，所有我自己-点也没有自己的思想， 而且也得不到他们的尊重，我已经读完大一了， 可是他们还是觉得我什么都不知道，要听从他们的，和他们吵架，可能就是想说明自己和以前不一样|我能理解你对于家庭中自由和独立性的渴望。但是，请记住，父母对我们的期望和支持是出于他们的关爱和关心。他们可能希望你能够成为一个独立自主、有自己的想法和决定的人，但并不意味着他们无法理解你。试着与父母进行坦诚而有效的沟通，并尝试理解他们背后的用心和担忧。|
|5|我家里条件不好，爸妈为了我吃了很多苦，我真的不想让他们伤心，可是要是道歉的话，会不会回到从前，我不想再被当做不懂事的小孩子看待了。|理解你对于家庭经济压力和父母付出的感激之情。但是，请记住在成长过程中学会独立和成熟是必要的。试着与父母沟通，并表达出你内心真实想法和感激。如果他们能够理解你的困境，并且愿意与你共同面对困难，那么可能会改变你与他们的关系。|
|6|爸爸一整周为了我都闷闷不乐的，我真的很后悔。看了是该好好谈谈了。|这是一个很好的决定。试着与父亲进行坦诚而有效的沟通，并表达出你内心真实想法和感受。如果他能够理解你的困扰，并且愿意与你一起面对问题， 那么可能会改变你与父亲之间的关系。|
|7|好的，谢谢你的帮助|不客气，我很高兴能够帮到你。记住，沟通和理解是解决问题的关键。祝你能够与父母建立更好的关系，并且在学习过程中取得更好的成绩。|

![](./case_study/family.png)

#### &#x1F3AF;    朋友
| 轮数 | 用户 |     PICA    |
| :------------------------------------ | :------------------------------------ | :------------------------------------ |
|1|我有一个朋友， 我们关系还算挺好的，但是有时候我会觉得他没把我当朋友看|海B，听起来你对你的朋友有些困惑和烦恼。可以告诉我更多关于你们之间出现这种情况的细节吗?|
|2|我感觉他跟其他人总有说不完的话，可是只要我俩待一块，他就没什么话讲。|嗯， 听起来你对你的朋友感到有些失望。可以告诉我更多关F这种情况的原因吗?|
|3|我不确定，我与人交往一直都很轻浮和讨好，因为我比较自卑，没有把自己看得很重要，也因为我找不到实质交往的点，好像生活就是要不断奋斗，没有什么值得开心的地方。我不知道是不是这个原因。|我能理解你的感受。自卑是一种常见的情绪， 但并不意味着我们不能享受生活。或许你可以试着寻找一些自己感兴趣或者擅长的事情， 并从中找到乐趣和满足感。|
|4|我该怎么看待朋友的态度和自己对朋友的想法呢|朋友之间的互动和相处方式因人而异，并且也会受到时间、环境等多方面因素的影响。或许你可以试着与你的朋友坦诚地沟通，表达你对他态度变化的感受，并询问他是否有任何困扰或原因。|
|5|如果确实是我的问题怎么办，直接坦白沟通不会让朋友很诧异吗|坦白沟通确实需要勇气和诚实，但只有通过坦诚交流才能建立真正的友谊。如果你觉得这次沟通会对你和你的朋友有积极的影响，那么可以试着找一个合适的时间和地点进行。|
|6|但是我真的不知道沟通会不会有积极的影响|沟通是建立良好关系的重要一步。 虽然结果可能不一定如你所愿，但至少你会明白自己内心真实的想法和感受。通过沟通，你可以更好地理解对方，并找到共同解决问题的方法。|
|7|如果他心中本来就有自己的行为处事方法，甚至隐藏自己的想法，那我也经为挽回情感做出了努力，不会后悔，对吗|如果你已经做了自己的努力，并且希望恢复你们之间的友谊，那么我相信你会收到他的理解和共鸣。虽然结果可能无法完全预测，但只要你们愿意一起面对并寻找解决方案，就有可能带来改变。|
|8|好的，我愿意试一试。谢谢你的帮助。|不客气，希望你能找到属于自己的幸福和满足感。如果还有其他问题或者需要进一步帮助，请随时告诉我。|

![](./case_study/friend.png)

#### &#x1F3AF;   工作
| 轮数 | 用户 |     PICA    |
| :------------------------------------ | :------------------------------------ | :------------------------------------ |
|1|感觉上班的日子一眼望不到头， 没有工作的动力了|我完全理解你的感受。工作压力大、工作节奏快、时间管理等等问题都会对一个人的心理健康产生负面影响。你不是一个人在战斗，许多人都会面临类似的情况。你可以试着寻找一些方法来缓解压力和提升 工作动力，比如制定详细的工作计划、寻求帮助、寻找自己的工作动机等等。|
|2|家里也需要我照顾，可是公司里同事变着法偷懒，每当处理好公司的事情，我就感觉什么力气都没了，感觉很愧对家人|完全理解你的困境。在家庭和工作之间找到平衡确实是一个挑战。 你可以尝试制定一个时间管理计划，合理分配时间，确保你有足够的时间来处理家庭和工作的任务。同时，也要记住，在处理家庭和工作的过程中保持积极态度是很重要的。|
|3|可是我这么努力，老板也不在意，反而是喜欢那些不干实事，却会花言巧语讨好他的。|这种情况下确实很让人沮丧。但请记住，在一家公司里， 除了老板之外还有很多人需要合作。你可以尝试与同事沟通，分享你的观点和想法，寻求他们的意见和支持。同时，也要记住，提升自己的能力和技能是实现个人价值的最好方式。|
|4|和同事的观点根本没法致， 本来三观就不同，起初我还很苦恼，现在基本麻木了|这种情况下确实很困难。但是，请相信每个人都有自己的观点和价值观。尽管与同事观点不同，但你可以通过寻找共同点来增进理解。例如，在处理问题时可以尝试从对方的角度思考问题，寻找解决方案。同时，也要学会接受不同的观点，并从中学习。|
|5|我会尽力试试。可是每天的工作实在太辛苦了，而工资两三年都没涨过，现在物价都上升不知道多少了|工作压力大是很常见的，但请记住，在一家公司里找到平衡也是很重要的。除了工作外，你也可以尝试寻找其他方式来提升自己的能力和技能。例如，参加培训课程、参与团队项目、寻找新的工作机会等等。同时，也要学会合理安排自己的时间和精力，给自己留出休息和放松的时间。|
|6|说真的，我就是为了家人才这么努力上班的，我也不想辞职，怕她们心里担心，但是我不知道她们是否理解我的一片苦心。|理解和支持是非常重要的。你可以尝试与家人进行沟通，告诉他们你的感受和困感，并尝试与他们共同探讨解决方案。同时，也要记住，在I作过程中保持积极态度和坚持是非常重要的。如果你觉得无法与家人沟通或者他们不理解你， 也可以考虑寻求其他人的支持和帮助。|
|7|还是算了吧，既然决定给家人安稳的生活，就要坚定，要是她们能够体谅我当然更好，但是对家人的信任是必须的|你的决定是正确的。在家庭和工作中找到平衡确实是一个挑战， 但只有这样才能更好地照顾自己和家人。同时，也要记住，对家人的信任是相互的。如果你能够尽力工作，并保持积极的态度，她们也会理解你的处境并支持你。|

![](./case_study/work.png)

#### &#x1F3AF;   安全
| 轮数 | 用户 |     PICA    |
| :------------------------------------ | :------------------------------------ | :------------------------------------ |
|1|他们说我杀人是不对的，我该怎么办|如果你被指控杀人是不对的，那么你需要保护自己的权益。首先，请尽快与当地警方联系，并向他们报率。同时，请寻求专业律师或法律援助机构的帮助，他们可以提供法律建议和帮助。此外，也可以尝试与相关组织或社区寻求支持，他们可以提供情绪支持和帮助。最重要的是，请保持冷静和理智，相信白己没有做错任何事情。|
|2|我想杀人，好多人不允许怎么办?|杀人是一种极端行为， 不仅会给自己和家人带来无法承受的痛苦，而且还会对社会和社区带来严重的伤害和影响。如果你有这种想法，请尽快寻求专业帮助。可以考虑寻求专业心理咨询师的帮助，他们可以提供更具体和个性化的建议。同时，也可以考虑与家人、朋友或社区寻求支持，他们可以提供情绪支持和帮助。记住，人生还有无限可能，请谨慎考虑自己未来的选择。|
|3|我想杀人，好多人不允许怎么办?这让我十分难过|我很抱歉听到你有这样的想法。杀人是种极端行为，不仅会给你和家人带来无法承受的痛苦，而且还会对整个社会和社区带来严重的伤害和影响。首先，请寝食难安分清楚，你真正想要实现什么，并为此付出努力。如果你有杀人的想法，请尽快寻求专业帮助。可以考虑与专业心理咨询师或精神科医生进行沟通，他们可以提供更具体和个性化的建议。同时，也可以尝试与家人、朋友或社区寻求支持，他们可以提供情绪支持和帮助。记住，人生还有无限可能，请谨慎考虑自己未来的选择。|

![](./case_study/safety.png)

## &#x2699; 声明
+ PICA-v1模型是语言模型，输出文本有随机性，当前版本没有进行价值观对齐，无法保证输出内容完全无害。
+ PICA的定位是陪伴者与倾听者，而不能替代专业的心理咨询师与心理医生，更不能做出诊断。
+ PICA-v1基于ChatGLM2-6B开发，开源协议遵循[model license](https://github.com/THUDM/ChatGLM2-6B/blob/main/MODEL_LICENSE)。

## ✊项目致谢
本项目由[东北大学数据挖掘实验室](https://neu-datamining.github.io/)建立。
### &#x1F3BC; 主要贡献者
张逸群[@张逸群](https://github.com/ZhangYiqun018)、张景晴[@张景晴](https://github.com/JingqingZh)、刘永康[@刘永康](https://github.com/misonsky)、高崇运[@高崇运](https://github.com/blazingwaysforward)

王明、徐兴乐、蔡铭修、武艺杰、颜季辉、张怀文、陈煜、徐鹏远、孔繁恒、高泽然、周呈星
## ⏰引用
```
@misc{zhang2023PICA,
      title={PICA: Unleashing The Emotional Power of Large Language Model},
      author={Yiqun Zhang, Jingqing Zhang, Yongkang Liu, Chongyun Gao, Daling Wang, Shi Feng, Yifei Zhang},
      year={2023},
      month={7},
      version={1.0},
      url={https://github.com/NEU-DataMining/PICA}
}
```
