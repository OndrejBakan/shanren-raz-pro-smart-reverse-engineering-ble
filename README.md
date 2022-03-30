
# Reverse engineering Shanren Raz Pro Smart BLE protocol

UUID for writing: 6E400002-B5A3-F393-E0A9-E50E24DCCA9E

UUID for reading: 6E400003-B5A3-F393-E0A9-E50E24DCCA9E

## Known BLE protocol

### Message structure
```
0x31 00 00 [00 08 00 01 02 03 04 05 06 0A]
  \   \  \   \____________________________ payload
   \   \  \_______________________________ packet count & direction (00 = command from app to light, > 00 = number of incoming packets from light)
    \   \_________________________________ mode
     \____________________________________ header
```

### All settings at once
Request:
```
0x31 0B 00
```

Reply:
```
0x31 0b 01 ff0000 00 08 01 03 00 02 04 05 06 0a
      \  \    \    \  \  \__\__\__\__\__\__\__\_ lighting modes 1 to 8 
       \  \    \    \  \________________________ number of lighting modes
        \  \    \    \__________________________ active lighting mode
         \  \    \______________________________ current color
          \  \__________________________________ packet order
           \____________________________________ mode

0x31 0b 02 01 09 ffff 0078 00
      \  \  \  \   \    \   \___________________ light intensity
       \  \  \  \   \    \______________________ sleep mode time 
        \  \  \  \   \__________________________ team-up settings
         \  \  \  \_____________________________ scenario (08 + 01 = light sense, 02 = breaking light, 04 = bumping alert)
          \  \  \_______________________________ scenario (00 = MTB, 01 = road bike, 02 = helmet, 03 = front warning light)
           \  \_________________________________ packet order
            \___________________________________ mode
```

### Color
```
0x31 01 01 FF0000
```

### Light modes
```
0x31 02 01 00 08 00 01 02 03 04 05 06 0A
      \  \  \  \  \__\__\__\__\__\__\__\_ lighting modes 1 to 8
       \  \  \  \________________________ length of available modes
        \  \  \__________________________ active mode
         \  \____________________________ packet count
          \______________________________ mode
```

### Scenario
```
0x31 03 01 00
      \  \  \
       \  \  \  
        \  \  \_________________________ selected mode (00 = MTB, 01 = road bike, 02 = helmet, 03 = front warning light)
         \  \___________________________ packet count
          \_____________________________ mode
```

```
0x31 04 01 00
      \  \  \
       \  \  \  
        \  \  \_________________________ selected mode (01 = light sense, 02 = breaking light, 04 = bumping alert, bitmasked)
         \  \___________________________ packet count
          \_____________________________ mode
```

### Team-Up
```
0x31 05 01 FFFF
      \  \   \
       \  \   \  
        \  \   \________________________ FFFF for auto team-up, otherwise passcode in hex (ex. 04D2 for 1234)
         \  \___________________________ packet count
          \_____________________________ mode
```

### Restore Factory Setting
Request:
```
0x31 07 01
```

Reply:
```
0x31 07 01 00
```

### Sleep timeout
```
0x31 09 01 0000
      \  \   \
       \  \   \  
        \  \   \________________________ timeout in seconds, hex (ex. 10 min = 0258)
         \  \___________________________ packet count
          \_____________________________ mode
```

### Light intensity
```
0x31 0A 01 00
      \  \  \
       \  \  \  
        \  \  \_________________________ intensity (00 = low, 01 = medium, 02 = high)
         \  \___________________________ packet count
          \_____________________________ mode
```
