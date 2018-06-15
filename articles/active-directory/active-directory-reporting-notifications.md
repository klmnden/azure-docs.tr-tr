---
title: Azure Active Directory raporlama bildirimleri
description: Azure Active Directory'ı bildirimleri şüpheli oturum için raporlama kullanma bileşenleri.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: ae6d4b0e-5931-4cb3-98bf-9be91b381c92
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.component: compliance-reports
ms.date: 01/03/2018
ms.author: dhanyahk;rolyon
ms.custom: oldportal
ms.reviewer: dhanyahk
ms.openlocfilehash: 8f4beece9d1d351c91f2bb055eedc0ae9851f532
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34588197"
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
Bağlantıya tıkladığınızda, Azure portalı içinde rapor sayfasına yönlendirileceksiniz. Rapora erişmek için her ikisinin de olması gerekir:

* Bir yönetici veya Azure aboneliğinizin ortak yönetici
* Dizinde genel yönetici ve bir Active Directory Premium lisansı atanmış. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-whatis.md).

## <a name="can-i-turn-off-these-emails"></a>Bu e-postaları kapatabilir miyim?
Evet, anormal oturum açma işlemlerine Azure portalındaki ilgili bildirimler devre dışı bırakmak için tıklatın **yapılandırma**ve ardından **devre dışı** altında **bildirimleri** bölümü.

## <a name="whats-next"></a>Sırada ne var?
* Hangi güvenlik, Denetim ve etkinlik raporları kullanılabilir merak ediyor? Kullanıma [Azure AD güvenlik, Denetim ve etkinlik raporları](active-directory-view-access-usage-reports.md)
* [Azure Active Directory Premium ile çalışmaya başlama](active-directory-get-started-premium.md)
* [Oturum Açma ve Erişim Paneli sayfalarınıza şirket markası ekleme](customize-branding.md)

