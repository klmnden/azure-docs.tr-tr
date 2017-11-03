---
title: "Azure AD raporlama API'si ile çalışmaya başlama | Microsoft Docs"
description: "Azure Active raporlama API'si ile dizinde nereden başlayacaksınız"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 8813b911-a4ec-4234-8474-2eef9afea11e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/18/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 9944cbd2b1b7c4acb18d37da1394c0bbc170f77d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="getting-started-with-the-azure-active-directory-reporting-api"></a>Azure Active raporlama API'si Directory ile çalışmaya başlama

Azure Active Directory ile çok çeşitli raporlar sağlar. Bu raporların verileri SIEM sistemleri, denetim ve iş zekası araçları gibi uygulamalarınız için çok yararlı olabilir. Azure AD raporlama API'leri, bir dizi REST tabanlı API aracılığıyla verilere programlı erişim sağlar. Çeşitli programlama dilleri ve araçlarından bu API'leri çağırabilirsiniz.

Bu makalede, Azure AD ile çalışmaya başlamak gereken bilgilerle API'leri raporlama sağlar.
Sonraki bölümde, Denetim kullanma hakkında daha fazla ayrıntı ve API oturum açın. 

Sık sorulan sorular için okuma bizim [SSS](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-reporting-faq). Sorunlar için lütfen [bir destek bileti dosya](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto)

## <a name="learning-map"></a>Öğrenme haritası
1. **Hazırlama** -API örneklerinizi test etmeden önce tamamlamanız gereken [Azure AD raporlama API'si erişmek için Önkoşullar](active-directory-reporting-api-prerequisites-azure-portal.md).
2. **Araştır** -raporlama API'leri bir ilk izlenim alın:
   
   * [Denetim API'si örnekleri kullanma](active-directory-reporting-api-audit-samples.md) 
   * [Oturum açma etkinliği raporu API örnekleri kullanma](active-directory-reporting-api-sign-in-activity-samples.md)
3. **Özelleştirme** -kendi çözümü oluşturun: 
   
   * [API Başvurusu denetim kullanma](active-directory-reporting-api-audit-reference.md) 
   * [Oturum açma etkinliği raporu API başvurusunu kullanma](active-directory-reporting-api-sign-in-activity-reference.md)

## <a name="next-steps"></a>Sonraki Adımlar
Tüm mevcut Azure AD Graph API uç noktaları görmek merak ediyorsanız bu bağlantıyı kullanın: [https://graph.windows.net/tenant-name/activities/$ metadata? api-version beta =](https://graph.windows.net/tenant-name/activities/$metadata?api-version=beta).

