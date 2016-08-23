<properties
    pageTitle="Managing groups in Azure Active Directory | Microsoft Azure"
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
    ms.date="06/14/2016"
    ms.author="curtand"/>


# Azure Active Directory içinde grupları yönetme

Azure Active Directory'nin (Azure AD) kullanıcı yönetimi özelliklerinden biri de kullanıcı grupları oluşturma becerisidir. Aynı anda birkaç kullanıcıya lisans veya izin atama gibi yönetim görevlerini gerçekleştirmek için grup kullanırsınız. Ayrıca, şunlara erişim izni atamak için de grupları kullanabilirsiniz:

- Dizindeki kaynaklar (örneğin, nesneler)
- SaaS uygulamaları, Azure hizmetleri, SharePoint siteleri veya şirket içi kaynaklar gibi dizin dışı olan kaynaklar

Kaynak sahibi başka birisine ait Azure AD grubundaki bir kaynağa da erişim atayabilir. Bu atama ilgili grubun üyelerinin kaynağa erişmelerine izin verir. Ardından grubun sahibi, grup içerisindeki üyeliği yönetir. Etkili bir biçimde, kaynak sahibi tarafından grubun sahibine kullanıcıları kaynaklarına atama izni verilir.

## Nasıl grup oluşturulur?

Kuruluşunuzun abone olduğu hizmetlere bağlı olarak aşağıdakilerden birini kullanarak bir grup oluşturabilirsiniz:
- Klasik Azure portalı
- Office 365 hesap portalı
- Windows Intune hesap portalı

Görevleri Klasik Azure portalında gerçekleştirilen şekilde açıklayacağız. Azure AD dizininizi yönetmek için Azure olmayan portal kullanımı hakkında daha fazla bilgi için bkz. [Azure AD dizinini yönetme](active-directory-administer.md).

1. [Klasik Azure portalında](https://manage.windowsazure.com) **Active Directory**'yi seçin ve ardından kuruluşunuza ait dizinin adını seçin.

2. **Groups (Gruplar)** sekmesini seçin.

3. **Grup Ekle**'yi seçin.

4. **Grup Ekle** penceresinde, adı ve grup açıklamasını belirtin.


## Güvenlik grubuna bireysel kullanıcıları nasıl eklerim veya kaldırırım?

**Gruba bireysel kullanıcı eklemek için**

1. [Klasik Azure portalında](https://manage.windowsazure.com) **Active Directory**'yi seçin ve ardından kuruluşunuza ait dizinin adını seçin.

2. **Groups (Gruplar)** sekmesini seçin.

3. Üye eklemek istediğiniz grubu açın. Henüz gösterilmiyorsa seçili grubun **Üyeler** sekmesini açın.

4. **Add Members (Üye Ekle)** seçeneğini belirleyin.

5. **Üye Ekle** sayfasında bu gruba üye olarak eklemek istediğiniz kullanıcının veya grubun adını seçin. Bu adın **Seçili** bölmesine eklendiğinden emin olun.


**Bireysel kullanıcıyı bir gruptan kaldırmak için**

1. [Klasik Azure portalında](https://manage.windowsazure.com) **Active Directory**'yi seçin ve ardından kuruluşunuza ait dizinin adını seçin.

2. **Groups (Gruplar)** sekmesini seçin.

3. Üyeleri kaldırmak istediğiniz grubu açın.

4. **Members (Üyeler)** sekmesini seçip bu gruptan kaldırmak istediğiniz üyenin adını işaretleyin, ardından **Remove (Kaldır)** düğmesine tıklayın.

6. Bu üyeyi gruptan kaldırmak istediğinizi komut istemcisinde onaylayın.


## Bir grubun üyeliğini dinamik olarak nasıl yönetebilirim?

Hangi kullanıcıların gruba üye olacağını belirlemek için Azure AD'de basit bir kuralı kolayca ayarlayabilirsiniz . Basit bir kural yalnızca tek bir karşılaştırma yapan kuraldır. Örneğin, bir grup bir SaaS uygulamasına atanırsa iş unvanı "Satış Temsilcisi" olan kullanıcıları eklemeye yönelik bir kural oluşturabilirsiniz. Bu kural daha sonra dizininizde o iş unvanına sahip tüm kullanıcılara bu SaaS uygulaması için erişim verir.

> [AZURE.NOTE] Güvenlik gruplarında veya Office 365 gruplarında dinamik üyelik için bir kural ayarlayabilirsiniz. Şu anda uygulamalara grup tabanlı atama yapmak için iç içe geçmiş grup üyelikleri desteklenmemektedir.
>
> Gruplara yönelik dinamik üyelikler, bir Azure AD Premium lisansının atanmasını gerektirir.
>
> - Grupta kuralı yöneten yönetici
> - Grubunun tüm üyeleri

**Bir gruba ilişkin dinamik üyelik etkinleştirmek için**

1. [Klasik Azure portalında](https://manage.windowsazure.com) **Active Directory**'yi seçin ve ardından kuruluşunuza ait dizinin adını seçin.

2. **Groups (Gruplar)** sekmesini seçip düzenlemek istediğiniz grubu açın.

3. **Configure (Yapılandır)** sekmesini seçin ve ardından **Enable Dynamic Memberships (Dinamik Üyelikleri Etkinleştir)** seçeneğini **Yes (Evet)** olarak ayarlayın.

4. Bu grubun işlevleri için dinamik üyeliği denetleyecek gruba ilişkin tek bir basit kural ayarlayın. **Add users where (Kullanıcıları ekleme yeri)** seçeneğinin belirlendiğinden emin olun ve ardından listeden bir kullanıcı özelliği (örneğin, departman, iş unvanı, vb.) seçin.

5. Ardından, bir koşul (Eşit Değildir, Eşittir, İle Başlamaz, İle Başlar, İçermez, İçerir, Eşleşmez, Eşleşir) seçin.

6. Seçilen kullanıcı özelliği için bir karşılaştırma değeri belirtin.

Dinamik grup üyeliğine ilişkin *gelişmiş* kuralların (birden çok karşılaştırma içeren kurallar) nasıl oluşturulacağı hakkında bilgi edinmek için bkz. [Gelişmiş kurallar oluşturmak için öznitelikleri kullanma](active-directory-accessmanagement-groups-with-advanced-rules.md).

## Ek bilgiler

Bu makalelerde Azure Active Directory ile ilgili ek bilgi sağlanmıştır.

* [Azure Active Directory grupları ile kaynaklara erişimi yönetme](active-directory-manage-groups.md)

* [Grup ayarlarını yapılandırmak için Azure Active Directory cmdlet'leri](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Azure Active Directory'de Uygulama Yönetimi için Makale Dizini](active-directory-apps-index.md)

* [Azure Active Directory nedir?](active-directory-whatis.md)

* [Şirket içi kimliklerinizi Azure Active Directory ile tümleştirme](active-directory-aadconnect.md)



<!-----HONumber=Aug16_HO1-->


