# Overview

stasher is a tool to manage DCOS infrastructure

# Requirements

Use the config for DCOS_PASSWORD_DEV, DCOS_PASSWORD_STAGE AND DCOS_PASSWORD_PROD

# Usage

```./stasher -h```

# Examples

```
./stasher -s ethos01-an1-control.dev.cloud.adobe.io -t apigateway
./stasher -s ethos01-an1-control.dev.cloud.adobe.io -v -t klam-ssh
./stasher -s ethos01-an1-control.dev.cloud.adobe.io -v -a all
./stasher -s ethos01-an1-control.dev.cloud.adobe.io -v -a capcom
```

# Usages

## Show Deployments

`./stasher -s ethos01-ew1-control.stage.cloud.adobe.io -d`

## Kill task

`./stasher -s ethos01-an1-control.dev.cloud.adobe.io -t ethos-datadog.765773b3-9977-11e7-9948-766e67cdfad4 -k`

## Show tasks

`./stasher -s ethos01-an1-control.dev.cloud.adobe.io -t ethos-datadog`

## List apps

`./stasher -s ethos01-ue1-control.dev.cloud.adobe.io -l | grep '"id"' | more`

## Show apps

`./stasher -s ethos01-an1-control.dev.cloud.adobe.io -a ethos-datadog`

## Edit app

`./stasher -s ethos01-ue1-control.dev.cloud.adobe.io -v -a ethos-datadog -e`

## Show Mesos slaves

`./stasher -s ethos01-ue1-control.dev.cloud.adobe.io -m slaves`

`./stasher -s ethos01-ue1-control.dev.cloud.adobe.io -m flags`

## Show Mesos tasks

`./stasher -s ethos01-ue1-control.dev.cloud.adobe.io -m tasks`

## Staging tasks

`./stasher -s ethos01-uw2-control.dev.cloud.adobe.io -m frameworks | jq -r '.frameworks[]' | grep -B 5 STAGI | grep '"id"'
"id": "liuapi-dev-b647512149.liuapi---790396c0bcc85c1012191faaa48ff57e715dded7----0ada80.c57ec2c6-a9d0-11e7-93f7-0ad74b1f6b8d",
"id": "rmcmahonorderapi-dev-a463154862.rmcmahonorderapi---fd1dde806e71666b7e04036efa3dcb16cf28df05----9ec70c.c9d9d5c1-a955-11e7-93f7-0ad74b1f6b8d`

## Count unreachable tasks

`./stasher -s ethos01-uw2-control.dev.cloud.adobe.io -m frameworks | jq -r '.frameworks[]' | grep -B 5 UNREA | grep '"id"' | wc -l
69`

## Killed tasks

`./stasher -s ethos01-uw2-control.dev.cloud.adobe.io -m frameworks | jq -r '.frameworks[]' | grep -B 40  KILLING | grep '"id"'`

## Kill specific task

`/stasher -s ethos01-ue1-control.stage.cloud.adobe.io -t $(./stasher -s ethos01-ue1-control.stage.cloud.adobe.io -t apigateway | grep 10.71.22.65 | awk '{ print $2 }') -k`

## Restart multiple hosts

`for i in "10.70.24.136" "10.70.24.143"; do ./stasher -s ethos01-an1-control.prod.cloud.adobe.io -t $(./stasher -s ethos01-an1-control.prod.cloud.adobe.io -t apigateway | grep $i | awk '{ print $2 }') -k; sleep 100; done`

## Create a JIRA ticket

`./stasher -z "Api gateway restart" -y "Api gateway in host 10.1.2.3" -j`

## Restart apigateway and create JIRA ticket

`SERVER=ethos01-ue1-control.prod.cloud.adobe.io; for i in "10.71.28.149" "10.71.30.90"; do ./stasher -s $SERVER -t $(./stasher -s $SERVER -t apigateway | grep $i | awk '{ print $2 }') -k -z "Api gateway restart in host $i" -y "Api gateway in host $i" -j; sleep 10;  done`

## Send message to slack CHANNEL

Go to https://channel.slack.com/apps/manage/custom-integrations and get the Webhook URL.

CHANNEL="#stasher" WEBHOOKURL="https://hooks.slack.com/services/XXXX/XXXX/" USERNAME="sancheza" ./stasher -c "This is posted to #ethos-ops-standup and comes from a bot named sancheza"

# Author

Alejandro Sanchez Acosta <sancheza@adobe.com>
