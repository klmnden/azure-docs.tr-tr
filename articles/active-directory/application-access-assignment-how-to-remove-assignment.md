---
title: "Bir uygulama için kullanıcının erişimini kaldırma | Microsoft Docs"
description: "Bir uygulama için kullanıcının erişimini kaldırmak öğrenme"
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
ms.openlocfilehash: bb1dc88164aa7971427984b5956e00b1d343cab7
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Bir uygulama için kullanıcının erişimini kaldırma

Bu makalede bir uygulama için kullanıcının erişimini kaldırmak nasıl tasarlandığını anlamanıza yardımcı olur.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Bir uygulama için belirli kullanıcının veya grubun atama kaldırılacağını

Bir kullanıcı veya grup ataması uygulamaya kaldırmak için listelenen adımları izleyin [bir kuruluş uygulama Azure Active Directory'de bir kullanıcı veya grup atamasını kaldırmak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) makalesi.

. ## Her kullanıcı için tüm uygulama erişimi devre dışı bırakmak istiyorum

Tüm kullanıcı oturum açmalarına bir uygulama için devre dışı bırakmak için listelenen adımları [kullanıcı oturum açma işlemlerine Azure Active Directory'de bir kurumsal uygulama için devre dışı bırakma](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) makalesi.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyor

İçin **bir uygulamayı silmek**, aşağıdaki yönergeleri izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklatın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada gösterisini istediğiniz uygulama görmüyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ve **Göster** için seçenek **tüm uygulamaları.**

6.  Silmek istediğiniz uygulamayı seçin.

7.  Uygulamanın yüklediği sonra tıklayın **silmek** üst uygulamanın simgesinden **genel bakış** dikey.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Herhangi bir uygulama için tüm gelecekteki kullanıcı onayı işlemleri devre dışı bırakmak istiyorum

Tüm dizin önlemek için son kullanıcılar için herhangi bir uygulama onaylıyorsunuz kullanıcı izni devre dışı bırakılıyor. Yöneticiler hala kullanıcının behalves üzerinde onay. Uygulama onayı hakkında daha fazla bilgi ve olabilir ya da bunu yapmak istiyor neden okumak için [anlama kullanıcı ve yönetici onayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

İçin **tüm dizininizdeki tüm gelecekteki kullanıcı onayı işlemleri devre dışı**, aşağıdaki yönergeleri izleyin:

1.  Açık [ **Azure Portal** ](https://portal.azure.com/) olarak oturum açın ve bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **daha fazla hizmet** ana sol taraftaki gezinti menüsünde sonundaki.

3.  Yazın **"Azure Active Directory**" Filtre Arama kutusuna seçip **Azure Active Directory** öğesi.

4.  tıklatın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  Tıklatın **kullanıcı ayarlarını**.

6.  Tüm gelecekteki kullanıcı onayı işlemleri ayarlayarak devre dışı **kullanıcılar, uygulamaların verilerine erişmesini izin verebilir** geç **Hayır** tıklatıp **kaydetmek** düğmesi.


# <a name="next-steps"></a>Sonraki adımlar
[Uygulamalara erişimi yönetme](active-directory-managing-access-to-apps.md)
