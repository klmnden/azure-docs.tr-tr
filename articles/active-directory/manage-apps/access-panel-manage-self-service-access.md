---
title: Self Servis uygulama erişiminin nasıl kullanıldığını | Microsoft Docs
description: Kullanıcıların kendi uygulamaları bulmak Self Servis uygulama erişimini etkinleştir
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: japere,asteen
ms.openlocfilehash: 4936c6a0c7323ff5b607519c6d86c2428d7003bb
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/11/2018
ms.locfileid: "44357618"
---
# <a name="how-to-use-self-service-application-access"></a>Self Servis uygulama erişimini kullanma

Kullanıcılarınıza kendi erişim panellerine uygulamalardan Self bulabilmesi için önce etkinleştirmeniz gerekir **Self Servis uygulama erişiminin** Self bulmak ve istek izin vermek istiyorsanız uygulamalara erişim.

Bu özellik, bir BT grubu zamandan ve paradan tasarruf harika bir yoludur ve Azure Active Directory ile modern uygulamalar dağıtımının bir parçası olarak önemle tavsiye edilir.

Bu özelliği kullanarak şunları yapabilirsiniz:

-   Kullanıcıların uygulamaları kendi kendine keşfedin [uygulama erişim panelinde](https://myapps.microsoft.com/) BT grubu rahatsız etme olmadan.

-   Erişim isteğinde bulundu bakın, erişimini kaldırmak ve kendilerine atanan rollerini yönetme önceden yapılandırılmış bir gruba kullanıcılar ekleyin.

-   İsteğe bağlı olarak, BT grubu zorunda değil, uygulama erişim istekleri onaylamak bir iş onaylayan izin verir.

-   İsteğe bağlı olarak bu uygulamaya erişimi onaylama en fazla 10 kişilere yapılandırın.

-   İsteğe bağlı olarak bir iş izin kullanıcılarla parolalar ayarlamak için onaylayan uygulamaya doğrudan iş onaylayanın oturum açmak için kullanabileceğiniz [uygulama erişim panelinde](https://myapps.microsoft.com/).

-   İsteğe bağlı olarak otomatik olarak kullanıcıların doğrudan bir uygulama rolüne atanmış bir Self Servis atayın.

## <a name="enable-self-service-application-access-to-allow-users-to-find-their-own-applications"></a>Kullanıcıların kendi uygulamaları bulmak Self Servis uygulama erişimini etkinleştir

Self Servis uygulama erişiminin kullanıcıların uygulamaları kendi kendine bulmak harika bir yoludur bu uygulamalara erişimi onaylamak için iş grubuna isteğe bağlı olarak sağlar. Parola çoklu oturum uygulamalar'ın üzerinde sağ için kullanıcılara erişim panelleri üzerinden atanan kimlik bilgilerini yönetmek iş grubuna izin verebilirsiniz.

Bir uygulamaya Self Servis uygulama erişimini etkinleştirmek için aşağıdaki adımları izleyin:

1.  Açık [ **Azure portalı** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol taraftaki gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

  * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Self Servis etkinleştirmek istediğiniz uygulamayı seçin listeden erişim.

7.  Uygulama yüklendikten sonra tıklayın **Self Servis** uygulamanın sol taraftaki gezinti menüsünde.

8.  Bu uygulama için Self Servis uygulama erişimini etkinleştirmek için kapatma **kullanıcıların bu uygulamaya erişim istemesine izin?** geç **Evet.**

9.  Ardından, isteyen kullanıcıların bu uygulamaya erişimi eklenmelidir grup seçmek için etiketi yanındaki seçiciyi **atanan kullanıcılar hangi gruba eklenir?** ve bir grubu seçin.

10. **İsteğe bağlı:** önce iş onay gerektiren istiyorsanız kullanıcılara erişim verilir, Ayarla **bu uygulamaya erişim vermeden önce onay gerektirir?** geç **Evet**.

11. **İsteğe bağlı: yalnızca, parola ile çoklu oturum üzerinde kullanan uygulamalar için** onaylanan kullanıcılar için bu uygulama için gönderilen parolaları belirlemek bu iş onaylayanlar izin vermek istediğiniz verilirse **kullanıcının ayarlanacak onaylayanlar izin ver Bu uygulama için parolaları?**  geç **Evet**.

12. **İsteğe bağlı:** bu uygulamaya erişimi onaylama için izin verilen iş onaylayanlar belirtmek için etiket yanındaki seçiciyi **kimin bu uygulamaya erişimi onaylama için verilir?** 10 adede kadar tek tek seçin İş onaylayanlar.

   * Grupları desteklenmez.

13. **İsteğe bağlı:** **rolleri gösterdiğiniz uygulamalar için**, Self Servis onaylanan kullanıcılar role atamak istiyorsanız yanındaki seçiciyi **bu uygulamada kullanıcılar hangi role atanmış?** Bu kullanıcılar atanan rolü seçmek için.

14. Tıklayın **Kaydet** tamamlamak için dikey pencerenin üstünde düğme.

Self Servis uygulama yapılandırmasını tamamladıktan sonra kullanıcılar için gidebilir, [uygulama erişim panelinde](https://myapps.microsoft.com/) tıklatıp **+ Ekle** Self Servis etkin uygulamaları bulmak için düğme erişim. İş onaylayan bir bildirim Ayrıca bkz: kendi [uygulama erişim panelinde](https://myapps.microsoft.com/). Kullanıcı onay gerektiren bir uygulama için erişim istedi gelmeyeceğini e-posta etkinleştirebilirsiniz. 

Bu onay, yani birden çok onaylayıcısı belirtirseniz, herhangi bir tek onaylayan uygulamaya erişimi onaylama tek onay iş akışları yalnızca destekler.

## <a name="next-steps"></a>Sonraki adımlar
[Azure Active Directory Self Servis Grup Yönetimi için ayarlama](../users-groups-roles/groups-self-service-management.md)
