---
title: "\"Buradan oraya ulaşamazsınız\" hata - Azure Active Directory sorunlarını giderme | Microsoft Docs"
description: "\"Buradan oraya ulaşamazsınız\" hata iletisi karşılaşacağınız olası nedenleri giderin."
services: active-directory
author: eross-msft
manager: daveba
ms.assetid: 8ad0156c-0812-4855-8563-6fbff6194174
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lizross
ms.reviewer: jairoc
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a317680a39d4594aacdf84ccdf963bb84bfbf07b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60473812"
---
# <a name="potential-reasons-for-the-you-cant-get-there-from-here-error-message"></a>Olası nedenleri "Buradan oraya ulaşamazsınız" hata iletisi
Kuruluşunuzun iç web uygulamalarına veya hizmetlerine erişirken yazan bir hata iletisi alabilirsiniz **oraya buradan ulaşamazsınız**. Bu ileti, kuruluşunuz cihazınızın, kuruluşunuzun kaynaklarına erişimi engelleyen bir yerde bir ilke getirdi anlamına gelir. Bu sorunu gidermek için yardım masasına başvurmak zorunda son, ancak ilk deneyebileceğiniz birkaç şey aşağıda verilmiştir.

## <a name="make-sure-youre-using-a-supported-browser"></a>Desteklenen bir tarayıcı kullandığınızdan emin olun
Alırsanız **oraya buradan ulaşamazsınız** çalıştırdığınızdan, çalıştığınız, desteklenmeyen bir tarayıcıdan kuruluşunuzun sitelerine erişmek tarayıcıyı denetleyin belirten bir ileti.

![Tarayıcı destek ile ilgili hata iletisi](media/user-help-device-remediation/browser-version.png)

Bu sorunu gidermek için yükleme ve desteklenen bir tarayıcı, işletim sisteminize göre çalıştırın. Windows 10 kullanıyorsanız, desteklenen tarayıcılar Microsoft Edge, Internet Explorer ve Google Chrome içerir. Farklı bir işletim sistemi kullanıyorsanız, tam listesini denetleyebilirsiniz [Desteklenen tarayıcılar](../conditional-access/technical-reference.md#supported-browsers).

## <a name="make-sure-youre-using-a-supported-operating-system"></a>Desteklenen bir işletim sistemi kullandığınızdan emin olun
İşletim sisteminin desteklenen bir sürümünü çalıştırdığınızdan emin olun dahil olmak üzere:

- **Windows İstemcisi.** Windows 7 veya üzeri.

- **Windows Server.** Windows Server 2008 R2 veya üzeri.

- **macOS.** macOS X veya üzeri

- **Android ve iOS.** Android ve iOS mobil işletim sistemlerini en son sürümünü

Bu sorunu gidermek için yükleme ve desteklenen bir işletim sistemi çalıştırması gerekir.

## <a name="make-sure-your-device-is-joined-to-your-network"></a>Cihazınızı ağınıza katıldığından emin olun
Alırsanız **oraya buradan ulaşamazsınız** cihazınızın uyumluluğunu dışı olduğunu belirten bir ileti, kuruluşunuzun erişim ilkesi ile kuruluşunuzun ağa Cihazınızı katılmış emin olun.

![Ağınızda olmanıza için ilgili hata iletisi](media/user-help-device-remediation/network-version.png)

### <a name="to-check-whether-your-device-is-joined-to-your-network"></a>Cihazınızı ağınıza hangisine katılması gerektiğini denetlemek için
1. İçin Windows iş veya Okul hesabınızı kullanarak oturum açın. Örneğin, alain@contoso.com.

2. Kuruluşunuzun ağa bir sanal özel ağ (VPN) veya DirectAccess aracılığıyla bağlanın.

3. Bağlandıktan sonra basın **Windows logosu tuşu + L** Cihazınızı kilitlemek üzere.

4. Çalışmanızı kullanarak cihazınızın kilidini açın ya da Okul hesabı ve sorunlu uygulamaya erişmek veya hizmetini yeniden deneyin.

    Görürseniz **oraya buradan ulaşamazsınız** hata iletisini tekrar seçin **daha fazla ayrıntı** bağlantısını ve ardından ayrıntılarını birlikte Yardım Masanızla iletişime geçin.

### <a name="to-join-your-device-to-your-network"></a>Cihazınızı ağınıza katılmak için
Cihazınızı kuruluşunuzun ağa katılmış değilse, bunu ikisinden birini yapabilirsiniz:

- **Cihazınızı çalışma alanına ekleyin.** Potansiyel olarak kısıtlanmış kaynaklara erişebilmesi için iş ait Windows 10 Cihazınızı kuruluşunuzun ağa katılmasını sağlayın. Daha fazla bilgi ve adım adım yönergeler için bkz. [iş cihazınızın, kuruluşunuzun ağına katılın](user-help-join-device-on-network.md).

- **İş için kişisel Cihazınızı kaydedin.** Kişisel Cihazınızı, genellikle bir telefonda veya tablette, kuruluşunuzun ağ kaydedin. Cihazınız kaydedildikten sonra kuruluşunuzun kısıtlanmış kaynaklara erişebilir. Daha fazla bilgi ve adım adım yönergeler için bkz. [kuruluşunuzun ağ üzerindeki kişisel cihazını kaydetmek](user-help-register-device-on-network.md).

## <a name="next-steps"></a>Sonraki adımlar
- [MyApps portalında nedir?](active-directory-saas-access-panel-introduction.md)

- [Parolanızla değil telefonunuzla oturum açma](user-help-auth-app-sign-in.md)
