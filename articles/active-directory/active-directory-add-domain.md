<properties
    pageTitle="Özel etki alanı adınızı Azure Active Directory'ye ekleme | Microsoft Azure"
    description="Şirketinizin etki alanı adlarını Azure Active Directory'ye ekleme ve etki alanı adını doğrulama."
    services="active-directory"
    documentationCenter=""
    authors="jeffsta"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="04/20/2016"
    ms.author="curtand;jeffsta"/>

# Özel etki alanı adınızı Azure Active Directory'ye ekleme

Kuruluşunuzun iş amaçlı kullandığı bir veya daha fazla etki alanı adı var ve kullanıcılarınız şirket ağınızda sizin şirket etki alanı adınızı kullanarak oturum açıyor. Artık Azure Active Directory'yi (Azure AD) kullandığınıza göre şirket etki alanı adınızı da Azure AD'ye ekleyebilirsiniz. Bu sayede dizininizde "alice@contoso.com" gibi kullanıcılarınızın aşina olduğu kullanıcı adları atayabilirsiniz. Bu basit bir işlemdir:

- Etki alanı adınızı klasik Azure portalında **Etki Alanı Ekleme** sihirbazına ekleyin

- Azure AD klasik portalında veya Azure AD Connect aracında DNS girişi alın

- Etki alanı adına ilişkin DNS girişini, DNS kayıt şirketi web sitesinin DNS bölge dosyasına ekleyin

- Azure AD klasik portalında veya Azure AD Connect aracındaki etki alanı adını doğrulayın


Siz özel etki alanı adınızı doğrulayana kadar, kullanıcılarınızın "alice@contoso.com" gibi dizininizin ilk etki alanı adını kullanan kullanıcı adlarıyla oturum açması gerekir. "contoso.com" ve "contosobank.com" gibi birden fazla özel etki alanı adı gerekiyorsa 900'e kadar etki alanı adı ekleyebilirsiniz. Her bir etki alanı adını eklemek için bu makaledeki adımları tekrarlayın.

## Dizininize özel etki alanı adı ekleme

1. Azure AD dizininizin genel yöneticisi olan bir kullanıcı hesabıyla [klasik Azure portalında](https://manage.windowsazure.com/) oturum açın.

2. **Active Directory**'de dizininizi açın ve **Etki alanları** sekmesini seçin.

3. Komut çubuğunda **Ekle**'yi seçin ve ardından özel etki alanınızın adını (örneğin, "contoso.com") girin. .com, .net veya diğer üst düzey uzantıları eklemeyi unutmayın.

4. Bu etki alanını şirket içi Active Directory'nizde [birleştirilmiş oturum açma](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Configuring-AD-FS-for-user-sign-in-with-Azure-AD-Connect) özelliği için yapılandırmayı planlıyorsanız onay kutusunu işaretleyin.

5. **Ekle**'yi seçin.

Etki alanı adınızı ekledikten sonra, Azure AD etki alanının kuruluşunuza ait olduğunu doğrulamalıdır. Azure AD'nin bu doğrulamayı gerçekleştirebilmesi için etki alanı adına ilişkin DNS bölge dosyasına bir DNS girişi eklemeniz gerekir. Bu işlem, etki alanı adının ait olduğu etki alanı adı kayıt şirketinin web sitesinden gerçekleştirilir.

## Etki alanı adına ilişkin DNS girişlerini alma

Şirket içi Windows Server Active Directory ile birleştirme işlemi gerçekleştirmiyorsanız DNS girişlerini **Etki alanı ekleme** sihirbazının ikinci sayfasında bulabilirsiniz.

Etki alanını federasyon için yapılandırıyorsanız Azure AD Connect aracını indirmek üzere yönlendirilirsiniz. [Etki alanı adı kayıt şirketinize eklemeniz gereken DNS girişlerini almak](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation) için Azure AD Connect aracını çalıştırın. Azure AD Connect aracı da Azure AD etki alanı adını doğrular.

## DNS girişini DNS bölge dosyasına ekleme

1.  Etki alanına ilişkin etki alanı adı kayıt şirketinde oturum açın. DNS girişini güncelleştirmek için yeterli izinlere sahip değilseniz bu erişime sahip olan kişilerden veya ekipten DNS girişini eklemesini isteyin.

2.  Azure AD tarafından size sağlanan DNS girişini ekleyerek etki alanına ilişkin DNS bölge dosyasını güncelleştirin. Azure AD, bu DNS girişi sayesinde etki alanının size ait olduğunu doğrulayabilir. DNS girişi, posta yönlendirme veya web barındırma gibi davranışları değiştirmez. DNS kayıtlarının yayılması bir saat sürebilir.

[Popüler DNS kayıt şirketlerinde DNS girişi ekleme yönergeleri](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## Azure AD ile etki alanı adını doğrulama

DNS girişini ekledikten sonra etki alanı adının Azure AD tarafından doğrulandığından emin olun. Bu son adımdır.

**Etki alanı ekleme** sihirbazı hâlâ açıksa sihirbazın üçüncü sayfasında **Doğrula**'yı seçin. Doğrulamadan önce DNS girişinin yayılması için bir saate kadar beklemeniz gerekir.

**Etki alanı ekleme** sihirbazı açık değilse etki alanını [klasik Azure portalında](https://manage.windowsazure.com/) doğrulayabilirsiniz:

1.  Azure AD dizininizin genel yöneticisi olan bir kullanıcı hesabıyla oturum açın.

2.  Dizininizi açıp **Etki alanları** sekmesini seçin.

3.  Doğrulamak istediğiniz etki alanını seçin.

4.  Komut çubuğunda **Doğrula**'yı seçin ve ardından iletişim kutusunda **Doğrula** seçeneğini belirleyin.

Başarılı, tebrikler! Artık [özel etki alanı adınızı içeren kullanıcı adları atayabilirsiniz](active-directory-add-domain-add-users.md). Etki alanı adını doğrulama konusunda sorun yaşıyorsanız [Sorun giderme](#troubleshooting) bölümüne göz atın.

## Sorun giderme
Özel etki alanı adını doğrulayamıyorsanız olası birkaç neden vardır. En sık karşılaşılan ile başlayıp en az karşılaşılana kadar devam edeceğiz.

- DNS girişi yaymadan önce etki alanı adını doğrulamayı denediniz. Bir süre bekleyin ve yeniden deneyin.

- DNS kaydı hiç girilmemiş. DNS girişini doğrulayıp yayması için bekleyin ve daha sonra yeniden deneyin.

- Etki alanı adı zaten başka bir dizinde doğrulandı. Etki alanı adını bulup diğer dizinden silin ve yeniden deneyin.

- DNS kaydı hata içeriyor. Hatayı düzeltip yeniden deneyin.

- DNS kayıtlarını güncelleştirmek için yeterli izniniz yok. Kuruluşunuzda erişim izni olan kişi veya ekiple DNS girişlerini paylaşın ve DNS girişini eklemelerini isteyin.


## Sonraki adımlar

-   [Özel etki alanı adınızı içeren kullanıcı adları atama](active-directory-add-domain-add-users.md)
-   [Özel etki alanı adlarını yönetme](active-directory-add-manage-domain-names.md)
-   [Azure AD'deki etki alanı yönetimi kavramları hakkında bilgi edinin](active-directory-add-domain-concepts.md)
-   [Kullanıcılarınız oturum açtığında şirketinizin markasını gösterme](active-directory-add-company-branding.md)
-   [PowerShell'i kullanarak Azure AD'deki etki alanı adlarını yönetme](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)



<!----HONumber=Jun16_HO2-->


