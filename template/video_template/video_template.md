---
<%* 
let video_url = await tp.system.prompt("Enter the url of video");
const video_url_parts = video_url.split('/');
const lastPart = video_url_parts[4];

let url = 'https://api.bilibili.com/x/web-interface/view?bvid=';
url += lastPart;
let res = await request({url: url,method: "GET"});
let json_res = JSON.parse(res);
res = json_res.data;
console.log(res);
let title = json_res.data.title;
let author = json_res.data.owner.name;
let save_folder_path = "/learn/video/" // 保存路径位置
// await tp.file.move(save_folder_path + title) // 如果不需要多一层文件夹可以使用这个
await tp.file.move(save_folder_path + title + "/" + title)
await tp.file.rename(title);
-%>
title: <% title %>
author: <% author %>
url: <% video_url %>
score: 
tags: 
type: "learning/video"
date: <% tp.file.creation_date("YYYY-MM-DD") %>
---
## Summary

## Notes

## Review


## Related
[video](<% video_url %>)

## Reference