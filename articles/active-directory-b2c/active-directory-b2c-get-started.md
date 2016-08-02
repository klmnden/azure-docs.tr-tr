<properties
    pageTitle="Azure Active Directory B2C önizlemesi: Azure Active Directory B2C kiracısı oluşturma | Microsoft Azure"
    description="Azure Active Directory B2C kiracısının nasıl oluşturulacağına ilişkin konu başlığı"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/16/2016"
    ms.author="swkrish"/>

# Azure Active Directory B2C önizlemesi: Azure AD B2C kiracısı oluşturma

Microsoft Azure Active Directory (Azure AD) B2C kullanmaya başlamak için bu makalede açıklanan üç adımı izleyin.

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## 1. Adım: Azure aboneliği için kaydolma

Azure aboneliğiniz zaten varsa bu adımı atlayın. Aboneliğiniz yoksa [Azure aboneliği](../active-directory/sign-up-organization.md) için kaydolun ve Azure AD B2C'ye erişim sağlayın.

> [AZURE.NOTE]
Şu anda Azure AD B2C önizlemesini ücretsiz kullanabilirsiniz ancak kullanım, kiracı başına 50.000 kullanıcı ile sınırlıdır. [Klasik Azure portalına](http://manage.windowsazure.com/) erişim sağlamak için Azure aboneliği gereklidir.

## 2. Adım: Azure AD B2C kiracısı oluşturma

Yeni bir Azure AD B2C kiracısı oluşturmak için aşağıdaki adımları kullanın. Şu anda B2C özellikleri, varsa mevcut dizinlerinizde etkinleştirilemez.

1. [Klasik Azure portalında](https://manage.windowsazure.com/) Abonelik Yöneticisi olarak oturum açın. Bu, Azure'a kaydolmak için kullandığınız aynı Microsoft hesabı ya da aynı iş veya okul hesabıdır.
2. **Yeni** > **Uygulama Hizmetleri** > **Active Directory** > **Dizin** > **Özel Oluştur** seçeneklerine tıklayın.

    ![Kiracı oluşturmaya başlama işleminin ekran görüntüsü](./media/active-directory-b2c-get-started/new-directory.png)

3. Kiracınız için **Ad**, **Etki Alanı Adı** ve **Ülke veya Bölge** seçin.
4. **Bu bir B2C dizinidir** ifadesini içeren seçeneği işaretleyin.
5. Eylemi tamamlamak için onay işaretine tıklayın.

    ![B2C dizini oluşturmak için onay işaretinin ekran görüntüsü](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Artık kiracınız oluşturuldu ve Active Directory uzantısında görünecek. Aynı zamanda kiracının Genel Yöneticisi oldunuz. Gerektiğinde başka Genel Yöneticiler ekleyebilirsiniz.

## 3. Adım: Azure portalındaki B2C özellikleri dikey penceresine gitme

1. Sol taraftaki gezinti çubuğunda Active Directory uzantısına gidin.
2. **Dizin** sekmesi kısmında kiracınızı bulun ve kiracınıza tıklayın.
3. **Yapılandır** sekmesine tıklayın.
4. **B2C yönetimi** bölümündeki **B2C ayarlarını yönet** bağlantısına tıklayın.

    ![B2C için dizin yapılandırmasının ekran görüntüsü](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. B2C özelliklerini gösteren dikey pencereye sahip Azure portalı, yeni tarayıcı sekmesinde veya penceresinde açılır.

    > [AZURE.IMPORTANT]
    Azure portalında kiracınızın erişilebilir olması 2-3 dakika kadar sürebilir. Bu adımları bir süre sonra yeniden denemek sorunu düzeltir. Sorun düzeltilmezse Destek ile iletişime geçin.

6. Kolay erişim için bu dikey pencereyi Başlangıç Panonuza sabitleyin. (Sabitleme aracı dikey pencerenin sağ üst köşesinde kırmızı ile işaretlenmiştir.)

    ![B2C özellikleri dikey penceresi ekran görüntüsü](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Kiracınızın kullanıcılarını ve gruplarını, self servis parola sıfırlama yapılandırmasını ve şirket markası ekleme özelliklerini [klasik Azure portalı](https://manage.windowsazure.com/) üzerinde yönetebilirsiniz.

## Sonraki adımlar

[Azure Active Directory B2C önizlemesi: Uygulamanızı kaydetme](active-directory-b2c-app-registration.md) makalesini okuyarak Azure AD B2C ile bir uygulamanın nasıl kaydedileceği ve bir Hızlı Başlangıç uygulamasının nasıl oluşturulacağı hakkında bilgi edinin.



<!---HONumber=Jun16_HO2-->


