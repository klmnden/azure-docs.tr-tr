---
title: Bir kullanıcının uygulamaya erişimini kaldırmak nasıl | Microsoft Docs
description: Bir kullanıcının uygulamaya erişimini kaldırmak öğrenme
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
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 0deb5215c1379ac552a492f4b9e90df83201aebf
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2018
ms.locfileid: "39364490"
---
# <a name="how-to-remove-a-users-access-to-an-application"></a>Bir uygulama için kullanıcı erişimi kaldırma

Bu makalede, bir kullanıcının uygulamaya erişimini kaldırmak nasıl anlamanıza yardımcı olur.

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Bir uygulama için belirli bir kullanıcının veya grubun atamasını kaldırmak istiyorum

Bir kullanıcı veya uygulamaya Grup atamasını kaldırmak için listelenen adımları izleyin. [kurumsal bir uygulamayı Azure Active Directory'de bir kullanıcı veya grup atamasını kaldırmak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) makalesi.

. ## Her kullanıcı için tüm uygulama erişimi devre dışı bırakmak istiyorum

Bir uygulama için tüm kullanıcı oturum açma işlemleri devre dışı bırakmak için listelenen adımları izleyin. [kullanıcı oturum açma işlemleri için kurumsal bir uygulamayı Azure Active Directory'de devre dışı bırak](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) makalesi.

## <a name="i-want-to-delete-an-application-entirely"></a>Bir uygulamayı tamamen silmek istiyorum

İçin **bir uygulamayı silmek**, bu yönergeleri izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici** veya **ortak yönetici**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  tıklayın **kurumsal uygulamalar** Azure Active Directory sol taraftaki gezinti menüsünde.

5.  tıklayın **tüm uygulamaları** tüm uygulamaların bir listesini görüntülemek için.

   * Burada show istediğiniz uygulamayı göremiyorsanız kullanın **filtre** üst kısmındaki denetim **tüm uygulamalar listesini** ayarlayıp **Göster** seçeneğini **tüm Uygulamalar.**

6.  Silmek istediğiniz uygulamayı seçin.

7.  Uygulama yüklendikten sonra tıklayın **Sil** üst uygulamanın simgesinden **genel bakış** bölmesi.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Herhangi bir uygulama için tüm gelecek kullanıcı onayı işlemleri devre dışı bırakmak istiyorum

Tüm dizininizin engellemek için son kullanıcıların herhangi bir uygulama konusunda çekince kullanıcı onayı devre dışı bırakılıyor. Yöneticiler, yine de kullanıcının behalves üzerinde onay verebilir. Uygulama onay hakkında daha fazla bilgi edinin ve neden olabilir ya da bunu yapmak istiyor okumak için [anlama kullanıcı ve yönetici onayı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

İçin **tüm dizininizdeki tüm gelecek kullanıcı onayı işlemleri devre dışı**, bu yönergeleri izleyin:

1.  Açık [ **Azure portalında** ](https://portal.azure.com/) ve oturum açma bir **genel yönetici.**

2.  Açık **Azure Active Directory uzantısını** tıklayarak **tüm hizmetleri** ana sol gezinti menüsünün üstünde.

3.  Yazın **"Azure Active Directory**" filtre arama kutusunu seçip **Azure Active Directory** öğesi.

4.  Tıklayın **kullanıcılar ve gruplar** Gezinti menüsünde.

5.  Tıklayın **kullanıcı ayarları**.

6.  Ayarlayarak tüm gelecek kullanıcı onayı işlemleri devre dışı **kullanıcılar uygulamaların verilerine erişmesine izin verebilir** geç **Hayır** tıklatıp **Kaydet** düğmesi.


# <a name="next-steps"></a>Sonraki adımlar
[Uygulamalara erişimi yönetme](manage-apps/what-is-access-management.md)
