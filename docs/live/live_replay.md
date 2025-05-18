# 直播回放

## 获取自己的直播回放列表

> https://api.live.bilibili.com/xlive/app-blink/v1/anchorVideo/AnchorGetReplayList

*请求方法: GET*

认证方式: Cookie (SESSDATA)

只能获取14天的回放，详细信息请查看[对应页面](https://link.bilibili.com/#/my-room/live-record)

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
| replay_id | num | 回放id？ |  |
| live_info | obj | 直播信息 |  |
| video_info | obj | 回放视频信息 |  |
| alarm_info | obj | 警报信息? |  |
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
| replay_status | num | (?) | 作用尚不明确 |
| estimated_time | str | (?) | `1970-01-01 08:00:00` |
| duration | num | 直播时长 | 单位秒 |
| alert_code | num | (?) | 作用尚不明确 |
| alert_message | str | (?) | 作用尚不明确 |

`data.replay_info[i].alarm_info` 对象：

| 字段 | 类型 | 内容 | 备注 |
| --- | --- | --- | --- |
| code | num | (?) |  |
| message | str | (?) |  |
| cur_time | num | 当前时间戳 | Unix秒时间戳 |

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
