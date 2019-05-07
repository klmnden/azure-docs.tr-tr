---
title: IOT güvenlik Aracı Mimarisi Önizleme için Azure Güvenlik Merkezi'ni anlama | Microsoft Docs
description: Azure Güvenlik Merkezi'nde IOT hizmeti için kullanılan aracıları için güvenlik aracı mimarisini anlama.
services: asc-for-iot
ms.service: ascforiot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/25/2019
ms.author: mlottner
ms.openlocfilehash: 3c05b7e9b1c6d1b9214da168f7abfcbb322f8f6d
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65192527"
---
# <a name="security-agent-reference-architecture"></a>Güvenlik aracı başvuru mimarisi

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanmaktadır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


IOT için Azure Güvenlik Merkezi (ASC), oturum, işlemek, toplama ve güvenlik verileri IOT hub'ı üzerinden göndermek güvenlik aracılar için başvuru mimarisi sağlar.

Güvenlik aracıları kısıtlanmış bir IOT ortamda çalışacak şekilde tasarlanmıştır ve değerleri kullandıkları kaynaklar için karşılaştırıldığında sağlamak açısından tümüyle özelleştirilebilir.

Güvenlik aracıları, aşağıdaki özellikleri destekler:

- Ham güvenlik olayları alttaki işletim sisteminden (Linux, Windows) toplayın. Kullanılabilir güvenlik veri toplayıcıları hakkında daha fazla bilgi için bkz: [ASC IOT Aracısı yapılandırması için](how-to-agent-configuration.md).

- IOT hub'ı aracılığıyla gönderilen iletilere ham güvenlik olaylarını toplama.

- Var olan bir cihaz kimliği veya adanmış modül kimliği ile kimlik doğrulaması. Bkz: [güvenlik aracı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md) daha fazla bilgi için.

- Aracılığıyla uzaktan yapılandırma **azureiotsecurity** modül ikizi. Daha fazla bilgi için bkz. [bir ASC IOT aracısı için yapılandırma](how-to-agent-configuration.md).

ASC IOT güvenlik aracılar için açık kaynaklı proje olarak geliştirilir ve Github'dan kullanılabilir: 

- [ASC için IOT C tabanlı aracı](https://github.com/Azure/Azure-IoT-Security-Agent-C) 
- [IOT için ASC C#-tabanlı aracı](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="agent-supported-platforms"></a>Aracı, desteklenen platformlar

IOT için ASC, 32 bit ve 64 bit Windows için farklı bir yükleyici aracıları ve 32 bit ve 64 bit Linux için aynı sunar. Aşağıdaki tabloda cihazlarınızın her biri için doğru aracı yükleyicisini olduğundan emin olun:

| 32 veya 64 bit | Linux | Windows |    Ayrıntılar|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32 bit  | C  | C#  ||
| 64 bit  | C#veya C           | C#      | Minimum kaynakları ile cihazları için C Aracısı'nı kullanın|

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IOT güvenlik aracı mimarisi ve kullanılabilir yükleyicileri hakkında ASC öğrendiniz.

ASC ile IOT dağıtımı için Başlarken devam etmek için aşağıdaki makaleleri kullanın:

- Anlamak [güvenlik aracı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md)
- Seçin ve dağıtan bir [güvenlik aracı](how-to-deploy-agent.md)
- ASC gözden geçirmek için IOT [hizmet önkoşulları](service-prerequisites.md)
- Bilgi edinmek için nasıl [ASC etkinleştirmek için IOT hub'ınızdaki IOT hizmeti](quickstart-onboard-iot-hub.md)
- Hizmet hakkında daha fazla bilgi [ASC için IOT hakkında SSS](resources-frequently-asked-questions.md)
