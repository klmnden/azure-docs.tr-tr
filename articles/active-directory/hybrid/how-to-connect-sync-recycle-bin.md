---
title: 'Azure AD Connect eşitleme: AD geri dönüşüm kutusunu etkinleştirme | Microsoft Docs'
description: Bu konuda, Azure AD Connect ile AD geri dönüşüm kutusu özelliğinin kullanılmasını önerir.
services: active-directory
keywords: AD geri dönüşüm kutusu, yanlışlıkla silinmesi, kaynak bağlantısı
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/17/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 5fe7d3ea7d4f6d648438efc1a484d5909ade2f23
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60382904"
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect eşitleme: AD geri dönüşüm kutusunu etkinleştirme
Şirket içi Active, Azure AD'ye eşitlenen dizinlerinize için AD Geri Dönüşüm Kutusu özelliği etkinleştirmeniz önerilir. 

Şirket içi yanlışlıkla sildiyseniz AD kullanıcı nesnesi ve bu özelliği kullanarak, Azure AD geri yükler karşılık gelen Azure AD kullanıcı nesnesi geri yükleme.  AD Geri Dönüşüm Kutusu özelliği hakkında daha fazla bilgi için makalesine bakın [geri silinmiş Active Directory nesneleri için senaryoya genel bakış](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>AD Geri Dönüşüm Kutusu'nu etkinleştirme avantajları
Bu özellik, Azure AD kullanıcı nesnelerinin aşağıdakileri yaparak geri yükleme konusunda yardımcı olur:

* Şirket içi yanlışlıkla sildiyseniz AD kullanıcı nesnesi, karşılık gelen Azure AD kullanıcı nesnesi, sonraki eşitleme döngüsünde silinecek. Azure AD varsayılan olarak, silinen tutar geçici olarak silinen durumunda 30 gün boyunca Azure AD kullanıcı nesnesi.

* Şirket içi varsa AD Geri Dönüşüm Kutusu özelliği etkin, silinen geri AD kullanıcı nesnesi, kaynak bağlantısı değerini değiştirmeden şirket. Zaman kurtarılan şirket içi AD kullanıcı nesnesini Azure AD'ye eşitlenen, Azure AD olacaktır geri yükleme karşılık gelen geçici olarak silinen Azure AD kullanıcı nesnesi. Kaynak bağlantısı özniteliği hakkında daha fazla bilgi için makalesine bakın [Azure AD Connect: Tasarım kavramları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Şirket içi olmayan AD geri dönüşüm kutusu özelliğinin etkin olabilir silinmiş nesneyi değiştirmek için bir AD kullanıcı nesnesi oluşturmak için gerekli. Azure AD Connect eşitleme hizmeti için kaynak bağlantısı özniteliği sistem tarafından oluşturulan AD özniteliği (örneğin, objectGUID) kullanmak için yapılandırılmışsa, yeni oluşturulan AD kullanıcı nesnesi silinen AD kullanıcı nesnesi aynı değere kaynak bağlantısı yoktur. Yeni oluşturulan AD kullanıcı nesnesini Azure AD'ye eşitlendiğinde, Azure AD yeni bir oluşturur geçici olarak silinen geri yüklemek yerine Azure AD kullanıcı nesnesini Azure AD'ye kullanıcı nesnesi.

> [!NOTE]
> Varsayılan olarak, Azure AD tutar kalıcı olarak silinmeden önce 30 gün boyunca Azure AD kullanıcı nesnelerinin geçici olarak silinen durumunda silindi. Bununla birlikte, Yöneticiler bu tür nesneleri silme işlemini hızlandırabilirsiniz. Kalıcı olarak silinen nesneleri sonra artık kurtarılabilir, şirket içinde AD Geri Dönüşüm Kutusu özelliği etkin olsa bile.

## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitlemesi: Anlama ve eşitleme özelleştirme](how-to-connect-sync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md)
