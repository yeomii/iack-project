## compile

openwrt 루트 디렉토리에서 다음 명령어 실행

```
$ make
```

## test

빌드된 ath9k 드라이버를 AP로 전송한다.

```
$ sshpass -p "network" scp openwrt/build_dir/target-mips_34kc_uClibc-0.9.33.2/linux-ar71xx_generic/compat-wireless-2014-05-22/ipkg-ar71xx/kmod-ath9k-common/lib/modules/3.10.49/ath9k* root@192.168.1.1:/tmp 
$ sshpass -p "network" scp openwrt/build_dir/target-mips_34kc_uClibc-0.9.33.2/linux-ar71xx_generic/compat-wireless-2014-05-22/ipkg-ar71xx/kmod-ath9k/lib/modules/3.10.49/ath9k* root@192.168.1.1:/tmp
```

AP에서 새로 빌드한 모듈로 바꾼다.

```
$ cp /tmp/ath9k* ./
$ rmmod ath9k
$ rmmod ath9k_common
$ rmmod ath9k_hw

$ modprobe mac80211
$ modprobe ath
$ insmod ath9k_hw.ko
$ insmod ath9k_common.ko
$ insmod ath9k.ko debug=0x00000080
$ /etc/init.d/network restart
```

스테이션에서 AP의 wireless로 연결한 뒤 아마존 서버와 통신을 수행한다. 그리고 AP에 ssh로 연결한 후 tcpdump를 수행하면 fake ACK를 확인할 수 있다.
