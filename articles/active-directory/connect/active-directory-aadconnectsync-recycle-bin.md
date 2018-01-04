---
title: "Azure AD Connect eşitleme: AD Geri Dönüşüm Kutusu'nu etkinleştirme | Microsoft Docs"
description: "Bu konuda, Azure AD Connect ile AD geri dönüşüm kutusu özelliğinin kullanımına önerir."
services: active-directory
keywords: "AD geri dönüşüm kutusu, yanlışlıkla silinmesi, kaynak bağlantısı"
documentationcenter: 
author: cychua
manager: mtillman
editor: 
ms.assetid: afec4207-74f7-4cdd-b13a-574af5223a90
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 36a9b5a50a18b73626b0c6e03ed258e387d5c20b
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-sync-enable-ad-recycle-bin"></a>Azure AD Connect eşitleme: AD Geri Dönüşüm Kutusu'nu etkinleştir
Şirket içi Active hangi Azure AD ile eşitlenir dizinlerinizi için AD geri dönüşüm kutusu özelliğini etkinleştirmeniz önerilir. 

Şirket içi yanlışlıkla sildiyseniz AD kullanıcı nesnesi ve bu özelliği kullanarak, Azure AD yükler karşılık gelen Azure AD kullanıcı nesnesini geri yükleme.  AD Geri Dönüşüm Kutusu özelliği hakkında daha fazla bilgi için makalesine başvurun [senaryoya genel bakış geri silinmiş Active Directory nesneleri için](https://technet.microsoft.com/library/dd379542.aspx).

## <a name="benefits-of-enabling-the-ad-recycle-bin"></a>AD Geri Dönüşüm Kutusu'nu etkinleştirme avantajları
Bu özellik, aşağıdakileri yaparak Azure AD kullanıcı nesneleri geri yüklemeye yardımcı olur:

* Şirket içi yanlışlıkla sildiyseniz AD kullanıcı nesnesi, ilgili Azure AD kullanıcı nesnesi sonraki eşitleme döngüsünde silinecek. Varsayılan olarak, Azure AD silinen tutan Azure AD kullanıcı nesnesi 30 gün için geçici olarak silinen durumda.

* Şirket içi varsa AD Geri Dönüşüm Kutusu'nu özelliğinin etkin, silinen geri AD Kullanıcı nesnesinin kaynak bağlantısı değerini değiştirmeden şirket. Zaman kurtarılan şirket içi AD kullanıcı nesnesi Azure AD ile eşitlenir, Azure AD olacak geri yükleme karşılık gelen geçici olarak silinen Azure AD kullanıcı nesnesi. Kaynak bağlantısı özniteliği hakkında daha fazla bilgi için makalesine başvurun [Azure AD Connect: tasarım kavramları](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#sourceanchor).

* Şirket içi yoksa AD Geri Dönüşüm Kutusu özelliği, etkin olabilir Silinmiş nesne değiştirmek için bir AD kullanıcı nesnesi oluşturmak için gerekli. Azure AD Connect eşitleme hizmeti için kaynak bağlantısı öznitelik sistem tarafından oluşturulan AD özniteliği (örneğin, objectGUID) kullanmak için yapılandırılmışsa, yeni oluşturulan AD kullanıcı nesnesi silinen AD kullanıcı nesnesi ile aynı kaynak bağlantısı değere sahip değil. Yeni oluşturulan AD kullanıcı nesnesi için Azure AD eşitlendiğinde, Azure AD yeni bir oluşturur geçici olarak silinen geri yerine Azure AD kullanıcı nesnesi Azure AD kullanıcı nesnesi.

> [!NOTE]
> Varsayılan olarak, Azure AD tutar geçici olarak silinen durumdaki Azure AD kullanıcı nesneler tamamen silinmeden önce 30 gün boyunca silindi. Bununla birlikte, Yöneticiler bu tür nesneleri silme işlemini hızlandırabilir. Nesneleri kalıcı olarak silinir sonra artık kurtarılabilir, bile içi bağlı AD Geri Dönüşüm Kutusu özelliği etkin hale gelir.



## <a name="next-steps"></a>Sonraki adımlar
**Genel bakış konuları**

* [Azure AD Connect eşitleme: anlamak ve eşitleme özelleştirme](active-directory-aadconnectsync-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
