# 与百分点对接的接口
# 社交网络接口

## 目录
### 1. [新增账号的验证](# 1. 新增账号的验证)
### 2. [新增社交网络爬虫任务](# 2. 新增社交网络爬虫任务)
### 3. [更新爬虫任务执行数据](# 3. 更新爬虫任务执行数据)
### 4. [新增社交网络帖子](# 4. 新增社交网络帖子)
### 5. [新增社交网络人物信息](# 5. 新增社交网络人物信息)

---
### 1. 新增账号的验证
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


### 2. 新增社交网络爬虫任务
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


### 3. 更新爬虫任务执行数据
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
|task_id|  是  |string或int |电科系统中的任务id|
|je_id|  是  |string或int  |电科系统中的执行id|
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
    "je_id": 123401  // 电科系统中的执行id
}
```


### 4. 新增社交网络帖子
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
|je_id|  是  |string或int |爬虫任务id|
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
    "je_id": 1234,  // 爬虫任务id
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
    "post_comments": [  // 评论，取最新的200条
        {
            "at_name": "nancy.miller.944",  // 评论者账号，@后面的字符串
            "user_name": "Nancy Miller",  // 评论者账号，账号在网站的名字
            "user_id": "100000667601679",  // 评论者账号在网站的id
            "comment_id": "10164386821570725", // 该评论在网站的id
            "comment_timeTS": 1586391992,  // 评论发布时间戳
            "comment_text": "I have one suggestion about when it is time to start opening up the economy. One of the first businesses that should be opened up are dental offices. We take very many precautions against infections. We were masks, gloves and we wipe everything down between patients. Also every instrument is sterilized. We should be one business that could open first without worry."  // 评论的内容
        },
        {
            "at_name": "dana.fountain.7",
            "user_name": "Dana Fountain",
            "user_id": "100000033535305",
            "comment_id": "10164386754595725",
            "comment_timeTS": 1586391992,
            "comment_text": "Thank you Mr. President for your strength in leadership. Having a strong leader for our country will cause our country to be back up and running in no time!!! Thank you sir!! You are beyond doubt the greatest President in our history. I feel blessed to be living during your presidency. God Bless you and keep you ❤️"
        }
    .......
    ]
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
    "je_id": 1234,  // 爬虫任务id
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
    "post_comments": [  // 评论，取最新的200条
        {
            "at_name": "nancy.miller.944",  // 评论者账号，@后面的字符串
            "user_name": "Nancy Miller",  // 评论者账号，账号在网站的名字
            "user_id": "100000667601679",  // 评论者账号在网站的id
            "comment_id": "10164386821570725", // 该评论在网站的id
            "comment_timeTS": 1586391992,  // 评论发布时间戳
            "comment_text": "I have one suggestion about when it is time to start opening up the economy. One of the first businesses that should be opened up are dental offices. We take very many precautions against infections. We were masks, gloves and we wipe everything down between patients. Also every instrument is sterilized. We should be one business that could open first without worry."  // 评论的内容
        },
        {
            "at_name": "dana.fountain.7",
            "user_name": "Dana Fountain",
            "user_id": "100000033535305",
            "comment_id": "10164386754595725",
            "comment_timeTS": 1586391992,
            "comment_text": "Thank you Mr. President for your strength in leadership. Having a strong leader for our country will cause our country to be back up and running in no time!!! Thank you sir!! You are beyond doubt the greatest President in our history. I feel blessed to be living during your presidency. God Bless you and keep you ❤️"
        }
    .......
    ]
    "hashtags":["#FarmerstoFamilies"],
   "other_info": null,
    "site_name": "facebook",
    "at_name": "IvankaTrump",
    "user_name": "Ivanka Trump",

}
```


### 5. 新增社交网络人物信息
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
|spider_timeTS|  是  |string或int |采集时间戳，精确到秒|
|website|  是  |string|网站，facebook或twitter|
|at_name|  是  |string |人物在网站的账号，@后面的字符串|
|user_name|  是  |string |人物在网站的名字|
|user_id|  是  |string或int  |账号在该网站的id|
|created_atTS|  是  |string或int |账号注册日期时间戳|
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
    "spider_timeTS": 1586391992,  // 采集时间戳
    "website": "facebook",  // 或twitter
    "at_name": "DonaldTrump",  
    "user_name": "Donald J. Trump",
    "created_atTS": 1386391992,
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