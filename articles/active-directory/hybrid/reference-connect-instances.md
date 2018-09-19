---
title: 'Azure AD Connect: Eşitleme hizmeti örnekleri | Microsoft Docs'
description: Bu sayfa, Azure AD örneğinde ilgili özel konular listelenmiştir.
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
ms.date: 06/18/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a41b236182c18a83b6c83a38fd8420a013313d56
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46315094"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Özel konular için örnekleri
Azure AD Connect ile Azure AD örneğini dünya çapında en yaygın olarak kullanılan ve Office 365. Ancak diğer örnekleri de vardır ve bu URL'leri ve diğer özel durumlar için farklı gereksinimler vardır.

## <a name="microsoft-cloud-germany"></a>Microsoft Bulut Almanya
[Microsoft Cloud Almanya](http://www.microsoft.de/cloud-deutschland) bir Alman veri Emanetçisi tarafından işletilir bağımsız bir bulut.

| URL'lerin Ara sunucuda açmak için |
| --- |
| \*.microsoftonline.de |
| \*.windows.net |
| + Sertifika iptal listeleri |

Azure AD kiracınız ile oturum açtığınızda onmicrosoft.de etki alanında bir hesap kullanmanız gerekir.

Microsoft Cloud Almanya'da şu anda mevcut özellikler:

* **Parola geri yazma** Önizleme ile Azure AD Connect sürümünüzü 1.1.570.0 ve sonra kullanılabilir.
* Diğer Azure AD Premium hizmetlerine kullanılabilir değil.

## <a name="microsoft-azure-government-cloud"></a>Microsoft Azure kamu Bulutu
[Microsoft Azure kamu bulutundaki](https://azure.microsoft.com/features/gov/) ABD kamu bulutudur.

Bu bulut DirSync önceki sürümleri tarafından desteklenen sahip. Derlemeden Azure AD Connect 1.1.180, bulutta yeni nesil desteklenir. Bu, yalnızca ABD tabanlı uç noktalarını kullanarak ve farklı bir proxy sunucunuz açmak için URL'lerin listesini sahiptir.

| URL'lerin Ara sunucuda açmak için |
| --- |
| \*.microsoftonline.com |
| \*.microsoftonline.us |
| \*. windows.net (otomatik gerekli Azure AD kamu kiracısı algılama) |
| \*.gov.us.microsoftonline.com |
| + Sertifika iptal listeleri |

> [!NOTE]
> AAD Connect sürümü itibarıyla 1.1.647.0, kayıt defterinde AzureInstance değeri ayarı artık, sağlanan gereklidir *. proxy sunucularınızda windows.net açıksa.

Microsoft Azure kamu bulutunda şu anda mevcut özellikler:

* **Parola geri yazma** Önizleme ile Azure AD Connect sürümünüzü 1.1.570.0 ve sonra kullanılabilir.
* Diğer Azure AD Premium hizmetlerine kullanılabilir değil.

## <a name="next-steps"></a>Sonraki adımlar
[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
