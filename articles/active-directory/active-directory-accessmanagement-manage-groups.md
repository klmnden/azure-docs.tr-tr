<properties
    pageTitle="Azure Active Directory içinde grupları yönetme | Microsoft Azure"
    description="How to create and manage groups to manage Azure users using Azure Active Directory."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/26/2016"
    ms.author="curtand"/>


# Azure Active Directory içinde grupları yönetme

Azure Active Directory'nin (Azure AD) kullanıcı yönetimi özelliklerinden biri de kullanıcı grupları oluşturma becerisidir. Ardından, kullanıcı sınıfına lisansları atamak için bir grup kullanabilirsiniz. Ayrıca, şunlara erişim izni atamak için de grupları kullanabilirsiniz:

- Dizindeki kaynaklar (örneğin, nesneler)
- SaaS uygulamaları, Azure hizmetleri, SharePoint siteleri veya şirket içi kaynaklar gibi dizin dışı olan kaynaklar.

Kaynak sahibi Azure AD grubundaki bir kaynağa da erişim atayabilir. Bu işlem, ilgili grubun üyelerinin kaynağa erişmelerine izin verir. Ardından grubun sahibi, grup içerisindeki üyeliği yönetir. Etkili bir biçimde, kaynak sahibi tarafından grubun sahibine kullanıcıları kaynaklarına atama izni verilir.

## Nasıl grup oluşturulur?

Bu görev, kuruluşunuzun abone olduğu hizmetlere bağlı olarak Office 365 hesap portalı, Windows Intune hesap portalı veya klasik Azure portalı kullanarak tamamlanabilir. Azure Active Directory'nizi yönetmek için Azure olmayan portal kullanımı hakkında daha fazla bilgi için bkz. [Azure AD dizinini yönetme](active-directory-administer.md).

1. [Klasik Azure portalında](https://manage.windowsazure.com) **Active Directory**'yi seçin ve ardından kuruluşunuzun dizin adını seçin.

2. **Gruplar** sekmesini seçin.

3. **Grup Ekle**'yi seçin.

4. **Grup Ekleme** penceresinde, adı ve grup açıklamasını belirtin.


## Güvenlik grubuna bireysel kullanıcıları nasıl eklerim veya kaldırırım?

**Gruba bireysel kullanıcı eklemek için**

1. [Klasik Azure portalında](https://manage.windowsazure.com) **Active Directory**'yi seçin ve ardından kuruluşunuzun dizin adını seçin.

2. **Gruplar** sekmesini seçin.

3. Üye eklemek istediğiniz grubu açın. Varsayılan olarak, seçilen grubun **Üyeler** sekmesi görüntülenir.

4. **Üye Ekleme**'yi seçin.

5. **Üye Ekleme** sayfasında, bu gruba üye olarak eklemek istediğiniz kullanıcının veya grubun adını seçip bu adın **Seçilen** bölmesine eklendiğinden emin olun.


**Bireysel kullanıcıyı bir gruptan kaldırmak için**

1. [Klasik Azure portalında](https://manage.windowsazure.com) **Active Directory**'yi seçin ve ardından kuruluşunuzun dizin adını seçin.

2. **Gruplar** sekmesini seçin.

3. Üyeleri kaldırmak istediğiniz grubu açın.

4. **Üyeler** sekmesini seçip bu gruptan kaldırmak istediğiniz üyenin adını işaretleyin, ardından **Kaldır**'a tıklayın.

6. Bu üyeyi gruptan kaldırmak istediğinizi komut istemcisinde onaylayın.


## Bir grubun üyeliğini dinamik olarak nasıl yönetebilirim?

Hangi kullanıcıların gruba üye olacağını belirlemek için Azure AD'de basit bir kuralı (yalnızca tek karşılaştırma yapan bir kuralı) kolayca ayarlayabilirsiniz . Örneğin, bir grup SaaS uygulamasına atandıysa ve "Satış Temsilcisi," iş unvanına sahip kullanıcıları eklemek için bir kural ayarlarsanız Azure AD dizininizdeki bu iş unvanına sahip tüm kullanıcıların SaaS uygulamasına erişimi olacaktır.

> [AZURE.NOTE] Güvenlik gruplarında veya Office 365 gruplarında dinamik üyelik için bir kural ayarlayabilirsiniz. Şu anda uygulamalara grup tabanlı atama yapmak için iç içe geçmiş grup üyelikleri desteklenmiyor.
>
> Gruplara yönelik dinamik üyelikler, bir Azure AD Premium lisansının atanmasını gerektirir.
>
> - Grupta kuralı yöneten yönetici
> - Grubun üyesi olmak için kural tarafından seçilen tüm kullanıcılar

**Bir gruba ilişkin dinamik üyelik etkinleştirmek için**

1. [Klasik Azure portalında](https://manage.windowsazure.com) **Active Directory**'yi seçin ve ardından kuruluşunuzun dizin adını seçin.

2. **Gruplar** sekmesini seçip düzenlemek istediğiniz grubu açın.

3. **Yapılandır** sekmesini seçin ve ardından **Dinamik Üyelikleri Etkinleştir** seçeneğini **Evet** olarak ayarlayın.

4. Bu grubun işlevleri için dinamik üyeliği denetleyecek gruba ilişkin tek bir basit kural ayarlayın. **Kullanıcıları ekleme yeri** seçeneğinin seçildiğinden emin olun ve ardından listeden bir kullanıcı özelliği seçin (örneğin, departman, iş unvanı, vb.).

5. Ardından, bir koşul (Eşit Değil, Eşittir, İle Başlamaz, İle Başlar, İçermez, İçerir ,Eşleşmez, Eşleşir) seçin ve son olarak seçilen kullanıcı özelliği için bir değer belirtin.

Dinamik grup üyeliğine ilişkin *gelişmiş* kuralların (birden çok karşılaştırma içeren kurallar) nasıl oluşturulacağı hakkında bilgi edinmek için bkz. [Gelişmiş kurallar oluşturmak için öznitelikleri kullanma](active-directory-accessmanagement-groups-with-advanced-rules.md).

## Ek bilgiler

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)

* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)

* [Azure Active Directory nedir?](active-directory-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)



<!--HONumber=Jun16_HO2-->


