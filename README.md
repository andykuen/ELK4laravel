# ELK for laravel 教學等相關說明文件

## 範例會使用的相關程式和環境

* macOS Mojave
* docker
* ubuntu 16.04
* php 7.2
* composer
* laravel 5.7.x
* Elasticsearch
* Logstash
* Kibana 
* filebeat

## 預計章節

* 簡介
    * 什麼是 E.L.K.
    * 為什麼會用他們
* 執行在 docker 上
    * 好用的 portainer 管理套件
    * IP 衝突解決方法
    * 設定 docker.yml
    * 設定 Laravel
    * 設定 Logstash
    * 設定 Kibana
* 成果展現
* 補充: Logstash 轉換成 filebeat
    * 什麼是 filebeat
    * filebeat 設定
    * Logstash 修正
* 其他還沒被歸類的資料