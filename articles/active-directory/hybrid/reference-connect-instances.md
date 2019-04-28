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
ms.date: 06/18/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ca481d50efb99d6e36c66388192e9f27cd66bf45
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62096105"
---
# <a name="azure-ad-connect-special-considerations-for-instances"></a>Azure AD Connect: Örneklerle ilgili özel konular
Azure AD Connect ile Azure AD örneğini dünya çapında en yaygın olarak kullanılan ve Office 365. Ancak diğer örnekleri de vardır ve bu URL'leri ve diğer özel durumlar için farklı gereksinimler vardır.

## <a name="microsoft-cloud-germany"></a>Microsoft Bulut Almanya
[Microsoft Cloud Almanya](https://www.microsoft.de/cloud-deutschland) bir Alman veri Emanetçisi tarafından işletilir bağımsız bir bulut.

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
