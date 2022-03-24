# hello_asm

and operator

一般课本不会讲的很细，因为没那么重要，但是这个东西可以开发看待的点有很多。

```
A AND B -> C
0     0    0
0     1    0
1     0    0
1     1    1
```

1. 二元操作符，区别于一元
2. 从结果来看，是为了筛选出全真的情况，即保证结果为真的需要1and2，因此命名为AND，即从C的角度看，有1则两个都满足，有0即起码有一个条件不满足，更何况两个都不满足了。那么着只是个初步判断，实际上你需要找补、解决问题的时候，你需要找到到底哪一环不行，所以C只是对结果的判断，真的想把事情做好，还得深入细致的去判断原先的每一个A或者B，甚至好几个AND串联只保证结果的判断而不得不深入每一环。除非联系后面的OR另寻出路，这条AB不是出不了结果1么，那就换OR的其他选项A1B1。
3. 但是基于四种真值表的具体搭配情况，可以通过很多其他方面去理解，比如，从0的角度看，就是含0则0.
4. 从操作数A来看，则对A进行AND 1操作，即用B的1来筛选A中的1和A中的1，比如子网掩码就是这么用。
5. 那么再来看AND 0操作有什么C的结果呢呢，只有00000，没有区别，即消除原来A所携带的信息

上面代表了初步的猜想、试探，其实总结下来就是对于A中的每一位，哪个位用1与就是保留这一位的原数，用0与就是混淆原数，因为原数可能是0可能是1，不一定错，所以不一定对。

但是00000看不出原先的信息，所以无所谓其中哪些对了，因为你不知道哪些位的0对了。看，这里部分的正确，但是你不知道哪个正确，所以也应当认为它是错误。

二元操作符在英文中可以用dyadic，虽然binary operation也可以，不过更精确一些，因为这个二元操作符不是指其数值系统用二进制，而是指其影响因子为2个。

只是恰巧，这里既是二元，元的值表示又是基于二进制的。

上面的真值表是一维的列表，其实可以简化为二维的列表
```
AND 0 1
0   0 0
1   0 1
```

这样更精简，只不过不大利于我人脑的思考方式，因为我还要脑部左边俩是A，上边俩是B，右下角四个值分别对应最后的结果C。很显然，只是比较看起来简介，但是用来归纳、演绎各种规律的尝试时不太方便：节省了纸的空间，却制造了更多理解的麻烦，就如同精简的高数课本，你还不如把辅导书写的好，一本是为了凸显自己的严谨正确，一本是为了学生怎么学着想，一口一喂。

就如同同样一个数值3000元，这里是十进制加中国元单位，其实还可以表示为二进制，也可以表示为美元作为单位，但不影响其本质表达的价值（value），而采用哪种形式在这里既重要，也不重要（在和价值相比较的时候）。

二进制上的逻辑运算符

OR

同为逻辑，其实不大懂什么叫逻辑，英文的logical翻译过来公平合理的，有原因的，应该的、有规律的，不是无缘无故的不是不公平的，符合事实的

那么言语上的逻辑和二进制上的逻辑运算，有什么不同呢

那就是二进制的运算的逻辑仅仅是二进制的运算，是人为规定的，不一定尽善尽美。只能做到尽量好用。这就是为什么AND的操作符的四种情况容易理解，但是蕴含的四种真值表不大容易懂，但那不重要，一致性才重要，随便弄一个即可。

OR

```
A B C
0 0 0
0 1 1
1 0 1
1 1 1
```

1. 从结果C来看，即为1则包含1，为0则一个都不行。这类似把A和B看作解决问题的并列选项而非相互搭配、依赖的一系列步骤。
2. 那么从实际操作来看，A或者B还是要选出到底哪个行啊，除非你两个解决方案同时进行，这样保证了结果就无所谓你们哪个方法是1（成功）的。
3. 从A的角度看，OR 0 可以保持原位的数不变，OR 1 则消除了信息，全为1了。
4. 所以OR 0 和 AND 1 一样，筛选出原先的A中的0 或者1.

这是几个角度呢，东西是依赖于解决问题而存在的，所以and 也好 or也好，要么是对A筛选，要么是对AB筛选出是否能成C。就这两个主要的角度，是解决问题的主要角度。

细分一下
1. 其中对A筛选，保持原位用OR 0和AND 1，如果想消除则AND 0，或者 OR 1.
2. 其中对AB判断，所有步骤用AND，选项option的判断用OR。option若能全部进行，在C为1的时候则不用细致管是哪个work，AND的步骤若全部进行，在C为1的时候全部都work了。
3. C要是为0，那么总结果已然保证不了，要么开源找新能work的方法为1， 使之OR结果为1，要么补全每个已有的旧方法的每一环，使之AND结果为1

从实现上来讲，and 遇见0 短路实现，or 遇见1短路实现.从机器的实现上，要考虑最小单位是8位的byte，从A的01分布来看，0多则多用and，1多则多用or，哪个更规律更适合做A容易先判断出结果那就让这个条件做A先算就不要让它做B后算。

这些细节的知而不用，和不知而不用是不一样。很多时候写代码的coding时间、运行时间、debug时间、扩展新功能时间、换了员工之后的普适性要做一个平衡，需要哪个就往哪里侧重。所以平常不写极端优化的代码，但纵观全局在某些要害吃紧的地方要优化的时候你要会。不然你就是那个无关紧要只负责平常工作的那个，既然你不能临危受命，担不了责任，自然你也不会分利益的时候有你的更多

口语中的or有好多意思，有的根据环境判断，类似一次多义通过更多信息确定其准确含义。or可以表示为可选的选择，也可以表示为二选一的必选其一就不能选其他，也可以表示同时可多选。那么能不能互相包括呢，默认的OR就是包括各个选项的inclusive or，还有个exclusive or就是多项限定为2项，由此选1则必不能选2，由此不能包含。所以加了这么个限定条件导致AB:11 -> c:0.

XOR就是eXclusive OR的缩写，不是XOR而是X和OR，有很多时候你从整体的角度去看待一个东西，没法搞清楚，其实它的意项在于他们的元素的组合，在于怎么把他们分开看才能理解。

XOR的结果就是从C的角度看：为1则异，为0则同。虽然可以这么表述，但其本来的意思（即它的命名）应该是另外一种表述，即OR之后为1的再去掉不包含的。也可以直接定义，不包含的或。

但我们知道命名并不是最重要的，它的最大用途，在真实世界中的最大用途才是最重要的，在历史的演变中，它最主要的用途会变化，甚至原先的用途会消失，导致什么个结果呢：就是XOR的最大用途不是在于异或这个用途本身，而在于其他。比如，判断是否不同，这个用途显然更有。

inclusive，指to close，闭环，exclusive，不包含的、独自的、不共享的。

从a的角度，xor 0，保持原位的数，xor 1 则取反。由此将abc，进行组合

当处理a的0或者1时，不能直接短路得出c来，因为此时正好2比2，不存在捷径。

a xor b之后的结果c，（强调这个过程之后），b中0的位，在c中和a一致，b中1的位，在c中和a相反。

c再与b xor，则 b中0的位与原先一致，b中1的位再次反转，所以结果d=a，只不过d中的0从来没变过，d中的1是由a中的1两次反转：1->0, 0->1.负负得正而来。魔法打败魔法，以毒攻毒而来的。

即a连续两次xorB会得到a，中间产物是C，即一次xor的结果。

若c再与a xor，则 c中经b0的位与原先一致，则相同的数xor后变成了0，而c中经B1的位取反，则两个相反的数xor后变成了1，即最后的结果是=b。即a xor b = c， 则不分先后再xor其中一个则得到另一个数。

这个结论可以通过真值表的结果进行证明，也可以根据转换过程进行推断是无关ac具体值，而是跟b相关的。

```
0 0 0 0
1 0 1 0

0 1 1 1
1 1 0 1
```

上面的c xor a 还可以从另一个角度，即a xor b = b xor a，但是交换律要先被证明已知，才可以这么套，即b xor a xor a = b，即通过第一个结论 a xor b xor b = a和交换律，进行ab替换成ba也能得出a xor b xor a = b。 能么？前提是交换律是已知的情况，所以 a xor b xor a = b xor a xor a

。那么 or and 也都符合交换律。

再进一步，a xor b xor a = b xor a xor a 能等于 a xor (b xor a) 吗？，如果需要这么用就去证明，这个类似结合律。。但是好处是什么呢，好处是发现后面的两个xor更快？

那么为什么后面两个会xor更快呢？？？？因为ba的真值表的概率虽然是2:2，但是具体涉及到具体的真值的时候，不还是要最后xor最前面 a吗，所以看不出必要，不探索了。

啊，可能下次要计算xor的时候直接等于a就好了，先计算后面，完全看不出什么用处。试一下

```
a b A xor B | A xor B xor A | B xor A | A xor (B xor A)
0 0    0               0         0         0
0 1    1               1         1         1
1 0    1               0         1         0
1 1    0               1         0         1
```
实验证明，符合结合律。。

上面混乱的遍历已存在物的各个属性，是自由探索的有雄厚的更底层的支撑，优点是忘了一点还可以从源头推论出来补全，脑袋会有一种安全感

而如果从实用角度，你就忽略了这些规律是怎么探索来的方法，而抓住了产出最大的几个有用的属性：masking，置0，置1，置反，分别对应and0, or1, xor1. 而剩下的and1， or0， xor0 则保持原数。

实用是实用了，但你不经常用就会忘。而如果你亲自梳理、推论过，你模糊的记得有这么哥置01反的功能，稍加尝试就能自己推出来，因为重要的虽然是结果，但你知道方法的结果能够再创解决类似问题，而不知方法，不知已存在的遍历、组合而直接记住结论好像记得少了，但在其它新问题的解决上，已知方法和已知旧问题的答案显然前者更好解决新问题。你天生的好奇心、发现的乐趣放置不用而用痛苦的记忆，祝你记忆的神经节发达并且擅长，人脑的存储显然是不如一台手机的，所以比拼精确性、容量大这种指标，为什么不让计算机帮你做呢。你做你人擅长的事情就好了。而实用的背记则不会有这个效果，你忘了就是忘了，他们为什么是这样也始终笼罩着你，恐吓着你让你本能的想要远离这些神经节、遗忘、忘记这些让你害怕的事。

回到计算机上，二进制数的and or xor的实现究竟是哪个更省电、哪个更快？01000分布不均的时候是不是置反再计算更快，同样的功能用哪个操作符更快？用1和0表示状态的时候，概率多的用1还是用0，省电和快是可以兼得的还是不一致的？计算的时候从位置作为顺序高位还是低位算起？还是不考虑位置而从值本身为0的位先算，还是值为1的位先算？需要选的时候再优化，因为我猜测他们的差异可以小到忽略不计。

如果仅仅有一位二进制，想让其置1还是置0或置反，直接新造一个数就好了，为什么还要新造一个数跟旧数进行运算？因为有时候要保留旧数的某些位的信息，而只处理部分位的值。那这种变化为啥不一开始就和铁定不变的值分开？？？？不管是以逻辑上最小的1位，还是实现上的1个byte，还是内存地址最小处理的64位，分开、隔离这些变化的啊。为什么混在一起让将来需要新造数、再进行and or 运算呢？

我们看分开，隔离是仅仅就减少运算这个方面的考虑，你还要考虑存储、传输、展示给人的空间和时间，所以兼顾很多面的时候，各个指标就不能在各自的范围内进行极限拉扯，因为这会损害其他指标（需要）。

归根结底，我们需要的不只这一点，是很多需要，每个需要的最佳形式并不是其他需要的最佳处理办法，他们需要相互妥协，要么自我检查下需要去掉其他需求。因为简单的单一目标当然可以肆无忌惮的优化。但是复杂的考量才真正考验你的智慧。

asm 指令有一两千，就像汉字有几十万一样，但是常用的就那几百个字，只考虑功能的话，最小集即可满足，至于更多的那些，显然是有着更多的需求：好听、典故、附带情感、偏好等等

mov 占 三分之一左右。。就这一个指令，在定义集中是一二千分之一，但是出现在实际的应用中的频率却是三分之一，而实际上它贡献的价值呢，却是我现在不知道的，所以同样是人，差距是很大的，但那些不提差距却强调共性的片面的忽悠者，就会称呼为美国人民。你是总统、精英、跟底层老百姓长得两个眼睛一个鼻子就好像用个美国人民的名称你们就一样了？做梦呢？如果为美国做贡献不看中钱的话，那为什么是总统可以是奴隶主可以有钱，而不是老百姓比总统有钱呢？瞎扯蛋，忽悠谁呢？越是忽悠越是说明事实的差距更大，只能用嘴来弥补。什么个人财产神圣不可侵犯，所以你们祖先可以侵略烧杀其他国家、可以贩卖黑奴，你赚完抢完钱了，开始装个文明人跟别人说要守规则你的是你的我的是我的不能互相抢了？？？还不是谁手里有枪谁说了算，你也配说什么自己守法、公平、靠自己才华、聪明才智？在混乱中算计他人压榨别人来的黑钱，动不动就跟别的国家开战，忽悠谁呢，为了和平？为了人权？为了帮助弱势群体？怎么帮着帮着全心全意为了美国人民那么事实上老百姓都穷了呢，当官的怎么说从不为一己之私却怎么一个个都不一不小心就生活、别墅、游艇、名表越来越好呢？

可能最初命名mov的时候是根据电路里某个实现mov了什么导致值在两个寄存器之间实现了复制的功能。类似往上翻页，才能实现看下一页的功能。实现和目标并不一致，甚至相反。也类似复制剪切一样，mov了这位置的东西到那个位置，如果再删掉原先电路的状态，这就是mov and delete了。但是却是真正的语义上的mov，担不是对电路的最小改动。

总之 mov的结果是copy，到底用了什么电子部件复现了a处的值到b处，这个mov到底是针对什么，已经被忽略掉了，因为你想深究就会去无限的深究下去，因为这个世界都是息息相关的，你从研究硅再到宇宙起源，我相信以你短短的几十年是研究不完这么多东西的。

we can use a,b to represent 2 operands, but to indicate more info on their direction, i'll use z,a which means not only 2 objects here, but also a is alpha, z is last, which means from a to z, a is source-operand, and z is destination-operand

but to get used on different style, here will use different style

i'm not going to write asm codes, i'm going to read the asm codes from highlevel Rust, so i won't go any irrelevant details.

```
mov z, a === z <- a
add d, o === d <- d + o
sub a, b === a <- a - b
lea 1, 2 === 1 <- 2's Address
ret      === return
call procedure_name
```

when destination-operand go first, which means operate on current register, then mov other value to here, add value to here, minus value from here

and if use a process-oriented style, which is a mov to b, then mov a b? 

and a add b to b?, then add a b? we'll see when we learn that sytle

```
jmp abc
```
