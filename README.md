## 如何编写简历中的项目经验

### 如何编写项目经验

- 项目名称和你的角色
- 简洁明了
- 突出你的使用技术和工具
- 你的贡献 突出你的工作内容，负责的部分，以及如何实现，遇到什么挑战，如何通过努力克服解决的
- 展示你的成果和影响 提供具体的成果展示，付费率上升 20%，营收 增长 30%，使用数据和百分比来进行量化
- 项目成就和成长 达成了什么成就？获得了什么成长？积累了什么经验？
- 保持相关性
- 格式要简洁美观 有些关键内容可以使用粗体和斜体来进行强调
- 项目亮点和难点 亮点和难点非常的关键，因为它们可以帮你引导面试官

## 精心准备亮点并且写到项目经验中

### 业务背景

### 面临挑战

### 实现原理

### 代码实现

### 难点扩展

- 文件加密
- 传输加密
- 文件压缩处理
  ...

## 处理文件的上传

- 为了提升性能，在上传大文件的时候，可以把一个大文件切成多个小文件，然后并行上传
- 另外为了后在实现类似秒传的功能，所以需要对文件进行唯一的标识
- 所以我们需要根据文件的内容生成一个 hash 值来唯一的这一个文件。
  - 文件内容如果一样，产生的文件名是一样

let chunk = file.slice(i*CHUNK_SIZE,(i+1)*CHUNK_SIZE);

250M
CHUNK_SIZE=100M
count=3
第一块 0->100M
第二块 100M-200M
第三块 200-250

扩展点

- 实现点击选择文件上传
- 并发量队列控制，在同一时间内最多并行上传 5 个分片
- 切片数量的动态调整，自适应
- 合并之后做一下文件校验，保证文件未被篡改
- 数据的对称加密
- 云服务器上传

请求体用文件流与 formdata 有什么区别
用文件流的话只能上传二进制文件
如果用 formData 的话可以同时上传文件和普通字段

这个进度是每一个切片的进度吧 是的
你还可以计算一个总进度

那要是吧文件上传服务器，不保存本地还用切片上传吗？

并行写入没问题么？搞个 1g 的大文件试试

没看出来是如何渲染出多个进度条的

应该吧所有分片进度 综合成一个总进度吧

解决网络发生错误的情况，或者用户点了暂停又继续的情况
或者此文件服务器上已经存在了，则不需要重复上传

要想实现秒传的功能，需要服务器提供一个接口，返回已经上传的分片和大小

uploadSize 是每个切片的其实位置吗？

进度条可以继续以上次为基础增加么

重传位置吗

暂停上传后，关掉浏览器会不会是重新上传呢

动态分片是如何做的？并发限制？出错重试上传机制？

progress 进度条设置错了

读到了上次的数据是进度条的问题吧

进度条计算那吧

进度条每次都是从 0 计算的

4 点继续

文件 hash 是怎么计算的呢？可以保证同一个文件是相的 hash 吗？

假如说要设计一个不跟技术栈耦合的大文件上传的 SDK，该从那些方面思考

刚那个进度条的问题是不是因为 axios

刚那个进度条的问题是不是因为 axios 的 update

事件他的计算问题

## 如何实现点击上传文件

## 如何确定合理的文件切片大小

考虑上传效率，还要考虑网络稳定性
切片过大可能会导致 上传失败，尤其是在网络不稳定的情况
而切片过小呢会导致出现太多的 HTTP 请求，降低上传效率
所以切片的大小根据实际情况进行考虑
网络状态
服务器限制
并发上传
根据试验进行调整
进度反馈

综合考虑一般来说切片的大小在 1Mb 到 10M 之间

## 如何控制并发上传的数量

处理多个切片上传的时候，确定确定合适的并发上传的数量
服务器的处理能力
用户的网络带宽

一般来说会在客户端维护类似的滑动窗口机制
比如说我有 100 个切片，每个切片是 10M
每次上传的控制在同一个时间点并发的上传分片是 5 个
有任何一个分片传完了，再取出一个分片上传

## 文件的校验

为了确保上传的文件在传输过程中没有被 篡改，可以增加一些校验机制
在我们这个例子中，我们的文件名就是通过 hash 算法 SHA-256 得到了
const hashBuffer = await crypto.subtle.digest('SHA-256', arrayBuffer);

## 文件加密

- 如果你使用 ssh https 协议
- 如果你使用的是 http 协议，可以在客户端对分片数据进行加密，在服务器机密
- 这个称为对称加密，需要双方维护一个密钥
- 最常用的算法就是 AES

## 上传云服务 阿里云

- 一般来说是当你要上传的时候你要先请求服务器
- 服务器会给一个带签名的临时 URL 地址或 STS(安全令牌)
- # 你可以直接向此地址或者使用此令牌直接向云服务上传文件

# BigFile

BigFile
