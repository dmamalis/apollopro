# apollopro
An unofficial and untested simple nodejs service to read out Apollo Pro Indoor Rower. Use at your own risk!

## Prerequisites
- Install `nodejs` and `npm` on your machine.
- Have a local or remote InfluxDB running.
- Init a database on your InfluxDB

##### Optional
Have a local or remote Grafana account to visualize your data.

## Info
The serial port is set to 9600-N-1. This app reads the 29B data stream and posts the data to an InfluxDB. Make sure you provide a URL of your InfluxDB and the name your your database.

## Communication
Without really diving into it I found out that in Windows the port is initially congifured at 1200-n-1 7 bits word. Then the windows app sets it to 9600-N-1 and the communication starts. However, when in Linux and probably Mac the device pops up as ttyUSB and you can directly set it to 9600-N-1. Downlink commands end with`\n` while uplink responses end with `\r\n`

##### Detected commands
While sniffing the com port in Windows I found out that the application sends the following commands which I didn't really bother to investigate.

| Command | Description |
|---	|---	|
| `C\n` | ? |
| `T\n` | ? |
| `V\n` | Possibly Version command |
| `L\n` | ? |
| `H\n`   | ? |
| `R\n` | Read command (Used for polling the rower) |


##### Data  
The rower starts spawning data in the following format.

| Byte | Description |
|---	|---	|
| 00-02 | ? (fixed to 0xA 0x8 0x0) |
| 03-04 | Total Minutes |
| 05-06 | Total Seconds |
| 07-11 | Total Distance |
| 12    | ? |
| 13-14 | Minutes to 500m |
| 15-16 | Seconds to 500m |
| 17-19 | SPM |
| 20-22 | Watt |
| 23-26 | Cal/h |
| 27-28 | Level |
| 29    | \n |


## HowTo
- Clone this repository
- Change to its root directory
- Run `npm install ` to install required node modules
- Run `npm start` to start the service
- Connect your rower
- Enjoy fancy grafs with Grafana or any visualization platform of your choise.

## Output
| Output    | Description     |
|---	|---	|
| Port is open  	|  the rower has been succesfully connected 	| 
| Data read	| 29B message has been received  	|  
| write point success  	| your data have been succesfully posted to your InfluxDB  	|  
| Total Time : XX:XX   	| Total time of activity | 
| Distance : Xm  	| Total distance of activity
| Time to 500m : XX:XX  	|   TIme to complete 500m	| 
| SPM : X 	|   Strokes per minute	|  
| Watt : X  	|  Stroke Watt	|   
| cal/h : X 	|   Callories per Hour	|   
| Level : X  	|  Level 	|   

## To do
The plan is to wrap this up to an electron app for storing and viewing data localy. If you feel like contributing please check https://github.com/dmamalis/apollopro_desktop 
