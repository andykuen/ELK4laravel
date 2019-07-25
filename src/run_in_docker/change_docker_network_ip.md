# IP 衝突解決方法

Docker 如果需要連線外部服務，有可能為跟其他外部服務打架。

Docker 會先以內部 container 的 IP 為優先考量，因此會有呼叫不到外部服務的問題。

目前解決的方法 把 Docker 使用的 IP 範圍，和外部的不相同。

私有IP範圍： 10.0.0.0/8 172.16.0.0/12 192.168.0.0/16
wiki: https://en.wikipedia.org/wiki/Private_network

```JSON
{
  "default-address-pools" : [
    {
      "base" : "192.168.0.0/16",
      "size" : 24
    }
  ],
}
```
help: https://docs.docker.com/engine/reference/commandline/dockerd/#daemon-configuration-file
