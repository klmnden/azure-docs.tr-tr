---
title: Yanlış kullanıcı kümesi için Azure AD galeri uygulaması hazırlanıyor | Microsoft Docs
description: Neden farklı bir kullanıcı kümesi hazırlanıyor uygulamaya beklediğiniz olanlardan öğrenmek öğrenin
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: mimart
ms.reviewer: asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef1727c45378e36abf695fca32c6e630806b4a6e
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784496"
---
# <a name="wrong-set-of-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Yanlış kullanıcı kümesi için Azure AD galeri uygulaması hazırlanıyor

Hangi kullanıcıların uygulamaya sağlanan öncelikli olarak hangi kullanıcıların ve grupların silinmiş tarafından yönetilir **atanan** uygulama.

Hangi kullanıcıların ve grupların Azure Active Directory içinde bir uygulamaya atanmış denetlemek nasıl öğrenmek için aşağıdaki kaynakları kullanın.

## <a name="assign-a-user-directly-as-an-administrator"></a>Doğrudan yönetici bir kullanıcı atama

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8. Açmak için **atama Ekle** bölmesinde tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama isteyen kullanıcının **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla kullanıcı eklemek**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına veya e-posta adresine göre arama** Arama kutusuna ve bu kullanıcıyı eklemek için onay kutusunu **seçili** listesi.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

Sağlama, yapılandırılmış ve bir uygulama zaten çalışıyor olması durumunda, yeni kullanıcıların yaklaşık 10 dakika içinde bir uygulamaya sağlanmalıdır. Denetleme **denetim günlüklerini** Ayrıntılar için.

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a>Yönetici olarak bir uygulamaya doğrudan bir grup atayabilir.

Bir veya daha fazla grup, doğrudan uygulamaya atamak için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7. Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8. Açmak için **atama Ekle** bölmesinde tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesi.

9. tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **grubu** göstermek için listedeki bir **onay kutusu**. Grubun profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla Grup Ekle**, başka bir tür **tam grup adı** içine **adına veya e-posta adresine göre arama** arama kutusuna ve bu gruba eklemek için onay kutusuna tıklayın için **seçili** listesi.

13. Grupları seçme işiniz bittiğinde, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz gruplara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi seçili gruplara uygulama atama.

Sağlama, yapılandırılmış ve bir uygulama zaten çalışıyor olması durumunda grup içinde bulunan yeni kullanıcılar uygulamaya yaklaşık 10 dakika içinde sağlanmalıdır. Denetleme **denetim günlüklerini** Ayrıntılar için.

>[!IMPORTANT]
>Grup adını ve üyelerinin yanı sıra grubu ayrıntıları bazı uygulamalar için destekleniyorsa sağlama. Etkinleştirme veya etkinleştirme veya devre dışı bırakarak bu işlevi devre dışı **eşleme** gösterilen grubu nesnelerinin **sağlama** sekmesi. 
>
>

Grupları sağlama etkinse, "Eşleşen kimliği" için uygun bir alanı kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirmeyi unutmayın. Bu eşleşen kimlik görünen ad veya e-posta diğer adı olabilir. Eşleşen özellik boş ya da doldurulmuş bir grup için Azure AD'de ise, Grup ve üyelerini sağlanmayan.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory ile SaaS Uygulamalarına Kullanıcı Hazırlama ve Sağlamayı Kaldırma İşlemlerini Otomatik Hale Getirme](user-provisioning.md)
