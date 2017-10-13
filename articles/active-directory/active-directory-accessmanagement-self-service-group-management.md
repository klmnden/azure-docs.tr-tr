---
title: "Azure Active Directory'de self servis uygulamaya erişim yönetimini ayarlama | Microsoft Docs"
description: "Azure Active Directory'de (Azure AD) güvenlik grupları veya Office 365 grupları oluşturma ve yönetin ve güvenlik grubu veya Office 365 grup üyelikleri isteme"
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
ms.date: 09/07/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 7a7370eb076ba8602a58a260a14bb863c55bc803
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-azure-active-directory-for-self-service-group-management"></a>Self servis grup yönetimi için Azure Active Directory'yi ayarlama
Kullanıcılarınız Azure Active Directory'de (Azure AD) güvenlik gruplarını veya Office 365 gruplarını oluşturup yönetebilir. Kullanıcılar ayrıca güvenlik grubu veya Office 365 grubu üyelikleri ister ve grubun sahibi üyeliği onaylayabilir ya da reddedebilir. Grup üyeliği günlük denetimi o üyeliğe ilişkin iş bağlamını bilen kişilere atanabilir. Self servis grup yönetimi özellikleri yalnızca güvenlik grupları ve Office 365 grupları için kullanılabilir, ancak posta etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

Self servis grup yönetimi, şu anda iki temel senaryo sunar: temsilcili grup yönetimi ve self servis grup yönetimi.

* **Temsilcili grup yönetimi** Şirketinin kullandığı SaaS uygulamasına erişimi yöneten bir yönetici, bu senaryoya örnek olarak verilebilir. Bu erişim haklarını yönetmek sıkıcı bir hal alabilir; bu durumda yönetici, işletme sahibinden yeni bir grup oluşturmasını ister. Yönetici, uygulamanın erişimini yeni gruba atar ve ardından uygulamaya zaten erişimi olan tüm kişileri gruba ekler. Ardından işletme sahibi daha fazla kullanıcı ekleyebilir ve bu kullanıcılar uygulamaya otomatik olarak sağlanır. İşletme sahibinin, yöneticinin kullanıcılar için erişimi yönetmesini beklemesi gerekmez. Yönetici farklı iş grubundaki bir yöneticiye aynı izin verirse söz konusu kişi kendi kullanıcılar için de erişimi yönetebilir. İşletme sahibi ve yönetici birbirlerinin kullanıcılarını görüntüleyemez veya yönetemez. Yönetici uygulamaya erişimi olan tüm kullanıcıları görebilir ve gerekirse erişim haklarını engelleyebilir.
* **Self servis grup yönetimi** SharePoint Online sitelerine sahip olan ve bu siteleri ayrı şekilde ayarlayan iki kullanıcı, bu senaryoya örnek olarak verilebilir. Bunlar birbirlerinin ekiplerine siteleri için erişim vermek ister. Bunun için Azure AD'de bir grup oluşturabilirler ve her biri SharePoint Online'da kendi sitelerine erişim sunmak üzere bu grubu seçerler. Birisi erişim istediğinde Erişim Paneli'nden ister ve onayın ardından otomatik olarak iki SharePoint Online sitesine de erişim sağlanır. Daha sonra bu kişilerden biri, siteye erişen herkesin belirli bir SaaS uygulamasına da erişmesi gerektiğine karar verir. SaaS uygulamasının yöneticisi uygulamanın erişim haklarını SharePoint Online sitesine ekleyebilir. Daha sonra onaylanan tüm istekler, iki SharePoint Online sitesine ve bu SaaS uygulamasına erişim sağlar.

## <a name="make-a-group-available-for-user-self-service"></a>Bir grubu kullanıcı self servisi için kullanıma sunma
1. Dizin için genel yönetici olan bir hesapla [Azure AD yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
2. **Kullanıcılar ve gruplar**'ı ve ardından **Grup ayarları**'nı seçin.
3. **Self servis grup yönetimi etkin** ayarını **Evet** olarak belirleyin.
4. **Kullanıcılar güvenlik grupları oluşturabilir** veya **Kullanıcılar Office 365 grupları oluşturabilir** ayarını **Evet** olarak belirleyin.
  * Bu ayarlar etkinleştirildiğinde, dizininizdeki tüm kullanıcılara yeni güvenlik grupları oluşturma ve bu gruplara üye ekleme izni verilir. Bu yeni gruplar ayrıca diğer tüm kullanıcılar Erişim Paneli’nde gösterilir. Grup üzerindeki ilke ayarı izin veriyorsa diğer kullanıcılar bu gruplara katılmak için istekler oluşturabilir. 
  * Bu ayarlar devre dışı ise kullanıcılar grup oluşturamaz ve sahibi oldukları mevcut grupları değiştiremez. Ancak, bu grupların üyeliklerini yönetmeye ve diğer kullanıcıların gruplara katılma isteklerini onaylamaya devam edebilirler.

**Kullanıcılar güvenlik grupları oluşturabilir** ve **Kullanıcılar Office 365 grupları oluşturabilir** ayarlarını kullanarak kullanıcılarınız için self servis grup yönetimi ile ilgili daha ayrıntılı bir erişim denetimi elde edebilirsiniz. **Kullanıcılar grup oluşturabilir** seçeneği etkinleştirildiğinde, kiracınızdaki tüm kullanıcılara yeni grup oluşturma ve bu gruplara üye ekleme izni verilir. Ayarı **Bazıları** olarak belirlediğinizde grup yönetimini bir kullanıcı grubuyla kısıtlamış olursunuz. Bu anahtar **Bazıları** olarak ayarlandığında kullanıcıların yeni gruplar oluşturabilmesi ve gruplara üye ekleyebilmesi için SSGMSecurityGroupsUsers grubuna kullanıcı eklemeniz gerekir. **Kullanıcılar güvenlik grupları oluşturabilir** ve **Kullanıcılar Office 365 grupları oluşturabilir** seçeneğini **Tümü** olarak ayarlayarak, kiracınızdaki tüm kullanıcıların yeni grup oluşturabilmesini sağlayabilirsiniz.

**Kullanıcılar güvenlik grupları oluşturabilir** ve **Kullanıcılar Office 365 grupları oluşturabilir** ayarlarını ayrıca üyeleri self servis hizmetini kullanacak tek bir grubu belirlemek için de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)
* [Azure Active Directory nedir?](active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)
