---
title: Python - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, Azure Event Hubs'tan gelen olayları alır bir Python uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: ShubhaVijayasarathy
manager: femila
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 11/26/2018
ms.author: shvija
ms.openlocfilehash: bc1cf07c5a74bc4d7182eea5281e75525fd04247
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53103193"
---
# <a name="receive-events-from-event-hubs-using-python"></a>Python kullanarak Event Hubs'dan olayları alma

Azure Event Hubs, milyonlarca işleyebilen ileri düzeyde ölçeklenebilir bir olay yönetim sistemi saniyede olayları, işlemek ve çok büyük miktardaki verileri çözümlemek uygulamaları etkinleştirme bağlı cihazlarınız ve diğer sistemleri tarafından üretilen. Bir olay hub'ına toplandıktan sonra almak ve işlem içi işleyicileri kullanarak olayları işleme veya diğer analiz sistemleri ileterek.

Event Hubs hakkında daha fazla bilgi için bkz: [Event Hubs'a genel bakış][Event Hubs overview].

Bu öğretici, olayları bir event hub Python'da yazılmış bir uygulamadan alacak şekilde açıklar. Olayları göndermek için bkz: [karşılık gelen gönderme makale](event-hubs-python-get-started-send.md).

Bu öğreticideki kod öğesinden alınır [bu GitHub örneklerine](https://github.com/Azure/azure-event-hubs-python/tree/master/examples), hangi tam görmek için inceleyebilirsiniz içeri aktarma deyimlerini ve değişken bildirimleri dahil olmak üzere çalışan bir uygulama. Diğer örnekler aynı GitHub klasöründe kullanılabilir.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap oluşturun](https://azure.microsoft.com/free/).
- Python 3.4 veya üzeri.
- Mevcut bir Event Hubs ad alanı ve olay hub '. Yönergeleri izleyerek bu varlıkları oluşturabilirsiniz [bu makalede](event-hubs-create.md). 

## <a name="install-python-package"></a>Python paketini yükle

Event Hubs için Python paketini yüklemek için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın: 

```bash
pip install azure-eventhub
```

## <a name="create-a-python-script-to-receive-events"></a>Olaylarını almak için bir Python betiği oluşturma

Ardından, olayları bir event hub'ından alan Python uygulamasını oluşturun:

1. En sevdiğiniz Python düzenleyiciniz gibi açın [Visual Studio Code][Visual Studio Code].
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

## <a name="receive-events"></a>Olayları alma

Betiği çalıştırmak için Python alt yolu olan bir komut istemi açın ve ardından bu komutu çalıştırın:

```bash
start python recv.py
```
 
## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, bir olay hub'ından iletiler alan bir Python uygulaması oluşturdunuz. Python kullanarak bir olay hub'ına olay gönderme hakkında bilgi edinmek için bkz: [olayları event hub'dan - Python Gönder](event-hubs-python-get-started-send.md).

<!-- Links -->
[Event Hubs overview]: event-hubs-about.md
[Visual Studio Code]: https://code.visualstudio.com/
[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
