<properties
    pageTitle="Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama | Microsoft Azure"
    description="Azure AD Uygulama Ara Sunucusu ile şirket içi uygulamaları bulutta yayımlayın."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="stevenpo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="kgremban"/>


# Azure AD Uygulama Ara Sunucusu ile uygulama yayımlama


Microsoft Azure Active Directory (AD) Uygulama Ara Sunucusunu etkinleştirdikten sonra, uzak kullanıcıların özel ağ dışından erişebilmeleri için şirket içi uygulamaları yayımlayabilirsiniz.

Bu makalede, yerel ağınızda çalışan uygulamaları yayımlamaya ve ağınızın dışından güvenli uzaktan erişim sağlamaya ilişkin adımlar bulunur. Uygulama Ara Sunucusunu ayarlamadıysanız veya herhangi bir Bağlayıcıyı yüklemediyseniz devam etmeden önce [Azure portalında Uygulama Ara Sunucusunu Etkinleştirme](active-directory-application-proxy-enable.md) makalesinde belirtilen adımları uygulayın.

Azure AD Uygulama Ara Sunucusunu ilk defa kullanıyorsanız uygulamaları yayımlamadan önce, özel ağınızdan bir web sitesi yayımlayarak Bağlayıcıyı test etmenizi öneririz.

> [AZURE.NOTE] Uygulama Ara Sunucusu özelliğini, yalnızca Azure Active Directory'nin Premium veya Basic sürümüne yükseltmeniz halinde kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md).

## Sihirbazı kullanarak uygulama yayımlama

1. [Klasik Azure portalında](https://manage.windowsazure.com/) yönetici olarak oturum açın.
2. Active Directory'ye gidip Uygulama Ara Sunucusunu etkinleştirdiğiniz dizini seçin.

    ![Active Directory - simge](./media/active-directory-application-proxy-publish/ad_icon.png)

3. **Uygulamalar** sekmesine tıklayın ve ardından ekranın altındaki **Ekle** düğmesine tıklayın.

    ![Uygulama ekleme](./media/active-directory-application-proxy-publish/aad_appproxy_selectdirectory.png)

4. **Ağınızın dışından erişilebilecek olan bir uygulamayı yayımlama** seçeneğini belirleyin.

    ![Ağınızın dışından erişilebilecek olan bir uygulamayı yayımlama](./media/active-directory-application-proxy-publish/aad_appproxy_addapp.png)

5. Uygulamanız ile ilgili şu bilgileri sağlayın:

    - **Ad**: Uygulamanız için kolay ad. Bu ad, dizininizde benzersiz olmalıdır.
    - **İç URL**: Uygulama Ara Sunucusu Bağlayıcısının özel ağınızdan uygulamaya erişmek için kullandığı adres. Arka uç sunucusundaki belirli bir yolun yayımlanmasını sağlayabilirsiniz. Sunucunun geri kalanı yayımlanmaz. Bu şekilde aynı sunucuda farklı siteleri yayımlayabilir; her biri için farklı bir ad ve erişim kuralları belirleyebilirsiniz.
    - **Ön Kimlik Doğrulama Yöntemi**: Uygulama Ara Sunucusunun, uygulamanıza erişim izni vermeden önce kullanıcıları doğrulama yöntemi. Açılır menüdeki seçeneklerden birini belirleyin.

        - Azure Active Directory: Uygulama Ara Sunucusu, kullanıcıları Azure AD'de oturum açmaya yönlendirir. Burada, kullanıcıların dizin ve uygulama izinlerine yönelik kimlik doğrulaması gerçekleştirilir.
        - Geçiş: Kullanıcıların uygulamaya erişmek için kimliklerini doğrulaması gerekmez.

    ![Uygulama özellikleri](./media/active-directory-application-proxy-publish/aad_appproxy_appproperties.png)  

6. Sihirbazı tamamlamak için ekranın altındaki onay işaretine tıklayın. Uygulama artık Azure AD içinde tanımlıdır.


## Uygulamaya kullanıcı ve grup atama

Kullanıcılarınızın yayımlanan uygulamanıza erişmeleri için onları ayrı ayrı veya gruplar halinde atamanız gerekir. Bu, ön kimlik doğrulaması gerektiren uygulamalar için uygulamayı kullanma izni verir. Ön kimlik doğrulaması gerektirmeyen uygulamalar için kullanıcıların izne ihtiyacı yoktur ancak yine de uygulamanın kendi uygulama listelerinde görünmesi için uygulamaya atanmaları gerekir.

1. Uygulama Ekleme sihirbazını tamamladıktan sonra uygulamanıza ilişkin Hızlı Başlangıç sayfasını görürsünüz. Uygulamaya kimlerin erişebildiğini yönetmek için **Kullanıcılar ve gruplar**'ı seçin.

    ![Uygulama Ara Sunucusu hızlı başlangıç kullanıcı atama - ekran görüntüsü](./media/active-directory-application-proxy-publish/aad_appproxy_usersgroups.png)

2. Dizininizde belirli grupları arayın veya tüm kullanıcılarınızı gösterin. Sonuçları görüntülemek için onay işaretine tıklayın.

    ![Grup veya kullanıcı arama - ekran görüntüsü](./media/active-directory-application-proxy-publish/aad_appproxy_search.png)

2. Bu uygulamaya atamak istediğiniz tüm kullanıcıları ve grupları seçip **Ata**'ya tıklayın. Bu eylemi onaylamanız istenir.

> [AZURE.NOTE] Tümleşik Windows Kimlik Doğrulaması Uygulamaları için yalnızca şirket içi Active Directory'nizden eşitlenen kullanıcıları ve grupları atayabilirsiniz. Microsoft hesabı ile oturum açan kullanıcılar ve konuklar, Azure Active Directory Uygulama Ara Sunucusu ile yayımlanan uygulamalar için atanamaz. Kullanıcılarınızın yayımladığınız uygulama ile aynı etki alanının parçası olan kimlik bilgileriyle oturum açtıklarından emin olun.


## Gelişmiş yapılandırma

Yapılandırma sayfasında, yayımlanan uygulamaları değiştirebilir veya gelişmiş seçenekler belirleyebilirsiniz. Bu sayfada, ad değiştirerek veya bir logoyu karşıya yükleyerek uygulamanızı özelleştirebilirsiniz. Ayrıca, ön kimlik doğrulama yöntemi veya çok faktörlü kimlik doğrulaması gibi erişim kurallarını yönetebilirsiniz.

![Gelişmiş yapılandırma](./media/active-directory-application-proxy-publish/aad_appproxy_configure.png)


Uygulamalar, Azure Active Directory Uygulama Ara Sunucusu kullanılarak yayımladıktan sonra Azure AD'de Uygulamalar listesinde görünür ve onları burada yönetebilirsiniz.

Uygulamaları yayımladıktan sonra Uygulama Ara Sunucusu hizmetlerini devre dışı bırakırsanız uygulamalar silinmez ancak onlara özel ağınızın dışından erişemezsiniz.

Bir uygulamayı görüntülemek ve erişilebilir durumda olduğundan emin olmak için uygulamanın adına çift tıklayın. Uygulama Ara Sunucusu hizmeti devre dışı bırakılır ve uygulama kullanılamazsa ekranın üstünde bir uyarı iletisi görünür.

Bir uygulamayı silmek için listeden bir uygulama seçin ve ardından **Sil**'e tıklayın.

## Sonraki adımlar

- [Kendi etki alanı adınızı kullanarak uygulama yayımlama](active-directory-application-proxy-custom-domains.md)
- [Çoklu oturum açmayı etkinleştirme](active-directory-application-proxy-sso-using-kcd.md)
- [Koşullu erişimi etkinleştirme](active-directory-application-proxy-conditional-access.md)
- [Talepleri kullanan uygulamalarla çalışma](active-directory-application-proxy-claims-aware-apps.md)

En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](http://blogs.technet.com/b/applicationproxyblog/) göz atın



<!--HONumber=Jun16_HO2-->


