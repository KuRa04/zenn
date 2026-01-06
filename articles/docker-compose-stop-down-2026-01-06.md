---
title: "docker compose stop / downの違いについて"
emoji: "🌍️"
type: "tech"
topics: ["Docker"]
published: true
---
## downとstopの違い
- `docker-compose down`はコンテナを停止し、 up で作成したコンテナ、ネットワーク、ボリューム、イメージを削除。
- `docker-compose stop`は稼働中のコンテナを停止するが、削除しない。 `docker-compose start`で、再起動可能。

## 参考
https://docs.docker.jp/compose/reference/down.html
https://docs.docker.jp/compose/reference/stop.html