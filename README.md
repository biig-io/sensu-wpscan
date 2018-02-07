# Sensu docker for wpscan
Docker ready to use for [sensu-wordpress plugin](https://github.com/sensu-plugins/sensu-plugins-wordpress)


## Fill the sensu_config with your sensu server informations
Edit `sensu_config/conf/client.json`.

⚠️ Dont forget to set your `sensu_config/config.json` with your config transport (Host, Password, SSL if need).

## Build the image
```
docker-compose build
```

## Start the docker
```
docker-compose up -d
```

## Install the check in your sensu-server
Create the `/etc/sensu/conf.d/checks/check_wpscan.json` file with the following content:

```
{
  "checks": {
    "check_wpscan_mywordpress": {
      "command": "/opt/sensu/embedded/bin/check-wpscan.rb -u https://www.mywordpress.com",
      "interval": 86400,
      "subscribers": [ "client:WPScanNodeName" ]
    }
}
```
