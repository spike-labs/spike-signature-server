# spike-signature-server
Signature service

## Build and install spike-signature-server
1. Clone the repository
```shell
cd  $(go env GOPATH)/src/github.com
git clone https://github.com/spike-engine/spike-signature-server.git
cd  spike-signature-server
```
2. Install all the dependencies
```shell
go mod tidy
```
3. Make build
```shell
go build -o spike-signature-server ./main.go
```
4. Update Config
```
cp config.toml.example config.toml
```
5. Run
```
./spike-signature-server start
```

## Config 

1. Config file localtion.  
By default, spike-signature-server reads configuration from config.toml under the current folder. If it is not created, you may copy from config.config.example. If you need to specify the config file, you may set the enviornment as follows:
```
export SPIKE_SIGNATURE_CONFIG=~/spike_home/config-signature.toml
```
2. keystore file.  
If spike-signature-server cannot find keystore file with *walletFile* config under folder with *walletFolder* config, you can use spike-signature-server cli generate one.(example: ./spike-signature-server wallet add) You also can  replace it with your own keystore file if needed.
3. Machine ID  
To run multiple signature services, you may assign each server a different machine ID with *machineId* config.

## Register spike as a system service
1. Link server into system binary path
```
sudo ln ./spike-signature-server /usr/local/bin
```
2. Copy config file into spike home
```
sudo mkdir -p /etc/spike/
sudo cp ./config.toml /etc/spike/config-signature.toml
```
3. Startup script
```shell
sudo vim /etc/systemd/system/spike-signature-server.service
```
Specify the path to the binary
```ini
[Service] 
ExecStart=/usr/local/bin/spike-signature-server start
Environment=SPIKE_SIGNATURE_CONFIG=/etc/spike/config-signature.toml
Restart=always
RestartSec=5
```
```shell
sudo systemctl daemon-reload
sudo systemctl start spike-signature-server
```
Check Program Exec Log 
```shell
sudo journalctl -u spike-signature-server.service  -f 
```
