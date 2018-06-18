# 网络知识学习笔记一

标签： 网络知识

---
### 五层模型

1. 五层模型
    互联网的实现，分成好几层。每一层都有自己的功能，每一层都靠下一层支持。用户接触到的，知识最上面的一层，根本没感觉到下面的曾。

    如何分层有不同的模型，有的分七层，有的分四层。可以分五层

    * 应用层
    * 传输层
    * 网络层
    * 链接层
    * 实体层

2.  层与协议
    每一层都是为了完成一种功能。为了实现这些功能，需要大家都遵守共同的规则。这规则，就称为协议。

    互联网的每一层，都定义了很多协议。这些协议的总称，就叫做互联网协议。

### 实体层

    实体层就是把电脑连接起来的物理手段。它主要规定了网络的一些电气特性。作用是负责传输0和1的电信号。

### 链接层

1. 定义
    单纯的0和1没有任何的意义，必须规定解读方式： 多少个电信号算一组？每个信号为有何意义？这就是链接层的功能，它在实体层的上方，确定了0和1的分组方式。

2. 以太网协议
    早期的时候，每家公司都有自己的电信号分组方式，逐渐地，一种叫做以太网的协议，占据了主导地位。以太网规定，一组电信号构成一个数据包，叫做"帧"。每一帧分成两个部分：标头和数据。

![此处输入图片的描述][2]

"标头"包含数据包的一些说明项，比如发送者、接收者、数据类型等等；"数据"则是数据包的具体内容。

标头的长度，固定为18字节。数据的长度，最短为46字节，最长为1500字节。因此整个帧的最短为64个字节，最长为1518字节。如何数据很长，就必须分割成多个帧进行发送。

3. MAC地址
    上面提到，以太网数据包的标头，包含了发送者和接收者的信息。那么，发送者和接收者是如何标识的呢？

    以太网规定，连入网络的所有设备，都必须具有"网卡"接口。数据包必须是从一块网卡，传送到另一块网卡。网卡的地址，就是数据包的发送地址和接收地址，这叫做MAC地址。

    每块网卡出厂的时候，都有一个全世界独一无二的MAC地址，长度是48个二进制位，通常用12个十六进制数表示。

    ![此处输入图片的描述][3]

    前6个十六进制数是厂商编号，后6个是该厂商的网卡流水号。有了MAC地址，就可以定位网卡和数据包的路径了。

4. 定义地址只是第一步

    * 首先： 一块网卡怎么能知道另一块网卡的MAC地址呢？
    回答： 是由一种ARP协议，可以解决这个问题。后面介绍，以太网的数据包必须知道接收方的MAC地址，然后才能发送。

    * 其次：就有有了MAC地址，系统怎么样才能把数据包准确送到接收方呢？
    回答： 是以太网采用了一种很原始的方式，它不是把数据包准确送到接收方，而是向本网络内的所有计算机发送，让每台计算机自己判断，是否为接收方。这种发送方式就叫做"广播"。

    ![此处输入图片的描述][4]

    有了数据包的定义、网卡的MAC地址、广播的发送方式，链接层就可以在多台计算机之间传送数据了。

### 网络层

1. 网络层的由来
    以太网协议，依靠MAC地址发送数据。理论上，单单依靠MAC地址，上海的网卡就可以找到洛杉矶的网卡了。但是，这样有一个重大的缺点。以太网采用广播方式发送数据包，所有成员人手一包，不仅效率低，而且局限在发送者所在的子网络。也就是说，如果两台计算机不在同一个子网络，广播是传不过去的。这种设计是合理的，否则互联网上每一台计算机都会收到包，那会引起灾难。

    因此，必须找到一种方法，能够区分哪些MAC地址属于同一个子网络，哪些不是。如果是同一个子网络，就采用广播方式发送，否则就采用路由方式发送。

    这就导致了网络层的出现，它的作用是引进一套新的地址，使得我们能够区分不同的计算机是否属于同一个网络。这套地址就叫做网络地址，简称网址。

    于是，网络层出现之后，每台计算机有了两种地址，一种是MAC地址，另一种是网络地址。两种地址之间没有任何联系。MAC地址是绑定在网卡上的，网络地址则是管理员分配的，它们只是随机组合在一起。

    网络地址帮助我们确定计算机所在的子网络，MAC地址则将数据包发送到子网络的目标网卡。因此，从逻辑上可以推断，必定是先处理网络地址，然后处理MAC地址。

2. IP协议
    规定网络地址的协议，叫做IP协议。它所定义的地址，就被称为IP地址。目前，广泛采用的IP协议是IP协议第四版，简称IPv4。IPv4规定，网络地址由32个二进制位组成。

    ![此处输入图片的描述][5]

    习惯上，我们用分成四段的十进制数表示IP地址，从 `0.0.0.0` 到 `255.255.255.255`.

    互联网上的每一台计算机，都会分配到一个IP地址，这个地址分成两个部分，前一部分代表网络，后一部分代表主机。比如，IP地址 `172.16.254.1`，这是一个32位的地址，假定它的网络部分是前24为，那么主机部分就是后8位。处于同一个子网络的计算机，它们IP地址的网络部分必定是相同的。

    但是，问题在于单单的IP地址，我们无法判断网络部分，还是以 `172.16.254.1` 为例，它的网络部分，到底是前24位，还是前16位，甚至前26位，从IP地址上是看不出来的。

    那么，怎么样才能从IP地址，判断两台计算机是否属于同一个子网络呢？这就要用到另一个参数 "子网掩码"。

    所谓子网掩码，就是表示子网络特征的一个参数，它在形式上等同与IP地址，也就是一个32位的二进制数，它的网络部分全部位1，主机部分全部为0。比如，IP地址172.16.254.1，如果已知网络部分是前24位，主机部分是后8位，那么子网络掩码就是11111111.11111111.11111111.00000000，写成十进制就是255.255.255.0。

    知道子网掩码，我们就能判断，任意两个IP地址是否处在同一个子网络。方法是将两个IP地址与子网掩码分别进行AND运算，然后比较结果是否相同，如果是的话，就表明它们在同一个子网络中，否则就不是。

    比如，已知IP地址172.16.254.1和172.16.254.233的子网掩码都是255.255.255.0，请问它们是否在同一个子网络？两者与子网掩码分别进行AND运算，结果都是172.16.254.0，因此它们在同一个子网络。

    总结，IP协议的主要作用有两个，一个是为每一台计算机分配IP地址，另一个是确定哪些地址在同一个子网络。

3. IP数据包
    根据IP协议发送的数据，就叫做IP数据包，不难想象，其中必定包含IP地址信息，但是前面说过，以太网的数据包只包含MAC地址，并没有IP地址的栏位，那么是否需要修改数据定义，再添加一个栏位呢？

    回答是不需要，我们可以将IP数据包直接放进以太网数据包的数据部分，因此完全不用修改以太网的规格。这就是网络分层结构的好处：上层的变动完全不涉及下层的结构。

    具体来说，IP数据包也分成标头和数据两部分

    ![此处输入图片的描述][6]

    标头部分只要包括版本、长度、IP地址等信息，数据部分则是IP数据包的具体内容。

    IP数据包的标头部分的长度为20到60字节，整个数据包的总长度最大为65535字节，因此，理论上，一个IP数据包的数据部分，最长为65515字节。前面说过，以太网数据包的数据部分，最长只有1500字节。因此如果IP数据包超过1500字节，它就需要分割成几个以太网数据包，分开发送了。

4. ARP协议
    因为IP数据包是放在以太网数据包里发送的，所以我们必须同时知道两个地址，MAC地址和IP地址。通常情况下，对方的IP地址是已知的（后文解释），但是我们不知道它的MAC地址。所以，我们需要一种机制，能够从IP地址得到MAC地址。

    这里又可以分成两种情况

    * 如果两台主机不在同一个子网络，那么事实上没有办法得到对方的MAC地址，只能把数据包传送到两个子网络连接处的网关，让网关去处理。
    * 如果两台主机在一个子网络，那么我们可以用ARP协议，得到对方的MAC地址。ARP协议也是发出一个数据包（包含在以太网的数据包中），其中包含它所要查询主机的IP地址，在对方的MAC地址这一栏，填的是 `FF:FF:FF:FF:FF:FF`, 表示这是一个广播地址。它所在子网络的每一台主机，都会收到这个数据包，从中取出IP地址，与自身的IP地址进行比较，如果两者相同，都做出回复，想对方报告自己的MAC地址，否者丢弃这个包。

    总之，有了ARP协议之后，我们就可以得到同一个子网络内的主机MAC地址，可以将数据包发送到任意一台主机之上了。

### 传输层

1. 传输层的由来
    有了MAC地址和IP地址，我们已经可以在互联网上任意两台主机上建立通信了。接下来的问题是，同一台主机上有许多的程序都需要用到网络，比如，你一边浏览网页，一边与朋友在线聊天。当一个数据包从互联网上发来的时候，你怎么知道，它是表示网页的内容，还是表示在线聊天的内容？

    也就是说，我们还需要一个参数，表示这个数据包到底供哪个程序使用，这个参数就叫做端口，它其实是每一个使用网卡的程序的编号。每个数据包都发到主机的特定端口，所以不同的程序就能取到自己所需要的数据。

    端口是0-65535之间的一个整数，正好16个二进制位，0-1023端口被系统占用，用户只能选用大于1023的端口。不管是浏览网页还是在线聊天，应用程序会随机选用一个端口，然后与服务器的相应端口联系。

    传输层的功能，就是建立端口到端口的通信。相比之下，网络层的功能是建立主机到主机的通信。只要确定主机和端口，我们就能实现程序之间的交流。因此，Unix系统就把主机+端口，叫做套接字socket。有了它，就可以进行网络应用程序的开发了。

2. UDP协议
    现在，我们必须在数据包中加入端口信息，这就需要新的协议。最简单的实现叫做UDP协议，它的格式几乎就是在数据前面，加上端口号。

    UDP数据包，也是由"标头"和"数据"两部分组成，标头部分定义了发出端口和接收端口。数据部分就是具体的内容。然后把整个UDP的数据包放入IP数据包的数据部分。UDP数据包非常简单，标头部分一共就只有8个字节，总长度不超过65535，正好放进一个IP数据包。

    ![此处输入图片的描述][7]

3. TCP协议
    UDP协议的优点是比较简单，容易实现，但是缺点是可靠性较差，一旦数据包发出，无法知道对方是否收到。为了解决这个问题，提高网络可靠性，TCP协议就诞生了。这个协议非常复杂，但可以近似地认为，它就是有确认机制的UDP协议，每发出一个数据包，都要求确认。如果有一个数据包遗失，就收不到确认，发出放就知道有必要重发这个数据包了。

    因此，TCP协议能够确保数据不会遗失，它的缺点是过程复杂，实现困难，消耗较多资源。

    TCO数据包也是嵌在IP数据包的数据部分，TCP数据包没有长度限制，理论上可以无限长，但是为了保证网络的效率，一般长度不会超过IP数据包的长度，以确保单个TCP数据包不必再分割。

### 应用层

应用程序收到"传输层"的数据，接下来就要进行解读。由于互联网是开放架构，数据来源五花八门，必须事先规定好格式，否则根本无法解读。"应用层"的作用，就是规定应用程序的数据格式。

举例来说，TCP协议可以为各种各样的程序传递数据，比如Email、WWW、FTP等等。那么，必须有不同协议规定电子邮件、网页、FTP数据的格式，这些应用程序协议就构成了"应用层"。这是最高的一层，直接面对用户。它的数据就放在TCP数据包的"数据"部分。


  [1]: https://s1.ax1x.com/2018/06/18/Cx7G7R.md.jpg
  [2]: http://www.52im.net/data/attachment/forum/201710/09/150632r66n62l8s317shh1.png
  [3]: http://www.52im.net/data/attachment/forum/201710/09/150749l7mzsvf9zoc9ku88.png
  [4]: http://www.52im.net/data/attachment/forum/201710/09/150918zvus5vkv75vveotv.png
  [5]: http://www.52im.net/data/attachment/forum/201710/09/151225lt2n0hilb24sbvxo.png
  [6]: http://www.52im.net/data/attachment/forum/201710/09/151348e5cwv7f9w2oowhch.png
  [7]: http://www.52im.net/data/attachment/forum/201710/09/151838az43w44k43db03cg.png