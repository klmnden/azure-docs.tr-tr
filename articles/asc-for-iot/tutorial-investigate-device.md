---
title: IOT cihaz araştırma öğretici Önizleme ASC | Microsoft Docs
description: Bu makalede, ASC IOT için Log Analytics kullanarak şüpheli bir IOT cihazı araştırmak için nasıl kullanılacağı açıklanmaktadır.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: b18b48ae-b445-48f8-9ac0-365d6e065b64
ms.service: ascforiot
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 5459c5a18742b92b77866241212f21c11d7851c9
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541584"
---
# <a name="tutorial-investigate-a-suspicious-iot-device"></a>Öğretici: Şüpheli bir IOT cihaz araştırın

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

ASC IOT hizmet uyarılarını ve kanıt IOT cihazları şüpheli etkinlikler veya ne zaman bir cihaz tehlikeye girmemesini göstergelerden mevcut olacağıyla şüphelenilen olduğunda açık ipuçları sağlar. 

Bu öğreticide, kuruluşunuz için olası riskleri belirlemek için düzeltmek ve gelecekte benzer saldırıları önlemek için en iyi yollarını keşfetmek nasıl karar vermenize yardımcı olmak için sağlanan araştırma önerileri kullanın.  

> [!div class="checklist"]
> * Cihazınızın verileri bulma
> * Kql sorguları kullanarak araştırma


## <a name="where-can-i-access-my-data"></a>Verilerim nerede erişebilir miyim

Varsayılan olarak, IOT için ASC Log Analytics çalışma alanınızda, güvenlik uyarıları ve öneriler depolar. Ham güvenlik verilerinizi depolamak seçebilirsiniz.

Bulmak için veri depolama için Log Analytics çalışma alanınızda:

1. IOT hub'ınızı açın. 
1. Tıklayın **güvenlik**, ardından **ayarları**.
1. Log Analytics çalışma alanı yapılandırma ayrıntılarınızı değiştirin. 
1. **Kaydet**’e tıklayın. 

Yapılandırma, Log Analytics çalışma alanınızda depolanan verilere erişmek için aşağıdakileri yapın:

1. Seçin ve bir ASC için IOT Hub'ınızın IOT uyarıya tıklayın. 
1. Tıklayın **daha fazla araştırma**. 
1. Seçin **buraya tıklayın ve cihaz kimliği sütunu görüntüleyin, bu uyarıyı hangi cihazların görmek için**.

## <a name="investigation-steps-for-suspicious-iot-devices"></a>Şüpheli IOT cihazları için araştırma adımları

Insights ve IOT cihazlarınızı hakkında ham verilere erişim için Log Analytics çalışma alanınıza gidin [miyim eriştiği verilerimi](#where-can-i-access-my-data).

Denetleyin ve aşağıdaki ayrıntıları ve etkinliklerin aşağıdaki kql sorguları kullanarak cihaz verileri araştırın:

- Başka uyarılar olup olmadığını öğrenmek için aşağıdaki kql sorguyu aynı zaman kullanımı geçici olarak tetikleyen:

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityAlert
  | where ExtendedProperties contains device and ResourceId contains tolower(hub)
  | project TimeGenerated, AlertName, AlertSeverity, Description, ExtendedProperties
  ~~~

- Öğrenmek için hangi kullanıcıların bu cihazı kullanmak aşağıdaki kql sorgu erişebilirsiniz: 

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "LocalUsers"
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     GroupNames=extractjson("$.GroupNames", EventDetails, typeof(string)),
     UserName=extractjson("$.UserName", EventDetails, typeof(string))
  | summarize FirstObserved=min(TimestampLocal) by GroupNames, UserName
  ~~~
Bu verileri bulmak için kullanın: 
  1. Hangi kullanıcıların cihaz erişimi?
  2. Beklenen izin düzeylerini erişimi olan kullanıcılar var mı? 

- Nasıl kullanıcılar cihazınıza erişim hakkı mümkün olduğunu öğrenmek için aşağıdaki kql sorguyu kullanın:

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "ListeningPorts"
     and extractjson("$.LocalPort", EventDetails, typeof(int)) <= 1024 // avoid short-lived TCP ports (Ephemeral)
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     Protocol=extractjson("$.Protocol", EventDetails, typeof(string)),
     LocalAddress=extractjson("$.LocalAddress", EventDetails, typeof(string)),
     LocalPort=extractjson("$.LocalPort", EventDetails, typeof(int)),
     RemoteAddress=extractjson("$.RemoteAddress", EventDetails, typeof(string)),
     RemotePort=extractjson("$.RemotePort", EventDetails, typeof(string))
  | summarize MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), AllowedRemoteIPAddress=makeset(RemoteAddress), AllowedRemotePort=makeset(RemotePort) by Protocol, LocalPort
  ~~~

    Bu verileri bulmak için kullanın:
  1. Hangi dinleme yuva cihazda şu anda etkin mi?
  2. Şu anda dinleme yuva etkin olması gerekiyor mu?

- Cihaza açan öğrenmek için aşağıdaki kql sorguyu kullanın: 
 
  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "Login"
     // filter out local, invalid and failed logins
     and EventDetails contains "RemoteAddress"
     and EventDetails !contains '"RemoteAddress":"127.0.0.1"'
     and EventDetails !contains '"UserName":"(invalid user)"'
     and EventDetails !contains '"UserName":"(unknown user)"'
     //and EventDetails !contains '"Result":"Fail"'
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     UserName=extractjson("$.UserName", EventDetails, typeof(string)),
     LoginHandler=extractjson("$.Executable", EventDetails, typeof(string)),
     RemoteAddress=extractjson("$.RemoteAddress", EventDetails, typeof(string)),
     Result=extractjson("$.Result", EventDetails, typeof(string))
  | summarize CntLoginAttempts=count(), MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), CntIPAddress=dcount(RemoteAddress), IPAddress=makeset(RemoteAddress) by UserName, Result, LoginHandler
  ~~~

    Sorgu sonuçlarını bulmak için kullanın:
  1. Cihaza hangi kullanıcıların oturum?
  2. Oturum açmış kullanıcıların, beklenen ya da beklenmeyen IP adreslerinden bağlanabilir?
  
- Cihaz beklendiği gibi davrandığından olursa bilgi edinmek için aşağıdaki kql sorguyu kullanın:

  ~~~
  let device = "YOUR_DEVICE_ID";
  let hub = "YOUR_HUB_NAME";
  SecurityIoTRawEvent
  | where
     DeviceId == device and AssociatedResourceId contains tolower(hub)
     and RawEventName == "ProcessCreate"
  | project
     TimestampLocal=extractjson("$.TimestampLocal", EventDetails, typeof(datetime)),
     Executable=extractjson("$.Executable", EventDetails, typeof(string)),
     UserId=extractjson("$.UserId", EventDetails, typeof(string)),
     CommandLine=extractjson("$.CommandLine", EventDetails, typeof(string))
  | join kind=leftouter (
     // user UserId details
     SecurityIoTRawEvent
     | where
        DeviceId == device and AssociatedResourceId contains tolower(hub)
        and RawEventName == "LocalUsers"
     | project
        UserId=extractjson("$.UserId", EventDetails, typeof(string)),
        UserName=extractjson("$.UserName", EventDetails, typeof(string))
     | distinct UserId, UserName
  ) on UserId
  | extend UserIdName = strcat("Id:", UserId, ", Name:", UserName)
  | summarize CntExecutions=count(), MinObservedTime=min(TimestampLocal), MaxObservedTime=max(TimestampLocal), ExecutingUsers=makeset(UserIdName), ExecutionCommandLines=makeset(CommandLine) by Executable
  ~~~

    Sorgu sonuçlarını bulmak için kullanın:

  1. Cihaz üzerinde çalışan herhangi bir şüpheli işlem vardı?
  2. İşlemler uygun kullanıcılar tarafından yürütülmüştür?
  3. Herhangi bir komut satırı yürütme doğru ve beklenen bağımsız değişken içeren?

## <a name="next-steps"></a>Sonraki adımlar
Bir aygıt araştırma ve daha iyi anlamak riskleri kazandıktan sonra düşünmek isteyebilirsiniz [özel uyarılar yapılandırma](quickstart-create-custom-alerts.md) IOT çözüm güvenliğini artırmak için. Bir cihaz Aracısı zaten yoksa, göz önünde bulundurun [güvenlik aracısı dağıtma](select-deploy-agent.md) veya [var olan bir cihaz Aracısı yapılandırmasını değiştirme](concept-agent-configuration.md) sonuçlarınızı geliştirmek için. 
 