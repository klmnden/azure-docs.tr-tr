<properties
    pageTitle="Azure AD kiracısı edinme | Microsoft Azure"
    description="Uygulamaları kaydetmek ve oluşturmak üzere Azure Active Directory kiracısı edinme."
    services="active-directory"
    documentationCenter=""
    authors="dstrockis"
    manager="terrylan"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.date="09/28/2015"
    ms.author="dastrock"/>

# Azure Active Directory kiracısı edinme

Azure Active Directory'de (Azure AD) [kiracı](https://msdn.microsoft.com/library/azure/jj573650.aspx#BKMK_WhatIsAnAzureADTenant), bir kuruluşun temsilcisidir.  Bir kuruluşun, Azure, Microsoft Intune veya Office 365 gibi bir Microsoft bulut hizmetine kaydolduğunda aldığı ve sahip olduğu adanmış bir Azure AD hizmeti örneğidir.  Her Azure AD kiracısı benzersizdir ve diğer Azure AD kiracılarından ayrıdır.  

Kiracı, bir şirket içindeki kullanıcıları ve bunlarla ilgili bilgileri (parolalarını, kullanıcı profili verilerini, izinlerini vb.) barındırır.  Ayrıca bir kuruluşa ve kuruluşun güvenliğine ilişkin grupları, uygulamaları ve diğer bilgileri de içerir.

Azure AD kullanıcılarının uygulamanızda oturum açmasına olanak tanımak için uygulamanızı kendi kiracınıza kaydetmeniz gerekir.  Bir Azure AD kiracısında uygulama yayımlamak **tamamen ücretsizdir**.  Hatta çoğu geliştirici, deneme, geliştirme, hazırlama ve test etme amacıyla çeşitli kiracılar ve uygulamalar oluşturur.  Uygulamanıza kaydolan ve uygulamanızı kullanan kuruluşlar, gelişmiş dizin özelliklerinden faydalanmak istemeleri halinde isteğe bağlı olarak lisans satın almayı tercih edebilir.

Peki, Azure AD kiracısını nasıl edinebilirsiniz?  Bu işlem, aşağıdaki koşullarda biraz farklı olabilir:

- [Mevcut bir Office 365 aboneliğiniz varsa](#use-an-existing-office-365-subscription)
- [Bir Microsoft Hesabı ile ilişkili olan mevcut bir Azure aboneliğiniz varsa](#use-an-msa-azure-subscription)
- [Bir kuruluş hesabı ile ilişkili olan mevcut bir Azure aboneliğiniz varsa](#use-an-organizational-azure-subscription)
- [Yukarıdakilerden hiçbirine sahip değilseniz ve sıfırdan başlamak istiyorsanız](#start-from-scratch)

## Mevcut bir Office 365 aboneliğini kullanma
Mevcut bir Office 365 aboneliğiniz varsa ancak Azure aboneliğiniz yoksa (ve [Azure Yönetim Portalı](https://manage.windowsazure.com) üzerinde oturum açamıyorsanız) Azure AD kiracınıza erişim edinmek için lütfen [bu yönergeleri](https://technet.microsoft.com/library/dn832618.aspx) uygulayın.

## MSA Azure aboneliğini kullanma
Daha önce bireysel Microsoft Hesabınız ile bir Azure aboneliğine kaydolduysanız kiracınız zaten mevcuttur!  [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda, "Tüm Öğeler" ve "Active Directory" altında listelenen "Varsayılan Kiracı" adlı kiracıyı bulmanız gerekir.  Bu kiracıyı uygun gördüğünüz şekilde kullanabilirsiniz ancak bir Kuruluş yöneticisi hesabı oluşturmak isteyebilirsiniz.

Bunu yapmak için şu adımları uygulayın.  Alternatif olarak, benzer bir işlemi izleyerek yeni bir kiracı oluşturmak ve kiracı içinde bir yönetici oluşturmak isteyebilirsiniz.

1.  Bireysel hesabınızla [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda oturum açın
2.  Portalın "Active Directory" bölümüne (sol gezinti çubuğunda bulunur) gidin
3.  Kullanılabilir dizinler listesinde "Varsayılan Dizin" girişini seçin
4.  Sayfanın üst kısmındaki Kullanıcılar bağlantısına tıklayın.  Listede, Kaynağı sütununda "Microsoft hesabı" değerine sahip olan bir tek kullanıcı göreceksiniz
5.  Sayfanın altındaki "Kullanıcı Ekle" seçeneğine tıklayın
6.  Kullanıcı Ekleme Formu'nda şu bilgileri sağlayın:
    - Kullanıcı Türü: Kuruluşunuzdaki yeni kullanıcı
    - Kullanıcı adı: (bu yönetici için bir kullanıcı adı seçin)
    - Ad/Soyadı/Görünen Ad: (uygun değerleri seçin)
    - Rol: Genel Yönetici
    - Alternatif E-posta Adresi: (uygun değerleri girin)
    - İsteğe bağlı: Multi-Factor Authentication'ı etkinleştirin
    - Son olarak, kullanıcı oluşturma işlemini sonlandırmak (ve geçici parolayı görüntülemek) için yeşil "OLUŞTUR" düğmesine tıklayın.
7.  Kullanıcı Ekleme Formu'nu doldurduktan ve yeni yönetici kullanıcı için geçici parolayı aldıktan sonra, parolayı değiştirmek üzere bu yeni kullanıcı ile oturum açmanız gerekeceğinden, bu parolayı kaydettiğinizden emin olun. Ayrıca alternatif bir e-posta adresi kullanarak parolayı doğrudan kullanıcıya da gönderebilirsiniz.
8.  Geçici parolayı değiştirmek için https://login.microsoftonline.com adresinde bu yeni kullanıcı hesabı ile oturum açın ve istendiğinde parolayı değiştirin.


## Kuruluş Azure aboneliği kullanma
Daha önce kuruluş hesabınızla bir Azure aboneliğine kaydolduysanız kiracınız zaten mevcuttur!  [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda, "Tüm Öğeler" ve "Active Directory" altında listelenen kiracıyı bulmanız gerekir.  Bu kiracıyı uygun gördüğünüz şekilde kullanabilirsiniz.  Ayrıca, portalın sol alt köşesindeki "Yeni" düğmesini kullanarak yeni bir kiracı oluşturmak isteyebilirsiniz.


## Sıfırdan başlama
Yukarıdakilerin hiçbiri sizin için bir anlam ifade etmiyorsa endişelenmeyin.  Yeni bir kuruluş ile Azure'a kaydolmak üzere [https://account.windowsazure.com/organization](https://account.windowsazure.com/organization) adresini ziyaret etmeniz yeterlidir.  İşlemi tamamladığınızda, kayıt sırasında seçtiğiniz etki alanı adıyla kendi Azure AD kiracınıza sahip olacaksınız.  [Azure Yönetim Portalı](https://manage.windowsazure.com)'nda, sol gezinti çubuğunda bulunan "Active Directory" konumuna giderek kiracınızı bulabilirsiniz.

Azure'a kaydolma işleminin bir parçası olarak, kredi kartı bilgileri sağlamanız gerekecektir.  Güvenle devam edebilirsiniz; Azure AD'de uygulama yayınlama veya yeni kiracı oluşturma işlemleri için sizden ücret tahsil edilmeyecektir.



<!--HONumber=Jun16_HO2-->


