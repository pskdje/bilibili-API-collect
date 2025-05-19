# 直播回放

## 获取直播回放列表

> https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/AnchorGetReplayList

*请求方法: GET*

认证方式: Cookie (SESSDATA)

只能获取自己14天的回放，详细信息请查看[对应页面](https://link.bilibili.com/#/my-room/live-record)

**url参数：**

| 参数名 | 类型 | 内容 | 必要性 | 备注 |
| ----- | --- | ---- | ----- | --- |
| page | num | 页码 | 非必要 | 默认第1页 |
| page_size | num | 每页内容数量 | 非必要 | 默认30项，最大30项 |

**json回复：**

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| code | num | 返回值 | 0：成功<br />-101：未登录 |
| message | str | 提示信息 | 成功时为`"0"` |
| ttl | num | `1` |  |
| data | obj | 信息本体 |  |

`data` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| replay_info | arr | 回放信息列表 | 无结果时为`null` |
| pagination | obj | 分页信息 |  |
| archive_flag | bool | (?) | 作用尚不明确 |
| can\_edit | num | (?) | 作用尚不明确 |

`data.replay_info` 数组中的对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| replay_id | num | 直播回放id |  |
| live_info | obj | 直播信息 |  |
| video_info | obj | 回放视频信息 |  |
| alarm_info | obj | 警报信息 |  |
| room_id | num | 直播间id |  |
| live_key | str | 标记直播场次的key |  |
| start_time | num | 直播开始秒时间戳 | 调用开始直播接口的时间 |
| end_time | num | 直播结束秒时间戳 | 调用结束直播接口的时间 |

`data.replay_info[i].live_info` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| title | str | 直播标题 | 直播结束时的标题 |
| cover | str | 直播封面 |  |
| live_time | num | 直播时间 | 同`data.replay_info[i].start_time` |
| live_type | num | 直播类型? | 作用尚不明确 |

`data.replay_info[i].video_info` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| replay_status | num | 回放状态 | 作用尚不明确 |
| estimated_time | str | 直播回放合成结束时间 | 未合成时为`"1970-01-01 08:00:00"` |
| duration | num | 直播时长 | 单位秒 |
| download_url | str | 下载链接片段 | 建议通过[请求整场直播回放下载链接](#请求整场直播回放下载链接)来获取下载链接 |
| alert_code | num | 快速检查警告代码 | 整场直播回放合成失败时不存在 |
| alert_message | str | 快速检查警告信息 | 整场直播回放合成失败时不存在 |

`data.replay_info[i].alarm_info` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| code | num | 回放合成警报代码 |  |
| message | str | 回放合成错误信息 |  |
| cur_time | num | 当前时间戳 | Unix秒时间戳 |
| is_ban_publish | bool | 是否禁止发布? |  |

`data.pagination` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| page | num | 请求的页码 |  |
| page_size | num | 内容数量 |  |
| total | num | 总计内容数量 |  |

**示例：**

获取自己直播回放列表的第1页，每页2项

```shell
curl 'https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/AnchorGetReplayList?page=1&page_size=2' \
  -b 'SESSDATA=xxx'
```

<details>
<summary>查看响应示例：</summary>

```json
{
  "code": 0,
  "message": "0",
  "ttl": 1,
  "data": {
    "replay_info": [
      {
        "replay_id": 10707737,
        "live_info": {
          "title": "摆",
          "cover": "https://i0.hdslb.com/bfs/live/59fc254c1f51a962dbf69ae85e4920f2f6fb8dcd.png",
          "live_time": 1747509268,
          "live_type": 1
        },
        "video_info": {
          "replay_status": 2,
          "estimated_time": "1970-01-01 08:00:00",
          "duration": 1820,
          "alert_code": 2,
          "alert_message": "录像时长远小于开播时长，请关注直播时网络状况"
        },
        "alarm_info": {
          "code": 2,
          "message": "录像生成失败，请稍后再试",
          "cur_time": 1747557808,
          "is_ban_publish": false
        },
        "room_id": 18992371,
        "live_key": "609043243693510451",
        "start_time": 1747509268,
        "end_time": 1747511088
      },
      {
        "replay_id": 10707664,
        "live_info": {
          "title": "摆",
          "cover": "https://i0.hdslb.com/bfs/live/59fc254c1f51a962dbf69ae85e4920f2f6fb8dcd.png",
          "live_time": 1747508293,
          "live_type": 1
        },
        "video_info": {
          "replay_status": 2,
          "estimated_time": "1970-01-01 08:00:00",
          "duration": 206,
          "alert_code": 2,
          "alert_message": "录像时长远小于开播时长，请关注直播时网络状况"
        },
        "alarm_info": {
          "code": 2,
          "message": "录像生成失败，请稍后再试",
          "cur_time": 1747557808,
          "is_ban_publish": false
        },
        "room_id": 18992371,
        "live_key": "609041817764368179",
        "start_time": 1747508293,
        "end_time": 1747508499
      }
    ],
    "pagination": {
      "page": 1,
      "page_size": 2,
      "total": 29
    },
    "archive_flag": false,
    "can_edit": 1
  }
}
```

</details>

## 获取已发布片段的信息

> https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/AnchorGetVideoSliceList

*请求方式: GET*

认证方式: Cookie (SESSDATA)

**url参数：**

| 参数名 | 类型 | 内容 | 必要性 | 备注 |
| ----- | --- | ---- | ----- | --- |
| page | num | 页码 | 非必要 | 默认第1页 |
| page_size | num | 每页内容数量 | 非必要 | 默认20项，最大20项 |

**json回复：**

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| code | num | 返回值 | 0：成功<br />-101：未登录 |
| message | str | 提示信息 | 成功时为`"0"` |
| ttl | num | `1` |  |
| data | obj | 信息本体 |  |

`data` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| list | arr | 切片信息 |  |
| page | num | 请求的页码 |  |
| page_size | num | 内容数量 |  |
| total | num | 总计内容数量 |  |

`data.list` 数组中的对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| silce_id | num | 切片id？ |  |
| av_title | str | 切片标题 |  |
| av_cover | str | 切片封面 |  |
| av_status | num | 切片状态 | 1：发布中<br />2：已投稿<br />3：投稿失败 |
| ctime | str | 切片创建时间 |  |
| start_tm | str | 切片开始时间 |  |
| end_tm | str | 切片结束时间 |  |
| av_duration | num | 切片时长 | 状态为2时存在 |
| failed_reason | str | 失败原因 | 状态为3时存在，2024-09-01前发布失败的切片可能不存在 |
| live_type | num | (?) | 作用尚不明确 |

**示例：**

获取自己第1页的已发布片段信息，每页3项

```shell
curl 'https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/AnchorGetVideoSliceList?page=1&page_size=3' \
  -b 'SESSDATA=xxx'
```

<details>
<summary>查看响应示例：</summary>

```json
{
  "code": 0,
  "message": "0",
  "ttl": 1,
  "data": {
    "list": [
      {
        "slice_id": 882357,
        "av_title": "2025051720 error",
        "av_cover": "https://i0.hdslb.com/bfs/live/59fc254c1f51a962dbf69ae85e4920f2f6fb8dcd.png",
        "av_status": 1,
        "ctime": "2025-05-18 18:13:13",
        "start_tm": "2025-05-17 21:07:04",
        "end_tm": "2025-05-17 21:16:00",
        "live_type": 1
      },
      {
        "slice_id": 879189,
        "av_title": "2025051721 zzz 0",
        "av_cover": "https://i0.hdslb.com/bfs/live/59fc254c1f51a962dbf69ae85e4920f2f6fb8dcd.png",
        "av_status": 3,
        "ctime": "2025-05-18 00:32:52",
        "start_tm": "2025-05-17 21:07:34",
        "end_tm": "2025-05-17 23:02:03",
        "failed_reason": "duration_false",
        "live_type": 1
      },
      {
        "slice_id": 876259,
        "av_title": "202505171449",
        "av_cover": "https://i0.hdslb.com/bfs/live/59fc254c1f51a962dbf69ae85e4920f2f6fb8dcd.png",
        "av_status": 2,
        "avid": 114521830065531,
        "ctime": "2025-05-17 14:49:18",
        "start_tm": "2025-05-17 14:19:36",
        "end_tm": "2025-05-17 14:23:48",
        "av_duration": 341,
        "live_type": 1
      }
    ],
    "page": 1,
    "page_size": 3,
    "total": 347
  }
}
```

</details>

## 请求整场直播回放下载链接

> https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/AnchorVideoDownload

*请求方法: POST*

认证方式: Cookie (SESSDATA)

鉴权方式: Cookie中`bili_jct`的值正确并与`csrf`相同

未生成整场直播回放时将进行生成。

**正文参数（ application/x-www-form-urlencoded ）：**

| 参数名 | 类型 | 内容 | 必要性 | 备注 |
| ----- | --- | ---- | ----- | --- |
| record_id | num | 直播回放id | 必要（可选） | `record_id`和`live_key`必选其一 |
| live_key | str | 标记直播场次的key | 必要（可选） | `record_id`和`live_key`必选其一 |
| csrf_token | str | CSRF Token（位于cookie） | 非必要 |  |
| csrf | str | CSRF Token（位于cookie） | 必要 |  |

**json回复：**

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| code | num | 返回值 | -101：未登录<br />-111：csrf校验失败<br />0：成功<br />100：非法参数<br />210：回放id或场次key无效 |
| message | str | 错误信息 |  |
| ttl | num | `1` |  |
| data | obj | 信息本体 |  |

`data` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| record | obj | 回放状态 |  |
| download_url | str | 回放下载链接 | 完成时存在 |
| download_url_list | arr | 回放下载链接列表 | 完成时存在 |

`data.record` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| uid | num | 用户mid |  |
| record_id | num | 直播回放id |  |
| status | num | 回放状态 |  |
| estimated_time | num | 预计结束时间 | Unix秒时间戳 |
| current_time | num | 当前时间 | Unix秒时间戳 |
| merge_time | num | 开始合并时间 | Unix秒时间戳 |
| toast | str | 提示信息 | 失败时存在 |

`data.download_url_list` 数组：

| 项 | 类型 | 内容 | 备注 |
| -- | --- | --- | --- |
| 0 | str | 回放下载链接 |  |

**示例：**

请求回放id为`10597910`的下载链接

```shell
curl 'https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/AnchorVideoDownload' \
  --data-urlencode 'record_id=10597910' \
  --data-urlencode 'live_key=607942821532667699' \
  -b 'SESSDATA=xxx;bili_jct=xxx'
```

<details>
<summary>查看响应示例：</summary>

```json
{
  "code": 0,
  "message": "0",
  "ttl": 1,
  "data": {
    "record": {
      "uid": 438160221,
      "record_id": 10597910,
      "status": 30,
      "estimated_time": 1747639543,
      "current_time": 1747639106,
      "merge_time": 1747638665
    },
    "download_url": "https://upos-sz-mirrorali.bilivideo.com/ugcever/n250519sa3hkpirw61hjskuit4d9fdsj.mp4?deadline=1747682306&gen=record2vod&os=upos&trid=da40b42594d5446da29cb0d2b2f25f45&uparams=deadline,gen,os,trid&upsig=c6ac5f218af40b2c120b3f5add2e4d6b&attname=直播回放_2025-05-13_20-49-04.mp4",
    "download_url_list": [
      "https://upos-sz-mirrorali.bilivideo.com/ugcever/n250519sa3hkpirw61hjskuit4d9fdsj.mp4?deadline=1747682306&gen=record2vod&os=upos&trid=da40b42594d5446da29cb0d2b2f25f45&uparams=deadline,gen,os,trid&upsig=c6ac5f218af40b2c120b3f5add2e4d6b&attname=直播回放_2025-05-13_20-49-04.mp4"
    ]
  }
}
```

</details>

## 获取回放的信息

> https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/GetAnchorVideoUidRecordsSubsect

*请求方法: GET*

认证方式: Cookie (SESSDATA)

**url参数：**

| 参数名 | 类型 | 内容 | 必要性 | 备注 |
| ----- | --- | ---- | ----- | --- |
| record_id | num | 直播回放id | 必要 |  |

**json回复：**

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| code | num | 返回值 | -400：参数错误<br />-101：未登录<br />0：成功 |
| message | str | 错误信息 | 成功时为`"0"` |
| ttl | num | `1` |  |
| data | obj | 信息本体 | 失败时不可用 |

`data` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| list | arr | 回放信息列表 |  |

`data.list` 数组中的对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| uid | num | 用户mid |  |
| record_id | num | 直播回放id |  |
| title | str | 直播标题 |  |
| cover | str | 直播封面 |  |
| status | num | 回放状态 |  |
| start\_time | num | 直播开始时间 | Unix秒时间戳 |
| end_time | num | 直播结束时间 | Unix秒时间戳 |

**示例：**

获取回放id为`10707664`的信息

```shell
curl 'https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/GetAnchorVideoUidRecordsSubsect?record_id=10707664' \
  -b 'SESSDATA=xxx'
```

<details>
<summary>查看响应示例：</summary>

```json
{
  "code": 0,
  "message": "0",
  "ttl": 1,
  "data": {
    "list": [
      {
        "uid": 438160221,
        "record_id": 10707664,
        "title": "摆",
        "cover": "https://i0.hdslb.com/bfs/live/59fc254c1f51a962dbf69ae85e4920f2f6fb8dcd.png",
        "status": 2,
        "start_time": 1747508293,
        "end_time": 1747508499
      }
    ]
  }
}
```

</details>

## 轮询回放合成状态

> https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/GetAnchorVideoUidRecord

*请求方法: POST*

认证方式: Cookie (SESSDATA)

鉴权方式: Cookie中`bili_jct`的值正确并与`csrf`相同

**正文参数（ application/x-www-form-urlencoded ）：**

| 参数名 | 类型 | 内容 | 必要性 | 备注 |
| ----- | --- | ---- | ----- | --- |
| records | str | 直播回放id列表 | 必要 | 用`,`分隔 |
| csrf_token | str | CSRF Token（位于cookie） | 非必要 |  |
| csrf | str | CSRF Token（位于cookie） | 必要 |  |

**json回复：**

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| code | num | 返回值 | -101：未登录<br />-400：参数错误<br />0：成功 |
| message | str | 错误信息 | 成功时为`"0"` |
| ttl | num | `1` |  |
| data | obj | 信息本体 |  |

`data` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| list | arr | 查询结果 | 无效的id会被忽略 |

`data.list` 数组中的对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| uid | num | 用户mid |  |
| record_id | num | 直播回放id |  |
| status | num | 回放状态 |  |
| current_time | num | 当前时间戳 | Unix秒时间戳 |
| estimated_time | num | 预计结束时间戳 | 初次[请求回放下载链接](#请求整场直播回放下载链接)后存在 |
| merge_time | num | 合成开始时间戳 | 初次[请求回放下载链接](#请求整场直播回放下载链接)后存在 |

**示例：**

查询各种回放id

```shell
curl 'https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/GetAnchorVideoUidRecord' \
  --data-urlencode 'records=10727160,10597910,10687720,10230000,99999999' \
  --data-urlencode 'csrf=xxx' \
  -b 'SESSDATA=xxx;bili_jct=xxx'
```

<details>
<summary>查看响应示例：</summary>

```json
{
  "code": 0,
  "message": "0",
  "ttl": 1,
  "data": {
    "list": [
      {
        "uid": 91089731,
        "record_id": 10230000,
        "status": 2,
        "current_time": 1747641604
      },
      {
        "uid": 438160221,
        "record_id": 10597910,
        "status": 30,
        "estimated_time": 1747639543,
        "current_time": 1747641604,
        "merge_time": 1747638665
      },
      {
        "uid": 438160221,
        "record_id": 10687720,
        "status": -30,
        "estimated_time": 1747635525,
        "current_time": 1747641604,
        "merge_time": 1747635486,
        "toast": "因直播过程中存在推流质量问题（网络波动或丢包），本场直播回放无法合成"
      },
      {
        "uid": 3493299121817771,
        "record_id": 10727160,
        "status": 2,
        "current_time": 1747641604
      }
    ]
  }
}
```

</details>

## 下载整场直播回放的流程

1. 先[请求整场直播回放下载链接](#请求整场直播回放下载链接)接口，让它开始合成回放；

2. (可选)请求[获取回放的信息](#获取回放的信息)接口，生成合成进度页面；

3. [轮询回放合成状态](#轮询回放合成状态)，当状态变为`30`转到4流程，变为`-30`转到5流程；

4. 再次[请求整场直播回放下载链接](#请求整场直播回放下载链接)，获取下载链接并下载。

5. 请求[获取直播回放列表](#获取直播回放列表)，刷新页面并根据信息提示失败。

## 直播回放剪辑页面

通过此处的链接可以打开web直播回放剪辑页面。

> https://live.bilibili.com/web-cut/quick-publish.html

**url查询参数：**

| 参数名 | 类型 | 内容 | 必要性 | 备注 |
| ----- | --- | ---- | ----- | --- |
| start_time | num | 直播开始时间 | 必要 | 对应[获取直播回放列表](#获取直播回放列表)的`data.replay_info[i].start_time` |
| end_time | num | 直播结束时间 | 必要 | 对应[获取直播回放列表](#获取直播回放列表)的`data.replay_info[i].end_time` |
| live_key | str | 标记直播场次的key | 必要 | 对应[获取直播回放列表](#获取直播回放列表)的`data.replay_info[i].live_key` |
| cover | str | 封面 | 非必要 | 可以自定义封面，或者在[获取直播回放列表](#获取直播回放列表)使用直播封面 |

**示例链接：** https://live.bilibili.com/web-cut/quick-publish.html?start_time=1747508293&end_time=1747508499&live_key=609041817764368179&cover=https%3A%2F%2Fi0.hdslb.com%2Fbfs%2Flive%2F59fc254c1f51a962dbf69ae85e4920f2f6fb8dcd.png
