---
title: "Self Servis uygulama atama yapılandırma | Microsoft Docs"
description: "Kullanıcıların kendi uygulamalarını bulmak izin vermek Self Servis uygulama erişimini etkinleştir"
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
ms.openlocfilehash: 7991dc19d41c5eb8e149c3ee08069e1a162929cc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-configure-self-service-application-assignment"></a>Self Servis uygulama atama yapılandırma

Kullanıcılarınıza kendi erişim paneli uygulamaları kendi kendine bulabilmesi için öncelikle etkinleştirmeniz gerekiyor **Self Servis uygulamaya erişim** otomatik olarak bulmak ve istemek kullanıcılara izin vermek istediğiniz herhangi bir uygulama erişimi.

Bu özellik, zaman ve para bir BT grubu olarak kaydetmek mükemmel bir yoldur ve Azure Active Directory ile modern uygulamalar dağıtımının bir parçası olarak önerilir.

Bu özelliği kullanarak, şunları yapabilirsiniz:

-   Kullanıcıların uygulamalardan Self Bul izin [uygulama erişim Paneli'ne](https://myapps.microsoft.com/) BT grubu rahatsız etme olmadan.

-   Erişim isteğinde bulundu bkz, erişimi kaldırmak ve atanmış rollerini yönetmek için bu kullanıcıların önceden yapılandırılmış bir gruba ekleyin.

-   İsteğe bağlı olarak BT grubu sahip değil için uygulama erişim isteklerini onaylamak bir iş onaylayan izin verir.

-   İsteğe bağlı olarak bu uygulamaya erişim onaylaması en fazla 10 kişiler yapılandırın.

-   İsteğe bağlı olarak bir iş izin kullanıcılarla parolalar ayarlamak için onaylayan uygulamaya sağ iş onaylayanın oturum açmak için kullanabileceğiniz [uygulama erişim Paneli'ne](https://myapps.microsoft.com/).

-   İsteğe bağlı olarak otomatik olarak Self Servis kullanıcıları için uygulama rolü doğrudan atanmış atayın.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Kullanıcıların kendi uygulamalarını bulmak izin vermek Self Servis uygulama erişimini etkinleştir

Self Servis uygulamaya erişim uygulamaları, kendi kendine Bul yapmalarına izin vermek için mükemmel bir yoldur bu uygulamalara erişimi onaylamak için iş grubuna isteğe bağlı olarak sağlar. Bu kullanıcılar için parola çoklu oturum uygulamalar üzerinde sağ bunların erişim paneller atanan kimlik bilgilerini yönetmek iş grubuna izin verebilirsiniz.

Bir uygulama Self Servis uygulama erişimi etkinleştirmek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Self Servis etkinleştirmek istediğiniz uygulamayı seçin listeden erişmek için.

7.  Uygulamanın yüklediği sonra tıklayın **Self Servis** uygulamanın sol taraftaki gezinti menüsünde.

8.  Bu uygulama için Self Servis uygulama erişimini etkinleştirmek için Aç **bu uygulamaya erişmek kullanıcıların?** geç **Evet.**

9.  Ardından, isteyen hangi kullanıcıların bu uygulamaya erişim eklenecek grubu seçmek için etiketi yanındaki seçiciyi **hangi grubuna atanan kullanıcıların eklenmesi?** ve bir grubu seçin.

10. **İsteğe bağlı:** önce iş onayı iste isterseniz, kullanıcılara erişim verilir, Ayarla **bu uygulamaya erişim vermeden önce onay gerektirir?** geç **Evet**.

11. **İsteğe bağlı: yalnızca parola çoklu oturum üzerinde kullanan uygulamalar için** onaylanan kullanıcılar için bu uygulamaya gönderilen parolalar belirtmek bu iş onaylayanlar izin vermek istiyorsanız, Ayarla **bu uygulama için kullanıcının parola ayarlamak onaylayanlar izin?** geç **Evet**.

12. **İsteğe bağlı:** bu uygulamaya erişimi onaylamak için izin verilen iş onaylayanlar belirtmek için etiketi yanındaki seçiciyi **kimin bu uygulamaya erişimi onaylamak için verilir?** en fazla 10 ayrı ayrı iş onaylayanlar seçin.

   >[!NOTE]
   >Grupları desteklenmez.
   >
   >

13. **İsteğe bağlı:** **rolleri kullanıma uygulamalar için**, Self Servis onaylanan kullanıcılar role atamak istiyorsanız Seçici tıklayın **hangi rolü için kullanıcıları bu uygulamada atanmalıdır?** , bu kullanıcılar atanabilir rol seçin.

14. Tıklatın **kaydetmek** tamamlamak için dikey pencerenin üstündeki düğmesi.

Self Servis uygulama yapılandırması tamamlandığında, kullanıcılar için gezinebilir kendi [uygulama erişim Paneli'ne](https://myapps.microsoft.com/) tıklatıp **+ Ekle** Self Servis erişim etkin uygulamalar Bul düğmesi. İş onaylayanlar Ayrıca bkz. bir bildirim kendi [uygulama erişim Paneli'ne](https://myapps.microsoft.com/). Bir kullanıcı kendi onay gerektiren bir uygulamaya erişim istendiğinde bildiren bir e-posta etkinleştirebilirsiniz. 

Bu onaylar, birden çok onaylayanlar belirtirseniz, tek bir onaylayan uygulamaya onaylayan erişim olabilir yani yalnızca tek onay iş akışları destekler.

## <a name="next-steps"></a>Sonraki adımlar
[Self Servis Grup Yönetimi için Azure Active Directory ayarlayan](active-directory-accessmanagement-self-service-group-management.md)
