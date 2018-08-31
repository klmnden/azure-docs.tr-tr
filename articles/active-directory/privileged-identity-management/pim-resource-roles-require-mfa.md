---
title: PIM Azure kaynak rolleri için çok faktörlü kimlik doğrulaması isteme | Microsoft Docs
description: Azure AD Privileged Identity Management (PIM) Azure kaynak rolleri için çok faktörlü kimlik doğrulaması (MFA) gerekli öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 04/02/2018
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 171d79856cf67dae9573dd1076c2ae4617cf86d1
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190577"
---
# <a name="require-multi-factor-authentication-for-azure-resource-roles-in-pim"></a>PIM Azure kaynak rolleri için çok faktörlü kimlik doğrulaması gerektir

Azure kaynak rolleri için Privileged Identity Management (PIM) kaynak yöneticileri ve kimlik yöneticileri zamana bağlı üyelik ve tam zamanında erişim kritik Azure altyapısını koruma sağlar. Ayrıca, iki farklı senaryolar için Azure multi-Factor Authentication isteğe bağlı zorlama PIM sağlar.

## <a name="require-multi-factor-authentication-to-activate"></a>Multi-Factor Authentication'ı etkinleştirmek gerekli

Kaynak yöneticileri etkinleştirilebilmesi Azure multi-Factor Authentication'ı çalıştırmak uygun rolü üyelerinin isteyebilir. Bu işlem, etkinleştirme kim makul kesin olarak söyledikleri isteyen kullanıcı sağlar. Kullanıcı hesabının tehlikede, bu seçenek zorlamayı durumlarda kritik kaynaklara korur. 

Bu gereksinim zorlamak için yönetilen kaynakları listesinden bir kaynak seçin. Gelen [genel bakış Panosu'nu](pim-resource-roles-overview-dashboards.md), ekranın sağ alt bölümünde roller listesinden bir rol seçin.

Ayrıca, rol ayarları araçtan alabileceğiniz **rolleri** veya **rol ayarları** sol bölmede sekme.

>[!Note]
>Sol bölmede seçenekler gri renkte ve sayfanın üst kısmında "Etkinleştirilebilmesi için uygun roller sahip" belirten bir başlık görüyorsanız, etkin bir Yöneticisi değilsiniz. Bu, gerektiği anlamına gelir [etkinleştirme](pim-resource-roles-activate-your-roles.md) devam etmeden önce.

!["Rolleri" ve "Rol ayarları" sekmeler ](media/azure-pim-resource-rbac/aadpim_rbac_manage_a_role_v2.png)

Bir rolün üyeliğini görüntülemek için seçin **rol ayarları** çubuğundan açmak için ekranın üst kısmındaki **rol ayarı ayrıntıları**.

Rol ayarlarını değiştirmek için seçin **Düzenle** üstünde düğme.

Bölümünde altında **etkinleştirme**, onay kutusunu işaretleyin **etkinleştirme multi-Factor Authentication iste**. Daha sonra **Kaydet**’e tıklayın.

![Etkinleştirme yapılırken Multi-Factor Authentication'ı gerekli kılın](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa.png)

## <a name="require-multi-factor-authentication-on-assignment"></a>Atamada multi-Factor Authentication iste

Bazı durumlarda, bir kaynak yöneticisi (örneğin bir gün) kısa bir süre için bir role üye atamak isteyebilirsiniz. Bu durumda, etkinleştirme isteği atanan üyeleri gerek yoktur. Üye kendi rol ataması kullandığında zaten atanmış olan şu rolden etkin olduğu Bu senaryoda, multi-Factor Authentication PIM zorunlu kılamaz.

Atama yerine getirmesini Kaynak Yöneticisi kimin söyledikleri olduğundan emin olmak için atamada multi-Factor Authentication zorunlu kılabilir.

Aynı rol ayarı ayrıntıları ekranından, onay kutusunu için **doğrudan atamada multi-Factor Authentication iste**.

![Doğrudan atamada multi-Factor Authentication iste](media/azure-pim-resource-rbac/aadpim_rbac_require_mfa_on_assignment.png)

## <a name="next-steps"></a>Sonraki adımlar

- [PIM'de Azure kaynak rol ayarlarını yapılandırma](pim-resource-roles-configure-role-settings.md)
- [Güvenlik Uyarıları Azure kaynak rolleri için PIM içinde yapılandırma](pim-resource-roles-configure-alerts.md)


