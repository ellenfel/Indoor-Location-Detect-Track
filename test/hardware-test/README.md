to flash the firmware:
esptool.py --port /dev/ttyUSB0 --baud 115200 write_flash -fm qio 0x00000 nodemcu-release-7-modules-2022-03-05-20-32-00-float.bin 
