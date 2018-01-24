---
title: "Azure olay hub'ları yakalama gözden geçirme | Microsoft Docs"
description: "Olay hub'ları yakalama özelliği kullanılarak göstermek için Azure Python SDK'sını kullanan bir örnektir."
services: event-hubs
documentationcenter: 
author: djrosanova
manager: timlt
editor: 
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: sethm
ms.openlocfilehash: 97cadbde2ddedade1a8688f1380b9ff9194613e7
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2018
---
# <a name="event-hubs-capture-walkthrough-python"></a>Olay hub'ları yakalama izlenecek yol: Python

Yakalama Azure Event hubs bir özelliktir. Tercih ettiğiniz bir Azure Blob Depolama hesabına olay hub'ınızdaki veri akışı otomatik olarak teslim etmek için kullanabilirsiniz. Bu özellik toplu gerçek zamanlı akış verilerini işleme gerçekleştirmek kolaylaştırır. Bu makalede, olay hub'ları yakalama Python ile kullanmayı açıklar. Olay hub'ları yakalama hakkında daha fazla bilgi için bkz: [genel bakış makalesi](event-hubs-capture-overview.md).

Bu örnekte [Azure Python SDK'sı](https://azure.microsoft.com/develop/python/) yakalama özelliği göstermek için. Sender.py program sanal ortam telemetriyi JSON biçiminde Event Hubs'a gönderir. Olay hub'ı, bu verileri Blob Depolama yığınlardaki yazmak için yakalama özelliği kullanmak için yapılandırılır. Capturereader.py uygulama bu BLOB'lar okur ve cihaz başına bir ekleme dosyası oluşturur. Uygulama verileri sonra .csv dosyasına yazar.

## <a name="what-youll-accomplish"></a>Gerçekleştirmek

1. Bir Azure Blob storage hesabı ve bir blob kapsayıcısına, Azure portalını kullanarak oluşturun.
2. Azure Portalı'nı kullanarak bir olay hub'ları ad alanını oluşturun.
3. Etkinleştirilirse, Azure portalını kullanarak yakalama özelliği ile bir olay hub'ı oluşturun.
4. Verilerini bir Python komut dosyası kullanarak event hub'ına gönderir.
5. Yakalamaya almak dosyaları okuma ve bunları başka bir Python komut dosyası kullanarak işleyin.

## <a name="prerequisites"></a>Önkoşullar

- Python 2.7.x
- Bir Azure aboneliği
- Etkin bir [olay hub'ları ad alanı ve olay hub'ı](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob storage hesabı oluşturma
1. [Azure portalında][Azure portal] oturum açın.
2. Portalın sol bölmesinde seçin **yeni** > **depolama** > **depolama hesabı**.
3. Seçimleri tamamladıktan **depolama hesabı oluşturma** bölmesi ve ardından **oluşturma**.
   
   !["Depolama hesabı oluşturma" bölmesi][1]
4. Gördükten sonra **dağıtımları başarılı** ileti, yeni depolama hesabı adını ve buna seçin **Essentials** bölmesi ve ardından **BLOB'lar**. Zaman **Blob hizmeti** bölmesini açar, seçin **+ kapsayıcı** üstünde. Kapsayıcı adı **yakalama**ve ardından kapatın **Blob hizmeti** bölmesi.
5. Seçin **erişim anahtarları** sol bölmede kopyalayıp depolama hesabı adını ve değerini **key1**. Bu değerleri Not Defteri'nde veya başka bir geçici konuma kaydedin.

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Olay hub'ınıza olayları göndermek üzere bir Python komut dosyası oluşturma
1. Sık kullanılan Python düzenleyici gibi açıp [Visual Studio Code][Visual Studio Code].
2. Adlı bir komut dosyası oluşturma **sender.py**. Bu komut dosyası 200 olayları olay hub'ınıza gönderir. JSON'da gönderilen basit ortam okumalar oldukları.
3. Sender.py aşağıdaki kodu yapıştırın:
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus import ServiceBusService
   
   sbs = ServiceBusService(service_namespace='INSERT YOUR NAMESPACE NAME', shared_access_key_name='RootManageSharedAccessKey', shared_access_key_value='INSERT YOUR KEY')
   devices = []
   for x in range(0, 10):
       devices.append(str(uuid.uuid4()))
   
   for y in range(0,20):
       for dev in devices:
           reading = {'id': dev, 'timestamp': str(datetime.datetime.utcnow()), 'uv': random.random(), 'temperature': random.randint(70, 100), 'humidity': random.randint(70, 100)}
           s = json.dumps(reading)
           sbs.send_event('INSERT YOUR EVENT HUB NAME', s)
       print y
   ```
4. Ad alanı adı, anahtar değeri ve Event Hubs ad alanı oluşturduğunuzda aldığınız olay hub'ı adı kullanmak için önceki kod güncelleştirin.

## <a name="create-a-python-script-to-read-your-capture-files"></a>Yakalama dosyalarınızı okuyabilecek bir Python komut dosyası oluşturma

1. Bölmesinde ve select doldurmak **oluşturma**.
2. Adlı bir komut dosyası oluşturma **capturereader.py**. Bu komut dosyası yakalanan dosyaları okur ve bu cihaz için yalnızca veri yazmak için cihaz başına bir dosya oluşturur.
3. Capturereader.py aşağıdaki kodu yapıştırın:
   
   ```python
   import os
   import string
   import json
   import avro.schema
   from avro.datafile import DataFileReader, DataFileWriter
   from avro.io import DatumReader, DatumWriter
   from azure.storage.blob import BlockBlobService
   
   def processBlob(filename):
       reader = DataFileReader(open(filename, 'rb'), DatumReader())
       dict = {}
       for reading in reader:
           parsed_json = json.loads(reading["Body"])
           if not 'id' in parsed_json:
               return
           if not dict.has_key(parsed_json['id']):
               list = []
               dict[parsed_json['id']] = list
           else:
               list = dict[parsed_json['id']]
               list.append(parsed_json)
       reader.close()
       for device in dict.keys():
           deviceFile = open(device + '.csv', "a")
           for r in dict[device]:
               deviceFile.write(", ".join([str(r[x]) for x in r.keys()])+'\n')
   
   def startProcessing(accountName, key, container):
       print 'Processor started using path: ' + os.getcwd()
       block_blob_service = BlockBlobService(account_name=accountName, account_key=key)
       generator = block_blob_service.list_blobs(container)
       for blob in generator:
           #content_length == 508 is an empty file, so only process content_length > 508 (skip empty files)
           if blob.properties.content_length > 508:
               print('Downloaded a non empty blob: ' + blob.name)
               cleanName = string.replace(blob.name, '/', '_')
               block_blob_service.get_blob_to_path(container, blob.name, cleanName)
               processBlob(cleanName)
               os.remove(cleanName)
           block_blob_service.delete_blob(container, blob.name)
   startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
   ```
4. Depolama hesabı adı ve anahtar için uygun değerleri çağrısı yapıştırın `startProcessing`.

## <a name="run-the-scripts"></a>Komut dosyalarını çalıştır
1. Python kendi yolunda sahip bir komut istemi açın ve Python önkoşul paketleri yüklemek için şu komutları çalıştırın:
   
   ```
   pip install azure-storage
   pip install azure-servicebus
   pip install avro
   ```
   
   Ya da önceki bir sürümü varsa **azure depolama** veya **azure**, kullanmanız gerekebilir **--yükseltme** seçeneği.
   
   (Çoğu sistemlerde gerekli değildir) aşağıdaki komutu çalıştırın gerekebilir:
   
   ```
   pip install cryptography
   ```
2. Dizininizin açtıkları her yerde sender.py ve capturereader.py kaydettiğiniz için değiştirmek ve bu komutu çalıştırın:
   
   ```
   start python sender.py
   ```
   
   Bu komut, gönderenin çalıştırmak için yeni bir Python işlemi başlatır.
3. Çalıştırmak yakalama için bir kaç dakika bekleyin. Ardından, özgün komut penceresine aşağıdaki komutu yazın:
   
   ```
   python capturereader.py
   ```

   Bu yakalama işlemci yerel dizin depolama hesabı/kapsayıcıdan BLOB indirmek için kullanır. Boş olmayan tüm işler ve sonuçları yerel dizine .csv dosyaları olarak yazar.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları kullanarak Event Hubs hakkında daha fazla bilgi alabilirsiniz:

* [Event Hubs'a genel bakış yakalama][Overview of Event Hubs Capture]
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Event Hubs'a genel bakış][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
