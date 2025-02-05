# 背景介绍

近年来，随着人口老龄化加剧和健康意识提升，医疗体系面临着日益增长的服务压力。在医疗信息化进程中，电子病历的广泛应用为解决这一挑战提供了新的可能。为实现医疗数据的标准化管理和共享，世界卫生组织制定了国际疾病分类标准（International Classification of Diseases，ICD）。该标准将数万种疾病及其组合转化为规范的字母数字编码体系，为跨地区、跨机构的医疗数据交换与分析奠定了基础。

然而，将电子病历文本手动转换为ICD编码不仅耗时耗力，还容易出现人为失误。开发自动化的ICD编码系统，既能提高编码效率和编码一致性，也能为疾病研究和医疗管理提供更可靠的数据支持。基于这一背景，本评测构建了一个专门用于评估中文电子病历ICD自动编码的数据集，该数据集基于脱敏病历数据而构建，共涉及5种主诊断和32种其他诊断ICD（ICD-10）编码，共计1485条数据。

# 一、数据集介绍

评测数据基于医院脱敏病历构建，共1485条数据。数据分为训练集、验证集和测试集，数据量分别为800条、200条和485条。本任务仅公开训练集和验证集数据，测试集数据不公开。评测结果需要参赛队伍提交模型docker镜像由本任务组织单位进行评测。

- 数据集申请

1. 下载《数据使用与保密承诺书》在文档末尾填写参赛队伍信息，下载地址：[点击此处](https://github.com/QLU-NLP/icdevaluator/raw/refs/heads/main/doc/%E6%95%B0%E6%8D%AE%E4%BD%BF%E7%94%A8%E4%B8%8E%E4%BF%9D%E5%AF%86%E6%89%BF%E8%AF%BA%E4%B9%A6.docx)。
2. 参赛队伍负责人签名（手写签名）。
3. 将签名的《数据使用与保密承诺书》扫描件（PDF）发送至以下邮箱icdevaluator@163.com，邮件标题为：“参赛单位-队伍名称-中文电子病历ICD诊断编码评测数据使用申请”。

- 标注数据的字段信息如下：

  - **病案标识**：患者在医院就诊的唯一病案编号。

  - **主诉**：患者在就诊时向医生描述的最主要、最直接的症状，用一句简短的话概括，通常是患者就医的主要原因。

  - **现病史**：对患者当前疾病的发生、发展、演变过程的详细描述，包括起病情况、症状特征、治疗经过和效果等。

  - **既往史**：患者以往的健康状况和疾病史，包括既往的主要疾病、手术、外伤、过敏史等。

  - **个人史**：患者的生活习惯、职业暴露、疫区接触等相关信息。

  - **婚姻史**：婚姻情况、结婚年龄、配偶健康情况、有无子女、性生活情况等。

  - **家族史**：描述患者直系亲属中是否存在遗传性疾病或特定疾病的病史。

  - **入院情况**：描述患者入院时的症状、体征和一般情况。

  - **入院诊断**：患者入院时医生根据现病史和检查得出的初步诊断结果。

  - **诊疗经过**：患者在院期间所接受的检查、治疗和病情变化过程的详细记录。

  - **出院情况**：患者出院时的健康状况和恢复情况的简要描述。

  - **出院医嘱**：医生对患者出院后生活、用药、复查等方面的指导。

  - **主诊断编码**：患者住院期间主要诊断所对应的标准化编码。

  - **其他诊断编码**：患者住院期间其他相关诊断所对应的标准化编码。

- 数据样例

```json
{
      "病案标识": "ZY020000982397",
      "主诉": "胸闷、喘7天。",
      "现病史": "患者于7天前无明显诱因出现胸闷、喘，呈阵发性，活动及情绪激动后明显加重，不能从事日常活动，时有心慌、心前区疼痛不适，无发热、头痛、头痛，无左肩及后背部放射痛，无咳嗽、咳痰，无黑朦及意识障碍，病后至“***”住院治疗，完善相关辅助检查：化验检查（不详），胸部CT：双肺多发纤维灶，双侧胸腔积液并局部胸膜增厚，建议治疗后复查：符合支气管炎并肺气肿、多发性结节灶CT表现：心影饱满，左室体积增大，冠脉钙化，请结合相关检查。",
      "既往史": "有“冠状动脉粥样硬化性心脏病”10余年，2021年于****行“冠状动脉移植术”（具体不详）",
      "个人史": "生长于原籍，否认疫区及地方病流行区长期居住史，生活规律。否认放射性物、工业粉尘、化学性物质接触史，否认冶游史，吸烟史40年，20支/日，已经戒烟1年余，否认饮酒史。",
      "婚姻史": "适龄结婚，育有1子，配偶及孩子身体健康。",
      "家族史": "父母已逝，具体不详。否认家族性遗传病及传染病史。",
      "入院情况": "患者老年男性，76岁，因“胸闷、喘7天”入院。有“冠状动脉粥样硬化性心脏病”10余年，2021年于***行“冠状动脉移植术”（具体不详），术后规律服用“酒石酸美托洛尔、螺内酯、单硝酸异山梨酯、阿司匹林”等药物治疗，平素活动耐量差。“高血压病”10余年，血压最高180/100mmHg，平素口服“沙坦氨氯地平”，血压控制在130-140/80-90mmHg。"，
      "入院诊断": "1.急性失代偿性心力衰竭心功能II级（Killip分级）2.肺炎3.急性呼吸衰竭（I型）",
      "诊疗经过": "入院后完善相关辅助检查，凝血常规：凝血酶原时间：13.1秒，D-二聚体：1.74mg/L；血酮体测定：弱阳性；心梗三联：肌红蛋白：143.15ng/mL；NT-proBNP：8199pg/mL；血气分析：PH值：7.47，二氧化碳分压：26mmHg，氧分压：68mmHg，血二氧化碳总量：19.7mmol/L，细胞外液剩余碱：-4.8mmol/L：尿常规检查加沉渣粒细胞：+-，尿蛋白：+，酮体：2+，尿潜血：+2，红细胞：174/ul；血细胞分析：单核细胞计数：0.83×109/L，血红蛋白：109.0g/L：糖化血红蛋白：6.80%：2024-01-01床边胸片DR：双肺炎症、渗出性改变，请结合临床；双侧胸腔积液。心电图：1.快速型心房纤颤2.ST-T改变3.异常Q波。",
      "出院情况": "双侧瞳孔等大等圆，对光反射及调节反射存在，前胸部可见长约15cm纵行手术切口，左侧斜肋部带状疱疹素色沉着，胸廓对称无畸形，双肺呼吸音粗糙，未闻及湿性啰音。心前区无隆起及凹陷，心界无扩大，节律规则，各瓣膜听诊区未闻及病理性杂音。腹部膨隆，腹软，无压痛，无反跳痛。肝、脾肋下未触及，Murphys征阴性，肝、肾区无叩痛，肠鸣音无亢进，移动性浊音阴性。双下肢无水肿。",
      "出院医嘱": "1、低盐低脂饮食，注意休息，避免劳累，按时服药；2、出院后继续药物治疗：r阿托伐他汀钙片（齐鲁），每次量：20mg，睡前，每次一片单硝酸异山梨酯片（鲁南），每次量：20mg，每天一次，每次一片1托拉塞米片，每次量：5mg，每天一次，每次一片螺内酯片（长江），每次量：20mg，每天一次，每次一片1琥珀酸美托洛尔缓释片（合源），每次量：47.5mg，每天一次，每次一片门冬氨酸钾镁片，每次量：1片，每天两次，每次一片|普瑞巴林胶囊（赛维）。",
      "主诊断编码": "J81.x00x00",
      "其他诊断编码": "I50.907; I50.903; I25.103; I20.000; I49.900; I48.x01;E11.900"。
    }
```

# 二、任务介绍

## 1、任务描述

任务目标：借助自然语言处理技术从电子病历文本中分析患者的临床症状并提取关键症状信息，确定主诊断编码和其他诊断编码，辅助医生或编码员更精准地完成ICD编码。

任务定义：给定一段由自然语言文本书写的患者的详细情况描述（包括主诉、现病史、既往史、出院情况等），模型需要预测出患者的主诊断编码和其他诊断编码。

## 2、任务说明

该任务给定一段由临床信息构成的文本作为输入，需要模型输出对应的主诊断编码和其他诊断编码。

•   输入：诊疗记录中各类临床信息构成的文本，最终拼接成一个str类型字段

•   输出：对应的主诊断编码和其他诊断编码

本数据集候选的全部主诊断编码和其他诊断编码类别分别如下：

   **主诊断编码：**

   [I10.x00x03; I20.000; I20.800x00; I21.401; I50.900x01]

   **其他诊断编码**：

   [E04.101; E11.900; E72.101; E78.500; E87.600; I10.x00x02; I10.x00x03; I20.000; I25.103; I25.200; I38.x01; I48.x01; I48.x02; I49.100x00; I49.300x00; I49.400x00; I49.900; I50.900x00; I50.900x01; I50.907; I63.900; I69.300x00; I70.203; I70.806; J18.900; J98.414; K76.000; K76.807; N28.101; Q24.501; R91.x00x00; Z95.501]

注：输出为列表格式，其中主诊断编码和其他诊断编码由‘|’符号隔开，其他诊断编码之间由‘;’符号隔开，如：[主诊断编码|其他诊断编码1;其他诊断编码2;…]。主诊断编码有且仅有1个，其他诊断编码有一个或多个。

## 3、评测指标

中文电子病历ICD诊断编码任务采用正确率（Acc）作为评测指标，计算公式如下：

![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20Acc%20%3D%20%5Cfrac%7B1%7D%7BN%7D%5Csum_%7Bi%3D1%7D%5EN%5C%7B0.5%5Ccdot%20I%28%5Chat%7By%7D_%7Bmain%7D%20%3D%3D%20y_%7Bmain%7D%29%20%2B%200.5%5Ccdot%20%5Cfrac%7BNUM%28y_%7Bother%7D%20%5Ccap%20%5Chat%7By%7D_%7Bother%7D%29%7D%7BNUM%28y_%7Bother%7D%29%7D%5C%7D_i)

其中，![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20I%28%5Ccdot%29)为指示函数，满足条件返回1，否则返回0；![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20%5Chat%7By%7D_%7Bmain%7D)和![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20y_%7Bmain%7D)分别表示主诊断编码的预测标签和真实标签；![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20NUM%28x%29)代表数量函数，用来计算x的数量，![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20%5Chat%7By%7D_%7Bother%7D)和![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20y_%7Bother%7D)分别表示其他诊断编码的预测标签集和真实标签集；![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20N)为测试样本的数量；![equation](https://latex.codecogs.com/png.latex?%5Cdpi%7B150%7D%20%5Csmall%20%5C%7B%5Ccdot%5C%7D_i)为第i个中文电子病历的预测准确率。

## 4、任务奖项

一等奖1名，奖金合计2500元；

二等奖1名，奖金合计1500元；

三等奖1名，奖金合计1000元。

## 5、赞助情况

本次评测奖金由上海联众网络信息股份有限公司赞助。


# 三、结果提交

本次测评分A榜（验证集）、B榜（测试集）两个榜单。

A榜评测结果采用邮件方式提交。邮件标题为：“CCL2025-中文电子病历ICD诊断编码评测-参赛单位-队伍名称-A榜结果”。测试结果命名为：ICD-Coding-A.json。榜单每日17:59:59在Github中进行更新。具体格式如下方样例：

```json
[{
    "病案标识":ZYxxxxxxx, 
    "预测结果":[主诊断编码|其他诊断编码1;其他诊断编码2;…]
}, 
{
…
}]

```

B榜评测结果使用docker镜像提交，参赛队伍需要将模型打包成docker镜像，同时提交使用说明。镜像通过百度云盘上传，提交镜像压缩包名称命名：“CCL2025-中文电子病历ICD诊断编码评测-参赛单位-队伍名称.tar”，同时使用邮件提交百度云盘下载链接，邮件标题为：“CCL2025-中文电子病历ICD诊断编码评测-参赛单位-队伍名称-B榜结果”。

注：模型参数不能超过7B。B榜为最终榜单。

# 四、系统排名

该评测任务采用百分制分数显示，小数点后保留2位。

# 五、Baseline

Baseline表现：

| Acc  |
| :--- |
|      |

# 六、评测数据

数据由json格式给出，数据集包含以下内容：

- ICD-Coding-train.json: 训练集标注数据。
- ICD-Coding-test-A.json: A榜测试集（验证集）。
- ICD-Coding-A.json: A榜提交示例
- ICD-Coding-test-B.json: B榜测试集（测试集）。B榜测试集不公开。

# 七、赛程安排

1. 本次大赛分为报名组队、A榜、B榜三个阶段，具体安排和要求如下：

   1. 报名时间：2025年2月10日-5月2日
   2. 训练、验证数据及baseline发布：2025年2月10日
   3. 测试A榜（验证集）数据发布：2025年2月10日
   4. 测试A榜评测截止：2025年5月4日 17:59:59
   5. 测试B榜（测试集）最终结果：2025年5月10日 17:59:59
   6. 公布测试结果：2025年5月15日前
   7. 提交中文或英文技术报告：2025年6月1日
   8. 中文或英文技术报告反馈：2025年6月20日
   9. 正式提交中英文评测论文：2025年7月1日
   10. 公布获奖名单：2025年7月5日
   11. 评测研讨会：2025年8月

   **注意：报名组队与认证（2025年2月10日—5月2日）**

# 八、报名方式

2025年2月10日将开放本次比赛的报名组队，给icdevaluator@163.com邮箱发送个人信息（包括学校、团队名、队长、组员）进行注册，即可报名参赛；选手可以单人参赛，也可以组队参赛。组队参赛的每个团队不超过5人，每位选手只能加入一支队伍；选手需确保报名信息准确有效，组委会有权取消不符合条件队伍的参赛资格及奖励；选手报名、组队变更等操作截止时间为5月2日23：59：59。 

向赛题举办方发送电子邮件进行报名，以获取数据解压密码。邮件标题为：“CCL2025-中文电子病历ICD诊断编码评测-参赛单位”，例如：“CCL2025-中文电子病历ICD诊断编码评测-齐鲁工业大学”；附件内容为队伍的参赛报名表，报名表点此下载，同时报名表应更名为“参赛队名+参赛队长信息+参赛单位名称”。请参加评测的队伍发送邮件至icdevaluator@163.com，报名成功后赛题数据解压密码会通过邮件发送给参赛选手。

# 九、赛事规则

1. 由于版权保护问题，该数据集只免费提供给用户用于非盈利性科学研究使用，参赛人员不得将数据用于任何商业用途。如果用于商业产品，请联系管红娇（hongjiao.guan@qlu.edu.cn）、廉颖（lianying525@sina.com）。
2. 每名参赛选手只能参加一支队伍，一旦发现某选手参加多支队伍，将取消相关队伍的参赛资格。
3. 数据集的具体内容、范围、规模及格式以最终发布的真实数据集为准，验证集不可用于模型训练。
4. 参赛队伍可在参赛期间随时上传测试集的预测结果，A榜阶段每天可提交1次、B榜阶段最多提交2次，本任务组织单位在每日17:59:59更新当前最新榜单排名情况，严禁参赛团队注册其它账号多次提交。
5. 允许使用公开的代码、工具、外部数据（从其他渠道获得的标注数据）等，但需要保证参赛结果可以复现。
6. 参赛队伍可以自行设计和调整模型，但需注意模型参数量最多不超过7B。
7. 算法与系统的知识产权归参赛队伍所有。要求最终结果排名前xx（看最终获奖名额）的队伍提供算法代码与系统报告（包括方法说明、数据处理、参考文献和使用的开源工具、外部数据等信息）。提交完毕将采用随机交叉检查的方法对各个队伍提交的模型进行检验，如果在排行榜上的结果无法复现，将取消获奖资格。
8. 参赛团队需保证提交作品的合规性，若出现下列或其他重大违规的情况，将取消参赛团队的参赛资格和成绩，获奖团队名单依次递补。重大违规情况如下：
         a. 使用小号、串通、剽窃他人代码等涉嫌违规、作弊行为；
         b. 团队提交的材料内容不完整，或提交任何虚假信息；
         c. 参赛团队无法就作品疑义进行足够信服的解释说明；
9. 获奖队伍必须注册会议并在线下参加（如遇特殊情况，可申请线上参加）。
