---
title: Azure Active Directory raporlarını kullanarak oturum açma hataları nasıl giderilir | Microsoft Docs
description: Azure portalında Azure Active Directory raporlarını kullanarak oturum açma hatalarında sorun giderme hakkında bilgi edinin
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: db68ad2a29dcaa53d219b679b9e0f24a50a6f576
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60286569"
---
# <a name="how-to-troubleshoot-sign-in-errors-using-azure-active-directory-reports"></a>Nasıl yapılır: Azure Active Directory raporlarını kullanarak oturum açma hatalarında sorun giderme

[Oturum açma işlemleri raporu](concept-sign-ins.md) Azure'da Active Directory (Azure AD), kuruluşunuzda uygulama erişimini yönetme etrafında soruların yanıtlarını bulmanızı sağlar dahil olmak üzere:

- Belirli bir kullanıcının oturum açma düzeni nedir?
- Bir hafta içerisinde kaç adet kullanıcı oturum açtı?
- Bu açılan oturumların durumu nedir?


Ayrıca, oturum açma işlemleri raporu da kuruluşunuzdaki kullanıcılar için oturum açma hatası gidermenize yardımcı olabilir. Bu kılavuzda, oturum açma başarısız oturum açma işlemleri raporu yalıtmak ve hatasının kök nedenini anlamak için kullanma konusunda bilgi edinin.

## <a name="prerequisites"></a>Önkoşullar

Gerekenler:

* Azure AD kiracısı (ö1/ö2) premium lisansına sahip. Bkz: [Azure Active Directory Premium ile çalışmaya başlama](../fundamentals/active-directory-get-started-premium.md) , Azure Active Directory sürümünü yükseltmek için.
* İçinde olan bir kullanıcı **genel yönetici**, **Güvenlik Yöneticisi**, **güvenlik okuyucusu**, veya **rapor okuyucu** kiracının rol. Ayrıca, herhangi bir kullanıcı kendi oturum açma etkinliklerine erişebilir. 

## <a name="troubleshoot-sign-in-errors-using-the-sign-ins-report"></a>Oturum açma işlemleri raporu kullanarak oturum açma hatalarında sorun giderme

1. Gidin [Azure portalında](https://portal.azure.com) ve dizininizi seçin.
2. Seçin **Azure Active Directory** seçip **oturum açma işlemleri** gelen **izleme** bölümü. 
3. Kullanıcı adı veya nesne tanımlayıcısı, uygulama adı veya tarih hatası daraltmak için sağlanan filtreleri kullanın. Ayrıca, seçin **hatası** gelen **durumu** yalnızca başarısız oturum açma işlemlerini görüntülemek için açılan. 

    ![Sonuçları filtreleme](./media/howto-troubleshoot-sign-in-errors/filters.png)
        
4. Başarısız araştırmak istediğiniz oturum belirleyin. Başarısız oturum açma hakkında daha fazla bilgi içeren ek ayrıntıları penceresini açmak için seçin. Not **oturum açma hata kodu** ve **hata nedeni**. 

    ![Kaydı seçin](./media/howto-troubleshoot-sign-in-errors/sign-in-failures.png)
        
5. Bu bilgiler de bulabilirsiniz **sorun giderme ve Destek** Ayrıntıları penceresinde sekmesi.

    ![Sorun giderme ve destek](./media/howto-troubleshoot-sign-in-errors/troubleshooting-and-support.png)

6. Hatanın nedenini hata açıklar. Örneğin, yukarıdaki senaryoda başarısızlık nedeni **geçersiz kullanıcı adı veya parola ya da geçersiz şirket içi kullanıcı adı veya parola**. İçin yalnızca oturum açma doğru kullanıcı adı ve parola ile yeniden açıklanmıştır.

7. Düzeltme için hata kodu için arama yaparak dahil olmak üzere ek bilgiler alabilirsiniz **50126** Bu örnekte, [oturum açma hata kodları başvurusu](reference-sign-ins-error-codes.md). 

8. Tüm denemeler başarısız olursa veya eylemin önerilen kursu alma rağmen sorun devam ederse [bir destek bileti açın](../fundamentals/active-directory-troubleshooting-support-howto.md) yer alan adımları uygulayarak **sorun giderme ve Destek** sekmesi. 

## <a name="next-steps"></a>Sonraki adımlar

* [Oturum açma hata kodları başvurusu](reference-sign-ins-error-codes.md)
* [Oturum açma işlemleri raporu genel bakış](concept-sign-ins.md)
