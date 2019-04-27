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
ms.date: 04/03/2019
ms.author: jegeib
ms.openlocfilehash: 502c1e8a422eb9e74586e5a6820d5b12ec4ae2a4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610640"
---
# <a name="threat-modeling-tool-update-release-71604081---492019"></a>Tehdit modelleme aracı güncelleştirme sürümü 7.1.60408.1 - 4/9/2019

Sürüm 7.1.60408.1, Microsoft tehdit modelleme Aracı (TMT) 9 Nisan 2019'üzerinde yayımlanan ve aşağıdaki değişiklikleri içerir:

- Azure anahtar kasası ve Azure Traffic Manager için yeni şablonlar
- TMT sürüm numara artık giriş ekranında gösterilir.
- Güncelleştirilmiş Destek bağlantılar
- Hata düzeltmeleri

## <a name="feature-changes"></a>Özellik değişiklikleri

### <a name="new-stencils-for-azure-key-vault-and-azure-traffic-manager"></a>Azure anahtar kasası ve Azure Traffic Manager için yeni şablonlar

![Azure anahtar kasası kalıbı](./media/azure-security-threat-modeling-tool-releases-71604081/tmt_keyvault_trafficmanager.PNG)

Yeni şablonlar ve Azure anahtar kasası ve Azure Traffic Manager tehditleri Azure kalıbı kümesine eklenmiş. Azure şablonu kümesi temel alınarak modelleri açarken, kullanıcılar modelle ilişkili şablonu güncelleştirmek için istenir. Azure şablonu kümesi temel alınarak bir modeli güncelleştirme ayrıca el ile "Dosya" menüsünden "Şablon" komutunu kullanarak ve en son Azure bulut Services.tb7 dosyasını yeniden uygulama tarafından başlatılabilir.

### <a name="tmt-version-number-is-now-shown-on-the-home-screen"></a>TMT sürüm numara artık giriş ekranında gösterilir.

Tehdit modelleme Aracı istemci sürümü artık uygulamasının erişim kolaylığı için giriş ekranında gösterilir.

![Azure anahtar kasası kalıbı](./media/azure-security-threat-modeling-tool-releases-71604081/tmt_version.PNG)

### <a name="support-links-have-been-updated"></a>Güncelleştirilmiş Destek bağlantılar

Aracı içindeki tüm destek bağlantılar kullanıcıları güncelleştirilip güncelleştirilmediğini [ tmtextsupport@microsoft.com ](mailto:tmtextsupport@microsoft.com) bir MSDN forumuna yerine.

## <a name="system-requirements"></a>Sistem gereksinimleri

- Desteklenen İşletim Sistemleri
  - [Microsoft Windows 10 Yıldönümü güncelleştirmesi](https://blogs.windows.com/windowsexperience/2016/08/02/how-to-get-the-windows-10-anniversary-update/#HTkoK5Zdv0g2F2Zq.97) veya üzeri
- Gerekli .NET sürümü
  - [.NET 4.7.1](http://go.microsoft.com/fwlink/?LinkId=863262) veya üzeri
- Ek Gereksinimler
  - Şablonları yanı sıra aracı güncelleştirmeleri almak için Internet bağlantısı gerekir.

## <a name="documentation-and-feedback"></a>Belgeler ve geri bildirim

- Tehdit modelleme aracı belgelerine yer alan [docs.microsoft.com](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool)ve bilgileri [aracını kullanma hakkında](https://docs.microsoft.com/azure/security/azure-security-threat-modeling-tool-getting-started).

## <a name="next-steps"></a>Sonraki adımlar

En son sürümünü indirin [Microsoft tehdit modelleme aracı](https://aka.ms/threatmodelingtool).