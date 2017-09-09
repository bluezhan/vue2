
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
- [乱入：I have a funny story from Finland](https://www.resavr.com/comment/how-replicated-an-86-9466504)
- [乱入：Inside Dev (Sep 3rd, 2017)](https://inside.com/campaigns/inside-dev-2017-09-03-3154/sections/development-dregs-17344)
- [乱入：谷歌无人车之父Sebastian Thrun：摄像头才是无人驾驶最好的方式](https://baijia.baidu.com/s?id=1576951828995877033&wfr=pc&fr=app_lst)

上文有翻译：
- [【57行代码搞定8600万美元项目】用开源工具DIY车牌识别系统](https://mp.weixin.qq.com/s/iwPI8g2JwabwiCO8kfw8Hw)
- [57行价值八千万美元的车牌识别代码](https://yq.aliyun.com/articles/199312)

![]()

当你使用现有的开源技术来进行实验时，仅仅只是为了能看到结果的话，那就不错的。

在Medium上的一篇文章“我如何以57行代码复制8600万美元的项目”，已经在过去几天激烈地进行了几轮，描述了为澳大利亚维多利亚警察局开发的自动牌照识别系统（ALPR）系统是否可以使用开放式来源ALPR系统OpenALPR。这基本上是无处不在的套路文章标题“为什么这需要多少多少美金？我可以在一个周末用代码就搞定了！”，对任何具备基础逻辑的（或复杂的）科技推导出的结论和对之评论。

然而，由于OpenALPR是免费且开源的，所以我们可以测试这个说法的合理性。

Ignoring the boring stuff like getting OpenALPR working on your local computer, let’s jump straight to trying to automatically pull license plates out of a dashcam video. For my test video I picked “Drive around Bendigo”, a thrilling 27 minute YouTube video of someone “Driving around Bendigo, Victoria, Australia,” which I felt would be a somewhat representative test, as it’s 1080p car footage that’s quite clear and from around the area where the system will be deployed. After downloading it with youtube-dl I fed it to OpenALPR with time alpr --clock -n 1 'Drive around Bendigo-hrD75ebjCms.mp4' > bendigo.txt and let it churn for…

请忽略无聊的东西，就像让OpenALPR在你的本地电脑上工作一样，让我们​​直接跳到试图自动将牌照从dashcam视频中拉出来。对于我的测试视频，我选择了“Drive around Bendigo(围绕本迪戈驾驶)”，这是一个惊人的27分钟的YouTube视频，有人“驾驶澳大利亚维多利亚州本迪戈”，我觉得这将是一个有代表性的测试，因为它的1080p车载镜头是非常明确的，围绕系统部署的区域。下载后，youtube-dl我将它加入到OpenALPR中`time alpr --clock -n 1 'Drive around Bendigo-hrD75ebjCms.mp4' > bendigo.txt`，让它来工作...

Hm. Well that’s a problem. Processing my 27 minute video took just over 3.5 hours on my 3.5GHz Core i7. Not exactly real-time. Pencil in a few quid for “optimization” and a few more for “ultra-beefy computer hardware in every patrol vehicle” I guess.

嗯；那是一个问题。处理我的27分钟视频在我的3.5GHz Core i7上花费了超过3.5小时。不完全知情得说。铅笔在几个“优化”，还有一些“每个巡逻车超强电脑硬件”我猜。

![OpenALPR processing time. Yikes.]()

Anyway, on to the results! Gotta spend CPU cycles to mak catching thieves easier, as the saying goes. Let’s filter the results down to just the potential license plates with fgrep confidence bendigo.txt. Pipe that in to wc -l and it looks like we’ve got 6,137 potential plates (or 1,653 if we filter those down to unique plate numbers). Not bad! Wait, that seems like a lot. Let’s take a closer look:

无论如何，总算有了结果！俗话说，要花费CPU周期才能使盗贼更容易。我们将结果过滤到只有潜在的车牌`fgrep confidence bendigo.txt`；管道中wc -l，它看起来像我们有6,137个潜在的板（如果我们过滤掉唯一的板号，则为1,653）。不错！等等，这似乎很多。我们来仔细看看：

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

Some of these seem…bad. Ok, no big deal, let’s deploy some of the “very straight forward code-first fixes” proposed in the article like adopting “a threshold […] that only accepts a confidence of greater than 90% before going on to validate the registration number.”

Running `fgrep 'confidence: 9' bendigo.txt | cut -d' ' -f 6 | sort -u` to cut it down to just the 90%+ confidence plate numbers and filter them to only the unique ones, what do we get?

其中有些似乎不好 好的，没有什么大不了的事情，让我们部署一些文章中提出的一些“非常直接的代码优先修复”，比如采用“只接受大于90％的置信度的阈值[...]，然后才能验证注册号码“。

运行`fgrep 'confidence: 9' bendigo.txt | cut -d' ' -f 6 | sort -u`将其切割成只有90％+置信板数，并将其过滤到唯一的，我们得到什么？

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

OK, so we still have some apparent duplicates and recognition errors, and presumably the registration validation will sort these out. Checking these with the VicRoads site, we wind up with a grand total of seven automatically-recognized “valid” plates for 27 minutes of video.

好的，所以我们还有一些明显的重复和识别错误，大概在注册验证的时候将会排除这些。在VicRoads网站上校正这些数据，我们总共有七个自动认可的“有效”板27分钟的视频。

![One of the handful of plates OpenALPR correctly recognized in the wild. Success!]()
OpenALPR中的少数几个板块在野外正确识别。成功！

![Is that TUG700 or TDG700 in the middle? OpenALPR can’t decide. Both are valid plate numbers. “YLJ641” next to it apparently isn’t in the VicRoads database.]()
那是TUG700还是TDG700在中间？OpenALPR无法决定。两者都是有效的牌号。显然，“YLJ641”不在VicRoads数据库中。

I’m not being intentionally disingenuous here: filtering the plate matches from OpenALPR down to just the “good” ones is a tricky problem. I encourage anyone to try, and post their methods & results. But even beyond filtering the data down to good matches, the basic problem is that OpenALPR outright misses a huge number of clearly-legible plates in every video I’ve thrown at it, and takes forever to do so.

我在这里不是故意虚伪的：从OpenALPR过滤板匹配到“好”的匹配是一个棘手的问题。我鼓励任何人尝试，并发布他们的方法和结果。但是，即使将数据过滤到很好的比赛中，基本的问题是OpenALPR在我抛出的每一个视频中都会丢失大量清晰可辨的盘符，并且永远都是这样做的。

![This plate is pretty legible in the full-resolution video. OpenALPR recognizes a plate in this one frame, but incorrectly, as “10DID”.]()
这个版本在全分辨率视频中非常清晰。OpenALPR在这一帧中识别出一个板，但不正确地识别为“10DID”。

I love open source! I’d love for there to be a free, open source, robust, fast, and accurate ALPR system! It would be great if this project released whatever they do use as open source! But OpenALPR isn’t there yet, and pretending there’s already an open source solution for every problem when there’s maybe 25% of a solution there instead is never going to improve its reputation for quality.

Could this project be done for less than $86M? Maybe. Could they use OpenALPR as a starting point? Also maybe. Would it actually reduce the cost? Who knows: it’s a complex project with complex requirements.

我喜欢开源！我会喜欢这里有一个免费的，开放源码，健壮，快速，准确的ALPR系统！这将是伟大的，如果这个项目发布任何他们使用的开源！但OpenALPR还没有，假装已经有一个开源的解决方案，每个问题，当然可能 25％的解决方案，而不是提高其质量的声誉。
这个项目能不到8600万美元呢？也许。可以用OpenALPR作为起点吗？也许。它会实际降低成本吗？谁知道：这是一个具有复杂要求的复杂项目。


tags: Open Source、Alpr、Computer Vision。

