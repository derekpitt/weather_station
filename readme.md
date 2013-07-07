# Weather: a package for getting data from the Vantage Vue Weather Station in Go

## Setup

The console doesn't come with a data cable, so you can get one here:
http://www.amazon.com/gp/product/B001AMKC14/ref=oh_details_o03_s00_i00?ie=UTF8&psc=1

If you are running some linux, you can plug it in, run dmesg and somewhere it should tell you what the tty is. For me it was: /dev/ttyUSB0

For Mac, I had to get this driver:
http://www.silabs.com/products/mcu/Pages/USBtoUARTBridgeVCPDrivers.aspx

Then when I plug it in, was able to use /dev/tty.SLAB_USBtoUART

## Other things

Even though the communication manual (http://www.davisnet.com/support/weather/download/VantageSerialProtocolDocs_v261.pdf) states that it needs a wake up procedure, I never had to do this to get data back. So I have not included it.

## Example

```go
package main

import (
  "fmt"
  "github.com/derekpitt/weather_station"
  "time"
)


func main() {
  ws, err := weather_station.New("/dev/tty.SLAB_USBtoUART")
  if err != nil {
    panic("dang..")
  }

  // we take out a lock each time we call GetSample. You can call from as many
  // goroutines as you want!
  go func() {
    sample, _ := ws.GetSample()
    fmt.Printf("%#v", sample)
  }()

  go func() {
    sample, _ := ws.GetSample()
    fmt.Printf("%#v", sample)
  }()

  time.Sleep(5 * time.Second)
}
```
