---
title: 'Azure AD Connect eşitleme: dizin genişletmeleri | Microsoft Docs'
description: Bu konu, Azure AD CONNECT'te dizin uzantıları özelliğini açıklar.
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
ms.date: 10/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: b6c5f55a575605eef3a280de4f973d9b2d0a4ace
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/08/2018
ms.locfileid: "51288314"
---
# <a name="azure-ad-connect-sync-directory-extensions"></a>Azure AD Connect eşitleme: dizin genişletmeleri
Dizin genişletmeleri kendi şirket içi Active Directory öznitelikleri ile Azure Active Directory'de (Azure AD) şemayı genişletmek için kullanabilirsiniz. Bu özellik, şirket içi yönetmeye devam öznitelikleri kullanma tarafından LOB uygulamaları oluşturmanıza olanak sağlar. Bu öznitelikler aracılığıyla tüketilebilir [Azure AD Graph API'si dizin genişletmeleri](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions) veya [Microsoft Graph](https://developer.microsoft.com/graph/). Kullanılabilir öznitelikler kullanarak gördüğünüz [Azure AD Graph Gezgini](https://graphexplorer.azurewebsites.net/) ve [Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer)sırasıyla.

Şu anda hiçbir Office 365 iş yükü bu öznitelikleri kullanır.

Size, Yükleme Sihirbazı'nda özel ayarları yolunda eşitlemek istediğiniz hangi ek öznitelikleri yapılandırabilirsiniz.

>[!NOTE]
>Kullanılabilir öznitelikler kutunun büyük/küçük harfe duyarlıdır.

![Şema uzantısı Sihirbazı](./media/how-to-connect-sync-feature-directory-extensions/extension2.png)  

Yükleme, geçerli adaylar olan aşağıdaki öznitelikler gösterir:

* Kullanıcı ve grup nesne türleri
* Tek değerli öznitelikler: String, Boolean, tamsayı, ikili dosya
* Birden çok değerli öznitelikler: dize, ikili dosya


>[!NOTE]
> Azure AD Connect birden çok değerli dizin genişletmeleri olarak Azure AD'ye eşitlenen birden çok değerli Active Directory öznitelikleri destekler. Ancak şu anda Azure AD'de herhangi bir özellik birden çok değerli dizin genişletmeleri kullanımını destekler.

Azure AD Connect yüklemesi sırasında oluşturulan şema önbelleğinden özniteliklerinin listesini okuyun. Ek öznitelikler ile Active Directory şemasını genişlettiyseniz gerekir [şemayı yenilemeniz](how-to-connect-installation-wizard.md#refresh-directory-schema) önce bu yeni öznitelikler görünür.

Dizin genişletmeleri için en fazla 100 öznitelikler Azure AD'de bir nesne olabilir. En fazla 250 karakterdir. Bir öznitelik değeri uzunsa, eşitleme altyapısı, keser.

Azure AD Connect yüklemesi sırasında bu öznitelikler kullanılabilir olduğu bir uygulama kaydedilir. Azure portalında bu uygulamayı görüntüleyebilirsiniz.

![Şema uzantısı uygulama](./media/how-to-connect-sync-feature-directory-extensions/extension3new.png)

Öznitelikleri uzantısıyla ön ekli \_{AppClientId}\_. AppClientId, Azure AD kiracınızda tüm öznitelikler için aynı değere sahiptir.

Bu öznitelikler, artık Azure AD Graph API aracılığıyla kullanılabilir. Bunları kullanarak sorgulama [Azure AD Graph Gezgini](https://graphexplorer.azurewebsites.net/).

![Azure AD Graph Gezgini](./media/how-to-connect-sync-feature-directory-extensions/extension4.png)

Ya da Microsoft Graph API aracılığıyla öznitelikleri kullanarak sorgulayabilirsiniz [Microsoft Graph Gezgini](https://developer.microsoft.com/graph/graph-explorer#).

>[!NOTE]
> Döndürülecek öznitelikleri için sormanız gerekir. Açıkça bu gibi öznitelikleri seçin: https://graph.microsoft.com/beta/users/abbie.spencer@fabrikamonline.com?$select extension_9d98ed114c4840d298fad781915f27e4_employeeID, extension_9d98ed114c4840d298fad781915f27e4_division =. 
>
> Daha fazla bilgi için [Microsoft Graph: Sorgu parametreleri kullanmak](https://developer.microsoft.com/graph/docs/concepts/query_parameters#select-parameter).

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinin [Azure AD Connect eşitleme](how-to-connect-sync-whatis.md) yapılandırma.

[Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](whatis-hybrid-identity.md) hakkında daha fazla bilgi edinin.
