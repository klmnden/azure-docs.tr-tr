---
title: ASC IOT Önizleme kullanarak verilere erişilmesi | Microsoft Docs
description: ASC IOT için kullanılırken, güvenlik uyarısı ve öneri veri erişim hakkında bilgi edinin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: fbd96ddd-cd9f-48ae-836a-42aa86ca222d
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: e394f6025f7898aad7dde7b1acefd9f95029a554
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58542000"
---
# <a name="access-your-security-data"></a>Güvenlik verilerinize erişin 

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

(Kaydetmeyi seçerseniz) ASC IOT için Log Analytics çalışma alanınızda güvenlik uyarıları, öneriler ve güvenlik ham verileri depolar.

## <a name="log-analytics"></a>Log Analytics

Hangi Log Analytics çalışma alanı kullanılan yapılandırmak için:

1. IOT hub'ınızı açın.
1. Tıklayın **güvenlik**
2. Tıklayın **ayarları**ve Log Analytics çalışma alanı yapılandırmanızı değiştirin.

Yapılandırmadan sonra Log Analytics çalışma alanınızın erişmek için:

1. Uyarı, IOT için artan düzende seçin. 
2. Tıklayın **daha fazla araştırma**, ardından **buraya tıklayın ve cihaz kimliği sütunu görüntüleyin, bu uyarıyı hangi cihazların görmek için**.

Log Analytics verilerini sorgulama hakkında daha fazla bilgi için bkz [sorgular, Log Analytics ile çalışmaya başlama](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries).

## <a name="security-alerts"></a>Güvenlik uyarıları

Güvenlik Uyarıları depolanır **ASCforIoT.SecurityAlert** yapılandırılmış bir Log Analytics çalışma alanınız içinde tablo.

Güvenlik Uyarıları keşfetmeye başlamak için aşağıdaki temel kql sorguları kullanın.

### <a name="sample-records-query"></a>Örnek kayıtları sorgusu

Rastgele birkaç kayıtları seçmek için: 

```
// Select a few random records
//
SecurityAlert
| project 
    TimeGenerated, 
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity, 
    DisplayName,
    Description,
    ExtendedProperties
| take 3
```

#### <a name="sample-query-results"></a>Örnek sorgu sonuçları 

| TimeGenerated           | IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Açıklama                                             | ExtendedProperties                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Başarılı deneme yanılma saldırısı           | Cihazda bir deneme yanılma yoluyla yapılan zorla saldırısı başarılı oldu        |    {"Tam kaynak Address": "[\"10.165.12.18:\"]", "Kullanıcı adlarını": "[\"\"]", "DeviceId": "IOT-cihaz-Linux"}                                                                       |
| 2018-11-19T12:40:31.000 | /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Cihazda yerel başarılı oturum açma      | Cihaza başarılı bir yerel oturum algılandı     | {"Uzak adres": "?", "Uzak bağlantı noktası": "", "Yerel bağlantı noktası": "", "Oturum açma kabuğu": "/ bin/su", "Oturum açma işlemi Id": "28207", "kullanıcı adı": "saldırgan", "DeviceId": "IOT-cihaz-Linux"} |
| 2018-11-19T12:40:31.000 | /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Cihazda yerel oturum açma denemesi başarısız oldu  | Bir cihazın başarısız yerel oturum açma girişimi algılandı |  {"Uzak adres": "?", "Uzak bağlantı noktası": "", "Yerel bağlantı noktası": "", "Oturum açma kabuğu": "/ bin/su", "Oturum açma işlemi Id": "22644", "kullanıcı adı": "saldırgan", "DeviceId": "IOT-cihaz-Linux"} |

### <a name="device-summary-query"></a>Cihaz özeti sorgu

Geçen hafta IOT Hub, cihaz, uyarı önem derecesi, uyarı türüne göre birkaç farklı güvenlik uyarısı seçmek için bu kql sorgu algılandı kullanın.

```
// Select number of distinct security alerts detected last week by 
//   IoT hub, device, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| summarize Cnt=dcount(SystemAlertId) by
    IoTHubId=ResourceId, 
    DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"]),
    AlertSeverity,
    DisplayName
```

#### <a name="device-summary-query-results"></a>Cihaz özeti sorgu sonuçları

| IoTHubId | DeviceId| AlertSeverity| DisplayName | Sayı |
|----------|---------|------------------|---------|---------|
|/Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Başarılı deneme yanılma saldırısı           | 9   |    
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Orta        | Cihazda yerel oturum açma denemesi başarısız oldu  | 242 |    
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Cihazda yerel başarılı oturum açma      | 31  |
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Orta        | Şifreleme para Miner                     | 4   |
|

### <a name="iot-hub-summary"></a>IOT Hub özeti

Çok sayıda uyarı son bir hafta içinde olan IOT hub, uyarı önem derecesi, uyarı türü tarafından ayrı cihaz seçmek için bu kql sorguyu kullanın:

```
// Select number of distinct devices which had alerts in the last week, by 
//   IoT hub, alert severity, alert type
//
SecurityAlert
| where TimeGenerated > ago(7d)
| extend DeviceId=tostring(parse_json(ExtendedProperties)["DeviceId"])
| summarize CntDevices=dcount(DeviceId) by
    IoTHubId=ResourceId, 
    AlertSeverity,
    DisplayName
```

#### <a name="iot-hub-summary-query-results"></a>IOT Hub özeti sorgu sonuçları

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntDevices |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | Yüksek          | Başarılı deneme yanılma saldırısı           | 1          |    
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | Orta        | Cihazda yerel oturum açma denemesi başarısız oldu  | 1          | 
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | Yüksek          | Cihazda yerel başarılı oturum açma      | 1          |
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | Orta        | Şifreleme para Miner                     | 1          |



## <a name="next-steps"></a>Sonraki adımlar

- IOT için ASC okuma [genel bakış](overview.md)
- ASC hakkında bilgi edinmek için IOT [mimarisi](architecture.md)
- Anlama ve keşfetme [ASC IOT uyarılar için](concept-security-alerts.md)
