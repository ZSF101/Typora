# ISIS

兼容TCP/IP和OSi模型

![image-20231003163800250](assets/image-20231003163800250.png)

isis是位于网络层，is是中间系统，中间系统就是路由器，is-is就是路由器和路由器之间运行的路由协议。es-is就是路由器到pc,   es就是终端。相当于ARP。

![image-20231003164405602](assets/image-20231003164405602.png)

我们通常写47/49.47是共有的ID，49是私有的ID。Area ID是49.0001。   系统ID通常拿MAC地址，所以48位，6个字节。1～13byte，说明这个是可变长的。

Net包含区域信息(类比ospf区域0和其他区域)和系统ID(即路由器ID)信息，   可以配置多个NET地址(方便网络割接或者变更)

但是可以是多个NET地址，但只能是一个

比如NET1:1111.1111.1111.00

NET2:2222.2222.2222.00(不行，只能是1111.1111.1111.00)



![image-20231003165131636](assets/image-20231003165131636.png)

![image-20231003171310595](assets/image-20231003171310595.png)

多区域：Level 1(末节或者常规)。Level2(骨干)  L1/L2(区域边界路由器)相当于ABR

![image-20231003172657797](assets/image-20231003172657797.png)

完成R4和R5的isis邻居

R4:

```java
isis
network-entity 49.0002.0004.0004.0004.00
```

一个设备默认的就是L1/L2

![image-20231003211106713](assets/image-20231003211106713.png)

```java
isis-name  ？    //就是路由器的名字
isis-name R4
```

可以看到是个String类型，

![image-20231003211334414](assets/image-20231003211334414.png)

接口下启动isis

```java
int g0/0/1
isis enable
```

竟然有两个邻居，因为这个设备默认即是L1，也是L2

![image-20231003212032263](assets/image-20231003212032263.png)

在R5上

```java
dis isis route-table      //发现既有L1的路由，也有L2的路由。
```

![image-20231003212501616](assets/image-20231003212501616.png)

![image-20231003212531611](assets/image-20231003212531611.png)

但是学习真正的路由却是只有L1的没有L2的

```java
dis ip rou pro isis
```

![image-20231003212956804](assets/image-20231003212956804.png)

为啥只有L1的，没有L2的。这其实是isis的选录原则。

一条路由即从L1学习到也从L2学习到，L1优于L2。

不过这个可以调

```java
isis-level level-2
可以看到只剩L2没有L1了。
```

![image-20231004092914889](assets/image-20231004092914889.png)

再去R4去看看，也只有L2，没有L1.

![image-20231004093030186](assets/image-20231004093030186.png)

其实最好是R4和R5都调。

R4

```java
isis
isis-level level-2
```

```java
dis th      
```

![image-20231004093453179](assets/image-20231004093453179.png)

![image-20231004093724486](assets/image-20231004093724486.png)

```java
dis cu conf isis
dis cu int g0/0/0
dis cu int lo0
dis isis int   
dis isis peer
```

![image-20231004094118877](assets/image-20231004094118877.png)

这个设备是L2的。L2代表了调整了设备的级别。

![image-20231004094511003](assets/image-20231004094511003.png)

这个是接口即工作在L1，也工作在L2

![image-20231004094528507](assets/image-20231004094528507.png)

![image-20231004100733718](assets/image-20231004100733718.png)

有L2就是骨干区域，所以x,y都是骨干区域

![image-20231004101211202](assets/image-20231004101211202.png)

R1不能学习到L2的明细路由，非常吻合特殊区域的概念，但是可以通过默认路由去往骨干区域。





​	![image-20231004110216882](assets/image-20231004110216882.png)

![image-20231004110959399](assets/image-20231004110959399.png)

P2p不支持改为广播类型，对isis来说，两测网络类型不一致无法建立邻居关系。

![image-20231004111357343](assets/image-20231004111357343.png)



## DIS

![image-20231004112304494](assets/image-20231004112304494.png)

![image-20231004113320866](assets/image-20231004113320866.png)

DIS的holdtime时间为正常时间的三分之一。至于为啥是R2是DIS，接口优先级默认都是64，那就说明MAC地址比其他的要高

![image-20231004114120096](assets/image-20231004114120096.png)

![image-20231004114436916](assets/image-20231004114436916.png)

这个和ospf的DR还是不太一样的·，ospf的DR中0是放弃选举。

还有就是ospf是不能抢占的，但是isis是可以抢占的。

Isis不存在副班长，而ospf是有BDR的。

```java
isis dis-priority ?
```

![image-20231004114947842](assets/image-20231004114947842.png)

```java
isis dis-priority 127
```

去R2上看看，DIS的holdtime整体基本在1-10s之间

![image-20231004115126690](assets/image-20231004115126690.png)

去R1上看看，g0/0/1这个接口，在L1中是DIS，在L2中不是DIS，因为这不存在L2的邻居关系

![image-20231004120256687](assets/image-20231004120256687.png)

![image-20231004121818988](assets/image-20231004121818988.png)

Isis中DR是基于链路的，而不是基于设备的。R2上g0/0/1接口在L1上不是DIS，在L2上是DIS。

![image-20231004122320416](assets/image-20231004122320416.png)

![image-20231004122502270](assets/image-20231004122502270.png)



## isis报文和格式（不重要，了解即可）

![image-20231004123049285](assets/image-20231004123049285.png)

Isis的源地址是MAC地址，但是ospf的源地址是IP地址

![image-20231004123330593](assets/image-20231004123330593.png)

isis封装在二层，目的MAC是一个组播MAC

![image-20231004124040989](assets/image-20231004124040989.png)

![image-20231004124229122](assets/image-20231004124229122.png)

01开头是组播地址。

![image-20231004124412091](assets/image-20231004124412091.png)

下面两个就是hello报文，（最后一个是特定头部，倒数第二个是通用头部，就是谁都有）倒数第三个是逻辑链路控制层，占三个字节，

所以isis的MTU是1500-3=1497.

![image-20231004124806032](assets/image-20231004124806032.png)

可以看到的是1497。

![image-20231004125744136](assets/image-20231004125744136.png)

MTU不一致将导致无法建立邻居关系



最大可以有3个区域地址。当然也可以改，不过不能改系统ID，

![image-20231004130527657](assets/image-20231004130527657.png)

打开报文的通用头部，可以看到

![image-20231004130642270](assets/image-20231004130642270.png)

![image-20231004131036132](assets/image-20231004131036132.png)

![image-20231004131116191](assets/image-20231004131116191.png)

![image-20231004133147859](assets/image-20231004133147859.png)

![image-20231004133233735](assets/image-20231004133233735.png)

来个示范

![image-20231004133335687](assets/image-20231004133335687.png)

128和129。  128说的直白就是路由信息。你能到哪里去。

![image-20231004133503513](assets/image-20231004133503513.png)

路由信息在LSP中。默认度量值也就是开销

![image-20231004141233490](assets/image-20231004141233490.png)

CSDN就是目录



## Isis的cost

![image-20231004142042787](assets/image-20231004142042787.png)

负载分担默认值是8,模拟器一般是8，真实设备可能是16或者32，不同设备，不同型号也不一样。

```java
maximum load-balacing ?
```

![image-20231004142507313](assets/image-20231004142507313.png)

IGp一般都是8,  egp一般是1。

在R4上的g0/0/2接口上增大开销值

```java
isis cost 15
```

R4干脆就不走R3了。

![image-20231004143558893](assets/image-20231004143558893.png)

如果不修改的话，是负载分担

![image-20231004143715721](assets/image-20231004143715721.png)

可以修改全局的开销值。但是不建议

```java
circuit-cost ?
```

![image-20231004143956066](assets/image-20231004143956066.png)

## 开销值的宽度量和窄度量

![image-20231004152002826](assets/image-20231004152002826.png)

```java
cost-style ?
```

![image-20231004152409135](assets/image-20231004152409135.png)

```java
cost-style wide
```

换成宽度量就变了160多万

![image-20231004152531504](assets/image-20231004152531504.png)

宽度量不影响邻居关系的建立

![image-20231004152813100](assets/image-20231004152813100.png)

但是可怕的来了，没路由。度量类型不一致导致无法计算路由。

![image-20231004152855801](assets/image-20231004152855801.png)

建议全都设置成宽度量。





![image-20231004153353307](assets/image-20231004153353307.png)

![image-20231004154318800](assets/image-20231004154318800.png)

R1和R2，R3都是Level-1  但修改了区域ID，就不能建立邻居。而level-2 不做要求

**第二点。网络类型必须一致**

![image-20231004161352359](assets/image-20231004161352359.png)

第三点，isis接口的地址要求必须处于同一网段。

![image-20231004161834964](assets/image-20231004161834964.png)

当然如果忽略就不会影响

```java
isis peer-ip-ignore 
暂时没有邻居，只是初始化，需要到R4上也要配置
```

![image-20231004162354964](assets/image-20231004162354964.png)

去R4上配置

```java
isis peer-ip-ignore
```

![image-20231004162728768](assets/image-20231004162728768.png)

这时候正常了。



**第四点：不可以邻居之间配置接口静默**

```java
isis slient 
```

抑制报文的发送与接受





## LSP

![image-20231004164425134](assets/image-20231004164425134.png)

![image-20231004165108615](assets/image-20231004165108615.png)

j可以看到p2p，也进行了三次握手，不过依旧可以调

![image-20231004165206471](assets/image-20231004165206471.png)

```java
isis ppp-negotiation 2-way
```

![image-20231004170340044](assets/image-20231004170340044.png)

![image-20231004170537079](assets/image-20231004170537079.png)

Remaining Lifetime 就是剩余寿命。还能活多久

![image-20231004173707422](assets/image-20231004173707422.png)



## ATT比特

![image-20231005105048932](assets/image-20231005105048932.png)

![image-20231005105127221](assets/image-20231005105127221.png)

直连不通，看arp



**overload过载位**

```java
isis
set-overload ?
```

![image-20231005110355976](assets/image-20231005110355976.png)

```java
set-overload
```

在看路由

![image-20231005111231204](assets/image-20231005111231204.png)

再来对比设置过载位后的路由，可以发现之间默认路由0.0.0.0是有2条，现在是1条。

![image-20231005111300865](assets/image-20231005111300865.png)

去R4上看看去往1.1.1.1路由

![image-20231005112318763](assets/image-20231005112318763.png)

去·R2上undo掉overload，再去R4上看看

![image-20231005112440985](assets/image-20231005112440985.png)





## LSP同步

![image-20231005114527170](assets/image-20231005114527170.png)

![image-20231005120045820](assets/image-20231005120045820.png)

![image-20231005120235106](assets/image-20231005120235106.png)

![image-20231005120759556](assets/image-20231005120759556.png)



## isis的路由计算【SPF算法】

**L2区域可以学习到L1区域的明细路由。L1区域默认只能通过默认路由去往L2或者外部路由**

影响路由计算的要素

1）路由级别  L1（被看作区域内部路由）要优于L2

2）开销值：同级别下，开销值小的路由优先。如果相同就是负载分担。

3）路由渗透：isis把L2的路由引入到L1路由，区域之间的路由

4）权重：



在R1上查看，可以看到2.2.2.2和3.3.3.3都是L1的路由

![image-20231005133055590](assets/image-20231005133055590.png)

现在R1与R2，R3也是L1的邻居关系

![image-20231005133300934](assets/image-20231005133300934.png)

现在改变一下，

```java
isis
is-level level-1-2
```

那现在我们既有L1的邻居也有L2的邻居

![image-20231005133506840](assets/image-20231005133506840.png)

那么R1上去往2.2.2.2和3.3.3.3的路由是L1，还是L2的。

```java
dis ip rou pro isis
```

![image-20231005133845651](assets/image-20231005133845651.png)

可以看到，还是L1的路由，因为L1优于L2

可以看一下。开销值都是10，但是L1放进了路由表，而L2没有放进路由表。

```java
dis isis route
```

![image-20231005134117601](assets/image-20231005134117601.png)

**L2区域可以学习到L1区域的明细路由。L1区域默认只能通过默认路由去往L2或者外部路由**

可以看到R5上有1.1.1.1和2.2.2.2  3.3.3.3的路由。L2区域可以学习到L1区域的明细路由。

![image-20231005135031737](assets/image-20231005135031737.png)

![image-20231005135342926](assets/image-20231005135342926.png)

而L1却不知道L2的情况，只知道0.0.0.0可以去往L2

![image-20231005135438191](assets/image-20231005135438191.png)

![image-20231005135635189](assets/image-20231005135635189.png)



**路由渗透：isis把L2的路由引入到L1路由，区域之间的路由**

R3:

```java
isis
import-route isis level-2 into level-1 ?
```

![image-20231005141414774](assets/image-20231005141414774.png)

```java
import-route isis level-2 into level-1 //把所有的L2路由引入到L1区域中
//对于L1而言，根据最长匹配原则，选择明细路由而不是默认路由去转发数据
```

不再是0.0.0.0了

![image-20231005142020127](assets/image-20231005142020127.png)

这个A表示添加到路由表，D表示是直连的。

![image-20231005142413535](assets/image-20231005142413535.png)

U表示up位,   up表示从L2引入L1的路由。

![image-20231005142624410](assets/image-20231005142624410.png)



## 路由泄漏

git remote set-url origin  https://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@github.com/ZSF101/Typora.git

