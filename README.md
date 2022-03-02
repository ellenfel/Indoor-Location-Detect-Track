
Almost entirely "copied" from:

https://github.com/schollz/find
&
https://github.com/futurice/whereareyou

Passive indoor localization using the Wifi signal strength of a users devices. A set of slaves (like Raspberry Pis) is distributed at the location and send the signal strengths of detected devices to a master server. Based on a pretrained model the master predicts where the devices currently are located.

![](https://cloud.githubusercontent.com/assets/6676439/24398514/06cde36a-13aa-11e7-80f8-bad677786fa7.png)

## Setup

- Install Cython  
`apt-get install cython`
- Install Python dependencies  
`pip install -r requirements.txt`

### Slaves
- Install aircrack-ng  
`apt-get install aircrack-ng`  


### Master  
- Copy `example.env` to `.env` and add the appropriate configuration keys. Make sure that HOST holds the URI under which your server is reachable. `HOST:5000/gCallback` must be configured in your Google API Console to be an authorized OAuth redirect URI.  
`cp example.env .env`
- Adapt `static/office.svg` and `static/office_mapping.json` to your office (we recommend [this](http://editor.method.ac/) online editor). You can test your office mapping at /test_mapping.
- Create the database initially  
`python -c "from master import db, load_locations; db.create_all(); load_locations()"`  


## Usage
### Slaves
- Copy `example.run.sh` to `run.sh` and adapt the configuration to your needs  
`cp example.run.sh run.sh`  
  * FLOW_TOKEN - token for the Flowdock channel that should receive notifications when the slave crashes
  * SLAVE_ID - unique identifier of the slave
  * INTERFACE - Wifi interface that can be set to monitor mode
  * NETWORK - Wifi network monitored people should be logged in to
  * MASTER_ADDRESS - Internal IP address of the master server
- Run `bash run.sh` on every slave device
- To run it every time your Linux device finished booting, put this in your `/etc/profile` file:  
```bash
CMD="sudo bash /home/pi/whereareyou/run.sh"
COUNT=$(ps aux | grep '$CMD' | wc -l)
if [ $COUNT -eq 1 ]; then
    echo "Starting whereareyou"
    cd /home/pi/whereareyou/
    eval "$CMD &"
fi
```

### Master
- Run `master.py` on a device that can be accessed by the slaves in your internal network  
`python master`  

### Differences
-as slaves i'll be using esp8266 models 
-i will be building and deploying Open-source Bluetooth Beacons for usin BLE signals as well as Wifi's to detect and track location. 
-i will be storing data of locations and movements of users in a remote server.  
-i am not sure about possibility of this one but i will also at least try to build a walls detection system using RSS and  ML algorithms.   



