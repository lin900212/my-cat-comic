# My Cat Comic 任務網站

偏鄉國小 AI 營隊使用的靜態任務網站。學生選擇自己的名字後，可以依照步驟完成：

- 無字四格漫畫
- 中文版四格漫畫
- 英文版四格漫畫

老師可以在頁面中的「老師管理」新增學生、上傳角色圖，或匯入 CSV / JSON。

## 使用方式

開啟 `index.html`，或使用 GitHub Pages 部署後的網址。

## 管理學生資料

正式上課時，建議直接更新 `students.json`：

```json
{
  "id": "xiao-an",
  "name": "小安",
  "group": "A 組",
  "role": "Coach",
  "story": "The Test of Courage",
  "image": "images/xiao-an-coach.png",
  "events": [
    "我在訓練中心教小騎士練習。",
    "我看到有小騎士很害怕。",
    "我告訴他們不要放棄。",
    "大家一起重新練習。"
  ]
}
```

角色圖請放在 `images/` 資料夾，並把 `image` 欄位填成對應路徑。

如果瀏覽器已經存過本機資料，可以在網站的「老師管理」按「重新載入網站資料」。
