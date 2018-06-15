---
title: Erişim paneli uygulamaları nasıl göründüğünü | Microsoft Docs
description: Bir uygulama erişim panelinde görünen neden sorun giderme
services: active-directory
documentationcenter: ''
author: ajamess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviewr: japere
ms.openlocfilehash: 0d4203ea39adf027d783a482198f523e18cbc246
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
ms.locfileid: "34054335"
---
# <a name="how-applications-appear-on-the-access-panel"></a>Erişim paneli uygulamaları nasıl göründüğünü

Erişim için bir iş veya Okul hesabı Azure Active Directory'de (görüntülemek ve Azure AD Yöneticisi bunları erişim izni bulut tabanlı uygulamalar başlatmak için Azure AD) olan bir kullanıcı sağlayan bir web tabanlı portal, panosudur. Bu uygulamalar, Azure AD portalında kullanıcı adına yapılandırılır. Yönetici kullanıcının uygulamaya doğrudan sağlayabilir veya bir gruba bir kullanıcı kullanıcının erişim panelinde görünen uygulama sonuçta parçasıdır.

## <a name="general-issues-to-check-first"></a>İlk denetlemek için genel sorunlar

-   Bir uygulama bir kullanıcı veya kullanıcının üyesi olduğu Grup kaldırılmışsa, içeri ve dışarı kullanıcının erişim paneline birkaç dakika sonra uygulama kaldırılırsa, görmek için oturum yeniden deneyin.

-   Bir lisans bir kullanıcı veya grup kaldırıldıysa bu üyesi boyutu ve karmaşıklığı grubunun yapılacak değişiklikler için bağlı olarak uzun bir süre devam edebilir kullanıcıdır. Erişim paneline oturum açmadan önce ek süresi için izin verin.

## <a name="problems-related-to-assigning-applications-to-users"></a>Uygulamaları kullanıcılara atamak için ilgili sorunlar

Bunlar daha önce kendisine atanmış çünkü bir kullanıcı bir uygulama kullanıcıların erişim panelinde görüyor olabilirsiniz. Denetlemek için bazı yöntemler şunlardır:

-   [Kullanıcı uygulamaya atandığından emin olun](#check-if-a-user-is-assigned-to-the-application)

-   [Bir kullanıcının uygulamayla ilgili bir lisansı altında olup olmadığını denetleyin](#check-if-a-user-is-under-a-license-related-to-the-application)


### <a name="check-if-a-user-is-assigned-to-the-application"></a>Kullanıcı uygulamaya atandığından emin olun

Kullanıcı uygulamaya atanıp atanmadığını denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

6.  **Arama** için söz konusu uygulamanın adı.

7.  tıklatın **kullanıcılar ve gruplar**.

8.  Kullanıcı uygulamaya atanıp atanmadığını denetleyin.

  * Uygulamasından kullanıcıyı kaldırmak istiyorsanız, **satıra tıklayın** seçin ve kullanıcı **silmek**.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Bir kullanıcının uygulamayla ilgili bir lisansı altında olup olmadığını denetleyin

Bir kullanıcının atanan lisansları denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **lisansları** , şu anda kullanıcı lisansları görmek üzere atanır.

   * Kullanıcı bir Office lisansı atanırsa, bu kullanıcının erişim panelinde için birinci taraf Office uygulamalarını sağlar.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Gruplarına atamayla ilgili sorunlar

Uygulama atanmış bir grubun parçası olmadığından bir kullanıcı bir uygulama kullanıcıların erişim panelinde görüyor olabilirsiniz. Denetlemek için bazı yöntemler şunlardır:

-   [Bir kullanıcı grup üyeliklerini denetleyin](#check-a-users-group-memberships)

-   [Bir kullanıcı için bir lisans atanması grubunun bir üyesi olup olmadığını denetleyin](#check-if-a-user-is-a-member-of-a-group-assigned-to-a-license)

### <a name="check-a-users-group-memberships"></a>Bir kullanıcı grup üyeliklerini denetleyin

Bir grubun üyeliğini denetlemek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **gruplar.**

8.  Kullanıcı uygulamaya atanan bir grubun parçası olup olmadığını denetleyin.

   * Kullanıcıyı gruptan kaldırmak istiyorsanız, **satıra tıklayın** seçin ve Grup DELETE.

### <a name="check-if-a-user-is-a-member-of-a-group-assigned-to-a-license"></a>Bir kullanıcı için bir lisans atanması grubunun bir üyesi olup olmadığını denetleyin

1.  Açık [ **Azure portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  tıklatın **tüm kullanıcılar**.

6.  **Arama** ilgilendiğiniz kullanıcı için ve **satıra tıklayın** seçin.

7.  tıklatın **gruplar.**

8.  belirli bir grup satıra tıklayın.

9.  tıklatın **lisansları** hangi grubun lisansları görmek için atanır.

  * Grup için bir Office lisansı atanırsa, bu kullanıcının erişim panelinde için belirli birinci taraf Office uygulamalarını sağlar.


## <a name="if-these-troubleshooting-steps-do-not-the-resolve-the-issue"></a>Bu sorun giderme adımları sorunu çözümleme yaparsanız

bir destek bileti aşağıdaki bilgilerle varsa açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı Kimliği

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesine hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Uygulamaları Azure Active Directory ile yönetme](manage-apps/what-is-application-management.md)
