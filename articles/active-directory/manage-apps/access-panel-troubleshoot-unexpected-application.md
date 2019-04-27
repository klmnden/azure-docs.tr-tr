---
title: Uygulama erişim panelinde nasıl göründüğünü | Microsoft Docs
description: Bir uygulamanın erişim panelinde gösterildiğini neden sorunlarını giderme
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: celested
ms.reviewr: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: fccf671edbc121501a17975be303453a798837e4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60442837"
---
# <a name="how-applications-appear-on-the-access-panel"></a>Uygulama erişim panelinde nasıl görünür?

Erişim paneli için bir kullanıcı Azure Active Directory (görüntüleyin ve bulut tabanlı uygulamalar Azure AD Yöneticisi bunları erişim izni vermiştir başlatmak için Azure AD) bir iş veya Okul hesabıyla sağlayan bir web tabanlı, portalıdır. Bu uygulamalar kullanıcılar Azure AD portalında yapılandırılır. Yönetici kullanıcının uygulamaya doğrudan sağlayabilir veya bir gruba bir kullanıcı, uygulama kullanıcının erişim panelinde görünmesini výsledek parçasıdır.

## <a name="general-issues-to-check-first"></a>İlk denetlenecek genel sorunlar

-   Bir uygulama bir kullanıcı ya da kullanıcının üyesi olduğu Grup kaldırıldıysa, giriş ve çıkış yeniden kullanıcının erişim paneline birkaç dakika sonra uygulama kaldırılıp kaldırılmadığını görmek için oturum açmayı deneyin.

-   Lisans, bir kullanıcı veya grup kaldırılmışsa bu üye, boyutu ve karmaşıklığı yapılacak değişiklikler grubuna bağlı olarak uzun zaman alabilir kullanıcıdır. Erişim paneline oturum açmadan önce ek süre izin verir.

## <a name="problems-related-to-assigning-applications-to-users"></a>Uygulamaları kullanıcılara atamak için ilgili sorunlar

Çünkü bunlar daha önce kendisine atanmış bir kullanıcı bir uygulamanın erişim panelinde görüyor olabilirsiniz. Denetlemek için bazı yollar şunlardır:

-   [Bir kullanıcı uygulamaya atanıp atanmadığını kontrol edin](#check-if-a-user-is-assigned-to-the-application)

-   [Bir kullanıcının uygulamayla ilgili bir lisans olup olmadığını denetleyin](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Bir kullanıcı uygulamaya atanıp atanmadığını kontrol edin

Bir kullanıcı uygulamaya atanıp atanmadığını kontrol etmek için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

6. **Arama** söz konusu uygulamanın adı.

7. tıklayın **kullanıcılar ve gruplar**.

8. Kullanıcı uygulamaya atanıp atanmadığını görmek için kontrol edin.

   * Kullanıcı, uygulamayı kaldırmak istiyorsanız **satıra tıklayın** seçin ve kullanıcı **Sil**.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Bir kullanıcının uygulamayla ilgili bir lisans olup olmadığını denetleyin

Bir kullanıcının lisans atanmış denetlemek için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm kullanıcılar**.

6. **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7. tıklayın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

   * Bu, kullanıcı bir Office lisansı atanmamışsa, birinci taraf Office uygulamaları, kullanıcının erişim panelinde görünmesini sağlar.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Uygulamaları gruplara atama için ilgili sorunlar

Atanan uygulama bir grubun parçası olduklarından bir kullanıcı bir uygulamanın erişim panelinde görüyor olabilirsiniz. Denetlemek için bazı yollar şunlardır:

-   [Bir kullanıcının grup üyeliklerini denetle](#check-a-users-group-memberships)

-   [Bir kullanıcı için bir lisans atanmış bir grubunun bir üyesi olup olmadığını denetleyin](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Bir kullanıcının grup üyeliklerini denetle

Bir grubun üyeliğini denetlemek için şu adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm kullanıcılar**.

6. **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7. tıklayın **grupları.**

8. Kullanıcı uygulamaya atanmış bir gruba ait olup olmadığını denetleyin.

   * Kullanıcıyı gruptan kaldırmak istiyorsanız **satıra tıklayın** seçin ve Grup DELETE.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Bir kullanıcı için bir lisans atanmış bir grubunun bir üyesi olup olmadığını denetleyin

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5. tıklayın **tüm kullanıcılar**.

6. **Arama** ilgilendiğiniz kullanıcı ve **satıra tıklayın** seçin.

7. tıklayın **grupları.**

8. belirli bir grubun satıra tıklayın.

9. tıklayın **lisansları** hangi Grup lisansları görmek için atanmış durumda.

   * Grup için bir Office lisansı atanmışsa, bu belirli birinci taraf Office uygulamaları, kullanıcının erişim panelinde görünmesini sağlayabilir.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımlarını sorunu çözümleme durumunda

Aşağıdaki bilgilerle varsa bir destek bileti açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesi sırasında hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](what-is-application-management.md)
