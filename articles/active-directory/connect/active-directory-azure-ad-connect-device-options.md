---
title: 'Azure AD Connect: Cihaz seçenekleri | Microsoft Docs'
description: Bu belge Azure AD Connect cihaz seçenekleri ayrıntıları
services: active-directory
documentationcenter: ''
author: billmath
manager: samueld
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: e52f691c75d491897b06a4ebb492d87fda682e38
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2018
ms.locfileid: "37917855"
---
#<a name="azure-ad-connect-device-options"></a>Azure AD Connect: Cihaz seçenekleri

Aşağıdaki belgeler Azure AD Connect çeşitli cihaz seçenekleri hakkında bilgi sağlar. Azure AD Connect, aşağıdaki iki işlemlerini yapılandırmak için kullanabilirsiniz: 
* **Hibrit Azure AD'ye katılma**: şirket içi ortamınız varsa, AD Ayak izi ve ayrıca Azure Active Directory tarafından sağlanan özellikler avantajından istiyorsanız, hibrit Azure AD'ye katılmış cihazlara uygulayabilirsiniz. Bunlar her ikisi de, cihazlar, şirket içi Active Directory ve Azure Active Directory'nize katılmış.
* **Cihaz geri yazmayı**: cihaz geri yazma, AD FS cihazlara dayalı koşullu erişimi etkinleştirmek için kullanılır (2012 R2 veya üzeri) korunan cihazlar

## <a name="configure-device-options-in-azure-ad-connect"></a>Azure AD Connect cihaz seçeneklerini yapılandır

1.  Çalıştırma Azure AD'ye bağlanın. İçinde **ek görevler** sayfasında **cihaz seçeneklerini yapılandır**.
    ![Cihaz seçeneklerini yapılandır](./media/active-directory-aadconnect-device-options/deviceoptions.png) 

    İleri tıklamak bir **genel bakış** sayfası görüntülenirse, hangi gerçekleştirilebilecek işlemleri ayrıntıları.
    ![Genel Bakış](./media/active-directory-aadconnect-device-options/deviceoverview.png)

    >[!NOTE]
    > Yeni yapılandırma cihaz seçenekleri, yalnızca sürüm 1.1.819.0 kullanılabilir ve daha yeni.

2.  Azure AD için kimlik bilgilerini girdikten sonra seçebilirsiniz cihaz seçenekleri sayfasında gerçekleştirilecek işlemi.
    ![Cihaz işlemleri](./media/active-directory-aadconnect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Karma Azure AD katılımını Yapılandır](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Yapılandırma / cihaz geri yazmayı devre dışı bırak](./active-directory-aadconnect-feature-device-writeback.md)

