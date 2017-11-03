---
title: "Erişim paneli uygulamaları nasıl göründüğünü | Microsoft Docs"
description: "Bir uygulama erişim panelinde görünen neden sorun giderme"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewr: japere
ms.openlocfilehash: f8ccf2cf66b49940bc7f2b9f4764020efc04838e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-applications-appear-on-the-access-panel"></a>Erişim paneli uygulamaları nasıl göründüğünü

Erişim için bir iş veya Okul hesabı Azure Active Directory'de (görüntülemek ve Azure AD Yöneticisi bunları erişim izni bulut tabanlı uygulamalar başlatmak için Azure AD) olan bir kullanıcı sağlayan bir web tabanlı portal panosudur. Bu uygulamalar, Azure AD portalında kullanıcı adına yapılandırılır. Yönetici kullanıcının uygulamaya doğrudan sağlayabilir veya bir gruba bir kullanıcı kullanıcının erişim panelinde görünen uygulama sonuçta parçasıdır.

## <a name="general-issues-to-check-first"></a>İlk denetlemek için genel sorunlar

-   Bir uygulamayı yalnızca bir kullanıcı veya kullanıcının üyesi olduğu Grup kaldırıldıysa, içeri ve dışarı kullanıcının erişim paneline birkaç dakika sonra uygulama kaldırılırsa, görmek için oturum yeniden deneyin.

-   Bir lisans yalnızca bir kullanıcı veya grup kaldırıldıysa bu üyesi boyutu ve karmaşıklığı grubunun yapılacak değişiklikler için bağlı olarak uzun bir süre devam edebilir kullanıcıdır. Erişim paneline oturum açmadan önce ek süresi için izin verin.

## <a name="problems-related-to-assigning-applications-to-users"></a>Uygulamaları kullanıcılara atamak için ilgili sorunlar

Bunlar daha önce kendisine atanmış çünkü bir kullanıcı bir uygulama kullanıcıların erişim panelinde görüyor olabilirsiniz. Aşağıda denetlemek için bazı yöntemler şunlardır:

-   [Kullanıcı uygulamaya atandığından emin olun](#check-if-a-user-is-assigned-to-the-application)

-   [Bir kullanıcının uygulamayla ilgili bir lisansı altında olup olmadığını denetleyin](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Kullanıcı uygulamaya atandığından emin olun

Kullanıcı uygulamaya atanıp atanmadığını denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

6.  **Arama** için söz konusu uygulamanın adı.

7.  tıklatın **kullanıcılar ve gruplar**.

8.  Kullanıcı uygulamaya atanıp atanmadığını denetleyin.

  * Uygulamasından kullanıcıyı kaldırmak istiyorsanız, **satıra tıklayın** seçin ve kullanıcı **silmek**.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Bir kullanıcının uygulamayla ilgili bir lisansı altında olup olmadığını denetleyin

Bir kullanıcının atanan lisansları denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

   * Kullanıcı bir Office atanırsa, kullanıcının erişim panelinde için bu etkinleştir birinci taraf Office uygulamalarını lisans.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Gruplarına atamayla ilgili sorunlar

Uygulama atanmış bir grubun parçası olmadığından bir kullanıcı bir uygulama kullanıcıların erişim panelinde görüyor olabilirsiniz. Aşağıda denetlemek için bazı yöntemler şunlardır:

-   [Bir kullanıcı grup üyeliklerini denetleyin](#check-a-users-group-memberships)

-   [Bir kullanıcı için bir lisans atanması grubunun bir üyesi olup olmadığını denetleyin](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Bir kullanıcı grup üyeliklerini denetleyin

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **gruplar.**

8.  Kullanıcı uygulamaya atanan bir grubun parçası olup olmadığını denetleyin.

   * Kullanıcıyı gruptan kaldırmak istiyorsanız, **satıra tıklayın** seçin ve Grup DELETE.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Bir kullanıcı için bir lisans atanması grubunun bir üyesi olup olmadığını denetleyin

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **gruplar.**

8.  belirli bir grup satıra tıklayın.

9.  tıklatın **lisansları** hangi grubun lisansları görmek için atanır.

  * Bu kullanıcının erişim panelinde için belirli birinci taraf Office uygulamalarını etkinleştirebilir bir Office lisansı için Grup atanmışsa.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözümleme yaparsanız

bir destek bileti aşağıdaki bilgilerle varsa açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesine hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile uygulamaları yönetme](active-directory-enable-sso-scenario.md)
