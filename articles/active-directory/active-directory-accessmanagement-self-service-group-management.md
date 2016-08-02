<properties
    pageTitle="Self servis uygulamaya erişim yönetimi için Azure Active Directory'yi ayarlama | Microsoft Azure"
    description="Self servis grup yönetimi, kullanıcıların Azure Active Directory'de güvenlik grupları veya Office 365 grupları oluşturup bunları yönetmelerine olanak sağlamanın yanı sıra kullanıcılara güvenlik grubu veya Office 365 grup üyeliği isteme olanağı sunar"
    services="active-directory"
    documentationCenter=""
  authors="curtand"
    manager="stevenpo"
    editor=""
    />

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="curtand"/>

# Self servis grup yönetimi için Azure Active Directory'yi ayarlama

Self servis grup yönetimi, kullanıcıların Azure Active Directory'de (Azure AD'de) güvenlik grupları veya Office 365 grupları oluşturmalarına ve bunları yönetmelerine olanak sağlamanın yanı sıra kullanıcılara güvenlik grubu veya Office 365 grup üyeliği isteme olanağı sunar. Bu istekler daha sonra grup sahibi tarafından onaylanabilir veya reddedilebilir. Self servis grup yönetimi özellikleri kullanılarak grup üyeliği günlük denetimi o üyeliğe ilişkin iş bağlamını bilen kişilere atanabilir. Self servis grup yönetimi özellikleri yalnızca güvenlik grupları ve Office 365 grupları için kullanılabilir, posta etkin güvenlik grupları veya dağıtım listeleri için kullanılamaz.

Self servis grup yönetimi, şu anda iki temel senaryo içerir: temsilcili grup yönetimi ve self servis grup yönetimi.

- **Temsilcili grup yönetimi:** Şirketinin kullandığı SaaS uygulamasına yönelik erişimi yöneten bir yönetici buna örnek olarak verilebilir. Bu erişim haklarını yönetmek sıkıcı bir hal alabilir; bu durumda yönetici, işletme sahibinden yeni bir grup oluşturmasını ister. Ardından yönetici, işletme sahibinin yeni oluşturduğu gruba uygulama erişimi atar ve uygulamaya erişimi olan herkesi bu gruba ekler. Ardından işletme sahibi daha fazla kullanıcı ekleyebilir ve bu kullanıcılar birkaç dakika sonra uygulamaya otomatik olarak sağlanır. İşletme sahibinin, işini yapması için yöneticiyi beklemesi gerekmez; kullanıcıları için erişimi kendi kendine yönetebilir. Yönetici de farklı bir iş grubunun yönetici yardımcısı için aynısını yapabilir; hem işletme sahibi hem de yönetici yardımcısı birbirlerinin kullanıcılarını görmeden kendi kullanıcılarının erişimini yönetebilir. Yönetici uygulamaya erişimi olan tüm kullanıcıları görebilir ve gerekirse erişim haklarını engelleyebilir.

- **Self servis grup yönetimi:** SharePoint Online siteleri olup onları ayrı şekilde ayarlayan ancak birbirilerinin ekiplerine kendi sayfaları için erişim izni vermek isteyen iki kullanıcı buna örnek olarak verilebilir. Bunun için Azure AD'de bir grup oluşturulur ve her ikisi de SharePoint Online'da kendi sitelerine erişim sunmak üzere aynı grubu seçer. Birisi erişim istediğinde Erişim Paneli'nden ister ve onayın ardından otomatik olarak iki SharePoint Online sitesine de erişim sağlanır. Daha sonra bu kişilerden biri, sitesine erişen herkesin belirli bir SaaS uygulamasına da erişmesi gerektiğine karar verir. Ardından bu SaaS uygulamasının yöneticisinden, kendi sitesine bu uygulamaya yönelik erişim hakları vermesini ister. Daha sonra bu kişinin onayladığı tüm istekler, iki SharePoint Online sitesine ve bu SaaS uygulamasına erişim sağlar.

## Bir grubu, son kullanıcı self servisi için kullanıma sunma

[Klasik Azure portalındaki](https://manage.windowsazure.com) **Yapılandır** sekmesinde, **Temsilcili grup yönetimi** seçeneğini Etkin olarak ayarlayın ve ardından **Kullanıcılar güvenlik grupları oluşturabilir** veya **Kullanıcılar Office grupları oluşturabilir** seçeneklerini Etkin olarak belirleyin.

**Kullanıcılar güvenlik grupları oluşturabilir** seçeneği etkinleştirildiğinde, dizininizdeki tüm kullanıcılara yeni güvenlik grupları oluşturma ve bu gruplara üye ekleme izni verilir. Ayrıca bu yeni gruplar, Erişim Paneli'nde diğer tüm kullanıcılar tarafından görüntülenebilir ve gruptaki ilke ayarının izin vermesi halinde diğer kullanıcılar bu gruplara katılmak için istek oluşturabilir. **Kullanıcılar güvenlik grupları oluşturabilir** seçeneği devre dışıysa kullanıcılar grup oluşturamaz ve sahip oldukları mevcut grupları değiştiremez ancak hâlâ bu grupların üyeliklerini yönetebilir ve diğer kullanıcılardan kendi gruplarına katılmak için gelen istekleri onaylayabilir.

Kullanıcılarınız için self servis grup yönetimi işlevleri ile ilgili daha ayrıntılı bir erişim denetimi elde etmek istiyorsanız **Güvenlik grupları için self servis kullanabilen kullanıcılar** seçeneğini de kullanabilirsiniz. **Kullanıcılar grup oluşturabilir** seçeneği etkinleştirildiğinde, dizininizdeki tüm kullanıcılara yeni grup oluşturma ve bu gruplara üye ekleme izni verilir. Ayrıca **Güvenlik grupları için self servis kullanabilen kullanıcılar** seçeneğini Bazıları olarak ayarlayarak, grup yönetimini yalnızca belirli bir kullanıcı grubuyla sınırlarsınız. Bu anahtar Bazıları olarak ayarlandığında, dizininizde SSGMSecurityGroupsUsers adlı bir grup oluşturulur ve yalnızca bu grubun üyesi yaptığınız kullanıcılar yeni güvenlik grupları oluşturup bu gruplara üye ekleyebilir. **Güvenlik grupları için self servis kullanabilen kullanıcılar** seçeneğini Tümü olarak ayarlayarak, dizininizdeki tüm kullanıcıların yeni grup oluşturabilmesini sağlayabilirsiniz.

Ayrıca self servisi kullanabilen ve dizininizde yeni grup oluşturabilen tüm kullanıcıları tutacak bir grup için kendi özel adınızı belirtmek üzere **Güvenlik grupları için self servis kullanabilen grup** kutusunu da kullanabilirsiniz. (Varsayılan olarak ‘SSGMSecurityGroupsUsers’ olarak belirlenmiştir.)

## Ek bilgiler

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)

* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)

* [Azure Active Directory nedir?](active-directory-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)



<!---HONumber=Jun16_HO2-->


