---
tile: "Raspberry pi 4 settings"
date: 2021-05-18
categories: armv7 arm64 rpi rpi4 dhcp
typora-root-url: ../
---



## DHCP auto connnect

/etc/wpa_supplicant/wpa_supplicant.conf
network={
        ssid="soda5"
        psk="99999990"
        proto=RSN
        key_mgmt=WP-PSK
        prewise=CCMP
        auth_alg=OPEN
}

/etc/network/interfaces

auto wlan0

allow-hotplug wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
iface default inet dhcp





![](../assets/images/123.png)



## Banner



서버 접속 배너설정

vi /etc/sshd/sshd_config

- Banner /etc/issue.net
- Banner /etc/issue



서버 접속 배너

vi /etc/issue.net



/etc/motd : 로그인 성공시

/etc/issue.net : 원격접속 시도 시

/etc/issue : 콘솔 접속시







로그인 성공 시 출력되는 파일

vi /etc/motd



## 시작 프로그램

sudo vi /etc/rc.local



exit 0 이전에 작성

&옵션을 주면 백그라운드에서 실행