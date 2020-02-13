vsm-cpp-ardupilot
===========

C++11 VSM for Ardupilot autopilot implemented using SDK of [Universal ground Control Software](http://www.ugcs.com/ "UgCS").

# Preparation

```
git clone git@github.com:sensyn-robotics/vsm-cpp-ardupilot.git
git clone git@github.com:ugcs/vsm-cpp-common.git
cd vsm-cpp-ardupilot
```

# Build the development environment as a Docker image

At first, you build the development environment as a Docker image.
The Docker image include some tools like gcc or cmake and VSM SDK.

```shell
make ci_image
```

# Buld VSM

## for Linux

### Run the Docker container as the development environment

```shell
docker-compose run --rm linux-dev bash
```

### Generate an executable binary

```shell
./linux/build.sh
```

Then, you would get `build/linux/vsm-ardupilot`.

## for Windows

### Run the Docker container as the development environment

```shell
docker-compose run --rm windows-dev bash
```

### Generate an executable binary

```shell
./windows/build.sh
```

Then, you would get `build/windows/vsm-ardupilot.exe`.

### C++ development with Visual Studio Code

1. Run the following commands in your host OS.

```shell
rm -rf build/include
mkdir -p build/include/system build/include/vsm
docker-compose run --rm --name windows-dev windows-dev \
    sh -c "tar zcvfh vsm-sdk.tgz /opt/vsm-sdk/vsm-sdk/include \
    && tar zcvfh system.tgz /usr/x86_64-w64-mingw32/include"
pushd build/include/vsm
tar zxvf ../../../vsm-sdk.tgz
mv opt/vsm-sdk/vsm-sdk/include/* .
mv generated/* .
mv google/* .
rm -rf opt generated google
popd
pushd build/include/system/
tar zxvf ../../../system.tgz
mv usr/x86_64-w64-mingw32/include/* .
rm -rf usr
popd
rm -f system.tgz vsm-sdk.tgz
```

2. Append the following lines into `configurations` > `includePath` in `.vscode/c_cpp_properties.json`.

```json
"${workspaceFolder}/build/include/vsm",
"${workspaceFolder}/build/include/system"
```
