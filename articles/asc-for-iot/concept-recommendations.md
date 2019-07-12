---
title: Azure Güvenlik Merkezi Önizleme için IOT güvenlik önerilerini anlama | Microsoft Docs
description: Kavramını güvenlik önerilerini ve IOT için Azure Güvenlik Merkezi'nde nasıl kullanıldığı hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 02ced504-d3aa-4770-9d10-b79f80af366c
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/24/2019
ms.author: mlottner
ms.openlocfilehash: 1ee71bbacdba7a14e94de41563a04be9c0f00d13
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67618403"
---
# <a name="security-recommendations"></a>Güvenlik önerileri

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IOT için Azure Güvenlik Merkezi (ASC), Azure kaynaklarını ve IOT cihazları tarar ve saldırı yüzeyinizi azaltmak için güvenlik önerileri sağlar. Güvenlik önerileri, eyleme dönüştürülebilir ve uymak için en iyi güvenlik uygulamalarını belirleyin müşterilerine yardımcı olmak için hedeflenir.

Bu makalede, IOT Hub ve/veya IOT cihazları üzerinde tetiklenebilen öneriler listesini bulabilirsiniz.

## <a name="recommendations-for-iot-devices"></a>IOT cihazları için öneriler

Cihaz öneriler, Öngörüler ve cihaz güvenliğini artırmak için önerileri sağlar. 

| Severity | Ad                                                      | Veri Kaynağı | Açıklama                                                                                                                                                                                           |
|----------|-----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Orta   | Cihaz üzerindeki bağlantı noktalarını Aç                                      | Aracı       | Cihazda dinleyen bir uç noktası bulunamadı                                                                                                                                                          |
| Orta   | Esnek güvenlik duvarı ilkesi zincirleri biri bulunamadı. | Aracı       | Güvenlik duvarı ilkesi bulunamadı (girdi/çıktı) izin verilir. Güvenlik Duvarı İlkesi varsayılan olarak tüm trafiği engellemek ve cihaz öğesine/öğesinden gerekli iletişime izin vermek için kuralları tanımlar gerekir.                               |
| Orta   | Giriş zincirindeki izin veren güvenlik duvarı kuralı bulunamadı     | Aracı       | Çok çeşitli IP adres veya bağlantı noktası için esnek bir düzen içeren bir güvenlik duvarı kuralında bulundu.                                                                                    |
| Orta   | Çıkış zincirindeki izin veren güvenlik duvarı kuralı bulunamadı    | Aracı       | Çok çeşitli IP adres veya bağlantı noktası için esnek bir düzen içeren bir güvenlik duvarı kuralında bulundu.                                                                                   |
| Orta   | Sistem taban çizgisi doğrulama işlemi başarısız oldu           | Aracı       | Cihaz uyumlu değil [CIS Linux değerlendirmeleri](https://www.cisecurity.org/cis-benchmarks/)                                                                                                         |

### <a name="operational-recommendations-for-iot-devices"></a>IOT cihazlarının işletimsel önerileri

İşletimsel öneriler, Öngörüler ve güvenlik aracı yapılandırması geliştirmek için öneriler sağlar.

| Severity | Ad                                    | Veri Kaynağı | Açıklama                                                                       |
|----------|-----------------------------------------|-------------|-----------------------------------------------------------------------------------|
| Düşük      | Aracı unutilized iletileri gönderir          | Aracı       | Son 24 saat, % 10 veya daha fazla güvenlik iletilerinin 4 KB'den küçük.  |
| Düşük      | Güvenlik ikizi yapılandırma en iyi değil | Aracı       | Güvenlik ikizi yapılandırma en iyi değil.                                        |
| Düşük      | Güvenlik ikizi yapılandırma çakışması    | Aracı       | Çakışmaları güvenlik ikizi yapılandırmada tanımlandı.                           |


## <a name="recommendations-for-iot-hub"></a>IOT hub'ına yönelik öneriler

Öneri uyarılar, içgörü ve ortamın güvenlik duruşunu eylemleri için öneriler sağlar.  

| Severity | Ad                                                     | Veri Kaynağı | Açıklama                                                                                                                                                                                                             |
|----------|----------------------------------------------------------|-------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Yüksek     | Birden çok cihazlar tarafından kullanılan aynı kimlik doğrulama kimlik bilgileri | IoT Hub     | IOT Hub kimlik doğrulama bilgilerini birden çok cihaz tarafından kullanılır. Bu yasal bir cihaz kimliğine bürünme aykırı bir cihaz gösterebilir. Yinelenen kimlik bilgisi kullanımı tarafından kötü amaçlı bir aktör cihaz kimliğe bürünme riskini artırır. |
| Orta   | Varsayılan IP Filtresi İlkesi olması Reddet                  | IoT Hub     | IP Filtresi yapılandırmasını, trafiğe izin verilmiyor ve varsayılan olarak gereken tanımlanmış kuralınız yok, varsayılan olarak diğer tüm trafiği reddetmeye.                                                                                                     |
| Orta   | IP filtresi kuralı büyük IP aralığı içerir                   | IoT Hub     | Bir izin IP filtresi kuralı kaynak IP aralığı çok büyük. Aşırı izin veren kuralları kötü amaçlı aktörler için IOT hub'ınıza kullanıma sunabilirsiniz.                                                                                       |
| Düşük      | IOT hub'ında tanılama günlüklerini etkinleştirme                       | IoT Hub     | Günlükleri etkinleştirmek ve bunlar için bir yıla kadar Beklet. Günlük tutma bir güvenlik olayı ortaya veya ağınızın tehlikeye araştırma amacıyla etkinlik kayıtlarını yeniden olanak tanır.                                       |
|