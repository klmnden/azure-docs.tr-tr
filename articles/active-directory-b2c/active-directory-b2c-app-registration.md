<properties
    pageTitle="Azure Active Directory B2C önizlemesi: Uygulama kaydı | Microsoft Azure"
    description="Uygulamanızı Azure Active Directory B2C'ye kaydetme"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/16/2016"
    ms.author="swkrish"/>


# Azure Active Directory B2C önizlemesi: Uygulamanızı kaydetme

[AZURE.INCLUDE [active-directory-b2c-preview-note](../../includes/active-directory-b2c-preview-note.md)]

## Önkoşul

Tüketicinin kaydolmasını ve oturum açmasını kabul eden bir uygulama oluşturmak için öncelikle uygulamayı Azure Active Directory B2C kiracısına kaydetmeniz gerekir. [Azure AD B2C kiracısı oluşturma](active-directory-b2c-get-started.md) makalesinde ana hatlarıyla belirtilen adımları izleyerek kendi kiracınızı edinin. Söz konusu makaledeki tüm adımları izledikten sonra B2C özellikleri dikey penceresi Başlangıç panonuza sabitlenir.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## B2C özellikleri dikey penceresine gitme

B2C özellikleri dikey penceresine Azure portalından veya klasik Azure portalından gidebilirsiniz.

### 1. Azure portalına erişme

B2C özellikleri dikey penceresini Başlangıç panonuza sabitlediyseniz dikey pencereyi, B2C kiracısının Genel Yöneticisi olarak [Azure portalında](https://portal.azure.com/) oturum açar açmaz görürsünüz.

Ayrıca dikey pencereye, [Azure portalındaki](https://portal.azure.com/) sol gezinti bölmesinde bulunan **Gözat** ve ardından **Azure AD B2C** seçeneklerine tıklayarak da erişebilirsiniz.

Dikey pencereye erişmek için [https://portal.azure.com/{tenant}.onmicrosoft.com/?#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/](https://portal.azure.com/{tenant}.onmicrosoft.com/?#blade/Microsoft_AAD_B2CAdmin/TenantManagementBlade/id/) adresine de gidebilirsiniz. Bu adresteki **{tenant}** ifadesi, kiracı oluşturma zamanında kullanılan ad ile değiştirilmelidir (örneğin, contosob2c). Gelecekte kullanım için bu bağlantıya yer işareti ekleyebilirsiniz.

    > [AZURE.IMPORTANT]
    B2C özellikleri dikey penceresine erişebilmek için B2C kiracısının Genel Yöneticisi olmanız gerekir. Başka bir kiracının Genel Yöneticisi veya başka bir kiracıdan gelen bir kullanıcı, dikey pencereye erişemez.

### 2. Klasik Azure portalına erişme

[Klasik Azure portalında](https://manage.windowsazure.com/) Abonelik Yöneticisi olarak oturum açın. (Bu, Azure'a kaydolmak için kullandığınız Microsoft hesabı ya da aynı iş veya okul hesabıdır.) Soldaki Active Directory uzantısına gidin ve B2C kiracısına tıklayın. **Hızlı Başlangıç** sekmesinde (açılan ilk sekme), **Yönetici** altındaki **B2C ayarlarını yönet** öğesine tıklayın. Bu işlem, B2C özellikleri dikey penceresini yeni bir tarayıcı penceresinde veya sekmesinde açar.

Ayrıca, **B2C ayarlarını yönet** bağlantısını **Yapılandır** sekmesindeki **B2C Yönetimi** bölümünde de bulabilirsiniz.

## Bir uygulamayı kaydetme

1. Azure portalındaki B2C özellikleri dikey penceresinde **Uygulamalar** seçeneğine tıklayın.
2. Dikey pencerenin en üstündeki **+Ekle** seçeneğine tıklayın.
3. Uygulamanızı tüketicilere tanımlayacak bir **Ad** girin. Örneğin, "Contoso B2C uygulaması"na girebilirsiniz.
4. Web tabanlı bir uygulama yazıyorsanız **Web uygulamasını/web API'sini dahil et** düğmesini **Evet** olarak değiştirin. **Yanıt URL'leri**, Azure AD B2C'nin, uygulamanız tarafından istenen belirteçleri getirdiği uç noktalardır. Örneğin, `https://localhost:44321/` girin. Uygulamanız, korunması gereken sunucu tarafı bileşeni (API) içeriyorsa **Anahtar Oluştur** düğmesine tıklayarak da bir **Uygulama Gizli Anahtarı** oluşturmak ve kopyalamak istersiniz.

    > [AZURE.NOTE]
    **Uygulama Gizli Anahtarı**, önemli bir güvenlik kimlik bilgisidir.

5. Mobil bir uygulama yazıyorsanız **Yerel istemci dahil et** düğmesini **Evet** olarak değiştirin. Sizin için otomatik olarak oluşturulan varsayılan **Yeniden Yönlendirme URI'sini** kopyalayın.
6. Uygulamanızı kaydetmek için **Oluştur** seçeneğine tıklayın.
7. Oluşturduğunuz uygulamaya tıklayın ve daha sonra kodunuzda kullanacağınız genel benzersiz **Uygulama İstemci Kimliğini** kopyalayın.

## Hızlı Başlangıç Uygulaması oluşturma

Azure AD B2C'ye kayıtlı bir uygulamaya sahip olduğunuza göre başlamak ve çalıştırmak için hızlı başlangıç öğreticilerimizden birini tamamlayabilirsiniz. İşte birkaç öneri:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]



<!---HONumber=Jun16_HO2-->


