# Wikidata 作業

## 論文閱讀

>[ARL White Paper on Wikidata: Opportunities and Recommendations](https://www.arl.org/wp-content/uploads/2019/04/2019.04.18-ARL-white-paper-on-Wikidata.pdf)

### Wikidata 為研究圖書館帶來的機會

- 豐富館藏，提升數據可見度和影響力
- 促進學術交流與相互引用
- 促進多元平等，紀錄弱勢群體和邊緣化知識

### Wikibase 簡介

Wikibase 是 Wikidata 的底層軟體，可獨立部署建構數據儲存庫

## Wikidata 實作
在 Wikidata 中搜尋臺灣棒球運動員

### 使用搜尋指令

```
SELECT DISTINCT ?item ?itemLabel WHERE {
  SERVICE wikibase:label { bd:serviceParam wikibase:language "[AUTO_LANGUAGE]". }
  {
    SELECT DISTINCT ?item WHERE {
      ?item p:P106 ?statement0.
      ?statement0 (ps:P106/(wdt:P279*)) wd:Q10871364.
      ?item p:P27 ?statement1.
      ?statement1 (ps:P27/(wdt:P279*)) wd:Q865.
    }
  }
}
```

- 職業：棒球運動員
- 國籍：中華民國

### 遇到的問題

- 有些球員缺少「所屬運動隊」屬性欄位，無法完整搜尋特定球隊完整球員名單
- 有些資料為舊資料，更新不及時

### 結論

有些球員資料不完整，非職業選手狀況更為嚴重，搜尋的時候可能要透過更多屬性去搜尋（例如：畢業學校）。相較於維基百科，有些球員資料更新較為不及時，例如「所屬運動隊」欄位仍為轉隊前的球隊，此問題看起來暫時沒有更有效的方式，只能透過手動修改的方式來解決。