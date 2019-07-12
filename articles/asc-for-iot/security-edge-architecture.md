---
title: IOT güvenlik modülü IOT Edge için Azure Güvenlik Merkezi'ni anlama | Microsoft Docs
description: İçin IOT Edge mimarisini ve IOT güvenlik modülü için Azure Güvenlik Merkezi özelliklerini anlayın.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 4581f66a3401764237621bee86228aac724ec0af
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616447"
---
# <a name="azure-iot-edge-security-module"></a>Azure IOT Edge güvenlik modülü

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim worklo§1ads için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[Azure IOT Edge](https://docs.microsoft.com/azure/iot-edge/) yönetmek ve iş akışlarını uçta gerçekleştirmek için güçlü özellikler sunar.
IOT Edge, IOT ortamlarda oynadığı anahtar bölümü yapmak, özellikle çekici kötü amaçlı aktörler için.

IOT güvenlik modülü için Azure Güvenlik Merkezi (ASC), cihazları, IOT Edge için kapsamlı güvenlik çözümü sağlar.
ASC IOT modülü için toplar, toplar ve eyleme dönüştürülebilir güvenlik önerileri ve uyarılar içinde işletim sistemi ve kapsayıcı sistemi ham güvenlik verileri analiz eder.

ASC IOT cihazlarının IOT güvenlik aracılar için ASC IOT Edge modülü, modül ikizi yüksek oranda özelleştirilebilen benzer.
Bkz: [aracınızı yapılandırmak](how-to-agent-configuration.md) daha fazla bilgi için.

ASC IOT güvenlik modülü IOT Edge için aşağıdaki özellikleri sunar:

- Temel işletim sistemi (Linux) ve IOT Edge kapsayıcı sistemleri ham güvenlik olaylarını toplar.
  
  Bkz: [ASC IOT Aracısı yapılandırması için](how-to-agent-configuration.md) kullanılabilir güvenlik veri toplayıcıları hakkında daha fazla bilgi edinmek için.

- IOT Edge dağıtımı analizini bildirimleri.

- Üzerinden gönderilen iletilere ham güvenlik olaylarını toplar [IOT Edge hub'ı](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub).

- Güvenlik modül ikizi aracılığıyla yapılandırmasını kaldırın.

  Bkz: [bir ASC IOT aracısı için yapılandırma](how-to-agent-configuration.md) daha fazla bilgi için.

ASC IOT güvenlik modülü IOT Edge için IOT Edge altında bir ayrıcalıklı modda çalışır.
Ayrıcalıklı modu, işletim sistemi izlemek üzere işlem modülü ve diğer IOT Edge modülleri izin vermek için gereklidir.

## <a name="agent-supported-platforms"></a>Aracı, desteklenen platformlar

ASC IOT güvenlik modülü IOT Edge için şu anda yalnızca Linux için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, hakkında mimarisini ve özelliklerini ASC, IOT güvenlik modülü IOT Edge için bilgi edindiniz.

ASC ile IOT dağıtımı için Başlarken devam etmek için aşağıdaki makaleleri kullanın:

- Dağıtma [IOT Edge için güvenlik modülü](how-to-deploy-edge.md)
- Bilgi edinmek için nasıl [güvenlik modülünüzde yapılandırma](how-to-agent-configuration.md)
- ASC gözden geçirmek için IOT [hizmet önkoşulları](service-prerequisites.md)
- Bilgi edinmek için nasıl [ASC etkinleştirmek için IOT hub'ınızdaki IOT hizmeti](quickstart-onboard-iot-hub.md)
- Hizmet hakkında daha fazla bilgi [ASC için IOT hakkında SSS](resources-frequently-asked-questions.md)