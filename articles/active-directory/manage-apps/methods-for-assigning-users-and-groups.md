---
title: Kullanıcıları ve grupları bir uygulamaya atama | Microsoft Docs
description: Erişim vermek için uygulamaya kullanıcı atama
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 04/26/2019
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: bdf19d2001f84fab6978bfc34ef14a361b89edea
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65441414"
---
# <a name="assign-users-and-groups-to-an-application-in-azure-active-directory"></a>Kullanıcıları ve grupları Azure Active Directory'de bir uygulamaya atama
Bu makalede bir uygulamayı Azure Active Directory (Azure AD) kullanıcıları veya grupları atama gösterilmektedir. Yöneticinin bunları aşağıdakileri yapmak için erişim izni verebilmeniz için önce kullanıcıların önce uygulamaya atanması gerekir:

-   Bir uygulama tarafından erişim **doğrudan uygulama URL'sine giderek** (diğer adıyla SP tarafından başlatılan oturum açma).

-   Kullanarak bir uygulamaya erişim **kullanıcı erişim URL'SİNDEN** bir uygulamanın üzerinde **özellikleri** sayfa (diğer adıyla IDP tarafından başlatılan oturum açma).

-   Görünür bir uygulama bkz kendi [uygulama erişim panelinde](https://myapps.microsoft.com/) veya mobil uygulama.

-   Görünür bir uygulama bkz kendi [Office 365 uygulama başlatıcısında](https://support.office.com/article/Meet-the-Office-365-app-launcher-79f12104-6fed-442f-96a0-eb089a3f476a).

Grup tabanlı atama kullanılabilirliğini, lisans sözleşmenize göre belirlenir. Grup tabanlı atama, yalnızca güvenlik grupları için desteklenir ve iç içe geçmiş grup üyelikleri şu anda desteklenmiyor.

## <a name="prerequisites"></a>Önkoşullar
Uygulamaya kullanıcıları ve grupları atamadan önce kullanıcı atamasını gerektirir. Kullanıcı ataması gerekli kılmak için:

1. Azure portalı ile bir yönetici hesabıyla oturum açın.
2. Tıklayarak **tüm hizmetleri** ana menüdeki öğe.
3. Uygulama için kullandığınız dizine seçin.
4. Tıklayarak **kurumsal uygulamalar** sekmesi.
5. Bu dizin ile ilişkili uygulamalar listesinden uygulamayı seçin.
6. Tıklayın **özellikleri** sekmesi.
7. Değişiklik **kullanıcı ataması gerekli mi?** Evet Aç/Kapat.
8. Tıklayın **Kaydet** ekranın üstünde düğme.

## <a name="assign-users"></a>Kullanıcıları atama

Bir veya daha fazla kullanıcıları uygulamaya doğrudan atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol taraftaki gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9.  tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam adı** veya **e-posta adresi** içine atama isteyen kullanıcının **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **kullanıcı** göstermek için listedeki bir **onay kutusu**. Kullanıcının profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla kullanıcı eklemek**, başka bir tür **tam adı** veya **e-posta adresi** içine **adına veya e-posta adresine göre arama** Arama kutusuna ve bu kullanıcıyı eklemek için onay kutusunu **seçili** listesi.

13. Kullanıcı seçme işlemini tamamladığınızda, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz kullanıcılara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi Seçilen kullanıcılara uygulamayı atamak için.

Bir kısa süre sonra seçtiğiniz kullanıcıların çözüm Açıklama bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatması mümkün.

## <a name="assign-groups"></a>Grupları atama

Bir veya daha fazla grup, doğrudan uygulamaya atamak için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol taraftaki gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

    * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Listeden bir kullanıcıya atamak istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **kullanıcılar ve gruplar** uygulamanın sol taraftaki gezinti menüsünde.

8.  Tıklayın **Ekle** üstünde düğme **kullanıcılar ve gruplar** listesini açmak için **atama Ekle** bölmesi.

9.  tıklayın **kullanıcılar ve gruplar** seçiciden **atama Ekle** bölmesi.

10. Yazın **tam grup adı** içine atama ilgilenen grubunun **adına veya e-posta adresine göre arama** arama kutusu.

11. Üzerine **grubu** göstermek için listedeki bir **onay kutusu**. Grubun profil fotoğrafı veya kullanıcı için eklenecek logosu yanındaki onay kutusuna tıklayın **seçili** listesi.

12. **İsteğe bağlı:** İsteyip istemediğini **birden fazla Grup Ekle**, başka bir tür **tam grup adı** içine **adına veya e-posta adresine göre arama** arama kutusuna ve bu gruba eklemek için onay kutusuna tıklayın için **seçili** listesi.

13. Grupları seçme işiniz bittiğinde, tıklayın **seçin** uygulamaya atanan kullanıcıların ve grupların listesi eklemek için düğme.

14. **İsteğe bağlı:** tıklayın **rolü Seç** seçicide **atama Ekle** bölmesinde seçtiğiniz gruplara atamak için bir rol seçin.

15. Tıklayın **atama** düğmesi seçili gruplara uygulama atama.

Bir kısa süre sonra seçtiğiniz gruplardaki kullanıcıların çözüm Açıklama bölümünde açıklanan yöntemleri kullanarak bu uygulamaları başlatması mümkün. Dinamik gruplar bunlar, bazı ek işleme gecikme olabilir, kullanıcıların bu atanan gruplar içinde görünen bu atamaları.

## <a name="enable-self-service-application-access"></a>Self Servis uygulama erişimini etkinleştir

Self Servis uygulama erişiminin kullanıcıların uygulamaları kendi kendine bulmak harika bir yoludur bu uygulamalara erişimi onaylamak için iş grubuna isteğe bağlı olarak sağlar. Parola çoklu oturum uygulamalar'ın üzerinde sağ için kullanıcılara erişim panelleri üzerinden atanan kimlik bilgilerini yönetmek iş grubuna izin verebilirsiniz.

Bir uygulamaya Self Servis uygulama erişimini etkinleştirmek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2. Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol taraftaki gezinti menüsünün üstünde.

3. Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4. tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5. tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6. Self Servis etkinleştirmek istediğiniz uygulamayı seçin listeden erişim.

7. Uygulama yüklendikten sonra tıklayın **Self Servis** uygulamanın sol taraftaki gezinti menüsünde.

8. Bu uygulama için Self Servis uygulama erişimini etkinleştirmek için kapatma **kullanıcıların bu uygulamaya erişim istemesine izin?** geç **Evet.**

9. Ardından, isteyen kullanıcıların bu uygulamaya erişimi eklenmelidir grup seçmek için etiketi yanındaki seçiciyi **atanan kullanıcılar hangi gruba eklenir?** ve bir grubu seçin.

10. **İsteğe bağlı:** Önce bir iş onayı iste istiyorsanız kullanıcılara erişim verilir, Ayarla **bu uygulamaya erişim vermeden önce onay gerektirir?** geç **Evet**.

11. **İsteğe bağlı: Yalnızca, parola ile çoklu oturum üzerinde kullanan uygulamalar için** onaylanan kullanıcılar için bu uygulama için gönderilen parolaları belirlemek bu iş onaylayanlar izin vermek istediğiniz verilirse **kullanıcının parolalarını bunu onaylayanların izin ver Uygulama?**  geç **Evet**.

12. **İsteğe bağlı:** Bu uygulamaya erişimi onaylama için izin verilen iş onaylayanlar belirtmek için etiket yanındaki seçiciyi **kimin bu uygulamaya erişimi onaylama için verilir?** 10 adede kadar tek tek iş onaylayanları seçin.

    >[!NOTE]
    >Grupları desteklenmez.
    >
    >

13. **İsteğe bağlı:** **Rolleri gösterdiğiniz uygulamalar için**, Self Servis onaylanan kullanıcılar role atamak istiyorsanız yanındaki seçiciyi **bu uygulamada kullanıcılar hangi role atanmış?** rolüne seçmek için Bu kullanıcılara atanmış olmalıdır.

14. Tıklayın **Kaydet** tamamlamak için bölmenin üstünde düğme.

Self Servis uygulama yapılandırmasını tamamladıktan sonra kullanıcılar için gidebilir, [uygulama erişim panelinde](https://myapps.microsoft.com/) tıklatıp **+ Ekle** Self Servis etkin uygulamaları bulmak için düğme erişim. İş onaylayan bir bildirim Ayrıca bkz: kendi [uygulama erişim panelinde](https://myapps.microsoft.com/). Kullanıcı onay gerektiren bir uygulama için erişim istedi gelmeyeceğini e-posta etkinleştirebilirsiniz. 

Bu onay, birden çok onaylayıcısı belirtirseniz, herhangi bir tek onaylayan uygulamaya erişim onaylayan olabilir yani yalnızca tek bir onay iş akışlarını destekler.

## <a name="next-steps"></a>Sonraki adımlar
[Uygulama Ara sunucusu ile uygulamalarınıza çoklu oturum açma sağlayın](application-proxy-configure-single-sign-on-with-kcd.md)
