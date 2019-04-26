---
title: Gönderme veya Python - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makale, Azure Event Hubs'a olayları gönderen bir Python uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 04/15/2019
ms.author: shvija
ms.openlocfilehash: 6a8f0ddcfe6de904219059c6e761ead4c004732d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60461459"
---
# <a name="send-events-to-or-receive-events-from-event-hubs-using-python"></a>Olayları göndermek veya Python kullanarak Event Hubs'dan olayları alma

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğreticide olayları gönderip olayları bir event hub'ından Python uygulamalarının nasıl oluşturulacağını açıklar. 

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs-python/tree/master/examples)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- Python 3.4 veya üzeri.
- Kullanım [Azure portalında](https://portal.azure.com) Event Hubs türünde bir ad alanı oluşturma, ardından uygulamanızın olay hub'ı ile iletişim kurmak için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md). Ardından, makaledeki yönergeleri izleyerek olay hub'ı erişim anahtarının değerini alın: [Bağlantı dizesini alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticinin ilerleyen bölümlerinde yazdığınız kod erişim anahtarını kullanın. Varsayılan anahtar adı şöyledir: **RootManageSharedAccessKey**.

## <a name="install-python-package"></a>Python paketini yükle

Event Hubs için Python paketini yüklemek için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın: 

```bash
pip install azure-eventhub
```

## <a name="send-events"></a>Olayları gönderme

### <a name="create-a-python-script-to-send-events"></a>Olayları göndermek için bir Python betiği oluşturma

Ardından, olay hub'ına olayları gönderen bir Python uygulaması oluşturun:

1. En sevdiğiniz Python düzenleyiciniz gibi açın [Visual Studio Code](https://code.visualstudio.com/)
2. Adlı bir betik oluşturma **send.py**. Bu betik, 100 olayları olay hub'ınıza gönderir.
3. Send.py, adresi, kullanıcı ve anahtar değerlerini önceki bölümde, Azure portalından aldığınız değerlerle değiştirerek aşağıdaki kodu yapıştırın: 

```python
import sys
import logging
import datetime
import time
import os

from azure.eventhub import EventHubClient, Sender, EventData

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"

try:
    if not ADDRESS:
        raise ValueError("No EventHubs URL supplied.")

    # Create Event Hubs client
    client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
    sender = client.add_sender(partition="0")
    client.run()
    try:
        start_time = time.time()
        for i in range(100):
            print("Sending message: {}".format(i))
            sender.send(EventData(str(i)))
    except:
        raise
    finally:
        end_time = time.time()
        client.stop()
        run_time = end_time - start_time
        logger.info("Runtime: {} seconds".format(run_time))

except KeyboardInterrupt:
    pass
```

### <a name="run-application-to-send-events"></a>Olayları göndermek için uygulamayı çalıştırma

Betiği çalıştırmak için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın:

```bash
start python send.py
```

Tebrikler! Bir olay hub'ına ileti gönderdiniz.

## <a name="receive-events"></a>Olayları alma

### <a name="create-a-python-script-to-receive-events"></a>Olaylarını almak için bir Python betiği oluşturma

Ardından, olayları bir event hub'ından alan Python uygulamasını oluşturun:

1. En sevdiğiniz Python düzenleyiciniz gibi açın [Visual Studio Code](https://code.visualstudio.com/)
2. Adlı bir betik oluşturma **recv.py**.
3. Recv.py, adresi, kullanıcı ve anahtar değerlerini önceki bölümde, Azure portalından aldığınız değerlerle değiştirerek aşağıdaki kodu yapıştırın: 

```python
import os
import sys
import logging
import time
from azure.eventhub import EventHubClient, Receiver, Offset

logger = logging.getLogger("azure")

# Address can be in either of these formats:
# "amqps://<URL-encoded-SAS-policy>:<URL-encoded-SAS-key>@<mynamespace>.servicebus.windows.net/myeventhub"
# "amqps://<mynamespace>.servicebus.windows.net/myeventhub"
# For example:
ADDRESS = "amqps://mynamespace.servicebus.windows.net/myeventhub"

# SAS policy and key are not required if they are encoded in the URL
USER = "RootManageSharedAccessKey"
KEY = "namespaceSASKey"
CONSUMER_GROUP = "$default"
OFFSET = Offset("-1")
PARTITION = "0"

total = 0
last_sn = -1
last_offset = "-1"
client = EventHubClient(ADDRESS, debug=False, username=USER, password=KEY)
try:
    receiver = client.add_receiver(CONSUMER_GROUP, PARTITION, prefetch=5000, offset=OFFSET)
    client.run()
    start_time = time.time()
    for event_data in receiver.receive(timeout=100):
        last_offset = event_data.offset
        last_sn = event_data.sequence_number
        print("Received: {}, {}".format(last_offset, last_sn))
        total += 1

    end_time = time.time()
    client.stop()
    run_time = end_time - start_time
    print("Received {} messages in {} seconds".format(total, run_time))

except KeyboardInterrupt:
    pass
finally:
    client.stop()
```

### <a name="receive-events"></a>Olayları alma

Betiği çalıştırmak için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın:

```bash
start python recv.py
```
 
## <a name="next-steps"></a>Sonraki adımlar
Bu makaleleri okuyun:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Özellikler ve Azure Event Hubs terminolojisinde](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)

