---
title: "Self servis uygulamaya erişim yönetimi için Azure Active Directory'yi ayarlama | Microsoft Docs"
description: "Self servis grup yönetimi, kullanıcıların Azure Active Directory'de güvenlik grupları veya Office 365 grupları oluşturup bunları yönetmelerine olanak sağlamanın yanı sıra kullanıcılara güvenlik grubu veya Office 365 grup üyeliği isteme olanağı sunar"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 904d5c70-c34a-46c4-a9a7-d1efecf4821c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/27/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.translationtype: HT
ms.sourcegitcommit: 349fe8129b0f98b3ed43da5114b9d8882989c3b2
ms.openlocfilehash: 92681a42ff1eb7e9bfa834308833b96749cbd078
ms.contentlocale: tr-tr
ms.lasthandoff: 07/26/2017

---
# <a name="setting-up-azure-active-directory-for-self-service-group-management"></a>Self servis grup yönetimi için Azure Active Directory'yi ayarlama
Self servis grup yönetimi kullanıcıların Azure Active Directory’de (Azure AD) güvenlik gruplarını veya Office 365 gruplarını oluşturup yönetmesine imkan tanır. Kullanıcılar ayrıca güvenlik grubu veya Office 365 grubu üyelikleri ister ve grubun sahibi üyeliği onaylayabilir ya da reddedebilir. Bu şekilde grup üyeliği günlük denetimi o üyeliğe ilişkin iş bağlamını bilen kişilere atanabilir. Self servis grup yönetimi özellikleri yalnızca güvenlik grupları ve Office 365 grupları için kullanılabilir, ancak posta etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

> [!IMPORTANT]
> Microsoft, Azure AD’yi bu makalede bahsedilen Klasik Azure Portalı yerine Azure portalındaki [Azure AD yönetim merkezini](https://aad.portal.azure.com) kullanarak yönetmenizi öneriyor.

Self servis grup yönetimi, şu anda iki temel senaryo içerir: temsilcili grup yönetimi ve self servis grup yönetimi.

* **Temsilcili grup yönetimi** Şirketinin kullandığı SaaS uygulamasına erişimi yöneten bir yönetici, bu senaryoya örnek olarak verilebilir. Bu erişim haklarını yönetmek sıkıcı bir hal alabilir; bu durumda yönetici, işletme sahibinden yeni bir grup oluşturmasını ister. Yönetici, uygulamanın erişimini yeni gruba atar ve ardından uygulamaya zaten erişimi olan tüm kişileri gruba ekler. Ardından işletme sahibi daha fazla kullanıcı ekleyebilir ve bu kullanıcılar uygulamaya otomatik olarak sağlanır. İşletme sahibinin, yöneticinin kullanıcılar için erişimi yönetmesini beklemesi gerekmez. Yönetici farklı iş grubundaki bir yöneticiye aynı izin verirse söz konusu kişi kendi kullanıcılar için de erişimi yönetebilir. İşletme sahibi ve yönetici birbirlerinin kullanıcılarını görüntüleyemez veya yönetemez. Yönetici uygulamaya erişimi olan tüm kullanıcıları görebilir ve gerekirse erişim haklarını engelleyebilir.
* **Self servis grup yönetimi** SharePoint Online sitelerine sahip olan ve bu siteleri ayrı şekilde ayarlayan iki kullanıcı, bu senaryoya örnek olarak verilebilir. Bunlar birbirlerinin ekiplerine siteleri için erişim vermek ister. Bunun için Azure AD'de bir grup oluşturabilirler ve her biri SharePoint Online'da kendi sitelerine erişim sunmak üzere bu grubu seçerler. Birisi erişim istediğinde Erişim Paneli'nden ister ve onayın ardından otomatik olarak iki SharePoint Online sitesine de erişim sağlanır. Daha sonra bu kişilerden biri, siteye erişen herkesin belirli bir SaaS uygulamasına da erişmesi gerektiğine karar verir. SaaS uygulamasının yöneticisi uygulamanın erişim haklarını SharePoint Online sitesine ekleyebilir. Daha sonra onaylanan tüm istekler, iki SharePoint Online sitesine ve bu SaaS uygulamasına erişim sağlar.

## <a name="making-a-group-available-for-end-user-self-service"></a>Bir grubu, son kullanıcı self servisi için kullanıma sunma
1. [Klasik Azure portalı](https://manage.windowsazure.com)’nda Azure AD dizininizi açın.
2. **Yapılandır** sekmesinde **Grup yönetimi temsilcisi** seçeneğini Etkin olarak ayarlayın.
3. **Kullanıcılar güvenlik grupları oluşturabilir** veya **Kullanıcılar Office grupları oluşturabilir** ayarını Etkin olarak belirleyin.

**Users can create security groups (Kullanıcılar güvenlik grupları oluşturabilir)** seçeneği etkinleştirildiğinde, dizininizdeki tüm kullanıcılara yeni güvenlik grupları oluşturma ve bu gruplara üye ekleme izni verilir. Bu yeni gruplar ayrıca diğer tüm kullanıcılar Erişim Paneli’nde gösterilir. Grup üzerindeki ilke ayarı izin veriyorsa diğer kullanıcılar bu gruplara katılmak için istekler oluşturabilir. **Kullanıcılar güvenlik grupları oluşturabilir** seçeneği devre dışı ise kullanıcılar grup oluşturamaz ve sahibi oldukları mevcut grupları değiştiremez. Ancak, bu grupların üyeliklerini yönetmeye ve diğer kullanıcıların gruplara katılma isteklerini onaylamaya devam edebilirler.

Kullanıcılarınız için self servis grup yönetimi ile ilgili daha ayrıntılı bir erişim denetimi elde etmek istiyorsanız **Güvenlik grupları için self servis kullanabilen kullanıcılar** seçeneğini de kullanabilirsiniz. **Users can create groups (Kullanıcılar grup oluşturabilir)** seçeneği etkinleştirildiğinde, dizininizdeki tüm kullanıcılara yeni grup oluşturma ve bu gruplara üye ekleme izni verilir. Ayrıca **Users who can use self-service for security groups (Güvenlik grupları için self servis kullanabilen kullanıcılar)** seçeneğini Some (Bazıları) olarak ayarlayarak, grup yönetimini yalnızca belirli bir kullanıcı grubuyla sınırlarsınız. Bu anahtar Bazıları olarak ayarlandığında kullanıcıların yeni gruplar oluşturabilmesi ve gruplara üye ekleyebilmesi için SSGMSecurityGroupsUsers grubuna kullanıcı eklemeniz gerekir. **Users who can use self-service for security groups (Güvenlik grupları için self servis kullanabilen kullanıcılar)** seçeneğini All (Tümü) olarak ayarlayarak, dizininizdeki tüm kullanıcıların yeni grup oluşturabilmesini sağlayabilirsiniz.

Ayrıca **Güvenlik grupları için self servis kullanabilen grup** kutusunu kullanarak, üyeleri self servis kullanabilen bir grup için özel bir ad belirtebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Azure Active Directory nedir?](active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)

