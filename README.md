基于51单片机（STC89C52）和指纹识别模块（AS608）的指纹锁项目的全部软硬件资料
This is a fingerprint lock project based on 51-MCU and AS608, which can be installed on most doors without any conflict.
文件（夹）说明：
1、Board_Layout:内含一个*.rst文件，是电路布局图，需要用lochmaster软件打开。读者可按照该电路图焊接电路板。如果没有安装lochmaster软件，可直接查看另外两个PDF文件；
2、datasheet：内含两个PDF文件和一个文件夹。L298N芯片为电机驱动芯片，由于单片机驱动能力有限，L298N芯片可以增大驱动能力，为减速电机提供足够的电流；STC89C52为宏晶科技公司生产的单片机芯片，本项目中用作主控芯片；AS608datasheet文件夹内有两个PDF文件，介绍AS608模块与单片机的通讯方式；
3、images：这是我首次制作的电路实物图，布局和Board_Layout文件夹内资料并不一致，仅供参考；
4、keil_project:这是用keil uvision3软件开发的指纹锁软件项目，已压缩成zip格式；
5、manifest：这是制作整个电路系统所需的元件清单，是用Excel写的*.xlsx格式；
6、schematic：*.ms12文件是用Multisim绘制的原理图（由于Multisim中缺少指纹模块元件，暂不可仿真分析，此处仅供指导电路板的焊接）。如果没有安装Multisim，可直接查看另一个*.PNG文件；*.DSN文件是用Proteus ISIS软件绘制的L298N芯片仿真测试电路，供参考；
7、src:其中包含C语言源文件（fingerprint_lock.c）、源文件备份文件(SourceCode.txt)以及编译好的十六进制文件（FingerprintLock.hex）

使用说明：
液晶显示主界面有3个功能：search finger（搜索指纹），add（添加指纹），delete（删除全部指纹），还有一个星号表示当前预选中的功能；
单片机P2口连接了3个按钮开关：分别为KEY_DOWN=P2^4;KEY_OK=P2^2;KEY_CANCEL=P2^0;
1、添加指纹：按KEY_DOWN键，将星号调到“Add”前，按KEY_OK键，屏幕中将显示指纹即将存入的ID号，如果你希望使用该ID号，按KEY_OK键，否则按KEY_DOWN切换ID号，再按KEY_OK键，此后你可以将手指放到指纹识别窗口上，指纹将被读入两次，每读取成功一次蜂鸣器会响一声，两次读取成功后，ID号自动切换到下一个，你可以继续录入指纹或者按KEY_CANCEL键取消；
2、搜索指纹：要执行开锁操作时，按KEY_OK键（为了能在门外操作，指纹识别模块上另有一个按钮开关通过小孔和门内的KEY_OK键并联），将手指放到指纹识别窗口上，若识别成功，单片机将控制减速电机开锁，同时蜂鸣器响1声，若识别失败，蜂鸣器响3声，你可以将手指拿开再重新放上去，将会被自动识别；
3、删除指纹：按KEY_DOWN键，将星号调到“delete”前，按KEY_OK键，屏幕将显示询问是否执行删除操作，按KEY_OK键确认删除，按KEY_CANCEL键取消；
补充：
1、如果当前指纹模块内存储了至少一个指纹模板，你在执行添加指纹或删除指纹操作时将需要“主人”授权。例如，我已经添加了一个指纹，ID号为000，现在需要再添加一个指纹，需要按KEY_DOWN键，将星号调到“Add”前，按KEY_OK键，这时需要匹配指纹，我需要将000号指纹放到指纹模块上，若识别成功，则可以执行后续操作添加指纹；
2、在执行任何操作时可以按KEY_CANCEL键退回到主界面；
3、本电路系统使用较为简单，但如果要彻底熟悉相关操作建议查看源代码；

如果你要使用本方案制作一个指纹锁，只需要按以下步骤操作：
1、基于Board_Layout文件夹内资料，参考schematic原理图以及images实物图焊接电路板，作为整个系统的控制部分；
2、直接使用stc-isp软件将*.hex文件下载到单片机中（或者你也可以用keil uvision3软件打开上面的keil_project项目抑或使用源码自行创建项目编译得到*.hex文件，实际上，由于不同门锁的结构，开锁关锁所需的时间可能与我的不同，你将有必要自行调整源代码中相关参数，这在代码注释中有相关说明）；
3、将AS608指纹模块连接到单片机相应管脚上，将L298N驱动芯片输出管脚与减速电机相连，减速电机通过绳子牵引门把手开锁；
补充：
1、程序下载时只需连接VCC,GNC,TX,RX四个管脚；
2、指纹识别模块VCC接3.3V，GND与单片机共地，AS608的TX接单片机RX，AS608的RX接单片机TX；

补充说明：
1、LCD1602液晶显示屏的背光灯耗电较为严重，因此在背光灯供电管脚之间添加开关控制是否打开背光灯，但我强烈建议默认状态为关闭；
2、本项目中供电方式为使用锂电池向电路系统供电，同时通过充电器向电池充电，也就是说电池处于边充边放的状态。这样可以保证在家庭电路供电异常的情况下，指纹锁仍能工作一段时间。如果你认为这种情况发生的概率极低，同时希望降低制作成本，可以不使用锂电池，而通过DC接口的充电器直接给电路供电；
3、普通电机所能输出的力矩有限，本系统中采用了减速电机，增大输出力矩，并在机械锁上连接省力杠杆，增加驱动力。两者之间使用绳子软连接。实际应用时可根据机械锁开锁方式及难易程度进行相应调整；
4、到目前为止，本项目已实际使用3月有余。依实际使用来看，电路在绝大多数时候能够正常工作，但也存在因接触故障而不能开锁的情况。因此建议不要完全依赖于电子锁，出门还是要带上钥匙（虽然大多数情况下你都不用拿出来），由于本项目的实现方式，机械锁原来的功能丝毫不受影响；
5、使用指纹解锁的主要优势是便捷，但不可避免的会降低安全性（如果有“不速之客”，将只需要破解机械锁或指纹锁任意一样即可）。依照指纹模块用户手册相关数据，该指纹识别模块的认假率低于0.001%,拒真率低于1%，这种概率是可以被接受的。但同时实测发现，拒真率要大于1%，分析原因可能是因为录入指纹模板时每个ID号只录了两次，再次识别时存在角度、力度、部位等变量影响，导致识别率下降。解决方案是将多个ID号录入同一个指纹（AS608模块可以录入300枚指纹），录入时使用不同的角度、力度、部位，尽可能录入更多信息。假如实际拒真率高达10%，而你将同一枚指纹录入了10次，并假设是否识别失败是独立随机的，那么拒真率将下降为百亿分之一，这是一个极小的概率。当然，如果你的指纹受损（例如沾了水等），识别失败将不能认为是拒真，你将始终难以识别成功。
