---
title: Python kullanarak Azure Event Hubs için olayları gönderme | Microsoft Docs
description: Python kullanarak Event Hubs'a olay göndermeye başlama
services: event-hubs
author: sethmanheim
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 07/26/2018
ms.author: sethm
ms.openlocfilehash: 762e21cfc7d16b614eb637c569f8bfc5b6115db1
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43703853"
---
# <a name="send-events-to-event-hubs-using-python"></a>Python kullanarak Event Hubs için olayları gönderme

Azure Event Hubs, milyonlarca işleyebilen ileri düzeyde ölçeklenebilir bir olay yönetim sistemi saniyede olayları, işlemek ve çok büyük miktardaki verileri çözümlemek uygulamaları etkinleştirme bağlı cihazlarınız ve diğer sistemleri tarafından üretilen. Bir olay hub'ına toplandıktan sonra almak ve işlem içi işleyicileri kullanarak olayları işleme veya diğer analiz sistemleri ileterek.

Event Hubs hakkında daha fazla bilgi için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

Bu öğreticide, Python'da yazılmış bir uygulamadan bir olay hub'ına olayları göndermek nasıl açıklar. Olayları almak için bkz: [karşılık gelen alma makale](event-hubs-python-get-started-receive.md).

Bu öğreticideki kod öğesinden alınır [bu GitHub örneklerine](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), hangi tam görmek için inceleyebilirsiniz içeri aktarma deyimlerini ve değişken bildirimleri dahil olmak üzere çalışan bir uygulama. Diğer örnekler aynı GitHub klasöründe kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Python 3.4 veya üzeri.
- Mevcut bir Event Hubs ad alanı ve olay hub '. Yönergeleri izleyerek bu varlıkları oluşturabilirsiniz [bu makalede](event-hubs-create.md). 

[!INCLUDE [create-account-note](../../includes/create-account-note.md)]


## <a name="install-python-package"></a>Python paketini yükle

Event Hubs için Python paketini yüklemek için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-send-events"></a>Olayları göndermek için bir Python betiği oluşturma

Ardından, olay hub'ına olayları gönderen bir Python uygulaması oluşturun:

1. En sevdiğiniz Python düzenleyiciniz gibi açın [Visual Studio Code][Visual Studio Code].
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

## <a name="send-events"></a>Olayları gönderme

Betiği çalıştırmak için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın:

```bash
start python send.py
```
 
## <a name="next-steps"></a>Sonraki adımlar

Almak için Python kullanarak bir olay hub'ına olayları gönderdik, olayları görmek [karşılık gelen alma makale](event-hubs-python-get-started-receive.md).

Event Hubs hakkında daha fazla bilgi için şu sayfaları ziyaret edin:

* [Event Hubs'a genel bakış][Event Hubs overview]
* [Olay Hub’ı oluşturma](event-hubs-create.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
