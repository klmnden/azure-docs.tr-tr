---
title: Kullanım ve öngörüleri Azure Active Directory Portalı'nda rapor | Microsoft Docs
description: Azure Active Directory portalındaki kullanım bilgilerini ve öngörüleri rapor giriş
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 3fba300d-18fc-4355-9924-d8662f563a1f
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: 3fe1e72d277bffd4bc9b38ac377e33b341967e17
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806364"
---
# <a name="usage-and-insights-report-in-the-azure-active-directory-portal"></a>Azure Active Directory portalındaki kullanım bilgilerini ve öngörüleri raporu

Kullanım ve öngörüleri raporunu kullanarak, uygulama odaklı bir oturum açma verilerinizin görünümünü alabilirsiniz. Aşağıdaki soruların yanıtlarını bulabilirsiniz:

*   Üst nelerdir uygulamaları Kuruluşumdaki kullanılan?
*   Hangi uygulamaların en çok başarısız oturum açma işlemleri var mı? 
*   Her uygulama için en çok oturum açma hataları nelerdir?

## <a name="prerequisites"></a>Önkoşullar 

Kullanım ve öngörüleri rapordan verilere erişmek için gerekir:

* Azure AD kiracısı
* Oturum açma verilerini görüntülemek için bir Azure AD premium (ö1/ö2) lisansı
* Bir kullanıcı genel yönetici, Güvenlik Yöneticisi, güvenlik okuyucu veya rapor okuyucu rolleri. Ayrıca, herhangi bir kullanıcı (Yönetici olmayanlar) kendi oturum açma etkinliklerine erişebilir. 

## <a name="access-the-usage-and-insights-report"></a>Kullanım ve öngörüleri rapor erişimi

1. [Azure portalına](https://portal.azure.com) gidin.
2. Doğru dizini seçin ve ardından **Azure Active Directory** ve **kurumsal uygulamalar**.
3. Gelen **etkinlik** bölümünden **kullanım & ınsights** seçerek raporu açın. 

![Kullanım ve İçgörüler raporu](./media/concept-usage-insights-report/main-menu.png)
                                     

## <a name="use-the-report"></a>Kullanım Raporu

Kullanım ve öngörüleri rapor denemesi bir veya daha fazla oturum uygulamaların listesini gösterir ve oturum açma başarılı ve başarısız oturum açma başarı oranı sayısına göre sıralamak sağlar.

Daha fazla bilgi için listenin tıklayarak yük ek uygulamalar sayfasında görüntülemenize olanak sağlar. Aralıkta kullanılan tüm uygulamaları görüntülemek için tarih aralığını seçebilirsiniz.

Belirli bir uygulama odağı de ayarlayabilirsiniz. Seçin **görüntülemek oturum açma etkinliği** en önemli hatalar yanı sıra, uygulama için zaman içinde etkinlik oturum görmek için.  

Uygulama kullanımı grafiğinde bir gün seçtiğinizde, uygulama için oturum açma etkinliklerinin ayrıntılı bir listesini alın.  

![Kullanım ve İçgörüler raporu](./media/concept-usage-insights-report/usage-and-insights-report.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Oturum açma işlemleri raporu](concept-sign-ins.md)