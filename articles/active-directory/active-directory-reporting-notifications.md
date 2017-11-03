---
title: Azure Active Directory raporlama bildirimleri
description: "Azure Active Directory'ı bildirimleri şüpheli oturum için raporlama kullanma bileşenleri."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: e561061cadd88e2c5670e27f2a66ef21002e30b0
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-reporting-notifications"></a>Azure Active Directory raporlama bildirimleri
## <a name="what-reports-generate-email-notifications"></a>E-posta bildirimleri hangi raporlar oluşturur
Şu anda yalnızca düzensiz oturum açma etkinliği raporu Tetikleyicileri e-posta bildirimleri.

## <a name="what-is-an-irregular-sign-in"></a>Bir "düzensiz oturum açma" nedir?
Düzensiz oturum açma işlemleri bizim machine learning algoritmaları, bir oturum açma anormal konum ve cihaz ile birlikte bir "mümkün olmayan seyahat" koşulu temel alarak tarafından tanımlanmış izinlerdir. Bu, bir bilgisayar korsanının bu hesabı kullanarak oturum açmaya çalışırken gösteriyor olabilir.

## <a name="who-receives-the-email-notifications"></a>Kimlerin e-posta bildirimleri alacağını?
Bir Active Directory Premium lisansı atanmış olan tüm genel yöneticilere e-posta gönderilir. Teslim emin olmak için biz bunu yöneticileri için alternatif e-posta adresi de gönderin. Yöneticileri içermelidir aad-alerts-noreply@mail.windowsazure.com e-posta kaçırmamak için kendi Güvenilir Gönderenler listesinde.

## <a name="how-often-are-these-emails-sent"></a>Gönderilen bu e-postaları ne sıklıkla misiniz?
E-posta, 10 yeni düzensiz oturum açma etkinliklerini son 30 gün içinde ortaya ya da son e-posta gönderilip gönderilmediğini olduğundan, hangisi daha az ise gönderilir.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Belirtilen e-posta ile rapor nasıl erişirim?
Bağlantıya tıkladığınızda, Klasik Azure portalı içinde rapor sayfasına yönlendirileceksiniz. Rapora erişmek için her ikisinin de olması gerekir:

* Bir yönetici veya Azure aboneliğinizin ortak yönetici
* Dizinde genel yönetici ve bir Active Directory Premium lisansı atanmış. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Bu e-postaları kapatabilir miyim?
Evet, Klasik Azure portalı içinde anormal oturum açma işlemlerine ilgili bildirimler devre dışı bırakmak için tıklatın **yapılandırma**ve ardından **devre dışı** altında **bildirimleri** bölüm.

## <a name="whats-next"></a>Sırada ne var?
* Hangi güvenlik, Denetim ve etkinlik raporları kullanılabilir merak ediyor? Kullanıma [Azure AD güvenlik, Denetim ve etkinlik raporları](active-directory-view-access-usage-reports.md)
* [Azure Active Directory Premium ile çalışmaya başlama](active-directory-get-started-premium.md)
* [Oturum Açma ve Erişim Paneli sayfalarınıza şirket markası ekleme](active-directory-add-company-branding.md)

