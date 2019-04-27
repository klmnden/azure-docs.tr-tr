---
title: Okuma yakalanan verileri Python uygulaması - Azure Event Hubs | Microsoft Docs
description: Event Hubs yakalama özelliğini kullanarak göstermek için Azure Python SDK'sını kullanan örnek.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: timlt
editor: ''
ms.assetid: bdff820c-5b38-4054-a06a-d1de207f01f6
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 639bc4ff9c69bca3d5f8bca6967bfc3e8e6a13d4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60822404"
---
# <a name="event-hubs-capture-walkthrough-python"></a>Event Hubs yakalama izlenecek yol: Python

Yakalama, Azure Event Hubs'ın bir özelliğidir. Bir Azure Blob Depolama hesabı, tercih ettiğiniz için olay hub'ınızdaki akış verilerini otomatik olarak teslim etmek için kullanabilirsiniz. Bu özellik, toplu üzerinde gerçek zamanlı akış verilerini işleme gerçekleştirmeyi kolaylaştırır. Bu makalede, Python ile Event Hubs yakalama özelliğini kullanmayı açıklar. Event Hubs yakalama hakkında daha fazla bilgi için bkz: [genel bakış makalesi](event-hubs-capture-overview.md).

Bu örnekte [Azure Python SDK'sı](https://azure.microsoft.com/develop/python/) yakalama özelliği göstermek için. Sender.py programı, Event Hubs'a JSON biçiminde sanal ortam telemetri gönderir. Olay hub'ı, bu verileri Blob Depolama alanında toplu yazılacak yakalama özelliğini kullanmak için yapılandırılır. Capturereader.py uygulama, bu BLOB'ları okur ve cihaz başına bir ekleme dosyası oluşturur. Uygulama, ardından .csv dosyalarına veri yazar.

## <a name="what-youll-accomplish"></a>Gerçekleştirilmesi

1. Azure portalını kullanarak bir Azure Blob Depolama hesabı ve içinde bir blob kapsayıcı oluşturun.
2. Azure portalını kullanarak bir Event Hubs ad alanı oluşturun.
3. Yakalama özelliğinin etkin, Azure portalını kullanarak bir olay hub'ı oluşturun.
4. Python betiğini kullanarak, verileri olay hub'ına gönderebilirsiniz.
5. Capture dosyaları okumak ve bunları başka bir Python betiğini kullanarak işleyin.

## <a name="prerequisites"></a>Önkoşullar

- Python 2.7.x
- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- Etkin bir [Event Hubs ad alanı ve olay hub'ı](event-hubs-create.md). 
- Etkinleştirme **yakalama** özelliği olay hub'ı yönergeleri izleyerek: [Azure portalını kullanarak Event Hubs yakalama özelliğini etkinleştirme](event-hubs-capture-enable-through-portal.md)

## <a name="create-an-azure-blob-storage-account"></a>Bir Azure Blob Depolama hesabı oluşturma
1. [Azure portalında][Azure portal] oturum açın.
2. Portalın sol bölmesinde seçin **yeni** > **depolama** > **depolama hesabı**.
3. Seçimleri tamamladıktan **depolama hesabı oluşturma** bölmesinde tıklayın ve ardından **Oluştur**.
   
   !["Depolama hesabı oluştur" bölmesi][1]
4. Gördükten sonra **dağıtımlar başarılı** iletisi, yeni depolama hesabı adını ve buna seçin **Essentials** bölmesinde tıklayın ve ardından **Blobları**. Zaman **Blob hizmeti** bölmesi açıldığında, seçin **+ kapsayıcı** en üstünde. Kapsayıcı adı **yakalama**ve ardından kapatın **Blob hizmeti** bölmesi.
5. Seçin **erişim anahtarları** sol bölmesi ve depolama hesabı adını ve değerini Kopyala **key1**. Bu değerleri Not Defteri'ne veya başka bir geçici konuma kaydedin.

## <a name="create-a-python-script-to-send-events-to-your-event-hub"></a>Olay hub'ınıza olayları göndermek için bir Python betiği oluşturma
1. En sevdiğiniz Python düzenleyiciniz gibi açın [Visual Studio Code][Visual Studio Code].
2. Adlı bir betik oluşturma **sender.py**. Bu betik, 200 olayları olay hub'ınıza gönderir. JSON'da gönderilen basit ortam okumalar değildirler.
3. Sender.py aşağıdaki kodu yapıştırın:
   
   ```python
   import uuid
   import datetime
   import random
   import json
   from azure.servicebus.control_client import ServiceBusService
   
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
4. Yukarıdaki kod, ad alanı adı, anahtar değeri ve Event Hubs ad alanını oluştururken elde ettiğiniz bir olay hub adı kullanacak şekilde güncelleştirin.

## <a name="create-a-python-script-to-read-your-capture-files"></a>Yakalama dosyaları okumak için bir Python betiği oluşturma

1. Bölmesi ve select doldurmak **Oluştur**.
2. Adlı bir betik oluşturma **capturereader.py**. Bu betik, yakalanan dosyalarını okur ve bu cihaz için yalnızca veri yazmak için cihaz başına bir dosya oluşturur.
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
4. Depolama hesabı adı ve anahtarı için uygun değerleri çağrısında yapıştırın `startProcessing`.

## <a name="run-the-scripts"></a>Betikleri çalıştırma
1. Python alt yolu olan bir komut istemi açın ve sonra Python önkoşul paketleri yüklemek için şu komutları çalıştırın:
   
   ```
   pip install azure-storage
   pip install azure-servicebus
   pip install avro
   ```
   
   Ya da daha önceki bir sürümüne sahipseniz **azure depolama** veya **azure**, kullanmanız gerekebilir **--yükseltme** seçeneği.
   
   (Çoğu sistemlerinde gerekli değildir) aşağıdaki komutu çalıştırın gerekebilir:
   
   ```
   pip install cryptography
   ```
2. Yerde sender.py ve capturereader.py kaydettiğiniz için dizininize geçin ve şu komutu çalıştırın:
   
   ```
   start python sender.py
   ```
   
   Bu komut, gönderen çalıştırmak için yeni bir Python işlemi başlatır.
3. Yakalama çalıştırmak birkaç dakika bekleyin. Ardından, özgün komut penceresine aşağıdaki komutu yazın:
   
   ```
   python capturereader.py
   ```

   Bu yakalama işlemci, depolama hesabı/kapsayıcısından tüm blobları indirmek için yerel bir dizin kullanır. Boş olmayan tüm işler ve sonuçları yerel bir dizine .csv dosyaları olarak yazar.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları kullanarak Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs'a genel bakış yakalama][Overview of Event Hubs Capture]
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Event Hubs'a genel bakış][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
