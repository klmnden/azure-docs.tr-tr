---
title: Öznitelikleri eşleme için Azure ad eşitleme | Microsoft Docs
description: Öznitelikleri şirket içi Active directory'nizden Azure AD'ye eşitlemeyi öğrenin. SaaS uygulamalarına kullanıcı hazırlamayı yapılandırırken, varsayılan olarak eşitlenmedi kaynak öznitelikleri eklemek için dizin uzantı özelliğini kullanın.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/13/2019
ms.author: mimart
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9460bc924ea662646360d1a3f5087949a39a03f
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806124"
---
# <a name="sync-an-attribute-from-your-on-premises-active-directory-to-azure-ad-for-provisioning-to-an-application"></a>Şirket içi Active directory'nizden bir öznitelik bir uygulama için sağlama için Azure ad eşitleme

Kullanıcı sağlama için öznitelik eşlemelerini özelleştirme, eşlemek istediğiniz öznitelik görünmez bulabileceğiniz **kaynak özniteliği** listesi. Bu makalede Azure Active Directory (Azure AD), şirket içi Active Directory (AD) eşitleyerek eksik öznitelik ekleme gösterilmektedir.

Azure AD SaaS uygulamasını Azure AD'den kullanıcı hesaplarına sağlanırken bir kullanıcı profili oluşturmak için gerekli tüm verileri içermesi gerekir. Bazı durumlarda, veri kullanımına öznitelikleri, şirket içinden eşitleme AD'yi Azure AD. Azure AD Connect, Azure AD'ye belirli öznitelikleri, ancak tüm öznitelikleri otomatik olarak eşitler. Ayrıca, varsayılan olarak eşitlenir bazı öznitelikler (örneğin, SAMAccountName) Azure AD Graph API'si ile gösterilmeyen. Bu durumlarda, Azure ad özniteliği eşitlemek için Azure AD Connect dizin uzantısı özelliği kullanabilirsiniz. Bu şekilde, öznitelik Azure AD Graph API ve Azure AD sağlama hizmeti için görünür olur.

Sağlama için ihtiyacınız olan verileri Active Directory'de ancak yukarıda açıklanan nedenlerle nedeniyle sağlamak için kullanılabilir değilse, aşağıdaki adımları izleyin.
 
## <a name="sync-an-attribute"></a>Bir öznitelik eşitleme 

1. Azure AD Connect Sihirbazı'nı açın, görevleri seçin ve ardından **eşitleme seçeneklerini özelleştirme**.

   ![Azure Active Directory Connect Sihirbazı ek görevler sayfası](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-customize.png)
 
2. Azure AD genel yönetici olarak oturum açın. 

3. Üzerinde **isteğe bağlı özellikler** sayfasında **dizin genişletme öznitelik eşitlemesi**.
 
   ![Azure Active Directory Connect Sihirbazı isteğe bağlı özellikler sayfası](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extension-attribute-sync.png)

4. Azure AD'ye genişletmek istediğiniz öznitelikleri seçin.
   > [!NOTE]
   > Arama altında **kullanılabilir öznitelikler** büyük/küçük harfe duyarlıdır.

   ![Azure Active Directory Connect Sihirbazı dizin uzantıları seçimi sayfası](media/user-provisioning-sync-attributes-for-mapping/active-directory-connect-directory-extensions.png)

5. Azure AD Connect Sihirbazı tamamlayın ve çalıştırmak bir tam eşitleme döngüsü sağlar. Döngüsü tamamlandıktan sonra şeması genişletilir ve yeni değerleri AD ile Azure AD şirket içi arasında eşitlenir.
 
6. Azure portalında, kullanmadığınız esnada [kullanıcı öznitelik eşlemelerini düzenleme](customize-application-attributes.md), **kaynak özniteliği** listesi şimdi eklendi öznitelik biçimde içeren `<attributename> (extension_<appID>_<attributename>)`. Öznitelik seçin ve hedef uygulama için sağlama eşleyin.

   ![Azure Active Directory Connect Sihirbazı dizin uzantıları seçimi sayfası](media/user-provisioning-sync-attributes-for-mapping/attribute-mapping-extensions.png)

> [!NOTE]
> Başvuru öznitelikleri sağlamasını yapma özelliği, AD gibi şirket **managedby** veya **DN/DistinguishedName**, şu anda desteklenmiyor. Bu özellik talep edebilir [User Voice](https://feedback.azure.com/forums/169401-azure-active-directory). 

## <a name="next-steps"></a>Sonraki adımlar

* [Kimin sağlama kapsamında olduğunu tanımlayın](define-conditional-rules-for-provisioning-user-accounts.md)
