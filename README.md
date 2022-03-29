
# Reverse engineering Shanren Raz Pro Smart BLE protocol

UUID for writing: 6E400002-B5A3-F393-E0A9-E50E24DCCA9E
UUID for reading: 6E400003-B5A3-F393-E0A9-E50E24DCCA9E

## Known BLE protocol

### Message structure
```
0x31 00 00 [00 08 00 01 02 03 04 05 06 0A]
  \   \  \   \____________________________ payload
   \   \  \_______________________________ direction or separator
    \   \_________________________________ mode
     \____________________________________ header
```

### Color
```
0x31 01 01 FF0000
```

### Light modes
```
0x31 02 01 00 08 00 01 02 03 04 05 06 0A
      \  \  \  \  \_____________________ 1 to 8 available modes
       \  \  \  \_______________________ length of available modes
        \  \  \_________________________ active mode
         \  \___________________________ direction or separator
          \_____________________________ mode
```

### Scenario
```
0x31 03 01 00
      \  \  \
       \  \  \  
        \  \  \_________________________ selected mode (00 = MTB, 01 = road bike, 02 = helmet, 03 = front warning light)
         \  \___________________________ direction or separator
          \_____________________________ mode
```

```
0x31 04 01 00
      \  \  \
       \  \  \  
        \  \  \_________________________ selected mode (01 = light sense, 02 = breaking light, 04 = bumping alert, bitmasked)
         \  \___________________________ direction or separator
          \_____________________________ mode
```

### Team-Up
```
0x31 05 01 FFFF
      \  \   \
       \  \   \  
        \  \   \________________________ FFFF for auto team-up, otherwise passcode in hex (ex. 04D2 for 1234)
         \  \___________________________ direction or separator
          \_____________________________ mode
```
### Sleep timeout
```
0x31 09 01 0000
      \  \   \
       \  \   \  
        \  \   \________________________ timeout in seconds, hex (ex. 10 min = 0258)
         \  \___________________________ direction or separator
          \_____________________________ mode
```

### Light intensity
```
0x31 0A 01 00
      \  \  \
       \  \  \  
        \  \  \_________________________ intensity (00 = low, 01 = medium, 02 = high)
         \  \___________________________ direction or separator
          \_____________________________ mode
```
