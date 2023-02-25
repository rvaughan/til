# Installing on a Raspberry Pi

To start using Greengrass you need to initially configure the AWS CLI on the Raspberry Pi. On my version I needed to create a symlink to the Python3 binary to allow the AWS CLI to work unimpeded.

```bash
sudo ln -s /usr/bin/python3 /usr/bin/python
```

As the Pi is a 32-bit machine, we need to use [v1](https://docs.aws.amazon.com/cli/v1/userguide/install-linux.html) of the CLI tool rather than v2.

```bash
curl "https://s3.amazonaws.com/aws-cli/awscli-bundle.zip" -o "awscli-bundle.zip"
unzip awscli-bundle.zip
sudo ./awscli-bundle/install -i /usr/local/aws -b /usr/local/bin/aws
```

Setup our AWS env

  - should probably set up an IAM user for the device to use.

```bash
aws configure
```

Install Java (specifically the OpenJSK)

```bash
sudo apt install default-jdk
```

Get the Greengrass installer.

```bash
curl -s https://d2s8p88vqu9w66.cloudfront.net/releases/greengrass-nucleus-latest.zip > greengrass-nucleus-latest.zip
unzip greengrass-nucleus-latest.zip -d GreengrassInstaller
```

Go to the GG console and select "[Setup One Core Device](https://eu-west-2.console.aws.amazon.com/iot/home?region=eu-west-2#/greengrass/v2/cores)]. In the window that pops up, call the device `demo-pi` and then name the group `demo`.

Install Greengrass on the Pi, and register the device using this command:

```bash
sudo -E java -Droot="/greengrass/v2" -Dlog.store=FILE -jar ./GreengrassInstaller/lib/Greengrass.jar \
    --aws-region eu-west-2 \
    --thing-name demo-pi \
    --thing-group-name demo \
    --component-default-user ggc_user:ggc_group \
    --provision true \
    --setup-system-service true \
    --deploy-dev-tools true
```

After a short while you should see the device show up in the AWS console, and then you will also see the Pi via this command line:

```bash
aws greengrassv2 list-effective-deployments --core-device-thing-name demo-pi
```

The Greengrass CLI should be installed on your device (`/greengrass/v2/bin/greengrass-cli`) but for some reason it isn't symlinked properly, so use this command:

```bash
sudo ln -s /greengrass/v2/bin/greengrass-cli /usr/bin/greengrass-cli
```
