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

#### For Intel architecture
```
curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/prod
sudo apt-get update
```
if following error is encountered, you need specify the architecture of the repository to [arch=amd64]:
```
update: N: Skipping acquire of configured file 'main/binary-i386/Packages' as repository 'https://packages.microsoft.com/ubuntu/18.04/prod' doesn't support support architecture 'i386'.
```
Modify etc/apt/sources.list. At the bottom of the file, change from:
```
deb https://packages.microsoft.com/ubuntu/18.04/prod bionic main
# deb-src https://packages.microsoft.com/ubuntu/18.04/prod bionic main
```
to:
```
deb [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/prod bionic main
# deb-src [arch=amd64] https://packages.microsoft.com/ubuntu/18.04/prod bionic main
```

#### For ARM architecture
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
sudo apt install ninja-build
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
Caution
- k4a-tools binary distribution installs libk4a1.4 which is incompatible with libk4abt1.0-dev
- to install libk4abt, k4a-tools need to be re-installed with 1.3.0 version specified
```
curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo apt-add-repository https://packages.microsoft.com/ubuntu/18.04/multiarch/prod
curl -sSL https://packages.microsoft.com/config/ubuntu/18.04/multiarch/prod.list | sudo tee /etc/apt/sources.list.d/microsoft-prod.list
curl -sSL https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -
sudo apt-get update
sudo apt install libk4a1.3-dev
sudo apt install libk4abt1.0-dev
sudo apt install k4a-tools=1.3.0
```

### Building Azure-Kinect-Samples/body-tracking-samples
- cloned repo https://github.com/microsoft/Azure-Kinect-Samples
```
git clone https://github.com/microsoft/Azure-Kinect-Samples
git submodule update --init --recursive
```
- follow instructions on this link: https://docs.microsoft.com/en-us/windows-server/administration/linux-package-repository-for-microsoft-software
```
apt install libk4abt0.9-dev
apt install libxi-dev
sudo apt-get install libglfw3-dev
```
- install vcpkg
```
cd /opt
git clone https://github.com/microsoft/vcpkg
sudo chown -hR user_name:user_name ./vcpkg
./vcpkg/bootstrap-vcpkg.sh
```
- Changed to git project root
```
mkdir build
cd build
cmake .. -GNinja
```
