---
title: 'Azure AD Connect: Aygıt Seçenekleri | Microsoft Docs'
description: Bu belge Azure AD Connect aygıt seçenekleri ayrıntıları
services: active-directory
documentationcenter: ''
author: anandy
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
ms.author: anandy
ms.openlocfilehash: 0eb3a33ee030dcda6a811a771578c9e976e36619
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34593291"
---
#<a name="azure-ad-connect-device-options"></a>Azure AD Connect: Aygıt Seçenekleri

Aşağıdaki belgeler Azure AD Connect çeşitli aygıt seçenekleri hakkında bilgi sağlar. Azure AD Connect, aşağıdaki iki işlem yapılandırmak için kullanabilirsiniz: 
* **Karma Azure AD birleştirme**: şirket içi ortamınız varsa, AD alanını ve ayrıca Azure Active Directory tarafından sağlanan özelliklerden avantajı istiyorsanız, karma Azure AD alanına katılmış aygıtlar uygulayabilirsiniz. Her ikisi de, cihazları bunlar şirket için Active Directory ve Azure Active Directory'yi katılmış.
* **Cihaz geri yazma**: cihaz geri yazma, AD FS cihaza bağlı olarak koşullu erişim sağlamak için kullanılır (2012 R2 veya üstü) aygıtları protected

## <a name="configure-device-options-in-azure-ad-connect"></a>Azure AD Connect aygıt seçeneklerini yapılandırma

1.  Azure AD çalışma bağlayın. İçinde **ek görevleri** sayfasında, **aygıt seçenekleri yapılandırmak**.
    ![Aygıt seçeneklerini yapılandırma](./media/active-directory-aadconnect-device-options/deviceoptions.png) 

    Ardından, tıklatıldığında bir **genel bakış** sayfası görüntülenirse, hangi gerçekleştirilebilir işlemleri ayrıntıları.
    ![Genel Bakış](./media/active-directory-aadconnect-device-options/deviceoverview.png)

    >[!NOTE]
    > Yeni yapılandırma aygıt seçenekleri, yalnızca sürüm 1.1.819.0 kullanılabilir ve daha yeni.

2.  Azure AD için kimlik bilgilerini sağladıktan sonra seçebilirsiniz aygıt seçenekleri sayfasında gerçekleştirilecek işlemi.
    ![Aygıt işlemleri](./media/active-directory-aadconnect-device-options/deviceoptionsselection.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Karma Azure AD birleştirme yapılandırın](../device-management-hybrid-azuread-joined-devices-setup.md)
* [Cihaz geri yazma özelliğini devre dışı bırak / yapılandırın](./active-directory-aadconnect-feature-device-writeback.md)

