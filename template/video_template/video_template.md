---
<%* 
function transform_page_to_chapter_str(page, view_url) {
  return "[" + page.part + "](" + view_url + "?p=" + String(page.page) + ")";
}

let video_url = await tp.system.prompt("Enter the url of video");
const video_url_parts = video_url.split('/');
let lastPart = video_url_parts[4];
lastPart = lastPart.split('?')[0];

video_url = "";
for (let idx = 0; idx < 4; idx++) {
  video_url += video_url_parts[idx] + "/";
}
video_url += lastPart;

let url = 'https://api.bilibili.com/x/web-interface/view?bvid=';
url += lastPart;
let res = await request({url: url,method: "GET"});
let json_res = JSON.parse(res);
res = json_res.data;
let title = json_res.data.title;
let author = json_res.data.owner.name;
let chapters = "";
for (let idx in json_res.data.pages) {
  chapters += transform_page_to_chapter_str(json_res.data.pages[idx], video_url) + "\n";
}
let save_folder_path = "/learn/video/" // 保存路径位置
// await tp.file.move(save_folder_path + title) // 如果不需要多一层文件夹可以使用这个
await tp.file.move(save_folder_path + title + "/" + title)
await tp.file.rename(title);
// code author: orzzzzzz
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


## Chapters
<% chapters %>

## Reference