---
title: 'Azure AD Connect eşitleme: dizin uzantıları | Microsoft Docs'
description: Bu konuda Azure AD Connect dizin uzantıları özelliğinde açıklanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 995ee876-4415-4bb0-a258-cca3cbb02193
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/12/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: dda35e63c209951547a667c46639dc0f37c87b43
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34593641"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect eşitleme: dizin uzantıları
Şirket içi Active Directory'den kendi özniteliklerle Azure Active Directory'de (Azure AD) şemayı genişletmek için dizin uzantıları kullanabilirsiniz. Bu özellik, şirket içi yönetmeye devam öznitelikleri kullanma tarafından LOB uygulamaları oluşturmanızı sağlar. Bu öznitelikler aracılığıyla tüketilebilir [Azure AD Graph API dizin uzantıları](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) veya [Microsoft Graph](https://graph.microsoft.io/). Mevcut öznitelikleri kullanarak gördüğünüz [Azure AD Graph Explorer'a](https://graphexplorer.azurewebsites.net/) ve [Microsoft Graph Explorer'a](https://developer.microsoft.com/en-us/graph/graph-explorer)sırasıyla.

Şu anda hiçbir Office 365 iş yükü bu öznitelikler tüketir.

Yükleme Sihirbazı'nda özel ayarları yolundaki eşitlemek istediğiniz hangi ek öznitelikleri yapılandırabilirsiniz.

![Şema genişletme Sihirbazı](./media/active-directory-aadconnectsync-feature-directory-extensions/extension2.png)  

Yükleme geçerli adaylar aşağıdaki öznitelikleri gösterir:

* Kullanıcı ve grup nesne türleri
* Tek değerli öznitelikler: dize, Boolean, tamsayı, ikili
* Birden çok değerli öznitelikleri: dize, ikili


>[!NOTE]
> Azure AD Connect, birden çok değerli dizin uzantıları olarak Azure ad eşitleme birden çok değerli Active Directory öznitelikleri destekler. Ancak şu anda Azure AD içinde hiçbir özellik birden çok değerli dizin uzantıları kullanımını destekler.

Azure AD Connect yüklemesi sırasında oluşturulan şema önbelleğinden özniteliklerinin listesini okuyun. Ek öznitelikler ile Active Directory şemasını genişlettiyseniz gerekir [şemasını Yenile](active-directory-aadconnectsync-installation-wizard.md#refresh-directory-schema) önce bu yeni öznitelikler görünür.

Azure AD'de bir nesne dizin uzantıları için en fazla 100 özniteliklere sahip olabilir. Uzunluk en fazla 250 karakterdir. Bir öznitelik değeri uzunsa, eşitleme altyapısı, tamsayıya dönüştürür.

Azure AD Connect yüklemesi sırasında bir uygulama bu öznitelikler kullanılabildiği kaydedilir. Azure portalında bu uygulamayı görebilirsiniz.

![Şema uzantısı uygulama](./media/active-directory-aadconnectsync-feature-directory-extensions/extension3new.png)

Öznitelikleri uzantısıyla önekli \_{AppClientId}\_. AppClientId, Azure AD kiracınızdaki tüm öznitelikler için aynı değere sahiptir.

Bu öznitelikler Azure AD Graph API aracılığıyla kullanıma sunulmuştur. Bunları kullanarak sorgulama [Azure AD Graph Explorer'a](https://graphexplorer.azurewebsites.net/).

![Azure AD Graph Explorer'a](./media/active-directory-aadconnectsync-feature-directory-extensions/extension4.png)

Ya da Microsoft Graph API'si aracılığıyla öznitelikleri kullanarak sorgulayabilirsiniz [Microsoft Graph Explorer'a](https://developer.microsoft.com/en-us/graph/graph-explorer#).

>[!NOTE]
> Döndürülecek özniteliklerini istemeniz gerekir. Açıkça böyle öznitelikleri seçin: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com?$select extension_9d98ed114c4840d298fad781915f27e4_employeeID, extension_9d98ed114c4840d298fad781915f27e4_division =. 
>
> Daha fazla bilgi için bkz: [Microsoft Graph: sorgu parametrelerini kullanmak](https://developer.microsoft.com/en-us/graph/docs/concepts/query_parameters#select-parameter).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmek [Azure AD Connect eşitleme](active-directory-aadconnectsync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md) hakkında daha fazla bilgi edinin.
