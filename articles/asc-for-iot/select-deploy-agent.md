---
title: Seçin ve bir ASC IOT Aracısı Önizleme dağıtma | Microsoft Docs
description: Hakkında nasıl seçin ve IOT cihazları IOT güvenlik aracıları için ASC dağıtma öğrenin.
services: ascforiot
documentationcenter: na
author: mlottner
manager: barbkess
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.service: ascforiot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: 5ce583fe98acb7e0a4aaeb720cb2d5ed5eebb9e3
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58541970"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Seçin ve bir IOT Cihazınızı güvenlik aracısında dağıtma

> [!IMPORTANT]
> ASC IOT için şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).


IOT için ASC güvenlik aracılar IOT cihazlarından veri toplamak ve izlemek için başvuru mimarileri sağlar.

Güvenlik aracıları, açık kaynaklı proje olarak geliştirilir ve iki model olarak sağlanır kullanılabilir: <br> [C](https://aka.ms/iot-security-github-c), ve [ C# ](https://aka.ms/iot-security-github-cs).

## <a name="supported-platforms-for-asc-for-iot-agents"></a>IOT aracılar için ASC için desteklenen platformlar

Aşağıdaki liste, şu anda desteklenen tüm platformlar içerir.  

|ASC IOT Aracısı |İşletim Sistemi |Mimari |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |   x64|
|C|Ubuntu 18.04 |   x64|
|C|Debian 9 |   x64, x86|
|C#|Ubuntu 16.04    |x64|
|C#|Ubuntu 18.04    |x64|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    X64|
|C#|Windows 10 IoT Core derleme 17763 |x64|


## <a name="which-flavor-should-i-use"></a>Hangi flavor kullanmalıyım?

IOT cihazlarınızı göre aşağıdaki soruları göz önünde bulundurun:

- Kullanmakta olduğunuz _Windows Server_ veya _Windows IOT Core_? 

    Kullanım [ASC ile Windows için IOT yüklemesi C#-tabanlı güvenlik aracı](tutorial-deploy-windows-cs.md).

- Bir Linux dağıtımı x86 ile kullandığınız mimarisi? 

    Kullanım [ASC IOT yükleme Linux için C tabanlı güvenlik aracısıyla](tutorial-deploy-linux-c.md).
- Bir Linux dağıtımı x64 ile kullandığınız mimarisi?

    Her iki model olarak sağlanır kullanabilirsiniz. <br>
    [ASC IOT yükleme Linux için C tabanlı güvenlik aracısıyla](tutorial-deploy-linux-c.md) ve/veya [ASC ile Linux için IOT yüklemesi C#-tabanlı güvenlik aracı](tutorial-deploy-linux-cs.md).

Her iki model olarak sağlanır. aynı özellik kümesini sahip ve benzer yapılandırma seçeneklerini destekler. C tabanlı güvenlik aracı daha küçük bellek Ayak izi var.


## <a name="next-steps"></a>Sonraki adımlar
- [Genel Bakış](overview.md)
- [Güvenlik aracıları](security-agent-architecture.md)
- [Güvenlik aracı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md)
- [ASC için IOT hakkında SSS](resources-frequently-asked-questions.md)