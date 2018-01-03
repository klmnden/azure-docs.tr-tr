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
ms.openlocfilehash: cdbb2baea2bc6c45908369ff821c264b66053d95
ms.sourcegitcommit: 6f33adc568931edf91bfa96abbccf3719aa32041
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="event-hubs-capture-walkthrough-python"></a>Olay hub'ları yakalama izlenecek yol: Python

Olay hub'ları yakalama Event hubs, tercih ettiğiniz bir Azure Blob Depolama hesabına olay hub'ınızı akış verilerini bir otomatik olarak göndermenizi sağlayan bir özelliktir. Bu özellik toplu gerçek zamanlı akış verilerini işleme gerçekleştirmek kolaylaştırır. Bu makalede, olay hub'ları yakalama Python ile kullanmayı açıklar. Olay hub'ları yakalama hakkında daha fazla bilgi için bkz: [genel bakış makalesi](event-hubs-capture-overview.md).

Bu örnekte [Azure Python SDK'sı](https://azure.microsoft.com/develop/python/) yakalama özelliği göstermek için. Sender.py program sanal ortam telemetriyi JSON biçiminde Event Hubs'a gönderir. Olay hub'ı, bu verileri BLOB depolamaya yığınlardaki yazmak için yakalama özelliği kullanmak için yapılandırılır. Capturereader.py uygulama bu BLOB'lar okur ve cihaz başına bir ekleme dosyası oluşturur, sonra verileri .csv dosyasına yazar.

## <a name="what-will-be-accomplished"></a>Ne elde edilecek

1. Bir Azure Blob Storage hesabı ve Azure portalını kullanarak bir blob kapsayıcısına, oluşturun.
2. Azure Portalı'nı kullanarak bir Event Hub'ad alanı oluşturun.
3. Bir event hub yakalama özelliği etkin, Azure portalını kullanarak oluşturun.
4. Olay hub'ına Python betiği ile verileri gönderin.
5. Yakalama dosyaları okumasına ve başka bir Python komut dosyasıyla işlem.

## <a name="prerequisites"></a>Önkoşullar

- Python 2.7.x
- Bir Azure aboneliği
- Etkin bir [olay hub'ları ad alanı ve olay hub'ı.](event-hubs-create.md)

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="create-an-azure-storage-account"></a>Azure Depolama hesabı oluşturma
1. [Azure portalında][Azure portal] oturum açın.
2. Portalın sol gezinti bölmesinde tıklatın **yeni**, ardından **depolama**ve ardından **depolama hesabı**.
3. Depolama hesabı dikey penceresindeki alanları doldurun ve ardından **oluşturma**.
   
   ![][1]
4. Gördükten sonra **dağıtımları başarılı** ileti, yeni depolama hesabı adını ve buna tıklayın **Essentials** dikey penceresinde tıklatın **BLOB'lar**. Zaman **Blob hizmeti** dikey penceresi açıldığında, tıklatın **+ kapsayıcı** üstünde. Kapsayıcı adı **yakalama**, ardından Kapat **Blob hizmeti** dikey.
5. Tıklatın **erişim anahtarları** Sol dikey kopyalayıp depolama hesabı adını ve değerini **key1**. Bu değerleri Not Defteri'nde veya başka bir geçici konuma kaydedin.

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

1. Dikey ve tıklatın doldurmak **oluşturma**.
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
          #content_length == 508 is an empty file, so only process content_length > 508 i.e. skip  empty files
          if blob.properties.content_length > 508:
              print('Downloaded a non empty blob: ' + blob.name)
              cleanName = string.replace(blob.name, '/', '_')
              block_blob_service.get_blob_to_path(container, blob.name, cleanName)
              processBlob(cleanName)
              os.remove(cleanName)
          block_blob_service.delete_blob(container, blob.name)
  startProcessing('YOUR STORAGE ACCOUNT NAME', 'YOUR KEY', 'capture')
  ```
4. Çağrısında, depolama hesabı adı ve anahtarınız için uygun değerleri yapıştırmak mutlaka `startProcessing`.

## <a name="run-the-scripts"></a>Komut dosyalarını çalıştır
1. Python kendi yolunda sahip bir komut istemi açın ve Python önkoşul paketleri yüklemek için şu komutları çalıştırın:
   
  ```
  pip install azure-storage
  pip install azure-servicebus
  pip install avro
  ```
   
  Azure-storage veya azure önceki bir sürümü varsa, kullanmanız gerekebilir **--yükseltme** seçeneği
   
  Aşağıdaki komutu çalıştırarak gerekebilir (çoğu sistemlerde gerekli değildir):
   
  ```
  pip install cryptography
  ```
2. Dizininizin açtıkları her yerde sender.py ve capturereader.py kaydettiğiniz için değiştirmek ve bu komutu çalıştırın:
   
  ```
  start python sender.py
  ```
   
  Bu komut, gönderenin çalıştırmak için yeni bir Python işlemi başlatır.
3. Şimdi çalıştırmak yakalama için bir kaç dakika bekleyin. Ardından, özgün komut penceresine aşağıdaki komutu yazın:
   
   ```
   python capturereader.py
   ```

   Bu yakalama işlemci yerel dizin depolama hesabı/kapsayıcıdan BLOB indirmek için kullanır. Boş olmayan tüm işler ve sonuçları yerel dizine .csv dosyaları olarak yazar.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki bağlantıları inceleyerek Event Hubs hakkında daha fazla bilgi edinebilirsiniz:

* [Event Hubs'a genel bakış yakalama][Overview of Event Hubs Capture]
* [Event Hubs kullanan örnek uygulamalar](https://github.com/Azure/azure-event-hubs/tree/master/samples)
* [Event Hubs'a genel bakış][Event Hubs overview]

[Azure portal]: https://portal.azure.com/
[Overview of Event Hubs Capture]: event-hubs-capture-overview.md
[1]: ./media/event-hubs-archive-python/event-hubs-python1.png
[About Azure storage accounts]:../storage/common/storage-create-storage-account.md
[Visual Studio Code]: https://code.visualstudio.com/
[Event Hubs overview]: event-hubs-what-is-event-hubs.md
