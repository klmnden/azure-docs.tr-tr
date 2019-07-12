---
title: Azure Güvenlik Merkezi için IOT Preview kullanarak verilere erişilmesi | Microsoft Docs
description: IOT için Azure Güvenlik Merkezi'ni kullanarak güvenlik uyarısı ve öneri verilerinizi erişim hakkında bilgi edinin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: fbd96ddd-cd9f-48ae-836a-42aa86ca222d
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 2d3f3c6ad194ff8c9582f0c9e71a29b37ba5d967
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616746"
---
# <a name="access-your-security-data"></a>Güvenlik verilerinize erişin 

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

(Kaydetmeyi seçerseniz) güvenlik uyarıları, öneriler ve güvenlik ham verileri Log Analytics çalışma alanınızda IOT için azure Güvenlik Merkezi (ASC) depolar.

## <a name="log-analytics"></a>Log Analytics

Hangi Log Analytics çalışma alanı kullanılan yapılandırmak için:

1. IOT hub'ınızı açın.
1. Tıklayın **güvenlik**
2. Tıklayın **ayarları**ve Log Analytics çalışma alanı yapılandırmanızı değiştirin.

Yapılandırmadan sonra Log Analytics çalışma alanınızın erişmek için:

1. Bir uyarı veya öneri artan düzende IOT için seçin. 
2. Tıklayın **daha fazla araştırma**, ardından **buraya tıklayın ve cihaz kimliği sütunu görüntüleyin, bu uyarıyı hangi cihazların görmek için**.

Log Analytics verilerini sorgulama hakkında daha fazla bilgi için bkz [sorgular, Log Analytics ile çalışmaya başlama](https://docs.microsoft.com//azure/log-analytics/query-language/get-started-queries).

## <a name="security-alerts"></a>Güvenlik uyarıları

Güvenlik Uyarıları depolanır _AzureSecurityOfThings.SecurityAlert_ ASC IOT çözümü için yapılandırılmış Log Analytics çalışma alanında Tablo.

Güvenlik Uyarıları araştırmaya yardımcı olması için yararlı sorguları bir dizi başlama sağlanan yaptıklarımız.

### <a name="sample-records"></a>Örnek kayıt

Birkaç rastgele kayıtları seçin

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

| TimeGenerated           | IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Açıklama                                             | ExtendedProperties                                                                                                                                                             |
|-------------------------|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|---------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 2018-11-18T18:10:29.000 | /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Başarılı deneme yanılma saldırısı           | Cihazda bir deneme yanılma yoluyla yapılan zorla saldırısı başarılı oldu        |    {"Tam kaynak Address": "[\"10.165.12.18:\"]", "Kullanıcı adlarını": "[\"\"]", "DeviceId": "IOT-cihaz-Linux"}                                                                       |
| 2018-11-19T12:40:31.000 | /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Cihazda yerel başarılı oturum açma      | Cihaza başarılı bir yerel oturum algılandı     | {"Uzak adres": "?", "Uzak bağlantı noktası": "", "Yerel bağlantı noktası": "", "Oturum açma kabuğu": "/ bin/su", "Oturum açma işlemi Id": "28207", "kullanıcı adı": "saldırgan", "DeviceId": "IOT-cihaz-Linux"} |
| 2018-11-19T12:40:31.000 | /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Cihazda yerel oturum açma denemesi başarısız oldu  | Bir cihazın başarısız yerel oturum açma girişimi algılandı |  {"Uzak adres": "?", "Uzak bağlantı noktası": "", "Yerel bağlantı noktası": "", "Oturum açma kabuğu": "/ bin/su", "Oturum açma işlemi Id": "22644", "kullanıcı adı": "saldırgan", "DeviceId": "IOT-cihaz-Linux"} |

### <a name="device-summary"></a>Cihaz özeti

Geçen hafta, IOT Hub, cihaz, uyarı önem derecesi, uyarı türü tarafından algılanan farklı güvenlik uyarılarını sayısını seçin.

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

| IoTHubId                                                                                                       | DeviceId      | AlertSeverity | DisplayName                           | Count |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------|---------------------------------------|-----|
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Başarılı deneme yanılma saldırısı           | 9   |   
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Orta        | Cihazda yerel oturum açma denemesi başarısız oldu  | 242 |    
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | Cihazda yerel başarılı oturum açma      | 31  |
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Orta        | Şifreleme para Miner                     | 4   |

### <a name="iot-hub-summary"></a>IOT hub özeti

Bir IOT Hub, uyarı önem derecesi, uyarı türü tarafından son bir hafta içinde uyarıları olan farklı cihaz sayısını seçin

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

| IoTHubId                                                                                                       | AlertSeverity | DisplayName                           | CntDevices |
|----------------------------------------------------------------------------------------------------------------|---------------|---------------------------------------|------------|
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | Yüksek          | Başarılı deneme yanılma saldırısı           | 1\.          |    
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | Orta        | Cihazda yerel oturum açma denemesi başarısız oldu  | 1          | 
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | Yüksek          | Cihazda yerel başarılı oturum açma      | 1\.          |
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | Orta        | Şifreleme para Miner                     | 1\.          |

## <a name="security-recommendations"></a>Güvenlik önerileri

Güvenlik önerilerini depolanır _AzureSecurityOfThings.SecurityRecommendation_ ASC IOT çözümü için yapılandırılmış Log Analytics çalışma alanında Tablo.

Güvenlik önerilerini keşfetmeye başlangıç yapmanıza yardımcı olacak yararlı sorguları bir dizi sağladık.

### <a name="sample-records"></a>Örnek kayıt

Birkaç rastgele kayıtları seçin

```
// Select a few random records
//
SecurityRecommendation
| project 
    TimeGenerated, 
    IoTHubId=AssessedResourceId, 
    DeviceId,
    RecommendationSeverity,
    RecommendationState,
    RecommendationDisplayName,
    Description,
    RecommendationAdditionalData
| take 2
```
    
| TimeGenerated | IoTHubId | DeviceId | RecommendationSeverity | RecommendationState | RecommendationDisplayName | Açıklama | RecommendationAdditionalData |
|---------------|----------|----------|------------------------|---------------------|---------------------------|-------------|------------------------------|
| 2019-03-22T10:21:06.060 | /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Orta | Etkin | Giriş zincirindeki izin veren güvenlik duvarı kuralı bulunamadı | Çok çeşitli IP adres veya bağlantı noktası için esnek bir düzen içeren bir güvenlik duvarı kuralında bulundu | {"Kurallar": "[{\"SourceAddress\":\"\",\"KaynakBağlantıNoktası\":\"\",\"DestinationAddress\":\" \" \"Trafficdirection\":\"1337\"}] "} |
| 2019-03-22T10:50:27.237 | /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Orta | Etkin | Giriş zincirindeki izin veren güvenlik duvarı kuralı bulunamadı | Çok çeşitli IP adres veya bağlantı noktası için esnek bir düzen içeren bir güvenlik duvarı kuralında bulundu | {"Kurallar": "[{\"SourceAddress\":\"\",\"KaynakBağlantıNoktası\":\"\",\"DestinationAddress\":\" \" \"Trafficdirection\":\"1337\"}] "} |

### <a name="device-summary"></a>Cihaz özeti

IOT Hub, cihaz, öneri önem derecesi ve türüne göre farklı etkin güvenlik önerileri sayısını seçin.

```
// Select number of distinct active security recommendations by 
//   IoT hub, device, recommendation severity and type
//
SecurityRecommendation
| extend IoTHubId=AssessedResourceId
| summarize CurrentState=arg_max(RecommendationState, DiscoveredTimeUTC) by IoTHubId, DeviceId, RecommendationSeverity, RecommendationDisplayName
| where CurrentState == "Active"
| summarize Cnt=count() by IoTHubId, DeviceId, RecommendationSeverity
```

| IoTHubId                                                                                                       | DeviceId      | RecommendationSeverity | Count |
|----------------------------------------------------------------------------------------------------------------|---------------|------------------------|-----|
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | 2   |    
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Orta        | 1 |  
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Yüksek          | 1\.  |
| /Subscriptions/ < subscription_id > /resourceGroups/ < resource_group > /providers/Microsoft.Devices/IotHubs/ < iot_hub > | < aygıt_adı > | Orta        | 4   |


## <a name="next-steps"></a>Sonraki adımlar

- IOT için ASC okuma [genel bakış](overview.md)
- ASC hakkında bilgi edinmek için IOT [mimarisi](architecture.md)
- Anlama ve keşfetme [ASC IOT uyarılar için](concept-security-alerts.md)
- Anlama ve keşfetme [ASC IOT öneri için](concept-recommendations.md)