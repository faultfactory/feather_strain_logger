# feather_strain_logger
Adafruit Strain Logger using Board from Brendan Zotto

This is based on the AvrAdcLogger project with modifications to fit this board. 

# Required libraries
This projet leverages the following: 
- [MAX5481](https://github.com/robertfchapman/MAX5481)
- SdFat

# State Chart

```mermaid
flowchart LR
  startup["`Startup <br> StateLED: Fast Blink<br> WriteLED: Off<br> ErrorLED: Off`"]
  serialMode["`SerialMode<br> StateLED: On permanently <br> WriteLED: Off<br> ErrorLED: Off`"]
  standaloneMode["`StandaloneMode<br> StateLED: Slow Blink <br> WriteLED: Off<br> ErrorLED: Off`"]
  logging["`Logging<br> StateLED: Depends on Mode <br> WriteLED: On <br> ErrorLED: Off`"]
  error["`ErrorState<br> StateLED: Depends on Mode <br> WriteLED: Off <br> ErrorLED: On`"]
  startup -- Timer expired with no serial connection -->standaloneMode
  startup -- Serial Connected -->serialMode
  standaloneMode-- Switch On -->logging
  logging -- Switch Off -->standaloneMode
  serialMode-->MenuHome
  MenuHome-- Switch On -->logging
  logging-- Switch Off -->MenuHome
  MenuHome-->OpenBinFile
  OpenBinFile-->MenuHome
  MenuHome-->ConvertToCSV
  ConvertToCSV-->MenuHome
  MenuHome-->ListFiles
  ListFiles-->MenuHome
  MenuHome-->PrintBinaryToSerial
  PrintBinaryToSerial-->MenuHome
  MonitorA0Pin-->MenuHome
  MenuHome-->MonitorA0Pin

  error
```

