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
ms.openlocfilehash: 9abd035b13a0d51c534eb8cac50c045012399a69
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/19/2018
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

Öznitelikleri uzantısıyla önekli\_{AppClientId}\_. AppClientId, Azure AD kiracınızdaki tüm öznitelikler için aynı değere sahiptir.

Bu öznitelikler şimdi aracılığıyla kullanılabilir **Azure AD grafik**:

Azure AD Graph Explorer'a aracılığıyla Azure AD grafik sorgulama yapabilirsiniz: [https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/)

![Graf](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Aracılığıyla veya **Microsoft Graph API**:

Microsoft Graph API aracılığıyla Microsoft Graph Explorer'a sorgulama yapabilirsiniz: [https://developer.microsoft.com/en-us/graph/graph-explorer#](https://developer.microsoft.com/en-us/graph/graph-explorer#)

>[!NOTE]
> Döndürülecek özniteliği için açıkça istemeniz gerekir. Bu öznitelikler şöyle açıkça seçerek yapılabilir: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com? $select gözden geçirin daha fazla bilgi için extension_9d98ed114c4840d298fad781915f27e4_employeeID,extension_9d98ed114c4840d298fad781915f27e4_division=[Microsoft Graph: sorgu parametrelerini kullanın](https://developer.microsoft.com/en-us/graph/docs/concepts/query_parameters#select-parameter)

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
