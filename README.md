![Logo](https://github.com/greats3an/pyncm/raw/master/demos/_logo.png)

# PyNCM

# 安装
    pip install pyncm
可选 （若不考虑使用CLI则请忽略）
- `mutagen` : 为下载的音乐打上封面等
- `tqdm`    : 显示实时下载进度
- `coloredlogs` : 彩色日志输出

**Windows 用户**: 在 [Releases](https://github.com/mos9527/pyncm/releases) 可下载已打包 `.exe` 版本
# 命令行使用
        positional arguments:
        链接                    网易云音乐分享链接

        optional arguments:
        -h, --help            show this help message and exit

        下载:
        --max-workers 最多同时下载任务数, -m 最多同时下载任务数
        --output-name 保存文件名模板, --template 保存文件名模板, -t 保存文件名模板
                                保存文件名模板
                                    参数：    
                                        id     - 网易云音乐资源 ID
                                        year   - 出版年份
                                        no     - 专辑中编号
                                        album  - 专辑标题
                                        track  - 单曲标题        
                                        title  - 完整标题
                                        artists- 艺术家名
                                    例：
                                        {track} - {artists} 等效于 {title}
        -o 输出, --output 输出    输出文件夹
                                    注：该参数也可使用模板，格式同 保存文件名模板
        --quality 音质          音频音质（高音质需要 CVIP）
                                    参数：
                                        hi-res  - Hi-Res
                                        lossless- “无损”
                                        exhigh  - 较高
                                        standard- 标准
        --no-overwrite        不重复下载已经存在的音频文件

        歌词:
        --lyric-no 跳过歌词 [跳过歌词 ...]
                                跳过某些歌词类型的合并
                                    参数：
                                        lrc    - 源语言歌词
                                        tlyric - 翻译后歌词
                                        romalrc- 罗马音歌词
                                    例：
                                        --lyric-no tlyric --lyric-no romalrc 将只下载源语言歌词

        登陆:
        --phone 手机            网易账户手机号
        --pwd 密码, --password 密码
                                网易账户密码
        --save [保存到]          写本次登录信息于文件
        --load [保存的登陆信息文件]    从文件读取登录信息供本次登陆使用
        --http                优先使用 HTTP，不保证不被升级
        --log-level LOG_LEVEL
                                日志等级

## 环境变量
|变量名|说明|
|-|-|
|`PYNCM_DEBUG`|调试日志输出等级,`'CRITICAL', 'DEBUG', 'ERROR','FATAL','INFO','WARNING'` 之一|
### 使用示例
## 下载单曲
[![asciicast](https://asciinema.org/a/4PEC5977rTcm4hp9jLuPFYUM1.svg)](https://asciinema.org/a/4PEC5977rTcm4hp9jLuPFYUM1)
## 使用 [UNM](https://github.com/UnblockNeteaseMusic/server) 下载灰色歌曲
[![asciicast](https://asciinema.org/a/AX4cdzD7YcgQlTebAdCTKZQnb.svg)](https://asciinema.org/a/AX4cdzD7YcgQlTebAdCTKZQnb)
其他功能详见 
- [Demo](https://github.com/mos9527/pyncm/tree/master/demos)

# API 使用示例
```python
>>> from pyncm import apis
# 获取歌曲信息    
>>> apis.track.GetTrackAudio(29732235)
{'data': [{'id': 29732235, 'url': 'http://m701.music...
# 获取歌曲详情
>>> apis.track.GetTrackDetail(29732235)    
{'songs': [{'name': 'Supernova', 'id': 2...
# 获取歌曲评论
>>> apis.track.GetTrackComments(29732235)    
{'isMusician': False, 'userId': -1, 'topComments': [], 'moreHot': True, 'hotComments': [{'user': {'locationInfo': None, 'liveIn ...
```
- 多 Session 示例
```python
LoginViaEmail(...) 
# 利用全局 Session 完成该 API Call
session = CreateNewSession() # 建立新的 Session
with session: # 进入该 Session, 在 `with` 内的 API 将由该 Session 完成
    LoginViaCellPhone(...)
    result = GetTrackAudio(...)
# 离开 Session. 此后 API 将继续由全局 Session 管理
GetTrackComments(...)
```
详见 [Session 说明](https://github.com/mos9527/pyncm/blob/master/pyncm/__init__.py#L52)
## API 说明
大部分 API 函数已经详细注释，可读性较高。推荐参阅 [API 源码](https://github.com/mos9527/pyncm/tree/master/pyncm) 获得支持

## FAQ
- 为什么 `GetTrackAudio` 几乎拿不到音频 URL？

你需要[进行登陆](https://github.com/mos9527/pyncm/blob/master/pyncm/apis/login.py)。若身边没有合适的账号，也可选择匿名登陆：
```python
>>> from pyncm.apis.login import LoginViaAnonymousAccount
>>> LoginViaAnonymousAccount()
{'tick': 1662870122.1159196,
 'content': {'code': 200,
  'userId': 8023914528,
  'createTime': 1662868134354,
  'profile': {'nickname': 'Ano...
```
# 感谢
[Android逆向——网易云音乐排行榜api(上)](https://juejin.im/post/6844903586879520775)

[Binaryify/NeteaseCloudMusicApi](https://github.com/Binaryify/NeteaseCloudMusicApi)

### 衍生项目
[PyNCMd](https://github.com/mos9527/pyncmd)
