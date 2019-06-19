---
title: Microsoft 365 kamu - Azure Active Directory Azure Active Directory'deki yenilikler | Microsoft Docs
description: Azure Active Directory (Azure AD), etkileyebilecek Microsoft 365 kamu bulut örneği bazı değişiklikler hakkında bilgi edinin.
services: active-directory
author: eross-msft
manager: daveba
ms.author: lizross
ms.reviewer: sumitp
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 05/07/2019
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5b077c7b5efbad2add971d42ff31938b56f6bc33
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66258902"
---
# <a name="whats-new-for-azure-active-directory-in-microsoft-365-government"></a>Microsoft 365 kamu Azure Active Directory'deki yenilikler

Aşağıdaki hizmetler kullanan müşteriler için geçerlidir Microsoft 365 kamu bulut örneğindeki size Azure Active Directory (Azure AD) bazı değişiklikler yaptık:

- Microsoft Azure Kamu

- Microsoft 365 kamu – GCC yüksek

- Microsoft 365 kamu – DoD

Bu makalede, Microsoft 365 kamu – GCC müşterileri için geçerli değildir.

## <a name="changes-to-the-initial-domain-name"></a>İlk etki alanı adında yapılan değişiklikler

Kuruluşunuzun ilk kurulum sırasında Microsoft 365 kamu çevrimiçi hizmet için kuruluşunuzun etki alanı adı seçmeniz istendiğinde `<your-domain-name>.onmicrosoft.com`. .Com sonekine sahip bir etki alanı adı zaten varsa, hiçbir şey değişecektir.

Yeni bir Microsoft 365 kamu hizmete kaydolan, ancak, bir etki alanı adını kullanarak seçin istenir `.us` soneki. Bu nedenle, olur `<your-domain-name>.onmicrosoft.us`.

>[!Note]
>Bu değişiklik, bulut hizmeti sağlayıcıları (CSP'ler) tarafından yönetilen tüm müşteriler için geçerli değildir.

## <a name="changes-to-portal-access"></a>Portal erişimi değişiklikleri

Portal uç noktaları için Microsoft Azure kamu, Microsoft 365 kamu – GCC yüksek ve Microsoft 365 kamu – DoD, gösterildiği şekilde güncelleştirdik [uç noktası eşleme tablosu](#endpoint-mapping).

Müşteriler dünya çapındaki Azure (portal.azure.com) ve (portal.office.com) Office 365 portallarını kullanarak daha önce oturum açılamadı. Bu güncelleştirme ile müşteriler artık belirli Microsoft Azure kamu, Microsoft 365 kamu - GCC yüksek ve Microsoft 365 kamu - DoD portalı kullanarak oturum açmanız gerekir.

## <a name="endpoint-mapping"></a>Uç noktası eşleme

Aşağıdaki tablo, tüm müşteriler için uç noktalar gösterir:

| Ad | uç noktası ayrıntıları |
|------|------------------|
| Portallar |Microsoft Azure kamu: https://portal.azure.us<p>Microsoft 365 kamu – GCC yüksek: https://portal.office365.us<p>Microsoft 365 kamu – DoD: https://portal.apps.mil |
| Azure Active Directory yetkili uç noktası | https://login.microsoftonline.us |
| Azure Active Directory Graph API'si | https://graph.windows.net |
| Microsoft 365 kamu - GCC için Microsoft Graph API'si yüksek | https://graph.microsoft.us |
| Microsoft 365 kamu - DoD için Microsoft Graph API'si | https://dod-graph.microsoft.us |
| Uç noktalar Azure kamu hizmetleri | Ayrıntılar için bkz [Azure kamu Geliştirici Kılavuzu](https://docs.microsoft.com/azure/azure-government/documentation-government-developer-guide) |
| Microsoft 365 kamu - GCC yüksek uç noktaları | Ayrıntılar için bkz [Office 365 ABD Kamu GCC yüksek uç noktaları](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-gcc-high-endpoints) |
| Microsoft 365 kamu - DoD | Ayrıntılar için bkz [Office 365 ABD Kamu Savunma Bakanlığı uç noktaları](https://docs.microsoft.com/office365/enterprise/office-365-u-s-government-dod-endpoints) |

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için şu makalelere bakın:

- [Azure Government nedir?](https://docs.microsoft.com/azure/azure-government/documentation-government-welcome)

- [Azure kamu AAD yetkili uç noktasını güncelleştirme işlemi](https://devblogs.microsoft.com/azuregov/azure-government-aad-authority-endpoint-update/)

- [ABD kamu bulutunda Microsoft Graph uç noktaları](https://developer.microsoft.com/graph/blogs/new-microsoft-graph-endpoints-in-us-government-cloud/)

- [Office 365 US Government GCC yüksek ve DoD](https://docs.microsoft.com/office365/servicedescriptions/office-365-platform-service-description/office-365-us-government/gcc-high-and-dod)