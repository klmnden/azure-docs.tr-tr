---
title: Azure Güvenlik Merkezi (ASC) IOT önizlemesini kullanmaya başlama | Microsoft Docs
description: ASC temel iş akışı IOT özelliklerini ve hizmet için anlamak kullanmaya başlayın.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 55c8d3b6-3126-4246-8d07-ef88fe5ea84f
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/26/2019
ms.author: mlottner
ms.openlocfilehash: 1186b362cf8f59f24020ae9afa3526e2e27b1794
ms.sourcegitcommit: c63fe69fd624752d04661f56d52ad9d8693e9d56
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2019
ms.locfileid: "58575224"
---
# <a name="get-started-with-azure-security-center-asc-for-iot"></a>IOT için Azure Güvenlik Merkezi (ASC) ile çalışmaya başlama 

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede IOT hizmeti için bir açıklama ASC farklı yapı taşları sağlar ve nasıl kullanmaya başlayacağınızı açıklayan [hizmetini etkinleştirme](quickstart-onboard-iot-hub.md). 

IOT için ASC, güvenlik analizi, IOT hub'ı yapılandırma, cihaz kimliği ve hub cihaz kimlik doğrulaması desenleri sağlamak için IOT hub'a sorunsuz bir şekilde tümleştirilebilir.
Gelişmiş güvenlik özellikleri için ASC IOT için güvenlik verilerini IOT cihazlarınızdan aracı tabanlı koleksiyonunu sağlar.

## <a name="asc-for-iot-seamless-iot-hub-integration"></a>ASC IOT sorunsuz IOT hub'ı tümleştirme

Tek tek IOT cihazlarınızı korumaya çalışırken, doğrudan cihazlardan ya da bunların ağ üzerinden veri toplama özelliğine gereklidir. Bu desteklemek için ASC için IOT cihaz izleme ve artırma sağlamak için düşük Ayak izi güvenlik aracıları cephaneliği sunar.

Linux ve Windows Güvenlik aracılar için hem de mimari IOT Önizleme için artan düzende başvuru C# ve C sağlanır.
Aracılar cihaz işletim sistemi maliyetini ve cihaz modül ikizi konfigürasyonuyla olay toplama ham olay koleksiyonundan işleyin.
Güvenlik iletileri, IOT Hub aracılığıyla IOT Analiz Hizmetleri için ASC halinde gönderilir.

## <a name="asc-for-iot-basics"></a>ASC için IOT temel bilgileri

IOT cihaz ve ortam gereksinimlerinize en uygun iş akışı senaryosunu seçin:

### <a name="get-started-with-asc-for-iot-seamless-iot-hub-integration"></a>IOT sorunsuz IOT hub'ı tümleştirme ASC ile çalışmaya başlama 

>[!Note]
>Bu iş akışı, IOT güvenlik aracıları ASC kullanmadan hizmet kullanmanıza olanak sağlar. 

Cihazınızı izlemeyi etkinleştirmek için Kimlik Yönetimi, Bulut ve cihaz iletişim düzenleri, buluta cihaza aşağıdaki temel iş akışı test etmek ve hizmeti başlatmak için kullanın: 

1. [IOT Hub'ınızın IOT hizmet ASC etkinleştir](quickstart-onboard-iot-hub.md)
1. Kayıtlı cihaz yok, IOT Hub'ınız varsa [yeni bir cihaz kayıt](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Bir ascforiot güvenlik modülünüzü oluşturmak](quickstart-create-security-twin.md) cihazlarınız için. 
1. Cihaz ve sistemi normal davranış aracılığıyla tanımlamak [özel uyarılar](quickstart-create-custom-alerts.md). 
1. Sistem hizmet ve cihaz durumunu doğrulamak için testleri gerçekleştirin. 
1. Keşfedin [uyarılar](concept-security-alerts.md), [önerileri](concept-recommendations.md), ve [yakından Log Analytics kullanarak](how-to-security-data-access.md) IOT hub'ı kullanarak. 


### <a name="get-started-with-asc-for-iot-security-agents"></a>IOT güvenlik aracıları ASC ile çalışmaya başlama

ASC, test ve hizmeti etkinleştirmek için aşağıdaki temel iş akışı kullanarak uzak bağlantıları, etkin uygulamalar, oturum açma olayları ve işletim sistemi yapılandırma en iyi izleme gibi gelişmiş IOT güvenlik özellikleri kullanmak olun: 

1. [IOT hub'ınıza IOT hizmeti ASC etkinleştir](quickstart-onboard-iot-hub.md)
1. Kayıtlı cihaz yok, IOT Hub'ınız varsa [yeni bir cihaz kayıt](https://docs.microsoft.com/azure/iot-accelerators/quickstart-device-simulation-deploy).
1. [Bir azureiotsecurity güvenlik modülünüzü oluşturmak](quickstart-create-security-twin.md) cihazlarınız için.
1. Gerçek bir cihaza yüklemek yerine bir Azure sanal cihazı aracıyı yüklemek için [döngü oluşturan yeni bir Azure sanal makine (VM)](https://docs.microsoft.com/azure/virtual-machines/linux/quick-create-portal) mevcut bir bölge içinde. 
1. [IOT güvenliği aracısı için bir ASC dağıtma](how-to-deploy-linux-cs.md) IOT cihaz veya yeni bir VM.
1. Yönergelerini izleyin [trigger_events](https://aka.ms/iot-security-github-trigger-events) zararsız bir saldırı simülasyonu çalıştırmak için.
1. Yanıt önceki adımda sanal saldırı olarak IOT uyarılar için ASC doğrulayın. 
    - Betiği çalıştırdıktan sonra beş dakika doğrulama başlayın.
1. Keşfedin [uyarılar](concept-security-alerts.md), [önerileri](concept-recommendations.md), ve [yakından Log Analytics kullanarak](how-to-security-data-access.md) IOT hub'ı kullanarak. 

## <a name="next-steps"></a>Sonraki adımlar

- Etkinleştirme [ASC IOT için](quickstart-onboard-iot-hub.md)
- Yapılandırma, [çözümü](quickstart-configure-your-solution.md)
- [Güvenlik modülleri oluşturma](quickstart-create-security-twin.md)
- Yapılandırma [özel uyarılar](quickstart-create-custom-alerts.md)
- [Güvenlik aracı dağıtma](how-to-deploy-agent.md)
