---
jupytext:
  formats: md:myst
  text_representation:
    extension: .md
    format_name: myst
    format_version: 0.13
    jupytext_version: 1.11.5
kernelspec:
  display_name: Python 3
  language: python
  name: python3
---

# 開源社群專案作業 - Django

>Django 是一個高級的 Python 網路框架，可以快速開發安全和可維護的網站。Django 是免費和開源的，有活躍繁榮的社區、豐富的文檔、以及很多免費和付費的解決方案。
>
>-- mdn web docs

## 相關網址

- [貢獻指引網址](https://docs.djangoproject.com/en/dev/internals/contributing/)
- [提交 Pull Requests 的說明網址](https://docs.djangoproject.com/en/dev/internals/contributing/committing-code/)

## 簡短統整
- 貢獻方式：修復錯誤、添加新功能、改進文件、協助翻譯
- Django 提交指南統整：
	1. 以 pull request 的形式提交請求
	2. 可以使用 Jenkins 或 GitHub actions 測試 pull request
	3. 合併時需確保每個提交符合提交指南，並使用 `git rebase -i` 和 `git commit --amend` 來確保提交內容的品質
	4. 發現錯誤提交時，應遵循指南進行處理