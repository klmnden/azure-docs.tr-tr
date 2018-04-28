---
title: "Sorun giderme: 'Active Directory' öğesi eksik veya kullanılabilir değil | Microsoft Docs"
description: Active Directory menü öğesi Azure Yönetim Portalı'nda görünmüyor ne yapmanız gerekenler.
services: active-directory
documentationcenter: na
author: bryanla
manager: mtillman
editor: ''
ms.assetid: 3383020d-6397-43ea-b7aa-c6a9d6a1e3df
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: bryanla
ms.openlocfilehash: c6e8557ec66db51df0bb463a9511b0cdc94811d4
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/19/2018
---
# <a name="troubleshooting-active-directory-item-is-missing-or-not-available"></a>Sorun giderme: 'Active Directory' öğesi eksik veya kullanılabilir değil
Azure Active Directory özelliklerini ve hizmetlerini kullanma yönergeleri çoğunu ile başlayan "Azure Yönetim Portalı'na gidin ve tıklayın **Active Directory**." Ancak ne yaparsınız Active Directory uzantısına veya menü öğesini görünmüyorsa veya işaretlenmiş olması durumunda **kullanılamaz**? Bu konuda yardımcı olmak için tasarlanmıştır. Hangi koşullar altında açıklar **Active Directory** görünmez veya kullanılamaz durumda ve devam etmek açıklanmaktadır.

## <a name="active-directory-is-missing"></a>Active Directory eksik
Genellikle, bir **Active Directory** öğesi sol gezinti menüsünde görünür. Azure Active Directory yordamları'ndaki yönergeleri Bu öğe görünümünüzde olduğunu varsayın.

![Ekran görüntüsü: Azure Active Directory'de](./media/active-directory-troubleshooting/typical-view.png)

Aşağıdaki koşullardan herhangi biri true olduğunda sol gezinti menüsünde Active Directory öğesi görünür. Aksi takdirde öğesi görünmez.

* Geçerli kullanıcının (eskiden Windows Live ID da bilinir) bir Microsoft hesabı ile oturum açmış.
  
    OR
* Azure Kiracı bir dizine sahip ve bir dizin Yöneticisi geçerli hesabıdır.
  
    OR
* Azure Kiracı en az bir Azure AD erişim denetimi (ACS) ad alanı vardır. Daha fazla bilgi için bkz: [erişim denetimi Namespace](https://msdn.microsoft.com/library/azure/gg185908.aspx).
  
    OR
* Azure Kiracı en az bir Azure çok faktörlü kimlik doğrulama sağlayıcısı sahiptir. Daha fazla bilgi için bkz: [yönetme Azure çok faktörlü kimlik doğrulama sağlayıcıları](authentication/howto-mfa-getstarted.md).

Erişim denetimi ad alanı veya çok faktörlü kimlik doğrulama sağlayıcısı oluşturmak için tıklatın **+ yeni** > **uygulama hizmetleri** > **Active Directory**.

Bir dizin için yönetici hakları almak için hesabınıza bir yönetici rolü atayın yönetici vardır. Ayrıntılar için bkz [yönetici rolleri atama](active-directory-assign-admin-roles-azure-portal.md).

## <a name="active-directory-is-not-available"></a>Active Directory kullanılabilir değil
Tıkladığınızda **+ yeni** > **uygulama hizmetleri**, bir **Active Directory** öğesi görünür. Dizin, erişim denetimi veya çok faktörlü yetki sağlayıcı gibi Active Directory özelliklerinden herhangi birini geçerli kullanıcıya kullanılabilir olduğunda özellikle, Active Directory öğesi görünür.

Ancak, sayfa yüklenirken öğe soluk ve işaretlenmiş **kullanılamaz**. Bu geçici bir durumdur. Birkaç saniye bekleyin, öğenin kullanılabilir hale gelir. Gecikme uzatılırsa, web sayfası genellikle yenileme sorunu giderir.

![Ekran görüntüsü: Active Directory kullanılabilir değil](./media/active-directory-troubleshooting/not-available.png)

