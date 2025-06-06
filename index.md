# Gomoku

上次我们的问题是 110000020 下一步最佳位置

110000020 转化成棋盘图就是

```
1 1 0
0 0 0
0 2 0
```

根据我们前面学习的活和冲的规则，和规则优先级

```
c1<h1<c2<h2<c3<h3<c4<h4<...
```

让我们重新来数一下每个空格如果落子能得到多少规则

位置3放2:

```
1 1 2
0 0 0
0 2 0
```

横112:    
竖200:c1    
撇200:c1    
捺2:    

3:c1,c1

位置7放2:

```
1 1 0
0 0 0
2 2 0
```

横220:c2  
竖102:  
撇002:c1  
捺2:  

7:c2,c1


从位置7和位置3得到的规则来看
3:c1,c1
7:c2,c1

明显7的规则优先级比3大， 应该选7，但是问题是如果2选7， 那1就选3，形成

```
1 1 1
0 0 0
2 2 0
```

这样2就输了

所以我们也知道2应该选3，而不是7， 那为什么按活和冲的规则， 和优先级，选不到最佳位置3呢？


答案是因为我们教给ai的规则不够好，不够聪明。为什么不够聪明呢？
从上面位置7和位置3来看，是因为2**需要防守**，所以优先选3
也就是说，ai之所以不选3， 是因为它不懂， 我们没教它， **防守**

让我们来回顾一下我们的规则，很简单，活和冲

```
活: 两端都没有阻挡且有机会连珠获胜的模式
冲: 有且只有一端被阻挡且有机会连珠获胜的模式
```

这2个规则，都是考虑自己如何连珠获胜的规则，也就是都是进攻的规则， 这就是为什么ai选7不选3的根本原因

下棋，对弈，是一种攻防游戏，好的策略应该要同时考虑进攻和防守，只考虑（自己）进攻的策略是很差劲的

有多差劲？比如上面选7不选3，那么差劲

那么如何防守呢？其实就是换位思考，对手怎么进攻，我们就怎么防守，因为我们的防守是对方的进攻造成的，兵法有云，知己知彼百战百胜。

所以，一个人，不懂换位思考的时候，有多差劲？ 选7不选3，那么差劲

知己尚且不容易，更别说知彼。唯有加强内观和外观，向内观测自己的内心，向外保持开放谦卑的心态，接触外在人、事、物，思考学习其背后的规律。

回到我们对弈游戏，对手会怎么进攻，实际是不容易琢磨的，不过，没关系，琢磨不了就随他去吧，我们做我们能做的事，假设，假设对手水平和我们差不多，也只学会了活和冲，以及规则优先级。

让我们重新来看最原始的问题, "110000020 下一步最佳位置"

```
1 1 0
0 0 0
0 2 0
```

学习了活和冲规则，以及优先级的ai，虽然它只懂得进攻，最终选择了位置7，是不是意味着ai就没有防守了呢？

也不是，2选择位置7，意味着，1不能进攻7， 也就是说2防守了1对位置7的进攻！

那么，1对位置7的进攻有什么阴谋？这个只有那个”彼“才知道，我们前面说了，随它去吧。现在假设对手就跟我们一样。。。

对，对手1在开始数位置7规则啦，假如1进攻位置7，棋盘图如下

```
1 1 0
0 0 0
1 2 0
```

横120:    
竖101:c2  
撇001:c1  
捺1  

1位置7: c2,c1

实际上，现在并不是轮到1，1不可能落子，那这个进攻只是1的**机会**进攻，叫做有机会，跟**机会成本**概念类似，

此处，少年说问一下deepseek，机会成本概念

机会成本是指在做出某一选择时，所放弃的其他最佳替代选择的收益。简单来说，就是为获得某种利益而必须放弃的其他可能的最大收益。

。。。


现在相对的2来说，2选择位置7进攻，意味着，消灭1进攻位置7的**机会**，这就是防守的意识啊！

是时候来进一步完善我们的活和冲的规则啦，活和冲，都是进攻的概念，那不让进攻，就是防守啦，怎么表示呢？其实这防守概念离不开进攻，在攻防游戏里面，是对立的两个概念，阴阳相生，你要进攻啥，我就不让你进攻啥，所谓不让，这里，我们取”抑制“的抑，抑活和抑冲，用符号!来表示


补充了防守意识规则后的规则概念定义如下:

```
活: 两端都没有阻挡且有机会连珠获胜的模式
冲: 有且只有一端被阻挡且有机会连珠获胜的模式
抑活: 假设对方进攻形成的活的模式
抑冲: 假设对方进攻形成的冲的模式
```

现在，然我们再一次回顾最原始的问题，"110000020 下一步最佳位置"， 这次，我们先尝试按上面定义来给之前不懂防守的ai，加入防守的**意识**

棋盘，

```
1 1 0
0 0 0
0 2 0
```

根据步数，轮到2

还是从空格数规则开始，空格有位置 345679


位置3:

进攻

```
1 1 2
0 0 0
0 2 0
```

横112:  
竖200:c1  
撇200:c1  
捺2:  

位置3**进攻**规则: c1,c1

上面过程想必认真听讲的小朋友，应该比较熟悉了

现在，我们来看防守

防守，就是抑制进攻啦，让我们按抑活和抑冲的定义来， **假设对方进攻***， 

即，假设对方1进攻位置3，现在让我们扮演一下角色1

位置3:

进攻

```
1 1 1
0 0 0
0 2 0
```

横111:c3  
竖100:c1  
撇100:c1  
捺1:  

位置3**进攻**规则: c3,c1,c1

好啦，现在角色扮演结束，灵魂出窍归位，回到角色2，上述角色1得出的规则对于角色2来说，都是**抑**！！

汇总角色扮演前和扮演后的规则，得到角色2在位置3的规则列表，如下

位置3: c1,c1,!c3,!c1,!c1


其他空格依样画葫芦

```
位置3: C1,C1,!C3,!C1,!C1
位置4: C1,!C1,!C2
位置5: H1,H1,!H1,!C2,!H1
位置6: C1,H1,!C1,!H1
位置7: C2,C1,!C2,!C1
位置9: C2,C1,!C2,!C1
```


初步感受一下，位置3和位置7的规则列表，发现位置3有个!C3, 根据我们之前的活和冲优先级，

```
c1<h1<c2<h2<c3<h3<c4<h4<...
```

我们大概有个印象，活和冲后面的数字越大，优先级越高

那么加入抑制的规则后，优先级是否还遵循上面的规律呢？在不学习规则的情况下，凭感觉和经验，我们也知道位置3优先级比位置7高，因为，角色2不得不落在位置3上，不然就输了


让我们仔细观察对比一下位置3和位置7的规则列表

位置3: C1,C1,!C3,!C1,!C1

位置7: C2,C1,!C2,!C1

再约掉相同的规则

位置3: C1,!C3,!C1

位置7: C2,!C2

哪个优先级比较高呢？我们前面定义的优先级，这里没定义，让我们来推断一下, 对于抑制的规则，我们有

```
!c1<!h1<!c2<!h2<!c3<!h3<!c4<!h4<...
```

为什么呢？简单来说，把感叹号!拿掉，就是对方机会进攻规则的优先级，对方优先级越高，收益越大的规则，我们抑制它的优先级也随着越高、收益越大，这就是前面为什么说，进攻和防守是对立的，阴阳相生的，对方进攻啥我们就防守啥

现在让我们尝试更严格一点的证明这个抑制规则优先级，比如证明 !h1<!c2 ？

因为假设2连胜，那!c2就是阻挡对方获胜，!h1就是阻挡对方形成活1， 这个明显是!c2优先级高

假设3连胜，那么 !c3就是阻挡对方获胜， !h2就是阻挡对方形成活2， 优先级也是 !c3 高， 以此类推

目前为止，我们得到了2条规则优先级链

```
c1<h1<c2<h2<c3<h3<c4<h4<...

!c1<!h1<!c2<!h2<!c3<!h3<!c4<!h4<...

```

现在，让我们尝试将这2条优先级链统一成一条，因为对弈决策的时候，在有进攻有防守多个策略选择的时候，我们还是需要作出最优的决策判断，而我们的依靠，就是背后的大一统规则优先级链

比如，拿下面规则来分析， 背景是典型的 五子棋，5连胜

h5,c5,!h5,!c5

h4,c4,!h4,!c4

根据已有的2条规则优先级链，有

h5>c5, !h5>!c5

h4>c4, !h4>!c4

再解释就是，如果能形成 h5, 那我们就优先选择 h5, 活5，5连胜，已经获胜，没有活5可以选了， 冲5也可以，同样，5连获胜。

那么，c5和!h5哪个优先级高呢？当然是c5, !h5意味着对方已经形成了h4, 活4，这个时候，已经没办法阻挡对方获胜了，当然，也不是完全没办法，办法就是我们立即就胜，不用阻挡，而c5冲5就满足这个条件。这里隐含的一个攻防哲理就是，最好的防守就是进攻。在这种情况下，也仅剩强攻的机会，才能反败为胜。所以，我们可以把上述优先级链条继续打通

h5>c5>!h5>!c5

h4>c4>!h4>!c4

那么，看一下上下2条链，能否串联起来？ h4和!c5哪个优先级更高？实际上，!c5意味着阻挡对方c5冲5，5连胜，当然比自己形成h4活4，优先级更高！所以把两条链条整合在一起就有

```
h5>c5>!h5>!c5>h4>c4>!h4>!c4
```

再推广一般化，就形成了我们的大一统优先级链

```
!c1<!h1<c1<h1<!c2<!h2<c2<h2<!c3<!h3<c3<h3<!c4<!h4<c4<h4<!c5<!h5<c5<h5<...
```

有了上面的大一统优先级链，让我们再来回顾前面的位置3和位置7的规则问题

```
再约掉相同的规则

位置3: C1,!C3,!C1

位置7: C2,!C2

哪个优先级比较高呢？
```

!C3优先级比所有*2的规则都高， 所以，学会了我们活和冲，抑活和抑冲，以及大一统规则优先级链的ai，再一次回答最原始的问题，"110000020 下一步最佳位置"，的时候， 会优先选择位置3，即

```
1 1 2
0 0 0
0 2 0
```

现在，让我们继续问ai，"112000020 下一步最佳位置"，在ai回答之前，先有请我们的两位小白鼠来角色扮演一下ai，来回答这个问题







