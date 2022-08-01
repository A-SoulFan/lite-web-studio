# Lite Studio

以 csv 文件作为数据库的纯前端音乐播放器，使用 vite 进行构建

# CHANGE LOG

- 2022-02-05
  - 从[这里](https://github.com/K-bai/csv-based-web-music-player)fork 代码

## Project setup

```
npm install
```

### Compiles and hot-reloads for development

```
npm run dev
```

### Compiles and minifies for production

```
npm run build
npm run build -- --mode staging
npm run preview
```

> **Note**
> 如果`public/favicon.svg`被替换，请执行
> 
> `npm i convert-svg-to-png&&npm run genimg`
> 
> 以生成PWA所依赖的不同尺寸LOGO

### Lints and fixes files

```
npm run lint
```

### Customize configuration

See [Configuration Reference](https://cn.vitejs.dev/config/#configuring-vite).

## Task List

- [x] vue3 project creation with vite.
- [x] Refactor csv-based-web-music-player modules into vue3 compisation API base.
- [x] Implement requests based on axios
- [x] Implement local cache based on indexed lib dexie.
- [ ] Implement save playlist to localstorage feature
- [x] Use lodash to implemmnt shuffle for the audio play shuffle mode.

## Project Dev Guideline

### Project Branche

Main: feature development for origin lite web studio. Please use the origin to contribute. No pull request will be accepted for this branch.
A-soul: A-soul customzied version of lite web studio, any new feature pull request please submit to here.
Release: branch for auto deplyment to [AS 录音棚 CN](https://studio.asf.ink/) or [AS 录音棚 Global](https://studio.a-soul.fans/)

### Directory Structure

```
LITE-WEB-STUDIO
├─public
│  └─datasheets
└─src
    ├─apis
    ├─assets
    │  └─ui
    ├─components
    │  └─popup
    ├─globals
    ├─styles
    └─utils
```

- public: 公共资源目录
  - datasheets: 数据库文件存放
- src: 项目代码
  - apis: 远程调用 API 格纳
  - assets: 静态资源
    - ui: ui 相关 svg 格纳
  - components: 播放器控件
    - popup: 弹窗相关功能子控件
  - globals: 全局变量结构定义，全局常量设置
  - styles: 各界面式样文件
  - utils: 共通处理

### CSV File Reference

**注意**：各属性前后请勿留存空格
| Field Name | Type | Mandatory | Description |
| :--- | :--- | :---: | :--- |
| id | String | Yes | 格式:**A000000**。唯一 ID。A 之后请使用 CSV 自动递增赋值。页面内控件显示状态等有关，和歌曲资源访问无关。 |
| 日期 | Date | Yes | 格式:**YYYY.MM.DD**。请不要省略单数字月日前面的 0，保持 01，02 这样的数值。歌曲采集的录播所在日期。和歌曲名共通决定歌曲资源寻址。 |
| 录播来源 | String | No | 用来提供 B 站相关录播传送门。 |
| 录播片段编号 | Number | No | B 站录播的分 P 号。配合**录播来源**和**起始时间点**以及**结束时间点**可以定位跳转 B 站相关录播节点。 |
| 起始时间点 | Number | Yes | B 站分 P 起始时间点。分 P 未收录是使用 0 代替。分 P 存在时格式**00:00:00.000** |
| 结束时间点 | Number | Yes | B 站分 P 起始时间点。**起始时间点**为 0 时，请使用歌曲 ms 数记录总歌曲时长。分 P 存在时格式**00:00:00.000** |
| 文件名 | String | | 存在时优先被使用来匹配资源，不填写时请使用**歌名**，**版本号**和**版本备注**来匹配资源文件 |
| 文件类型 | String | Yes | 文件的扩展名 |
| 歌名 | String | | 歌曲名称。最终和**版本号**以及**版本备注**会按照：日期 歌名【版本号 版本备注】格式来匹配 |
| 版本号 | String | | 歌曲版本号，跟随演唱者升级版本，共同演唱时取最大版本号同步至每一位演唱者作为后续的起步版本号。 |
| 版本备注 | String | | 歌曲内容的备注描述，如改词，钢琴伴奏等 |
| 中文歌名 | String | No | 歌名之后显示在括号里的内容，可以用作歌曲的备注添加用。 |
| 原曲艺术家 | String | No | 原曲作者信息。 |
| 演唱者 | String | Yes | 收录曲演唱者。多人存在的情况下请使用英文""和,来记载多人合唱信息，如："AA,BB"。 |
| 演唱状态 | String | Yes | 是否整首演唱或者片段。 |
| 语言 | String | Yes | 歌曲演唱语言。 |
| 备注 | String | No | 关于歌曲较长的备注信息，比如剪辑版本的备注等。 |
| 参考路灯 man | String | No | 歌曲切片提供者信息 |
| 有没有音频 | Boolean | Yes | TRUE 或者 FALSE。用来标注是否歌曲切片已被收录入库并处在可以播放的状态。 |
| 切片源 | String | No | 切片来自的源文件传送门。 |
| 有没有第二版本 | Boolean | Yes | TRUE 或者 FALSE。用来标注是否歌曲切片存在优化版本或单曲版本已被收录入库处在可以播放的状态。 |
| 第二版文件类型 | String | Yes | 次版本文件的扩展名设置 |

#### Song File Naming Convention

为方便音源入库，本文档简要说明文件名命名规范

请切片 man 在完成切片后参照 csv 命名，如果 csv 有误，请在切片组内反馈。

- 文件名格式为：**时间+空格+参与者代号+空格+歌曲名称【版本号+空格+版本备注】**

  > 如：2022.01.05 C 芒种【2.0 预录】

- 歌曲名称请注意英文大小写和简体繁体字，如《素敌だね》不等于《素敵だね》
- 参与成员代号有：ABCDEFL
- 版本备注有：预录 合唱 改词 单曲 视频，等等。

```
例1（单人演唱）
2022.03.02 E 牛奶面包.m4a
时间：2022.03.02
参与者：E，Eileen，代表乃琳
歌曲名称：牛奶面包

例2（2-3人合作）
2021.09.21 CD 青城山下白素贞.m4a
参与者：CD，"珈乐,嘉然"，按字母顺序排序
2022.02.04 ADE 吉祥三宝.m4a
参与者：ADE，"向晚,嘉然,乃琳"，按字母顺序排序
注：合作歌曲包括所有参与者，如《花海》由向晚伴奏，乃琳演唱，标为AE
《繁花》由贝拉伴舞，乃琳演唱，标为BE

例3（4-5人合作，F）
2022.02.03 F 一千零一个愿望.m4a
参与成员代号：F，4-5位成员参与

例4（联动，L）
2021.04.17 L Hopeful Dreamer【合唱版】.mp3
参与者："嘉然,中国绊爱"
2022.02.03 L 除夕【团曲】.mp3
参与者："A-SOUL,音阙诗听"

例5（版本号 版本备注）
2021.10.28 DE 私奔到月球【2.0】.m4a
2022.01.14 E Sweet Counter【单曲】.m4a
2022.01.05 C 芒种【2.0 预录】.m4a
```

### Naming Convention

- 变量名，函数参数：Camel Case 小驼峰式命名法：首字母小写。开头单词为名词。eg：studentInfo、userInfo
- 方法名：Camel Case 小驼峰式命名法：首字母小写。开头单词为动词。eg：canRead、getName
- 函数内局部临时变量名: 前导下划线小字母。 eg：\_studentinfo, \_username
- 组件名，全局变量名：Pascal Case 大驼峰式命名法：首字母大写。eg：AudiooPlayer、HelloWorld
- 全局变量属性名，缓存对象名,object 属性名：Snake Case 蛇形命名法。全小写字母借由下划线连接。eg：empty_song、code_src
- bus event 事件名称：Kebab Case 短横线命名。单词以 ‘-’ 短横线连接，最后以 event 结尾。eg：update-song-list-event
