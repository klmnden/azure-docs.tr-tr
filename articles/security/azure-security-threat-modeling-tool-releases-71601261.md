---
title: Tehdit modelleme Aracı sürümleri - Microsoft tehdit modelleme aracı - Azure | Microsoft Docs
description: Tehdit modelleme aracı için sürüm notları belgeleme
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: jegeib
ms.openlocfilehash: c96b924294286be57de90dae7e6534b5ed9306ea
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60586238"
---
# <a name="threat-modeling-tool-update-release-71601261---1292019"></a>Threat Modeling Tool güncelleştirme sürümü 7.1.60126.1 - 29.1.2019

Microsoft tehdit modelleme Aracı sürümünün 7.1.60126.1 29 Ocak 2019'üzerinde yayımlanan ve aşağıdaki değişiklikleri içerir:

- .NET, gerekli en düşük sürümü adına artırıldı [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262).
- Gerekli en düşük Windows sürümü için artırılmıştır [Windows 10 Yıldönümü güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) .NET bağımlılık nedeniyle.
- Aracı'nın Seçenekler menüsüne bir model doğrulama Değiştir özelliği eklendi.
- Tehdit özelliklerini birkaç bağlantı güncelleştirildi.
- Küçük UX Aracı'nın ana ekrana değiştirir.
- Tehdit modelleme aracı şimdi, konak işletim sistemi TLS ayarlarını devralır ve büyük veya TLS 1.2 gerektiren ortamlarında desteklenir.

## <a name="feature-changes"></a>Özellik değişiklikleri

### <a name="model-validation-option"></a>Model doğrulama seçeneği

Müşteri geri bildirimi doğrultusunda, etkinleştirme veya devre dışı model doğrulama aracına bir seçenek eklendi. Şablonunuzu iki nesne arasındaki tek tek yönlü veri akışı kullandıysanız, daha önce bir hata iletisi bildiren iletiler çerçeve almış olabilirsiniz: ObjectsName gerektirir en az bir 'Any'. Model doğrulama devre dışı bırakma, bu uyarılar görünümünde gösteren öğesinden engeller.

Model doğrulama açıp kapatarak seçeneği dosyasında bulunabilir -> Ayarlar -> Seçenekler menüsü. Bu ayar için varsayılan değer devre dışı bırakıldı.

![Model doğrulama seçeneği](./media/azure-security-threat-modeling-tool-releases-71601261/tmt_model_validation_option.png)

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen İşletim Sistemleri
  - [Microsoft Windows 10 Yıldönümü güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya üzeri
- Gerekli .NET sürümü
  - [.NET 4.7.1](https://go.microsoft.com/fwlink/?LinkId=863262) veya üzeri
- Ek Gereksinimler
  - Şablonları yanı sıra aracı güncelleştirmeleri almak için Internet bağlantısı gerekir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="unsupported-systems"></a>Desteklenmeyen sistemleri

#### <a name="issue"></a>Sorun

.NET 4.7.1 yüklenemiyor veya üstü, Windows 10 Enterprise LTSB (sürüm 1507) gibi Windows 10 sistemlerinin kullanıcıları yükselttikten sonra Aracı'nı açmanız mümkün olmayacaktır. Bu eski Windows sürümleri artık tehdit modelleme aracı için desteklenen platformları olan ve en son güncelleştirmeyi yüklememeniz gerekir.

#### <a name="workaround"></a>Geçici çözüm

Windows 10 Enterprise LTSB (sürüm 1507) en son güncelleştirmeyi yüklemiş olan kullanıcıları Kaldır iletişim kutusunda, uygulamalar ve özellikler aracılığıyla tehdit modelleme Aracı'nın önceki sürümüne geri dönebilirsiniz.

## <a name="documentation-and-feedback"></a>Belgeler ve geri bildirim

- Tehdit modelleme aracı belgelerine yer alan [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool)ve bilgileri [aracını kullanma hakkında](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Sonraki adımlar

En son sürümünü indirin [Microsoft tehdit modelleme aracı](https://aka.ms/threatmodelingtool).
