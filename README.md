# AnalysisAVP

Analysis of audio and video protocols

## 音视频基本原理

### 视频录制原理

![音视频录制原理](./images/音视频录制原理.png)

### 音视频播放原理

![音视频播放原理](./images/音视频播放原理.png)

## 音视频媒体格式

### 视频主要概念

- **视频码率**：**kb/s**，是指视频文件在单位时间内使用的数据流量，也叫码流率。码率越大，说明单位时间内取样率越大，数据流精度就越高。
- **视频帧率**：**fps**，通常说一个视频的25帧，指的就是这个视频帧率，即1秒中会显示25帧。帧率越高，给人的视觉就越流畅。
- **视频分辨率**：分辨率就是我们常说的640x480分辨率、1920x1080分辨率，分辨率影响视频图像的大小。
- **I**帧（**Intra coded frames**）：
- I帧不需要参考其他画面而生成，解码时仅靠自己就重构完整图像；
  
- I帧图像采用帧内编码方式；
  
- I帧所占数据的信息量比较大；
  
- I帧图像是周期性出现在图像序列中的，出现频率可由编码器选择；
  
- I帧是P帧和B帧的参考帧（其质量直接影响到同组中以后各帧的质量）；
  
- **IDR**帧是帧组GOP的基础帧（第一个（I）帧），在一组GOP中只有一个IDR帧；
  
- I帧不需要考虑运动矢量；
- **P**帧（**Predicted frames**）根据本帧与相邻的前一帧（I帧或P帧）的不同点来压缩本帧数据，同时利用了空间和时间上的相关性。
- P帧属于前向预测的帧间编码。它需要参考前面最靠近它的**I**帧或**P**帧来解码。
- **B**帧（**Bi-directional predicted frames**）采用双向时间预测，可以大大提高压缩倍数。

### 音频主要概念

- **采样频率**：每秒钟采样的点的个数。常用的采样频率有：

    |      采样率      |   常见用途    |
    | :--------------: | :-----------: |
    |  22000（22kHz）  |   无线广播    |
    | 44100（44.1kHz） |    CD音质     |
    |  48000（48kHz）  | 数字电视，DVD |
    |  96000（96kHz）  | 蓝光，高清DVD |
    | 192000（192kHz） | 蓝光，高清DVD |

- **采样精度（采样深度）**：每个“样本点”的大小，常用的大小为8bit， 16bit，24bit。

- **通道数**：单声道，双声道，四声道，5.1声道。

- **比特率**：每秒传输的bit数，单位为：bps（Bit Per Second）。间接衡量声音质量的一个标准。没有压缩的音频数据的比特率 = 采样频率 * 采样精度 * 通道数。

- **码率**：压缩后的音频数据的比特率。常见的码率：

    |    码率     |   常见用途   |
    | :---------: | :----------: |
    |   96kbps    |    FM质量    |
    | 128-160kbps | 一般质量音频 |
    |   192kbps   |    CD质量    |
    | 256-320Kbps |  高质量音频  |

    - 码率越大，压缩效率越低，音质越好，压缩后数据越大。

    - 码率 = 音频文件大小 / 时长。

- **帧**：每次编码的采样单元数，比如MP3通常是1152个采样点作为一个编码单元，AAC通常是1024个采样点作为一个编码单元。

- **帧长**：可以指每帧播放持续的时间。每帧持续时间（秒）= 每帧采样点数 / 采样频率（HZ）。比如：MP3 48k，1152个采样点，每帧则为24毫秒，1152/48000 = 0.024秒 = 24毫秒；也可以指压缩后每帧的数据长度。

- **交错模式**：数字音频信号存储的方式。数据以连续帧的方式存放，即首先记录帧1的左声道样本和右声道样本，再开始帧2的记录...

- **非交错模式**：首先记录的是一个周期内所有帧的左声道样本，再记录所有右声道样本。

- 数字音频压缩编码在保证信号在听觉方面不产生失真的前提下，对音频数据信号进行尽可能大的压缩，降低数据量。数字音频压缩编码采取去除声音信号中冗余成分的方法来实现。所谓冗余成分指的是音频中不能被人耳感知到的信号，它们对确定声音的音色，音调等信息没有任何的帮助。

### 封装概念

- **封装格式**（也叫容器）就是将已经编码压缩好的视频流、音频流及字幕按照一定的方案放到一个文件中，便于播放软件播放。

- **一般来说，视频文件的后缀名就是它的封装格式。**

- **封装的格式不一样，后缀名也就不一样。**

### 音视频同步

- **DTS**（**Decoding Time Stamp**）：即解码时间戳，这个时间戳的意义在于告诉播放器该在什么时候解码这一帧的数据。

- **PTS**（**Presentation Time Stamp**）：即显示时间戳，这个时间戳用来告诉播放器该在什么时候显示这一帧的数据。

- **音视频同步方式：**

    |         名称          |           实现           |
    | :-------------------: | :----------------------: |
    |     Audio Master      |      同步视频到音频      |
    |     Video Master      |      同步音频到视频      |
    | External Clock Master | 同步音频和视频到外部时钟 |

    - 一般情况下选择：Audio Master > External Clock Master > Video Master

### YUV格式

- 主要用于视频信号的压缩、传输和存储，和向后相容老式黑白电视。
- 其中**Y**表示明亮度（Luminance或Luma），**也称灰阶值**。
- **U**和**V**表示的则是色度（Chrominance或Chroma），作用是描述影像**色彩**及**饱和度**，用于指定像素的颜色。
- 对于planar的YUV格式，先连续存储所有像素点的Y，紧接着存储所有像素点的U，随后是所有像素点的V。
- 对于packed的YUV格式，每个像素点的Y，U，V是连续存储的。
- **libyuv**，Google开源的实现各种YUV与RGB间相互转换、旋转、缩放的高效库。
- YUV4:4:4采样，每一个Y对应一组UV分量。
- YUV4:2:2采样，每两个Y共用一组UV分量。 
- YUV4:2:0采样，每四个Y共用一组UV分量。

### H264（新一代视频压缩编码标准）

- H264采⽤了16*16的分块⼤⼩对视频帧图像进⾏相似⽐较和压缩编码。

- H264使⽤帧内压缩和帧间压缩的⽅式提⾼编码压缩率。

- H264采⽤了独特的I帧、P帧和B帧策略来实现连续帧之间的压缩。

    ![h264层次](./images/h264层次.png)

- H264将视频分为连续的帧进⾏传输，在连续的帧之间使⽤I帧、P帧和B帧。同时对于帧内⽽⾔，将图像分为⽚、宏块和子块进⾏传输；通过这个过程实现对视频⽂件的压缩包装。

- ⼀个序列的第⼀个图像叫做IDR图像（⽴即刷新图像），IDR图像都是I帧图像。I帧和IDR帧都使⽤帧内预测。I帧不⽤参考任何帧，但是之后的P帧和B帧是有可能参考这个I帧之前的帧的。IDR就不允许这样，其核⼼作⽤是为了解码的重同步，当解码器解码到IDR图像时，⽴即将参考帧队列清空，将已解码的数据全部输出或抛弃，重新查找参数集，开始⼀个新的序列。这样，如果前⼀个序列出现重⼤错误，在这⾥可以获得重新同步的机会。IDR图像之后的图像永远不会使⽤IDR之前的图像的数据来解码。

    ![gop](./images/gop.png)

- SPS：序列参数集。SPS中保存了⼀组**编码视频序列（Coded video sequence）**的全局参数。

- PPS：图像参数集。对应的是⼀个序列中某⼀幅图像或者某⼏幅图像的参数。

- I帧：帧内编码帧，可独⽴解码⽣成完整的图⽚。

- P帧： 前向预测编码帧，需要参考其前⾯的⼀个I帧或者B帧来⽣成⼀张完整的图⽚。

- B帧：双向预测内插编码帧，则要参考其前⼀个I或者P帧及其后⾯的⼀个P帧来⽣成⼀张完整的图⽚。  

- 发I帧之前，⾄少要发⼀次SPS和PPS。 

    ![H.264码流分层结构](./images/H.264码流分层结构.png)

- NAL层，视频数据网络抽象层（Network Abstraction Layer）

- VCL层，视频数据编码层（Video Coding Layer）

    ![RBSP与SODB](./images/RBSP与SODB.png)

- SODB，数据位串（String Of Data Bits）。原始数据比特流，长度不一定是8的倍数，故需要补齐。由VCL层产生。

- RBSP，原始字节序列负载（Raw Byte Sequence Playload）。SODB + trailing bits，算法是如果SODB最后一个字节不对齐，则补1和多个0。

    ![NALU](./images/NALU.png)

    ![NAL_Header](./images/NAL_Header.png)

- NALU，NAL单元。NAL Header（1B）+ RBSP。

- H.264标准指出，当数据流是储存在介质上时，在每个NALU前添加起始码0x000001或0x00000001，⽤来指示⼀个NALU的起始和终⽌位置 。

- 3字节的0x000001只有⼀种场合下使⽤，就是⼀个完整的帧被编为多个slice（⽚）的时候，包含这些slice的NALU使⽤3字节起始码。其余场合都是4字节0x00000001的。  

- H264有两种封装
    - ⼀种是annexb模式，传统模式，有startcode，SPS和PPS是在ES中。
    - ⼀种是mp4模式，⼀般mp4 mkv都是mp4模式，没有startcode，SPS和PPS以及其它信息被封装在container中，每⼀个frame前⾯4个字节是这个frame的⻓度。很多解码器只⽀持annexb这种模式，因此需要将mp4做转换。在ffmpeg中⽤h264_mp4toannexb_filter可以做转换。
    
- 通过提⾼GOP值来提⾼图像质量是有限度的，在遇到场景切换的情况时，H.264编码器会⾃动强制插⼊⼀个I帧，此时实际的GOP值被缩短了。另⼀⽅⾯，在⼀个GOP中，P、B帧是由I帧预测得到的，当I帧的图像质量⽐较差时，会影响到⼀个GOP中后续P、B帧的图像质量，直到下⼀个GOP开始才有可能得以恢复，所以GOP值也不宜设置过⼤。  

- 由于P、B帧的复杂度⼤于I帧，所以过多的P、B帧会影响编码效率，使编码效率降低。另外，过⻓的GOP还会影响seek操作的响应速度，由于P、B帧是由前⾯的I或P帧预测得到的，所以seek操作需要直接定位，解码某⼀个P或B帧时，需要先解码得到本GOP内的I帧及之前的N个预测帧才可以。GOP值越⻓，需要解码的预测帧就越多，seek响应的时间也越⻓。  

- P帧特点：
    1) P帧是I帧后⾯相隔1~2帧的编码帧；
    2) P帧采⽤运动补偿的⽅法传送它与前⾯的I或P帧的差值及运动⽮量（预测误差）；
    3) 解码时必须将I帧中的预测值与预测误差求和后才能重构完整的P帧图像；
    4) P帧属于前向预测的帧间编码。它只参考前⾯最靠近它的I帧或P帧；
    5) P帧可以是其后⾯P帧的参考帧，也可以是其前后的B帧的参考帧；
    6) **由于P帧是参考帧，它可能造成解码错误的扩散；**
    7) 由于是差值传送，P帧的压缩⽐较⾼。  
    
- B帧特点：
    1）B帧是由前⾯的I或P帧和后⾯的P帧来进⾏预测的；
    2）B帧传送的是它与前⾯的I或P帧和后⾯的P帧之间的预测误差及运动⽮量；
    3）B帧是双向预测编码帧；
    4）B帧压缩⽐最⾼，因为它只反映两参考帧间运动主体的变化情况，预测⽐较准确；
    5）**B帧不是参考帧，不会造成解码错误的扩散。**
    
- H264分析代码：[H264分析代码](./analysis/h264/)

### AAC（高级音频编码）

- ADIF，音频数据交换格式（Audio Data Interchange Format）。这种格式的特征是可以确定的找到这个音频数据的开始，不需进行在音频数据流中间开始的解码，即它的解码必须在明确定义的开始处进行。故这种格式常用在磁盘文件中。

    ![ADIF](./images/ADIF.png)

    ![AAC](./images/AAC.png)

    ![ADTS](./images/ADTS.png)

- ADTS，音频数据传输流（Audio Data Transport Stream）。这种格式的特征是它是一个有同步字的比特流，解码可以在这个流中任何位置开始。它的特征类似于mp3数据流格式。

    ![ADTS的固定头信息](./images/ADTS的固定头信息.png)

    ![ADTS的可变头信息](./images/ADTS的可变头信息.png)

    - 每⼀帧的ADTS的头⽂件都包含了⾳频的采样率，声道，帧⻓度等信息，这样解码器才能解析读取。⼀般情况下ADTS的头信息都是7个字节，分为2部分：

        - adts_fixed_header()；

        - adts_variable_header()；

        - 其⼀为固定头信息，紧接着是可变头信息。固定头信息中的数据每⼀帧都相同，⽽可变头信息则在帧与帧之间可变。

    - syncword：同步头，总是0xFFF，all bits must be 1，代表着⼀个ADTS帧的开始。
    
    - ID：MPEG标识符，0标识MPEG-4，1标识MPEG-2。
    
    - Layer：always: '00'。
    
    - protection_absent：表示是否误码校验。Warning, set to 1 if there is no CRC and 0 if there is CRC。
    
    - profile：表示使⽤哪个级别的AAC，如01 Low Complexity(LC)--- AAC LC。有些芯⽚只⽀持AAC LC。profile的值等于 Audio Object Type的值减1。profile = MPEG-4 Audio Object Type - 1。
    
        ![audio object type](images/audio_object_type.png)
    
    - sampling_frequency_index：表示使⽤的采样率下标，通过这个下标在Sampling Frequencies[ ]数组中查找得知采样率的值。  
    
        ![Sampling Frequencies](./images/Sampling_Frequencies.png)
    
    - channel_configuration： 表示声道数，⽐如2表示⽴体声双声道。
    
        ![channel configuration](./images/channel_configuration.png)
    
    - frame_length：⼀个ADTS帧的⻓度包括ADTS头和AAC原始流：
        - frame length, this value must include 7 or 9 bytes of header length；
        - aac_frame_length = (protection_absent == 1 ? 7 : 9) + size(AACFrame)；
        - protection_absent=0时， header length=9bytes；
        - protection_absent=1时，header length=7bytes；
    - adts_buffer_fullness：0x7FF说明是码率可变的码流。
    - number_of_raw_data_blocks_in_frame：表示ADTS帧中有number_of_raw_data_blocks_in_frame + 1个AAC原始帧。所以说number_of_raw_data_blocks_in_frame == 0 表示说ADTS帧中有⼀个AAC数据块。

- ADTS可以在任意帧解码，也就是说它每⼀帧都有头信息。ADIF只有⼀个统⼀的头，所以必须得到所有的数据后解码。

- AAC分析代码：[AAC分析代码](./analysis/aac/)

### FLV（Flash Video）

![flv文件结构](./images/flv文件结构.png)

![flv详细文件结构](./images/flv详细文件结构.png)

- FLV（Flash Video）是Adobe公司推出的⼀种流媒体格式，由于其封装后的⾳视频⽂件体积⼩、封装简单等特点，⾮常适合于互联⽹上使⽤。⽬前主流的视频⽹站基本都⽀持FLV。  

- FLV封装格式是由⼀个⽂件头和⽂件体组成。其中，FLV body由⼀对对的（Previous Tag Size字段 + tag）组成。Previous Tag Size字段排列在Tag之前，占⽤4个字节。Previous Tag Size记录了前⾯⼀个Tag的⼤⼩，⽤于逆向读取处理。FLV header后的第⼀个Pervious Tag Size的值为0。

- FLV header由如下字段组成，其中前三个字节内容固定是*FLV*，最后4个字节内容固定是9（对*FLV*版本1来说）：

    | 字段 | 字段类型 | 字段含义 |
    | :- | :--: | :- |
    | Signature | UI8 |签名，固定为'F' (0x46)|
    | Signature | UI8 |签名，固定为'L' (0x4c)|
    | Signature | UI8 |签名，固定为'V' (0x56)|
    | Version | UI8 |版本，比如 0x01 表示 FLV 版本 1|
    | TypeFlagsReserved | UB[5] |全为0|
    | TypeFlagsAudio | UB[1] |1表示有audio tag，0表示没有|
    | TypeFlagsReserved | UB[1] |全为0|
    | TypeFlagsVideo | UB[1] |1表示有video tag，0表示没有|
    | DataOffset | UI32 |FLV header的大小，单位是字节|

- FLV file body很有规律，由一系列的*TagSize*和*Tag*组成，其中*PreviousTagSize0*总是为0；*tag*由*tag header*、*tag body*组成；对*FLV*版本1，*tag header*固定为11个字节。因此，*PreviousTagSize*（除第1个）的值为 11 + 前一个*tag*的*tag body*的大小：

    | 字段 | 字段类型 | 字段含义 |
    | :- | :-: | :- |
    | PreviousTagSize0 | UI32 |总是0|
    | Tag1 | FLVTAG |第1个tag|
    | PreviousTagSize1 | UI32 |前一个tag的大小，包括tag header|
    | Tag2 | FLVTAG |第2个tag|
    | ... | ... |...|
    | PreviousTagSizeN-1 | UI32 |第N-1个tag的大小|
    | TagN | FLVTAG |第N个tag|
    | PreviousTagSizeN | UI32 |第N个tag的大小，包含tag header|

- FLV tag由*tag header*+*tag body*组成。*tag header*如下，总共占据11个字节：
  
| 字段 | 字段类型 |字段含义|
| :- | :-: | :- |
| TagType | UI8 |tag类型：<br>8：audio<br>9：video<br>18：script data<br>其他：保留 |
| DataSize | UI24 |tag body的大小|
| Timestamp | UI24 |相对于第一个tag的时间戳（单位是毫秒）第一个tag的Timestamp为0|
| TimestampExtended | UI8 |时间戳的扩展字段，当 Timestamp 3个字节不够时，会启用这个字段，代表高8位|
| StreamID | UI24 |总是0|
| Data | 取决于根据TagType |TagType=8，则为AUDIODATA<br>TagType=9，则为VIDEODATA<br>TagType=18，则为SCRIPTDATAOBJECT|

- Tag⼀般可以分为3种类型：脚本(帧)数据类型、⾳频数据类型、视频数据。FLV数据以⼤端序进⾏存储，在解析时需要注意。

    ![flv解析过程](./images/flv解析过程.png)

- ⼀个FLV⽂件，每种类型的tag都属于⼀个流，也就是⼀个flv⽂件最多只有⼀个⾳频流，⼀个视频流，不存在多个独⽴的⾳视频流在⼀个⽂件的情况。

- flv⽂件中Timestamp和TimestampExtended拼出来的是dts。也就是解码时间。Timestamp和TimestampExtended拼出来dts单位为ms。（如果不存在B帧，当然dts等于pts）。

- Script data脚本数据就是描述视频或⾳频的信息的数据，如宽度、⾼度、时间等等，⼀个⽂件中通常只有⼀个元数据，⾳频tag和视频tag就是⾳视频信息了，采样、声道、频率，编码等信息。 

- **Script Tag Data结构（脚本类型、帧类型）**

    ![amf](./images/amf.png)

    - 该类型Tag⼜被称为MetaData Tag，存放⼀些关于FLV视频和⾳频的元信息，⽐如：duration、width、height等。通常该类型Tag会作为FLV⽂件的第⼀个tag，并且只有⼀个，跟在File Header后。

    - AMF包中第一个字节为类型标识：

        |        类型        |  值  |
        | :----------------: | :--: |
        |       Number       | 0x00 |
        |      Boolean       | 0x01 |
        |       String       | 0x02 |
        |       Object       | 0x03 |
        |     MovieClip      | 0x04 |
        |        Null        | 0x05 |
        |     Undefined      | 0x06 |
        |     Reference      | 0x07 |
        |     ECMAArray      | 0x08 |
        |     ObjectEnd      | 0x09 |
        |    StrictArray     | 0x0a |
        |        Date        | 0x0b |
        |     LongString     | 0x0c |
        |    Unsupported     | 0x0d |
        |     Recordset      | 0x0e |
        |     XMLObject      | 0x0f |
        | TypedObject(Class) | 0×10 |

* **Audio Tag Data结构（⾳频类型）**

    - *Audio tags*定义如下所示：

        | 字段 | 字段类型 |字段含义|
        | :- | :-: | :- |
        | SoundFormat | UB[4] |音频格式，重点关注 10 = AAC<br>0 = Linear PCM, platform endian<br>1 = ADPCM<br>2 = MP3<br>3 = Linear PCM, little endian<br>4 = Nellymoser 16-kHz mono<br>5 = Nellymoser 8-kHz mono<br>6 = Nellymoser<br>7 = G.711 A-law logarithmic PCM<br>8 = G.711 mu-law logarithmic PCM<br>9 = reserved<br>10 = AAC<br>11 = Speex<br>14 = MP3 8-Khz<br>15 = Device-specific sound|
        | SoundRate | UB[2] |采样率，对AAC来说，永远等于3<br>0 = 5.5-kHz<br>1 = 11-kHz<br>2 = 22-kHz<br>3 = 44-kHz|
        | SoundSize | UB[1] |采样精度，对于压缩过的音频，永远是16位<br>0 = snd8Bit<br>1 = snd16Bit|
        | SoundType | UB[1] |声道类型，对Nellymoser来说，永远是单声道；对AAC来说，永远是双声道；<br>0 = sndMono 单声道<br>1 = sndStereo 双声道|
        | SoundData | UI8[size of sound data] |如果是AAC，则为AACAUDIODATA；其他请参考规范。|
    
        - *AACAUDIODATA*

            当*SoundFormat*为10时，表示音频采AAC进行编码，此时，*SoundData*的定义如下：
    
            | 字段 | 字段类型 | 字段含义|
            | :- | :-: | :- |
            | AACPacketType | UI8 |0: AAC sequence header<br>1: AAC raw|
            | Data | UI8[n] |如果AACPacketType为0，则为AudioSpecificConfig；如果AACPacketType为1，则为AAC帧数据。|
    
        - *AudioSpecificConfig*
    
            | 字段 | 字段类型 | 字段含义 |
            | :- | :-: | :- |
            | AudioObjectType | UB[5] |编码器类型，比如2表示AAC-LC。|
            | SamplingFrequencyIndex | UB[4] | 采样率索引值，比如4表示44100。 |
            | ChannelConfiguration | UB[4] | 声道配置，比如2代表双声道，front-left，front-right。 |
            | AOT Specific Config | UB[n] ||
    
    - ⾳频Tag Data区域开始的第⼀个字节包含了⾳频数据的参数信息，第⼆个字节开始为⾳频流数据。（这两个字节属于tag的data部分，不是header部分）。第⼆个字节开始为⾳频数据（需要判断该数据是真正的⾳频数据，还是⾳频config信息）。
    
        ![aac audio data](./images/aac_audio_data.png)
    
- **Video Tag Data结构（视频类型）** 

    - *Video tags*定义如下：

        | 字段 | 字段类型 | 字段含义 |
        | :- | :-: | :- |
        | FrameType | UB[4] |重点关注1、2：<br>1: keyframe (for AVC, a seekable frame) —— 即H.264的IDR帧；<br>2: inter frame (for AVC, a non- seekable frame) —— H.264的普通I帧；<br>3: disposable inter frame (H.263 only)<br>4: generated keyframe (reserved for server use only)<br>5: video info/command frame|
        | CodecID | UB[4] |编解码器，主要关注 7（AVC）<br>1: JPEG (currently unused)<br>2: Sorenson H.263<br>3: Screen video<br>4: On2 VP6<br>5: On2 VP6 with alpha channel<br>6: Screen video version 2<br>7: AVC|
        | VideoData | 取决于CodecID |实际的媒体类型，主要关注 7:AVCVIDEOPACKE<br>2: H263VIDEOPACKET<br>3: SCREENVIDEOPACKET<br>4: VP6FLVVIDEOPACKET<br>5: VP6FLVALPHAVIDEOPACKET<br>6: SCREENV2VIDEOPACKET<br>7: AVCVIDEOPACKE|

        - *AVCVIDEOPACKE*当*CodecID*为7时，*VideoData*为 *AVCVIDEOPACKE*，也即*H.264*媒体数据。
        *AVCVIDEOPACKE*的定义如下：

        | 字段 | 字段类型 | 字段含义 |
        | :- | :-: | :- |
        | AVCPacketType | UI8 | 0: AVC sequence header<br>1: AVC NALU<br>2: AVC end of sequence |
        | CompositionTime | SI24 | 如果AVCPacketType=1，则为时间cts偏移量；否则，为0。当B帧的存在时，视频解码呈现过程中，dts、pts可能不同，cts的计算公式为 pts - dts/90，单位为毫秒；如果B帧不存在，则cts固定为0。 |
        | Data | UI8[n] | 1、如果如果AVCPacketType=0，则为AVCDecoderConfigurationRecord，H.264 视频解码所需要的参数集（SPS、PPS）<br>2、如果AVCPacketType=1，则为NALU（一个或多个）<br>3、如果AVCPacketType=2，则为空 |

    - 视频Tag Data开始的第⼀个字节包含视频数据的参数信息，第⼆个字节开始为视频流数据。
    
    - CompositionTime 表示PTS相对于DTS的偏移值， 在每个视频tag的第14-16字节。显示时间(pts) = 解码时间（tag的第5-8字节） + CompositionTime，CompositionTime的单位也是ms。
    
    - FLV分析代码：[FLV分析代码](./analysis/flv/)

## 音视频框架

### FFmpeg

- 编译

    ```shell
    # 安装环境依赖
    sudo apt-get update 
    sudo apt-get -y install autoconf automake build-essential cmake git-core libass-dev libfreetype6-dev libsdl2-dev libtool libva-dev libvdpau-dev libvorbis-dev libxcb1-dev libxcb-shm0-dev libxcb-xfixes0-dev pkg-config texinfo wget zlib1g-dev
    # 安装依赖库
    sudo apt-get -y install nasm yasm libx264-dev libx265-dev libnuma-dev libvpx-dev libfdk-aac-dev libmp3lame-dev libopus-dev
    # 下载ffmpeg源码
    wget https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2
    # 编译
    tar xjvf ffmpeg-snapshot.tar.bz2
    cd ffmpeg
    ./configure --prefix="$PWD/ffmpeg_build" --pkg-config-flags="--static" --extra-cflags="-I$PWD/ffmpeg_build/include" --extra-ldflags="-L$PWD/ffmpeg_build/lib" --extra-libs="-lpthread -lm" --bindir="$PWD/ffmpeg_build/bin" --enable-gpl --enable-libass --enable-libfdk-aac --enable-libfreetype --enable-libmp3lame --enable-libopus --enable-libvorbis --enable-libvpx --enable-libx264 --enable-libx265 --enable-nonfree
    make -j 8
    make install
    ```
    
- 推流命令

    ```shell
    # h264推流
    ffmpeg -re -i /mnt/e/Code/AnalysisAVP/media/gx.flv -vcodec h264 -acodec aac -f rtsp -rtsp_transport tcp rtsp://172.22.192.1/live/test
    # h265推流
    ffmpeg -re -i /mnt/e/Code/AnalysisAVP/media/gx.flv -vcodec hevc -acodec aac -f rtsp -rtsp_transport tcp rtsp://172.22.192.1/live/test
    # copy
    ffmpeg -re -i /mnt/e/Code/AnalysisAVP/media/gx.flv -vcodec copy -acodec copy -f flv -y rtmp://172.22.192.1/live/test
    ```
- 拉流命令

    ```shell
    ffplay rtmp://172.22.192.1/live/test
    ffplay http://172.22.192.1/live/test.m3u8
    ffplay rtsp://172.22.192.1/live/test
    ```

- 数据结构

    - AVFormatContext

        ```C++
        /**
         * Format I/O context.
         * New fields can be added to the end with minor version bumps.
         * Removal, reordering and changes to existing fields require a major
         * version bump.
         * sizeof(AVFormatContext) must not be used outside libav*, use
         * avformat_alloc_context() to create an AVFormatContext.
         *
         * Fields can be accessed through AVOptions (av_opt*),
         * the name string used matches the associated command line parameter name and
         * can be found in libavformat/options_table.h.
         * The AVOption/command line parameter names differ in some cases from the C
         * structure field names for historic reasons or brevity.
         */
        typedef struct AVFormatContext {
            /**
             * A class for logging and @ref avoptions. Set by avformat_alloc_context().
             * Exports (de)muxer private options if they exist.
             */
            const AVClass *av_class;
        
            /**
             * The input container format.
             *
             * Demuxing only, set by avformat_open_input().
             */
            ff_const59 struct AVInputFormat *iformat;
        
            /**
             * The output container format.
             *
             * Muxing only, must be set by the caller before avformat_write_header().
             */
            ff_const59 struct AVOutputFormat *oformat;
        
            /**
             * Format private data. This is an AVOptions-enabled struct
             * if and only if iformat/oformat.priv_class is not NULL.
             *
             * - muxing: set by avformat_write_header()
             * - demuxing: set by avformat_open_input()
             */
            void *priv_data;
        
            /**
             * I/O context.
             *
             * - demuxing: either set by the user before avformat_open_input() (then
             *             the user must close it manually) or set by avformat_open_input().
             * - muxing: set by the user before avformat_write_header(). The caller must
             *           take care of closing / freeing the IO context.
             *
             * Do NOT set this field if AVFMT_NOFILE flag is set in
             * iformat/oformat.flags. In such a case, the (de)muxer will handle
             * I/O in some other way and this field will be NULL.
             */
            AVIOContext *pb;
        
            /* stream info */
            /**
             * Flags signalling stream properties. A combination of AVFMTCTX_*.
             * Set by libavformat.
             */
            int ctx_flags;
        
            /**
             * Number of elements in AVFormatContext.streams.
             *
             * Set by avformat_new_stream(), must not be modified by any other code.
             */
            unsigned int nb_streams;
            /**
             * A list of all streams in the file. New streams are created with
             * avformat_new_stream().
             *
             * - demuxing: streams are created by libavformat in avformat_open_input().
             *             If AVFMTCTX_NOHEADER is set in ctx_flags, then new streams may also
             *             appear in av_read_frame().
             * - muxing: streams are created by the user before avformat_write_header().
             *
             * Freed by libavformat in avformat_free_context().
             */
            AVStream **streams;
        
        #if FF_API_FORMAT_FILENAME
            /**
             * input or output filename
             *
             * - demuxing: set by avformat_open_input()
             * - muxing: may be set by the caller before avformat_write_header()
             *
             * @deprecated Use url instead.
             */
            attribute_deprecated
            char filename[1024];
        #endif
        
            /**
             * input or output URL. Unlike the old filename field, this field has no
             * length restriction.
             *
             * - demuxing: set by avformat_open_input(), initialized to an empty
             *             string if url parameter was NULL in avformat_open_input().
             * - muxing: may be set by the caller before calling avformat_write_header()
             *           (or avformat_init_output() if that is called first) to a string
             *           which is freeable by av_free(). Set to an empty string if it
             *           was NULL in avformat_init_output().
             *
             * Freed by libavformat in avformat_free_context().
             */
            char *url;
        
            /**
             * Position of the first frame of the component, in
             * AV_TIME_BASE fractional seconds. NEVER set this value directly:
             * It is deduced from the AVStream values.
             *
             * Demuxing only, set by libavformat.
             */
            int64_t start_time;
        
            /**
             * Duration of the stream, in AV_TIME_BASE fractional
             * seconds. Only set this value if you know none of the individual stream
             * durations and also do not set any of them. This is deduced from the
             * AVStream values if not set.
             *
             * Demuxing only, set by libavformat.
             */
            int64_t duration;
        
            /**
             * Total stream bitrate in bit/s, 0 if not
             * available. Never set it directly if the file_size and the
             * duration are known as FFmpeg can compute it automatically.
             */
            int64_t bit_rate;
        
            unsigned int packet_size;
            int max_delay;
        
            /**
             * Flags modifying the (de)muxer behaviour. A combination of AVFMT_FLAG_*.
             * Set by the user before avformat_open_input() / avformat_write_header().
             */
            int flags;
        #define AVFMT_FLAG_GENPTS       0x0001 ///< Generate missing pts even if it requires parsing future frames.
        #define AVFMT_FLAG_IGNIDX       0x0002 ///< Ignore index.
        #define AVFMT_FLAG_NONBLOCK     0x0004 ///< Do not block when reading packets from input.
        #define AVFMT_FLAG_IGNDTS       0x0008 ///< Ignore DTS on frames that contain both DTS & PTS
        #define AVFMT_FLAG_NOFILLIN     0x0010 ///< Do not infer any values from other values, just return what is stored in the container
        #define AVFMT_FLAG_NOPARSE      0x0020 ///< Do not use AVParsers, you also must set AVFMT_FLAG_NOFILLIN as the fillin code works on frames and no parsing -> no frames. Also seeking to frames can not work if parsing to find frame boundaries has been disabled
        #define AVFMT_FLAG_NOBUFFER     0x0040 ///< Do not buffer frames when possible
        #define AVFMT_FLAG_CUSTOM_IO    0x0080 ///< The caller has supplied a custom AVIOContext, don't avio_close() it.
        #define AVFMT_FLAG_DISCARD_CORRUPT  0x0100 ///< Discard frames marked corrupted
        #define AVFMT_FLAG_FLUSH_PACKETS    0x0200 ///< Flush the AVIOContext every packet.
        /**
         * When muxing, try to avoid writing any random/volatile data to the output.
         * This includes any random IDs, real-time timestamps/dates, muxer version, etc.
         *
         * This flag is mainly intended for testing.
         */
        #define AVFMT_FLAG_BITEXACT         0x0400
        #if FF_API_LAVF_MP4A_LATM
        #define AVFMT_FLAG_MP4A_LATM    0x8000 ///< Deprecated, does nothing.
        #endif
        #define AVFMT_FLAG_SORT_DTS    0x10000 ///< try to interleave outputted packets by dts (using this flag can slow demuxing down)
        #define AVFMT_FLAG_PRIV_OPT    0x20000 ///< Enable use of private options by delaying codec open (this could be made default once all code is converted)
        #if FF_API_LAVF_KEEPSIDE_FLAG
        #define AVFMT_FLAG_KEEP_SIDE_DATA 0x40000 ///< Deprecated, does nothing.
        #endif
        #define AVFMT_FLAG_FAST_SEEK   0x80000 ///< Enable fast, but inaccurate seeks for some formats
        #define AVFMT_FLAG_SHORTEST   0x100000 ///< Stop muxing when the shortest stream stops.
        #define AVFMT_FLAG_AUTO_BSF   0x200000 ///< Add bitstream filters as requested by the muxer
        
            /**
             * Maximum size of the data read from input for determining
             * the input container format.
             * Demuxing only, set by the caller before avformat_open_input().
             */
            int64_t probesize;
        
            /**
             * Maximum duration (in AV_TIME_BASE units) of the data read
             * from input in avformat_find_stream_info().
             * Demuxing only, set by the caller before avformat_find_stream_info().
             * Can be set to 0 to let avformat choose using a heuristic.
             */
            int64_t max_analyze_duration;
        
            const uint8_t *key;
            int keylen;
        
            unsigned int nb_programs;
            AVProgram **programs;
        
            /**
             * Forced video codec_id.
             * Demuxing: Set by user.
             */
            enum AVCodecID video_codec_id;
        
            /**
             * Forced audio codec_id.
             * Demuxing: Set by user.
             */
            enum AVCodecID audio_codec_id;
        
            /**
             * Forced subtitle codec_id.
             * Demuxing: Set by user.
             */
            enum AVCodecID subtitle_codec_id;
        
            /**
             * Maximum amount of memory in bytes to use for the index of each stream.
             * If the index exceeds this size, entries will be discarded as
             * needed to maintain a smaller size. This can lead to slower or less
             * accurate seeking (depends on demuxer).
             * Demuxers for which a full in-memory index is mandatory will ignore
             * this.
             * - muxing: unused
             * - demuxing: set by user
             */
            unsigned int max_index_size;
        
            /**
             * Maximum amount of memory in bytes to use for buffering frames
             * obtained from realtime capture devices.
             */
            unsigned int max_picture_buffer;
        
            /**
             * Number of chapters in AVChapter array.
             * When muxing, chapters are normally written in the file header,
             * so nb_chapters should normally be initialized before write_header
             * is called. Some muxers (e.g. mov and mkv) can also write chapters
             * in the trailer.  To write chapters in the trailer, nb_chapters
             * must be zero when write_header is called and non-zero when
             * write_trailer is called.
             * - muxing: set by user
             * - demuxing: set by libavformat
             */
            unsigned int nb_chapters;
            AVChapter **chapters;
        
            /**
             * Metadata that applies to the whole file.
             *
             * - demuxing: set by libavformat in avformat_open_input()
             * - muxing: may be set by the caller before avformat_write_header()
             *
             * Freed by libavformat in avformat_free_context().
             */
            AVDictionary *metadata;
        
            /**
             * Start time of the stream in real world time, in microseconds
             * since the Unix epoch (00:00 1st January 1970). That is, pts=0 in the
             * stream was captured at this real world time.
             * - muxing: Set by the caller before avformat_write_header(). If set to
             *           either 0 or AV_NOPTS_VALUE, then the current wall-time will
             *           be used.
             * - demuxing: Set by libavformat. AV_NOPTS_VALUE if unknown. Note that
             *             the value may become known after some number of frames
             *             have been received.
             */
            int64_t start_time_realtime;
        
            /**
             * The number of frames used for determining the framerate in
             * avformat_find_stream_info().
             * Demuxing only, set by the caller before avformat_find_stream_info().
             */
            int fps_probe_size;
        
            /**
             * Error recognition; higher values will detect more errors but may
             * misdetect some more or less valid parts as errors.
             * Demuxing only, set by the caller before avformat_open_input().
             */
            int error_recognition;
        
            /**
             * Custom interrupt callbacks for the I/O layer.
             *
             * demuxing: set by the user before avformat_open_input().
             * muxing: set by the user before avformat_write_header()
             * (mainly useful for AVFMT_NOFILE formats). The callback
             * should also be passed to avio_open2() if it's used to
             * open the file.
             */
            AVIOInterruptCB interrupt_callback;
        
            /**
             * Flags to enable debugging.
             */
            int debug;
        #define FF_FDEBUG_TS        0x0001
        
            /**
             * Maximum buffering duration for interleaving.
             *
             * To ensure all the streams are interleaved correctly,
             * av_interleaved_write_frame() will wait until it has at least one packet
             * for each stream before actually writing any packets to the output file.
             * When some streams are "sparse" (i.e. there are large gaps between
             * successive packets), this can result in excessive buffering.
             *
             * This field specifies the maximum difference between the timestamps of the
             * first and the last packet in the muxing queue, above which libavformat
             * will output a packet regardless of whether it has queued a packet for all
             * the streams.
             *
             * Muxing only, set by the caller before avformat_write_header().
             */
            int64_t max_interleave_delta;
        
            /**
             * Allow non-standard and experimental extension
             * @see AVCodecContext.strict_std_compliance
             */
            int strict_std_compliance;
        
            /**
             * Flags for the user to detect events happening on the file. Flags must
             * be cleared by the user once the event has been handled.
             * A combination of AVFMT_EVENT_FLAG_*.
             */
            int event_flags;
        #define AVFMT_EVENT_FLAG_METADATA_UPDATED 0x0001 ///< The call resulted in updated metadata.
        
            /**
             * Maximum number of packets to read while waiting for the first timestamp.
             * Decoding only.
             */
            int max_ts_probe;
        
            /**
             * Avoid negative timestamps during muxing.
             * Any value of the AVFMT_AVOID_NEG_TS_* constants.
             * Note, this only works when using av_interleaved_write_frame. (interleave_packet_per_dts is in use)
             * - muxing: Set by user
             * - demuxing: unused
             */
            int avoid_negative_ts;
        #define AVFMT_AVOID_NEG_TS_AUTO             -1 ///< Enabled when required by target format
        #define AVFMT_AVOID_NEG_TS_MAKE_NON_NEGATIVE 1 ///< Shift timestamps so they are non negative
        #define AVFMT_AVOID_NEG_TS_MAKE_ZERO         2 ///< Shift timestamps so that they start at 0
        
            /**
             * Transport stream id.
             * This will be moved into demuxer private options. Thus no API/ABI compatibility
             */
            int ts_id;
        
            /**
             * Audio preload in microseconds.
             * Note, not all formats support this and unpredictable things may happen if it is used when not supported.
             * - encoding: Set by user
             * - decoding: unused
             */
            int audio_preload;
        
            /**
             * Max chunk time in microseconds.
             * Note, not all formats support this and unpredictable things may happen if it is used when not supported.
             * - encoding: Set by user
             * - decoding: unused
             */
            int max_chunk_duration;
        
            /**
             * Max chunk size in bytes
             * Note, not all formats support this and unpredictable things may happen if it is used when not supported.
             * - encoding: Set by user
             * - decoding: unused
             */
            int max_chunk_size;
        
            /**
             * forces the use of wallclock timestamps as pts/dts of packets
             * This has undefined results in the presence of B frames.
             * - encoding: unused
             * - decoding: Set by user
             */
            int use_wallclock_as_timestamps;
        
            /**
             * avio flags, used to force AVIO_FLAG_DIRECT.
             * - encoding: unused
             * - decoding: Set by user
             */
            int avio_flags;
        
            /**
             * The duration field can be estimated through various ways, and this field can be used
             * to know how the duration was estimated.
             * - encoding: unused
             * - decoding: Read by user
             */
            enum AVDurationEstimationMethod duration_estimation_method;
        
            /**
             * Skip initial bytes when opening stream
             * - encoding: unused
             * - decoding: Set by user
             */
            int64_t skip_initial_bytes;
        
            /**
             * Correct single timestamp overflows
             * - encoding: unused
             * - decoding: Set by user
             */
            unsigned int correct_ts_overflow;
        
            /**
             * Force seeking to any (also non key) frames.
             * - encoding: unused
             * - decoding: Set by user
             */
            int seek2any;
        
            /**
             * Flush the I/O context after each packet.
             * - encoding: Set by user
             * - decoding: unused
             */
            int flush_packets;
        
            /**
             * format probing score.
             * The maximal score is AVPROBE_SCORE_MAX, its set when the demuxer probes
             * the format.
             * - encoding: unused
             * - decoding: set by avformat, read by user
             */
            int probe_score;
        
            /**
             * number of bytes to read maximally to identify format.
             * - encoding: unused
             * - decoding: set by user
             */
            int format_probesize;
        
            /**
             * ',' separated list of allowed decoders.
             * If NULL then all are allowed
             * - encoding: unused
             * - decoding: set by user
             */
            char *codec_whitelist;
        
            /**
             * ',' separated list of allowed demuxers.
             * If NULL then all are allowed
             * - encoding: unused
             * - decoding: set by user
             */
            char *format_whitelist;
        
            /**
             * An opaque field for libavformat internal usage.
             * Must not be accessed in any way by callers.
             */
            AVFormatInternal *internal;
        
            /**
             * IO repositioned flag.
             * This is set by avformat when the underlaying IO context read pointer
             * is repositioned, for example when doing byte based seeking.
             * Demuxers can use the flag to detect such changes.
             */
            int io_repositioned;
        
            /**
             * Forced video codec.
             * This allows forcing a specific decoder, even when there are multiple with
             * the same codec_id.
             * Demuxing: Set by user
             */
            AVCodec *video_codec;
        
            /**
             * Forced audio codec.
             * This allows forcing a specific decoder, even when there are multiple with
             * the same codec_id.
             * Demuxing: Set by user
             */
            AVCodec *audio_codec;
        
            /**
             * Forced subtitle codec.
             * This allows forcing a specific decoder, even when there are multiple with
             * the same codec_id.
             * Demuxing: Set by user
             */
            AVCodec *subtitle_codec;
        
            /**
             * Forced data codec.
             * This allows forcing a specific decoder, even when there are multiple with
             * the same codec_id.
             * Demuxing: Set by user
             */
            AVCodec *data_codec;
        
            /**
             * Number of bytes to be written as padding in a metadata header.
             * Demuxing: Unused.
             * Muxing: Set by user via av_format_set_metadata_header_padding.
             */
            int metadata_header_padding;
        
            /**
             * User data.
             * This is a place for some private data of the user.
             */
            void *opaque;
        
            /**
             * Callback used by devices to communicate with application.
             */
            av_format_control_message control_message_cb;
        
            /**
             * Output timestamp offset, in microseconds.
             * Muxing: set by user
             */
            int64_t output_ts_offset;
        
            /**
             * dump format separator.
             * can be ", " or "\n      " or anything else
             * - muxing: Set by user.
             * - demuxing: Set by user.
             */
            uint8_t *dump_separator;
        
            /**
             * Forced Data codec_id.
             * Demuxing: Set by user.
             */
            enum AVCodecID data_codec_id;
        
        #if FF_API_OLD_OPEN_CALLBACKS
            /**
             * Called to open further IO contexts when needed for demuxing.
             *
             * This can be set by the user application to perform security checks on
             * the URLs before opening them.
             * The function should behave like avio_open2(), AVFormatContext is provided
             * as contextual information and to reach AVFormatContext.opaque.
             *
             * If NULL then some simple checks are used together with avio_open2().
             *
             * Must not be accessed directly from outside avformat.
             * @See av_format_set_open_cb()
             *
             * Demuxing: Set by user.
             *
             * @deprecated Use io_open and io_close.
             */
            attribute_deprecated
            int (*open_cb)(struct AVFormatContext *s, AVIOContext **p, const char *url, int flags, const AVIOInterruptCB *int_cb, AVDictionary **options);
        #endif
        
            /**
             * ',' separated list of allowed protocols.
             * - encoding: unused
             * - decoding: set by user
             */
            char *protocol_whitelist;
        
            /**
             * A callback for opening new IO streams.
             *
             * Whenever a muxer or a demuxer needs to open an IO stream (typically from
             * avformat_open_input() for demuxers, but for certain formats can happen at
             * other times as well), it will call this callback to obtain an IO context.
             *
             * @param s the format context
             * @param pb on success, the newly opened IO context should be returned here
             * @param url the url to open
             * @param flags a combination of AVIO_FLAG_*
             * @param options a dictionary of additional options, with the same
             *                semantics as in avio_open2()
             * @return 0 on success, a negative AVERROR code on failure
             *
             * @note Certain muxers and demuxers do nesting, i.e. they open one or more
             * additional internal format contexts. Thus the AVFormatContext pointer
             * passed to this callback may be different from the one facing the caller.
             * It will, however, have the same 'opaque' field.
             */
            int (*io_open)(struct AVFormatContext *s, AVIOContext **pb, const char *url,
                           int flags, AVDictionary **options);
        
            /**
             * A callback for closing the streams opened with AVFormatContext.io_open().
             */
            void (*io_close)(struct AVFormatContext *s, AVIOContext *pb);
        
            /**
             * ',' separated list of disallowed protocols.
             * - encoding: unused
             * - decoding: set by user
             */
            char *protocol_blacklist;
        
            /**
             * The maximum number of streams.
             * - encoding: unused
             * - decoding: set by user
             */
            int max_streams;
        
            /**
             * Skip duration calcuation in estimate_timings_from_pts.
             * - encoding: unused
             * - decoding: set by user
             */
            int skip_estimate_duration_from_pts;
        
            /**
             * Maximum number of packets that can be probed
             * - encoding: unused
             * - decoding: set by user
             */
            int max_probe_packets;
        } AVFormatContext;
        ```

        - iformat：输入媒体的AVInputFormat，比如指向ff_flv_demuxer；
        - nb_streams：输入媒体的AVStream个数；
        - streams：输入媒体的AVStream[]数组；
        - duration：输入媒体的时长（以微秒为单位），计算方式可以参考av_dump_format()函数；
        - bit_rate：输入媒体的码率；

    - AVInputFormat

        ```C++
        /**
         * @addtogroup lavf_decoding
         * @{
         */
        typedef struct AVInputFormat {
            /**
             * A comma separated list of short names for the format. New names
             * may be appended with a minor bump.
             */
            const char *name;
        
            /**
             * Descriptive name for the format, meant to be more human-readable
             * than name. You should use the NULL_IF_CONFIG_SMALL() macro
             * to define it.
             */
            const char *long_name;
        
            /**
             * Can use flags: AVFMT_NOFILE, AVFMT_NEEDNUMBER, AVFMT_SHOW_IDS,
             * AVFMT_NOTIMESTAMPS, AVFMT_GENERIC_INDEX, AVFMT_TS_DISCONT, AVFMT_NOBINSEARCH,
             * AVFMT_NOGENSEARCH, AVFMT_NO_BYTE_SEEK, AVFMT_SEEK_TO_PTS.
             */
            int flags;
        
            /**
             * If extensions are defined, then no probe is done. You should
             * usually not use extension format guessing because it is not
             * reliable enough
             */
            const char *extensions;
        
            const struct AVCodecTag * const *codec_tag;
        
            const AVClass *priv_class; ///< AVClass for the private context
        
            /**
             * Comma-separated list of mime types.
             * It is used check for matching mime types while probing.
             * @see av_probe_input_format2
             */
            const char *mime_type;
        
            /*****************************************************************
             * No fields below this line are part of the public API. They
             * may not be used outside of libavformat and can be changed and
             * removed at will.
             * New public fields should be added right above.
             *****************************************************************
             */
        #if FF_API_NEXT
            ff_const59 struct AVInputFormat *next;
        #endif
        
            /**
             * Raw demuxers store their codec ID here.
             */
            int raw_codec_id;
        
            /**
             * Size of private data so that it can be allocated in the wrapper.
             */
            int priv_data_size;
        
            /**
             * Tell if a given file has a chance of being parsed as this format.
             * The buffer provided is guaranteed to be AVPROBE_PADDING_SIZE bytes
             * big so you do not have to check for that unless you need more.
             */
            int (*read_probe)(const AVProbeData *);
        
            /**
             * Read the format header and initialize the AVFormatContext
             * structure. Return 0 if OK. 'avformat_new_stream' should be
             * called to create new streams.
             */
            int (*read_header)(struct AVFormatContext *);
        
            /**
             * Read one packet and put it in 'pkt'. pts and flags are also
             * set. 'avformat_new_stream' can be called only if the flag
             * AVFMTCTX_NOHEADER is used and only in the calling thread (not in a
             * background thread).
             * @return 0 on success, < 0 on error.
             *         Upon returning an error, pkt must be unreferenced by the caller.
             */
            int (*read_packet)(struct AVFormatContext *, AVPacket *pkt);
        
            /**
             * Close the stream. The AVFormatContext and AVStreams are not
             * freed by this function
             */
            int (*read_close)(struct AVFormatContext *);
        
            /**
             * Seek to a given timestamp relative to the frames in
             * stream component stream_index.
             * @param stream_index Must not be -1.
             * @param flags Selects which direction should be preferred if no exact
             *              match is available.
             * @return >= 0 on success (but not necessarily the new offset)
             */
            int (*read_seek)(struct AVFormatContext *,
                             int stream_index, int64_t timestamp, int flags);
        
            /**
             * Get the next timestamp in stream[stream_index].time_base units.
             * @return the timestamp or AV_NOPTS_VALUE if an error occurred
             */
            int64_t (*read_timestamp)(struct AVFormatContext *s, int stream_index,
                                      int64_t *pos, int64_t pos_limit);
        
            /**
             * Start/resume playing - only meaningful if using a network-based format
             * (RTSP).
             */
            int (*read_play)(struct AVFormatContext *);
        
            /**
             * Pause playing - only meaningful if using a network-based format
             * (RTSP).
             */
            int (*read_pause)(struct AVFormatContext *);
        
            /**
             * Seek to timestamp ts.
             * Seeking will be done so that the point from which all active streams
             * can be presented successfully will be closest to ts and within min/max_ts.
             * Active streams are all streams that have AVStream.discard < AVDISCARD_ALL.
             */
            int (*read_seek2)(struct AVFormatContext *s, int stream_index, int64_t min_ts, int64_t ts, int64_t max_ts, int flags);
        
            /**
             * Returns device list with it properties.
             * @see avdevice_list_devices() for more details.
             */
            int (*get_device_list)(struct AVFormatContext *s, struct AVDeviceInfoList *device_list);
        
            /**
             * Initialize device capabilities submodule.
             * @see avdevice_capabilities_create() for more details.
             */
            int (*create_device_capabilities)(struct AVFormatContext *s, struct AVDeviceCapabilitiesQuery *caps);
        
            /**
             * Free device capabilities submodule.
             * @see avdevice_capabilities_free() for more details.
             */
            int (*free_device_capabilities)(struct AVFormatContext *s, struct AVDeviceCapabilitiesQuery *caps);
        }
        /**
         * @}
         */
        ```

        - name：封装格式名称；
        - extensions：封装格式的扩展名；
        - 一些封装格式处理的接口函数，比如read_packet()；

    - AVStream

        ```C++
        /**
         * Stream structure.
         * New fields can be added to the end with minor version bumps.
         * Removal, reordering and changes to existing fields require a major
         * version bump.
         * sizeof(AVStream) must not be used outside libav*.
         */
        typedef struct AVStream {
            int index;    /**< stream index in AVFormatContext */
            /**
             * Format-specific stream ID.
             * decoding: set by libavformat
             * encoding: set by the user, replaced by libavformat if left unset
             */
            int id;
        #if FF_API_LAVF_AVCTX
            /**
             * @deprecated use the codecpar struct instead
             */
            attribute_deprecated
            AVCodecContext *codec;
        #endif
            void *priv_data;
        
            /**
             * This is the fundamental unit of time (in seconds) in terms
             * of which frame timestamps are represented.
             *
             * decoding: set by libavformat
             * encoding: May be set by the caller before avformat_write_header() to
             *           provide a hint to the muxer about the desired timebase. In
             *           avformat_write_header(), the muxer will overwrite this field
             *           with the timebase that will actually be used for the timestamps
             *           written into the file (which may or may not be related to the
             *           user-provided one, depending on the format).
             */
            AVRational time_base;
        
            /**
             * Decoding: pts of the first frame of the stream in presentation order, in stream time base.
             * Only set this if you are absolutely 100% sure that the value you set
             * it to really is the pts of the first frame.
             * This may be undefined (AV_NOPTS_VALUE).
             * @note The ASF header does NOT contain a correct start_time the ASF
             * demuxer must NOT set this.
             */
            int64_t start_time;
        
            /**
             * Decoding: duration of the stream, in stream time base.
             * If a source file does not specify a duration, but does specify
             * a bitrate, this value will be estimated from bitrate and file size.
             *
             * Encoding: May be set by the caller before avformat_write_header() to
             * provide a hint to the muxer about the estimated duration.
             */
            int64_t duration;
        
            int64_t nb_frames;                 ///< number of frames in this stream if known or 0
        
            int disposition; /**< AV_DISPOSITION_* bit field */
        
            enum AVDiscard discard; ///< Selects which packets can be discarded at will and do not need to be demuxed.
        
            /**
             * sample aspect ratio (0 if unknown)
             * - encoding: Set by user.
             * - decoding: Set by libavformat.
             */
            AVRational sample_aspect_ratio;
        
            AVDictionary *metadata;
        
            /**
             * Average framerate
             *
             * - demuxing: May be set by libavformat when creating the stream or in
             *             avformat_find_stream_info().
             * - muxing: May be set by the caller before avformat_write_header().
             */
            AVRational avg_frame_rate;
        
            /**
             * For streams with AV_DISPOSITION_ATTACHED_PIC disposition, this packet
             * will contain the attached picture.
             *
             * decoding: set by libavformat, must not be modified by the caller.
             * encoding: unused
             */
            AVPacket attached_pic;
        
            /**
             * An array of side data that applies to the whole stream (i.e. the
             * container does not allow it to change between packets).
             *
             * There may be no overlap between the side data in this array and side data
             * in the packets. I.e. a given side data is either exported by the muxer
             * (demuxing) / set by the caller (muxing) in this array, then it never
             * appears in the packets, or the side data is exported / sent through
             * the packets (always in the first packet where the value becomes known or
             * changes), then it does not appear in this array.
             *
             * - demuxing: Set by libavformat when the stream is created.
             * - muxing: May be set by the caller before avformat_write_header().
             *
             * Freed by libavformat in avformat_free_context().
             *
             * @see av_format_inject_global_side_data()
             */
            AVPacketSideData *side_data;
            /**
             * The number of elements in the AVStream.side_data array.
             */
            int            nb_side_data;
        
            /**
             * Flags for the user to detect events happening on the stream. Flags must
             * be cleared by the user once the event has been handled.
             * A combination of AVSTREAM_EVENT_FLAG_*.
             */
            int event_flags;
        #define AVSTREAM_EVENT_FLAG_METADATA_UPDATED 0x0001 ///< The call resulted in updated metadata.
        
            /**
             * Real base framerate of the stream.
             * This is the lowest framerate with which all timestamps can be
             * represented accurately (it is the least common multiple of all
             * framerates in the stream). Note, this value is just a guess!
             * For example, if the time base is 1/90000 and all frames have either
             * approximately 3600 or 1800 timer ticks, then r_frame_rate will be 50/1.
             */
            AVRational r_frame_rate;
        
        #if FF_API_LAVF_FFSERVER
            /**
             * String containing pairs of key and values describing recommended encoder configuration.
             * Pairs are separated by ','.
             * Keys are separated from values by '='.
             *
             * @deprecated unused
             */
            attribute_deprecated
            char *recommended_encoder_configuration;
        #endif
        
            /**
             * Codec parameters associated with this stream. Allocated and freed by
             * libavformat in avformat_new_stream() and avformat_free_context()
             * respectively.
             *
             * - demuxing: filled by libavformat on stream creation or in
             *             avformat_find_stream_info()
             * - muxing: filled by the caller before avformat_write_header()
             */
            AVCodecParameters *codecpar;
        
            /*****************************************************************
             * All fields below this line are not part of the public API. They
             * may not be used outside of libavformat and can be changed and
             * removed at will.
             * Internal note: be aware that physically removing these fields
             * will break ABI. Replace removed fields with dummy fields, and
             * add new fields to AVStreamInternal.
             *****************************************************************
             */
        
        #define MAX_STD_TIMEBASES (30*12+30+3+6)
            /**
             * Stream information used internally by avformat_find_stream_info()
             */
            struct {
                int64_t last_dts;
                int64_t duration_gcd;
                int duration_count;
                int64_t rfps_duration_sum;
                double (*duration_error)[2][MAX_STD_TIMEBASES];
                int64_t codec_info_duration;
                int64_t codec_info_duration_fields;
                int frame_delay_evidence;
        
                /**
                 * 0  -> decoder has not been searched for yet.
                 * >0 -> decoder found
                 * <0 -> decoder with codec_id == -found_decoder has not been found
                 */
                int found_decoder;
        
                int64_t last_duration;
        
                /**
                 * Those are used for average framerate estimation.
                 */
                int64_t fps_first_dts;
                int     fps_first_dts_idx;
                int64_t fps_last_dts;
                int     fps_last_dts_idx;
        
            } *info;
        
            int pts_wrap_bits; /**< number of bits in pts (used for wrapping control) */
        
            // Timestamp generation support:
            /**
             * Timestamp corresponding to the last dts sync point.
             *
             * Initialized when AVCodecParserContext.dts_sync_point >= 0 and
             * a DTS is received from the underlying container. Otherwise set to
             * AV_NOPTS_VALUE by default.
             */
            int64_t first_dts;
            int64_t cur_dts;
            int64_t last_IP_pts;
            int last_IP_duration;
        
            /**
             * Number of packets to buffer for codec probing
             */
            int probe_packets;
        
            /**
             * Number of frames that have been demuxed during avformat_find_stream_info()
             */
            int codec_info_nb_frames;
        
            /* av_read_frame() support */
            enum AVStreamParseType need_parsing;
            struct AVCodecParserContext *parser;
        
            /**
             * last packet in packet_buffer for this stream when muxing.
             */
            struct AVPacketList *last_in_packet_buffer;
            AVProbeData probe_data;
        #define MAX_REORDER_DELAY 16
            int64_t pts_buffer[MAX_REORDER_DELAY+1];
        
            AVIndexEntry *index_entries; /**< Only used if the format does not
                                            support seeking natively. */
            int nb_index_entries;
            unsigned int index_entries_allocated_size;
        
            /**
             * Stream Identifier
             * This is the MPEG-TS stream identifier +1
             * 0 means unknown
             */
            int stream_identifier;
        
            /**
             * Details of the MPEG-TS program which created this stream.
             */
            int program_num;
            int pmt_version;
            int pmt_stream_idx;
        
            int64_t interleaver_chunk_size;
            int64_t interleaver_chunk_duration;
        
            /**
             * stream probing state
             * -1   -> probing finished
             *  0   -> no probing requested
             * rest -> perform probing with request_probe being the minimum score to accept.
             */
            int request_probe;
            /**
             * Indicates that everything up to the next keyframe
             * should be discarded.
             */
            int skip_to_keyframe;
        
            /**
             * Number of samples to skip at the start of the frame decoded from the next packet.
             */
            int skip_samples;
        
            /**
             * If not 0, the number of samples that should be skipped from the start of
             * the stream (the samples are removed from packets with pts==0, which also
             * assumes negative timestamps do not happen).
             * Intended for use with formats such as mp3 with ad-hoc gapless audio
             * support.
             */
            int64_t start_skip_samples;
        
            /**
             * If not 0, the first audio sample that should be discarded from the stream.
             * This is broken by design (needs global sample count), but can't be
             * avoided for broken by design formats such as mp3 with ad-hoc gapless
             * audio support.
             */
            int64_t first_discard_sample;
        
            /**
             * The sample after last sample that is intended to be discarded after
             * first_discard_sample. Works on frame boundaries only. Used to prevent
             * early EOF if the gapless info is broken (considered concatenated mp3s).
             */
            int64_t last_discard_sample;
        
            /**
             * Number of internally decoded frames, used internally in libavformat, do not access
             * its lifetime differs from info which is why it is not in that structure.
             */
            int nb_decoded_frames;
        
            /**
             * Timestamp offset added to timestamps before muxing
             */
            int64_t mux_ts_offset;
        
            /**
             * Internal data to check for wrapping of the time stamp
             */
            int64_t pts_wrap_reference;
        
            /**
             * Options for behavior, when a wrap is detected.
             *
             * Defined by AV_PTS_WRAP_ values.
             *
             * If correction is enabled, there are two possibilities:
             * If the first time stamp is near the wrap point, the wrap offset
             * will be subtracted, which will create negative time stamps.
             * Otherwise the offset will be added.
             */
            int pts_wrap_behavior;
        
            /**
             * Internal data to prevent doing update_initial_durations() twice
             */
            int update_initial_durations_done;
        
            /**
             * Internal data to generate dts from pts
             */
            int64_t pts_reorder_error[MAX_REORDER_DELAY+1];
            uint8_t pts_reorder_error_count[MAX_REORDER_DELAY+1];
        
            /**
             * Internal data to analyze DTS and detect faulty mpeg streams
             */
            int64_t last_dts_for_order_check;
            uint8_t dts_ordered;
            uint8_t dts_misordered;
        
            /**
             * Internal data to inject global side data
             */
            int inject_global_side_data;
        
            /**
             * display aspect ratio (0 if unknown)
             * - encoding: unused
             * - decoding: Set by libavformat to calculate sample_aspect_ratio internally
             */
            AVRational display_aspect_ratio;
        
            /**
             * An opaque field for libavformat internal usage.
             * Must not be accessed in any way by callers.
             */
            AVStreamInternal *internal;
        } AVStream;
        ```

        - index：标识该视频/音频流；
        - time_base：该流的时基， PTS*time_base=真正的时间（秒）；
        - duration：该视频/音频流长度；
        - avg_frame_rate：该流的帧率；
        - codecpar：编解码器参数属性；

    - AVCodecParameters

        ```C++
        /**
         * This struct describes the properties of an encoded stream.
         *
         * sizeof(AVCodecParameters) is not a part of the public ABI, this struct must
         * be allocated with avcodec_parameters_alloc() and freed with
         * avcodec_parameters_free().
         */
        typedef struct AVCodecParameters {
            /**
             * General type of the encoded data.
             */
            enum AVMediaType codec_type;
            /**
             * Specific type of the encoded data (the codec used).
             */
            enum AVCodecID   codec_id;
            /**
             * Additional information about the codec (corresponds to the AVI FOURCC).
             */
            uint32_t         codec_tag;
        
            /**
             * Extra binary data needed for initializing the decoder, codec-dependent.
             *
             * Must be allocated with av_malloc() and will be freed by
             * avcodec_parameters_free(). The allocated size of extradata must be at
             * least extradata_size + AV_INPUT_BUFFER_PADDING_SIZE, with the padding
             * bytes zeroed.
             */
            uint8_t *extradata;
            /**
             * Size of the extradata content in bytes.
             */
            int      extradata_size;
        
            /**
             * - video: the pixel format, the value corresponds to enum AVPixelFormat.
             * - audio: the sample format, the value corresponds to enum AVSampleFormat.
             */
            int format;
        
            /**
             * The average bitrate of the encoded data (in bits per second).
             */
            int64_t bit_rate;
        
            /**
             * The number of bits per sample in the codedwords.
             *
             * This is basically the bitrate per sample. It is mandatory for a bunch of
             * formats to actually decode them. It's the number of bits for one sample in
             * the actual coded bitstream.
             *
             * This could be for example 4 for ADPCM
             * For PCM formats this matches bits_per_raw_sample
             * Can be 0
             */
            int bits_per_coded_sample;
        
            /**
             * This is the number of valid bits in each output sample. If the
             * sample format has more bits, the least significant bits are additional
             * padding bits, which are always 0. Use right shifts to reduce the sample
             * to its actual size. For example, audio formats with 24 bit samples will
             * have bits_per_raw_sample set to 24, and format set to AV_SAMPLE_FMT_S32.
             * To get the original sample use "(int32_t)sample >> 8"."
             *
             * For ADPCM this might be 12 or 16 or similar
             * Can be 0
             */
            int bits_per_raw_sample;
        
            /**
             * Codec-specific bitstream restrictions that the stream conforms to.
             */
            int profile;
            int level;
        
            /**
             * Video only. The dimensions of the video frame in pixels.
             */
            int width;
            int height;
        
            /**
             * Video only. The aspect ratio (width / height) which a single pixel
             * should have when displayed.
             *
             * When the aspect ratio is unknown / undefined, the numerator should be
             * set to 0 (the denominator may have any value).
             */
            AVRational sample_aspect_ratio;
        
            /**
             * Video only. The order of the fields in interlaced video.
             */
            enum AVFieldOrder                  field_order;
        
            /**
             * Video only. Additional colorspace characteristics.
             */
            enum AVColorRange                  color_range;
            enum AVColorPrimaries              color_primaries;
            enum AVColorTransferCharacteristic color_trc;
            enum AVColorSpace                  color_space;
            enum AVChromaLocation              chroma_location;
        
            /**
             * Video only. Number of delayed frames.
             */
            int video_delay;
        
            /**
             * Audio only. The channel layout bitmask. May be 0 if the channel layout is
             * unknown or unspecified, otherwise the number of bits set must be equal to
             * the channels field.
             */
            uint64_t channel_layout;
            /**
             * Audio only. The number of audio channels.
             */
            int      channels;
            /**
             * Audio only. The number of audio samples per second.
             */
            int      sample_rate;
            /**
             * Audio only. The number of bytes per coded audio frame, required by some
             * formats.
             *
             * Corresponds to nBlockAlign in WAVEFORMATEX.
             */
            int      block_align;
            /**
             * Audio only. Audio frame size, if known. Required by some formats to be static.
             */
            int      frame_size;
        
            /**
             * Audio only. The amount of padding (in samples) inserted by the encoder at
             * the beginning of the audio. I.e. this number of leading decoded samples
             * must be discarded by the caller to get the original audio without leading
             * padding.
             */
            int initial_padding;
            /**
             * Audio only. The amount of padding (in samples) appended by the encoder to
             * the end of the audio. I.e. this number of decoded samples must be
             * discarded by the caller from the end of the stream to get the original
             * audio without any trailing padding.
             */
            int trailing_padding;
            /**
             * Audio only. Number of samples to skip after a discontinuity.
             */
            int seek_preroll;
        }
        ```

        - codec_type：媒体类型，比如AVMEDIA_TYPE_VIDEO、AVMEDIA_TYPE_AUDIO等；
        - codec_id：编解码器类型， 比如AV_CODEC_ID_H264、AV_CODEC_ID_AAC等；

    - AVCodecContext

        ```C++
        /**
         * main external API structure.
         * New fields can be added to the end with minor version bumps.
         * Removal, reordering and changes to existing fields require a major
         * version bump.
         * You can use AVOptions (av_opt* / av_set/get*()) to access these fields from user
         * applications.
         * The name string for AVOptions options matches the associated command line
         * parameter name and can be found in libavcodec/options_table.h
         * The AVOption/command line parameter names differ in some cases from the C
         * structure field names for historic reasons or brevity.
         * sizeof(AVCodecContext) must not be used outside libav*.
         */
        typedef struct AVCodecContext {
            /**
             * information on struct for av_log
             * - set by avcodec_alloc_context3
             */
            const AVClass *av_class;
            int log_level_offset;
        
            enum AVMediaType codec_type; /* see AVMEDIA_TYPE_xxx */
            const struct AVCodec  *codec;
            enum AVCodecID     codec_id; /* see AV_CODEC_ID_xxx */
        
            /**
             * fourcc (LSB first, so "ABCD" -> ('D'<<24) + ('C'<<16) + ('B'<<8) + 'A').
             * This is used to work around some encoder bugs.
             * A demuxer should set this to what is stored in the field used to identify the codec.
             * If there are multiple such fields in a container then the demuxer should choose the one
             * which maximizes the information about the used codec.
             * If the codec tag field in a container is larger than 32 bits then the demuxer should
             * remap the longer ID to 32 bits with a table or other structure. Alternatively a new
             * extra_codec_tag + size could be added but for this a clear advantage must be demonstrated
             * first.
             * - encoding: Set by user, if not then the default based on codec_id will be used.
             * - decoding: Set by user, will be converted to uppercase by libavcodec during init.
             */
            unsigned int codec_tag;
        
            void *priv_data;
        
            /**
             * Private context used for internal data.
             *
             * Unlike priv_data, this is not codec-specific. It is used in general
             * libavcodec functions.
             */
            struct AVCodecInternal *internal;
        
            /**
             * Private data of the user, can be used to carry app specific stuff.
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            void *opaque;
        
            /**
             * the average bitrate
             * - encoding: Set by user; unused for constant quantizer encoding.
             * - decoding: Set by user, may be overwritten by libavcodec
             *             if this info is available in the stream
             */
            int64_t bit_rate;
        
            /**
             * number of bits the bitstream is allowed to diverge from the reference.
             *           the reference can be CBR (for CBR pass1) or VBR (for pass2)
             * - encoding: Set by user; unused for constant quantizer encoding.
             * - decoding: unused
             */
            int bit_rate_tolerance;
        
            /**
             * Global quality for codecs which cannot change it per frame.
             * This should be proportional to MPEG-1/2/4 qscale.
             * - encoding: Set by user.
             * - decoding: unused
             */
            int global_quality;
        
            /**
             * - encoding: Set by user.
             * - decoding: unused
             */
            int compression_level;
        #define FF_COMPRESSION_DEFAULT -1
        
            /**
             * AV_CODEC_FLAG_*.
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            int flags;
        
            /**
             * AV_CODEC_FLAG2_*
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            int flags2;
        
            /**
             * some codecs need / can use extradata like Huffman tables.
             * MJPEG: Huffman tables
             * rv10: additional flags
             * MPEG-4: global headers (they can be in the bitstream or here)
             * The allocated memory should be AV_INPUT_BUFFER_PADDING_SIZE bytes larger
             * than extradata_size to avoid problems if it is read with the bitstream reader.
             * The bytewise contents of extradata must not depend on the architecture or CPU endianness.
             * Must be allocated with the av_malloc() family of functions.
             * - encoding: Set/allocated/freed by libavcodec.
             * - decoding: Set/allocated/freed by user.
             */
            uint8_t *extradata;
            int extradata_size;
        
            /**
             * This is the fundamental unit of time (in seconds) in terms
             * of which frame timestamps are represented. For fixed-fps content,
             * timebase should be 1/framerate and timestamp increments should be
             * identically 1.
             * This often, but not always is the inverse of the frame rate or field rate
             * for video. 1/time_base is not the average frame rate if the frame rate is not
             * constant.
             *
             * Like containers, elementary streams also can store timestamps, 1/time_base
             * is the unit in which these timestamps are specified.
             * As example of such codec time base see ISO/IEC 14496-2:2001(E)
             * vop_time_increment_resolution and fixed_vop_rate
             * (fixed_vop_rate == 0 implies that it is different from the framerate)
             *
             * - encoding: MUST be set by user.
             * - decoding: the use of this field for decoding is deprecated.
             *             Use framerate instead.
             */
            AVRational time_base;
        
            /**
             * For some codecs, the time base is closer to the field rate than the frame rate.
             * Most notably, H.264 and MPEG-2 specify time_base as half of frame duration
             * if no telecine is used ...
             *
             * Set to time_base ticks per frame. Default 1, e.g., H.264/MPEG-2 set it to 2.
             */
            int ticks_per_frame;
        
            /**
             * Codec delay.
             *
             * Encoding: Number of frames delay there will be from the encoder input to
             *           the decoder output. (we assume the decoder matches the spec)
             * Decoding: Number of frames delay in addition to what a standard decoder
             *           as specified in the spec would produce.
             *
             * Video:
             *   Number of frames the decoded output will be delayed relative to the
             *   encoded input.
             *
             * Audio:
             *   For encoding, this field is unused (see initial_padding).
             *
             *   For decoding, this is the number of samples the decoder needs to
             *   output before the decoder's output is valid. When seeking, you should
             *   start decoding this many samples prior to your desired seek point.
             *
             * - encoding: Set by libavcodec.
             * - decoding: Set by libavcodec.
             */
            int delay;
        
        
            /* video only */
            /**
             * picture width / height.
             *
             * @note Those fields may not match the values of the last
             * AVFrame output by avcodec_decode_video2 due frame
             * reordering.
             *
             * - encoding: MUST be set by user.
             * - decoding: May be set by the user before opening the decoder if known e.g.
             *             from the container. Some decoders will require the dimensions
             *             to be set by the caller. During decoding, the decoder may
             *             overwrite those values as required while parsing the data.
             */
            int width, height;
        
            /**
             * Bitstream width / height, may be different from width/height e.g. when
             * the decoded frame is cropped before being output or lowres is enabled.
             *
             * @note Those field may not match the value of the last
             * AVFrame output by avcodec_receive_frame() due frame
             * reordering.
             *
             * - encoding: unused
             * - decoding: May be set by the user before opening the decoder if known
             *             e.g. from the container. During decoding, the decoder may
             *             overwrite those values as required while parsing the data.
             */
            int coded_width, coded_height;
        
            /**
             * the number of pictures in a group of pictures, or 0 for intra_only
             * - encoding: Set by user.
             * - decoding: unused
             */
            int gop_size;
        
            /**
             * Pixel format, see AV_PIX_FMT_xxx.
             * May be set by the demuxer if known from headers.
             * May be overridden by the decoder if it knows better.
             *
             * @note This field may not match the value of the last
             * AVFrame output by avcodec_receive_frame() due frame
             * reordering.
             *
             * - encoding: Set by user.
             * - decoding: Set by user if known, overridden by libavcodec while
             *             parsing the data.
             */
            enum AVPixelFormat pix_fmt;
        
            /**
             * If non NULL, 'draw_horiz_band' is called by the libavcodec
             * decoder to draw a horizontal band. It improves cache usage. Not
             * all codecs can do that. You must check the codec capabilities
             * beforehand.
             * When multithreading is used, it may be called from multiple threads
             * at the same time; threads might draw different parts of the same AVFrame,
             * or multiple AVFrames, and there is no guarantee that slices will be drawn
             * in order.
             * The function is also used by hardware acceleration APIs.
             * It is called at least once during frame decoding to pass
             * the data needed for hardware render.
             * In that mode instead of pixel data, AVFrame points to
             * a structure specific to the acceleration API. The application
             * reads the structure and can change some fields to indicate progress
             * or mark state.
             * - encoding: unused
             * - decoding: Set by user.
             * @param height the height of the slice
             * @param y the y position of the slice
             * @param type 1->top field, 2->bottom field, 3->frame
             * @param offset offset into the AVFrame.data from which the slice should be read
             */
            void (*draw_horiz_band)(struct AVCodecContext *s,
                                    const AVFrame *src, int offset[AV_NUM_DATA_POINTERS],
                                    int y, int type, int height);
        
            /**
             * callback to negotiate the pixelFormat
             * @param fmt is the list of formats which are supported by the codec,
             * it is terminated by -1 as 0 is a valid format, the formats are ordered by quality.
             * The first is always the native one.
             * @note The callback may be called again immediately if initialization for
             * the selected (hardware-accelerated) pixel format failed.
             * @warning Behavior is undefined if the callback returns a value not
             * in the fmt list of formats.
             * @return the chosen format
             * - encoding: unused
             * - decoding: Set by user, if not set the native format will be chosen.
             */
            enum AVPixelFormat (*get_format)(struct AVCodecContext *s, const enum AVPixelFormat * fmt);
        
            /**
             * maximum number of B-frames between non-B-frames
             * Note: The output will be delayed by max_b_frames+1 relative to the input.
             * - encoding: Set by user.
             * - decoding: unused
             */
            int max_b_frames;
        
            /**
             * qscale factor between IP and B-frames
             * If > 0 then the last P-frame quantizer will be used (q= lastp_q*factor+offset).
             * If < 0 then normal ratecontrol will be done (q= -normal_q*factor+offset).
             * - encoding: Set by user.
             * - decoding: unused
             */
            float b_quant_factor;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int b_frame_strategy;
        #endif
        
            /**
             * qscale offset between IP and B-frames
             * - encoding: Set by user.
             * - decoding: unused
             */
            float b_quant_offset;
        
            /**
             * Size of the frame reordering buffer in the decoder.
             * For MPEG-2 it is 1 IPB or 0 low delay IP.
             * - encoding: Set by libavcodec.
             * - decoding: Set by libavcodec.
             */
            int has_b_frames;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int mpeg_quant;
        #endif
        
            /**
             * qscale factor between P- and I-frames
             * If > 0 then the last P-frame quantizer will be used (q = lastp_q * factor + offset).
             * If < 0 then normal ratecontrol will be done (q= -normal_q*factor+offset).
             * - encoding: Set by user.
             * - decoding: unused
             */
            float i_quant_factor;
        
            /**
             * qscale offset between P and I-frames
             * - encoding: Set by user.
             * - decoding: unused
             */
            float i_quant_offset;
        
            /**
             * luminance masking (0-> disabled)
             * - encoding: Set by user.
             * - decoding: unused
             */
            float lumi_masking;
        
            /**
             * temporary complexity masking (0-> disabled)
             * - encoding: Set by user.
             * - decoding: unused
             */
            float temporal_cplx_masking;
        
            /**
             * spatial complexity masking (0-> disabled)
             * - encoding: Set by user.
             * - decoding: unused
             */
            float spatial_cplx_masking;
        
            /**
             * p block masking (0-> disabled)
             * - encoding: Set by user.
             * - decoding: unused
             */
            float p_masking;
        
            /**
             * darkness masking (0-> disabled)
             * - encoding: Set by user.
             * - decoding: unused
             */
            float dark_masking;
        
            /**
             * slice count
             * - encoding: Set by libavcodec.
             * - decoding: Set by user (or 0).
             */
            int slice_count;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
             int prediction_method;
        #define FF_PRED_LEFT   0
        #define FF_PRED_PLANE  1
        #define FF_PRED_MEDIAN 2
        #endif
        
            /**
             * slice offsets in the frame in bytes
             * - encoding: Set/allocated by libavcodec.
             * - decoding: Set/allocated by user (or NULL).
             */
            int *slice_offset;
        
            /**
             * sample aspect ratio (0 if unknown)
             * That is the width of a pixel divided by the height of the pixel.
             * Numerator and denominator must be relatively prime and smaller than 256 for some video standards.
             * - encoding: Set by user.
             * - decoding: Set by libavcodec.
             */
            AVRational sample_aspect_ratio;
        
            /**
             * motion estimation comparison function
             * - encoding: Set by user.
             * - decoding: unused
             */
            int me_cmp;
            /**
             * subpixel motion estimation comparison function
             * - encoding: Set by user.
             * - decoding: unused
             */
            int me_sub_cmp;
            /**
             * macroblock comparison function (not supported yet)
             * - encoding: Set by user.
             * - decoding: unused
             */
            int mb_cmp;
            /**
             * interlaced DCT comparison function
             * - encoding: Set by user.
             * - decoding: unused
             */
            int ildct_cmp;
        #define FF_CMP_SAD          0
        #define FF_CMP_SSE          1
        #define FF_CMP_SATD         2
        #define FF_CMP_DCT          3
        #define FF_CMP_PSNR         4
        #define FF_CMP_BIT          5
        #define FF_CMP_RD           6
        #define FF_CMP_ZERO         7
        #define FF_CMP_VSAD         8
        #define FF_CMP_VSSE         9
        #define FF_CMP_NSSE         10
        #define FF_CMP_W53          11
        #define FF_CMP_W97          12
        #define FF_CMP_DCTMAX       13
        #define FF_CMP_DCT264       14
        #define FF_CMP_MEDIAN_SAD   15
        #define FF_CMP_CHROMA       256
        
            /**
             * ME diamond size & shape
             * - encoding: Set by user.
             * - decoding: unused
             */
            int dia_size;
        
            /**
             * amount of previous MV predictors (2a+1 x 2a+1 square)
             * - encoding: Set by user.
             * - decoding: unused
             */
            int last_predictor_count;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int pre_me;
        #endif
        
            /**
             * motion estimation prepass comparison function
             * - encoding: Set by user.
             * - decoding: unused
             */
            int me_pre_cmp;
        
            /**
             * ME prepass diamond size & shape
             * - encoding: Set by user.
             * - decoding: unused
             */
            int pre_dia_size;
        
            /**
             * subpel ME quality
             * - encoding: Set by user.
             * - decoding: unused
             */
            int me_subpel_quality;
        
            /**
             * maximum motion estimation search range in subpel units
             * If 0 then no limit.
             *
             * - encoding: Set by user.
             * - decoding: unused
             */
            int me_range;
        
            /**
             * slice flags
             * - encoding: unused
             * - decoding: Set by user.
             */
            int slice_flags;
        #define SLICE_FLAG_CODED_ORDER    0x0001 ///< draw_horiz_band() is called in coded order instead of display
        #define SLICE_FLAG_ALLOW_FIELD    0x0002 ///< allow draw_horiz_band() with field slices (MPEG-2 field pics)
        #define SLICE_FLAG_ALLOW_PLANE    0x0004 ///< allow draw_horiz_band() with 1 component at a time (SVQ1)
        
            /**
             * macroblock decision mode
             * - encoding: Set by user.
             * - decoding: unused
             */
            int mb_decision;
        #define FF_MB_DECISION_SIMPLE 0        ///< uses mb_cmp
        #define FF_MB_DECISION_BITS   1        ///< chooses the one which needs the fewest bits
        #define FF_MB_DECISION_RD     2        ///< rate distortion
        
            /**
             * custom intra quantization matrix
             * Must be allocated with the av_malloc() family of functions, and will be freed in
             * avcodec_free_context().
             * - encoding: Set/allocated by user, freed by libavcodec. Can be NULL.
             * - decoding: Set/allocated/freed by libavcodec.
             */
            uint16_t *intra_matrix;
        
            /**
             * custom inter quantization matrix
             * Must be allocated with the av_malloc() family of functions, and will be freed in
             * avcodec_free_context().
             * - encoding: Set/allocated by user, freed by libavcodec. Can be NULL.
             * - decoding: Set/allocated/freed by libavcodec.
             */
            uint16_t *inter_matrix;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int scenechange_threshold;
        
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int noise_reduction;
        #endif
        
            /**
             * precision of the intra DC coefficient - 8
             * - encoding: Set by user.
             * - decoding: Set by libavcodec
             */
            int intra_dc_precision;
        
            /**
             * Number of macroblock rows at the top which are skipped.
             * - encoding: unused
             * - decoding: Set by user.
             */
            int skip_top;
        
            /**
             * Number of macroblock rows at the bottom which are skipped.
             * - encoding: unused
             * - decoding: Set by user.
             */
            int skip_bottom;
        
            /**
             * minimum MB Lagrange multiplier
             * - encoding: Set by user.
             * - decoding: unused
             */
            int mb_lmin;
        
            /**
             * maximum MB Lagrange multiplier
             * - encoding: Set by user.
             * - decoding: unused
             */
            int mb_lmax;
        
        #if FF_API_PRIVATE_OPT
            /**
             * @deprecated use encoder private options instead
             */
            attribute_deprecated
            int me_penalty_compensation;
        #endif
        
            /**
             * - encoding: Set by user.
             * - decoding: unused
             */
            int bidir_refine;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int brd_scale;
        #endif
        
            /**
             * minimum GOP size
             * - encoding: Set by user.
             * - decoding: unused
             */
            int keyint_min;
        
            /**
             * number of reference frames
             * - encoding: Set by user.
             * - decoding: Set by lavc.
             */
            int refs;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int chromaoffset;
        #endif
        
            /**
             * Note: Value depends upon the compare function used for fullpel ME.
             * - encoding: Set by user.
             * - decoding: unused
             */
            int mv0_threshold;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int b_sensitivity;
        #endif
        
            /**
             * Chromaticity coordinates of the source primaries.
             * - encoding: Set by user
             * - decoding: Set by libavcodec
             */
            enum AVColorPrimaries color_primaries;
        
            /**
             * Color Transfer Characteristic.
             * - encoding: Set by user
             * - decoding: Set by libavcodec
             */
            enum AVColorTransferCharacteristic color_trc;
        
            /**
             * YUV colorspace type.
             * - encoding: Set by user
             * - decoding: Set by libavcodec
             */
            enum AVColorSpace colorspace;
        
            /**
             * MPEG vs JPEG YUV range.
             * - encoding: Set by user
             * - decoding: Set by libavcodec
             */
            enum AVColorRange color_range;
        
            /**
             * This defines the location of chroma samples.
             * - encoding: Set by user
             * - decoding: Set by libavcodec
             */
            enum AVChromaLocation chroma_sample_location;
        
            /**
             * Number of slices.
             * Indicates number of picture subdivisions. Used for parallelized
             * decoding.
             * - encoding: Set by user
             * - decoding: unused
             */
            int slices;
        
            /** Field order
             * - encoding: set by libavcodec
             * - decoding: Set by user.
             */
            enum AVFieldOrder field_order;
        
            /* audio only */
            int sample_rate; ///< samples per second
            int channels;    ///< number of audio channels
        
            /**
             * audio sample format
             * - encoding: Set by user.
             * - decoding: Set by libavcodec.
             */
            enum AVSampleFormat sample_fmt;  ///< sample format
        
            /* The following data should not be initialized. */
            /**
             * Number of samples per channel in an audio frame.
             *
             * - encoding: set by libavcodec in avcodec_open2(). Each submitted frame
             *   except the last must contain exactly frame_size samples per channel.
             *   May be 0 when the codec has AV_CODEC_CAP_VARIABLE_FRAME_SIZE set, then the
             *   frame size is not restricted.
             * - decoding: may be set by some decoders to indicate constant frame size
             */
            int frame_size;
        
            /**
             * Frame counter, set by libavcodec.
             *
             * - decoding: total number of frames returned from the decoder so far.
             * - encoding: total number of frames passed to the encoder so far.
             *
             *   @note the counter is not incremented if encoding/decoding resulted in
             *   an error.
             */
            int frame_number;
        
            /**
             * number of bytes per packet if constant and known or 0
             * Used by some WAV based audio codecs.
             */
            int block_align;
        
            /**
             * Audio cutoff bandwidth (0 means "automatic")
             * - encoding: Set by user.
             * - decoding: unused
             */
            int cutoff;
        
            /**
             * Audio channel layout.
             * - encoding: set by user.
             * - decoding: set by user, may be overwritten by libavcodec.
             */
            uint64_t channel_layout;
        
            /**
             * Request decoder to use this channel layout if it can (0 for default)
             * - encoding: unused
             * - decoding: Set by user.
             */
            uint64_t request_channel_layout;
        
            /**
             * Type of service that the audio stream conveys.
             * - encoding: Set by user.
             * - decoding: Set by libavcodec.
             */
            enum AVAudioServiceType audio_service_type;
        
            /**
             * desired sample format
             * - encoding: Not used.
             * - decoding: Set by user.
             * Decoder will decode to this format if it can.
             */
            enum AVSampleFormat request_sample_fmt;
        
            /**
             * This callback is called at the beginning of each frame to get data
             * buffer(s) for it. There may be one contiguous buffer for all the data or
             * there may be a buffer per each data plane or anything in between. What
             * this means is, you may set however many entries in buf[] you feel necessary.
             * Each buffer must be reference-counted using the AVBuffer API (see description
             * of buf[] below).
             *
             * The following fields will be set in the frame before this callback is
             * called:
             * - format
             * - width, height (video only)
             * - sample_rate, channel_layout, nb_samples (audio only)
             * Their values may differ from the corresponding values in
             * AVCodecContext. This callback must use the frame values, not the codec
             * context values, to calculate the required buffer size.
             *
             * This callback must fill the following fields in the frame:
             * - data[]
             * - linesize[]
             * - extended_data:
             *   * if the data is planar audio with more than 8 channels, then this
             *     callback must allocate and fill extended_data to contain all pointers
             *     to all data planes. data[] must hold as many pointers as it can.
             *     extended_data must be allocated with av_malloc() and will be freed in
             *     av_frame_unref().
             *   * otherwise extended_data must point to data
             * - buf[] must contain one or more pointers to AVBufferRef structures. Each of
             *   the frame's data and extended_data pointers must be contained in these. That
             *   is, one AVBufferRef for each allocated chunk of memory, not necessarily one
             *   AVBufferRef per data[] entry. See: av_buffer_create(), av_buffer_alloc(),
             *   and av_buffer_ref().
             * - extended_buf and nb_extended_buf must be allocated with av_malloc() by
             *   this callback and filled with the extra buffers if there are more
             *   buffers than buf[] can hold. extended_buf will be freed in
             *   av_frame_unref().
             *
             * If AV_CODEC_CAP_DR1 is not set then get_buffer2() must call
             * avcodec_default_get_buffer2() instead of providing buffers allocated by
             * some other means.
             *
             * Each data plane must be aligned to the maximum required by the target
             * CPU.
             *
             * @see avcodec_default_get_buffer2()
             *
             * Video:
             *
             * If AV_GET_BUFFER_FLAG_REF is set in flags then the frame may be reused
             * (read and/or written to if it is writable) later by libavcodec.
             *
             * avcodec_align_dimensions2() should be used to find the required width and
             * height, as they normally need to be rounded up to the next multiple of 16.
             *
             * Some decoders do not support linesizes changing between frames.
             *
             * If frame multithreading is used and thread_safe_callbacks is set,
             * this callback may be called from a different thread, but not from more
             * than one at once. Does not need to be reentrant.
             *
             * @see avcodec_align_dimensions2()
             *
             * Audio:
             *
             * Decoders request a buffer of a particular size by setting
             * AVFrame.nb_samples prior to calling get_buffer2(). The decoder may,
             * however, utilize only part of the buffer by setting AVFrame.nb_samples
             * to a smaller value in the output frame.
             *
             * As a convenience, av_samples_get_buffer_size() and
             * av_samples_fill_arrays() in libavutil may be used by custom get_buffer2()
             * functions to find the required data size and to fill data pointers and
             * linesize. In AVFrame.linesize, only linesize[0] may be set for audio
             * since all planes must be the same size.
             *
             * @see av_samples_get_buffer_size(), av_samples_fill_arrays()
             *
             * - encoding: unused
             * - decoding: Set by libavcodec, user can override.
             */
            int (*get_buffer2)(struct AVCodecContext *s, AVFrame *frame, int flags);
        
            /**
             * If non-zero, the decoded audio and video frames returned from
             * avcodec_decode_video2() and avcodec_decode_audio4() are reference-counted
             * and are valid indefinitely. The caller must free them with
             * av_frame_unref() when they are not needed anymore.
             * Otherwise, the decoded frames must not be freed by the caller and are
             * only valid until the next decode call.
             *
             * This is always automatically enabled if avcodec_receive_frame() is used.
             *
             * - encoding: unused
             * - decoding: set by the caller before avcodec_open2().
             */
            attribute_deprecated
            int refcounted_frames;
        
            /* - encoding parameters */
            float qcompress;  ///< amount of qscale change between easy & hard scenes (0.0-1.0)
            float qblur;      ///< amount of qscale smoothing over time (0.0-1.0)
        
            /**
             * minimum quantizer
             * - encoding: Set by user.
             * - decoding: unused
             */
            int qmin;
        
            /**
             * maximum quantizer
             * - encoding: Set by user.
             * - decoding: unused
             */
            int qmax;
        
            /**
             * maximum quantizer difference between frames
             * - encoding: Set by user.
             * - decoding: unused
             */
            int max_qdiff;
        
            /**
             * decoder bitstream buffer size
             * - encoding: Set by user.
             * - decoding: unused
             */
            int rc_buffer_size;
        
            /**
             * ratecontrol override, see RcOverride
             * - encoding: Allocated/set/freed by user.
             * - decoding: unused
             */
            int rc_override_count;
            RcOverride *rc_override;
        
            /**
             * maximum bitrate
             * - encoding: Set by user.
             * - decoding: Set by user, may be overwritten by libavcodec.
             */
            int64_t rc_max_rate;
        
            /**
             * minimum bitrate
             * - encoding: Set by user.
             * - decoding: unused
             */
            int64_t rc_min_rate;
        
            /**
             * Ratecontrol attempt to use, at maximum, <value> of what can be used without an underflow.
             * - encoding: Set by user.
             * - decoding: unused.
             */
            float rc_max_available_vbv_use;
        
            /**
             * Ratecontrol attempt to use, at least, <value> times the amount needed to prevent a vbv overflow.
             * - encoding: Set by user.
             * - decoding: unused.
             */
            float rc_min_vbv_overflow_use;
        
            /**
             * Number of bits which should be loaded into the rc buffer before decoding starts.
             * - encoding: Set by user.
             * - decoding: unused
             */
            int rc_initial_buffer_occupancy;
        
        #if FF_API_CODER_TYPE
        #define FF_CODER_TYPE_VLC       0
        #define FF_CODER_TYPE_AC        1
        #define FF_CODER_TYPE_RAW       2
        #define FF_CODER_TYPE_RLE       3
            /**
             * @deprecated use encoder private options instead
             */
            attribute_deprecated
            int coder_type;
        #endif /* FF_API_CODER_TYPE */
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int context_model;
        #endif
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int frame_skip_threshold;
        
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int frame_skip_factor;
        
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int frame_skip_exp;
        
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int frame_skip_cmp;
        #endif /* FF_API_PRIVATE_OPT */
        
            /**
             * trellis RD quantization
             * - encoding: Set by user.
             * - decoding: unused
             */
            int trellis;
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int min_prediction_order;
        
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int max_prediction_order;
        
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int64_t timecode_frame_start;
        #endif
        
        #if FF_API_RTP_CALLBACK
            /**
             * @deprecated unused
             */
            /* The RTP callback: This function is called    */
            /* every time the encoder has a packet to send. */
            /* It depends on the encoder if the data starts */
            /* with a Start Code (it should). H.263 does.   */
            /* mb_nb contains the number of macroblocks     */
            /* encoded in the RTP payload.                  */
            attribute_deprecated
            void (*rtp_callback)(struct AVCodecContext *avctx, void *data, int size, int mb_nb);
        #endif
        
        #if FF_API_PRIVATE_OPT
            /** @deprecated use encoder private options instead */
            attribute_deprecated
            int rtp_payload_size;   /* The size of the RTP payload: the coder will  */
                                    /* do its best to deliver a chunk with size     */
                                    /* below rtp_payload_size, the chunk will start */
                                    /* with a start code on some codecs like H.263. */
                                    /* This doesn't take account of any particular  */
                                    /* headers inside the transmitted RTP payload.  */
        #endif
        
        #if FF_API_STAT_BITS
            /* statistics, used for 2-pass encoding */
            attribute_deprecated
            int mv_bits;
            attribute_deprecated
            int header_bits;
            attribute_deprecated
            int i_tex_bits;
            attribute_deprecated
            int p_tex_bits;
            attribute_deprecated
            int i_count;
            attribute_deprecated
            int p_count;
            attribute_deprecated
            int skip_count;
            attribute_deprecated
            int misc_bits;
        
            /** @deprecated this field is unused */
            attribute_deprecated
            int frame_bits;
        #endif
        
            /**
             * pass1 encoding statistics output buffer
             * - encoding: Set by libavcodec.
             * - decoding: unused
             */
            char *stats_out;
        
            /**
             * pass2 encoding statistics input buffer
             * Concatenated stuff from stats_out of pass1 should be placed here.
             * - encoding: Allocated/set/freed by user.
             * - decoding: unused
             */
            char *stats_in;
        
            /**
             * Work around bugs in encoders which sometimes cannot be detected automatically.
             * - encoding: Set by user
             * - decoding: Set by user
             */
            int workaround_bugs;
        #define FF_BUG_AUTODETECT       1  ///< autodetection
        #define FF_BUG_XVID_ILACE       4
        #define FF_BUG_UMP4             8
        #define FF_BUG_NO_PADDING       16
        #define FF_BUG_AMV              32
        #define FF_BUG_QPEL_CHROMA      64
        #define FF_BUG_STD_QPEL         128
        #define FF_BUG_QPEL_CHROMA2     256
        #define FF_BUG_DIRECT_BLOCKSIZE 512
        #define FF_BUG_EDGE             1024
        #define FF_BUG_HPEL_CHROMA      2048
        #define FF_BUG_DC_CLIP          4096
        #define FF_BUG_MS               8192 ///< Work around various bugs in Microsoft's broken decoders.
        #define FF_BUG_TRUNCATED       16384
        #define FF_BUG_IEDGE           32768
        
            /**
             * strictly follow the standard (MPEG-4, ...).
             * - encoding: Set by user.
             * - decoding: Set by user.
             * Setting this to STRICT or higher means the encoder and decoder will
             * generally do stupid things, whereas setting it to unofficial or lower
             * will mean the encoder might produce output that is not supported by all
             * spec-compliant decoders. Decoders don't differentiate between normal,
             * unofficial and experimental (that is, they always try to decode things
             * when they can) unless they are explicitly asked to behave stupidly
             * (=strictly conform to the specs)
             */
            int strict_std_compliance;
        #define FF_COMPLIANCE_VERY_STRICT   2 ///< Strictly conform to an older more strict version of the spec or reference software.
        #define FF_COMPLIANCE_STRICT        1 ///< Strictly conform to all the things in the spec no matter what consequences.
        #define FF_COMPLIANCE_NORMAL        0
        #define FF_COMPLIANCE_UNOFFICIAL   -1 ///< Allow unofficial extensions
        #define FF_COMPLIANCE_EXPERIMENTAL -2 ///< Allow nonstandardized experimental things.
        
            /**
             * error concealment flags
             * - encoding: unused
             * - decoding: Set by user.
             */
            int error_concealment;
        #define FF_EC_GUESS_MVS   1
        #define FF_EC_DEBLOCK     2
        #define FF_EC_FAVOR_INTER 256
        
            /**
             * debug
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            int debug;
        #define FF_DEBUG_PICT_INFO   1
        #define FF_DEBUG_RC          2
        #define FF_DEBUG_BITSTREAM   4
        #define FF_DEBUG_MB_TYPE     8
        #define FF_DEBUG_QP          16
        #if FF_API_DEBUG_MV
        /**
         * @deprecated this option does nothing
         */
        #define FF_DEBUG_MV          32
        #endif
        #define FF_DEBUG_DCT_COEFF   0x00000040
        #define FF_DEBUG_SKIP        0x00000080
        #define FF_DEBUG_STARTCODE   0x00000100
        #define FF_DEBUG_ER          0x00000400
        #define FF_DEBUG_MMCO        0x00000800
        #define FF_DEBUG_BUGS        0x00001000
        #if FF_API_DEBUG_MV
        #define FF_DEBUG_VIS_QP      0x00002000
        #define FF_DEBUG_VIS_MB_TYPE 0x00004000
        #endif
        #define FF_DEBUG_BUFFERS     0x00008000
        #define FF_DEBUG_THREADS     0x00010000
        #define FF_DEBUG_GREEN_MD    0x00800000
        #define FF_DEBUG_NOMC        0x01000000
        
        #if FF_API_DEBUG_MV
            /**
             * debug
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            int debug_mv;
        #define FF_DEBUG_VIS_MV_P_FOR  0x00000001 // visualize forward predicted MVs of P-frames
        #define FF_DEBUG_VIS_MV_B_FOR  0x00000002 // visualize forward predicted MVs of B-frames
        #define FF_DEBUG_VIS_MV_B_BACK 0x00000004 // visualize backward predicted MVs of B-frames
        #endif
        
            /**
             * Error recognition; may misdetect some more or less valid parts as errors.
             * - encoding: unused
             * - decoding: Set by user.
             */
            int err_recognition;
        
        /**
         * Verify checksums embedded in the bitstream (could be of either encoded or
         * decoded data, depending on the codec) and print an error message on mismatch.
         * If AV_EF_EXPLODE is also set, a mismatching checksum will result in the
         * decoder returning an error.
         */
        #define AV_EF_CRCCHECK  (1<<0)
        #define AV_EF_BITSTREAM (1<<1)          ///< detect bitstream specification deviations
        #define AV_EF_BUFFER    (1<<2)          ///< detect improper bitstream length
        #define AV_EF_EXPLODE   (1<<3)          ///< abort decoding on minor error detection
        
        #define AV_EF_IGNORE_ERR (1<<15)        ///< ignore errors and continue
        #define AV_EF_CAREFUL    (1<<16)        ///< consider things that violate the spec, are fast to calculate and have not been seen in the wild as errors
        #define AV_EF_COMPLIANT  (1<<17)        ///< consider all spec non compliances as errors
        #define AV_EF_AGGRESSIVE (1<<18)        ///< consider things that a sane encoder should not do as an error
        
        
            /**
             * opaque 64-bit number (generally a PTS) that will be reordered and
             * output in AVFrame.reordered_opaque
             * - encoding: Set by libavcodec to the reordered_opaque of the input
             *             frame corresponding to the last returned packet. Only
             *             supported by encoders with the
             *             AV_CODEC_CAP_ENCODER_REORDERED_OPAQUE capability.
             * - decoding: Set by user.
             */
            int64_t reordered_opaque;
        
            /**
             * Hardware accelerator in use
             * - encoding: unused.
             * - decoding: Set by libavcodec
             */
            const struct AVHWAccel *hwaccel;
        
            /**
             * Hardware accelerator context.
             * For some hardware accelerators, a global context needs to be
             * provided by the user. In that case, this holds display-dependent
             * data FFmpeg cannot instantiate itself. Please refer to the
             * FFmpeg HW accelerator documentation to know how to fill this
             * is. e.g. for VA API, this is a struct vaapi_context.
             * - encoding: unused
             * - decoding: Set by user
             */
            void *hwaccel_context;
        
            /**
             * error
             * - encoding: Set by libavcodec if flags & AV_CODEC_FLAG_PSNR.
             * - decoding: unused
             */
            uint64_t error[AV_NUM_DATA_POINTERS];
        
            /**
             * DCT algorithm, see FF_DCT_* below
             * - encoding: Set by user.
             * - decoding: unused
             */
            int dct_algo;
        #define FF_DCT_AUTO    0
        #define FF_DCT_FASTINT 1
        #define FF_DCT_INT     2
        #define FF_DCT_MMX     3
        #define FF_DCT_ALTIVEC 5
        #define FF_DCT_FAAN    6
        
            /**
             * IDCT algorithm, see FF_IDCT_* below.
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            int idct_algo;
        #define FF_IDCT_AUTO          0
        #define FF_IDCT_INT           1
        #define FF_IDCT_SIMPLE        2
        #define FF_IDCT_SIMPLEMMX     3
        #define FF_IDCT_ARM           7
        #define FF_IDCT_ALTIVEC       8
        #define FF_IDCT_SIMPLEARM     10
        #define FF_IDCT_XVID          14
        #define FF_IDCT_SIMPLEARMV5TE 16
        #define FF_IDCT_SIMPLEARMV6   17
        #define FF_IDCT_FAAN          20
        #define FF_IDCT_SIMPLENEON    22
        #define FF_IDCT_NONE          24 /* Used by XvMC to extract IDCT coefficients with FF_IDCT_PERM_NONE */
        #define FF_IDCT_SIMPLEAUTO    128
        
            /**
             * bits per sample/pixel from the demuxer (needed for huffyuv).
             * - encoding: Set by libavcodec.
             * - decoding: Set by user.
             */
             int bits_per_coded_sample;
        
            /**
             * Bits per sample/pixel of internal libavcodec pixel/sample format.
             * - encoding: set by user.
             * - decoding: set by libavcodec.
             */
            int bits_per_raw_sample;
        
        #if FF_API_LOWRES
            /**
             * low resolution decoding, 1-> 1/2 size, 2->1/4 size
             * - encoding: unused
             * - decoding: Set by user.
             */
             int lowres;
        #endif
        
        #if FF_API_CODED_FRAME
            /**
             * the picture in the bitstream
             * - encoding: Set by libavcodec.
             * - decoding: unused
             *
             * @deprecated use the quality factor packet side data instead
             */
            attribute_deprecated AVFrame *coded_frame;
        #endif
        
            /**
             * thread count
             * is used to decide how many independent tasks should be passed to execute()
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            int thread_count;
        
            /**
             * Which multithreading methods to use.
             * Use of FF_THREAD_FRAME will increase decoding delay by one frame per thread,
             * so clients which cannot provide future frames should not use it.
             *
             * - encoding: Set by user, otherwise the default is used.
             * - decoding: Set by user, otherwise the default is used.
             */
            int thread_type;
        #define FF_THREAD_FRAME   1 ///< Decode more than one frame at once
        #define FF_THREAD_SLICE   2 ///< Decode more than one part of a single frame at once
        
            /**
             * Which multithreading methods are in use by the codec.
             * - encoding: Set by libavcodec.
             * - decoding: Set by libavcodec.
             */
            int active_thread_type;
        
            /**
             * Set by the client if its custom get_buffer() callback can be called
             * synchronously from another thread, which allows faster multithreaded decoding.
             * draw_horiz_band() will be called from other threads regardless of this setting.
             * Ignored if the default get_buffer() is used.
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            int thread_safe_callbacks;
        
            /**
             * The codec may call this to execute several independent things.
             * It will return only after finishing all tasks.
             * The user may replace this with some multithreaded implementation,
             * the default implementation will execute the parts serially.
             * @param count the number of things to execute
             * - encoding: Set by libavcodec, user can override.
             * - decoding: Set by libavcodec, user can override.
             */
            int (*execute)(struct AVCodecContext *c, int (*func)(struct AVCodecContext *c2, void *arg), void *arg2, int *ret, int count, int size);
        
            /**
             * The codec may call this to execute several independent things.
             * It will return only after finishing all tasks.
             * The user may replace this with some multithreaded implementation,
             * the default implementation will execute the parts serially.
             * Also see avcodec_thread_init and e.g. the --enable-pthread configure option.
             * @param c context passed also to func
             * @param count the number of things to execute
             * @param arg2 argument passed unchanged to func
             * @param ret return values of executed functions, must have space for "count" values. May be NULL.
             * @param func function that will be called count times, with jobnr from 0 to count-1.
             *             threadnr will be in the range 0 to c->thread_count-1 < MAX_THREADS and so that no
             *             two instances of func executing at the same time will have the same threadnr.
             * @return always 0 currently, but code should handle a future improvement where when any call to func
             *         returns < 0 no further calls to func may be done and < 0 is returned.
             * - encoding: Set by libavcodec, user can override.
             * - decoding: Set by libavcodec, user can override.
             */
            int (*execute2)(struct AVCodecContext *c, int (*func)(struct AVCodecContext *c2, void *arg, int jobnr, int threadnr), void *arg2, int *ret, int count);
        
            /**
             * noise vs. sse weight for the nsse comparison function
             * - encoding: Set by user.
             * - decoding: unused
             */
             int nsse_weight;
        
            /**
             * profile
             * - encoding: Set by user.
             * - decoding: Set by libavcodec.
             */
             int profile;
        #define FF_PROFILE_UNKNOWN -99
        #define FF_PROFILE_RESERVED -100
        
        #define FF_PROFILE_AAC_MAIN 0
        #define FF_PROFILE_AAC_LOW  1
        #define FF_PROFILE_AAC_SSR  2
        #define FF_PROFILE_AAC_LTP  3
        #define FF_PROFILE_AAC_HE   4
        #define FF_PROFILE_AAC_HE_V2 28
        #define FF_PROFILE_AAC_LD   22
        #define FF_PROFILE_AAC_ELD  38
        #define FF_PROFILE_MPEG2_AAC_LOW 128
        #define FF_PROFILE_MPEG2_AAC_HE  131
        
        #define FF_PROFILE_DNXHD         0
        #define FF_PROFILE_DNXHR_LB      1
        #define FF_PROFILE_DNXHR_SQ      2
        #define FF_PROFILE_DNXHR_HQ      3
        #define FF_PROFILE_DNXHR_HQX     4
        #define FF_PROFILE_DNXHR_444     5
        
        #define FF_PROFILE_DTS         20
        #define FF_PROFILE_DTS_ES      30
        #define FF_PROFILE_DTS_96_24   40
        #define FF_PROFILE_DTS_HD_HRA  50
        #define FF_PROFILE_DTS_HD_MA   60
        #define FF_PROFILE_DTS_EXPRESS 70
        
        #define FF_PROFILE_MPEG2_422    0
        #define FF_PROFILE_MPEG2_HIGH   1
        #define FF_PROFILE_MPEG2_SS     2
        #define FF_PROFILE_MPEG2_SNR_SCALABLE  3
        #define FF_PROFILE_MPEG2_MAIN   4
        #define FF_PROFILE_MPEG2_SIMPLE 5
        
        #define FF_PROFILE_H264_CONSTRAINED  (1<<9)  // 8+1; constraint_set1_flag
        #define FF_PROFILE_H264_INTRA        (1<<11) // 8+3; constraint_set3_flag
        
        #define FF_PROFILE_H264_BASELINE             66
        #define FF_PROFILE_H264_CONSTRAINED_BASELINE (66|FF_PROFILE_H264_CONSTRAINED)
        #define FF_PROFILE_H264_MAIN                 77
        #define FF_PROFILE_H264_EXTENDED             88
        #define FF_PROFILE_H264_HIGH                 100
        #define FF_PROFILE_H264_HIGH_10              110
        #define FF_PROFILE_H264_HIGH_10_INTRA        (110|FF_PROFILE_H264_INTRA)
        #define FF_PROFILE_H264_MULTIVIEW_HIGH       118
        #define FF_PROFILE_H264_HIGH_422             122
        #define FF_PROFILE_H264_HIGH_422_INTRA       (122|FF_PROFILE_H264_INTRA)
        #define FF_PROFILE_H264_STEREO_HIGH          128
        #define FF_PROFILE_H264_HIGH_444             144
        #define FF_PROFILE_H264_HIGH_444_PREDICTIVE  244
        #define FF_PROFILE_H264_HIGH_444_INTRA       (244|FF_PROFILE_H264_INTRA)
        #define FF_PROFILE_H264_CAVLC_444            44
        
        #define FF_PROFILE_VC1_SIMPLE   0
        #define FF_PROFILE_VC1_MAIN     1
        #define FF_PROFILE_VC1_COMPLEX  2
        #define FF_PROFILE_VC1_ADVANCED 3
        
        #define FF_PROFILE_MPEG4_SIMPLE                     0
        #define FF_PROFILE_MPEG4_SIMPLE_SCALABLE            1
        #define FF_PROFILE_MPEG4_CORE                       2
        #define FF_PROFILE_MPEG4_MAIN                       3
        #define FF_PROFILE_MPEG4_N_BIT                      4
        #define FF_PROFILE_MPEG4_SCALABLE_TEXTURE           5
        #define FF_PROFILE_MPEG4_SIMPLE_FACE_ANIMATION      6
        #define FF_PROFILE_MPEG4_BASIC_ANIMATED_TEXTURE     7
        #define FF_PROFILE_MPEG4_HYBRID                     8
        #define FF_PROFILE_MPEG4_ADVANCED_REAL_TIME         9
        #define FF_PROFILE_MPEG4_CORE_SCALABLE             10
        #define FF_PROFILE_MPEG4_ADVANCED_CODING           11
        #define FF_PROFILE_MPEG4_ADVANCED_CORE             12
        #define FF_PROFILE_MPEG4_ADVANCED_SCALABLE_TEXTURE 13
        #define FF_PROFILE_MPEG4_SIMPLE_STUDIO             14
        #define FF_PROFILE_MPEG4_ADVANCED_SIMPLE           15
        
        #define FF_PROFILE_JPEG2000_CSTREAM_RESTRICTION_0   1
        #define FF_PROFILE_JPEG2000_CSTREAM_RESTRICTION_1   2
        #define FF_PROFILE_JPEG2000_CSTREAM_NO_RESTRICTION  32768
        #define FF_PROFILE_JPEG2000_DCINEMA_2K              3
        #define FF_PROFILE_JPEG2000_DCINEMA_4K              4
        
        #define FF_PROFILE_VP9_0                            0
        #define FF_PROFILE_VP9_1                            1
        #define FF_PROFILE_VP9_2                            2
        #define FF_PROFILE_VP9_3                            3
        
        #define FF_PROFILE_HEVC_MAIN                        1
        #define FF_PROFILE_HEVC_MAIN_10                     2
        #define FF_PROFILE_HEVC_MAIN_STILL_PICTURE          3
        #define FF_PROFILE_HEVC_REXT                        4
        
        #define FF_PROFILE_AV1_MAIN                         0
        #define FF_PROFILE_AV1_HIGH                         1
        #define FF_PROFILE_AV1_PROFESSIONAL                 2
        
        #define FF_PROFILE_MJPEG_HUFFMAN_BASELINE_DCT            0xc0
        #define FF_PROFILE_MJPEG_HUFFMAN_EXTENDED_SEQUENTIAL_DCT 0xc1
        #define FF_PROFILE_MJPEG_HUFFMAN_PROGRESSIVE_DCT         0xc2
        #define FF_PROFILE_MJPEG_HUFFMAN_LOSSLESS                0xc3
        #define FF_PROFILE_MJPEG_JPEG_LS                         0xf7
        
        #define FF_PROFILE_SBC_MSBC                         1
        
        #define FF_PROFILE_PRORES_PROXY     0
        #define FF_PROFILE_PRORES_LT        1
        #define FF_PROFILE_PRORES_STANDARD  2
        #define FF_PROFILE_PRORES_HQ        3
        #define FF_PROFILE_PRORES_4444      4
        #define FF_PROFILE_PRORES_XQ        5
        
        #define FF_PROFILE_ARIB_PROFILE_A 0
        #define FF_PROFILE_ARIB_PROFILE_C 1
        
        #define FF_PROFILE_KLVA_SYNC 0
        #define FF_PROFILE_KLVA_ASYNC 1
        
            /**
             * level
             * - encoding: Set by user.
             * - decoding: Set by libavcodec.
             */
             int level;
        #define FF_LEVEL_UNKNOWN -99
        
            /**
             * Skip loop filtering for selected frames.
             * - encoding: unused
             * - decoding: Set by user.
             */
            enum AVDiscard skip_loop_filter;
        
            /**
             * Skip IDCT/dequantization for selected frames.
             * - encoding: unused
             * - decoding: Set by user.
             */
            enum AVDiscard skip_idct;
        
            /**
             * Skip decoding for selected frames.
             * - encoding: unused
             * - decoding: Set by user.
             */
            enum AVDiscard skip_frame;
        
            /**
             * Header containing style information for text subtitles.
             * For SUBTITLE_ASS subtitle type, it should contain the whole ASS
             * [Script Info] and [V4+ Styles] section, plus the [Events] line and
             * the Format line following. It shouldn't include any Dialogue line.
             * - encoding: Set/allocated/freed by user (before avcodec_open2())
             * - decoding: Set/allocated/freed by libavcodec (by avcodec_open2())
             */
            uint8_t *subtitle_header;
            int subtitle_header_size;
        
        #if FF_API_VBV_DELAY
            /**
             * VBV delay coded in the last frame (in periods of a 27 MHz clock).
             * Used for compliant TS muxing.
             * - encoding: Set by libavcodec.
             * - decoding: unused.
             * @deprecated this value is now exported as a part of
             * AV_PKT_DATA_CPB_PROPERTIES packet side data
             */
            attribute_deprecated
            uint64_t vbv_delay;
        #endif
        
        #if FF_API_SIDEDATA_ONLY_PKT
            /**
             * Encoding only and set by default. Allow encoders to output packets
             * that do not contain any encoded data, only side data.
             *
             * Some encoders need to output such packets, e.g. to update some stream
             * parameters at the end of encoding.
             *
             * @deprecated this field disables the default behaviour and
             *             it is kept only for compatibility.
             */
            attribute_deprecated
            int side_data_only_packets;
        #endif
        
            /**
             * Audio only. The number of "priming" samples (padding) inserted by the
             * encoder at the beginning of the audio. I.e. this number of leading
             * decoded samples must be discarded by the caller to get the original audio
             * without leading padding.
             *
             * - decoding: unused
             * - encoding: Set by libavcodec. The timestamps on the output packets are
             *             adjusted by the encoder so that they always refer to the
             *             first sample of the data actually contained in the packet,
             *             including any added padding.  E.g. if the timebase is
             *             1/samplerate and the timestamp of the first input sample is
             *             0, the timestamp of the first output packet will be
             *             -initial_padding.
             */
            int initial_padding;
        
            /**
             * - decoding: For codecs that store a framerate value in the compressed
             *             bitstream, the decoder may export it here. { 0, 1} when
             *             unknown.
             * - encoding: May be used to signal the framerate of CFR content to an
             *             encoder.
             */
            AVRational framerate;
        
            /**
             * Nominal unaccelerated pixel format, see AV_PIX_FMT_xxx.
             * - encoding: unused.
             * - decoding: Set by libavcodec before calling get_format()
             */
            enum AVPixelFormat sw_pix_fmt;
        
            /**
             * Timebase in which pkt_dts/pts and AVPacket.dts/pts are.
             * - encoding unused.
             * - decoding set by user.
             */
            AVRational pkt_timebase;
        
            /**
             * AVCodecDescriptor
             * - encoding: unused.
             * - decoding: set by libavcodec.
             */
            const AVCodecDescriptor *codec_descriptor;
        
        #if !FF_API_LOWRES
            /**
             * low resolution decoding, 1-> 1/2 size, 2->1/4 size
             * - encoding: unused
             * - decoding: Set by user.
             */
             int lowres;
        #endif
        
            /**
             * Current statistics for PTS correction.
             * - decoding: maintained and used by libavcodec, not intended to be used by user apps
             * - encoding: unused
             */
            int64_t pts_correction_num_faulty_pts; /// Number of incorrect PTS values so far
            int64_t pts_correction_num_faulty_dts; /// Number of incorrect DTS values so far
            int64_t pts_correction_last_pts;       /// PTS of the last frame
            int64_t pts_correction_last_dts;       /// DTS of the last frame
        
            /**
             * Character encoding of the input subtitles file.
             * - decoding: set by user
             * - encoding: unused
             */
            char *sub_charenc;
        
            /**
             * Subtitles character encoding mode. Formats or codecs might be adjusting
             * this setting (if they are doing the conversion themselves for instance).
             * - decoding: set by libavcodec
             * - encoding: unused
             */
            int sub_charenc_mode;
        #define FF_SUB_CHARENC_MODE_DO_NOTHING  -1  ///< do nothing (demuxer outputs a stream supposed to be already in UTF-8, or the codec is bitmap for instance)
        #define FF_SUB_CHARENC_MODE_AUTOMATIC    0  ///< libavcodec will select the mode itself
        #define FF_SUB_CHARENC_MODE_PRE_DECODER  1  ///< the AVPacket data needs to be recoded to UTF-8 before being fed to the decoder, requires iconv
        #define FF_SUB_CHARENC_MODE_IGNORE       2  ///< neither convert the subtitles, nor check them for valid UTF-8
        
            /**
             * Skip processing alpha if supported by codec.
             * Note that if the format uses pre-multiplied alpha (common with VP6,
             * and recommended due to better video quality/compression)
             * the image will look as if alpha-blended onto a black background.
             * However for formats that do not use pre-multiplied alpha
             * there might be serious artefacts (though e.g. libswscale currently
             * assumes pre-multiplied alpha anyway).
             *
             * - decoding: set by user
             * - encoding: unused
             */
            int skip_alpha;
        
            /**
             * Number of samples to skip after a discontinuity
             * - decoding: unused
             * - encoding: set by libavcodec
             */
            int seek_preroll;
        
        #if !FF_API_DEBUG_MV
            /**
             * debug motion vectors
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            int debug_mv;
        #define FF_DEBUG_VIS_MV_P_FOR  0x00000001 //visualize forward predicted MVs of P frames
        #define FF_DEBUG_VIS_MV_B_FOR  0x00000002 //visualize forward predicted MVs of B frames
        #define FF_DEBUG_VIS_MV_B_BACK 0x00000004 //visualize backward predicted MVs of B frames
        #endif
        
            /**
             * custom intra quantization matrix
             * - encoding: Set by user, can be NULL.
             * - decoding: unused.
             */
            uint16_t *chroma_intra_matrix;
        
            /**
             * dump format separator.
             * can be ", " or "\n      " or anything else
             * - encoding: Set by user.
             * - decoding: Set by user.
             */
            uint8_t *dump_separator;
        
            /**
             * ',' separated list of allowed decoders.
             * If NULL then all are allowed
             * - encoding: unused
             * - decoding: set by user
             */
            char *codec_whitelist;
        
            /**
             * Properties of the stream that gets decoded
             * - encoding: unused
             * - decoding: set by libavcodec
             */
            unsigned properties;
        #define FF_CODEC_PROPERTY_LOSSLESS        0x00000001
        #define FF_CODEC_PROPERTY_CLOSED_CAPTIONS 0x00000002
        
            /**
             * Additional data associated with the entire coded stream.
             *
             * - decoding: unused
             * - encoding: may be set by libavcodec after avcodec_open2().
             */
            AVPacketSideData *coded_side_data;
            int            nb_coded_side_data;
        
            /**
             * A reference to the AVHWFramesContext describing the input (for encoding)
             * or output (decoding) frames. The reference is set by the caller and
             * afterwards owned (and freed) by libavcodec - it should never be read by
             * the caller after being set.
             *
             * - decoding: This field should be set by the caller from the get_format()
             *             callback. The previous reference (if any) will always be
             *             unreffed by libavcodec before the get_format() call.
             *
             *             If the default get_buffer2() is used with a hwaccel pixel
             *             format, then this AVHWFramesContext will be used for
             *             allocating the frame buffers.
             *
             * - encoding: For hardware encoders configured to use a hwaccel pixel
             *             format, this field should be set by the caller to a reference
             *             to the AVHWFramesContext describing input frames.
             *             AVHWFramesContext.format must be equal to
             *             AVCodecContext.pix_fmt.
             *
             *             This field should be set before avcodec_open2() is called.
             */
            AVBufferRef *hw_frames_ctx;
        
            /**
             * Control the form of AVSubtitle.rects[N]->ass
             * - decoding: set by user
             * - encoding: unused
             */
            int sub_text_format;
        #define FF_SUB_TEXT_FMT_ASS              0
        #if FF_API_ASS_TIMING
        #define FF_SUB_TEXT_FMT_ASS_WITH_TIMINGS 1
        #endif
        
            /**
             * Audio only. The amount of padding (in samples) appended by the encoder to
             * the end of the audio. I.e. this number of decoded samples must be
             * discarded by the caller from the end of the stream to get the original
             * audio without any trailing padding.
             *
             * - decoding: unused
             * - encoding: unused
             */
            int trailing_padding;
        
            /**
             * The number of pixels per image to maximally accept.
             *
             * - decoding: set by user
             * - encoding: set by user
             */
            int64_t max_pixels;
        
            /**
             * A reference to the AVHWDeviceContext describing the device which will
             * be used by a hardware encoder/decoder.  The reference is set by the
             * caller and afterwards owned (and freed) by libavcodec.
             *
             * This should be used if either the codec device does not require
             * hardware frames or any that are used are to be allocated internally by
             * libavcodec.  If the user wishes to supply any of the frames used as
             * encoder input or decoder output then hw_frames_ctx should be used
             * instead.  When hw_frames_ctx is set in get_format() for a decoder, this
             * field will be ignored while decoding the associated stream segment, but
             * may again be used on a following one after another get_format() call.
             *
             * For both encoders and decoders this field should be set before
             * avcodec_open2() is called and must not be written to thereafter.
             *
             * Note that some decoders may require this field to be set initially in
             * order to support hw_frames_ctx at all - in that case, all frames
             * contexts used must be created on the same device.
             */
            AVBufferRef *hw_device_ctx;
        
            /**
             * Bit set of AV_HWACCEL_FLAG_* flags, which affect hardware accelerated
             * decoding (if active).
             * - encoding: unused
             * - decoding: Set by user (either before avcodec_open2(), or in the
             *             AVCodecContext.get_format callback)
             */
            int hwaccel_flags;
        
            /**
             * Video decoding only. Certain video codecs support cropping, meaning that
             * only a sub-rectangle of the decoded frame is intended for display.  This
             * option controls how cropping is handled by libavcodec.
             *
             * When set to 1 (the default), libavcodec will apply cropping internally.
             * I.e. it will modify the output frame width/height fields and offset the
             * data pointers (only by as much as possible while preserving alignment, or
             * by the full amount if the AV_CODEC_FLAG_UNALIGNED flag is set) so that
             * the frames output by the decoder refer only to the cropped area. The
             * crop_* fields of the output frames will be zero.
             *
             * When set to 0, the width/height fields of the output frames will be set
             * to the coded dimensions and the crop_* fields will describe the cropping
             * rectangle. Applying the cropping is left to the caller.
             *
             * @warning When hardware acceleration with opaque output frames is used,
             * libavcodec is unable to apply cropping from the top/left border.
             *
             * @note when this option is set to zero, the width/height fields of the
             * AVCodecContext and output AVFrames have different meanings. The codec
             * context fields store display dimensions (with the coded dimensions in
             * coded_width/height), while the frame fields store the coded dimensions
             * (with the display dimensions being determined by the crop_* fields).
             */
            int apply_cropping;
        
            /*
             * Video decoding only.  Sets the number of extra hardware frames which
             * the decoder will allocate for use by the caller.  This must be set
             * before avcodec_open2() is called.
             *
             * Some hardware decoders require all frames that they will use for
             * output to be defined in advance before decoding starts.  For such
             * decoders, the hardware frame pool must therefore be of a fixed size.
             * The extra frames set here are on top of any number that the decoder
             * needs internally in order to operate normally (for example, frames
             * used as reference pictures).
             */
            int extra_hw_frames;
        
            /**
             * The percentage of damaged samples to discard a frame.
             *
             * - decoding: set by user
             * - encoding: unused
             */
            int discard_damaged_percentage;
        
            /**
             * The number of samples per frame to maximally accept.
             *
             * - decoding: set by user
             * - encoding: set by user
             */
            int64_t max_samples;
        
            /**
             * Bit set of AV_CODEC_EXPORT_DATA_* flags, which affects the kind of
             * metadata exported in frame, packet, or coded stream side data by
             * decoders and encoders.
             *
             * - decoding: set by user
             * - encoding: set by user
             */
            int export_side_data;
        } AVCodecContext;
        ```

        - codec：编解码器的AVCodec，比如指向ff_aac_latm_decoder；
        - width、height：图像的宽高（只针对视频）；
        - pix_fmt：像素格式（只针对视频）；
        - sample_rate：采样率（只针对音频）；
        - channels：声道数（只针对音频）；
        - sample_fmt：采样格式（只针对音频）；

    - AVCodec

        ```C++
        /**
         * AVCodec.
         */
        typedef struct AVCodec {
            /**
             * Name of the codec implementation.
             * The name is globally unique among encoders and among decoders (but an
             * encoder and a decoder can share the same name).
             * This is the primary way to find a codec from the user perspective.
             */
            const char *name;
            /**
             * Descriptive name for the codec, meant to be more human readable than name.
             * You should use the NULL_IF_CONFIG_SMALL() macro to define it.
             */
            const char *long_name;
            enum AVMediaType type;
            enum AVCodecID id;
            /**
             * Codec capabilities.
             * see AV_CODEC_CAP_*
             */
            int capabilities;
            const AVRational *supported_framerates; ///< array of supported framerates, or NULL if any, array is terminated by {0,0}
            const enum AVPixelFormat *pix_fmts;     ///< array of supported pixel formats, or NULL if unknown, array is terminated by -1
            const int *supported_samplerates;       ///< array of supported audio samplerates, or NULL if unknown, array is terminated by 0
            const enum AVSampleFormat *sample_fmts; ///< array of supported sample formats, or NULL if unknown, array is terminated by -1
            const uint64_t *channel_layouts;         ///< array of support channel layouts, or NULL if unknown. array is terminated by 0
            uint8_t max_lowres;                     ///< maximum value for lowres supported by the decoder
            const AVClass *priv_class;              ///< AVClass for the private context
            const AVProfile *profiles;              ///< array of recognized profiles, or NULL if unknown, array is terminated by {FF_PROFILE_UNKNOWN}
        
            /**
             * Group name of the codec implementation.
             * This is a short symbolic name of the wrapper backing this codec. A
             * wrapper uses some kind of external implementation for the codec, such
             * as an external library, or a codec implementation provided by the OS or
             * the hardware.
             * If this field is NULL, this is a builtin, libavcodec native codec.
             * If non-NULL, this will be the suffix in AVCodec.name in most cases
             * (usually AVCodec.name will be of the form "<codec_name>_<wrapper_name>").
             */
            const char *wrapper_name;
        
            /*****************************************************************
             * No fields below this line are part of the public API. They
             * may not be used outside of libavcodec and can be changed and
             * removed at will.
             * New public fields should be added right above.
             *****************************************************************
             */
            int priv_data_size;
        #if FF_API_NEXT
            struct AVCodec *next;
        #endif
            /**
             * @name Frame-level threading support functions
             * @{
             */
            /**
             * Copy necessary context variables from a previous thread context to the current one.
             * If not defined, the next thread will start automatically; otherwise, the codec
             * must call ff_thread_finish_setup().
             *
             * dst and src will (rarely) point to the same context, in which case memcpy should be skipped.
             */
            int (*update_thread_context)(struct AVCodecContext *dst, const struct AVCodecContext *src);
            /** @} */
        
            /**
             * Private codec-specific defaults.
             */
            const AVCodecDefault *defaults;
        
            /**
             * Initialize codec static data, called from avcodec_register().
             *
             * This is not intended for time consuming operations as it is
             * run for every codec regardless of that codec being used.
             */
            void (*init_static_data)(struct AVCodec *codec);
        
            int (*init)(struct AVCodecContext *);
            int (*encode_sub)(struct AVCodecContext *, uint8_t *buf, int buf_size,
                              const struct AVSubtitle *sub);
            /**
             * Encode data to an AVPacket.
             *
             * @param      avctx          codec context
             * @param      avpkt          output AVPacket (may contain a user-provided buffer)
             * @param[in]  frame          AVFrame containing the raw data to be encoded
             * @param[out] got_packet_ptr encoder sets to 0 or 1 to indicate that a
             *                            non-empty packet was returned in avpkt.
             * @return 0 on success, negative error code on failure
             */
            int (*encode2)(struct AVCodecContext *avctx, struct AVPacket *avpkt,
                           const struct AVFrame *frame, int *got_packet_ptr);
            int (*decode)(struct AVCodecContext *, void *outdata, int *outdata_size, struct AVPacket *avpkt);
            int (*close)(struct AVCodecContext *);
            /**
             * Encode API with decoupled frame/packet dataflow. This function is called
             * to get one output packet. It should call ff_encode_get_frame() to obtain
             * input data.
             */
            int (*receive_packet)(struct AVCodecContext *avctx, struct AVPacket *avpkt);
        
            /**
             * Decode API with decoupled packet/frame dataflow. This function is called
             * to get one output frame. It should call ff_decode_get_packet() to obtain
             * input data.
             */
            int (*receive_frame)(struct AVCodecContext *avctx, struct AVFrame *frame);
            /**
             * Flush buffers.
             * Will be called when seeking
             */
            void (*flush)(struct AVCodecContext *);
            /**
             * Internal codec capabilities.
             * See FF_CODEC_CAP_* in internal.h
             */
            int caps_internal;
        
            /**
             * Decoding only, a comma-separated list of bitstream filters to apply to
             * packets before decoding.
             */
            const char *bsfs;
        
            /**
             * Array of pointers to hardware configurations supported by the codec,
             * or NULL if no hardware supported.  The array is terminated by a NULL
             * pointer.
             *
             * The user can only access this field via avcodec_get_hw_config().
             */
            const struct AVCodecHWConfigInternal **hw_configs;
        
            /**
             * List of supported codec_tags, terminated by FF_CODEC_TAGS_END.
             */
            const uint32_t *codec_tags;
        } AVCodec;
        ```

        - name：编解码器名称
        - type：编解码器类型；
        - id：编解码器ID；
        - 一些编解码的接口函数；

    - AVCodecParser
      
      - ⽤于解析输⼊的数据流并把它分成⼀帧⼀帧的压缩编码数据。⽐较形象
        的说法就是把⻓⻓的⼀段连续的数据“切割”成⼀段段的数据；
      
      ```C++
      typedef struct AVCodecParser {
              int codec_ids[5]; /* several codec IDs are permitted */
              int priv_data_size;
              int (*parser_init)(AVCodecParserContext *s);
              /* This callback never returns an error, a negative value means that
               * the frame start was in a previous packet. */
              int (*parser_parse)(AVCodecParserContext *s,
                                  AVCodecContext *avctx,
                                  const uint8_t **poutbuf, int *poutbuf_size,
                                  const uint8_t *buf, int buf_size);
              void (*parser_close)(AVCodecParserContext *s);
              int (*split)(AVCodecContext *avctx, const uint8_t *buf, int buf_size);
              struct AVCodecParser *next;
          } AVCodecParser;
      ```
    - AVPacket

        ```C++
        /**
         * This structure stores compressed data. It is typically exported by demuxers
         * and then passed as input to decoders, or received as output from encoders and
         * then passed to muxers.
         *
         * For video, it should typically contain one compressed frame. For audio it may
         * contain several compressed frames. Encoders are allowed to output empty
         * packets, with no compressed data, containing only side data
         * (e.g. to update some stream parameters at the end of encoding).
         *
         * AVPacket is one of the few structs in FFmpeg, whose size is a part of public
         * ABI. Thus it may be allocated on stack and no new fields can be added to it
         * without libavcodec and libavformat major bump.
         *
         * The semantics of data ownership depends on the buf field.
         * If it is set, the packet data is dynamically allocated and is
         * valid indefinitely until a call to av_packet_unref() reduces the
         * reference count to 0.
         *
         * If the buf field is not set av_packet_ref() would make a copy instead
         * of increasing the reference count.
         *
         * The side data is always allocated with av_malloc(), copied by
         * av_packet_ref() and freed by av_packet_unref().
         *
         * @see av_packet_ref
         * @see av_packet_unref
         */
        typedef struct AVPacket {
            /**
             * A reference to the reference-counted buffer where the packet data is
             * stored.
             * May be NULL, then the packet data is not reference-counted.
             */
            AVBufferRef *buf;
            /**
             * Presentation timestamp in AVStream->time_base units; the time at which
             * the decompressed packet will be presented to the user.
             * Can be AV_NOPTS_VALUE if it is not stored in the file.
             * pts MUST be larger or equal to dts as presentation cannot happen before
             * decompression, unless one wants to view hex dumps. Some formats misuse
             * the terms dts and pts/cts to mean something different. Such timestamps
             * must be converted to true pts/dts before they are stored in AVPacket.
             */
            int64_t pts;
            /**
             * Decompression timestamp in AVStream->time_base units; the time at which
             * the packet is decompressed.
             * Can be AV_NOPTS_VALUE if it is not stored in the file.
             */
            int64_t dts;
            uint8_t *data;
            int   size;
            int   stream_index;
            /**
             * A combination of AV_PKT_FLAG values
             */
            int   flags;
            /**
             * Additional packet data that can be provided by the container.
             * Packet can contain several types of side information.
             */
            AVPacketSideData *side_data;
            int side_data_elems;
        
            /**
             * Duration of this packet in AVStream->time_base units, 0 if unknown.
             * Equals next_pts - this_pts in presentation order.
             */
            int64_t duration;
        
            int64_t pos;                            ///< byte position in stream, -1 if unknown
        
        #if FF_API_CONVERGENCE_DURATION
            /**
             * @deprecated Same as the duration field, but as int64_t. This was required
             * for Matroska subtitles, whose duration values could overflow when the
             * duration field was still an int.
             */
            attribute_deprecated
            int64_t convergence_duration;
        #endif
        } AVPacket;
        ```

        - pts：显示时间戳；
        - dts：解码时间戳；
        - data：压缩编码数据；
        - size：压缩编码数据大小；
        - pos：数据的偏移地址；
        - stream_index：所属的流索引；

        ![AVPacket常用函数](./images/AVPacket常用函数.png)

    - AVFrame

        ```C++
        /**
         * This structure describes decoded (raw) audio or video data.
         *
         * AVFrame must be allocated using av_frame_alloc(). Note that this only
         * allocates the AVFrame itself, the buffers for the data must be managed
         * through other means (see below).
         * AVFrame must be freed with av_frame_free().
         *
         * AVFrame is typically allocated once and then reused multiple times to hold
         * different data (e.g. a single AVFrame to hold frames received from a
         * decoder). In such a case, av_frame_unref() will free any references held by
         * the frame and reset it to its original clean state before it
         * is reused again.
         *
         * The data described by an AVFrame is usually reference counted through the
         * AVBuffer API. The underlying buffer references are stored in AVFrame.buf /
         * AVFrame.extended_buf. An AVFrame is considered to be reference counted if at
         * least one reference is set, i.e. if AVFrame.buf[0] != NULL. In such a case,
         * every single data plane must be contained in one of the buffers in
         * AVFrame.buf or AVFrame.extended_buf.
         * There may be a single buffer for all the data, or one separate buffer for
         * each plane, or anything in between.
         *
         * sizeof(AVFrame) is not a part of the public ABI, so new fields may be added
         * to the end with a minor bump.
         *
         * Fields can be accessed through AVOptions, the name string used, matches the
         * C structure field name for fields accessible through AVOptions. The AVClass
         * for AVFrame can be obtained from avcodec_get_frame_class()
         */
        typedef struct AVFrame {
        #define AV_NUM_DATA_POINTERS 8
            /**
             * pointer to the picture/channel planes.
             * This might be different from the first allocated byte
             *
             * Some decoders access areas outside 0,0 - width,height, please
             * see avcodec_align_dimensions2(). Some filters and swscale can read
             * up to 16 bytes beyond the planes, if these filters are to be used,
             * then 16 extra bytes must be allocated.
             *
             * NOTE: Except for hwaccel formats, pointers not needed by the format
             * MUST be set to NULL.
             */
            uint8_t *data[AV_NUM_DATA_POINTERS];
        
            /**
             * For video, size in bytes of each picture line.
             * For audio, size in bytes of each plane.
             *
             * For audio, only linesize[0] may be set. For planar audio, each channel
             * plane must be the same size.
             *
             * For video the linesizes should be multiples of the CPUs alignment
             * preference, this is 16 or 32 for modern desktop CPUs.
             * Some code requires such alignment other code can be slower without
             * correct alignment, for yet other it makes no difference.
             *
             * @note The linesize may be larger than the size of usable data -- there
             * may be extra padding present for performance reasons.
             */
            int linesize[AV_NUM_DATA_POINTERS];
        
            /**
             * pointers to the data planes/channels.
             *
             * For video, this should simply point to data[].
             *
             * For planar audio, each channel has a separate data pointer, and
             * linesize[0] contains the size of each channel buffer.
             * For packed audio, there is just one data pointer, and linesize[0]
             * contains the total size of the buffer for all channels.
             *
             * Note: Both data and extended_data should always be set in a valid frame,
             * but for planar audio with more channels that can fit in data,
             * extended_data must be used in order to access all channels.
             */
            uint8_t **extended_data;
        
            /**
             * @name Video dimensions
             * Video frames only. The coded dimensions (in pixels) of the video frame,
             * i.e. the size of the rectangle that contains some well-defined values.
             *
             * @note The part of the frame intended for display/presentation is further
             * restricted by the @ref cropping "Cropping rectangle".
             * @{
             */
            int width, height;
            /**
             * @}
             */
        
            /**
             * number of audio samples (per channel) described by this frame
             */
            int nb_samples;
        
            /**
             * format of the frame, -1 if unknown or unset
             * Values correspond to enum AVPixelFormat for video frames,
             * enum AVSampleFormat for audio)
             */
            int format;
        
            /**
             * 1 -> keyframe, 0-> not
             */
            int key_frame;
        
            /**
             * Picture type of the frame.
             */
            enum AVPictureType pict_type;
        
            /**
             * Sample aspect ratio for the video frame, 0/1 if unknown/unspecified.
             */
            AVRational sample_aspect_ratio;
        
            /**
             * Presentation timestamp in time_base units (time when frame should be shown to user).
             */
            int64_t pts;
        
        #if FF_API_PKT_PTS
            /**
             * PTS copied from the AVPacket that was decoded to produce this frame.
             * @deprecated use the pts field instead
             */
            attribute_deprecated
            int64_t pkt_pts;
        #endif
        
            /**
             * DTS copied from the AVPacket that triggered returning this frame. (if frame threading isn't used)
             * This is also the Presentation time of this AVFrame calculated from
             * only AVPacket.dts values without pts values.
             */
            int64_t pkt_dts;
        
            /**
             * picture number in bitstream order
             */
            int coded_picture_number;
            /**
             * picture number in display order
             */
            int display_picture_number;
        
            /**
             * quality (between 1 (good) and FF_LAMBDA_MAX (bad))
             */
            int quality;
        
            /**
             * for some private data of the user
             */
            void *opaque;
        
        #if FF_API_ERROR_FRAME
            /**
             * @deprecated unused
             */
            attribute_deprecated
            uint64_t error[AV_NUM_DATA_POINTERS];
        #endif
        
            /**
             * When decoding, this signals how much the picture must be delayed.
             * extra_delay = repeat_pict / (2*fps)
             */
            int repeat_pict;
        
            /**
             * The content of the picture is interlaced.
             */
            int interlaced_frame;
        
            /**
             * If the content is interlaced, is top field displayed first.
             */
            int top_field_first;
        
            /**
             * Tell user application that palette has changed from previous frame.
             */
            int palette_has_changed;
        
            /**
             * reordered opaque 64 bits (generally an integer or a double precision float
             * PTS but can be anything).
             * The user sets AVCodecContext.reordered_opaque to represent the input at
             * that time,
             * the decoder reorders values as needed and sets AVFrame.reordered_opaque
             * to exactly one of the values provided by the user through AVCodecContext.reordered_opaque
             */
            int64_t reordered_opaque;
        
            /**
             * Sample rate of the audio data.
             */
            int sample_rate;
        
            /**
             * Channel layout of the audio data.
             */
            uint64_t channel_layout;
        
            /**
             * AVBuffer references backing the data for this frame. If all elements of
             * this array are NULL, then this frame is not reference counted. This array
             * must be filled contiguously -- if buf[i] is non-NULL then buf[j] must
             * also be non-NULL for all j < i.
             *
             * There may be at most one AVBuffer per data plane, so for video this array
             * always contains all the references. For planar audio with more than
             * AV_NUM_DATA_POINTERS channels, there may be more buffers than can fit in
             * this array. Then the extra AVBufferRef pointers are stored in the
             * extended_buf array.
             */
            AVBufferRef *buf[AV_NUM_DATA_POINTERS];
        
            /**
             * For planar audio which requires more than AV_NUM_DATA_POINTERS
             * AVBufferRef pointers, this array will hold all the references which
             * cannot fit into AVFrame.buf.
             *
             * Note that this is different from AVFrame.extended_data, which always
             * contains all the pointers. This array only contains the extra pointers,
             * which cannot fit into AVFrame.buf.
             *
             * This array is always allocated using av_malloc() by whoever constructs
             * the frame. It is freed in av_frame_unref().
             */
            AVBufferRef **extended_buf;
            /**
             * Number of elements in extended_buf.
             */
            int        nb_extended_buf;
        
            AVFrameSideData **side_data;
            int            nb_side_data;
        
        /**
         * @defgroup lavu_frame_flags AV_FRAME_FLAGS
         * @ingroup lavu_frame
         * Flags describing additional frame properties.
         *
         * @{
         */
        
        /**
         * The frame data may be corrupted, e.g. due to decoding errors.
         */
        #define AV_FRAME_FLAG_CORRUPT       (1 << 0)
        /**
         * A flag to mark the frames which need to be decoded, but shouldn't be output.
         */
        #define AV_FRAME_FLAG_DISCARD   (1 << 2)
        /**
         * @}
         */
        
            /**
             * Frame flags, a combination of @ref lavu_frame_flags
             */
            int flags;
        
            /**
             * MPEG vs JPEG YUV range.
             * - encoding: Set by user
             * - decoding: Set by libavcodec
             */
            enum AVColorRange color_range;
        
            enum AVColorPrimaries color_primaries;
        
            enum AVColorTransferCharacteristic color_trc;
        
            /**
             * YUV colorspace type.
             * - encoding: Set by user
             * - decoding: Set by libavcodec
             */
            enum AVColorSpace colorspace;
        
            enum AVChromaLocation chroma_location;
        
            /**
             * frame timestamp estimated using various heuristics, in stream time base
             * - encoding: unused
             * - decoding: set by libavcodec, read by user.
             */
            int64_t best_effort_timestamp;
        
            /**
             * reordered pos from the last AVPacket that has been input into the decoder
             * - encoding: unused
             * - decoding: Read by user.
             */
            int64_t pkt_pos;
        
            /**
             * duration of the corresponding packet, expressed in
             * AVStream->time_base units, 0 if unknown.
             * - encoding: unused
             * - decoding: Read by user.
             */
            int64_t pkt_duration;
        
            /**
             * metadata.
             * - encoding: Set by user.
             * - decoding: Set by libavcodec.
             */
            AVDictionary *metadata;
        
            /**
             * decode error flags of the frame, set to a combination of
             * FF_DECODE_ERROR_xxx flags if the decoder produced a frame, but there
             * were errors during the decoding.
             * - encoding: unused
             * - decoding: set by libavcodec, read by user.
             */
            int decode_error_flags;
        #define FF_DECODE_ERROR_INVALID_BITSTREAM   1
        #define FF_DECODE_ERROR_MISSING_REFERENCE   2
        #define FF_DECODE_ERROR_CONCEALMENT_ACTIVE  4
        #define FF_DECODE_ERROR_DECODE_SLICES       8
        
            /**
             * number of audio channels, only used for audio.
             * - encoding: unused
             * - decoding: Read by user.
             */
            int channels;
        
            /**
             * size of the corresponding packet containing the compressed
             * frame.
             * It is set to a negative value if unknown.
             * - encoding: unused
             * - decoding: set by libavcodec, read by user.
             */
            int pkt_size;
        
        #if FF_API_FRAME_QP
            /**
             * QP table
             */
            attribute_deprecated
            int8_t *qscale_table;
            /**
             * QP store stride
             */
            attribute_deprecated
            int qstride;
        
            attribute_deprecated
            int qscale_type;
        
            attribute_deprecated
            AVBufferRef *qp_table_buf;
        #endif
            /**
             * For hwaccel-format frames, this should be a reference to the
             * AVHWFramesContext describing the frame.
             */
            AVBufferRef *hw_frames_ctx;
        
            /**
             * AVBufferRef for free use by the API user. FFmpeg will never check the
             * contents of the buffer ref. FFmpeg calls av_buffer_unref() on it when
             * the frame is unreferenced. av_frame_copy_props() calls create a new
             * reference with av_buffer_ref() for the target frame's opaque_ref field.
             *
             * This is unrelated to the opaque field, although it serves a similar
             * purpose.
             */
            AVBufferRef *opaque_ref;
        
            /**
             * @anchor cropping
             * @name Cropping
             * Video frames only. The number of pixels to discard from the the
             * top/bottom/left/right border of the frame to obtain the sub-rectangle of
             * the frame intended for presentation.
             * @{
             */
            size_t crop_top;
            size_t crop_bottom;
            size_t crop_left;
            size_t crop_right;
            /**
             * @}
             */
        
            /**
             * AVBufferRef for internal use by a single libav* library.
             * Must not be used to transfer data between libraries.
             * Has to be NULL when ownership of the frame leaves the respective library.
             *
             * Code outside the FFmpeg libs should never check or change the contents of the buffer ref.
             *
             * FFmpeg calls av_buffer_unref() on it when the frame is unreferenced.
             * av_frame_copy_props() calls create a new reference with av_buffer_ref()
             * for the target frame's private_ref field.
             */
            AVBufferRef *private_ref;
        } AVFrame;
        ```

        - data：解码后的音视频数据；
        - linesize：对视频来说是图像中一行像素的大小；对音频来说是整个音频帧的大小；
        - width、height：图像的宽高（只针对视频）；
        - nb_samples：音频每通道采样数（只针对音频）；
        - key_frame：是否为关键帧（只针对视频）；
        - pict_type：帧类型（只针对视频）；
        - pts：显示时间戳；
        - sample_rate：音频采样率（只针对音频）；

        ![AVFrame常用函数](./images/AVFrame常用函数.png)

- 初始化

    - ~~av_register_all()~~：注册所有组件，4.0已经弃用；

      ```C++
      /**
       * Initialize libavformat and register all the muxers, demuxers and
       * protocols. If you do not call this function, then you can select
       * exactly which formats you want to support.
       *
       * @see av_register_input_format()
       * @see av_register_output_format()
       */
      attribute_deprecated
      void av_register_all(void);
      ```

    - avdevice_register_all()对设备进行注册，比如V4L2等；

      ```C++
      /**
       * Initialize libavdevice and register all the input and output devices.
       */
      void avdevice_register_all(void);
      ```

    - avformat_network_init()初始化网络库以及网络加密协议相关的库（比如openssl）；

      ```C++
      /**
       * Do global initialization of network libraries. This is optional,
       * and not recommended anymore.
       *
       * This functions only exists to work around thread-safety issues
       * with older GnuTLS or OpenSSL libraries. If libavformat is linked
       * to newer versions of those libraries, or if you do not use them,
       * calling this function is unnecessary. Otherwise, you need to call
       * this function before any other threads using them are started.
       *
       * This function will be deprecated once support for older GnuTLS and
       * OpenSSL libraries is removed, and this function has no purpose
       * anymore.
       */
      int avformat_network_init(void);
      ```

- 封装

    - avformat_alloc_context()负责申请一个AVFormatContext结构的内存，并进行简单初始化；

      ```C++
      /**
       * Allocate an AVFormatContext.
       * avformat_free_context() can be used to free the context and everything
       * allocated by the framework within it.
       */
      AVFormatContext *avformat_alloc_context(void);
      ```

    - avformat_free_context()释放该结构里的所有东西以及该结构本身；

      ```C++
      /**
       * Free an AVFormatContext and all its streams.
       * @param s context to free
       */
      void avformat_free_context(AVFormatContext *s);
      ```

    - avformat_close_input()关闭解复用器。关闭后就不再需要使用avformat_free_context 进行释放；

      ```C++
      /**
       * Close an opened input AVFormatContext. Free it and all its contents
       * and set *s to NULL.
       */
      void avformat_close_input(AVFormatContext **s);
      ```

    - avformat_open_input()打开输入视频文件；

      ```C++
      /**
       * Open an input stream and read the header. The codecs are not opened.
       * The stream must be closed with avformat_close_input().
       *
       * @param ps Pointer to user-supplied AVFormatContext (allocated by avformat_alloc_context).
       *           May be a pointer to NULL, in which case an AVFormatContext is allocated by this
       *           function and written into ps.
       *           Note that a user-supplied AVFormatContext will be freed on failure.
       * @param url URL of the stream to open.
       * @param fmt If non-NULL, this parameter forces a specific input format.
       *            Otherwise the format is autodetected.
       * @param options  A dictionary filled with AVFormatContext and demuxer-private options.
       *                 On return this parameter will be destroyed and replaced with a dict containing
       *                 options that were not found. May be NULL.
       *
       * @return 0 on success, a negative AVERROR on failure.
       *
       * @note If you want to use custom IO, preallocate the format context and set its pb field.
       */
      int avformat_open_input(AVFormatContext **ps, const char *url, ff_const59 AVInputFormat *fmt, AVDictionary **options);
      ```

    - avformat_find_stream_info()获取音视频文件信息；

      ```C++
      /**
       * Read packets of a media file to get stream information. This
       * is useful for file formats with no headers such as MPEG. This
       * function also computes the real framerate in case of MPEG-2 repeat
       * frame mode.
       * The logical file position is not changed by this function;
       * examined packets may be buffered for later processing.
       *
       * @param ic media file handle
       * @param options  If non-NULL, an ic.nb_streams long array of pointers to
       *                 dictionaries, where i-th member contains options for
       *                 codec corresponding to i-th stream.
       *                 On return each dictionary will be filled with options that were not found.
       * @return >=0 if OK, AVERROR_xxx on error
       *
       * @note this function isn't guaranteed to open all the codecs, so
       *       options being non-empty at return is a perfectly normal behavior.
       *
       * @todo Let the user decide somehow what information is needed so that
       *       we do not waste time getting stuff the user does not need.
       */
      int avformat_find_stream_info(AVFormatContext *ic, AVDictionary **options);
      ```

    - av_read_frame()读取音视频包；

      ```C++
      /**
       * Return the next frame of a stream.
       * This function returns what is stored in the file, and does not validate
       * that what is there are valid frames for the decoder. It will split what is
       * stored in the file into frames and return one for each call. It will not
       * omit invalid data between valid frames so as to give the decoder the maximum
       * information possible for decoding.
       *
       * On success, the returned packet is reference-counted (pkt->buf is set) and
       * valid indefinitely. The packet must be freed with av_packet_unref() when
       * it is no longer needed. For video, the packet contains exactly one frame.
       * For audio, it contains an integer number of frames if each frame has
       * a known fixed size (e.g. PCM or ADPCM data). If the audio frames have
       * a variable size (e.g. MPEG audio), then it contains one frame.
       *
       * pkt->pts, pkt->dts and pkt->duration are always set to correct
       * values in AVStream.time_base units (and guessed if the format cannot
       * provide them). pkt->pts can be AV_NOPTS_VALUE if the video format
       * has B-frames, so it is better to rely on pkt->dts if you do not
       * decompress the payload.
       *
       * @return 0 if OK, < 0 on error or end of file. On error, pkt will be blank
       *         (as if it came from av_packet_alloc()).
       *
       * @note pkt will be initialized, so it may be uninitialized, but it must not
       *       contain data that needs to be freed.
       */
      int av_read_frame(AVFormatContext *s, AVPacket *pkt);
      ```

    - avformat_seek_file()定位文件；

      ```C++
      /**
       * Seek to timestamp ts.
       * Seeking will be done so that the point from which all active streams
       * can be presented successfully will be closest to ts and within min/max_ts.
       * Active streams are all streams that have AVStream.discard < AVDISCARD_ALL.
       *
       * If flags contain AVSEEK_FLAG_BYTE, then all timestamps are in bytes and
       * are the file position (this may not be supported by all demuxers).
       * If flags contain AVSEEK_FLAG_FRAME, then all timestamps are in frames
       * in the stream with stream_index (this may not be supported by all demuxers).
       * Otherwise all timestamps are in units of the stream selected by stream_index
       * or if stream_index is -1, in AV_TIME_BASE units.
       * If flags contain AVSEEK_FLAG_ANY, then non-keyframes are treated as
       * keyframes (this may not be supported by all demuxers).
       * If flags contain AVSEEK_FLAG_BACKWARD, it is ignored.
       *
       * @param s media file handle
       * @param stream_index index of the stream which is used as time base reference
       * @param min_ts smallest acceptable timestamp
       * @param ts target timestamp
       * @param max_ts largest acceptable timestamp
       * @param flags flags
       * @return >=0 on success, error code otherwise
       *
       * @note This is part of the new seek API which is still under construction.
       */
      int avformat_seek_file(AVFormatContext *s, int stream_index, int64_t min_ts, int64_t ts, int64_t max_ts, int flags);
      ```

    - av_seek_frame()定位文件；

      ```C++
      /**
       * Seek to the keyframe at timestamp.
       * 'timestamp' in 'stream_index'.
       *
       * @param s media file handle
       * @param stream_index If stream_index is (-1), a default
       * stream is selected, and timestamp is automatically converted
       * from AV_TIME_BASE units to the stream specific time_base.
       * @param timestamp Timestamp in AVStream.time_base units
       *        or, if no stream is specified, in AV_TIME_BASE units.
       * @param flags flags which select direction and seeking mode
       * @return >= 0 on success
       */
      int av_seek_frame(AVFormatContext *s, int stream_index, int64_t timestamp,
                        int flags);
      ```

- 解码

    - avcodec_alloc_context3()分配解码器上下文；
    
      ```C++
      /**
       * Allocate an AVCodecContext and set its fields to default values. The
       * resulting struct should be freed with avcodec_free_context().
       *
       * @param codec if non-NULL, allocate private data and initialize defaults
       *              for the given codec. It is illegal to then call avcodec_open2()
       *              with a different codec.
       *              If NULL, then the codec-specific defaults won't be initialized,
       *              which may result in suboptimal default settings (this is
       *              important mainly for encoders, e.g. libx264).
       *
       * @return An AVCodecContext filled with default values or NULL on failure.
       */
      AVCodecContext *avcodec_alloc_context3(const AVCodec *codec);
      ```
    
    - avcodec_find_decoder()根据ID查找解码器；
    
      ```C++
      /**
       * Find a registered decoder with a matching codec ID.
       *
       * @param id AVCodecID of the requested decoder
       * @return A decoder if one was found, NULL otherwise.
       */
      AVCodec *avcodec_find_decoder(enum AVCodecID id);
      ```
    
    - avcodec_find_decoder_by_name()根据解码器名字查找解码器；
    
      ```C++
      /**
       * Find a registered decoder with the specified name.
       *
       * @param name name of the requested decoder
       * @return A decoder if one was found, NULL otherwise.
       */
      AVCodec *avcodec_find_decoder_by_name(const char *name);
      ```
    
    - avcodec_open2()打开编解码器；
    
      ```C++
      /**
       * Initialize the AVCodecContext to use the given AVCodec. Prior to using this
       * function the context has to be allocated with avcodec_alloc_context3().
       *
       * The functions avcodec_find_decoder_by_name(), avcodec_find_encoder_by_name(),
       * avcodec_find_decoder() and avcodec_find_encoder() provide an easy way for
       * retrieving a codec.
       *
       * @warning This function is not thread safe!
       *
       * @note Always call this function before using decoding routines (such as
       * @ref avcodec_receive_frame()).
       *
       * @code
       * avcodec_register_all();
       * av_dict_set(&opts, "b", "2.5M", 0);
       * codec = avcodec_find_decoder(AV_CODEC_ID_H264);
       * if (!codec)
       *     exit(1);
       *
       * context = avcodec_alloc_context3(codec);
       *
       * if (avcodec_open2(context, codec, opts) < 0)
       *     exit(1);
       * @endcode
       *
       * @param avctx The context to initialize.
       * @param codec The codec to open this context for. If a non-NULL codec has been
       *              previously passed to avcodec_alloc_context3() or
       *              for this context, then this parameter MUST be either NULL or
       *              equal to the previously passed codec.
       * @param options A dictionary filled with AVCodecContext and codec-private options.
       *                On return this object will be filled with options that were not found.
       *
       * @return zero on success, a negative value on error
       * @see avcodec_alloc_context3(), avcodec_find_decoder(), avcodec_find_encoder(),
       *      av_dict_set(), av_opt_find().
       */
      int avcodec_open2(AVCodecContext *avctx, const AVCodec *codec, AVDictionary **options);
      ```
    
    - avcodec_decode_video2()解码一帧视频数据；
    
      ```C++
      /**
       * Decode the video frame of size avpkt->size from avpkt->data into picture.
       * Some decoders may support multiple frames in a single AVPacket, such
       * decoders would then just decode the first frame.
       *
       * @warning The input buffer must be AV_INPUT_BUFFER_PADDING_SIZE larger than
       * the actual read bytes because some optimized bitstream readers read 32 or 64
       * bits at once and could read over the end.
       *
       * @warning The end of the input buffer buf should be set to 0 to ensure that
       * no overreading happens for damaged MPEG streams.
       *
       * @note Codecs which have the AV_CODEC_CAP_DELAY capability set have a delay
       * between input and output, these need to be fed with avpkt->data=NULL,
       * avpkt->size=0 at the end to return the remaining frames.
       *
       * @note The AVCodecContext MUST have been opened with @ref avcodec_open2()
       * before packets may be fed to the decoder.
       *
       * @param avctx the codec context
       * @param[out] picture The AVFrame in which the decoded video frame will be stored.
       *             Use av_frame_alloc() to get an AVFrame. The codec will
       *             allocate memory for the actual bitmap by calling the
       *             AVCodecContext.get_buffer2() callback.
       *             When AVCodecContext.refcounted_frames is set to 1, the frame is
       *             reference counted and the returned reference belongs to the
       *             caller. The caller must release the frame using av_frame_unref()
       *             when the frame is no longer needed. The caller may safely write
       *             to the frame if av_frame_is_writable() returns 1.
       *             When AVCodecContext.refcounted_frames is set to 0, the returned
       *             reference belongs to the decoder and is valid only until the
       *             next call to this function or until closing or flushing the
       *             decoder. The caller may not write to it.
       *
       * @param[in] avpkt The input AVPacket containing the input buffer.
       *            You can create such packet with av_init_packet() and by then setting
       *            data and size, some decoders might in addition need other fields like
       *            flags&AV_PKT_FLAG_KEY. All decoders are designed to use the least
       *            fields possible.
       * @param[in,out] got_picture_ptr Zero if no frame could be decompressed, otherwise, it is nonzero.
       * @return On error a negative value is returned, otherwise the number of bytes
       * used or zero if no frame could be decompressed.
       *
       * @deprecated Use avcodec_send_packet() and avcodec_receive_frame().
       */
      attribute_deprecated
      int avcodec_decode_video2(AVCodecContext *avctx, AVFrame *picture,
                               int *got_picture_ptr,
                               const AVPacket *avpkt);
      ```
    
    - avcodec_decode_audio4()解码一帧音频数据；
    
      ```C++
      /**
       * Decode the audio frame of size avpkt->size from avpkt->data into frame.
       *
       * Some decoders may support multiple frames in a single AVPacket. Such
       * decoders would then just decode the first frame and the return value would be
       * less than the packet size. In this case, avcodec_decode_audio4 has to be
       * called again with an AVPacket containing the remaining data in order to
       * decode the second frame, etc...  Even if no frames are returned, the packet
       * needs to be fed to the decoder with remaining data until it is completely
       * consumed or an error occurs.
       *
       * Some decoders (those marked with AV_CODEC_CAP_DELAY) have a delay between input
       * and output. This means that for some packets they will not immediately
       * produce decoded output and need to be flushed at the end of decoding to get
       * all the decoded data. Flushing is done by calling this function with packets
       * with avpkt->data set to NULL and avpkt->size set to 0 until it stops
       * returning samples. It is safe to flush even those decoders that are not
       * marked with AV_CODEC_CAP_DELAY, then no samples will be returned.
       *
       * @warning The input buffer, avpkt->data must be AV_INPUT_BUFFER_PADDING_SIZE
       *          larger than the actual read bytes because some optimized bitstream
       *          readers read 32 or 64 bits at once and could read over the end.
       *
       * @note The AVCodecContext MUST have been opened with @ref avcodec_open2()
       * before packets may be fed to the decoder.
       *
       * @param      avctx the codec context
       * @param[out] frame The AVFrame in which to store decoded audio samples.
       *                   The decoder will allocate a buffer for the decoded frame by
       *                   calling the AVCodecContext.get_buffer2() callback.
       *                   When AVCodecContext.refcounted_frames is set to 1, the frame is
       *                   reference counted and the returned reference belongs to the
       *                   caller. The caller must release the frame using av_frame_unref()
       *                   when the frame is no longer needed. The caller may safely write
       *                   to the frame if av_frame_is_writable() returns 1.
       *                   When AVCodecContext.refcounted_frames is set to 0, the returned
       *                   reference belongs to the decoder and is valid only until the
       *                   next call to this function or until closing or flushing the
       *                   decoder. The caller may not write to it.
       * @param[out] got_frame_ptr Zero if no frame could be decoded, otherwise it is
       *                           non-zero. Note that this field being set to zero
       *                           does not mean that an error has occurred. For
       *                           decoders with AV_CODEC_CAP_DELAY set, no given decode
       *                           call is guaranteed to produce a frame.
       * @param[in]  avpkt The input AVPacket containing the input buffer.
       *                   At least avpkt->data and avpkt->size should be set. Some
       *                   decoders might also require additional fields to be set.
       * @return A negative error code is returned if an error occurred during
       *         decoding, otherwise the number of bytes consumed from the input
       *         AVPacket is returned.
       *
      * @deprecated Use avcodec_send_packet() and avcodec_receive_frame().
       */
      attribute_deprecated
      int avcodec_decode_audio4(AVCodecContext *avctx, AVFrame *frame,
                                int *got_frame_ptr, const AVPacket *avpkt);
      ```
    
    - avcodec_send_packet()发送编码数据包，输⼊参数可以为NULL，或者AVPacket的data域设置为NULL或者size域设置为0，表示将刷新所有的包，意味着数据流已经结束了；
    
      ```C++
      /**
       * Supply raw packet data as input to a decoder.
       *
       * Internally, this call will copy relevant AVCodecContext fields, which can
       * influence decoding per-packet, and apply them when the packet is actually
       * decoded. (For example AVCodecContext.skip_frame, which might direct the
       * decoder to drop the frame contained by the packet sent with this function.)
       *
       * @warning The input buffer, avpkt->data must be AV_INPUT_BUFFER_PADDING_SIZE
       *          larger than the actual read bytes because some optimized bitstream
       *          readers read 32 or 64 bits at once and could read over the end.
       *
       * @warning Do not mix this API with the legacy API (like avcodec_decode_video2())
       *          on the same AVCodecContext. It will return unexpected results now
       *          or in future libavcodec versions.
       *
       * @note The AVCodecContext MUST have been opened with @ref avcodec_open2()
       *       before packets may be fed to the decoder.
       *
       * @param avctx codec context
       * @param[in] avpkt The input AVPacket. Usually, this will be a single video
       *                  frame, or several complete audio frames.
       *                  Ownership of the packet remains with the caller, and the
       *                  decoder will not write to the packet. The decoder may create
       *                  a reference to the packet data (or copy it if the packet is
       *                  not reference-counted).
       *                  Unlike with older APIs, the packet is always fully consumed,
       *                  and if it contains multiple frames (e.g. some audio codecs),
       *                  will require you to call avcodec_receive_frame() multiple
       *                  times afterwards before you can send a new packet.
       *                  It can be NULL (or an AVPacket with data set to NULL and
       *                  size set to 0); in this case, it is considered a flush
       *                  packet, which signals the end of the stream. Sending the
       *                  first flush packet will return success. Subsequent ones are
       *                  unnecessary and will return AVERROR_EOF. If the decoder
       *                  still has frames buffered, it will return them after sending
       *                  a flush packet.
       *
       * @return 0 on success, otherwise negative error code:
       *      AVERROR(EAGAIN):   input is not accepted in the current state - user
       *                         must read output with avcodec_receive_frame() (once
       *                         all output is read, the packet should be resent, and
       *                         the call will not fail with EAGAIN).
       *      AVERROR_EOF:       the decoder has been flushed, and no new packets can
       *                         be sent to it (also returned if more than 1 flush
       *                         packet is sent)
       *      AVERROR(EINVAL):   codec not opened, it is an encoder, or requires flush
       *      AVERROR(ENOMEM):   failed to add packet to internal queue, or similar
       *      other errors: legitimate decoding errors
       */
      int avcodec_send_packet(AVCodecContext *avctx, const AVPacket *avpkt);
      ```
    
    - avcodec_receive_frame()接收解码后数据；
    
      ```C++
      /**
       * Return decoded output data from a decoder.
       *
       * @param avctx codec context
       * @param frame This will be set to a reference-counted video or audio
       *              frame (depending on the decoder type) allocated by the
       *              decoder. Note that the function will always call
       *              av_frame_unref(frame) before doing anything else.
       *
       * @return
       *      0:                 success, a frame was returned
       *      AVERROR(EAGAIN):   output is not available in this state - user must try
       *                         to send new input
       *      AVERROR_EOF:       the decoder has been fully flushed, and there will be
       *                         no more output frames
       *      AVERROR(EINVAL):   codec not opened, or it is an encoder
       *      AVERROR_INPUT_CHANGED:   current decoded frame has changed parameters
       *                               with respect to first decoded frame. Applicable
       *                               when flag AV_CODEC_FLAG_DROPCHANGED is set.
       *      other negative values: legitimate decoding errors
       */
      int avcodec_receive_frame(AVCodecContext *avctx, AVFrame *frame);
      ```
    
    - avcodec_free_context()释放解码器上下文，包含了avcodec_close()；
    
      ```C++
      /**
       * Free the codec context and everything associated with it and write NULL to
       * the provided pointer.
       */
      void avcodec_free_context(AVCodecContext **avctx);
      ```
    
    - avcodec_close()关闭解码器；
    
      ```C++
      /**
       * Close a given AVCodecContext and free all the data associated with it
       * (but not the AVCodecContext itself).
       *
       * Calling this function on an AVCodecContext that hasn't been opened will free
       * the codec-specific data allocated in avcodec_alloc_context3() with a non-NULL
       * codec. Subsequent calls will do nothing.
       *
       * @note Do not use this function. Use avcodec_free_context() to destroy a
       * codec context (either open or closed). Opening and closing a codec context
       * multiple times is not supported anymore -- use multiple codec contexts
       * instead.
       */
      int avcodec_close(AVCodecContext *avctx);
      ```
    
    - av_parser_init初始化AVCodecParserContext；
    
      ```C++
      AVCodecParserContext *av_parser_init(int codec_id);
      ```
    
    - av_parser_parse2解析获得⼀个Packet；
    
      ```C++
      /**
       * Parse a packet.
       *
       * @param s             parser context.
       * @param avctx         codec context.
       * @param poutbuf       set to pointer to parsed buffer or NULL if not yet finished.
       * @param poutbuf_size  set to size of parsed buffer or zero if not yet finished.
       * @param buf           input buffer.
       * @param buf_size      buffer size in bytes without the padding. I.e. the full buffer
                              size is assumed to be buf_size + AV_INPUT_BUFFER_PADDING_SIZE.
                              To signal EOF, this should be 0 (so that the last frame
                              can be output).
       * @param pts           input presentation timestamp.
       * @param dts           input decoding timestamp.
       * @param pos           input byte position in stream.
       * @return the number of bytes of the input bitstream used.
       *
       * Example:
       * @code
       *   while(in_len){
       *       len = av_parser_parse2(myparser, AVCodecContext, &data, &size,
       *                                        in_data, in_len,
       *                                        pts, dts, pos);
       *       in_data += len;
       *       in_len  -= len;
       *
       *       if(size)
       *          decode_frame(data, size);
       *   }
       * @endcode
       */
      int av_parser_parse2(AVCodecParserContext *s,
                           AVCodecContext *avctx,
                           uint8_t **poutbuf, int *poutbuf_size,
                           const uint8_t *buf, int buf_size,
                           int64_t pts, int64_t dts,
                           int64_t pos);
      
      ```
    
    - av_get_bytes_per_sample获取每个sample中的字节数；
    
      ```C++
      /**
       * Return number of bytes per sample.
       *
       * @param sample_fmt the sample format
       * @return number of bytes per sample or zero if unknown for the given
       * sample format
       */
      int av_get_bytes_per_sample(enum AVSampleFormat sample_fmt);
      ```
    
    - avcodec_flush_buffers重置codec；
    
      ```C++
      /**
       * Reset the internal codec state / flush internal buffers. Should be called
       * e.g. when seeking or when switching to a different stream.
       *
       * @note for decoders, when refcounted frames are not used
       * (i.e. avctx->refcounted_frames is 0), this invalidates the frames previously
       * returned from the decoder. When refcounted frames are used, the decoder just
       * releases any references it might keep internally, but the caller's reference
       * remains valid.
       *
       * @note for encoders, this function will only do something if the encoder
       * declares support for AV_CODEC_CAP_ENCODER_FLUSH. When called, the encoder
       * will drain any remaining packets, and can then be re-used for a different
       * stream (as opposed to sending a null frame which will leave the encoder
       * in a permanent EOF state after draining). This can be desirable if the
       * cost of tearing down and replacing the encoder instance is high.
       */
      void avcodec_flush_buffers(AVCodecContext *avctx);
      ```
    
- 编码

    - avcodec_find_encoder根据指定的AVCodecID查找注册的编码器；
    
        ```C++
        /**
         * Find a registered encoder with a matching codec ID.
         *
         * @param id AVCodecID of the requested encoder
         * @return An encoder if one was found, NULL otherwise.
         */
        AVCodec *avcodec_find_encoder(enum AVCodecID id);
        ```
    
    - avcodec_find_encoder_by_name根据指定的编码器名称查找注册的编码器；
    
        ```C++
        /**
         * Find a registered encoder with the specified name.
         *
         * @param name name of the requested encoder
         * @return An encoder if one was found, NULL otherwise.
         */
        AVCodec *avcodec_find_encoder_by_name(const char *name);
        ```
    
    - avcodec_alloc_context3为AVCodecContext分配内存；
    
        ```C++
        /**
         * Allocate an AVCodecContext and set its fields to default values. The
         * resulting struct should be freed with avcodec_free_context().
         *
         * @param codec if non-NULL, allocate private data and initialize defaults
         *              for the given codec. It is illegal to then call avcodec_open2()
         *              with a different codec.
         *              If NULL, then the codec-specific defaults won't be initialized,
         *              which may result in suboptimal default settings (this is
         *              important mainly for encoders, e.g. libx264).
         *
         * @return An AVCodecContext filled with default values or NULL on failure.
         */
        AVCodecContext *avcodec_alloc_context3(const AVCodec *codec);
        ```
    
    - avcodec_open2打开编码器；
    
        ```C++
        /**
         * Initialize the AVCodecContext to use the given AVCodec. Prior to using this
         * function the context has to be allocated with avcodec_alloc_context3().
         *
         * The functions avcodec_find_decoder_by_name(), avcodec_find_encoder_by_name(),
         * avcodec_find_decoder() and avcodec_find_encoder() provide an easy way for
         * retrieving a codec.
         *
         * @warning This function is not thread safe!
         *
         * @note Always call this function before using decoding routines (such as
         * @ref avcodec_receive_frame()).
         *
         * @code
         * avcodec_register_all();
         * av_dict_set(&opts, "b", "2.5M", 0);
         * codec = avcodec_find_decoder(AV_CODEC_ID_H264);
         * if (!codec)
         *     exit(1);
         *
         * context = avcodec_alloc_context3(codec);
         *
         * if (avcodec_open2(context, codec, opts) < 0)
         *     exit(1);
         * @endcode
         *
         * @param avctx The context to initialize.
         * @param codec The codec to open this context for. If a non-NULL codec has been
         *              previously passed to avcodec_alloc_context3() or
         *              for this context, then this parameter MUST be either NULL or
         *              equal to the previously passed codec.
         * @param options A dictionary filled with AVCodecContext and codec-private options.
         *                On return this object will be filled with options that were not found.
         *
         * @return zero on success, a negative value on error
         * @see avcodec_alloc_context3(), avcodec_find_decoder(), avcodec_find_encoder(),
         *      av_dict_set(), av_opt_find().
         */
        int avcodec_open2(AVCodecContext *avctx, const AVCodec *codec, AVDictionary **options);
        ```
    
    - avcodec_send_frame将AVFrame⾮压缩数据给编码器；
    
        ```C++
        /**
         * Supply a raw video or audio frame to the encoder. Use avcodec_receive_packet()
         * to retrieve buffered output packets.
         *
         * @param avctx     codec context
         * @param[in] frame AVFrame containing the raw audio or video frame to be encoded.
         *                  Ownership of the frame remains with the caller, and the
         *                  encoder will not write to the frame. The encoder may create
         *                  a reference to the frame data (or copy it if the frame is
         *                  not reference-counted).
         *                  It can be NULL, in which case it is considered a flush
         *                  packet.  This signals the end of the stream. If the encoder
         *                  still has packets buffered, it will return them after this
         *                  call. Once flushing mode has been entered, additional flush
         *                  packets are ignored, and sending frames will return
         *                  AVERROR_EOF.
         *
         *                  For audio:
         *                  If AV_CODEC_CAP_VARIABLE_FRAME_SIZE is set, then each frame
         *                  can have any number of samples.
         *                  If it is not set, frame->nb_samples must be equal to
         *                  avctx->frame_size for all frames except the last.
         *                  The final frame may be smaller than avctx->frame_size.
         * @return 0 on success, otherwise negative error code:
         *      AVERROR(EAGAIN):   input is not accepted in the current state - user
         *                         must read output with avcodec_receive_packet() (once
         *                         all output is read, the packet should be resent, and
         *                         the call will not fail with EAGAIN).
         *      AVERROR_EOF:       the encoder has been flushed, and no new frames can
         *                         be sent to it
         *      AVERROR(EINVAL):   codec not opened, refcounted_frames not set, it is a
         *                         decoder, or requires flush
         *      AVERROR(ENOMEM):   failed to add packet to internal queue, or similar
         *      other errors: legitimate encoding errors
         */
        int avcodec_send_frame(AVCodecContext *avctx, const AVFrame *frame);
        ```
    
    - avcodec_receive_packet获取编码后的AVPacket数据，收到的packet需要⾃⼰释放内存；
    
        ```C++
        /**
         * Read encoded data from the encoder.
         *
         * @param avctx codec context
         * @param avpkt This will be set to a reference-counted packet allocated by the
         *              encoder. Note that the function will always call
         *              av_packet_unref(avpkt) before doing anything else.
         * @return 0 on success, otherwise negative error code:
         *      AVERROR(EAGAIN):   output is not available in the current state - user
         *                         must try to send input
         *      AVERROR_EOF:       the encoder has been fully flushed, and there will be
         *                         no more output packets
         *      AVERROR(EINVAL):   codec not opened, or it is a decoder
         *      other errors: legitimate encoding errors
         */
        int avcodec_receive_packet(AVCodecContext *avctx, AVPacket *avpkt);
        ```
    
    - av_frame_get_buffer为⾳频或视频帧分配新的buffer。在调⽤这个函数之前，必须在AVFame上设
        置好以下属性：
        
        - format（视频为像素格式，⾳频为样本格式）
        - nb_samples（样本个数，针对⾳频）
        - channel_layout（通道类型，针对⾳频）
        - width/height（宽⾼，针对视频）
        
        ```C++
        /**
         * Allocate new buffer(s) for audio or video data.
         *
         * The following fields must be set on frame before calling this function:
         * - format (pixel format for video, sample format for audio)
         * - width and height for video
         * - nb_samples and channel_layout for audio
         *
         * This function will fill AVFrame.data and AVFrame.buf arrays and, if
         * necessary, allocate and fill AVFrame.extended_data and AVFrame.extended_buf.
         * For planar formats, one buffer will be allocated for each plane.
         *
         * @warning: if frame already has been allocated, calling this function will
         *           leak memory. In addition, undefined behavior can occur in certain
         *           cases.
         *
         * @param frame frame in which to store the new buffers.
         * @param align Required buffer size alignment. If equal to 0, alignment will be
         *              chosen automatically for the current CPU. It is highly
         *              recommended to pass 0 here unless you know what you are doing.
         *
         * @return 0 on success, a negative AVERROR on error.
         */
        int av_frame_get_buffer(AVFrame *frame, int align);
        ```
        
    - av_frame_make_writable确保AVFrame是可写的。使⽤av_frame_make_writable()的问题是，在最坏的情况下，它会在您使⽤encode再次更改整个输⼊frame之前复制它。如果frame不可写，
        av_frame_make_writable()将分配新的缓冲区，并复制这个输⼊input frame数据，避免和编码器需要缓存该帧时造成冲突；
        
        ```C++
        /**
         * Ensure that the frame data is writable, avoiding data copy if possible.
         *
         * Do nothing if the frame is writable, allocate new buffers and copy the data
         * if it is not.
         *
         * @return 0 on success, a negative AVERROR on error.
         *
         * @see av_frame_is_writable(), av_buffer_is_writable(),
         * av_buffer_make_writable()
         */
        int av_frame_make_writable(AVFrame *frame);
        ```
        
    - av_samples_fill_arrays填充⾳频帧；
    
        ```C++
        /**
         * Fill plane data pointers and linesize for samples with sample
         * format sample_fmt.
         *
         * The audio_data array is filled with the pointers to the samples data planes:
         * for planar, set the start point of each channel's data within the buffer,
         * for packed, set the start point of the entire buffer only.
         *
         * The value pointed to by linesize is set to the aligned size of each
         * channel's data buffer for planar layout, or to the aligned size of the
         * buffer for all channels for packed layout.
         *
         * The buffer in buf must be big enough to contain all the samples
         * (use av_samples_get_buffer_size() to compute its minimum size),
         * otherwise the audio_data pointers will point to invalid data.
         *
         * @see enum AVSampleFormat
         * The documentation for AVSampleFormat describes the data layout.
         *
         * @param[out] audio_data  array to be filled with the pointer for each channel
         * @param[out] linesize    calculated linesize, may be NULL
         * @param buf              the pointer to a buffer containing the samples
         * @param nb_channels      the number of channels
         * @param nb_samples       the number of samples in a single channel
         * @param sample_fmt       the sample format
         * @param align            buffer size alignment (0 = default, 1 = no alignment)
         * @return                 >=0 on success or a negative error code on failure
         * @todo return minimum size in bytes required for the buffer in case
         * of success at the next bump
         */
        int av_samples_fill_arrays(uint8_t **audio_data, int *linesize,
                                   const uint8_t *buf,
                                   int nb_channels, int nb_samples,
                                   enum AVSampleFormat sample_fmt, int align);
        ```
    
    - av_image_fill_arrays存储⼀帧像素数据到AVFrame对应的data buffer。av_image_fill_arrays()⾃身不具备内存申请的功能，此函数类似于格式化已经申请的内存，即通过av_malloc()函数申请的内存空间，或者av_frame_get_buffer()函数申请的内存空间；
    
        ```C++
        /**
         * Setup the data pointers and linesizes based on the specified image
         * parameters and the provided array.
         *
         * The fields of the given image are filled in by using the src
         * address which points to the image data buffer. Depending on the
         * specified pixel format, one or multiple image data pointers and
         * line sizes will be set.  If a planar format is specified, several
         * pointers will be set pointing to the different picture planes and
         * the line sizes of the different planes will be stored in the
         * lines_sizes array. Call with src == NULL to get the required
         * size for the src buffer.
         *
         * To allocate the buffer and fill in the dst_data and dst_linesize in
         * one call, use av_image_alloc().
         *
         * @param dst_data      data pointers to be filled in
         * @param dst_linesize  linesizes for the image in dst_data to be filled in
         * @param src           buffer which will contain or contains the actual image data, can be NULL
         * @param pix_fmt       the pixel format of the image
         * @param width         the width of the image in pixels
         * @param height        the height of the image in pixels
         * @param align         the value used in src for linesize alignment
         * @return the size in bytes required for src, a negative error code
         * in case of failure
         */
        int av_image_fill_arrays(uint8_t *dst_data[4], int dst_linesize[4],
                                 const uint8_t *src,
                                 enum AVPixelFormat pix_fmt, int width, int height, int align);
        ```
    
    - av_image_get_buffer_size通过指定像素格式、图像宽、图像⾼来计算所需的内存⼤⼩。参数align是设定内存对⻬的对⻬数，也就是按多⼤的字节进⾏内存对⻬；
      
        ```C++
        /**
         * Return the size in bytes of the amount of data required to store an
         * image with the given parameters.
         *
         * @param pix_fmt  the pixel format of the image
         * @param width    the width of the image in pixels
         * @param height   the height of the image in pixels
         * @param align    the assumed linesize alignment
         * @return the buffer size in bytes, a negative error code in case of failure
         */
        int av_image_get_buffer_size(enum AVPixelFormat pix_fmt, int width, int height, int align);
        ```
        
    - av_image_alloc按照指定的宽、⾼、像素格式来分配图像内存；
    
        ```C++
        /**
         * Allocate an image with size w and h and pixel format pix_fmt, and
         * fill pointers and linesizes accordingly.
         * The allocated image buffer has to be freed by using
         * av_freep(&pointers[0]).
         *
         * @param align the value to use for buffer size alignment
         * @return the size in bytes required for the image buffer, a negative
         * error code in case of failure
         */
        int av_image_alloc(uint8_t *pointers[4], int linesizes[4],
                           int w, int h, enum AVPixelFormat pix_fmt, int align);
        ```
    
- 封装

    - avformat_write_header写⽂件头；
    
        ```C++
        /**
         * Allocate the stream private data and write the stream header to
         * an output media file.
         *
         * @param s Media file handle, must be allocated with avformat_alloc_context().
         *          Its oformat field must be set to the desired output format;
         *          Its pb field must be set to an already opened AVIOContext.
         * @param options  An AVDictionary filled with AVFormatContext and muxer-private options.
         *                 On return this parameter will be destroyed and replaced with a dict containing
         *                 options that were not found. May be NULL.
         *
         * @return AVSTREAM_INIT_IN_WRITE_HEADER on success if the codec had not already been fully initialized in avformat_init,
         *         AVSTREAM_INIT_IN_INIT_OUTPUT  on success if the codec had already been fully initialized in avformat_init,
         *         negative AVERROR on failure.
         *
         * @see av_opt_find, av_dict_set, avio_open, av_oformat_next, avformat_init_output.
         */
        av_warn_unused_result
        int avformat_write_header(AVFormatContext *s, AVDictionary **options);
        ```
    
    - av_write_frame/av_interleaved_write_frame写packet；
    
        ```C++
        /**
         * Write a packet to an output media file.
         *
         * This function passes the packet directly to the muxer, without any buffering
         * or reordering. The caller is responsible for correctly interleaving the
         * packets if the format requires it. Callers that want libavformat to handle
         * the interleaving should call av_interleaved_write_frame() instead of this
         * function.
         *
         * @param s media file handle
         * @param pkt The packet containing the data to be written. Note that unlike
         *            av_interleaved_write_frame(), this function does not take
         *            ownership of the packet passed to it (though some muxers may make
         *            an internal reference to the input packet).
         *            <br>
         *            This parameter can be NULL (at any time, not just at the end), in
         *            order to immediately flush data buffered within the muxer, for
         *            muxers that buffer up data internally before writing it to the
         *            output.
         *            <br>
         *            Packet's @ref AVPacket.stream_index "stream_index" field must be
         *            set to the index of the corresponding stream in @ref
         *            AVFormatContext.streams "s->streams".
         *            <br>
         *            The timestamps (@ref AVPacket.pts "pts", @ref AVPacket.dts "dts")
         *            must be set to correct values in the stream's timebase (unless the
         *            output format is flagged with the AVFMT_NOTIMESTAMPS flag, then
         *            they can be set to AV_NOPTS_VALUE).
         *            The dts for subsequent packets passed to this function must be strictly
         *            increasing when compared in their respective timebases (unless the
         *            output format is flagged with the AVFMT_TS_NONSTRICT, then they
         *            merely have to be nondecreasing).  @ref AVPacket.duration
         *            "duration") should also be set if known.
         * @return < 0 on error, = 0 if OK, 1 if flushed and there is no more data to flush
         *
         * @see av_interleaved_write_frame()
         */
        int av_write_frame(AVFormatContext *s, AVPacket *pkt);
        
        /**
         * Write a packet to an output media file ensuring correct interleaving.
         *
         * This function will buffer the packets internally as needed to make sure the
         * packets in the output file are properly interleaved in the order of
         * increasing dts. Callers doing their own interleaving should call
         * av_write_frame() instead of this function.
         *
         * Using this function instead of av_write_frame() can give muxers advance
         * knowledge of future packets, improving e.g. the behaviour of the mp4
         * muxer for VFR content in fragmenting mode.
         *
         * @param s media file handle
         * @param pkt The packet containing the data to be written.
         *            <br>
         *            If the packet is reference-counted, this function will take
         *            ownership of this reference and unreference it later when it sees
         *            fit.
         *            The caller must not access the data through this reference after
         *            this function returns. If the packet is not reference-counted,
         *            libavformat will make a copy.
         *            <br>
         *            This parameter can be NULL (at any time, not just at the end), to
         *            flush the interleaving queues.
         *            <br>
         *            Packet's @ref AVPacket.stream_index "stream_index" field must be
         *            set to the index of the corresponding stream in @ref
         *            AVFormatContext.streams "s->streams".
         *            <br>
         *            The timestamps (@ref AVPacket.pts "pts", @ref AVPacket.dts "dts")
         *            must be set to correct values in the stream's timebase (unless the
         *            output format is flagged with the AVFMT_NOTIMESTAMPS flag, then
         *            they can be set to AV_NOPTS_VALUE).
         *            The dts for subsequent packets in one stream must be strictly
         *            increasing (unless the output format is flagged with the
         *            AVFMT_TS_NONSTRICT, then they merely have to be nondecreasing).
         *            @ref AVPacket.duration "duration") should also be set if known.
         *
         * @return 0 on success, a negative AVERROR on error. Libavformat will always
         *         take care of freeing the packet, even if this function fails.
         *
         * @see av_write_frame(), AVFormatContext.max_interleave_delta
         */
        int av_interleaved_write_frame(AVFormatContext *s, AVPacket *pkt);
        ```
    
    - av_write_trailer写⽂件尾；
    
        ```C++
        /**
         * Write the stream trailer to an output media file and free the
         * file private data.
         *
         * May only be called after a successful call to avformat_write_header.
         *
         * @param s media file handle
         * @return 0 if OK, AVERROR_xxx on error
         */
        int av_write_trailer(AVFormatContext *s);
        ```
    
    - avcodec_parameters_from_context将AVCodecContext结构体中码流参数拷⻉到AVCodecParameters结构体中，和avcodec_parameters_to_context刚好相反；
    
        ```C++
        /**
         * Fill the parameters struct based on the values from the supplied codec
         * context. Any allocated fields in par are freed and replaced with duplicates
         * of the corresponding fields in codec.
         *
         * @return >= 0 on success, a negative AVERROR code on failure
         */
        int avcodec_parameters_from_context(AVCodecParameters *par,
                                            const AVCodecContext *codec);
        
        /**
         * Fill the codec context based on the values from the supplied codec
         * parameters. Any allocated fields in codec that have a corresponding field in
         * par are freed and replaced with duplicates of the corresponding field in par.
         * Fields in codec that do not have a counterpart in par are not touched.
         *
         * @return >= 0 on success, a negative AVERROR code on failure.
         */
        int avcodec_parameters_to_context(AVCodecContext *codec,
                                          const AVCodecParameters *par);
        ```
    
    - avformat_alloc_output_context2分配输出上下文；
    
        ```C++
        /**
         * Allocate an AVFormatContext for an output format.
         * avformat_free_context() can be used to free the context and
         * everything allocated by the framework within it.
         *
         * @param *ctx is set to the created format context, or to NULL in
         * case of failure
         * @param oformat format to use for allocating the context, if NULL
         * format_name and filename are used instead
         * @param format_name the name of output format to use for allocating the
         * context, if NULL filename is used instead
         * @param filename the name of the filename to use for allocating the
         * context, may be NULL
         * @return >= 0 in case of success, a negative AVERROR code in case of
         * failure
         */
        int avformat_alloc_output_context2(AVFormatContext **ctx, ff_const59 AVOutputFormat *oformat,
                                           const char *format_name, const char *filename);
        ```
    
    - avformat_new_stream创建流；
    
        ```C++
        /**
         * Add a new stream to a media file.
         *
         * When demuxing, it is called by the demuxer in read_header(). If the
         * flag AVFMTCTX_NOHEADER is set in s.ctx_flags, then it may also
         * be called in read_packet().
         *
         * When muxing, should be called by the user before avformat_write_header().
         *
         * User is required to call avcodec_close() and avformat_free_context() to
         * clean up the allocation by avformat_new_stream().
         *
         * @param s media file handle
         * @param c If non-NULL, the AVCodecContext corresponding to the new stream
         * will be initialized to use this codec. This is needed for e.g. codec-specific
         * defaults to be set, so codec should be provided if it is known.
         *
         * @return newly created stream or NULL on error.
         */
        AVStream *avformat_new_stream(AVFormatContext *s, const AVCodec *c);
        ```
    
    - av_compare_ts比较时间戳；
    
        ```C++
        /**
         * Compare two timestamps each in its own time base.
         *
         * @return One of the following values:
         *         - -1 if `ts_a` is before `ts_b`
         *         - 1 if `ts_a` is after `ts_b`
         *         - 0 if they represent the same position
         *
         * @warning
         * The result of the function is undefined if one of the timestamps is outside
         * the `int64_t` range when represented in the other's timebase.
         */
        int av_compare_ts(int64_t ts_a, AVRational tb_a, int64_t ts_b, AVRational tb_b);
        ```
    
    - av_q2d()将时间从AVRational形式转换为double形式。AVRational是分数类型，double是双精度浮点数
        类型，转换的结果单位是秒。转换前后的值基于同⼀时间基，仅仅是数值的表现形式不同⽽已；
        
        ```C++
        /**
         * Convert an AVRational to a `double`.
         * @param a AVRational to convert
         * @return `a` in floating-point form
         * @see av_d2q()
         */
        static inline double av_q2d(AVRational a){
            return a.num / (double) a.den;
        }
        ```
        
    - av_rescale_q()⽤于不同时间基的转换，⽤于将时间值从⼀种时间基转换为另⼀种时间基。将a数值由 bq时间基转成 cq的时间基，通过返回结果获取以cq时间基表示的新数值；
      
        ```C++
        int64_t av_rescale_q(int64_t a, AVRational bq, AVRational cq)
        {
            return av_rescale_q_rnd(a, bq, cq, AV_ROUND_NEAR_INF);
        }
        ```
        
    - av_packet_rescale_ts()将AVPacket中各种时间值从⼀种时间基转换为另⼀种时间基；
    
        ```C++
        /**
         * Convert valid timing fields (timestamps / durations) in a packet from one
         * timebase to another. Timestamps with unknown values (AV_NOPTS_VALUE) will be
         * ignored.
         *
         * @param pkt packet on which the conversion will be performed
         * @param tb_src source timebase, in which the timing fields in pkt are
         *               expressed
         * @param tb_dst destination timebase, to which the timing fields will be
         *               converted
         */
        void av_packet_rescale_ts(AVPacket *pkt, AVRational tb_src, AVRational tb_dst);
        ```

### SDL2

![SDL架构](./images/SDL架构.png)

#### 编译源码

```shell
./configure
make
sudo make install
#如果出现Could not initialize SDL - No available video device(Did you set the DISPLAY variable?)错误，说明系统中没有安装x11的库文件，因此编译出来的SDL库实际上不能用。下载安装
sudo apt-get install libx11-dev
sudo apt-get install xorg-dev
```

#### SDL子系统

|       名称       |           值            |
| :--------------: | :---------------------: |
|      定时器      |     SDL_INIT_TIMER      |
|       音频       |     SDL_INIT_AUDIO      |
|       摇杆       |    SDL_INIT_JOYSTICK    |
|       视频       |     SDL_INIT_VIDEO      |
|      触摸屏      |     SDL_INIT_HAPTIC     |
|    游戏控制器    | SDL_INIT_GAMECONTROLLER |
|       事件       |     SDL_INIT_EVENTS     |
| 包含上述所有选项 |   SDL_INIT_EVERYTHING   |

#### SDL视频显示

- **SDL_Init()** 初始化 **SDL** 系统

  <details>
  <summary>SDL_Init</summary>
  
  ```C++
  /**
   *  This function initializes  the subsystems specified by \c flags
   */
  extern DECLSPEC int SDLCALL SDL_Init(Uint32 flags);
  ```
  </details>

- **SDL_CreateWindow()** 创建窗口 **SDL_Window**

  <details>
  <summary>SDL_CreateWindow</summary>

  ```C++
  /**
   *  \brief Create a window with the specified position, dimensions, and flags.
   *
   *  \param title The title of the window, in UTF-8 encoding.
   *  \param x     The x position of the window, ::SDL_WINDOWPOS_CENTERED, or
   *               ::SDL_WINDOWPOS_UNDEFINED.
   *  \param y     The y position of the window, ::SDL_WINDOWPOS_CENTERED, or
   *               ::SDL_WINDOWPOS_UNDEFINED.
   *  \param w     The width of the window, in screen coordinates.
   *  \param h     The height of the window, in screen coordinates.
   *  \param flags The flags for the window, a mask of any of the following:
   *               ::SDL_WINDOW_FULLSCREEN,    ::SDL_WINDOW_OPENGL,
   *               ::SDL_WINDOW_HIDDEN,        ::SDL_WINDOW_BORDERLESS,
   *               ::SDL_WINDOW_RESIZABLE,     ::SDL_WINDOW_MAXIMIZED,
   *               ::SDL_WINDOW_MINIMIZED,     ::SDL_WINDOW_INPUT_GRABBED,
   *               ::SDL_WINDOW_ALLOW_HIGHDPI, ::SDL_WINDOW_VULKAN.
   *
   *  \return The created window, or NULL if window creation failed.
   *
   *  If the window is created with the SDL_WINDOW_ALLOW_HIGHDPI flag, its size
   *  in pixels may differ from its size in screen coordinates on platforms with
   *  high-DPI support (e.g. iOS and Mac OS X). Use SDL_GetWindowSize() to query
   *  the client area's size in screen coordinates, and SDL_GL_GetDrawableSize(),
   *  SDL_Vulkan_GetDrawableSize(), or SDL_GetRendererOutputSize() to query the
   *  drawable size in pixels.
   *
   *  If the window is created with any of the SDL_WINDOW_OPENGL or
   *  SDL_WINDOW_VULKAN flags, then the corresponding LoadLibrary function
   *  (SDL_GL_LoadLibrary or SDL_Vulkan_LoadLibrary) is called and the
   *  corresponding UnloadLibrary function is called by SDL_DestroyWindow().
   *
   *  If SDL_WINDOW_VULKAN is specified and there isn't a working Vulkan driver,
   *  SDL_CreateWindow() will fail because SDL_Vulkan_LoadLibrary() will fail.
   *
   *  \note On non-Apple devices, SDL requires you to either not link to the
   *        Vulkan loader or link to a dynamic library version. This limitation
   *        may be removed in a future version of SDL.
   *
   *  \sa SDL_DestroyWindow()
   *  \sa SDL_GL_LoadLibrary()
   *  \sa SDL_Vulkan_LoadLibrary()
   */
  extern DECLSPEC SDL_Window * SDLCALL SDL_CreateWindow(const char *title,
                                                        int x, int y, int w,
                                                        int h, Uint32 flags);
  ```
  </details>

- **SDL_CreateRenderer()** 创建渲染器 **SDL_Renderer**

  <details>
  <summary>SDL_CreateRenderer</summary>

  ```C++
  /**
   *  \brief Create a 2D rendering context for a window.
   *
   *  \param window The window where rendering is displayed.
   *  \param index    The index of the rendering driver to initialize, or -1 to
   *                  initialize the first one supporting the requested flags.
   *  \param flags    ::SDL_RendererFlags.
   *
   *  \return A valid rendering context or NULL if there was an error.
   *
   *  \sa SDL_CreateSoftwareRenderer()
   *  \sa SDL_GetRendererInfo()
   *  \sa SDL_DestroyRenderer()
   */
  extern DECLSPEC SDL_Renderer * SDLCALL SDL_CreateRenderer(SDL_Window * window,
                                                 int index, Uint32 flags);
  ```
  </details>

- **SDL_CreateTexture()** 创建纹理 **SDL_Texture**

  <details>
  <summary>SDL_CreateTexture</summary>

  ```C++
  /**
   *  \brief Create a texture for a rendering context.
   *
   *  \param renderer The renderer.
   *  \param format The format of the texture.
   *  \param access One of the enumerated values in ::SDL_TextureAccess.
   *  \param w      The width of the texture in pixels.
   *  \param h      The height of the texture in pixels.
   *
   *  \return The created texture is returned, or NULL if no rendering context was
   *          active,  the format was unsupported, or the width or height were out
   *          of range.
   *
   *  \note The contents of the texture are not defined at creation.
   *
   *  \sa SDL_QueryTexture()
   *  \sa SDL_UpdateTexture()
   *  \sa SDL_DestroyTexture()
   */
  extern DECLSPEC SDL_Texture * SDLCALL SDL_CreateTexture(SDL_Renderer * renderer,
                                                          Uint32 format,
                                                          int access, int w,
                                                          int h);
  ```
  </details>

- **SDL_UpdateTexture()** 更新纹理数据

  <details>
  <summary>SDL_UpdateTexture</summary>
  
  ```C++
  /**
   *  \brief Update the given texture rectangle with new pixel data.
   *
   *  \param texture   The texture to update
   *  \param rect      A pointer to the rectangle of pixels to update, or NULL to
   *                   update the entire texture.
   *  \param pixels    The raw pixel data in the format of the texture.
   *  \param pitch     The number of bytes in a row of pixel data, including padding between lines.
   *
   *  The pixel data must be in the format of the texture. The pixel format can be
   *  queried with SDL_QueryTexture.
   *
   *  \return 0 on success, or -1 if the texture is not valid.
   *
   *  \note This is a fairly slow function.
   */
  extern DECLSPEC int SDLCALL SDL_UpdateTexture(SDL_Texture * texture,
                                                const SDL_Rect * rect,
                                                const void *pixels, int pitch);
  ```
  </details>

- **SDL_RenderCopy()** 将纹理的数据拷贝给渲染器

  <details>
  <summary>SDL_RenderCopy</summary>

  ```C++
  /**
   *  \brief Copy a portion of the texture to the current rendering target.
   *
   *  \param renderer The renderer which should copy parts of a texture.
   *  \param texture The source texture.
   *  \param srcrect   A pointer to the source rectangle, or NULL for the entire
   *                   texture.
   *  \param dstrect   A pointer to the destination rectangle, or NULL for the
   *                   entire rendering target.
   *
   *  \return 0 on success, or -1 on error
   */
  extern DECLSPEC int SDLCALL SDL_RenderCopy(SDL_Renderer * renderer,
                                             SDL_Texture * texture,
                                             const SDL_Rect * srcrect,
                                             const SDL_Rect * dstrect);
  ```
  </details>

- **SDL_RenderPresent()** 刷新渲染

  <details>
  <summary>SDL_RenderPresent</summary>

  ```C++
  /**
   *  \brief Update the screen with rendering performed.
   */
  extern DECLSPEC void SDLCALL SDL_RenderPresent(SDL_Renderer * renderer);
  ```
  </details>

- **SDL_Delay()** 工具函数，用于延时

  <details>
  <summary>SDL_Delay</summary>

  ```C++
  /**
   * \brief Wait a specified number of milliseconds before returning.
   */
  extern DECLSPEC void SDLCALL SDL_Delay(Uint32 ms);
  ```
  </details>

- **SDL_Quit()** 退出 **SDL** 系统

  <details>
  <summary>SDL_Quit</summary>

  ```C++
  /**
   *  This function cleans up all initialized subsystems. You should
   *  call it upon all exit conditions.
   */
  extern DECLSPEC void SDLCALL SDL_Quit(void);
  ```
  </details>

#### SDL数据结构

- **SDL_Window** 代表了一个"窗口"

  <details>
  <summary>SDL_Window</summary>
  
  ```C++
  /**
   *  \brief The type used to identify a window
   *
   *  \sa SDL_CreateWindow()
   *  \sa SDL_CreateWindowFrom()
   *  \sa SDL_DestroyWindow()
   *  \sa SDL_GetWindowData()
   *  \sa SDL_GetWindowFlags()
   *  \sa SDL_GetWindowGrab()
   *  \sa SDL_GetWindowPosition()
   *  \sa SDL_GetWindowSize()
   *  \sa SDL_GetWindowTitle()
   *  \sa SDL_HideWindow()
   *  \sa SDL_MaximizeWindow()
   *  \sa SDL_MinimizeWindow()
   *  \sa SDL_RaiseWindow()
   *  \sa SDL_RestoreWindow()
   *  \sa SDL_SetWindowData()
   *  \sa SDL_SetWindowFullscreen()
   *  \sa SDL_SetWindowGrab()
   *  \sa SDL_SetWindowIcon()
   *  \sa SDL_SetWindowPosition()
   *  \sa SDL_SetWindowSize()
   *  \sa SDL_SetWindowBordered()
   *  \sa SDL_SetWindowResizable()
   *  \sa SDL_SetWindowTitle()
   *  \sa SDL_ShowWindow()
   */
  typedef struct SDL_Window SDL_Window;
  ```
  </details>

- **SDL_Renderer** 代表了一个"渲染器"

  <details>
  <summary>SDL_Renderer</summary>

  ```C++
  /**
   *  \brief A structure representing rendering state
   */
  struct SDL_Renderer;
  typedef struct SDL_Renderer SDL_Renderer;
  ```
  </details>

- **SDL_Texture** 代表了一个"纹理"

  <details>
  <summary>SDL_Texture</summary>

  ```C++
  /**
   *  \brief An efficient driver-specific representation of pixel data
   */
  struct SDL_Texture;
  typedef struct SDL_Texture SDL_Texture;
  ```
  </details>

- **SDL_Rect** 矩形结构

  <details>
  <summary>SDL_Rect</summary>

  ```C++
  /**
   *  \brief A rectangle, with the origin at the upper left (integer).
   *
   *  \sa SDL_RectEmpty
   *  \sa SDL_RectEquals
   *  \sa SDL_HasIntersection
   *  \sa SDL_IntersectRect
   *  \sa SDL_UnionRect
   *  \sa SDL_EnclosePoints
   */
  typedef struct SDL_Rect
  {
      int x, y;
      int w, h;
  } SDL_Rect;
  ```
  </details>

#### SDL事件

- **SDL_WaitEvent()** 等待一个事件

  <details>
  <summary>SDL_WaitEvent</summary>
  
  ```C++
  /**
   *  \brief Waits indefinitely for the next available event.
   *
   *  \return 1, or 0 if there was an error while waiting for events.
   *
   *  \param event If not NULL, the next event is removed from the queue and
   *               stored in that area.
   */
  extern DECLSPEC int SDLCALL SDL_WaitEvent(SDL_Event * event);
  ```
  </details>
  
- **SDL_PushEvent()** 发送一个事件

  <details>
  <summary>SDL_PushEvent</summary>
  
  ```C++
  /**
   *  \brief Add an event to the event queue.
   *
   *  \return 1 on success, 0 if the event was filtered, or -1 if the event queue
   *          was full or there was some other error.
   */
  extern DECLSPEC int SDLCALL SDL_PushEvent(SDL_Event * event);
  ```
  </details>
  
- **SDL_PumpEvents()** 将硬件设备产生的事件放入事件队列

  <details>
  <summary>SDL_PumpEvents</summary>
  
  ```C++
  /**
   *  Pumps the event loop, gathering events from the input devices.
   *
   *  This function updates the event queue and internal input device state.
   *
   *  This should only be run in the thread that sets the video mode.
   */
  extern DECLSPEC void SDLCALL SDL_PumpEvents(void);
  ```
  </details>
  
- **SDL_PeepEvents()** 从事件队列提取一个事件

  <details>
  <summary>SDL_PeepEvents</summary>
  
  ```C++
  /**
   *  Checks the event queue for messages and optionally returns them.
   *
   *  If \c action is ::SDL_ADDEVENT, up to \c numevents events will be added to
   *  the back of the event queue.
   *
   *  If \c action is ::SDL_PEEKEVENT, up to \c numevents events at the front
   *  of the event queue, within the specified minimum and maximum type,
   *  will be returned and will not be removed from the queue.
   *
   *  If \c action is ::SDL_GETEVENT, up to \c numevents events at the front
   *  of the event queue, within the specified minimum and maximum type,
   *  will be returned and will be removed from the queue.
   *
   *  \return The number of events actually stored, or -1 if there was an error.
   *
   *  This function is thread-safe.
   */
  extern DECLSPEC int SDLCALL SDL_PeepEvents(SDL_Event * events, int numevents,
                                             SDL_eventaction action,
                                             Uint32 minType, Uint32 maxType);
  ```
  </details>
  
- **SDL_Event** 事件

  <details>
  <summary>SDL_Event</summary>

  ```C++
  /**
   *  \brief General event structure
   */
  typedef union SDL_Event
  {
      Uint32 type;                    /**< Event type, shared with all events */
      SDL_CommonEvent common;         /**< Common event data */
      SDL_DisplayEvent display;       /**< Window event data */
      SDL_WindowEvent window;         /**< Window event data */
      SDL_KeyboardEvent key;          /**< Keyboard event data */
      SDL_TextEditingEvent edit;      /**< Text editing event data */
      SDL_TextInputEvent text;        /**< Text input event data */
      SDL_MouseMotionEvent motion;    /**< Mouse motion event data */
      SDL_MouseButtonEvent button;    /**< Mouse button event data */
      SDL_MouseWheelEvent wheel;      /**< Mouse wheel event data */
      SDL_JoyAxisEvent jaxis;         /**< Joystick axis event data */
      SDL_JoyBallEvent jball;         /**< Joystick ball event data */
      SDL_JoyHatEvent jhat;           /**< Joystick hat event data */
      SDL_JoyButtonEvent jbutton;     /**< Joystick button event data */
      SDL_JoyDeviceEvent jdevice;     /**< Joystick device change event data */
      SDL_ControllerAxisEvent caxis;      /**< Game Controller axis event data */
      SDL_ControllerButtonEvent cbutton;  /**< Game Controller button event data */
      SDL_ControllerDeviceEvent cdevice;  /**< Game Controller device event data */
      SDL_AudioDeviceEvent adevice;   /**< Audio device event data */
      SDL_SensorEvent sensor;         /**< Sensor event data */
      SDL_QuitEvent quit;             /**< Quit request event data */
      SDL_UserEvent user;             /**< Custom event data */
      SDL_SysWMEvent syswm;           /**< System dependent window event data */
      SDL_TouchFingerEvent tfinger;   /**< Touch finger event data */
      SDL_MultiGestureEvent mgesture; /**< Gesture event data */
      SDL_DollarGestureEvent dgesture; /**< Gesture event data */
      SDL_DropEvent drop;             /**< Drag and drop event data */
  
      /* This is necessary for ABI compatibility between Visual C++ and GCC
         Visual C++ will respect the push pack pragma and use 52 bytes for
         this structure, and GCC will use the alignment of the largest datatype
         within the union, which is 8 bytes.
  
         So... we'll add padding to force the size to be 56 bytes for both.
      */
      Uint8 padding[56];
  } SDL_Event;
  ```
  </details>

#### SDL多线程

- **SDL** 线程创建 **SDL_CreateThread**

  <details>
  <summary>SDL_CreateThread</summary>

  ```C++
  /**
   *  Create a thread.
   */
  extern DECLSPEC SDL_Thread *SDLCALL
  SDL_CreateThread(SDL_ThreadFunction fn, const char *name, void *data,
                   pfnSDL_CurrentBeginThread pfnBeginThread,
                   pfnSDL_CurrentEndThread pfnEndThread);
  ```
  </details>

- **SDL** 线程等待 **SDL_WaitThead**

  <details>
  <summary>SDL_WaitThead</summary>

  ```C++
  /**
   *  Wait for a thread to finish. Threads that haven't been detached will
   *  remain (as a "zombie") until this function cleans them up. Not doing so
   *  is a resource leak.
   *
   *  Once a thread has been cleaned up through this function, the SDL_Thread
   *  that references it becomes invalid and should not be referenced again.
   *  As such, only one thread may call SDL_WaitThread() on another.
   *
   *  The return code for the thread function is placed in the area
   *  pointed to by \c status, if \c status is not NULL.
   *
   *  You may not wait on a thread that has been used in a call to
   *  SDL_DetachThread(). Use either that function or this one, but not
   *  both, or behavior is undefined.
   *
   *  It is safe to pass NULL to this function; it is a no-op.
   */
  extern DECLSPEC void SDLCALL SDL_WaitThread(SDL_Thread * thread, int *status);
  ```
  </details>

- **SDL** 创建/销毁互斥锁 **SDL_CreateMutex/SDL_DestroyMutex**

  <details>
  <summary>SDL_CreateMutex/SDL_DestroyMutex</summary>
  
  ```C++
  /**
   *  Create a mutex, initialized unlocked.
   */
  extern DECLSPEC SDL_mutex *SDLCALL SDL_CreateMutex(void);
  
  /**
   *  Destroy a mutex.
   */
  extern DECLSPEC void SDLCALL SDL_DestroyMutex(SDL_mutex * mutex);
  ```
  </details>

- **SDL** 锁互斥 **SDL_LockMutex/SDL_UnlockMutex**

  <details>
  <summary>SDL_LockMutex/SDL_UnlockMutex</summary>
  
  ```C++
  /**
   *  Lock the mutex.
   *
   *  \return 0, or -1 on error.
   */
  #define SDL_mutexP(m)   SDL_LockMutex(m)
  extern DECLSPEC int SDLCALL SDL_LockMutex(SDL_mutex * mutex);
  
  /**
   *  Unlock the mutex.
   *
   *  \return 0, or -1 on error.
   *
   *  \warning It is an error to unlock a mutex that has not been locked by
   *           the current thread, and doing so results in undefined behavior.
   */
  #define SDL_mutexV(m)   SDL_UnlockMutex(m)
  extern DECLSPEC int SDLCALL SDL_UnlockMutex(SDL_mutex * mutex);
  ```
  </details>

- **SDL** 创建/销毁条件变量（信号量） **SDL_CreateCond/SDL_DestoryCond**

  <details>
  <summary>SDL_CreateCond/SDL_DestoryCond</summary>
  
  ```C++
  /**
   *  Create a condition variable.
   *
   *  Typical use of condition variables:
   *
   *  Thread A:
   *    SDL_LockMutex(lock);
   *    while ( ! condition ) {
   *        SDL_CondWait(cond, lock);
   *    }
   *    SDL_UnlockMutex(lock);
   *
   *  Thread B:
   *    SDL_LockMutex(lock);
   *    ...
   *    condition = true;
   *    ...
   *    SDL_CondSignal(cond);
   *    SDL_UnlockMutex(lock);
   *
   *  There is some discussion whether to signal the condition variable
   *  with the mutex locked or not.  There is some potential performance
   *  benefit to unlocking first on some platforms, but there are some
   *  potential race conditions depending on how your code is structured.
   *
   *  In general it's safer to signal the condition variable while the
   *  mutex is locked.
   */
  extern DECLSPEC SDL_cond *SDLCALL SDL_CreateCond(void);
  
  /**
   *  Destroy a condition variable.
   */
  extern DECLSPEC void SDLCALL SDL_DestroyCond(SDL_cond * cond);
  ```
  </details>

- **SDL** 等待/通知条件变量（信号量） **SDL_CondWait/SDL_CondSignal**

  <details>
  <summary>SDL_CondWait/SDL_CondSignal</summary>

  ```C++
  /**
   *  Wait on the condition variable, unlocking the provided mutex.
   *
   *  \warning The mutex must be locked before entering this function!
   *
   *  The mutex is re-locked once the condition variable is signaled.
   *
   *  \return 0 when it is signaled, or -1 on error.
   */
  extern DECLSPEC int SDLCALL SDL_CondWait(SDL_cond * cond, SDL_mutex * mutex);
  
  /**
   *  Restart one of the threads that are waiting on the condition variable.
   *
   *  \return 0 or -1 on error.
   */
  extern DECLSPEC int SDLCALL SDL_CondSignal(SDL_cond * cond);
  ```
  </details>

#### **SDL**播放音频

- 打开音频设备 **SDL_OpenAudio**

  <details>
  <summary>SDL_OpenAudio</summary>

  ```C++
  /**
   *  The calculated values in this structure are calculated by SDL_OpenAudio().
   *
   *  For multi-channel audio, the default SDL channel mapping is:
   *  2:  FL FR                       (stereo)
   *  3:  FL FR LFE                   (2.1 surround)
   *  4:  FL FR BL BR                 (quad)
   *  5:  FL FR FC BL BR              (quad + center)
   *  6:  FL FR FC LFE SL SR          (5.1 surround - last two can also be BL BR)
   *  7:  FL FR FC LFE BC SL SR       (6.1 surround)
   *  8:  FL FR FC LFE BL BR SL SR    (7.1 surround)
   */
  typedef struct SDL_AudioSpec
  {
     int freq;                   /**< DSP frequency -- samples per second */
     SDL_AudioFormat format;     /**< Audio data format */
     Uint8 channels;             /**< Number of channels: 1 mono, 2 stereo */
     Uint8 silence;              /**< Audio buffer silence value (calculated) */
     Uint16 samples;             /**< Audio buffer size in sample FRAMES (total samples divided by channel count) */
     Uint16 padding;             /**< Necessary for some compile environments */
     Uint32 size;                /**< Audio buffer size in bytes (calculated) */
     SDL_AudioCallback callback; /**< Callback that feeds the audio device (NULL to use SDL_QueueAudio()). */
     void *userdata;             /**< Userdata passed to callback (ignored for NULL callbacks). */
   } SDL_AudioSpec;
  
  /**
   *  This function opens the audio device with the desired parameters, and
  
   *  returns 0 if successful, placing the actual hardware parameters in the
   *  structure pointed to by \c obtained.  If \c obtained is NULL, the audio
   *  data passed to the callback function will be guaranteed to be in the
   *  requested format, and will be automatically converted to the hardware
   *  audio format if necessary.  This function returns -1 if it failed
   *  to open the audio device, or couldn't set up the audio thread.
   *
   *  When filling in the desired audio spec structure,
   *  - \c desired->freq should be the desired audio frequency in samples-per-   
   *  second.
   *  - \c desired->format should be the desired audio format.
   *  - \c desired->samples is the desired size of the audio buffer, in
   *  samples.  This number should be a power of two, and may be adjusted by
   *  the audio driver to a value more suitable for the hardware.  Good values
   *  seem to range between 512 and 8096 inclusive, depending on the
   *  application and CPU speed.  Smaller values yield faster response time,
   *  but can lead to underflow if the application is doing heavy processing
   *  and cannot fill the audio buffer in time.  A stereo sample consists of
   *  both right and left channels in LR ordering.
   *  Note that the number of samples is directly related to time by the
   *  following formula:  \code ms = (samples*1000)/freq \endcode
   *  - \c desired->size is the size in bytes of the audio buffer, and is
   *  calculated by SDL_OpenAudio().
   *  - \c desired->silence is the value used to set the buffer to silence,
   *  and is calculated by SDL_OpenAudio().
   *  - \c desired->callback should be set to a function that will be called
   *  when the audio device is ready for more data.  It is passed a pointer
   *  to the audio buffer, and the length in bytes of the audio buffer.
   *  This function usually runs in a separate thread, and so you should
   *  protect data structures that it accesses by calling SDL_LockAudio()
   *  and SDL_UnlockAudio() in your code. Alternately, you may pass a NULL
   *  pointer here, and call SDL_QueueAudio() with some frequency, to queue
   *  more audio samples to be played (or for capture devices, call
   *  SDL_DequeueAudio() with some frequency, to obtain audio samples).
   *  - \c desired->userdata is passed as the first parameter to your callback
   *  function. If you passed a NULL callback, this value is ignored.
   *
   *  The audio device starts out playing silence when it's opened, and should
   *  be enabled for playing by calling \c SDL_PauseAudio(0) when you are ready
   *  for your audio callback function to be called.  Since the audio driver
   *  may modify the requested size of the audio buffer, you should allocate
   *  any local mixing buffers after you open the audio device.
   */
  extern DECLSPEC int SDLCALL SDL_OpenAudio(SDL_AudioSpec * desired,
                                                   SDL_AudioSpec * obtained);
  ```
  </details>

- 数据回调 **SDL_AudioCallback**

  <details>
  <summary>SDL_AudioCallback</summary>
  
  ```C++
  /**
   *  This function is called when the audio device needs more data.
   *
   *  \param userdata An application-specific parameter saved in
   *                  the SDL_AudioSpec structure
   *  \param stream A pointer to the audio data buffer.
   *  \param len    The length of that buffer in bytes.
   *
   *  Once the callback returns, the buffer will no longer be valid.
   *  Stereo samples are stored in a LRLRLR ordering.
   *
   *  You can choose to avoid callbacks and use SDL_QueueAudio() instead, if
   *  you like. Just open your audio device with a NULL callback.
   */
  typedef void (SDLCALL * SDL_AudioCallback) (void *userdata, Uint8 * stream,
                                              int len);
  ```
  </details>

- 播放音频数据 **SDL_PauseAudio**

  <details>
  <summary>SDL_PauseAudio</summary>
  
  ```C++
  /**
   *  \name Pause audio functions
   *
   *  These functions pause and unpause the audio callback processing.
   *  They should be called with a parameter of 0 after opening the audio
   *  device to start playing sound.  This is so you can safely initialize
   *  data for your callback function after opening the audio device.
   *  Silence will be written to the audio device during the pause.
   */
  /* @{ */
  extern DECLSPEC void SDLCALL SDL_PauseAudio(int pause_on);
  ```
  </details>

#### [SDL使用例子](./example/SDL2)

<details>
<summary>SDL使用例子</summary>

```C++
/*
 * @Author: gongluck 
 * @Date: 2021-01-23 14:12:40 
 * @Last Modified by:   gongluck 
 * @Last Modified time: 2021-01-23 14:12:40 
 */

#define SDL_MAIN_HANDLED

#include <iostream>
#include <fstream>
#include "SDL.h"

#define MY_QUIT		SDL_USEREVENT+1
#define MY_REFRESH	SDL_USEREVENT+2

const int WIDTH = 500;
const int HEIGHT = 300;
const int width = 10;
const int height = 10;

bool exitflag = false;

int mythread(void* param)
{
	while (!exitflag)
	{
		SDL_Event event;
		event.type = MY_REFRESH;
		SDL_PushEvent(&event);
		SDL_Delay(100);
	}

	SDL_Event event;
	event.type = MY_QUIT;
	SDL_PushEvent(&event);

	return 0;
}
void SDLCALL mypcm(void* userdata, Uint8* stream, int len)
{
	auto pcm = static_cast<std::iostream*>(userdata);
	auto buf = static_cast<char*>(malloc(len));
	pcm->read(buf, len);
	if (!pcm)
	{
		free(buf);
		return;
	}
	memcpy(stream, buf, len);
	free(buf);
}

//SDL2 ../../../media/gx.yuv 640 480 ../../../media/gx.pcm
int main(int argc, char* argv[])
{
	std::cout << "SDL2 demo" << std::endl;

	std::cout << "Usage : " << "thisfilename YUVfile width height PCMfile" << std::endl;

	if (argc < 5)
	{
		std::cerr << "please see the usage message." << std::endl;
		return -1;
	}
	std::ifstream yuv(argv[1], std::ios::binary);
	if (yuv.fail())
	{
		std::cerr << "can not open file " << argv[1] << std::endl;
		return -1;
	}
	auto yuvwidth = atoi(argv[2]);
	auto yuvheight = atoi(argv[3]);
	std::ifstream pcm(argv[4], std::ios::binary);
	if (pcm.fail())
	{
		std::cerr << "can not open file " << argv[4] << std::endl;
		return -1;
	}

	auto ret = SDL_Init(SDL_INIT_VIDEO | SDL_INIT_AUDIO);
	SDL_Window* window = SDL_CreateWindow("SDL2", SDL_WINDOWPOS_UNDEFINED, SDL_WINDOWPOS_UNDEFINED, WIDTH, HEIGHT, SDL_WINDOW_OPENGL | SDL_WINDOW_RESIZABLE);
	auto renderer = SDL_CreateRenderer(window, -1, 0);
	auto texture = SDL_CreateTexture(renderer, SDL_PIXELFORMAT_RGBA8888, SDL_TEXTUREACCESS_TARGET, WIDTH, HEIGHT);

	int i = 0;
	while (i++ < 20)
	{
		ret = SDL_SetRenderTarget(renderer, texture);
		ret = SDL_SetRenderDrawColor(renderer, 0, 0, 0, 255);
		ret = SDL_RenderClear(renderer);
		SDL_Rect rect = { rand() % (WIDTH - width), rand() % (HEIGHT - height), width, height };
		ret = SDL_RenderDrawRect(renderer, &rect);
		ret = SDL_SetRenderDrawColor(renderer, 255, 0, 0, 255);
		ret = SDL_RenderFillRect(renderer, &rect);

		ret = SDL_SetRenderTarget(renderer, nullptr);
		ret = SDL_RenderCopy(renderer, texture, nullptr, nullptr);

		SDL_RenderPresent(renderer);
		SDL_Delay(100);
	}

	auto yuvtexture = SDL_CreateTexture(renderer, SDL_PIXELFORMAT_IYUV, SDL_TEXTUREACCESS_STREAMING, yuvwidth, yuvheight);
	auto datasize = yuvwidth * yuvheight * 3 / 2;
	auto yuvdata = static_cast<char*>(malloc(datasize));
	auto th = SDL_CreateThread(mythread, nullptr, nullptr);

	SDL_AudioSpec spec = { 0 };
	spec.freq = 44100;
	spec.format = AUDIO_S16SYS;
	spec.channels = 2;
	spec.silence = 0;
	spec.samples = 1024;
	spec.callback = mypcm;
	spec.userdata = &pcm;
	ret = SDL_OpenAudio(&spec, nullptr);
	SDL_PauseAudio(0);

	SDL_Event event = { 0 };
	while (!exitflag)
	{
		ret = SDL_WaitEvent(&event);
		switch (event.type)
		{
		case SDL_KEYDOWN:
			if (event.key.keysym.sym >= SDLK_a && event.key.keysym.sym <= SDLK_z)
			{
				std::cout << char('a' + event.key.keysym.sym - SDLK_a) << " down" << std::endl;
			}
			else if (event.key.keysym.sym == SDLK_ESCAPE)
			{
				SDL_Event event_q;
				event_q.type = MY_QUIT;
				ret = SDL_PushEvent(&event_q);
				break;
			}
			break;
		case SDL_MOUSEBUTTONDOWN:
			if (event.button.button == SDL_BUTTON_LEFT)
			{
				std::cout << "mouse left down" << std::endl;
			}
			else if (event.button.button == SDL_BUTTON_RIGHT)
			{
				std::cout << "mouse right down" << std::endl;
			}
			else
			{
				std::cout << "mouse down" << std::endl;
			}
			break;
		case SDL_MOUSEMOTION:
			std::cout << "mouse move " << event.button.x << ", " << event.button.y << std::endl;
			break;
		case MY_REFRESH:
		{
			yuv.read(yuvdata, datasize);
			if (!yuv)
			{
				exitflag = true;
				break;
			}

			SDL_UpdateTexture(yuvtexture, nullptr, yuvdata, yuvwidth);
			SDL_RenderClear(renderer);
			//SDL_RenderCopy(renderer, yuvtexture, nullptr, nullptr);

			//旋转90°并且铺满渲染区域
			SDL_Point center = { yuvwidth, 0 };//src坐标系
			SDL_Rect  dstrect;//旋转后src坐标系
			dstrect.x = -yuvwidth;
			dstrect.y = 0;
			dstrect.w = yuvwidth;
			dstrect.h = yuvheight;
			SDL_RenderSetScale(renderer, (float)HEIGHT / dstrect.w, (float)WIDTH / dstrect.h);//按比例缩放
			SDL_RenderCopyEx(renderer, yuvtexture, nullptr, &dstrect, -90, &center, SDL_FLIP_NONE);

			SDL_RenderPresent(renderer);
		}
		break;
		case MY_QUIT:
			std::cout << "my quit envent." << std::endl;
			exitflag = true;
			break;
		}
	}

	SDL_WaitThread(th, nullptr);
	SDL_DestroyTexture(yuvtexture);
	SDL_DestroyTexture(texture);
	SDL_DestroyWindow(window);
	SDL_DestroyRenderer(renderer);

	SDL_PauseAudio(1);
	SDL_CloseAudio();

	SDL_Quit();

	yuv.close();
	free(yuvdata);
	pcm.close();

	return 0;
}
```
</details>

### SRS

- 编译SRS

  ```shell
  # SRS源码获取
  git clone https://gitee.com/winlinvip/srs.oschina.git srs
  cd srs/trunk
  git remote set-url origin https://github.com/ossrs/srs.git
  git pull
  git checkout 3.0release
  # 编译
  ./configure
  make -j 4
  # 运行
  ./objs/srs -c conf/srs.conf
  #./objs/srs -c conf/rtmp.conf
  #./objs/srs -c conf/hls.conf
  # 查看服务
  ps -ef | grep srs
  # 退出服务
  kill -SIGQUIT srspid
  ```

### ZLMediaKit

- 编译ZLMediaKit

  ```shell
  # 下载源码
  git clone --depth 1 https://gitee.com/xia-chu/ZLMediaKit
  cd ZLMediaKit
  # 千万不要忘记执行这句命令
  git submodule update --init
  # 安装依赖库
  sudo apt-get install libssl-dev -y
  sudo apt-get install libsdl-dev -y
  sudo apt-get install libavcodec-dev -y
  sudo apt-get install libavutil-dev -y
  sudo apt-get install ffmpeg -y
  # 编译
  mkdir build
  cd build
  cmake ..
  make -j 8
  # 运行
  cd ../release/linux/Debug
  #通过-h可以了解启动参数
  ./MediaServer -h
  #以守护进程模式启动
  sudo ./MediaServer -d &
  ```


### coturn

- 编译coturn

  ```shell
  # 安装依赖
  sudo apt-get install libssl-dev
  sudo apt-get install libevent-dev
  # 下载源码
  git clone https://github.com/coturn/coturn
  cd coturn
  # 编译安装
  ./configure prefix=/mnt/e/ubuntu/coturn/bin/
  make -j 8
  sudo make install
  # 启动
  turnserver --min-port 40000 --max-port 60000 -L 0.0.0.0 -a -u gongluck:123456 -v -f -r nort.gov
  # 浏览器测试
  https://webrtc.github.io/samples/src/content/peerconnection/trickle-ice/
  ```

### Nginx

- 编译nginx

    ```shell
    # 安装依赖
    sudo apt-get update
    sudo apt-get install build-essential libtool -y
    sudo apt-get install libpcre3 libpcre3-dev -y
    sudo apt-get install zlib1g-dev -y
    sudo apt-get install openssl -y
    #下载nginx
    wget http://nginx.org/download/nginx-1.19.0.tar.gz
    tar zxvf nginx-1.19.0.tar.gz
    cd nginx-1.19.0/
    # 配置，支持https
    ./configure --with-http_ssl_module
    # 编译
    make -j 8
    # 安装
    sudo make install
    # 启动
    sudo /usr/local/nginx/sbin/nginx
    # 停止
    sudo /usr/local/nginx/sbin/nginx -s stop
    # 重新加载配置文件
    sudo /usr/local/nginx/sbin/nginx -s reload
    ```
    
- 生成证书

    ```shell
    sudo mkdir -p /root/cert
    sudo cd /root/cert
    # CA私钥
    sudo openssl genrsa -out key.pem 2048
    # 自签名证书
    sudo openssl req -new -x509 -key key.pem -out cert.pem -days 1095
    ```

- 配置Web服务

    - 创建webrtc-­https.conf文件：

        ```shell
        server{
        	listen 443 ssl;
        	ssl_certificate /root/cert/cert.pem;
        	ssl_certificate_key /root/cert/key.pem;
        	charset utf‐8;
        	# ip地址或者域名
        	server_name www.gongluck.icu;
        	location / {
        		add_header 'Access-Control-Allow-Origin' '*';
        		add_header 'Access-Control-Allow-Credentials' 'true';
        		add_header 'Access-Control-Allow-Methods' '*';
        		add_header 'Access-Control-Allow-Headers' 'Origin, X-Requested-With, Content-Type,Accept';
        	# web页面所在目录
        	root /code/AnalysisAVP/example/WebRTC/demo/client/;
        	index index.php index.html index.htm;
        	}
        }
        ```

    - 创建webrtc-websocket-proxy.conf文件：

        ```shell
        map $http_upgrade $connection_upgrade {
        	default upgrade;
    	'' close;
        	}
    	upstream websocket {
        		server www.gongluck.icu:8099;
        	}
        	server {
        		listen 8098 ssl;
        		#ssl on;
        		ssl_certificate /root/cert/cert.pem;
        		ssl_certificate_key /root/cert/key.pem;
        		server_name www.gongluck.icu;
        	location /ws {
        		proxy_pass http://websocket;
        		proxy_http_version 1.1;
        		proxy_connect_timeout 4s; #配置点1
        		proxy_read_timeout 6000s; #配置点2，如果没效，可以考虑这个时间配置长一点
        		proxy_send_timeout 6000s; #配置点3
        		proxy_set_header Upgrade $http_upgrade;
        		proxy_set_header Connection $connection_upgrade;
        	}
        }
        ```
    
    - 编辑nginx.conf文件，在末尾}之前添加包含文件：
    
        ```shell
        include /code/AnalysisAVP/example/WebRTC/demo/client/webrtc-https.conf;
        include /code/AnalysisAVP/example/WebRTC/demo/client/webrtc-websocket-proxy.conf;
        ```
    

### nodejs

- 安装nodejs

  ```shell
  # 下载
  wget https://nodejs.org/dist/v15.0.0/node-v15.0.0-linux-x64.tar.xz
  # 解压
  tar -xvf node-v15.0.0-linux-x64.tar.xz
  # 进入目录
  cd node-v15.0.0-linux-x64
  # 执行软连接
  sudo ln ‐s /mnt/e/ubuntu/node-v15.0.0-linux-x64/bin/npm /usr/local/bin/
  sudo ln ‐s /mnt/e/ubuntu/node-v15.0.0-linux-x64/bin/node /usr/local/bin/
  ```

## 流媒体协议

### RTMP

-  RTMP简介

    - RTMP(Real Time Messaging Protocol)是一个应用层协议，主要用于在Flash player和服务器之间传输视频、音频、控制命令等内容。该协议的突出优点是低延时。RTMP基于TCP，默认使用端口1935。

- RTMP播放基本流程

    ![RTMP播放基本流程](./images/RTMP播放基本流程.png)

    - RTMP是基于TCP的应用层协议。通过TCP三次握手，可实现RTMP客户端与RTMP服务器的指定端口（默认端口为1935）建立一个可靠的网络连接。这里的网络连接才是真正的物理连接。完成了三次握手，客户端和服务器端就可以开始传送数据。

    ![RTMP-HandShake](./images/RTMP-HandShake.png)

    - RTMP握手主要分为：简单握手和复杂握手。
    - RTMP连接时不同的Application Instance可根据功能等进行区分，比如直播可以用live来表示，点播回放可以用vod来表示。
    - RTMP创建流用于创建逻辑通道，该通道用于传输视频、音频、metadata。在服务器的响应报文中会返回 ，用于唯一的标示该Stream。
    - RTMP播放用来播放指定流。开始传输音视频数据。如果发送play命令后想要立即播放，需要清空play队列中的其它流，并将reset置为true。
    - RTMP删除指定Stream ID的流。服务器不用对这条命令发送响应报文。

- RTMP消息结构

    ![RTMP消息结构](./images/RTMP消息结构.png)

    ![RTMP消息](./images/RTMP消息.png)

    - 消息主要分为三类：协议控制消息、数据消息、命令消息等。
    - 协议控制消息

        - Message Type ID = 1、2、3、5、6和Message Type ID = 4两大类，主要用于协议内的控制；

    - 数据消息

        - Message Type ID = 8、9、18；
        - 8：Audio 音频数据；
        - 9：Video 视频数据；
        - 18：Metadata 包括音视频编码、视频宽高等信息；

    - 命令消息 

        - Command Message（20、17）；
        - 此类型消息主要有NetConnection和NetStream两个类，两个类分别有多个函数，该消息的调用，可理解为远程函数调用；

    - Message StreamID是音视频流的唯一ID，一路流如果既有音频包又有视频包，那么这路流音频包的StreamID和他视频包的StreamID相同；

    ![RTMP的chunk结构](./images/RTMP的chunk结构.png)

    - RTMP流中视频和音频拥有单独的Chunk Stream ID。比如音频的cs id=20，视频的cs id=21。接收端接收到Chunk之后，根据cs id分别将音频和视频“拼成消息”；
    - RTMP协议最多⽀持65597个⽤户⾃定义chunk stream ID，范围为[3，65599] ，ID 0、1、2被协议规范直接使⽤，其中ID值为0、1分表表示了Basic Header占⽤2个字节和3个字节：
        - ID值0：代表Basic Header占⽤2个字节，CSID在[64，319]之间；
        - ID值1：代表Basic Header占⽤3个字节，CSID在[64，65599]之间；
        - ID值2：代表该chunk是控制信息和⼀些命令信息；
    - 当Basic Header为1个字节时，CSID占6位，6位最多可以表示64个数，因此这种情况下CSID在[0，63]之间，其中⽤户可⾃定义的范围为[3，63]；
    - 当Basic Header为2个字节时，CSID占只占8位，第⼀个字节除chunk type占⽤的bit都置为0，第⼆个字节⽤来表示CSID-64，8位可以表示 [0, 255] 共256个数，ID的计算⽅法为（第⼆个字节+64），范围为[64，319]；
    - 当Basic Header为3个字节时，以在此字段⽤3字节版本编码。ID的计算⽅法为（第三字节*256+第⼆字节+64）（Basic Header是采⽤⼩端存储的⽅式），范围为[64，65599]；

    ![RTMP的chunk](./images/RTMP的chunk.png)

    - Message被切割成一个或多个Chunk，然后在网络上进行发送。当发送时， 一个chunk发送完毕后才可以发送下一个chunk；
    - 拆分的时候， 默认的Chunk Size是128字节；

    ![RTMP的chunktype](./images/RTMP的chunktype.png)

    - RTMP Chunk Header的长度不是固定的， 分为:12 Bytes、8 Bytes、4 Bytes、1 Byte四种，由RTMP Chunk Header前2位决定；

    ![RTMP的chunk头1](./images/RTMP的chunk头1.png)
    ![RTMP的chunk头2](./images/RTMP的chunk头2.png)

    - 一般情况下，msg stream id是不会变的，所以针对视频或音频，除了第一个RTMP Chunk Header是12Bytes的，后续即可采用8Bytes的；
    - 如果消息的长度（message length）和类型（msg type id，如视频为9或音频为8）又相同，即可将这两部分也省去，RTMP Chunk Header采用4Bytes类型的；
    - 如果当前Chunk与之前的Chunk相比，msg stream id相同，msg typeid相同，message length相同，而且都属于同一个消息（由同一个Message切割成），这类Chunk的时间戳也是相同的，故后续的也可以省去，RTMP Chunk Header采用1Byte类型的；
    - 当Chunk Size足够大时（一般不这么干），此时所有的Message都只能相应切割成一个Chunk，该Chunk仅msg stream id相同。此时基本上除了第一个Chunk的Header是12Bytes外，其它所有Chunk的Header都是8Bytes；
    - 一般只有rtmp流刚开始的metadata、绝对时间戳的视频或音频是12Bytes；
    - 有些控制消息也是12 Bytes，比如connect；

- RTMP传输流程

    ![RTMP传输基本流程](./images/RTMP传输基本流程.png)

    - 不同类型的消息会被分配不同的优先级，当网络传输能力受限时，优先级用来控制消息在网络底层的排队顺序。比如当客户端网络不佳时，流媒体服务器可能会选择丢弃视频消息，以保证音频消息可及时送达客户端；

    ![RTMP消息优先级](./images/RTMP消息优先级.png)

    - RTMP Chunk Stream层级允许在Message stream层次，将大消息切割成小消息，这样可以避免大的低优先级的消息（如视频消息）阻塞小的高优先级的消息（如音频消息或控制消息）；

    - RTMP中时间戳的单位为毫秒。时间戳为相对于某个时间点的相对值。时间戳的长度为32bit，不考虑回滚的话，最大可表示49天17小时2分钟47.296秒。Timestamp delta单位也是毫秒，为相对于前一个时间戳的一个无符号整数；可能为24bit或32bit。RTMP Message的时间戳4个字节，大端存储；
    - 用wireshark转包分析发现，rtmp流的chunk视频流（或音频流）除第一个视频时间戳为绝对时间戳外，后续的时间戳均为timestamp delta，即当前时间戳与上一个时间戳的差值；
    - 通常情况下，Chunk的时间戳（包括绝对时间戳Timestamp delta）是3个字节。但时间戳值超过0xFFFFFF时，启用Extended Timestamp（4个字节）来表示时间戳；
    - timestamp delta的值超过16777215（即16进制的0xFFFFFF）时，这时候这三个字节必须被置为0xFFFFFF，以此来标示Extended Timestamp（4字节）将会存在，由Extended Timestamp来表示时间戳；

### HLS

- HLS简介

    - 作为Apple提出的⼀种基于HTTP的协议，HLS（HTTP Live Streaming）⽤于解决实时⾳视频流的传输。尤其是在移动端，由于iOS/H5不⽀持flash，使得HLS成了移动端实时视频流传输的⾸选。HLS经常⽤在直播领域，⼀些国内的直播云通常⽤HLS拉流（将视频流从服务器拉到客户端）。 HLS值得诟病之处就是其延迟严重，延迟通常在10-30s之间；

        ![HLS框架](./images/HLS框架.png)

- HLS协议

    ![HLS使用流程](./images/HLS使用流程.png)

    - 相对于常⻅的流媒体直播协议，例如RTMP协议、RTSP协议、MMS协议等，HLS直播最⼤的不同在于，直播客户端获取到的，并不是⼀个完整的数据流。HLS协议在服务器端将直播数据流存储为连续的、很短时⻓的媒体⽂件（MPEG-TS格式），⽽客户端则不断的下载并播放这些⼩⽂件，因为服务器端总是会将最新的直播数据⽣成新的⼩⽂件，这样客户端只要不停的按顺序播放从服务器获取到的⽂件，就实现了直播。由此可⻅，基本上可以认为，HLS是以点播的技术⽅式来实现直播。由于数据通过HTTP协议传输，所以完全不⽤考虑防⽕墙或者代理的问题，⽽且分段⽂件的时⻓很短，客户端可以很快的选择和切换码率，以适应不同带宽条件下的播放。不过HLS的这种技术特点，决定了它的延迟⼀般总是会⾼于普通的流媒体直播协议；

    - HLS协议由三部分组成：HTTP、M3U8、TS。这三部分中，HTTP 是传输协议，M3U8 是索引⽂件，TS是⾳视频的媒体信息；

    - HLS的m3u8，是⼀个ts的列表，也就是告诉浏览器可以播放这些ts⽂件。有⼏个关键的参数，这些参数在 SRS 的配置⽂件中都有配置项：

        |        配置项         |                             作用                             |
        | :-------------------: | :----------------------------------------------------------: |
        |        #EXTM3U        |          每个M3U⽂件第⼀⾏必须是这个tag，起标示作⽤          |
        |    #EXT-X-VERSION     |        该属性可以没有，⽬前主要是version 3，最新的是7        |
        | #EXT-X-MEDIA-SEQUENCE | 每⼀个media URI在PlayList中只有唯⼀的序号，相邻之间序号+1，⼀个media URI并不是必须要包含的，如果没有，默认为0 |
        | #EXT-X-TARGETDURATION | 所有切⽚的最⼤时⻓。有些Apple设备这个参数不正确会⽆法播放。SRS会⾃动计算出 ts⽂件的最⼤时⻓，然后更新m3u8时会⾃动更新这个值。⽤户不必⾃⼰配置 |
        |        #EXTINF        | ts切⽚的实际时⻓，SRS提供配置项hls_fragment，但实际上的ts时⻓还受gop影响 |
        |     ts⽂件的数⽬      | SRS可配置hls_window（单位是秒，不是数量），指定m3u8中保存多少个切⽚。譬如，每个ts切⽚为10秒，窗⼝为60秒，那么m3u8中最多保存6个ts切⽚，SRS会⾃动清理旧的切⽚ |
        |    livestream-0.ts    | SRS会⾃动维护ts切⽚的⽂件名，在编码器重推之后，这个编号会继续增⻓，保证流的连续性。直到SRS重启，这个编号才重置为0 |

    - 每⼀个m3u8⽂件，分别对应若⼲个ts⽂件，这些ts⽂件才是真正存放视频的数据。m3u8⽂件只是存放了⼀些ts⽂件的配置信息和相关路径，当视频播放时，m3u8是动态改变的，video标签会解析这个⽂件，并找到对应的ts⽂件来播放，所以⼀般为了加快速度，m3u8放在web服务器上，ts⽂件放在cdn上；

- HLS协议优势

    - HLS相对于RTMP来讲使⽤了标准的HTTP协议来传输数据，可以避免在⼀些特殊的⽹络环境下被屏蔽；
    - HLS相⽐RTMP在服务器端做负载均衡要简单得多。因为HLS是基于⽆状态协议 HTTP实现的，客户端只需要按照顺序使⽤下载存储在服务器的普通ts⽂件进⾏播放就可以。⽽RTMP是⼀种有状态协议，很难对视频服务器进⾏平滑扩展，因为需要为每⼀个播放视频流的客户端维护状态；
    - HLS协议本身实现了码率⾃适应，在不同带宽情况下，设备可以⾃动切换到最适合⾃⼰码率的视频播放；

- HLS协议劣势

    - HLS协议在直播的视频延迟时间很难做到10s以下延时，⽽RTMP协议的延时可以降到1s左右；

- TS文件分层

    - ts⽂件为传输流⽂件，视频编码主要格式为 H264/MPEG4，⾳频为 AAC/MP3。ts⽂件分为三层：
        ![TS文件分层](./images/TS文件分层.png)
        - ts层：Transport Stream，是在pes层的基础上加⼊数据流的识别和传输必须的信息；
        - pes层： Packet Elemental Stream，是在⾳视频数据上加了时间戳等对数据帧的说明信息；
        - es层：Elementary Stream，即⾳视频数据；

    - ts包⼤⼩固定为188字节，ts层分为三个部分：ts header、adaptation field、payload；
    - ts header固定4个字节；adaptation field可能存在也可能不存在，主要作⽤是给不⾜188字节的数据做填充；payload是pes数据；

    - ts header:
        | name | size | desc |
        | :-: | :-: | :-: |
        | sync_byte | 8b | 同步字节，固定为0x47 |
        | transport_error_indicator | 1b | 传输错误指示符，表明在ts头的adapt域后由⼀个⽆⽤字节，通常都为0，这个字节算在adapt域⻓度内 |
        | payload_unit_start_indicator | 1b | 负载单元起始标示符，⼀个完整的数据包开始时标记为1 |
        | transport_priority | 1b | 传输优先级，0为低优先级，1为⾼优先级，通常取0 |
        | pid | 13b | pid值 |
        | transport_scrambling_control | 2b | 传输加扰控制，00表示未加密 |
        | adaptation_field_control | 2b | 是否包含⾃适应区，‘00’保留；‘01’为⽆⾃适应域，仅含有效负载；‘10’为仅含⾃适应域，⽆有效负载；‘11’为同时带有⾃适应域和有效负载 |
        | continuity_counter | 4b | 递增计数器，从0-f，起始值不⼀定取0，但必须是连续的 |

    - ts层的内容是通过PID值来标识的，主要内容包括：PAT表、PMT表、⾳频流、视频流。解析ts流要先找到PAT表，只要找到PAT就可以找到PMT，然后就可以找到⾳视频流了。PAT表和PMT表需要定期插⼊ts流，因为⽤户随时可能加⼊ts流，这个间隔⽐较⼩，通常每隔⼏个视频帧就要加⼊PAT和PMT。PAT和PMT表是必须的，还可以加⼊其它表如SDT（业务描述表）等，不过hls流只要有PAT和PMT就可以播放了；

    - PAT表：主要的作⽤就是指明了PMT表的PID值；
    - PMT表：主要的作⽤就是指明了⾳视频流的PID值；
    - ⾳频流/视频流：承载⾳视频内容；

    - adaptation field:
        | name | size | desc |
        | :-: | :-: | :-: |
        | adaptation_field_length | 1B | ⾃适应域⻓度，后⾯的字节数 |
        | flag | 1B | 取0x50表示包含PCR或0x40表示不包含PCR |
        | PCR | 5B | Program Clock Reference，节⽬时钟参考，⽤于恢复出与编码端⼀致的系统时序时钟STC（System Time Clock） |
        | stuffing_bytes | xB | 填充字节，取值0xff |

    - PAT:
        | name | size | desc |
        | :-: | :-: | :-: |
        | table_id | 8b | PAT表固定为0x00 |
        | section_syntax_indicator | 1b | 固定为14 |
        | zero | 1b | 固定为0 |
        | reserved | 2b | 固定为11 |
        | section_length | 12b | 后⾯数据的⻓度 |
        | transport_stream_id | 16b | 传输流ID，固定为0x0001 |
        | reserved | 2b | 固定为11 |
        | version_number | 5b | 版本号，固定为00000，如果PAT有变化则版本号加1 |
        | current_next_indicator | 1b| 固定为1，表示这个PAT表可以⽤，如果为0则要等待下⼀个PAT表 |
        | section_number | 8b | 固定为0x00 |
        | last_section_number | 8b | 固定为0x00 |
        | 开始循环 | | |
        | program_number | 16b | 节⽬号为0x0000时表示这是NIT，节⽬号为0x0001时,表示这是PMT |
        | reserved | 3b | 固定为111 |
        | PID | 13b | 节⽬号对应内容的PID值 |
        | 结束循环 | | |
        | CRC32 | 32b | 前⾯数据的CRC32校验码 |

    - PMT:
        | name | size | desc |
        | :-: | :-: | :-: |
        | table_id | 8b | PMT表取值随意，0x02 |
        | section_syntax_indicator | 1b | 固定为1 |
        | zero | 1b | 固定为0 |
        | reserved | 2b | 固定为11 |
        | section_length | 12b | 后⾯数据的⻓度 |
        | program_number | 16b | 频道号码，表示当前的PMT关联到的频道，取值0x0001 |
        | reserved | 2b | 固定为11 |
        | version_number | 5b | 版本号，固定为00000，如果PAT有变化则版本号加1 |
        | current_next_indicator | 1b | 固定为1 |
        | section_number | 8b | 固定为0x005 |
        | last_section_number | 8b | 固定为0x00 |
        | reserved | 3b | 固定为111 |
        | PCR_PID | 13b | PCR(节⽬参考时钟)所在TS分组的PID，指定为视频PID |
        | reserved | 4b | 固定为1111 |
        | program_info_length | 12b | 节⽬描述信息，指定为0x000表示没有 |
        | 开始循环 | | |
        | stream_type | 8b | 流类型，标志是Video还是Audio还是其他数据，h.264编码对应0x1b，aac编码对应0x0f，mp3编码对应0x03 |
        | reserved | 3b | 固定为111 |
        | elementary_PID | 13b | 与stream_type对应的PID |
        | reserved | 4b | 固定为1111 |
        | ES_info_length | 12b | 描述信息，指定为0x000表示没有 |
        | 结束循环 | | |
        | CRC32 | 32b | 前⾯数据的CRC32校验码 |

    - pes:
        | name | size | desc |
        | :-: | :-: | :-: |
        | pes start code | 3B | 开始码，固定为0x000001 |
        | stream id | 1B | ⾳频取值（0xc0-0xdf），通常为0xc0;视频取值（0xe0-0xef），通常为0xe0 |
        | pes packet length | 2B | 后⾯pes数据的⻓度，0表示⻓度不限制，只有视频数据⻓度会超过0xffff |
        | flag | 1B | 通常取值0x80，表示数据不加密、⽆优先级、备份的数据 |
        | flag | 1B | 取值0x80表示只含有pts，取值0xc0表示含有pts和dts |
        | pes data length | 1B | 后⾯数据的⻓度，取值5或10 |
        | pts | 5B | 33bit值 |
        | dts | 5B | 33bit值 |

### RTSP



### WebSocket

- WebSocket 是 HTML5 开始提供的一种在单个 TCP 连接上进行全双工通讯的协议；

- WebSocket 使得客户端和服务器之间的数据交换变得更加简单，允许服务端主动向客户端推送数据。在 WebSocket API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输；

    ![Ajax和WebSocket](./images/Ajax和WebSocket.png)

- 浏览器通过 JavaScript 向服务器发出建立 WebSocket 连接的请求，连接建立以后，客户端和服务器端就可以通过TCP 连接直接交换数据；

- 当你获取 Web Socket 连接后，你可以通过 send() 方法来向服务器发送数据，并通过onmessage 事件来接收服务器返回的数据；

- 为了建立一个 WebSocket 连接，客户端浏览器首先要向服务器发起一个 HTTP 请求，这个请求和通常的 HTTP 请求不同，包含了一些附加头信息，其中附加头信息"Upgrade: WebSocket"表明这是一个申请协议升级的 HTTP 请求，服务器端解析这些附加的头信息然后产生应答信息返回给客户端，客户端和服务器端的 WebSocket 连接就建立起来了，双方就可以通过这个连接通道自由的传递信息，并且这个连接会持续存在直到客户端或者服务器端的某一方主动的关闭连接；

- 服务器端使用websocket需要安装nodejs-­websocket

    ```shell
    cd 工程目录
    sudo npm init
    #创建package.json文件
    sudo npm install nodejs-websocket
    ```

### WebRTC

- WebRTC简介

    - WebRTC（Web Real­Time Communication）是 Google于2010以6829万美元从 Global IP Solutions 公司购买，并于2011年将其开源，旨在建立一个互联网浏览器间的实时通信的平台，让 WebRTC技术成为 H5标准之一；
    - WebRTC虽然冠以“web”之名，但并不受限于传统互联网应用或浏览器的终端运行环境。实际上无论终端运行环境是浏览器、桌面应用、移动设备（Android或iOS）还是IoT设备，只要IP连接可到达且符合WebRTC规范就可以互通。这一点释放了大量智能终端（或运行在智能终端上的app）的实时通信能力，打开了许多对于实时交互性要求较高的应用场景的想象空间，譬如在线教育、视频会议、视频社交、远程协助、远程操控等等都是其合适的应用领域。全球领先的技术研究和咨询公司Technavio最近发布了题为“全球网络实时通讯（WebRTC）市场，2017­2021”的报告。报告显示，2017­2021年期间，全球网络实时通信（WebRTC）市场将以34.37％的年均复合增长率增长，增长十分迅速。增长主要来自北美、欧洲及亚太地区；

- WebRTC框架

    ![WebRTC架构](./images/WebRTC架构.png)

- WebRTC通话原理

    - 媒体协商

        - 有一个专门的协议，称为Session Description Protocol(SDP)，可用于描述上述这类信息，在WebRTC中，参与视频通讯的双方必须先交换SDP信息，这样双方才能知根知底，而交换SDP的过程，也称为"媒体协商"。

    - 网络协商

        ![WebRTC网络协商](./images/WebRTC网络协商.png)
        - 彼此要了解对方的网络情况，这样才有可能找到一条相互通讯的链路
        (1)获取外网IP地址映射；
        (2)通过信令服务器（signal server）交换"网络信息"。
        - 理想的网络情况是每个浏览器的电脑都是私有公网IP，可以直接进行点对点连接。
        - 实际情况是：我们的电脑和电脑之前或大或小都是在某个局域网中，需要NAT（Network Address Translation，网络地址转换）。
        - 在解决WebRTC使用过程中的上述问题的时候，我们需要用到STUN和TURN。

    - STUN

        - STUN（Session Traversal Utilities for NAT，NAT会话穿越应用程序）是一种网络协议，它允许位于NAT（或多重NAT）后的客户端找出自己的公网地址，查出自己位于哪种类型的NAT之后以及NAT为某一个本地端口所绑定的Internet端端口。这些信息被用来在两个同时处于NAT路由器之后的主机之间创建UDP通信。该协议由RFC 5389定义。

    - TURN
      
        - TURN的全称为Traversal Using Relays around NAT，是STUN/RFC5389的一个拓展，主要添加了Relay功能。如果终端在NAT之后，那么在特定的情景下，有可能使得终端无法和其对等端（peer）进行直接的通信，这时就需要公网的服务器作为一个中继，对来往的数据进行转发。这个转发的协议就被定义为TURN。

    - 信令服务器

        ![信令服务器](./images/信令服务器.png)

        - 在基于WebRTC API开发应用（APP）时，可以将彼此的APP连接到信令服务器（Signal Server，一般搭建在公网，或者两端都可以访问到的局域网），借助信令服务器，就可以实现上面提到的SDP媒体信息及Candidate网络信息交换。信令服务器不只是交互媒体信息sdp和网络信息candidate，不如房间管理和人员进出。

    - 一对一通话

        ![WebRTC一对一通话](./images/WebRTC一对一通话.png)
        
        - 在一对一通话场景中，每个 Peer均创建有一个 PeerConnection 对象，由一方主动发 Offer SDP，另一方则应答AnswerSDP，最后双方交换 ICE Candidate 从而完成通话链路的建立。但是在中国的网络环境中，据一些统计数据显示，至少1半的网络是无法直接穿透打通，这种情况下只能借助TURN服务器中转。

