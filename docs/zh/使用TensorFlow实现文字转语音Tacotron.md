## 使用 TensorFlow 实现文字转语音 Tacotron

```
https://www.cnblogs.com/com3/articles/13266909.html
```

Tacotron 是完全端到端的文本到语音合成模型，主要是将文本转化为语音，使用了预训练模型(pre-trained)技术。

Tacotron 可利用文本生成类似真人的语音，建议安装 Python 3 版本。

### 使用 pre-trained 模型

#### 下载和解压模型

```
curl http://data.keithito.com/data/speech/tacotron-20170720.tar.bz2 | tar xjC /tmp
```

 

#### 运行 demo server

```
python3 demo_server.py --checkpoint /tmp/tacotron-20170720/model.ckpt
```

 

访问 localhost:9000

输入你想要合成的东西

 ===============================

**自然语言处理 NLP** ，人机交互的语言分析

现如今，人工智能已经成为大众耳熟能详的词汇，而自然语言处理却很少有人了解。自然语言处理(Natural Language Processing，NLP)属于人工智能的一个子领域，是指用计算机对自然语言的形、音、义等信息进行处理，即对字、词、句、篇章的输入、输出、识别、分析、理解、生成等的操作和加工。它对计算机和人类的交互方式有许多重要的影响。

人类语言经过数千年的发展，已经成为一种微妙的交流形式，承载着丰富的信息，这些信息往往超越语言本身。自然语言处理将成为填补人类通信与数字数据鸿沟的一项重要技术。下面就介绍一下自然语言处理的几个常见应用：

##### 　1、机器翻译

随着通信技术与互联网技术的飞速发展、信息的急剧增加以及国际联系愈加紧密，让世界上所有人都能跨越语言障碍获取信息的挑战已经超出了人类翻译的能力范围。

机器翻译因其效率高、成本低满足了全球各国多语言信息快速翻译的需求。机器翻译属于自然语言信息处理的一个分支，能够将一种自然语言自动生成另一种自然语言又无需人类帮助的计算机系统。目前，谷歌翻译、百度翻译、搜狗翻译等人工智能行业巨头推出的翻译平台逐渐凭借其翻译过程的高效性和准确性占据了翻译行业的主导地位。

##### 　2、打击垃圾邮件

当前，垃圾邮件过滤器已成为抵御垃圾邮件问题的第一道防线。不过，有许多人在使用电子邮件时遇到过这些问题：不需要的电子邮件仍然被接收，或者重要的电子邮件被过滤掉。事实上，判断一封邮件是否是垃圾邮件，首先用到的方法是“关键词过滤”，如果邮件存在常见的垃圾邮件关键词，就判定为垃圾邮件。但这种方法效果很不理想，一是正常邮件中也可能有这些关键词，非常容易误判，二是将关键词进行变形，就很容易规避关键词过滤。

自然语言处理通过分析邮件中的文本内容，能够相对准确地判断邮件是否为垃圾邮件。目前，贝叶斯(Bayesian)垃圾邮件过滤是备受关注的技术之一，它通过学习大量的垃圾邮件和非垃圾邮件，收集邮件中的特征词生成垃圾词库和非垃圾词库，然后根据这些词库的统计频数计算邮件属于垃圾邮件的概率，以此来进行判定。

##### 　3、信息提取

金融市场中的许多重要决策正日益脱离人类的监督和控制。算法交易正变得越来越流行，这是一种完全由技术控制的金融投资形式。但是，这些财务决策中的许多都受到新闻的影响。因此，自然语言处理的一个主要任务是获取这些明文公告，并以一种可被纳入算法交易决策的格式提取相关信息。例如，公司之间合并的消息可能会对交易决策产生重大影响，将合并细节(包括参与者、收购价格)纳入到交易算法中，这或将带来数百万美元的利润影响。

##### 　4、文本情感分析

在数字时代，信息过载是一个真实的现象，我们获取知识和信息的能力已经远远超过了我们理解它的能力。并且，这一趋势丝毫没有放缓的迹象，因此总结文档和信息含义的能力变得越来越重要。情感分析作为一种常见的自然语言处理方法的应用，可以让我们能够从大量数据中识别和吸收相关信息，而且还可以理解更深层次的含义。比如，企业分析消费者对产品的反馈信息，或者检测在线评论中的差评信息等。

##### 5、自动问答

随着互联网的快速发展，网络信息量不断增加，人们需要获取更加精确的信息。传统的搜索引擎技术已经不能满足人们越来越高的需求，而自动问答技术成为了解决这一问题的有效手段。自动问答是指利用计算机自动回答用户所提出的问题以满足用户知识需求的任务，在回答用户问题时，首先要正确理解用户所提出的问题，抽取其中关键的信息，在已有的语料库或者知识库中进行检索、匹配，将获取的答案反馈给用户。

##### 6、个性化推荐

自然语言处理可以依据大数据和历史行为记录，学习出用户的兴趣爱好，预测出用户对给定物品的评分或偏好，实现对用户意图的精准理解，同时对语言进行匹配计算，实现精准匹配。例如，在新闻服务领域，通过用户阅读的内容、时长、评论等偏好，以及社交网络甚至是所使用的移动设备型号等，综合分析用户所关注的信息源及核心词汇，进行专业的细化分析，从而进行新闻推送，实现新闻的个人定制服务，最终提升用户粘性。

##### 写在最后：

自然语言处理的目标是弥补人类交流(自然语言)与计算机理解(机器语言)之间的差距，最终实现计算机在理解自然语言上像人类一样智能。未来，自然语言处理的发展将使人工智能可以逐渐面对更加复杂的情况、解决更多的问题，也必将为我们带来一个更加智能化的时代。

========================================

本文简要介绍Python自然语言处理(NLP)，使用Python的NLTK库。NLTK是Python的自然语言处理工具包，在NLP领域中，最常使用的一个Python库。

#### 什么是NLP？

简单来说，自然语言处理(NLP)就是开发能够理解人类语言的应用程序或服务。

这里讨论一些自然语言处理(NLP)的实际应用例子，如语音识别、语音翻译、理解完整的句子、理解匹配词的同义词，以及生成语法正确完整句子和段落。

这并不是NLP能做的所有事情。

#### NLP实现

搜索引擎: 比如谷歌，Yahoo等。谷歌搜索引擎知道你是一个技术人员，所以它显示与技术相关的结果；

社交网站推送:比如Facebook News Feed。如果News Feed算法知道你的兴趣是自然语言处理，就会显示相关的广告和帖子。

语音引擎:比如Apple的Siri。

垃圾邮件过滤:如谷歌垃圾邮件过滤器。和普通垃圾邮件过滤不同，它通过了解邮件内容里面的的深层意义，来判断是不是垃圾邮件。

##### NLP库

下面是一些开源的自然语言处理库(NLP)：

Natural language toolkit (NLTK);
Apache OpenNLP;
Stanford NLP suite;
Gate NLP library

其中自然语言工具包(NLTK)是最受欢迎的自然语言处理库(NLP)，它是用Python编写的，而且背后有非常强大的社区支持。

NLTK也很容易上手，实际上，它是最简单的自然语言处理(NLP)库。

在这个NLP教程中，我们将使用Python NLTK库。

安装 NLTK

如果您使用的是Windows/Linux/Mac，您可以使用pip安装NLTK:

`pip install nltk`

打开python终端导入NLTK检查NLTK是否正确安装：

`import nltk`

如果一切顺利，这意味着您已经成功地安装了NLTK库。首次安装了NLTK，

需要通过运行以下代码来安装NLTK扩展包:

```
import nltk

nltk.download()
```
这将弹出NLTK 下载窗口来选择需要安装哪些包:

您可以安装所有的包，因为它们的大小都很小，所以没有什么问题。

使用Python Tokenize文本

首先，我们将抓取一个web页面内容，然后分析文本了解页面的内容。

我们将使用urllib模块来抓取web页面:
```
import urllib.request

response = urllib.request.urlopen('http://php.net/')
html = response.read()
print (html)
```
从打印结果中可以看到，结果包含许多需要清理的HTML标签。

然后BeautifulSoup模块来清洗这样的文字:
```
from bs4 import BeautifulSoup

import urllib.request
response = urllib.request.urlopen('http://php.net/')
html = response.read()
soup = BeautifulSoup(html,"html5lib")
```
这需要安装html5lib模块
```
text = soup.get_text(strip=True)
print (text)
```
现在我们从抓取的网页中得到了一个干净的文本。

下一步，将文本转换为tokens,像这样:
```
from bs4 import BeautifulSoup
import urllib.request

response = urllib.request.urlopen('http://php.net/')
html = response.read()
soup = BeautifulSoup(html,"html5lib")
text = soup.get_text(strip=True)
tokens = text.split()
print (tokens)
```
##### 统计词频

text已经处理完毕了，现在使用Python NLTK统计token的频率分布。

可以通过调用NLTK中的FreqDist()方法实现:
```
from bs4 import BeautifulSoup
import urllib.request
import nltk

response = urllib.request.urlopen('http://php.net/')
html = response.read()
soup = BeautifulSoup(html,"html5lib")
text = soup.get_text(strip=True)
tokens = text.split()
freq = nltk.FreqDist(tokens)
for key,val in freq.items():
print (str(key) + ':' + str(val))
```
如果搜索输出结果，可以发现最常见的token是PHP。

您可以调用plot函数做出频率分布图:

```
freq.plot(20, cumulative=False)
```

需要安装matplotlib库

这上面这些单词。比如of,a,an等等，这些词都属于停用词。

一般来说，停用词应该删除，防止它们影响分析结果。

##### 处理停用词

NLTK自带了许多种语言的停用词列表，如果你获取英文停用词:
```
from nltk.corpus import stopwords

stopwords.words('english')
```
现在，修改下代码,在绘图之前清除一些无效的token:
```
clean_tokens = list()
sr = stopwords.words('english')
for token in tokens:
if token not in sr:
clean_tokens.append(token)
```
最终的代码应该是这样的:
```
from bs4 import BeautifulSoup
import urllib.request
import nltk
from nltk.corpus import stopwords

response = urllib.request.urlopen('http://php.net/')
html = response.read()
soup = BeautifulSoup(html,"html5lib")
text = soup.get_text(strip=True)
tokens = text.split()
clean_tokens = list()
sr = stopwords.words('english')
for token in tokens:
if not token in sr:
clean_tokens.append(token)
freq = nltk.FreqDist(clean_tokens)
for key,val in freq.items():
print (str(key) + ':' + str(val))
```
现在再做一次词频统计图，效果会比之前好些，因为剔除了停用词:
```
freq.plot(20,cumulative=False)
```



##### 使用NLTK Tokenize文本

在之前我们用split方法将文本分割成tokens，现在我们使用NLTK来Tokenize文本。

文本没有Tokenize之前是无法处理的，所以对文本进行Tokenize非常重要的。token化过程意味着将大的部件分割为小部件。

你可以将段落tokenize成句子，将句子tokenize成单个词，NLTK分别提供了句子tokenizer和单词tokenizer。

假如有这样这段文本:
```
Hello Adam, how are you? I hope everything is going well. Today is a good day, see you dude
```
使用句子tokenizer将文本tokenize成句子:
```
from nltk.tokenize import sent_tokenize

mytext = "Hello Adam, how are you? I hope everything is going well. Today is a good day, see you dude."
print(sent_tokenize(mytext))
```
输出如下:
```
['Hello Adam, how are you?', 'I hope everything is going well.', 'Today is a good day, see you dude.']
```
这是你可能会想，这也太简单了，不需要使用NLTK的tokenizer都可以，直接使用正则表达式来拆分句子就行，因为每个句子都有标点和空格。

那么再来看下面的文本:
```
Hello Mr. Adam, how are you? I hope everything is going well. Today is a good day, see you dude.
```
这样如果使用标点符号拆分,Hello Mr将会被认为是一个句子，如果使用NLTK:
```
from nltk.tokenize import sent_tokenize

mytext = "Hello Mr. Adam, how are you? I hope everything is going well. Today is a good day, see you dude."
print(sent_tokenize(mytext))
```
输出如下:
```
['Hello Mr. Adam, how are you?', 'I hope everything is going well.', 'Today is a good day, see you dude.']
```
这才是正确的拆分。

接下来试试单词tokenizer:
```
from nltk.tokenize import word_tokenize

mytext = "Hello Mr. Adam, how are you? I hope everything is going well. Today is a good day, see you dude."
print(word_tokenize(mytext))
```
输出如下:
```
['Hello', 'Mr.', 'Adam', ',', 'how', 'are', 'you', '?', 'I', 'hope', 'everything', 'is', 'going', 'well', '.', 'Today', 'is', 'a', 'good', 'day', ',', 'see', 'you', 'dude', '.']
```
Mr.这个词也没有被分开。NLTK使用的是punkt模块的PunktSentenceTokenizer，它是NLTK.tokenize的一部分。而且这个tokenizer经过训练，可以适用于多种语言。

非英文Tokenize

Tokenize时可以指定语言:
```
from nltk.tokenize import sent_tokenize

mytext = "Bonjour M. Adam, comment allez-vous? J'espère que tout va bien. Aujourd'hui est un bon jour."
print(sent_tokenize(mytext,"french"))
```
输出结果如下:
```
['Bonjour M. Adam, comment allez-vous?', "J'espère que tout va bien.", "Aujourd'hui est un bon jour."]
```
##### 同义词处理

使用nltk.download()安装界面，其中一个包是WordNet。

WordNet是一个为自然语言处理而建立的数据库。它包括一些同义词组和一些简短的定义。

您可以这样获取某个给定单词的定义和示例:
```
from nltk.corpus import wordnet

syn = wordnet.synsets("pain")
print(syn[0].definition())
print(syn[0].examples())
```
输出结果是:
```
a symptom of some physical hurt or disorder
['the patient developed severe pain and distension']
```
WordNet包含了很多定义：
```
from nltk.corpus import wordnet

syn = wordnet.synsets("NLP")
print(syn[0].definition())
syn = wordnet.synsets("Python")
print(syn[0].definition())
```
结果如下:
```
the branch of information science that deals with natural language information
large Old World boas
```
可以像这样使用WordNet来获取同义词:
```
from nltk.corpus import wordnet

synonyms = []
for syn in wordnet.synsets('Computer'):
for lemma in syn.lemmas():
synonyms.append(lemma.name())
print(synonyms)
```
输出:
```
['computer', 'computing_machine', 'computing_device', 'data_processor', 'electronic_computer', 'information_processing_system', 'calculator', 'reckoner', 'figurer', 'estimator', 'computer']
```
##### 反义词处理

也可以用同样的方法得到反义词：
```
from nltk.corpus import wordnet

antonyms = []
for syn in wordnet.synsets("small"):
for l in syn.lemmas():
if l.antonyms():
antonyms.append(l.antonyms()[0].name())
print(antonyms)
```
输出:
```
['large', 'big', 'big']
```
词干提取

语言形态学和信息检索里，词干提取是去除词缀得到词根的过程，例如working的词干为work。

搜索引擎在索引页面时就会使用这种技术，所以很多人为相同的单词写出不同的版本。

有很多种算法可以避免这种情况，最常见的是波特词干算法。NLTK有一个名为PorterStemmer的类，就是这个算法的实现:
```
from nltk.stem import PorterStemmer

stemmer = PorterStemmer()
print(stemmer.stem('working'))
print(stemmer.stem('worked'))
```
输出结果是:
```
work
work
```
还有其他的一些词干提取算法，比如 Lancaster词干算法。

非英文词干提取

除了英文之外，SnowballStemmer还支持13种语言。

支持的语言:
```
from nltk.stem import SnowballStemmer

print(SnowballStemmer.languages)

'danish', 'dutch', 'english', 'finnish', 'french', 'german', 'hungarian', 'italian', 'norwegian', 'porter', 'portuguese', 'romanian', 'russian', 'spanish', 'swedish'
```
你可以使用SnowballStemmer类的stem函数来提取像这样的非英文单词：
```
from nltk.stem import SnowballStemmer

french_stemmer = SnowballStemmer('french')

print(french_stemmer.stem("French word"))
```
##### 单词变体还原

单词变体还原类似于词干，但不同的是，变体还原的结果是一个真实的单词。不同于词干，当你试图提取某些词时，它会产生类似的词:
```
from nltk.stem import PorterStemmer

stemmer = PorterStemmer()

print(stemmer.stem('increases'))
```
结果:
```
increas
```
现在，如果用NLTK的WordNet来对同一个单词进行变体还原，才是正确的结果:
```
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()

print(lemmatizer.lemmatize('increases'))
```
结果:
```
increase
```
结果可能会是一个同义词或同一个意思的不同单词。

有时候将一个单词做变体还原时，总是得到相同的词。

这是因为语言的默认部分是名词。要得到动词，可以这样指定：
```
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()

print(lemmatizer.lemmatize('playing', pos="v"))
```
结果:
```
play
```
实际上，这也是一种很好的文本压缩方式，最终得到文本只有原先的50%到60%。

结果还可以是动词(v)、名词(n)、形容词(a)或副词(r)：
```
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()
print(lemmatizer.lemmatize('playing', pos="v"))
print(lemmatizer.lemmatize('playing', pos="n"))
print(lemmatizer.lemmatize('playing', pos="a"))
print(lemmatizer.lemmatize('playing', pos="r"))
```
输出:
```
play
playing
playing
playing
```
词干和变体的区别

通过下面例子来观察:
```
from nltk.stem import WordNetLemmatizer
from nltk.stem import PorterStemmer

stemmer = PorterStemmer()
lemmatizer = WordNetLemmatizer()
print(stemmer.stem('stones'))
print(stemmer.stem('speaking'))
print(stemmer.stem('bedroom'))
print(stemmer.stem('jokes'))
print(stemmer.stem('lisa'))
print(stemmer.stem('purple'))
print('----------------------')
print(lemmatizer.lemmatize('stones'))
print(lemmatizer.lemmatize('speaking'))
print(lemmatizer.lemmatize('bedroom'))
print(lemmatizer.lemmatize('jokes'))
print(lemmatizer.lemmatize('lisa'))
print(lemmatizer.lemmatize('purple'))
```
输出:
```
stone
speak
bedroom
joke
lisa

purpl
stone
speaking
bedroom
joke
lisa
purple
```
词干提取不会考虑语境，这也是为什么词干提取比变体还原快且准确度低的原因。

个人认为，变体还原比词干提取更好。单词变体还原返回一个真实的单词，即使它不是同一个单词，也是同义词，但至少它是一个真实存在的单词。

如果你只关心速度，不在意准确度，这时你可以选用词干提取。