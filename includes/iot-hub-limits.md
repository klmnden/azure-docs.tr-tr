---
author: robinsh
ms.author: robinsh
ms.service: iot-hub
ms.topic: include
ms.date: 10/26/2018
ms.openlocfilehash: 1bdf73dc6a4edf0c170b51e70fca2128d22e0eb8
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59805369"
---
Aşağıdaki tabloda farklı hizmet katmanlarında S1, S2, S3 ve F1 ile ilişkili sınırlar listelenmektedir. Her birinin maliyeti hakkında bilgi için *birim* her katmanında bkz [Azure IOT Hub fiyatlandırması](https://azure.microsoft.com/pricing/details/iot-hub/).

| Kaynak | S1 Standart | S2 Standart | S3 Standart | F1 Ücretsiz |
| --- | --- | --- | --- | --- |
| İleti/gün |400,000 |6,000,000 |300,000,000 |8,000 |
| En fazla birim |200 |200 |10 |1 |

> [!NOTE]
> Bir S1 ve S2 katmanı hub veya bir S3 katmanı hub'ı ile 10 birim ile 200'den fazla birimini kullanmayı düşünüyorsanız, Microsoft Support başvurun.
> 
> 

Aşağıdaki tabloda, IOT Hub kaynakları için geçerli olan sınırlar listelenmektedir.

| Kaynak | Sınır |
| --- | --- |
| Azure aboneliği başına en fazla ücretli IoT hub’ı sayısı |50 |
| Azure aboneliği başına en fazla ücretsiz IoT hub’ı sayısı |1 |
| Bir cihaz kimliği karakter sayısı | 128 |
| En fazla cihaz kimliği sayısı<br/> (tek bir çağrıda döndürülen) |1000 |
| Cihazdan buluta iletiler için IoT Hub en fazla ileti bekletme süresi |7 gün |
| Cihazdan bulut iletinin en büyük boyutu |256 KB |
| Cihazdan buluta toplu işin en büyük boyutu |AMQP ve HTTP: Toplu işin tamamını için 256 KB <br/>MQTT: Her ileti için 256 KB |
| Cihazdan buluta toplu işte en fazla ileti sayısı |500 |
| Buluttan cihaza iletinin en büyük boyutu |64 KB |
| Buluttan cihaza iletiler için en büyük TTL |2 gün |
| Buluttan cihaza iletiler için en fazla teslim <br/> sayısı |100 |
| Geri bildirim iletileri için en fazla teslim sayısı <br/> (buluttan cihaza iletiye yanıt olarak) |100 |
| Buluttan cihaza iletiye yanıt olarak <br/> geri bildirim iletileri için en fazla TTL |2 gün |
| En büyük cihaz ikizi boyutu <br/> (etiketler, rapor edilen özellikler ve istenen özellikler) | 8 KB |
| Cihaz ikizi dize değerinin en büyük boyutu | 4 KB |
| Cihaz ikizindeki en büyük nesne derinliği | 5 |
| Doğrudan yöntem yükünün en büyük boyutu | 128 KB |
| En büyük iş geçmişi bekletme süresi | 30 gün |
| En fazla eşzamanlı iş sayısı | 10 (S3 için), 5 (S2 için), 1 (S1 için) |
| En fazla ek uç nokta sayısı | 10 (S1, S2 ve S3) |
| En fazla ileti yönlendirme kuralı sayısı | 100 (S1, S2 ve S3) |
| En fazla eşzamanlı olarak bağlı cihaza akış sayısı | 50 (S1, S2, S3 ve yalnızca F1) |
| En fazla cihaz akış veri aktarımı | 300 MB / gün (için S1, S2, S3 ve yalnızca F1) |


> [!NOTE]
> 50'den fazla Ücretli IOT hub'ı bir Azure aboneliğine ihtiyacınız varsa, Microsoft Support başvurun.


> [!NOTE]
> Şu anda, cihazları tek bir IOT hub'ına bağlanabilir sayısı 1.000.000 ' dir. Bu sınırı artırmak istiyorsanız, ilgili kişi [Microsoft Support](https://azure.microsoft.com/support/options/).

Aşağıdaki kotalar aşıldığında IOT hub'ı istekleri kısıtlar.

| Kısıtlama | Hub başına değer |
| --- | --- |
| Kimlik kayıt defteri işlemleri <br/> (oluşturma, Al, Listele, güncelleştirme ve silme), <br/> tek tek veya toplu içeri/dışarı aktarma |(S3 için) 83.33/sec/Unit (5000/dk/birim). <br/> (100/dk/birim) 1.67/sec/Unit (için S1 ve S2 için). |
| Cihaz bağlantıları |6.000/sn/birim (S3 için), 120/sn/birim (S2 için), 12/sn/birim (S1 için). <br/>En az 100/sn. |
| Cihazdan buluta gönderim |6.000/sn/birim (S3 için), 120/sn/birim (S2 için), 12/sn/birim (S1 için). <br/>En az 100/sn. |
| Buluttan cihaza gönderim | (S1 ve S2 için için) 83.33/sec/Unit (5000/dk/birim) (S3 için), 1.67/sec/unit (100/dk/birim). |
| Buluttan cihaza alım |(S1 ve S2 için için) 833.33/sec/Unit (50.000/dk/birim) (S3 için), 16.67/sec/unit (1000/dk/birim). |
| Dosya karşıya yükleme işlemleri |83.33 dosya karşıya yükleme bildirimi/sn/birim (5000/dk/birim) (S3 için), 1,67 dosya karşıya yükleme bildirimler/sn/birim (100/dk/birim) (S1 ve S2 için). <br/> 10.000 SAS URI'leri bir Azure depolama hesabı için tek seferde çıkarılabilir.<br/> Tek seferde 10 SAS URI’si/cihaz çıkarılabilir. |
| Doğrudan yöntemler | 24 MB/sn/birim (S3 için), 480 KB/sn/birim (S2 için), 160 KB/sn/birim (S1 için).<br/> 8 KB'lık azaltma ölçüm boyutuna göre. |
| Cihaz ikizi okumaları | 500/sn/birim (S3 için), en fazla 100/sn veya 10/sn/birim (S2 için), 100/sn (S1 için) |
| Cihaz ikizi güncelleştirmeleri | 250/sn/birim (S3 için), en fazla 50/sn veya 5/sn/birim (S2 için), 50/sn (S1 için) |
| İş işlemleri <br/> (oluşturma, liste, güncelleştirme ve silme) | 83.33/sec/Unit (5000/dk/birim) (S3 için), 1.67/sec/unit (100/dk/birim) (S2 için), (S1 için) 1.67/sec/unit (100/dk/birim). |
| İşler işlemleri için cihaz başına aktarım hızı | 50/sn/birim (S3 için), en fazla 10/sn veya 1/sn/birim (S2 için), 10/sn (S1 için). |
| Cihaz akış başlatma hızı | 5 yeni akışlar/sn (S1, S2, S3 ve yalnızca F1). |
