---
title: 'Azure AD Connect: Eşitleme hizmeti örnekleri | Microsoft Docs'
description: Bu sayfa, Azure AD örnekleri için özel hususlar belgeler.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: f340ea11-8ff5-4ae6-b09d-e939c76355a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/26/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 158a0f6d948172ec7d986703e9fa95dd19bdde6a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34592271"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Özel konular örnekleri
Azure AD Connect ile Azure AD dünya çapındaki örneğini en yaygın olarak kullanılan ve Office 365. Ancak aynı zamanda diğer örneği vardır ve bu URL'leri ve diğer özel durumlar için farklı gereksinimlere sahip.

## <a name="microsoft-cloud-germany"></a>Microsoft Bulut Almanya
[Microsoft Cloud Almanya](http://www.microsoft.de/cloud-deutschland) Almanca veri güvenliği tarafından işletilen bir sovereign bulut.

| Proxy sunucu açmak için URL'leri |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| + Sertifika iptal listeleri |

Azure AD kiracınıza oturum açtığınızda onmicrosoft.de etki alanındaki bir hesabı kullanmanız gerekir.

Özellikleri Microsoft Cloud Almanya şu anda yok:

* **Azure AD Connect Health** kullanılabilir değil.
* **Otomatik Güncelleştirmeler** kullanılabilir değil.
* **Parola geri yazma** 1.1.570.0 Azure AD Connect sürümüyle ve sonra Önizleme için kullanılabilir.
* Diğer Azure AD Premium hizmetlerine kullanılabilir değil.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure kamu bulut
[Microsoft Azure kamu bulut](https://azure.microsoft.com/features/gov/) ABD devlet kurumları için bir bulut.

Bu bulut DirSync önceki sürümleri tarafından desteklenen. Yapıdan Azure AD Connect 1.1.180, yeni nesil bulut desteklenir. Bu oluşturma yalnızca ABD tabanlı uç noktalarını kullanarak ve URL'leri proxy sunucunuzun açmak için farklı bir listesi vardır.

| Proxy sunucu açmak için URL'leri |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (otomatik gerekli Azure AD kamu Kiracı algılama) |
| \*.gov.us.microsoftonline.com |
| + Sertifika iptal listeleri |

> [!NOTE]
> AAD bağlanma sürümü itibariyle 1.1.647.0, kayıt defterinde AzureInstance değeri ayarı artık koşuluyla gereklidir *. windows.net, proxy sunucuları açık.

Özellikleri Microsoft Azure kamu bulutta şu anda yok:

* **Azure AD Connect Health** kullanılabilir değil.
* **Otomatik Güncelleştirmeler** kullanılabilir değil.
* **Parola geri yazma** 1.1.570.0 Azure AD Connect sürümüyle ve sonra Önizleme için kullanılabilir.
* Diğer Azure AD Premium hizmetlerine kullanılabilir değil.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
