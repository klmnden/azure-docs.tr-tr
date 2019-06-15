---
title: Self Servis uygulama erişimini kullanarak sorunu | Microsoft Docs
description: Self Servis uygulama erişimi ile ilgili sorunları giderme
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
ms.topic: article
ms.date: 07/11/2017
ms.author: mimart
ms.reviewer: japere,asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: a981dfb1d72c21eccf2ad7119ea219114ed15aed
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65784274"
---
# <a name="problem-using-self-service-application-access"></a>Self Servis uygulama erişimini kullanarak sorunu

Self Servis uygulama erişiminin kullanıcıların uygulamaları kendi kendine bulmak harika bir yoludur bu uygulamalara erişimi onaylamak için iş grubuna isteğe bağlı olarak sağlar. Parola çoklu oturum uygulamalar'ın üzerinde sağ için kullanıcılara erişim panelleri üzerinden atanan kimlik bilgilerini yönetmek iş grubuna izin verebilirsiniz.

Kullanıcılarınıza kendi erişim panellerine uygulamalardan Self bulabilmesi için önce etkinleştirmeniz gerekir **Self Servis uygulama erişiminin** Self bulmak ve istek izin vermek istiyorsanız uygulamalara erişim.

## <a name="general-issues-to-check-first"></a>İlk denetlenecek genel sorunlar

-   Self Servis uygulama erişiminin doğru yapılandırıldığından emin olun. "Self Servis uygulama erişimi yapılandırmak" bakın.

-   Kullanıcı veya grup, Self Servis uygulama erişimi istemek için etkinleştirildi emin olun.

-   Kullanıcı Self Servis uygulama erişimi için doğru yere ziyaret eden emin olun. Kullanıcılar gidebilirsiniz kendi [uygulama erişim panelinde](https://myapps.microsoft.com/) tıklatıp **+ Ekle** Self Servis erişimi etkin uygulamaları bulmak için düğme.

-   Self Servis uygulama erişimini yalnızca kısa bir süre önce yapılandırıldıysa, giriş ve çıkış yeniden kullanıcının erişim paneline birkaç dakika sonra Self Servis erişimi değişiklikleri görüntülenmediğini görmek için oturum açmayı deneyin.

## <a name="how-to-configure-self-service-application-access"></a>Self Servis uygulama erişimi yapılandırma

Bir uygulamaya Self Servis uygulama erişimini etkinleştirmek için aşağıdaki adımları izleyin:

1. Açık [ **Azure portalı** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

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
    > Grupları desteklenmez.
    >
    >

13. **İsteğe bağlı:** **Rolleri gösterdiğiniz uygulamalar için**, Self Servis onaylanan kullanıcılar role atamak istiyorsanız yanındaki seçiciyi **bu uygulamada kullanıcılar hangi role atanmış?** rolüne seçmek için Bu kullanıcılara atanmış olmalıdır.

14. Tıklayın **Kaydet** tamamlamak için dikey pencerenin üstünde düğme.

Self Servis uygulama yapılandırmasını tamamladıktan sonra kullanıcılar için gidebilir, [uygulama erişim panelinde](https://myapps.microsoft.com/) tıklatıp **+ Ekle** Self Servis etkin uygulamaları bulmak için düğme erişim. İş onaylayan bir bildirim Ayrıca bkz: kendi [uygulama erişim panelinde](https://myapps.microsoft.com/). Kullanıcı onay gerektiren bir uygulama için erişim istedi gelmeyeceğini e-posta etkinleştirebilirsiniz. 

Bu onay, yani birden çok onaylayıcısı belirtirseniz, herhangi bir tek onaylayan uygulamaya erişimi onaylama tek onay iş akışları yalnızca destekler.

## <a name="if-these-troubleshooting-steps-do-not-resolve-the-issue"></a>Bu sorun giderme adımlarını sorunu çözmezse 

Aşağıdaki bilgilerle varsa bir destek bileti açın:

-   Bağıntı hata kimliği

-   UPN (kullanıcı e-posta adresi)

-   Kiracı kimliği

-   Tarayıcı türü

-   Saat dilimi ve saat/zaman çerçevesi sırasında hata oluşuyor

-   Fiddler izlemeleri

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory Self Servis Grup Yönetimi için ayarlama](../users-groups-roles/groups-self-service-management.md)
