---
<%* 
const LESSON_SEASON_KEY = 'ugc_season';

function transform_page_to_chapter_str(page, view_url) {
  return "[" + page.part + "](" + view_url + "?p=" + String(page.page) + ")";
}

function deleteIllegalChars(s) {
  return s.split('').map(c => (c !== '/' && c !== '|') ? c : ' ').join(''); 
}

function transformTitleToChapterStr(title, bvid) {
  return `[${title}](https://www.bilibili.com/video/${bvid})`;
}

function get_chapters_from_ugc_season(respData) {
const episodes = respData[LESSON_SEASON_KEY].sections[0].episodes; const chapterStrList = episodes.map(episode=> transformTitleToChapterStr(episode.title, episode.bvid)); return chapterStrList.join('\n');
}

let title = "";
let author = "";
let source = "";
let url = "";
let chapters = "";

if (true) {
let video_url = await tp.system.prompt("Enter the url of video");
const video_url_parts = video_url.split('/');
let bvid = video_url_parts[4];
bvid = bvid.split('?')[0];
url = "https://www.bilibili.com/video/" + bvid;
let api_url = 'https://api.bilibili.com/x/web-interface/view?bvid=';
api_url += bvid;
let res = await request({url: api_url,method: "GET"});
let json_res = JSON.parse(res);
res = json_res.data;
title = json_res.data.title;
author = json_res.data.owner.name;
chapters = "";
if ('ugc_season' in json_res.data) {
  chapters = get_chapters_from_ugc_season(json_res.data)
} else {
for (let idx in json_res.data.pages) {
  chapters += transform_page_to_chapter_str(json_res.data.pages[idx], url) + "\n";
}
}
source = "bilibili";
}
let save_folder_path = "/learn/video/" // 保存路径位置
title = deleteIllegalChars(title)
// await tp.file.move(save_folder_path + title) // 如果不需要多一层文件夹可以使用这个
await tp.file.move(save_folder_path + title + "/" + title)
await tp.file.rename(title);
// code author: orzzzzzz
-%>
title: <% title %>
author: <% author %>
url: <% url %>
source: <% source %>
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