---
title: Self Servis Grup Yönetimi ayarlama - Azure Active Directory | Microsoft Docs
description: Azure Active Directory'de (Azure AD) güvenlik grupları veya Office 365 grupları oluşturma ve yönetin ve güvenlik grubu veya Office 365 grup üyelikleri isteme
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro;seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: b860257fd1b3f0897152dc3d48bff0c7e1d3d994
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60469870"
---
# <a name="set-up-self-service-group-management-in-azure-active-directory"></a>Self Servis Grup Yönetimi Azure Active Directory'de ayarlama 

Kullanıcılar kendi güvenlik grupları veya Office 365 grupları Azure Active Directory'de (Azure AD) oluşturmak ve yönetmek etkinleştirebilirsiniz. Grubun sahibi onaylayabilir veya üyelik istekleri reddetme ve için temsilci grup üyeliği denetimi. Self Servis Grup Yönetimi özellikleri, posta etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

## <a name="self-service-group-membership-defaults"></a>Self Servis grup üyeliği Varsayılanları

Ne zaman Azure portalında güvenlik grupları oluşturulur veya Azure AD PowerShell kullanarak, yalnızca grubun sahipleri üyeliği güncelleştirebilirsiniz. Oluşturduğunuz güvenlik gruplarını [erişim paneli](https://account.activedirectory.windowsazure.com/r#/joinGroups) ve tüm Office 365 grupları sahibi tarafından onaylanan ya da otomatik olarak onaylanan tüm kullanıcılar için katılmak kullanılabilir. Erişim Paneli'nde grup oluşturduğunuzda, üyeliği seçenekleri değiştirebilirsiniz.

Oluşturulan gruplar | Güvenlik grubu varsayılan davranışı | Office 365 grubu varsayılan davranışı
------------------ | ------------------------------- | ---------------------------------
[Azure AD PowerShell](groups-settings-cmdlets.md) | Yalnızca sahipleri üyelerini ekleyebilirsiniz.<br>Erişim Paneli'nde katılmak kullanılabilir ancak görünür | Tüm kullanıcılar katılabilir Aç
[Azure portal](https://portal.azure.com) | Yalnızca sahipleri üyelerini ekleyebilirsiniz.<br>Erişim Paneli'nde katılmak kullanılabilir ancak görünür<br>Sahip grubu oluşturma sırasında otomatik olarak atanmamış | Tüm kullanıcılar katılabilir Aç
[Erişim paneli](https://account.activedirectory.windowsazure.com/r#/joinGroups) | Tüm kullanıcılar katılabilir Aç<br>Grup oluşturulduğunda, üyelik seçenekleri değiştirilebilir | Tüm kullanıcılar katılabilir Aç<br>Grup oluşturulduğunda, üyelik seçenekleri değiştirilebilir

## <a name="self-service-group-management-scenarios"></a>Self Servis Grup Yönetimi senaryoları

* **Temsilcili grup yönetimi** Şirketinin kullandığı SaaS uygulamasına erişimi yöneten bir yönetici, bu senaryoya örnek olarak verilebilir. Bu erişim haklarını yönetmek sıkıcı bir hal alabilir; bu durumda yönetici, işletme sahibinden yeni bir grup oluşturmasını ister. Yönetici, uygulamanın erişimini yeni gruba atar ve ardından uygulamaya zaten erişimi olan tüm kişileri gruba ekler. Ardından işletme sahibi daha fazla kullanıcı ekleyebilir ve bu kullanıcılar uygulamaya otomatik olarak sağlanır. İşletme sahibinin, yöneticinin kullanıcılar için erişimi yönetmesini beklemesi gerekmez. Yönetici farklı iş grubundaki bir yöneticiye aynı izin verirse sonra söz konusu kişinin de erişim kendi grup üyeleri için yönetebilirsiniz. İşletme sahibinin, ne de Yöneticisi görüntüleyebilir veya diğer kişilerin grup üyeliğini yönetme. Yönetici uygulamaya erişimi olan tüm kullanıcıları görebilir ve gerekirse erişim haklarını engelleyebilir.
* **Self servis grup yönetimi** SharePoint Online sitelerine sahip olan ve bu siteleri ayrı şekilde ayarlayan iki kullanıcı, bu senaryoya örnek olarak verilebilir. Bunlar birbirlerinin ekiplerine siteleri için erişim vermek ister. Bunun için Azure AD'de bir grup oluşturabilirler ve her biri SharePoint Online'da kendi sitelerine erişim sunmak üzere bu grubu seçerler. Birisi erişim istediğinde Erişim Paneli'nden ister ve onayın ardından otomatik olarak iki SharePoint Online sitesine de erişim sağlanır. Daha sonra bu kişilerden biri, siteye erişen herkesin belirli bir SaaS uygulamasına da erişmesi gerektiğine karar verir. SaaS uygulamasının yöneticisi uygulamanın erişim haklarını SharePoint Online sitesine ekleyebilir. Daha sonra onaylanan tüm istekler, iki SharePoint Online sitesine ve bu SaaS uygulamasına erişim sağlar.

## <a name="make-a-group-available-for-user-self-service"></a>Bir grubu kullanıcı self servisi için kullanıma sunma

1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. **Kullanıcılar ve gruplar**'ı ve ardından **Grup ayarları**'nı seçin.
3. **Self servis grup yönetimi etkin** ayarını **Evet** olarak belirleyin.
4. **Kullanıcılar güvenlik grupları oluşturabilir** veya **Kullanıcılar Office 365 grupları oluşturabilir** ayarını **Evet** olarak belirleyin.
   * Bu ayarlar etkinleştirildiğinde, dizininizdeki tüm kullanıcılara yeni güvenlik grupları oluşturma ve bu gruplara üye ekleme izni verilir. Bu yeni gruplar ayrıca diğer tüm kullanıcılar Erişim Paneli’nde gösterilir. Grup üzerindeki ilke ayarı izin veriyorsa diğer kullanıcılar bu gruplara katılmak için istekler oluşturabilir. 
   * Bu ayarlar devre dışı ise kullanıcılar grup oluşturamaz ve sahibi oldukları mevcut grupları değiştiremez. Ancak, bu grupların üyeliklerini yönetmeye ve diğer kullanıcıların gruplara katılma isteklerini onaylamaya devam edebilirler.

**Kullanıcılar güvenlik grupları oluşturabilir** ve **Kullanıcılar Office 365 grupları oluşturabilir** ayarlarını kullanarak kullanıcılarınız için self servis grup yönetimi ile ilgili daha ayrıntılı bir erişim denetimi elde edebilirsiniz. **Kullanıcılar grup oluşturabilir** seçeneği etkinleştirildiğinde, kiracınızdaki tüm kullanıcılara yeni grup oluşturma ve bu gruplara üye ekleme izni verilir. Kendi gruplarını oluşturabilir kişilere belirtemezsiniz. Yalnızca bir grup sahibi başka bir grup üyesi yapmak için kişiler belirtebilirsiniz.

Ayarlayarak **güvenlik grupları için Self Servis kullanabilen kullanıcılar** ve **kullanıcılar Office 365 grupları oluşturabilir** için **Evet**, tüm kullanıcılar yeni kiracınızda etkinleştir gruplar.

**Kullanıcılar güvenlik grupları oluşturabilir** ve **Kullanıcılar Office 365 grupları oluşturabilir** ayarlarını ayrıca üyeleri self servis hizmetini kullanacak tek bir grubu belirlemek için de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md)
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
