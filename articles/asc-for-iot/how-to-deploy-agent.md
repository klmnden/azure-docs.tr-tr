---
title: Seçin ve Azure Güvenlik Merkezi Önizleme IOT Aracısı dağıtma | Microsoft Docs
description: Hakkında nasıl seçin ve IOT cihazları IOT güvenlik aracıları için Azure Güvenlik Merkezi dağıtma öğrenin.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: 32a9564d-16fd-4b0d-9618-7d78d614ce76
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2019
ms.author: mlottner
ms.openlocfilehash: c549e5ccbda9b364b3e7d20c9572eb777c32299e
ms.sourcegitcommit: 6a42dd4b746f3e6de69f7ad0107cc7ad654e39ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2019
ms.locfileid: "67616822"
---
# <a name="select-and-deploy-a-security-agent-on-your-iot-device"></a>Seçin ve bir IOT Cihazınızı güvenlik aracısında dağıtma

> [!IMPORTANT]
> IOT için Azure Güvenlik Merkezi şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

IOT için Azure Güvenlik Merkezi (ASC), IOT cihazlarından veri toplamak ve izlemek güvenlik aracılar için başvuru mimarileri sağlar.
Bkz: [güvenlik aracı başvuru mimarisi](security-agent-architecture.md) daha fazla bilgi için.

Aracılar, açık kaynaklı proje olarak geliştirilir ve iki model olarak sağlanır kullanılabilir: <br> [C](https://aka.ms/iot-security-github-c), ve [ C# ](https://aka.ms/iot-security-github-cs).

Bu makalede şunları öğreneceksiniz: 
> [!div class="checklist"]
> * Güvenlik aracı özellikleri karşılaştırın
> * Desteklenen aracı platformları keşfedin
> * Çözümünüz için doğru aracı türü seçin

## <a name="understand-security-agent-options"></a>Güvenlik aracı seçenekleri anlama

Her ASC IOT güvenlik aracı flavor için aynı özellik kümesini sunar ve benzer yapılandırma seçeneklerini destekler. 

C tabanlı güvenlik aracı olan daha küçük bellek Ayak izi ve daha az kullanılabilir kaynaklarla cihazlar için ideal bir seçimdir. 

|     | C tabanlı güvenlik aracı | C#-tabanlı güvenlik aracı |
| --- | ----------- | --------- |
| Açık kaynak | Altında kullanılabilir [MIT lisansı](https://en.wikipedia.org/wiki/MIT_License) içinde [Github](https://aka.ms/iot-security-github-cs) | Altında kullanılabilir [MIT lisansı](https://en.wikipedia.org/wiki/MIT_License) içinde [Github](https://aka.ms/iot-security-github-c) |
| Bir geliştirme dilini    | C | C# |
| Desteklenen Windows platformları? | Hayır | Evet |
| Windows önkoşulları | --- | [WMI](https://docs.microsoft.com/windows/desktop/wmisdk/) |
| Desteklenen Linux platformlarından? | Evet, x64 ve x86 | Evet, yalnızca x64 |
| Linux önkoşulları | libunwind8, libcurl3, UUID çalışma zamanı, auditd, audispd eklentileri | libunwind8, libcurl3, UUID çalışma zamanı, auditd, audispd eklentileri, sudo, netstat, iptables |
| Disk Ayak izi | 10.5 MB | 90MB |
| (Ortalama üzerinde) bellek Ayak izi | 5.5 MB | 33MB |
| [Kimlik doğrulaması](concept-security-agent-authentication-methods.md) IOT hub'ına | Evet | Evet |
| Güvenlik veri [koleksiyonu](how-to-agent-configuration.md#supported-security-events) | Evet | Evet |
| Olay toplama | Evet | Evet |
| Uzak konfigürasyonuyla [güvenlik modül ikizi](concept-security-module.md) | Evet | Evet |


## <a name="choose-an-agent-flavor"></a>Bir aracı türü seçin 

Doğru aracı seçmek için IOT cihazlarınızı hakkında aşağıdaki soruları yanıtlayın:

- Kullanmakta olduğunuz _Windows Server_ veya _Windows IOT Core_? 

    [Dağıtım bir C#-güvenlik aracı için Windows tabanlı](how-to-deploy-windows-cs.md).

- Bir Linux dağıtımı x86 ile kullandığınız mimarisi? 

    [Linux için C tabanlı güvenlik aracısı dağıtma](how-to-deploy-linux-c.md).

- Bir Linux dağıtımı x64 ile kullandığınız mimarisi?

    Her iki aracı flavor kullanabilirsiniz. <br>
    [Linux için C tabanlı güvenlik aracısı dağıtma](how-to-deploy-linux-c.md) ve/veya [Dağıt bir C#-Linux için aracıyı güvenlik tabanlı](how-to-deploy-linux-cs.md).

Her iki aracı özellikleri aynı özellik kümesi sunan ve benzer yapılandırma seçeneklerini destekler.
Bkz: [güvenlik aracı karşılaştırma](how-to-deploy-agent.md#understand-security-agent-options) daha fazla bilgi için.

## <a name="supported-platforms"></a>Desteklenen platformlar

Aşağıdaki liste, şu anda desteklenen tüm platformlar içerir.

|ASC IOT Aracısı |İşletim sistemi |Mimari |
|--------------|------------|--------------|
|C|Ubuntu 16.04 |   x64|
|C|Ubuntu 18.04 |   x64|
|C|Debian 9 |   x64, x86|
|C#|Ubuntu 16.04    |x64|
|C#|Ubuntu 18.04    |x64|
|C#|Debian 9    |x64|
|C#|Windows Server 2016|    X64|
|C#|Windows 10 IoT Core derleme 17763 |x64|

## <a name="next-steps"></a>Sonraki adımlar

Yapılandırma seçenekleri hakkında daha fazla bilgi için aracı yapılandırması ile ilgili nasıl yapılır kılavuzuna devam edin. 
> [!div class="nextstepaction"]
> [Aracı Yapılandırma Kılavuzu nasıl](./how-to-agent-configuration.md)
