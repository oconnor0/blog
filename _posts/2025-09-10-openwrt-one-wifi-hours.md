---
layout: post
title: "Configuring OpenWrt One's Active Wifi Hours"
categories: tech
---

To configure the wifi on an [OpenWrt One](https://openwrt.org/toh/openwrt/one) to turn off at 10PM and on at 6AM, modify the `crontab` to be:

```crontab
0 22 * * * uci set wireless.radio0.disabled=1; uci set wireless.radio1.disabled=1; wifi
0 06 * * * uci set wireless.radio0.disabled=0; uci set wireless.radio1.disabled=0; wifi
```

This can be done by SSHing into the router as root.
I configured my One to be on IP address 192.168.1.2 since I have a router/firewall on 192.168.1.1.

```shell
ssh root@192.168.1.2
```

And then run the following to set up the crontab and to start cron:

```shell
echo "0 22 * * * uci set wireless.radio0.disabled=1; uci set wireless.radio1.disabled=1; wifi" >> /etc/crontab
echo "0 06 * * * uci set wireless.radio0.disabled=0; uci set wireless.radio1.disabled=0; wifi" >> /etc/crontab
/etc/init.d/cron enable
/etc/init.d/cron start
/etc/init.d/cron restart
```

[Copied from instructions on the OpenWrt forum](https://forum.openwrt.org/t/scheduling-on-off-wifi/3385/7)
