---
title: 'Azure AD Connect: Eşitleme hizmeti örnekleri | Microsoft Docs'
description: Bu sayfa, Azure AD örneğinde ilgili özel konular listelenmiştir.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/27/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: c342eac5460d8d52422b0497b1283f367660eb3c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66298826"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Örneklerle ilgili özel konular
Azure AD Connect ile Azure AD örneğini dünya çapında en yaygın olarak kullanılan ve Office 365. Ancak diğer örnekleri de vardır ve bu URL'leri ve diğer özel durumlar için farklı gereksinimler vardır.

## <a name="microsoft-cloud-germany"></a>Microsoft Bulut Almanya
[Microsoft Cloud Almanya](https://www.microsoft.de/cloud-deutschland) bir Alman veri Emanetçisi tarafından işletilir bağımsız bir bulut.

| URL'lerin Ara sunucuda açmak için |
| --- |
| \*.microsoftonline.de |
| \*. windows.net |
| \+ Sertifika iptal listeleri |

Azure AD kiracınız ile oturum açtığınızda onmicrosoft.de etki alanında bir hesap kullanmanız gerekir.

Microsoft Cloud Almanya'da şu anda mevcut özellikler:

* **Parola geri yazma** Önizleme ile Azure AD Connect sürümünüzü 1.1.570.0 ve sonra kullanılabilir.
* Diğer Azure AD Premium hizmetlerine kullanılabilir değil.

## <a name="microsoft-azure-government"></a>Microsoft Azure Kamu
[Microsoft Azure kamu bulutundaki](https://azure.microsoft.com/features/gov/) ABD kamu bulutudur.

Bu bulut DirSync önceki sürümleri tarafından desteklenen sahip. Derlemeden Azure AD Connect 1.1.180, bulutta yeni nesil desteklenir. Bu, yalnızca ABD tabanlı uç noktalarını kullanarak ve farklı bir proxy sunucunuz açmak için URL'lerin listesini sahiptir.

| URL'lerin Ara sunucuda açmak için |
| --- |
| \*. microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (otomatik Azure kamu kiracısı algılama için gereklidir) |
| \*.gov.us.microsoftonline.com |
| \+ Sertifika iptal listeleri |

> [!NOTE]
> Azure AD Connect itibaren sürüm 1.1.647.0, kayıt defterinde AzureInstance değeri ayarı artık, sağlanan gerekli, *. proxy sunucularınızda windows.net açıksa. Ancak, kendi Azure AD Connect sunucuları Internet bağlantısı izin vermeyen müşteriler için aşağıdaki el ile yapılandırma kullanılabilir.

### <a name="manual-configuration"></a>El ile yapılandırma

Aşağıdaki el ile yapılandırma adımları, Azure AD Connect kullanan Azure kamu eşitleme uç noktaları emin olmak için kullanılır.

1. Azure AD Connect yüklemesi başlatın.
2. Burada, EULA kabul etmek için gereken ilk sayfasını gördüğünüzde, devam eder ancak Yükleme Sihirbazı'nı çalıştıran bırakın.
3. Regedit komutunu çalıştırın ve kayıt defteri anahtarını `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` değerine `4`.
4. Azure AD Connect Yükleme Sihirbazı'için geri EULA'yı kabul edin ve devam gidin. Yükleme sırasında kullandığınızdan emin olun **özel yapılandırma** yükleme yolu (ve hızlı yükleme), ardından devam yüklemeyi her zaman olduğu gibi.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
