---
title: Azure IoT device management with IoT extension for Azure CLI
description: Use the IoT extension for Azure CLI tool for Azure IoT Hub device management, featuring the Direct methods and the Twin’s desired properties management options.
author: kgremban

ms.author: kgremban
ms.service: iot-hub
ms.custom: devx-track-azurecli
ms.topic: how-to
ms.date: 01/16/2018
---

# Use the IoT extension for Azure CLI for Azure IoT Hub device management

![End-to-end diagram](media/iot-hub-get-started-e2e-diagram/2.png)

In this article, you learn how to use the IoT extension for Azure CLI with various management options on your development machine. [The IoT extension for Azure CLI](https://github.com/Azure/azure-iot-cli-extension) is an open-source IoT extension that adds to the capabilities of the [Azure CLI](/cli/azure/overview). The Azure CLI includes commands for interacting with Azure Resource Manager and management endpoints. For example, you can use Azure CLI to create an Azure VM or an IoT hub. A CLI extension enables an Azure service to augment the Azure CLI giving you access to additional service-specific capabilities. The IoT extension gives IoT developers command-line access to all IoT Hub, IoT Edge, and IoT Hub Device Provisioning Service capabilities.

| Management option          | Task  |
|----------------------------|-----------|
| Direct methods             | Make a device act such as starting or stopping sending messages or rebooting the device.                                        |
| Twin desired properties    | Put a device into certain states, such as setting an LED to green or setting the telemetry send interval to 30 minutes.         |
| Twin reported properties   | Get the reported state of a device. For example, the device reports the LED is blinking now.                                    |
| Twin tags                  | Store device-specific metadata in the cloud. For example, the deployment location of a vending machine.                         |
| Device twin queries        | Query all device twins to retrieve those twins with arbitrary conditions, such as identifying the devices that are available for use. |

For more detailed explanation on the differences and guidance on using these options, see [Device-to-cloud communication guidance](iot-hub-devguide-d2c-guidance.md) and [Cloud-to-device communication guidance](iot-hub-devguide-c2d-guidance.md).

Device twins are JSON documents that store device state information (metadata, configurations, and conditions). IoT Hub persists a device twin for each device that connects to it. For more information about device twins, see [Get started with device twins](device-twins-node.md).

[!INCLUDE [iot-hub-get-started-note](../../includes/iot-hub-get-started-note.md)]

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

## Prerequisites

* Complete the [Raspberry Pi online simulator](iot-hub-raspberry-pi-web-simulator-get-started.md) tutorial or one of the device tutorials. For example, you can go to [Raspberry Pi with Node.js](iot-hub-raspberry-pi-kit-node-get-started.md) or to one of the [Send telemetry](../iot-develop/quickstart-send-telemetry-iot-hub.md?pivots=programming-language-csharp) quickstarts. These articles cover the following requirements:

  * An active Azure subscription.
  * An Azure IoT hub under your subscription.
  * A client application that sends messages to your Azure IoT hub.

* Make sure your device is running with the client application during this tutorial.

* [Python 2.7x or Python 3.x](https://www.python.org/downloads/)

* The Azure CLI. If you need to install it, see [Install the Azure CLI](/cli/azure/install-azure-cli). At a minimum, your Azure CLI version must be 2.0.70 or above. Use `az –version` to validate.

[!INCLUDE [iot-hub-cli-version-info](../../includes/iot-hub-cli-version-info.md)]

* Install the IoT extension. The simplest way is to run `az extension add --name azure-iot`. [The IoT extension readme](https://github.com/Azure/azure-iot-cli-extension/blob/master/README.md) describes several ways to install the extension.

## Sign in to your Azure account

Sign in to your Azure account by running the following command:

```azurecli
az login
```

## Direct methods

```azurecli
az iot hub invoke-device-method --device-id <your device id> \
  --hub-name <your hub name> \
  --method-name <the method name> \
  --method-payload <the method payload>
```

## Device twin desired properties

Set a desired property interval = 3000 by running the following command:

```azurecli
az iot hub device-twin update -n <your hub name> \
  -d <your device id> --set properties.desired.interval=3000
```

This property can be read from your device.

## Device twin reported properties

Get the reported properties of the device by running the following command:

```azurecli
az iot hub device-twin show -n <your hub name> -d <your device id>
```

One of the twin reported properties is $metadata.$lastUpdated, which shows the last time the device app updated its reported property set.

## Device twin tags

Display the tags and properties of the device by running the following command:

```azurecli
az iot hub device-twin show --hub-name <your hub name> --device-id <your device id>
```

Add a field role = temperature&humidity to the device by running the following command:

```azurecli
az iot hub device-twin update \
  --hub-name <your hub name> \
  --device-id <your device id> \
  --set tags='{"role":"temperature&humidity"}'
```

## Device twin queries

Query devices with a tag of role = 'temperature&humidity' by running the following command:

```azurecli
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role = 'temperature&humidity'"
```

Query all devices except those with a tag of role = 'temperature&humidity' by running the following command:

```azurecli
az iot hub query --hub-name <your hub name> \
  --query-command "SELECT * FROM devices WHERE tags.role != 'temperature&humidity'"
```

## Next steps

You’ve learned how to monitor device-to-cloud messages and send cloud-to-device messages between your IoT device and Azure IoT Hub.

[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]
