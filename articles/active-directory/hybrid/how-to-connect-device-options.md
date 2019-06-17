---
title: 'Azure AD Connect: Cihaz seçenekleri | Microsoft Docs'
description: Bu belge Azure AD Connect cihaz seçenekleri ayrıntıları
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: billmath
ms.assetid: c0ff679c-7ed5-4d6e-ac6c-b2b6392e7892
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/13/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96ddcdb67ef079cfa23902a1dcb03b0ec61077fe
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67109536"
---
# <a name="azure-ad-connect-device-options"></a>Azure AD Connect: Cihaz seçenekleri

Aşağıdaki belgeler Azure AD Connect çeşitli cihaz seçenekleri hakkında bilgi sağlar. Azure AD Connect, aşağıdaki iki işlemlerini yapılandırmak için kullanabilirsiniz: 
* **Hibrit Azure AD'ye katılma**: Şirket içi ortamınız varsa, AD Ayak izi ve Azure AD'nin avantajlarını istiyorsanız, hibrit Azure AD'ye katılmış cihazlara uygulayabilirsiniz. Bu cihazları hem şirket içi Active Directory'niz ve Azure Active Directory'nize katılmış.
* **Cihaz geri yazmayı**: Cihaz geri yazma, AD FS cihazlara dayalı koşullu erişimi etkinleştirmek için kullanılır (2012 R2 veya üzeri) korunan cihazlar

## <a name="configure-device-options-in-azure-ad-connect"></a>Azure AD Connect cihaz seçeneklerini yapılandır

1.  Çalıştırma Azure AD'ye bağlanın. İçinde **ek görevler** sayfasında **cihaz seçeneklerini yapılandır**.  **İleri**’ye tıklayın.
    ![Cihaz seçeneklerini yapılandır](./media/how-to-connect-device-options/deviceoptions.png) 

    **Genel bakış** sayfası ayrıntılarını görüntüler.
    ![Genel bakış](./media/how-to-connect-device-options/deviceoverview.png)

    >[!NOTE]
    > Yeni yapılandırma cihaz seçenekleri, yalnızca sürüm 1.1.819.0 kullanılabilir ve daha yeni.

2.  Azure AD için kimlik bilgilerini girdikten sonra seçebilirsiniz cihaz seçenekleri sayfasında gerçekleştirilecek işlemi.
    ![Cihaz işlemleri](./media/how-to-connect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Karma Azure AD katılımını Yapılandır](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Yapılandırma / cihaz geri yazmayı devre dışı bırak](how-to-connect-device-writeback.md)

