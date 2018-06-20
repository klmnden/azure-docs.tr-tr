---
title: Var olan Azure AD etki alanı Hizmetleri yönetilen etki alanı için eşleşmeyen dizin hataları çözümleyin | Microsoft Docs
description: Anlama ve mevcut Azure AD etki alanı Hizmetleri yönetilen etki alanı için eşleşmeyen dizin hatalarını çözümleme
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 40eb75b7-827e-4d30-af6c-ca3c2af915c7
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/11/2017
ms.author: maheshu
ms.openlocfilehash: 7e7786ac36485a792e9c77b10925f01790f95ab1
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36218206"
---
# <a name="resolve-mismatched-directory-errors-for-existing-azure-ad-domain-services-managed-domains"></a>Var olan Azure AD etki alanı Hizmetleri yönetilen etki alanı için eşleşmeyen dizin hatalarını çözümleme
Mevcut bir Azure AD etki alanı Hizmetleri yönetilen etki alanına sahip. Azure Portalı'na gidin ve yönetilen etki alanı görüntülediğiniz zaman, aşağıdaki hata iletisini görürsünüz:

![Eşleşmeyen directory hatası](.\media\getting-started\mismatched-tenant-error.png)

Hata çözümlenene kadar bu yönetilen etki alanı yönetemez.


## <a name="whats-causing-this-error"></a>Ne bu hataya neden oluyor mu?
Yönetilen etki alanınızı ve etkin olarak sanal ağı iki farklı ait olduğunda bu hataya Azure AD kiracılarıyla. Örneğin, "contoso.com" adlı bir yönetilen etki alanınız ve Contoso Azure AD kiracısı için etkinleştirildi. Ancak, yönetilen etki alanı etkinleştirildiğini Azure sanal ağı için Fabrikam - farklı bir ait Azure AD kiracısı.

Yeni Azure portalına (ve özellikle Azure AD etki alanı Hizmetleri Uzantısı) Azure Resource Manager üzerinde oluşturulmuştur. Modern Azure Resource Manager ortamında, daha fazla güvenlik sunar ve rol tabanlı erişim (RBAC) kaynaklara denetlemek için bazı kısıtlamalar uygulanır. Yönetilen bir etki alanına eşitlenmesi kimlik bilgisi karmalarını neden olduğundan Azure AD kiracısı Azure AD Etki Alanı Hizmetleri'ni etkinleştirme hassas bir işlemdir. Bu işlem dizin için bir kiracı yöneticisi olmanız gerekir. Ayrıca, yönetilen etki alanını etkinleştirmek sanal ağ üzerinden yönetici ayrıcalıkları olmalıdır. RBAC denetimleri tutarlı bir şekilde çalışması yönetilen etki alanı ve sanal ağ aynı Azure AD kiracısı ait olmalıdır.

Kısacası, başka bir Azure AD kiracısı 'fabrikam.com' tarafından sahip olunan bir Azure aboneliğine ait bir sanal ağdaki "contoso.com" Azure AD kiracısı için yönetilen bir etki alanına etkinleştiremezsiniz. 

**Geçerli yapılandırma**: Bu dağıtım senaryosunda, Contoso yönetilen etki alanı Contoso Azure AD Kiracı için etkinleştirilir. Contoso Azure AD Kiracı tarafından sahip olunan bir Azure aboneliğine ait bir sanal ağdaki yönetilen etki alanı açılır. Bu nedenle, hem yönetilen etki alanı, hem de sanal ağ aynı Azure AD kiracısı ait. Bu, geçerli ve tam olarak desteklenen yapılandırmadır.

![Geçerli bir kiracı yapılandırma](./media/getting-started/valid-tenant-config.png)

**Eşleşmeyen Kiracı Yapılandırması**: Bu dağıtım senaryosunda, Contoso yönetilen etki alanı Contoso Azure AD Kiracı için etkinleştirilir. Ancak, yönetilen etki alanı Fabrikam Azure AD Kiracı tarafından sahip olunan bir Azure aboneliğine ait bir sanal ağdaki açıktır. İki farklı bu nedenle, yönetilen etki alanı ve sanal ağ ait Azure AD kiracılarıyla. Bu yapılandırma eşleşmeyen Kiracı yapılandırması ve desteklenmiyor. Sanal ağ aynı Azure AD kiracısı (yani, Contoso) yönetilen etki alanı olarak taşınması gerekir. Bkz: [çözümleme](#resolution) ayrıntıları bölümü.

![Eşleşmeyen Kiracı yapılandırma](./media/getting-started/mismatched-tenant-config.png)

Bu nedenle, ne zaman yönetilen etki alanı ve etkin olarak sanal ağ ait iki farklı Azure AD kiracılarıyla bu hataya bakın.

Resource Manager ortamında aşağıdaki kurallar geçerli olur:
- Azure AD dizini birden çok Azure aboneliğiniz olabilir.
- Bir Azure aboneliği sanal ağlar gibi birden fazla kaynak olabilir.
- Tek bir Azure AD etki alanı Hizmetleri yönetilen etki alanı için bir Azure AD dizini etkinleştirilir.
- Azure AD etki alanı Hizmetleri yönetilen etki alanı içinde aynı Azure AD kiracısı Azure abonelikleri hiçbirine ait bir sanal ağ üzerinde etkinleştirilebilir.


## <a name="resolution"></a>Çözüm
Eşleşmeyen dizin hatayı gidermek için iki seçeneğiniz vardır. Görebilirsiniz:

- Tıklatın **silmek** varolan silmek için düğmesini yönetilen etki alanı. Kullanarak yeniden oluşturun [Azure portal](https://portal.azure.com), böylece yönetilen etki alanı ve sanal ağ içinde çıktığında Azure AD dizinine ait. Daha önce silinmiş etki alanına yeni oluşturulan yönetilen etki alanına katılan tüm makineler katılın.

- Yönetilen etki alanınızı ait olduğu Azure AD dizini sanal ağa içeren Azure aboneliği taşıyın. Adımları [başka bir hesap için bir Azure aboneliği sahipliğini aktarma](../billing/billing-subscription-transfer.md) makalesi.


## <a name="related-content"></a>İlgili içerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](active-directory-ds-getting-started.md)
* [Sorun giderme kılavuzu - Azure AD etki alanı Hizmetleri](active-directory-ds-troubleshooting.md)
