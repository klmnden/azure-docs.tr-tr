---
title: 'Azure durumu İzleyicisi v2 API Başvurusu: İzleme altyapısını etkinleştirirsiniz | Microsoft Docs'
description: Durum İzleyicisi'ni v2 API başvuru Enable-InstrumentationEngine. Web sitesi yeniden dağıtmaya gerek kalmadan Web sitesi performansını izleyin. Şirket içinde, sanal makinelerde veya Azure üzerinde ASP.NET web uygulamaları ile çalışır.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: 7d337c9b6b22f3abfcb4aea1c47127706ed9e9d7
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870515"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>Durum İzleyicisi'ni v2 API'si: Etkinleştir-InstrumentationEngine (v0.2.1-alpha)

Bu belge, bir üyesi olarak sunulan bir cmdlet açıklar [Az.ApplicationMonitor PowerShell Modülü](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/).

> [!IMPORTANT]
> Durum İzleyicisi'ni v2 şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>Açıklama

Bu cmdlet, bazı kayıt defteri anahtarlarını ayarlayarak izleme altyapısı olanak tanır.
Bu değişikliklerin etkili olması için IIS yeniden başlatın.

İzleme altyapısının .NET SDK'ları tarafından toplanan verileri destekleyebilirsiniz.
Yönetilen bir işlemin yürütülmesi açıklayan olayları ve iletileri toplanmayacak. Ancak bunlarla sınırlı olmamak bağımlılık sonuç kodları, HTTP fiilleri ve SQL komut metnini içeren. 

İzleme altyapısının, etkinleştirin:
- Zaten etkin cmdlet'ini kullanarak izleme etkinleştirildi ancak InstrumentationEngine etkinleştirme kaydetmedi.
- Uygulamanızı .NET SDK'ları ile el ile işaretlediyseniz ve ek telemetri toplamak istiyorsanız.

> [!IMPORTANT] 
> Bu cmdlet, yönetici izinlerine sahip bir PowerShell oturumu gerektirir.

> [!NOTE] 
> Bu cmdlet gözden geçirin ve lisans ve gizlilik bildirimimizi kabul etmenizi gerektirir.

> [!NOTE] 
> İzleme altyapısı, ek yükü ekler ve varsayılan olarak kapalıdır.

## <a name="examples"></a>Örnekler

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>Parametreler 

### <a name="-acceptlicense"></a>-AcceptLicense
**İsteğe bağlı.** Gözetimsiz yüklemelerinde lisans ve gizlilik bildirimini kabul etmek için bu anahtarı kullanın.

### <a name="-verbose"></a>-Verbose
**Ortak parametresi.** Ayrıntılı günlük çıktısını almak için bu anahtarı kullanın.

## <a name="output"></a>Çıktı


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>İzleme altyapısı başarıyla etkinleştirme gelen örnek çıktı

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```
