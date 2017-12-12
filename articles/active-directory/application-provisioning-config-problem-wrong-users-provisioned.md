---
title: "Kullanıcı yanlış kümesi, bir Azure AD galeri uygulamaya sağlanan | Microsoft Docs"
description: "Neden farklı bir kullanıcı kümesini sağlanan bir uygulamaya beklediğiniz olandan öğrenmek öğrenin"
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: c9e268aad5d3a7dcde9102216e9b3e8abfcca2cb
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="wrong-set-of-users-are-being-provisioned-to-an-azure-ad-gallery-application"></a>Kullanıcı yanlış kümesi, bir Azure AD galeri uygulamaya sağlanan

Hangi kullanıcıların uygulama için sağlanan öncelikle hangi kullanıcıların ve grupların silinmiş tarafından yönetilir **atanan** uygulama.

Hangi kullanıcıların ve grupların Azure Active Directory içinde bir uygulama atanmış denetlemek öğrenmek için aşağıdaki kaynakları kullanın.

## <a name="assign-a-user-directly-as-an-administrator"></a>Yönetici doğrudan kullanıcı atama

Bir veya daha fazla kullanıcının uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** dikey.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** dikey.

10. Yazın **tam adı** veya **e-posta adresi** içine atama ilgilenen kullanıcının **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **kullanıcı** ortaya çıkarmak için listedeki bir **onay kutusunu**. Kullanıcının profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla kullanıcı ekleme**, başka bir tür **tam adı** veya **e-posta adresi** içine **ad veya e-posta adresine göre arama** arama kutusu ve bu kullanıcıyı eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Kullanıcıların seçerek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** seçtiğiniz kullanıcılara atamak için bir rol seçin dikey.

15. Tıklatın **atamak** uygulamayı Seçilen kullanıcılara atamak için düğmesi.

Sağlama yapılandırılmış ve bir uygulama zaten çalışıyor ise, yeni kullanıcılar yaklaşık 10 dakika içinde bir uygulama için hazırlanması gerekir. Denetleme **denetim günlüklerini** Ayrıntılar için.

## <a name="assign-a-group-directly-to-an-application-as-an-administrator"></a>Yönetici olarak bir uygulamaya doğrudan bir gruba atamak

Bir veya daha fazla grupları doğrudan bir uygulamaya atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklatın **Ekle** üstünde düğmesini **kullanıcılar ve gruplar** açmak için liste **eklemek atama** dikey.

9.  tıklatın **kullanıcılar ve gruplar** seçicisini **eklemek atama** dikey.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **ad veya e-posta adresine göre arama** arama kutusu.

11. Üzerine gelerek **grup** ortaya çıkarmak için listedeki bir **onay kutusunu**. Grubun profil fotoğrafınız veya logosu, kullanıcı eklemek için yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** başlamayı tercih ederseniz **birden fazla grubu Ekle**, başka bir tür **tam grup adı** içine **ad veya e-posta adresine göre arama** arama kutusu ve bu gruba eklemek için onay kutusunu işaretleyin **seçili** listesi.

13. Grupları seçmek bittiğinde tıklatın **seçin** düğmesi uygulamaya atanan kullanıcılar ve gruplar listesi eklemek için.

14. **İsteğe bağlı:** tıklatın **rolü Seç** seçicide **eklemek atama** seçtiğiniz gruplarına atamak için bir rol seçin dikey.

15. Tıklatın **atamak** seçilen grupları uygulamaya atamak için düğmesi.

Sağlama yapılandırılmış ve bir uygulama zaten çalışıyor ise, yeni kullanıcılar grubunda yer alan yaklaşık 10 dakika içinde bir uygulama için hazırlanması gerekir. Denetleme **denetim günlüklerini** Ayrıntılar için.

>[!IMPORTANT]
>Grup adı ve üyeleri yanı sıra Grup ayrıntılarını bazı uygulamalar için destekleniyorsa sağlama. Etkinleştirmek veya bu işlevi etkinleştirmek veya devre dışı bırakarak devre dışı **eşleme** gösterilen grubu nesnelerinin **sağlama** sekmesi. 
>
>

Grupları sağlama etkinse, uygun bir alan "Eşleşen kimliği" kullanıldığından emin olmak için öznitelik eşlemelerini gözden geçirmeyi unutmayın. Bu görünen ad olabilir ve e-posta diğer adı veya eşleşen özellik boşsa, Grup ve üyelerini sağlanması değil gibi bir grup için Azure AD içinde doldurulamaz.

## <a name="next-steps"></a>Sonraki adımlar
[Kullanıcı sağlama ve Azure Active Directory ile SaaS uygulamalarına sağlamayı otomatikleştirme](active-directory-saas-app-provisioning.md)
