---
title: 'Untersuchen von Daten in Azure Blob Storage mit „pandas“: Team Data Science-Prozess'
description: Erfahren Sie, wie Sie in einem Azure-Blobcontainer gespeicherte Daten mithilfe des Python-Pakets „pandas“ untersuchen.
services: machine-learning
author: marktab
manager: marktab
editor: marktab
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 04/30/2021
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: 488e71925e557d8697d56575a6541b9aff047b35
ms.sourcegitcommit: 3bb9f8cee51e3b9c711679b460ab7b7363a62e6b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/14/2021
ms.locfileid: "112078274"
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Untersuchen von Daten in Azure Blob Storage mit „pandas“

In diesem Artikel wird erläutert, wie Sie in einem Azure-Blobcontainer gespeicherte Daten mithilfe des Python-Pakets [pandas](https://pandas.pydata.org/) untersuchen.

Dieser Task ist ein Schritt im [Team Data Science-Prozess](overview.md).

## <a name="prerequisites"></a>Voraussetzungen
In diesem Artikel wird davon ausgegangen, dass Sie Folgendes abgeschlossen haben:

* Sie haben ein Azure-Speicherkonto erstellt. Anweisungen finden Sie unter [Erstellen eines Azure-Speicherkontos](../../storage/common/storage-account-create.md).
* Die Daten wurden in einem Azure Blob Storage-Konto gespeichert. Anweisungen finden Sie unter [Verschieben von Daten in und aus Azure Storage](../../storage/common/storage-choose-data-transfer-solution.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>Laden der Daten in ein Pandas-DataFrame
Damit ein Dataset untersucht und bearbeitet werden kann, muss es zuerst aus der Blobquelle in eine lokale Datei heruntergeladen werden, die anschließend in ein Pandas-DataFrame geladen werden kann. Nachfolgend sehen Sie für dieses Verfahren erforderlichen Schritte:

1. Laden Sie die Daten mithilfe des Blob-Diensts und folgenden Python-Beispielcodes aus dem Azure-Blob herunter. Ersetzen Sie die Variablen im folgenden Code durch die für Ihre Umgebung geltenden Werte:

    ```python
    from azure.storage.blob import BlobServiceClient
    import pandas as pd

    STORAGEACCOUNTURL= <storage_account_url>
    STORAGEACCOUNTKEY= <storage_account_key>
    LOCALFILENAME= <local_file_name>
    CONTAINERNAME= <container_name>
    BLOBNAME= <blob_name>

    #download from blob
    t1=time.time()
    blob_service_client_instance = BlobServiceClient(account_url=STORAGEACCOUNTURL, credential=STORAGEACCOUNTKEY)
    blob_client_instance = blob_service_client_instance.get_blob_client(CONTAINERNAME, BLOBNAME, snapshot=None)
    with open(LOCALFILENAME, "wb") as my_blob:
        blob_data = blob_client_instance.download_blob()
        blob_data.readinto(my_blob)
    t2=time.time()
    print(("It takes %s seconds to download "+BLOBNAME) % (t2 - t1))
    ```

1. Lesen Sie die Daten aus der heruntergeladenen Datei in ein Pandas-DataFrame ein.

    ```python
    # LOCALFILE is the file path
    dataframe_blobdata = pd.read_csv(LOCALFILENAME)
    ```

Wenn Sie allgemeinere Informationen zum Lesen aus einem Azure Storage Blob benötigen, finden Sie diese in unserer Dokumentation [Azure Storage Blobs-Clientbibliothek für Python](/python/api/overview/azure/storage-blob-readme).  

Sie können nun die Daten durchsuchen und Funktionen mit diesem DataSet generieren.  

## <a name="examples-of-data-exploration-using-pandas"></a><a name="blob-dataexploration"></a>Beispiele für das Untersuchen von Daten mithilfe von Pandas
Hier sind einige Beispiele für Möglichkeiten zum Durchsuchen von Daten mithilfe von Pandas:

1. Untersuchen der **Anzahl von Zeilen und Spalten**

    ```python
    print('the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape)
    ```

1. **Überprüfen** der ersten oder letzten **Zeilen** im folgenden Dataset:

    ```python
    dataframe_blobdata.head(10)

    dataframe_blobdata.tail(10)
    ```

1. Überprüfen des **Datentyps** der einzelnen importierten Spalten:

    ```python
    for col in dataframe_blobdata.columns:
        print(dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype)
    ```

1. Überprüfen der **Basisstatistiken** für die Spalten im DataSet:

    ```python
    dataframe_blobdata.describe()
    ```

1. Überprüfen der Anzahl der Einträge für jeden Spaltenwert:

    ```python
    dataframe_blobdata['<column_name>'].value_counts()
    ```

1. **Zählen der fehlenden Werte** im Vergleich zur tatsächlichen Anzahl der Einträge in den einzelnen Spalten:

    ```python
    miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
    print(miss_num)
    ```

1. Löschen **fehlender Werte** in einer bestimmten Spalte aus den Daten:

    ```python
    dataframe_blobdata_noNA = dataframe_blobdata.dropna()
    dataframe_blobdata_noNA.shape
    ```

    Alternatives Ersetzen der fehlenden Werte mit der "mode"-Funktion:

    ```python
    dataframe_blobdata_mode = dataframe_blobdata.fillna(
        {'<column_name>': dataframe_blobdata['<column_name>'].mode()[0]})
    ```

1. Erstellen eines **Histogramms** mithilfe der Variablenanzahl von Gruppen zur Darstellung der Verteilung einer Variablen:

    ```python
    dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')

    np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
    ```

1. Überprüfen der **Korrelationen** zwischen Variablen mithilfe eines Punktdiagramms oder einer integrierten „correlation“-Funktion:

    ```python
    # relationship between column_a and column_b using scatter plot
    plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])

    # correlation between column_a and column_b
    dataframe_blobdata[['<column_a>', '<column_b>']].corr()
    ```
