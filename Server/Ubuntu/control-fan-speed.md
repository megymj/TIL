# Fan speed 조절



```bash
# 팬 속도 수동조절 허가
$ ipmitool raw 0x30 0x30 0x01 0x00

# 팬 속도 조절
$ ipmitool raw 0x30 0x30 0x02 0xff [speed in hex]

# 팬 상태(속도) 확인
$ ipmitool sdr type fan

# +tip) MacOS에서는 brew command를 통해 간편하게 ipmitool 설치 가능
brew install ipmitool
```

