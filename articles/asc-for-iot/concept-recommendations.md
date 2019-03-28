---
title: ASC Önizleme için IOT güvenlik önerilerini anlama | Microsoft Docs
description: Güvenlik önerilerini ve artan düzende IOT için almaları kavramı hakkında bilgi edinin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 02ced504-d3aa-4770-9d10-b79f80af366c
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: 1e4582d93d1e3380ecdabdb241f27839d4da4565
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541865"
---
# <a name="security-recommendations"></a>Güvenlik önerileri

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Devam eden çözüm analize dayalı olarak, IOT için ASC artırmak ve cihazlarınızı, işlemsel durum ve genel IOT hub'ı ortamının korunmasına yardımcı olmak için gereken kullanırken aşağıdaki önerilerileri sağlar. 


## <a name="device-recommendations"></a>Cihaz önerileri

Cihaz öneriler, Öngörüler ve cihaz güvenliğini ve davranışını geliştirmek için öneriler sağlar. 

| Severity | Ad                                                      | Veri Kaynağı | Açıklama                                                                                                                                                                                           |
|----------|-----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Orta   | Cihaz üzerindeki bağlantı noktalarını Aç                                      | Aracı       | Cihazda bir dinleme uç noktası bulundu                                                                                                                                                          |
| Orta   | Esnek güvenlik duvarı ilkesi zincirleri biri bulunamadı. | Aracı       | Güvenlik duvarı ilkesi bulunamadı (girdi/çıktı) izin verilir. Güvenlik Duvarı İlkesi varsayılan olarak tüm trafiği engellemek ve cihaz öğesine/öğesinden gerekli iletişime izin vermek için kuralları tanımlar gerekir.                               |
| Orta   | Giriş zincirindeki izin veren güvenlik duvarı kuralı bulunamadı     | Aracı       | Çok çeşitli IP adres veya bağlantı noktası için esnek bir düzen içeren bir güvenlik duvarı kuralında bulundu.                                                                                    |
| Orta   | Çıkış zincirindeki izin veren güvenlik duvarı kuralı bulunamadı    | Aracı       | Çok çeşitli IP adres veya bağlantı noktası için esnek bir düzen içeren bir güvenlik duvarı kuralında bulundu.                                                                                   |
| Orta   | Sistem taban çizgisi doğrulama işlemi başarısız oldu           | Aracı       | Cihaz uyumlu değil [CIS Linux değerlendirmeleri](https://www.cisecurity.org/cis-benchmarks/)                                                                                                         |

### <a name="operational-recommendation"></a>İşletimsel önerisi

İşletimsel öneriler, Öngörüler ve aracı yapılandırması geliştirmek için öneriler sağlar.

| Severity | Ad                                    | Veri Kaynağı | Açıklama                                                                       |
|----------|-----------------------------------------|-------------|-----------------------------------------------------------------------------------|
| Düşük      | Aracı unutilized iletileri gönderir          | Aracı       | Son 24 saat, % 10 veya daha fazla güvenlik iletilerinin 4 KB'den küçük.  |
| Düşük      | Güvenlik ikizi yapılandırma en iyi değil | Aracı       | Güvenlik ikizi yapılandırma en iyi değil.                                        |
| Düşük      | Güvenlik ikizi yapılandırma çakışması    | Aracı       | Çakışmaları güvenlik ikizi yapılandırmada tanımlandı.                           |


## <a name="iot-hub-recommendations"></a>IOT hub'ı önerileri

Öneri uyarılar, içgörü ve ortamın güvenlik duruşunu eylemleri için öneriler sağlar.  

| Severity | Ad                                                     | Veri Kaynağı | Açıklama                                                                                                                                                                                                             |
|----------|----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yüksek     | Birden çok cihazlar tarafından kullanılan aynı kimlik doğrulama kimlik bilgileri | IoT Hub     | IOT Hub kimlik doğrulama bilgilerini birden çok cihaz tarafından kullanılır. Bu yasal bir cihaz kimliğine bürünme aykırı bir cihaz gösterebilir. Yinelenen kimlik bilgisi kullanımı tarafından kötü amaçlı bir aktör cihaz kimliğe bürünme riskini artırır. |
| Orta   | Varsayılan IP Filtresi İlkesi olması Reddet                  | IoT Hub     | IP Filtresi yapılandırmasını, trafiğe izin verilmiyor ve varsayılan olarak gereken tanımlanmış kuralınız yok, varsayılan olarak diğer tüm trafiği reddetmeye.                                                                                                     |
| Orta   | IP filtresi kuralı büyük IP aralığı içerir                   | IoT Hub     | Bir izin IP filtresi kuralı kaynak IP aralığı çok büyük. Aşırı izin veren kuralları kötü amaçlı aktörler için IOT hub'ınıza kullanıma sunabilirsiniz.                                                                                       |
| Düşük      | IoT Hub'da tanılama günlüklerini etkinleştirin                       | IoT Hub     | Günlükleri etkinleştirmek ve bunlar için bir yıla kadar Beklet. Günlük tutma bir güvenlik olayı ortaya veya ağınızın tehlikeye araştırma amacıyla etkinlik kayıtlarını yeniden olanak tanır.                                       |
|