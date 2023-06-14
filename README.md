# tg-vnstat-monitor-bot
O script relata o uso do tráfego no mês atual (da ferramenta vnstat)

# Requisitos
* python3
* vnstat 

#Como parece?

![screenshot](readme/screenshot2.png)

# Instalação

# vnstat

```sh
# instalar vnstat
sudo apt-get install vnstat

# modifique a configuração ao seu gosto (a configuração padrão está ok)
sudo nano /etc/vnstat.conf 

# Começar serviço
sudo systemctl enable vnstat.service
sudo systemctl start vnstat.service

# verifique o status do serviço
sudo systemctl status vnstat.service

# verifique se o vnstat está funcionando
vnstat
```

# o script

1) Clonar repositório
```sh
cd ~
mkdir vnstat-watchdog # change it if you want
cd vnstat-watchdog

git clone https://github.com/WdsUtilidades/tg-vnstat-monitor-bot
cd tg-vnstat-monitor-bot
pip3 install -r requirements.txt
```

2) Faça um script de execução como este em `~/vnstat-watchdog/report.sh`

```sh
#!/bin/bash

set -euo pipefail

export LIMIT_GIB=1024 # 10 TB é o limite não faturado padrão na Oracle
export INTERFACE=ens3
export TOKEN="your telegram bot token"
export TG_CHAT_ID="your telegram chat id"

set -x

cd tg-vnstat-monitor-bot
# git pull # auto-update repo

python3 main.py
```

3) Adicionar ao cron

```sh
# todos os dias às 13:00. ver: https://crontab.guru/#0_13_*_*_*
# assumindo que /home/user/ é seu $HOME.
*/0 13 * * * cd /home/user/vnstat-watchdog && ./report.sh > /dev/null
```
