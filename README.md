# Project kinect azure

## Reference
https://docs.microsoft.com/ja-jp/azure/kinect-dk/about-azure-kinect-dk
### Setup Azure Kinect D
https://docs.microsoft.com/ja-jp/azure/kinect-dk/set-up-azure-kinect-dk
### Download SDK
https://docs.microsoft.com/ja-jp/azure/kinect-dk/sensor-sdk-download

## Installation Azure-Kinect-Sensor-SDK
### Install device to Linux
```
sudo apt install k4a-tools
```
### Configure Linux repository
https://docs.microsoft.com/ja-jp/windows-server/administration/linux-package-repository-for-microsoft-software
https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/usage.md#linux-device-setup

```
curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/multiarch/prod
sudo apt-get update
```
#### Clone Azure-Kinect-Sensor-SDK git repository
```
git clone https://github.com/microsoft/Azure-Kinect-Sensor-SDK
```
#### Linux Device Setup
On Linux, once attached, the device should automatically enumerate and load all drivers. However, in order to use the Azure Kinect SDK with the device and without being 'root', you will need to setup udev rules. We have these rules checked into this repo under 'scripts/99-k4a.rules'. To do so:
- Copy 'scripts/99-k4a.rules' into '/etc/udev/rules.d/'.
- Detach and reattach Azure Kinect devices if attached during this process.
- Once complete, the Azure Kinect camera is available without being 'root'.

#### Use Azure Kinect viewer
```
k4aviewer
```
#### Use Azure Kinect recorder
```
k4arecorder
```
### Building own Azure-Kinect-Sensor-SDK
#### Clone Azure-Kinect-Sensor-SDK git repository
```
git clone https://github.com/microsoft/Azure-Kinect-Sensor-SDK
```
#### Build
- Install if required or see building.md (https://github.com/microsoft/Azure-Kinect-Sensor-SDK/blob/develop/docs/building.md)
```
sudo apt install ninja-buid
sudo apt-get install libssl-dev
sudo apt-get install libxinerama-dev
sudo apt-get install libxcursor-dev
sudo apt install libsoundio-dev
sudo apt-get install libudev-dev libusb-1.0.0-dev
```
- Build with ninja
```
cd Azure-Kinect-Sensor-SDK
mkdir build
cd build
cmake .. -GNinja
```
- Debug Build:
```
cmake .. -GNinja -DCMAKE_BUILD_TYPE=Debug
```
- Run the build (ninja).
```
ninja
```
#### Insatall libdepthengine.so and libdepthengine.so.2.0
https://note.com/blkcatman/n/n49059b8b0b36
```
cd Azure-Kinect-Sensor-SDK
mkdir packages
cd packages
wget https://www.nuget.org/api/v2/package/Microsoft.Azure.Kinect.Sensor/1.4.0-alpha.4
mv 1.4.0-alpha.4 1.4.0-alpha.4.zip
unzip 1.4.0-alpha.4.zip
cd linux/lib/native/arm64/release/
cp libdepthengine.so /lib/aarch64-linux-gnu/
cp libdepthengine.so.2.0 /lib/aarch64-linux-gnu/
```

