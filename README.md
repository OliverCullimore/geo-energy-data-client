[![License](https://img.shields.io/github/license/OliverCullimore/geo-energy-data-client?style=for-the-badge)](https://github.com/OliverCullimore/geo-energy-data-client)

# geo Energy Data Client

Client for the geotogether.com API written in Go.

### Install
```
go get github.com/olivercullimore/geo-energy-data-client
```

### Example
main.go

```
package main

import (
	"encoding/json"
	"github.com/olivercullimore/geo-energy-data-client"
	"log"
)

func main() {
	var geoUser = "" // Enter your geo Home app username to use
	var geoPass = "" // Enter your geo Home app password to use

	// Get an access token
	accessToken, err := geo.GetAccessToken(geoUser, geoPass)
	if err != nil {
		log.Fatal(err)
	}

	// Get device data to get the system ID
	deviceData, err := geo.GetDeviceData(accessToken)
	if err != nil {
		log.Fatal(err)
	}
	log.Println(deviceData)
	// Set system ID
	geoSystemID := deviceData.SystemDetails[0].SystemID

	// Get live meter data
	liveData, err := geo.GetLiveMeterData(accessToken, geoSystemID)
	if err != nil {
		log.Fatal(err)
	}
	liveDataParsed, err := json.MarshalIndent(liveData, "", "  ")
	if err != nil {
		log.Fatal(err)
	}
	log.Printf("%s: \n%s\n", "Live meter data", string(liveDataParsed))

	// Get periodic meter data
	periodicData, err := geo.GetPeriodicMeterData(accessToken, geoSystemID)
	if err != nil {
		log.Fatal(err)
	}
	periodicDataParsed, err := json.MarshalIndent(periodicData, "", "  ")
	if err != nil {
		log.Fatal(err)
	}
	log.Printf("%s: \n%s\n", "Periodic meter data", string(periodicDataParsed))

}
```