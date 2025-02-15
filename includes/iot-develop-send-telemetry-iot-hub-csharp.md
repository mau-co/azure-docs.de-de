---
title: include file
description: include file
author: timlt
ms.service: iot-develop
ms.topic: include
ms.date: 05/05/2021
ms.author: timlt
ms.custom: include file
ms.openlocfilehash: 5a72409880953570163728d1e8e943ebdbeb1ba0
ms.sourcegitcommit: 38d81c4afd3fec0c56cc9c032ae5169e500f345d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/07/2021
ms.locfileid: "109518277"
---
## <a name="prerequisites"></a>Voraussetzungen
- Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto erstellen](https://azure.microsoft.com/free/?WT.mc_id=A261C142F), bevor Sie beginnen.
- [Visual Studio (Community, Professional oder Enterprise) 2019](https://visualstudio.microsoft.com/downloads/)
- Eine lokale Kopie des GitHub-Repositorys mit [Microsoft Azure IoT-Beispielen für C# (.NET)](https://github.com/Azure-Samples/azure-iot-samples-csharp). Laden Sie eine Kopie des Repositorys herunter, und extrahieren Sie es: [Zip herunterladen](https://github.com/Azure-Samples/azure-iot-samples-csharp/archive/master.zip)
- Azure-Befehlszeilenschnittstelle. In dieser Schnellstartanleitung gibt es zwei Möglichkeiten zum Ausführen von Azure CLI-Befehlen:
    - Verwenden Sie Azure Cloud Shell. Dabei handelt es sich um eine interaktive Shell, mit der CLI-Befehle im Browser ausgeführt werden. Diese Option wird empfohlen, da Sie nichts installieren müssen. Wenn Sie Cloud Shell zum ersten Mal verwenden, melden Sie sich beim [Azure-Portal](https://portal.azure.com) an. Führen Sie in der [Cloud Shell-Schnellstartanleitung](/azure/cloud-shell/quickstart) die Schritte zum **Starten von Cloud Shell** und **Auswählen der Bash-Umgebung** aus.
    - Führen Sie optional die Azure CLI auf dem lokalen Computer aus. Für den Schnellstart ist mindestens Version 2.0.76 der Azure CLI erforderlich. Führen Sie `az --version` aus, um die Version zu prüfen. Führen Sie die Schritte unter [Installieren der Azure CLI]( /cli/azure/install-azure-cli) aus, um die Azure CLI zu installieren oder zu aktualisieren, führen Sie sie aus, und melden Sie sich an. Installieren Sie die Azure CLI-Erweiterungen bei der ersten Verwendung, wenn Sie dazu aufgefordert werden.

[!INCLUDE [iot-hub-include-create-hub-cli](iot-hub-include-create-hub-cli.md)]

## <a name="run-a-simulated-device"></a>Ausführen eines simulierten Geräts
In diesem Abschnitt konfigurieren Sie Ihre lokale Umgebung und führen ein Beispiel aus, mit dem ein simulierter Temperaturregler erstellt wird.

So führen Sie die Beispielanwendung in Visual Studio aus:

1. Öffnen Sie in dem Order, in dem Sie die Azure IoT-Beispiele für C# extrahiert haben, die Projektmappendatei *azure-iot-samples-csharp-master\iot-hub\Samples\device\IoTHubDeviceSamples.sln"* in Visual Studio. 

1. Wählen Sie im **Projektmappen-Explorer** die Projektdatei **PnpDeviceSamples > TemperatureController** aus, klicken Sie mit der rechten Maustaste darauf, und wählen Sie **Als Startprojekt festlegen** aus.

1. Klicken Sie mit der rechten Maustaste auf das Projekt **TemperatureController**, wählen Sie **Eigenschaften** und dann die Registerkarte **Debuggen** aus, und fügen Sie dem Projekt die folgenden Umgebungsvariablen hinzu:

    | Name | Wert |
    | ---- | ----- |
    | IOTHUB_DEVICE_SECURITY_TYPE | *connectionString* |
    | IOTHUB_DEVICE_CONNECTION_STRING | Die Verbindungszeichenfolge, die Sie zuvor gespeichert haben |

1. Speichern Sie die aktualisierte Projektdatei **TemperatureController**.

1. Führen Sie in der CLI-App den Befehl [az iot hub monitor-events](/cli/azure/iot/hub#az_iot_hub_monitor_events) aus, um mit der Überwachung von Ereignissen auf dem simulierten IoT-Gerät zu beginnen.  Ereignismeldungen werden im Terminal angezeigt, sobald sie eintreffen.

    ```azurecli-interactive
    az iot hub monitor-events --output table --hub-name {YourIoTHubName}
    ```

1. Drücken Sie in Visual Studio STRG+F5, um das Beispiel auszuführen.

    Nachdem Ihr simuliertes Gerät eine Verbindung mit der IoT Central-Anwendung hergestellt hat, beginnt es mit dem Senden von Telemetriedaten. Die Verbindungsdetails und die Telemetrieausgabe werden in der Konsole angezeigt: 
    
    ```output
    Starting event monitor, use ctrl-c to stop...
    event:
      component: thermostat1
      interface: dtmi:com:example:TemperatureController;2
      module: ''
      origin: myDevice
      payload:
        temperature: 39.8
    
    event:
      component: thermostat2
      interface: dtmi:com:example:TemperatureController;2
      module: ''
      origin: myDevice
      payload:
        temperature: 36.7
    ```