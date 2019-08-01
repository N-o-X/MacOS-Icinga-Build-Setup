# MacOS-Icinga-Build-Setup

### 1) Install dependencies

```sh
brew install ccache boost cmake bison flex openssl@1.1 mysql-connector-c++
brew install monitoring-plugins
brew install mariadb
brew install redis
```

### 2) Setup repository
```sh
cd ~/projects
git clone git@github.com:Icinga/icinga2.git
cd icinga2
```

### 3) Setup debug build
```sh
mkdir debug
cd debug
cmake .. -DICINGA2_USER=noah \
         -DICINGA2_GROUP=staff \
         -DICINGA2_COMMAND_GROUP=staff \
         -DICINGA2_RUNDIR=/Users/noah/i2/var/run/ \
         -DICINGA2_WITH_PGSQL=off \
         -DICINGA2_PLUGINDIR=usr/lib/nagios/plugins \
         -DUSE_SYSTEMD=off \
         -DCMAKE_INSTALL_PREFIX=/Users/noah/i2/ \
         -DOPENSSL_CRYPTO_LIBRARY=/usr/local/opt/openssl/lib/libcrypto.dylib \
         -DOPENSSL_INCLUDE_DIR=/usr/local/opt/openssl/include \
         -DOPENSSL_SSL_LIBRARY=/usr/local/opt/openssl/lib/libssl.dylib
```

### 4) Build and install Icinga
```sh
make install -j 8
```

### 5) Add Icinga to PATH
```sh
echo 'export PATH="/Users/noah/i2/sbin:$PATH"' >> ~/.zshr
```

### 6) Setup CLion
#### 6.1) CMake
Open `CLion => Preferences => Build, Execution, Deployment => CMake => Debug` and change the following:
- CMake options: `-DICINGA2_USER=noah -DICINGA2_GROUP=staff -DICINGA2_COMMAND_GROUP=staff -DICINGA2_RUNDIR=/Users/noah/i2/var/run/ -DICINGA2_WITH_PGSQL=off -DICINGA2_PLUGINDIR=usr/lib/nagios/plugins -DUSE_SYSTEMD=off -DCMAKE_INSTALL_PREFIX=/Users/noah/i2/ -DOPENSSL_CRYPTO_LIBRARY=/usr/local/opt/openssl/lib/libcrypto.dylib -DOPENSSL_INCLUDE_DIR=/usr/local/opt/openssl/include -DOPENSSL_SSL_LIBRARY=/usr/local/opt/openssl/lib/libssl.dylib`
- Build options: `-j 8`

After that click apply and wait until CLion is done indexing all the files.

#### 6.2) Build & Run
Open `Run => Edit Configurations` and find a CMake Application called `icinga` and change the following:
- Executable: `/Users/noah/i2/lib/icinga2/sbin/icinga2`
- Programm arguments: `daemon`
- Before launch: Remove `build` and add `install` instead

After clicking apply and waiting a few seconds, you should be able to select and run `icinga` in the top right corner.

#### 6.3) Enable Code Insight
Right click the `icinga2/lib` folder and select `Mark Directory as => Project Sources and Headers`.

