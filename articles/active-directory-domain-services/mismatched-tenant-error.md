---
title: Mevcut Azure AD Domain Services yönetilen etki alanı için eşleşmeyen dizin hataları gidermek | Microsoft Docs
description: Mevcut Azure AD Domain Services yönetilen etki alanı için eşleşmeyen dizin hatalarını anlama ve çözme
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 40eb75b7-827e-4d30-af6c-ca3c2af915c7
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/22/2019
ms.author: iainfou
ms.openlocfilehash: 1ab6a535c9ffebcb423e7a5cb7f158224c004bd1
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67472906"
---
# <a name="resolve-mismatched-directory-errors-for-existing-azure-ad-domain-services-managed-domains"></a>Mevcut Azure AD Domain Services yönetilen etki alanı için eşleşmeyen dizin hataları çözün
Mevcut bir Azure AD Domain Services yönetilen etki alanına sahip olduğunuz. Azure portalına gidin ve yönetilen etki alanında görüntülemek, aşağıdaki hata iletisini görürsünüz:

![Eşleşmeyen dizin hatası](./media/getting-started/mismatched-tenant-error.png)

Hata çözümlenene kadar bu yönetilen etki alanını yönetemezsiniz.


## <a name="whats-causing-this-error"></a>Ne bu hataya neden olan?
Yönetilen etki alanınızı ve sanal ağı etkin olarak iki farklı ait olduğunda bu hataya neden Azure AD kiracılarıyla. Örneğin, "contoso.com" adlı bir yönetilen etki alanınız ve Contoso Azure AD kiracınız için etkinleştirildi. Ancak, yönetilen etki alanı etkinleştirildiğini Azure sanal ağı için Fabrikam - farklı bir ait Azure AD kiracısı.

Yeni Azure portalına (ve özel Azure AD Domain Services uzantısı) Azure Resource Manager üzerinde oluşturulmuştur. Modern Azure Resource Manager ortamında, daha yüksek güvenlik sağlamak için rol tabanlı erişim (RBAC) kaynaklarına erişimi denetlemek için bazı kısıtlamalar uygulanır. Yönetilen etki alanı için eşitlenmesi gereken kimlik bilgisi karmalarını neden bu yana Azure AD kiracısı için Azure AD Etki Alanı Hizmetleri'ni etkinleştirme hassas bir işlemdir. Bu işlem, bir dizin için Kiracı yönetici olmanızı gerektirir. Ayrıca, yönetilen etki alanı etkinleştirdiğiniz sanal ağ üzerinden yönetici ayrıcalıkları olmalıdır. RBAC denetimleri tutarlı bir şekilde çalışması yönetilen etki alanı ve sanal ağ aynı Azure AD kiracısına ait olmalıdır.

Kısacası, başka bir Azure AD kiracısı 'fabrikam.com' tarafından sahip olunan bir Azure aboneliğine ait olan bir sanal ağda Azure AD kiracısı için 'contoso.com' yönetilen bir etki alanı etkinleştirilemiyor. 

**Geçerli yapılandırma**: Bu dağıtım senaryosunda, Contoso yönetilen etki alanı Contoso Azure AD kiracısı için etkinleştirilir. Yönetilen etki alanı, Contoso Azure AD kiracısı tarafından sahip olunan bir Azure aboneliğine ait olan bir sanal ağda kullanıma sunulur. Bu nedenle, hem yönetilen etki alanı, hem de sanal ağ aynı Azure AD kiracısına ait. Bu yapılandırma, geçerli ve tam olarak desteklenir.

![Geçerli Kiracı yapılandırma](./media/getting-started/valid-tenant-config.png)

**Eşleşmeyen Kiracı yapılandırmasına**: Bu dağıtım senaryosunda, Contoso yönetilen etki alanı Contoso Azure AD kiracısı için etkinleştirilir. Ancak, yönetilen etki alanı, Fabrikam Azure AD kiracısı tarafından sahip olunan bir Azure aboneliğine ait olan bir sanal ağda kullanıma sunulur. İki farklı bu nedenle, yönetilen etki alanı ve sanal ağa ait Azure AD kiracılarıyla. Bu yapılandırma, eşleşmeyen Kiracı yapılandırması ve desteklenmiyor. Sanal ağ aynı Azure AD kiracısı (Contoso), yönetilen etki alanı taşınmalıdır. Bkz: [çözümleme](#resolution) ayrıntıları bölümü.

![Eşleşmeyen Kiracı yapılandırma](./media/getting-started/mismatched-tenant-config.png)

Bu nedenle, yönetilen etki alanı ve etkin olarak sanal ağ ait iki farklı Azure AD kiracıları bu hataya bakın.

Resource Manager ortamında aşağıdaki kurallar geçerlidir:
- Azure AD dizini, birden çok Azure aboneliğiniz olabilir.
- Bir Azure aboneliği, sanal ağlar gibi birden fazla kaynak olabilir.
- Tek bir Azure AD Domain Services yönetilen etki alanı, bir Azure AD dizini için etkinleştirildi.
- Azure AD Domain Services yönetilen etki alanı aynı Azure AD kiracısı içinde Azure aboneliklerden herhangi birine ait bir sanal ağ üzerindeki etkinleştirilebilir.


## <a name="resolution"></a>Çözüm
Eşleşmeyen dizin hatayı gidermek için iki seçeneğiniz vardır. Şunları yapabilirsiniz:

- Tıklayın **Sil** varolan silmek için düğmeyi yönetilen etki alanı. Kullanarak yeniden oluşturun [Azure portalında](https://portal.azure.com), böylece yönetilen etki alanı ve kullanılabilir olduğu sanal ağ Azure AD dizinine ait. Daha önce yeni oluşturulan yönetilen etki alanında silinen etki alanına katılmış tüm makinelerde katılın.

- Sanal ağa ait olduğu yönetilen etki alanınızı Azure AD dizinini içeren Azure aboneliğinin taşıyın. Bağlantısındaki [bir Azure aboneliğinin sahipliğini başka bir hesaba](../billing/billing-subscription-transfer.md) makalesi.


## <a name="related-content"></a>İlgili içerik
* [Azure AD etki alanı Hizmetleri - başlangıç kılavuzu](create-instance.md)
* [Sorun giderme kılavuzu - Azure AD etki alanı Hizmetleri](troubleshoot.md)
