#### Python文字转语音(调研&成品函数)

由于项目需要, 我需要将文字转换为语音, 那么第一步就要进行调研

##### 什么是语音合成技术?

语音合成（text to speech）,简称TTS。是将文字转化为语音的一种技术，是让计算机模拟人类的嘴巴，通过不同的音色说出想表达的内容, 是人机对话的一部分。
TTS可以通过神经网络的设计，把文字智能地转化为自然语音流。极大的方便了视障患者的使用, 也提升了文本的可读性。TTS应用包括语音驱动的硬件以及声音敏感系统，并常与声音识别程序一起使用。

现在许多厂家都推出了自己的语音合成服务或API, 大家也可以去自行查看, 本文仅做了python环境下语音合成第三方库的调研

##### 如何用代码实现?

如前文所述, 虽然市面上产品繁多, 但是作为一个开发者, 我想要一款免费的, 可代码调试的工具, 经过查找材料, 我找到了gTTs库、pyttsx3库、speech库都能满足我的需求, 来做个横向对比, 可以让大家少走弯路。

|第三方库名称|需要联网 |支持中英文|支持日语|可调节语速|像人声程度|
| ---- | ---- | ---- | ---- | ---- | ---- |
|ggts | √ |√|√|X|很像导航|
|pyttsx3| X |√| X        |√|适合读小说|
|speech| X |√|X|X|很像快一点的导航|

##### gTTS库

gTTS库 (Google Text-to-Speech) : 用于与 Google Translate 的文本转语音 API 进行交互。将语音mp3数据写入文件
优点 : 支持包括中英日文在内的多种语言, 有谷歌翻译API的加持, 人声蛮好听
缺点 : 不支持语速调节, 每次使用必须科学上网, 不能单机使用
在语音播放功能, 我们选用了两种方法,
第一种是playsound库自动播放音频(不可调播放进度)
第二种是os库调用系统自带播放器(可调节进度)

- 请看playsound库播放 & GTTS库转文字函数

```python
# 函数功能: 用gtts库阅读文本,保存为.mp3文件后, 用系统内置的浏览器阅读出来, 打开mp3文件, 函数执行结束(播放方式为os库)
def gtts_os_debug(text,mp3_filepath,language):#参数说明:参数1是朗读的文字,参数2是保存路径,参数3是数字{0英文,1中文,2日语}
    #大成功,可惜的是os调用自带播放器, 实际上只执行了"打开mp3"的操作, 它并不会在音频播报完后再进行下一条语句
    from gtts import gTTS
    import os
    # 已知zh-tw版本违和感较高,所以我们用zh-CN来进行后续工作
    if int(language) ==0 :
        s = gTTS(text=text, lang='en', tld='com')
        # s = gTTS(text=text, lang='en', tld='co.uk')#我比较喜欢美音,但是如果你喜欢英国口音可以尝试这个
    elif int(language) ==1 :
        s = gTTS(text=text, lang='zh-CN')
    elif int(language) ==2 :
        s = gTTS(text=text, lang='ja')
    try:
        s.save(mp3_filepath)
    except:
        os.remove(mp3_filepath)
        print(mp3_filepath,"文件已经存在,但是没有关系!已经删掉了")
        s.save(mp3_filepath)
    print(mp3_filepath,"保存成功")
    os.system(mp3_filepath)#调用系统自带的播放器播放MP3
gtts_os_debug(text="I'm gtts library,from google Artificial Intelligence & Google Translate.",mp3_filepath="gtts英文测试.mp3",language=0)
gtts_os_debug(text="我是gtts库, 你想听听我的声音吗",mp3_filepath="gtts中文测试.mp3",language=1)
gtts_os_debug(text="真実はいつもひとつ" ,mp3_filepath="gtts日语测试.mp3",language=2)

```

- 请看**os库播放 & GTTS库转文字**函数

```python
# 函数功能: 用gtts库阅读文本,保存为.mp3文件后, 用playsound库阅读出来, 阅读完毕, 函数执行结束
def gtts_debug(text,mp3_filepath,language):#参数说明:参数1是朗读的文字,参数2是保存路径,参数3是数字{0英文,1中文,2日语}
    #大成功,已经实现了定制化文字转语音,但是播放的playsound需要改进(playsound库本身可能会出现bug...)
    from gtts import gTTS
    from playsound import playsound
    import os
    if int(language) ==0 :
        s = gTTS(text=text, lang='en', tld='com')
        # s = gTTS(text=text, lang='en', tld='co.uk')#我比较喜欢美音,但是如果你喜欢英国口音可以尝试这个
    elif int(language) ==1 :
        s = gTTS(text=text, lang='zh-CN')
    elif int(language) ==2 :
        s = gTTS(text=text, lang='ja')
    try:
        s.save(mp3_filepath)
    except:
        os.remove(mp3_filepath)
        print(mp3_filepath,"文件已经存在,但是没有关系!已经删掉了")
        s.save(mp3_filepath)
    print(mp3_filepath,"保存成功")
    playsound(mp3_filepath)
gtts_debug(text="I'm gtts library,from google Artificial Intelligence & Google Translate.",mp3_filepath="gtts英文测试.mp3",language=0)
gtts_debug(text="我是gtts库, 你想听听我的声音吗",mp3_filepath="gtts中文测试.mp3",language=1)
gtts_debug(text="真実はいつもひとつ" ,mp3_filepath="gtts日语测试.mp3",language=2)
```

##### pyttsx3库

pyttsx3库 : 是Python中的文本到语音转换库, 它可以脱机工作
优点 : 可以脱机工作, 支持将语音直接朗读, 可调节音量和速度
缺点 : 初始只有英语(女)和中文(女)的语音包, 其他语言的语音包需要另外下载

- 请看pyttsx3库转文字&自朗读函数

```python
def pyttsx3_debug(text,language,rate,volume,filename,sayit=0):
    #参数说明: 六个重要参数,阅读的文字,语言(0-英文/1-中文),语速,音量(0-1),保存的文件名(以.mp3收尾),是否发言(0否1是)
    import pyttsx3
    engine = pyttsx3.init()  # 初始化语音引擎
    engine.setProperty('rate', rate)  # 设置语速
    #速度调试结果:50戏剧化的慢,200正常,350用心听小说,500敷衍了事
    engine.setProperty('volume', volume)  # 设置音量
    voices = engine.getProperty('voices')  # 获取当前语音的详细信息
    if int(language)==0:
        engine.setProperty('voice', voices[0].id)  # 设置第一个语音合成器 #改变索引，改变声音。0中文,1英文(只有这两个选择)
    elif int(language)==1:
        engine.setProperty('voice', voices[1].id)
    if int(sayit)==1:
        engine.say(text)  # pyttsx3->将结果念出来
    elif int(sayit)==0:
        print("那我就不念了哈")
    engine.save_to_file(text, filename) # 保存音频文件
    print(filename,"保存成功")
    engine.runAndWait() # pyttsx3结束语句(必须加)
    engine.stop() # pyttsx3结束语句(必须加)
pyttsx3_debug(text="我是pyttsx3, 初次见面, 给您拜个早年",language=0,rate=200,volume=0.9,filename="ptttsx3中文测试.mp3",sayit=1)
pyttsx3_debug(text="I'm fake Siri, your smart voice Manager",language=1,rate=200,volume=0.9,filename="ptttsx3英文测试.mp3",sayit=1)
```

##### speech库

speech : 基于Windows的语音合成模块, 一行代码即可实现朗读
优点 : 依靠windows系统, 安装使用究极简单 , 超级方便。
适合在代码调试过程中, 让冰冷的AI语言来骂醒写bug的我QAQ
缺点 : 只有系统语言(中文&英文), 不支持语速调节和音频导出

- 请看speech转文字函数

```python
import speech
speech.say("甘霖娘,又出bug了")
speech.say("Don't ask me .I have no idea why bug exist again")
# 如你所见, 代码编译究极简单, 而且单机, 但是!每次使用都会呼出微软语音助手...
```

