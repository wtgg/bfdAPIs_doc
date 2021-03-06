# 与百分点对接的接口
# 社交网络接口

## 目录
### 1. [新增账号的验证](#新增账号的验证)
### 2. [~~新增社交网络爬虫任务~~](#新增社交网络爬虫任务)
### 2. [新增社交网络爬虫任务](https://github.com/wtgg/bfdAPIs_doc/blob/master/新增采集任务.md)
### 3. [更新爬虫任务执行数据](#更新爬虫任务执行数据)
### 4. [新增社交网络帖子](#新增社交网络帖子)
### 5. [新增社交网络人物信息](#新增社交网络人物信息)
### 6. [社交网络人物账号详情](#社交网络人物账号详情)
### 7. [社交网络帖子详情](#社交网络帖子详情)
### 8. [任务执行详情](#任务执行详情)
### 9. [新增社交网络帖子的评论](#新增社交网络帖子的评论)

---
### 新增账号的验证
> 此接口用于验证用户自定义的人物账号是否有效，防止用户胡乱输入无效人物
> 电科请求百分点

**请求方式：**
- POST

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|website|  是  |string |网站名|
|at_name|  是  |string |人物在网站的账号|
|remark_name|  是  |string |用户给人物的自定义备注名|

Form Data示例:
```json
{
    "website": "facebook",
    "at_name": "DonaldTrump",
    "remark_name": "特朗普"
}
```


**返回结果：**

```json
失败：
{
    "valid": 0,
    "msg": "无效账号！",
    "website": "facebook",
    "at_name": "DonaldTrump"
}

--------------------------------------
成功：
{
    "valid": 1,
    "msg": "有效账号！",
    "website": "facebook",
    "at_name": "DonaldTrump"
}
```
### [返回目录](#目录)

### 新增社交网络爬虫任务
> 电科请求百分点

**请求方式：**
- POST

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|je_id|  是  |string或int |爬虫任务id|
|spider_type|  是  |string |任务类型，可能的值有2个："社交网络采集"或"关键词采集"|
|website|  是  |string |网站名|
|keywords|  是  |string |采集内容，根据spider_type的值有不同的形式，见示例|
|at_name|  否  |string |所要采集人物在对应网站的账号|
|remark_name|  否  |string |用户给人物的自定义备注名|





Form Data示例:
```json
-----------------------------------示例1-----------------------------------
{
  "je_id": 1234,
  "spider_type":"社交网络采集",  //spider_type为社交网络采集时keywords是序列化的字典
  "website": "facebook",
  "keywords": "{\\"at_name\\": \\"DonaldTrump\\", \\"remark_name\\": \\"特朗普\\"}"
}
-----------------------------------示例2-----------------------------------
{
  "je_id": 1234,
  "spider_type":"关键词采集",  //spider_type为社交网络采集时keywords是字符串
  "website": "facebook",
  "keywords": "特朗普"
}

```



**返回结果：**

```json

成功：
{
    "je_id": 1234,
    "status": "ok",
    "msg": "任务添加成功，爬虫即将运行。",
    "website": "facebook",  // 或者 twitter
    
}
```
### [返回目录](#目录)

### 更新爬虫任务执行数据
>  百分点请求电科

`/api/job_excution/update/<je_id>`

**请求方式：**
- PUT

**Header：**

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|token |  是  |    string   |    用来作请求验证  |

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|je_id|  是  |string  |电科系统中的执行id,此参数直接放在在url中,例如`/api/job_excution/update/dev_123`|
|running_status|  否  |int |此次(百分点系统中的)爬虫执行状态：0即将运行, 1正在运行，2已完成, 3被中止|
|end_timeTS|  否 |string或int |此次(百分点系统中的)爬虫运行结束的时间戳，精确到秒|
|finish_reason|  否 |string |此次(百分点系统中的)爬虫运行结束的原因，比如爬取到重复数据已超过3条，用户中止等等|
|error_msg|  否 |string |此次(百分点系统中的)爬虫报错信息|

**返回结果：**

```json

成功：
{
    "status": "ok",
    "msg": "更新成功",
    "je_id": "dev_123"  // 电科系统中的执行id
}
```
### [返回目录](#目录)

### 新增社交网络帖子
> 百分点请求电科

`/api/snp/add`

**请求方式：**
- POST

**Header：**

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|token |  是  |    string   |    用来作请求验证  |

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|je_id|  是  |string |爬虫任务id|
|spider_timeTS|  是  |string或int |采集时间戳，精确到秒|
|post_id|  是  |string或int |帖子在来源网站的id|
|post_url|  是  |string |帖子在来源网站的url|
|post_timeTS|  是  |string或int |发帖时间戳，精确到秒|
|post_content|  是  |string |帖子内容|
|post_content_translated|  是  |string |帖子内容汉化|
|language|  是  |string |原文语种|
|comments|  是  |string |取最新的200条评论|
|comments_count|  是  |string或int |当前帖子的评论条数|
|share_count|  是  |string或int |当前帖子的分享或转发次数|
|is_transmitted|  是  |int |是否是转发别人的帖子，是1，否0|
|like_count|  是  |string或int |当前帖子的点赞次数|
|dislike_count|  是  |string或int |当前帖子的点踩次数|
|imgs|  是  |string |当前帖子中包含的图片的url,列表，可以为空|
|imgs_src|  是  |string |当前帖子中包含的图片的原始src,列表，可以为空|
|video_url|  是  |string |当前帖子中包含的视频的url，可以为空|
|links|  是  |string |当前帖子中包含的链接的url，可以为空|
|hashtags|  是  |string |hashtags信息，可以为空|
|other_info|  是  |string |其他爬到的字段，序列化的字典类型，可以为空|
|site_name|  是  |string |源网站 `facebook`或者`twitter`|
|at_name|  是  |string |帖子发布者的账号 @后面的字符串|
|user_name|  是  |string |帖子发布者的昵称 @上面的字符串|





Form Data示例:
```json

----------------------------------------示例1----------------------------------------
{
    "je_id": "pro_123",  // 爬虫任务id
    "spider_timeTS": 1586391992,  // 采集时间戳
    "post_id": "10164386532535725",  // 帖子在网站的id
    "post_url": "https://www.facebook.com/DonaldTrump/posts/10164386532535725",  //帖子url
    "post_timeTS": "1586391912",  // 发帖时间戳
    "post_content": "TOMORROW: Team Trump hosts Empower Hour at 1:00 pm ET featuring Lara Trump, RNC Chairwoman Ronna McDaniel, Michigan Republican Party Chairwoman Laura Cox, and Dr. Merlynn Carson! RSVP: bit.ly/2JSrV28 #EmpowerHour",
    "post_content_translated": "明天: 特朗普团队主持人在东部时间下午1:00点, 由lara trump, rnc主席ronna mcdaniel, 密歇根州共和党主席劳拉·考克斯和merlyn carson博士!  回复:  #EmpowerHour · ",
    "language": "英语",
    "share_count": "650",
    "is_transmitted": 0,
    "like_count": "1.5万赞",  // 尽量精确
    "dislike_count": null,
    "imgs": [
        "https://scontent-hkg3-2.xx.fbcdn.net/v/t1.0-0/p526x296/92808688_2601304416825766_2255410206143938560_o.jpg?_nc_cat=1&_nc_sid=8024bb&_nc_ohc=CWF30rtu2ycAX8tFUKv&_nc_ht=scontent-hkg3-2.xx&_nc_tp=6&oh=b3e3897bec0c12ab0529f417b10222dc&oe=5EB3AFFF",
        "https://scontent-hkg3-2.xx.fbcdn.net/v/t1.0-0/p526x296/92808688_2601304416825766_2255410206143938560_o.jpg?_nc_cat=1&_nc_sid=8024bb&_nc_ohc=CWF30rtu2ycAX8tFUKv&_nc_ht=scontent-hkg3-2.xx&_nc_tp=6&oh=b3e3897bec0c12ab0529f417b10222dc&oe=5EB3AFFF",
        "https://scontent-hkg3-2.xx.fbcdn.net/v/t1.0-0/p526x296/92808688_2601304416825766_2255410206143938560_o.jpg?_nc_cat=1&_nc_sid=8024bb&_nc_ohc=CWF30rtu2ycAX8tFUKv&_nc_ht=scontent-hkg3-2.xx&_nc_tp=6&oh=b3e3897bec0c12ab0529f417b10222dc&oe=5EB3AFFF"
    ],
    "video_url": null,
    "links": null,
    "comments_count": "1511",
    "hashtags": {
        "xx": "xxxx",
        "xxx": "xxxx"
    },
   "other_info": {
        "loveCount": "1,708大爱",
    },
    "site_name": "facebook",
    "at_name": "DonaldTrump",
    "user_name": "Donald J. Trump",
}
----------------------------------------示例2----------------------------------------
{
    "je_id": "dev_1234",  // 爬虫任务id
    "spider_timeTS": 1592384301,  // 采集时间戳
    "post_id": "10164386532535725",  // 帖子在网站的id
    "post_url": "https://twitter.com/IvankaTrump/status/1272998476171878400",  //帖子url
    "post_timeTS": "1592341462",  // 发帖时间戳
    "post_content": "Thank you Bishop Mann for hosting us at the Pentecostal Temple Church of God in Christ in Pittsburgh, PA.\n\nThrough amazing faith based organizations such as yours our #FarmerstoFamilies food box program has delivered over 20 M boxes of fruit, meat and dairy to families in need! pic.twitter.com/b4ZWkgFCQc",
    "post_content_translated": "感谢曼恩主教在宾夕法尼亚州匹兹堡基督五旬节神殿教堂接待我们。\n\n通过像你们这样令人惊叹的以信仰为基础的组织，我们的“农夫家庭食品盒计划”已经为需要帮助的家庭提供了超过2000万箱水果、肉和奶制品！pic.twitter.com/b4ZWkgFCQ",
    "language": "英语",
    "share_count": "1259",
    "is_transmitted": 0,
    "like_count": "6346",  // 尽量精确
    "dislike_count": null,
    "imgs": [
        "/group1/default/20201015/11/21/1/41_file",
        "/group1/default/20201015/11/21/1/42_file",
        "/group1/default/20201015/11/21/1/43_file",
        "/group1/default/20201015/11/21/1/44_file"
    ],
    "imgs_src": [
            "https://pbs.twimg.com/media/EaqZVswXsAEf0lg.jpg",
            "https://pbs.twimg.com/media/EaqZV6bXsAUn8z_.jpg",
            "https://pbs.twimg.com/media/EaqZWQsWAAAmMkc.jpg",
            "https://pbs.twimg.com/media/EaqZXA9XgAAqB53.jpg"
        ],
    "video_url": null,
    "links": null,
    "comments_count": "1511",
    "hashtags":["#FarmerstoFamilies"],
   "other_info": null,
    "site_name": "facebook",
    "at_name": "IvankaTrump",
    "user_name": "Ivanka Trump",

}
```
### [返回目录](#目录)

### 新增社交网络人物信息
> 百分点请求电科
`/api/sna/add`

**请求方式：**
- POST

**Header：**

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|token |  是  |    string   |    用来作请求验证  |

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|je_id|  是  |string | 爬虫任务的唯一标识 |
|spider_timeTS|  是  |string或int |采集时间戳，精确到秒|
|website|  是  |string|网站，facebook或twitter|
|at_name|  是  |string |人物在网站的账号，@后面的字符串|
|user_name|  是  |string |人物在网站的名字|
|user_id|  是  |string或int  |账号在该网站的id|
|created_atTS|  是  |string或int |账号注册日期时间戳|
|regtime|  是  |string |账号注册日期|
|description|  是  |string或int  |人物描述信息|
|personal_home_page|  是  |string  |账号在网站的主页|
|avatar_url|  是  |string|人物头像|
|avatar_src|  是  |string|人物头像图片的原始src|
|birthday|  是  |string|生日|
|followers_count|  是  |string或int  |粉丝数|
|friends_count|  是  |string或int  |好友数|
|location|  是  |string  |位置信息|
|media_count|  是  |string或int  |媒体数|
|posts_count|  是  |string或int  |发帖数|






Form Data示例:
```json

----------------------------------------示例1----------------------------------------
{
    "je_id": "dev_1234",  // 电科采集系统任务的唯一标识
    "spider_timeTS": 1586391992,  // 采集时间戳
    "website": "facebook",  // 或twitter
    "at_name": "DonaldTrump",  
    "user_name": "Donald J. Trump",
    "created_atTS": 1386391992,
    "regtime": "2013年4月",
    "description": "商家位置725 Fifth Ave纽约查询路线其他联系方式http://www.donaldjtrump.com简介This is the official Facebook page for Donald J. Trump性别男categories政治候选人 · 政界人士商家故事45th President of the United StatesDonald J. Trump has always dreamed big and pushed the boundaries of what is possible. He’s devoted his life to building business, jobs and achieving the American Dream – now, that is what he is doing for our country.In June 2015, Donald Trump announced his candidacy for President of the United States and started a movement unlike anything before. T... ",
    "personal_home_page": "https://www.facebook.com/donaldtrump",
    "avatar_url": "/group1/default/20200604/04/33/1/71_file"  // fdfs地址,
    "avatar_src": "https://scontent-hkg3-1.xx.fbcdn.net/v/t1.0-0/p526x296/92133248_2596276283995246_6699196800395378688_o.jpg?_nc_cat=1&_nc_sid=8024bb&_nc_ohc=iX5oFfYLpfIAX8t8mA0&_nc_ht=scontent-hkg3-1"  // 图片的原始地址,
    "is_transmitted": 0,
    "like_count": "1.5万赞",  // 尽量精确
    "birthday": "1948-06-06",
    "followers_count": 44551556656,
    "friends_count": 47,
    "location": "Washington D.C",
    "media_count": "1511",
    "posts_count": 5646545
}
```
### [返回目录](#目录)


### 社交网络人物账号详情
> 百分点请求电科
`/api/sna/detail/<sna_id>`

**请求方式：**
- GET

**Header：**

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|token |  是  |    string   |    用来作请求验证  |

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|sna_id|  是  |string或int |社交网络账号表的id,url参数|






返回结果示例:
```json
{
    "sna_id": 1,
    "spider_time": "2019-10-12 16:04:17",
    "website": "twitter",
    "at_name": "realDonaldTrump",
    "user_name": "Donald J. Trump",
    "remark_name": "美国总统",
    "created_at": "2009-03-18 21:46:38",
    "description": "45th President of the United States of America??",
    "user_id": "VXNlcjoyNTA3Mzg3Nw==",
    "personal_home_page": "https://twitter.com/realdonaldtrump",
    "fast_followers_count": 2245,
    "favourites_count": 7,
    "followers_count": 65600254,
    "friends_count": 47,
    "location": "Washington, DC",
    "media_count": 3455,
    "posts_count": 45126,
    "avatar_url": "https://pbs.twimg.com/profile_images/874276197357596672/kUuht00m_400x400.jpg",
    "birthday": null
}

```
### [返回目录](#目录)


### 社交网络帖子详情
> 百分点请求电科
`/api/snp/detail/<snp_id>`

**请求方式：**
- GET

**Header：**

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|token |  是  |    string   |    用来作请求验证  |

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|snp_id|  是  |string或int |社交网络帖子表的id,url参数|






返回结果示例:
```json
{
    "snp_id": 300,
    "sna_id": 5,
    "spider_time": "2020-07-31 11:22:48",
    "post_id": "10165181176165725",
    "post_url": "https://www.facebook.com/DonaldTrump/photos/a.10156483516640725/10165181176165725",
    "post_datetime": "2020-07-31 06:35:22",
    "post_content": "As the Wall goes up, illegal crossings go down. This past week we built over 10 miles of Wall at our Southern Border. We now have 256 miles of NEW Wall and we are on track to have 300 miles completed by the end of August!",
    "post_content_translated": "随着隔离墙的上升, 非法越野越野越野. 过去的一周, 我们在我们的南部边界建了10英里的墙. 我们现在有了256英里的新墙, 我们走上了8月底完成300英里的轨道!  · ",
    "language": "英语",
    "comments_count": "9027",
    "share_count": "7389",
    "is_transmitted": null,
    "like_count": "10万赞",
    "site_name": "facebook",
    "imgs": [
        {
            "img_alt": "图片中可能有：天空、云、户外和大自然",
            "img_src": "https://scontent-hkt1-1.xx.fbcdn.net/v/t1.0-0/p235x350/115772180_10165181176170725_133670527837873105_o.jpg?_nc_cat=1&_nc_sid=110474&_nc_ohc=mz6NRrx82uwAX-3D26U&_nc_ht=scontent-hkt1-1.xx&_nc_tp=6&oh=83b25494bf87acd6a6ae3e45e441a4d7&oe=5F4B0279"
        }
    ],
    "video_url": null,
    "links": "https://xxxx",
    "comments": [
        {
            "at_name": "joanirayl",
            "user_name": "Joani Rayl",
            "user_id": "752333961",
            "comment_id": "192540585618340",
            "comment_datetime": "2020-07-31 06:57:23",
            "comment_text": "Thank you! This is a vital step in reducing human trafficking into our country. Well done to the workers (I am impressed)!"
        },
        {
            "at_name": "yvette.connelly",
            "user_name": "Yvette Connelly",
            "user_id": "1031919893",
            "comment_id": "2372609649550624",
            "comment_datetime": "2020-07-31 07:31:01",
            "comment_text": "So thankful for you President Trump.  We see what the Dems have put you through since day #1.   Thankfully you’re a strong man who lives by his convictions of what’s right and wrong. You’re doing for our country what many former presidents failed to do. May God bless you and your family."
        },
        ...
    ]
}

```
### [返回目录](#目录)


### 任务执行详情
> 百分点请求电科
`/api/snp/detail/<snp_id>`

**请求方式：**
- GET

**Header：**

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|token |  是  |    string   |    用来作请求验证  |

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|je_id|  是  |string |执行表的id,url参数|


返回结果示例:
```json
{
    "project_id": 1,
    "job_execution_id": 666,
    "job_instance_id": 93,
    "spider_name": "facebook",
    "keywords": "{\"at_name\": \"DonaldTrump\", \"remark_name\": \"特朗普\"}",
    "service_job_execution_id": "73f6bf4cd82011e9bf5b00163e00cc7b",
    "date_created": "2019-09-16 00:00:00",
    "start_time": "2019-09-16 00:00:00",
    "end_time": "2019-09-16 09:23:00",
    "running_status": 2,
    "running_on": "http://172.31.49.57:6800",
    "user_name": "admin",
    "spider_rows_count": 0,
    "job_instance": {
        "id": 93,
        "date_created": "2019-09-16",
        "job_instance_id": 93,
        "user_name": "admin",
        "job_name": "Trump",
        "data_type": "社交网络数据",
        "spider_type": "社交网络采集",
        "keywords": "[{\"at_name\":\"DonaldTrump\",\"remark_name\":\"特朗普\"}]",
        "project_id": 1,
        "spider_names": "facebook",
        "start_date": "1999-01-01",
        "end_date": "2222-02-02",
        "how_often": "每天采集",
        "sleep_days": null,
        "frequency": 1,
        "excute_hours": "[\"04:00\"]",
        "resolution": "1080p",
        "else_prefer": "最高分辨率",
        "video_all_info": "[\"1-40\"]",
        "info_except_video": "[]",
        "upload_time_type": "任务运行周期内最新",
        "upload_time_start_date": "2019-09-15",
        "upload_time_end_date": "2222-02-02",
        "run_type": "长期",
        "spider_arguments": "daemon=http://172.31.49.57:6800",
        "enabled": -1,
        "user_id": 1,
        "pri": "紧急",
        "is_deleted": 0,
        "priority": 0,
        "tags": null
    }
}

```
### [返回目录](#目录)


### 新增社交网络帖子的评论
> 百分点请求电科

`/api/snpc/add`

**请求方式：**
- POST

**Header：**

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|token |  是  |    string   |    用来作请求验证  |

**参数：** 

|参数名|必选|类型|说明|
|:----    |:---|:----- |-----   |
|je_id|  是  |string | 爬虫任务的唯一标识 |
|spider_timeTS|  是  |string或int |采集时间戳，精确到秒|
|site_name|  是  |string |来源网站,`twitter` 或者`facebook`|
|post_id|  是  |string或int |帖子在来源网站的id|
|at_name|  是  |string |评论者@账号名称|
|user_name|  是  |string |评论者昵称|
|user_id|  是  |string |评论者在对应网站的id|
|comment_id|  是  |string |评论id|
|comment_timeTS|  是  |string或int |评论发布的时间戳，精确到秒|
|text|  是  |string |评论内容|

Form Data示例:
```json

{
    "je_id": "dev_123",  // 爬虫任务的唯一标识
    "spider_timeTS":1603268305,
    "site_name": "twitter",
    "post_id": "1318823569988612096",
    "at_name": "realDonaldTrump",
    "user_name": "Donald J. Trump",
    "user_id": "1339835893",
    "comment_id": "1318823569988612096",
    "comment_timeTS": 1603264305,
    "text": "Why isn’t Biden corruption trending number one on Twitter? Biggest world story, and nowhere to be found. There is no”trend”, only negative stories that Twitter wants to put up. Disgraceful! Section 230",
}
```

### [返回目录](#目录)