
# 本人未能在一行代码中搞定一个8600万美元的项目

>瞎逛medium瞎搞，其实也没什么的。真的...
>原文翻译出处：[How I failed to replicate an $86 million project in 1 line of code](https://medium.com/@ryanfb/how-i-failed-to-replicate-an-86-million-project-in-1-line-of-code-615048a1f9d0)

最近被澳大利亚一个8600万的项目被程序猿几行代码刷屏了；简直就是为了搞事。

相关链接：
- [How I replicated an $86 million project in 57 lines of code](https://medium.freecodecamp.org/how-i-replicated-an-86-million-project-in-57-lines-of-code-277031330ee9)
- [Automatic License Plate Recognition library](https://github.com/openalpr/openalpr)
- [Flock is your virtual security guard](http://www.flocksafety.com/)
- [How I failed to replicate an $86 million project in 1 line of code](https://medium.com/@ryanfb/how-i-failed-to-replicate-an-86-million-project-in-1-line-of-code-615048a1f9d0)
- [Hacker News-How I failed to replicate an $86M project in 1 line of code](https://news.ycombinator.com/item?id=15158811)
- 乱入：[I have a funny story from Finland](https://www.resavr.com/comment/how-replicated-an-86-9466504)
- 乱入：[Inside Dev (Sep 3rd, 2017)](https://inside.com/campaigns/inside-dev-2017-09-03-3154/sections/development-dregs-17344)
- 乱入：[谷歌无人车之父Sebastian Thrun：摄像头才是无人驾驶最好的方式](https://baijia.baidu.com/s?id=1576951828995877033&wfr=pc&fr=app_lst)

上文有翻译：
- [【57行代码搞定8600万美元项目】用开源工具DIY车牌识别系统](https://mp.weixin.qq.com/s/iwPI8g2JwabwiCO8kfw8Hw)
- [57行价值八千万美元的车牌识别代码](https://yq.aliyun.com/articles/199312)

![]()

当你使用现有的开源技术来进行实验时，仅仅只是为了能看到结果的话，那就不错的。

在Medium上的一篇文章“我如何以57行代码复制8600万美元的项目”，已经在过去几天激烈地进行了几轮，描述了为澳大利亚维多利亚警察局开发的自动牌照识别系统（ALPR）系统是否可以使用开放式来源ALPR系统OpenALPR。这基本上是无处不在的套路文章标题“为什么这需要多少多少美金？我可以在一个周末用代码就搞定了！”，对任何具备基础逻辑的（或复杂的）科技推导出的结论和对之评论。

然而，由于OpenALPR是免费且开源的，所以我们可以测试这个说法的合理性。

但我们将OpenALPR跑在本地电脑上的时候就要忽略一些无鬼用的东西，​​直接跳到我们试图从dashcam视频中自动将牌照识别出来。对于这些我们要测试视频，当初我选择了“Drive around Bendigo(围绕本迪戈驾驶)”，这是一个惊人的27分钟的YouTube视频，并且是很有代表性的，因为它的1080p车载镜头是非常明确的，而且也在我们围绕系统部署的区域里。从youtube下载后，我将它加入到OpenALPR中来，并运行了命令行。 `time alpr --clock -n 1 'Drive around Bendigo-hrD75ebjCms.mp4' > bendigo.txt`...

嗯；好好好，问题来了。我在处理这27分钟视频里使用3.5GHz Core i7的电脑配置上花费了超过3.5小时，而且不是实时的。铅笔在几个“优化”，还有一些“每个巡逻车超强电脑硬件”我猜。
我猜，如果要达到最佳的帧数并读取数据比较靠谱的话，得有部配置超腻害的电脑来跑。

![OpenALPR的处理时间，让人很惊讶]()

无论如何，总算出了结果！常言道，加快CPU的周期才能更容易更快地得到我们想要的数据。我们将结果过滤到只有潜在的车牌命名为`bendigo.txt`的文件里；每条数据组中单词，它匹配起来会达到高达6,137个相识的词语中（如果我们从中再过滤掉一些唯一的单词，则大概为1,653）。不错！很好，差不多了。这似乎还是很多啊。让我们来仔细看看先：

```
fgrep confidence bendigo.txt| cut -d' ' -f 6 | sort -u | shuf | head
SURANT
1IR9DT
111DIDI
ARR0W1
I311
SGRD
D1R91T
0DI10D
II000
1ID1
```

其中有一些看起来并不是我们想要的。好吧，不过这并没什么大碍，在前面我们在开始部署的代码中提出的“非常直接的代码优先修复”，比如在验证注册号之前采用“只接受大于90%的置信度”。

运行`fgrep 'confidence: 9' bendigo.txt | cut -d' ' -f 6 | sort -u`
把它切成90%可信度的车牌号码，将它们过滤成唯一的车牌号码，那这样我们得到了些什么？

```
0G700       HERE      M5ER      TUG700    WKX2D2
0NRED       HM5ER     MP356     TUG70Q    X036
1HM5ER      IUG700    R1GHT     TUG70U    XP036
1IR9IT      JG700     R1LV      TUG7Q0    XS036
1ZZ735      KEEP      SLV522    TZ2735    XSP036
DH0SAHUT    KX212     SLV52Z    TZZ735    XSP036E
ERGE        KX2D2     SLV5Z2    UG700     XSPQ36
ERQGHT      KXZ12     T0G700    UG70U     YLJ641
G700        LANE      T0G70U    VKX212    YLJ64D
GR1L        LJ641     T2Z735    WKX212    YLJ64I
GRILL       LV522     TDG700
```

好的，所以我们还有一些明显的重复和识别错误，大概会在注册验证的时候将会排除这些。不过之后我们把27分钟的视频放到VicRoads网站上校正这些数据时，我们总共得到了7个自动认可并是有效号码。

![OpenALPR中的少数几个板块在野外正确识别，成功！]()！

![那是TUG700还是TDG700在中间？OpenALPR无法决定。两者都是有效的牌号。显然，“YLJ641”不在VicRoads数据库中。]()

我在这里不是故意虚伪的：从OpenALPR过滤器中匹配到“有效”的号码是一个棘手的问题。我鼓励任何人可以去尝试一下，并发布他们的方法和结果。但是，即使将数据过滤到很好的匹配中，基本的问题是OpenALPR在我抛出的每一个视频中都会丢失大量清晰可辨的盘符，并且它一直都是这样去处理的。

![这个版本在全分辨率视频中非常清晰。OpenALPR在这一帧中识别出一个板，但不正确地识别为“10DID”。]()

我喜欢开源的东西！这里有一个免费的、开放源码、健壮、快速、准确的ALPR系统！这是一个很棒的集成系统，如果这个项目发布之后，任何人使用了他们的开源代码！但OpenALPR还没有去弄这一块。假设现在已经有一个开源的并很完善的解决方案，在上面的每个问题中，当然可能是有了25％的解决方案也好，而这不是仅仅提高了其质量的问题了，更多是在算法和识别技术上了，也可能是硬件的问题，哈哈哈哈。

这个项目能不到8600万美元吗？也许，可以用OpenALPR作为起点吗？也许，它会实际降低成本吗？谁知道呢：这是也行是一个具有复杂、很有挑战性的需求项目。

tags: Open Source、Alpr、Computer Vision。

