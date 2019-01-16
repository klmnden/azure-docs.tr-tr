---
title: Azure AD'de Self Servis Grup yönetimini ayarlama | Microsoft Docs
description: Azure Active Directory'de (Azure AD) güvenlik grupları veya Office 365 grupları oluşturma ve yönetin ve güvenlik grubu veya Office 365 grup üyelikleri isteme
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: get-started-article
ms.date: 01/14/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.openlocfilehash: 541de125ea16b853a6fc6b3dd5a3e75e3bb9b065
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2019
ms.locfileid: "54319384"
---
# <a name="set-up-azure-active-directory-for-self-service-group-management"></a>Self servis grup yönetimi için Azure Active Directory'yi ayarlama

Kullanıcılarınız Azure Active Directory'de (Azure AD) güvenlik gruplarını veya Office 365 gruplarını oluşturup yönetebilir. Kullanıcılar ayrıca güvenlik grubu veya Office 365 grubu üyelikleri ister ve grubun sahibi üyeliği onaylayabilir ya da reddedebilir. Grup üyeliği günlük denetimi o üyeliğe ilişkin iş bağlamını bilen kişilere atanabilir. Self servis grup yönetimi özellikleri yalnızca güvenlik grupları ve Office 365 grupları için kullanılabilir, ancak posta etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

İki senaryo Self Servis Grup Yönetimi Hizmetleri: 

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

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](../fundamentals/active-directory-manage-groups.md)
* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](groups-settings-cmdlets.md)
* [Azure Active Directory’de Uygulama Yönetimi](../manage-apps/what-is-application-management.md)
* [Azure Active Directory nedir?](../fundamentals/active-directory-whatis.md)
* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](../hybrid/whatis-hybrid-identity.md)
