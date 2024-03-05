## 
### 1. 使用Crontab定时执行任务
```bash
*/10 * * * * cd /var/www/LRM && python3 manage.py shell < /var/www/LRM/import_keyword_and_update_doc_html.py >> /var/www/LRM/import_keyword_and_update_doc_html.log
```

