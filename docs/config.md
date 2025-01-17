# Configuration Guide

## Overview

This SDK uses a structure called "Config" to store and manage configuration, read comments of public functions in ["config/config.go"](https://github.com/qingstor/qingstor-sdk-go/blob/master/config/config.go) for details.

Except for Access Key, you can also configure the API endpoint for private cloud usage scenario. All available configurable items are listed in the default configuration file.

___Default Configuration File:___

``` yaml
# QingStor services configuration

access_key_id: 'ACCESS_KEY_ID'
secret_access_key: 'SECRET_ACCESS_KEY'

host: 'qingstor.com'
port: 443
protocol: 'https'
connection_retries: 3

endpoint: 'https://qingstor.com:443'

enable_virtual_host_style: false # default false.
enable_dual_stack: false # default false.

# Valid log levels are "debug", "info", "warn", "error", and "fatal".
log_level: 'warn'
```

We also support setting the following environment variables:

- QINGSTOR_ACCESS_KEY_ID
- QINGSTOR_SECRET_ACCESS_KEY
- QINGSTOR_CONFIG_PATH
- QINGSTOR_ENABLE_VIRTUAL_HOST_STYLE
- QINGSTOR_ENABLE_DUAL_STACK

## Usage

Just create a config structure instance with your API Access Key, and initialize services you need with Init() function of the target service.

## Code Snippet

Create default configuration

```go
defaultConfig, _ := config.NewDefault()
```

Create configuration from Access Key

```go
configuration, _ := config.New("ACCESS_KEY_ID", "SECRET_ACCESS_KEY")

anotherConfiguration := config.NewDefault()
anotherConfiguration.AccessKeyID = "ACCESS_KEY_ID"
anotherConfiguration.SecretAccessKey = "SECRET_ACCESS_KEY"
```

Load user configuration

```go
userConfig, _ := config.NewDefault().LoadUserConfig()
```

Load configuration from config file

```go
configFromFile, _ := config.NewDefault().LoadConfigFromFilePath("PATH/TO/FILE")
```

Change API endpoint

```go
moreConfiguration, _ := config.NewDefault()

moreConfiguration.Protocol = "http"
moreConfiguration.Host = "api.private.com"
moreConfiguration.Port = 80
```

Change http timeout

```go
customConfiguration, _ := config.NewDefault().LoadUserConfig()
// For the default value refers to DefaultHTTPClientSettings in config package
// ReadTimeout affect each call to HTTPResponse.Body.Read()
customConfiguration.HTTPSettings.ReadTimeout = 2 * time.Minute
// WriteTimeout affect each write in io.Copy while sending HTTPRequest
customConfiguration.HTTPSettings.WriteTimeout = 2 * time.Minute
// Re-initialize the client to take effect
customConfiguration.InitHTTPClient()
```
