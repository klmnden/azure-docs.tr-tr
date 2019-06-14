---
title: Azure AD Connect yükleme sorunlarını giderme | Microsoft Docs
description: Bu konuda, Azure AD Connect yükleme sorunlarını gidermek için adımları sağlar.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: e077127681f8bd7b650ab22f2d036efd7f9733ee
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60454804"
---
# <a name="troubleshoot-azure-ad-connect-install-issues"></a>Sorun giderme: Azure AD Connect yükleme sorunları

## <a name="recommended-steps"></a>**Önerilen Adımlar**
Lütfen hangi denetleyin [Azure AD Connect yükleme türünü](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-select-installation) sizin için uygundur. Hızlı yükleme ölçütlerini karşılıyorsanız, ardından, hızlı yükleme ile gitmenizi öneririz. Hızlı yükleme yükleme işleminin tamamlanması için gereken en düşük seçenekleri sağlar, bu nedenle daha az olasılığını herhangi bir sorun yoktur. 

Hızlı yükleme ölçütlerine uymayan ve en iyi yöntemlerden bazıları aşağıda verilmiştir sonra özel bir yükleme yapmanız gerekir, ancak sık karşılaşılan sorunları önlemek için izleyebilirsiniz. Basitleştirmek amacıyla yalnızca seçmeli seçenekleri aşağıda belirtilmiştir:

* AAD Connect'i yüklüyorsanız makinede bir yönetici olduğundan emin olun. Üzerinde makine aynı yönetici kimlik bilgileriyle oturum açın.

* Mevcut SQL Server kullanmak istiyorsanız "Kullanımı mevcut bir SQL Server" dışında aşağıdaki sayfasında, varsayılan olarak tüm seçenekler sağlar. İşte [daha fazla ayrıntı](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-custom) özel yükleme seçenekleri kullanma hakkında. 

    ![Mevcut SQL Server'ı kullanın](media/tshoot-connect-install-issues/tshoot-connect-install-issues/useexistingsqlserver.png)

* Herhangi bir izni önlemek için sorunları var olan bir hesapla, şu sayfada "Oluştur yeni AD hesabı" seçeneğini seçin.

    ![AD ormanı hesabı](media/tshoot-connect-install-issues/tshoot-connect-install-issues/createnewaccount.png)

### <a name="common-issues"></a>**Genel Sorunlar**

* [Şirket içi Active Directory ile ilgili bağlantı sorunlarını](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-adconnectivitytools).

* [Çevrimiçi Azure Active Directory ile ilgili bağlantı sorunlarını](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-connectivity).

* [Şirket içi Active Directory ile izin sorunları](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-configure-ad-ds-connector-account).

## <a name="recommended-documents"></a>**Önerilen Belgeler**
* [Azure AD Connect Önkoşulları](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-prerequisites)
* [Azure AD Connect için hangi yükleme türünün kullanılacağını seçme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-select-installation)
* [Hızlı ayarları kullanarak Azure AD Connect ile çalışmaya başlama](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express)
* [Azure AD Connect özel yüklemesi](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-custom)
* [Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-upgrade-previous-version)
* [Azure AD Connect: Hazırlama sunucusu nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/plan-connect-topologies#staging-server)
* [ADConnectivityTool PowerShell Modülü nedir?](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-adconnectivitytools)

## <a name="next-steps"></a>Sonraki adımlar
- [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md).
- [Karma kimlik nedir? ](whatis-hybrid-identity.md).





