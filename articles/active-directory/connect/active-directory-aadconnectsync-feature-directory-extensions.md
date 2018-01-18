---
title: "Azure AD Connect eşitleme: dizin uzantıları | Microsoft Docs"
description: "Bu konuda Azure AD Connect dizin uzantıları özelliğinde açıklanmaktadır."
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 31758568f7ce916a4c242aad743bb4b0cb9b2d6e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect eşitleme: dizin uzantıları
Dizin uzantıları şirket içi Active Directory'den kendi özniteliklerle Azure ad'deki şemayı genişletmenizi sağlar. Bu özellik, şirket içi yönetmeye devam öznitelikleri kullanma iş KOLU uygulamaları oluşturmanızı sağlar. Bu öznitelikler aracılığıyla tüketilebilir [Azure AD grafik dizin uzantıları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) veya [Microsoft Graph](https://graph.microsoft.io/). Öznitelikleri kullanılabilir kullanarak görebilirsiniz [Azure AD Graph Explorer'a](https://graphexplorer.azurewebsites.net/) ve [Microsoft Graph Explorer'a](https://developer.microsoft.com/en-us/graph/graph-explorer) sırasıyla.

Şu anda hiçbir Office 365 iş yükü bu öznitelikler tüketir.

Yükleme Sihirbazı'nda özel ayarları yolundaki eşitlemek istediğiniz hangi ek öznitelikleri yapılandırabilirsiniz.
![Şema genişletme Sihirbazı](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  
Yükleme geçerli adaylar aşağıdaki öznitelikleri gösterir:

* Kullanıcı ve grup nesne türleri
* Tek değerli öznitelikler: dize, Boolean, tamsayı, ikili
* Birden çok değerli öznitelikleri: dize, ikili


>[!NOTE]
> Azure AD Connect birden çok değerli AD öznitelikleri birden çok değerli dizin uzantıları olarak Azure ad eşitleme desteklerken da şu anda birden çok değerli dizin uzantıları kullanımını destekleyen hiçbir Azure AD özelliği vardır.

Azure AD Connect yüklemesi sırasında oluşturulan şema önbelleğinden özniteliklerinin listesini okuyun. Ek öznitelikler ile Active Directory şemasını genişlettiyseniz sonra [şema yenileneceğini](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) önce bu yeni öznitelikler görünür.

Azure AD'de bir nesne, en fazla 100 dizin genişletmeleri öznitelikleri olabilir. Uzunluk üst sınırı 250 karakterdir. Bir öznitelik değeri uzunsa eşitleme altyapısı tarafından kesilir.

Azure AD Connect yüklemesi sırasında bir uygulama bu öznitelikler kullanılabildiği kaydedilir. Azure portalında bu uygulamayı görebilirsiniz.  
![Şema uzantısı uygulama](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Bu öznitelikler artık grafik kullanılabilir:  
![Graf](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Öznitelikleri uzantısıyla önekli\_{AppClientId}\_. AppClientId, Azure AD kiracınızdaki tüm öznitelikler için aynı değere sahiptir.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
