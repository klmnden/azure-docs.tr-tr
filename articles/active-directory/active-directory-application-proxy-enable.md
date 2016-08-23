<properties
    pageTitle="Azure AD Uygulama Ara Sunucusunu Etkinleştirme | Microsoft Azure"
    description="Klasik Azure portalındaki Uygulama Ara Sunucusunu kapatıp ters ara sunucuya ilişkin Bağlayıcıları yükleyin."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/19/2016"
    ms.author="kgremban"/>

# Azure portalında Uygulama Ara Sunucusunu etkinleştirme

Bu makale, Azure AD'deki bulut dizininiz için Microsoft Azure AD Uygulama Ara Sunucusunu etkinleştirme adımlarında size kılavuzluk eder.

Uygulama Ara Sunucusu ile neler yapabileceğinizi öğrenmek istiyorsanız [Şirket içi uygulamalara güvenli uzaktan erişim sağlama](active-directory-application-proxy-get-started.md) hakkında daha fazla bilgi edinin.

## Uygulama Ara Sunucusu önkoşulları
Uygulama Ara Sunucusu hizmetlerini etkinleştirip kullanabilmeniz için şunlara sahip olmanız gerekir:

- [Microsoft Azure AD temel veya premium aboneliği](active-directory-editions.md) ve genel yöneticisi olduğunuz bir Azure AD dizini.
- Uygulama Ara Sunucusu Bağlayıcısı'nı yükleyebileceğiniz; Windows Server 2012 R2, Windows 8.1 veya sonraki sürümlerini çalıştıran bir sunucu. Sunucu, buluttaki Uygulama Ara Sunucusu hizmetlerine istek gönderir ve yayımlamakta olduğunuz uygulamalara yönelik bir HTTP veya HTTPS bağlantısının olmasını gerektirir.

    - Yayımladığınız uygulamalarda çoklu oturum açma için bu makinenin yayımlamakta olduğunuz uygulamalarla aynı AD etki alanına katılmış olması gerekir.

- Yolda bir güvenlik duvarı varsa Bağlayıcının, Uygulama Ara Sunucusuna HTTPS (TCP) istekleri yapabilmesi için güvenlik duvarının açık olduğundan emin olun. Bağlayıcı, bu bağlantı noktalarını üst düzey etki alanlarının bir parçası olan şu alt etki alanlarıyla birlikte kullanır: msappproxy.net ve servicebus.windows.net. Aşağıdaki bağlantı noktalarının **giden** trafiğine açtığınızdan emin olun:

  	| Bağlantı Noktası Numarası | Açıklama |
  	| --- | --- |
  	| 80 | Güvenlik doğrulaması için giden HTTP trafiğini etkinleştirin. |
  	| 443 | Azure AD'ye yönelik kullanıcı kimlik doğrulamasını etkinleştirin (Yalnızca Bağlayıcı kayıt işlemi için gereklidir) |
  	| 10100–10120 | Ara sunucuya geri gönderilen LOB HTTP yanıtlarını etkinleştirin |
  	| 9352, 5671 | Gelen istekler için Azure hizmetiyle Bağlayıcı arasındaki iletişimi etkinleştirin. |
  	| 9350 | İsteğe bağlı olarak, gelen istekler konusunda daha iyi performans sağlamak için kullanılır |
  	| 8080 | Bağlayıcı önyükleme sırasını ve Bağlayıcı otomatik güncelleştirmesini etkinleştirin |
  	| 9090 | Bağlayıcı kaydını etkinleştirin (Yalnızca Bağlayıcı kayıt işlemi için gereklidir) |
  	| 9091 | Bağlayıcı güven sertifikası için otomatik yenilemeyi etkinleştirin |

    Güvenlik duvarınız kaynak kullanıcılar için trafiği zorunlu kılarsa Ağ Hizmeti olarak çalışan Windows hizmetlerinden gelen trafik için bu bağlantı noktalarını açın. Ayrıca, NT Authority\System için bağlantı noktası 8080'i etkinleştirdiğinizden emin olun.

- Kuruluşunuz İnternet'e bağlanmak için proxy sunucuları kullanıyorsa bunları yapılandırma ile ilgili ayrıntılar için [Var olan şirket içi proxy sunucuları ile çalışma](https://blogs.technet.microsoft.com/applicationproxyblog/2016/03/07/working-with-existing-on-prem-proxy-servers-configuration-considerations-for-your-connectors/) blog gönderisine bakın.

## 1. Adım: Azure AD'de Uygulama Ara Sunucusunu etkinleştirme
1. [Klasik Azure portalında](https://manage.windowsazure.com/) yönetici olarak oturum açın.
2. Active Directory'ye gidip Uygulama Ara Sunucusunu etkinleştirmek istediğiniz dizini seçin.

    ![Active Directory - simge](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Dizin sayfasında **Configure (Yapılandır)** seçeneğini belirleyip **Application Proxy (Uygulama Ara Sunucusu)** için aşağı kaydırın.
4. **Enable Application Proxy Services for this Directory (Bu Dizin için Uygulama Ara Sunucusu Hizmetlerini Etkinleştir)** seçeneğini **Enabled (Etkin)** olarak değiştirin.

    ![Uygulama Ara Sunucusunu etkinleştirme](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. **Download now (Şimdi indir)** seçeneğini belirleyin. **Azure AD Uygulama Proxy Bağlayıcısı İndirme** sayfasına yönlendirileceksiniz. Lisans koşullarını okuyup kabul edin ve bağlayıcıya ait Windows Installer dosyasını (.exe) kaydetmek için **İndir**'e tıklayın.

## 2. Adım: Bağlayıcıyı yükleme ve kaydetme
1. Önkoşullara göre hazırladığınız sunucuda **AADApplicationProxyConnectorInstaller.exe** öğesini çalıştırın.
2. Yüklemek için sihirbazdaki yönergeleri uygulayın.
3. Yükleme sırasında bağlayıcıyı Azure AD kiracınızın Uygulama Proxy Sunucusuna kaydetmeniz istenir.

  - Azure AD genel yönetici kimlik bilgilerinizi sağlayın. Genel yönetici kiracınız, Microsoft Azure kimlik bilgilerinizden farklı olabilir.
  - Bağlayıcıyı kaydeden yöneticinin Uygulama Proxy hizmetini etkinleştirdiğiniz dizinde olduğundan emin olun. Örneğin, kiracı etki alanı contoso.com ise yönetici admin@contoso.com veya bu etki alanındaki başka bir diğer ad olmalıdır.
  - **IE Artırılmış Güvenlik Yapılandırması** Bağlayıcıyı yüklediğiniz sunucuda **Açık** olarak ayarlandıysa kayıt ekranı engellenebilir. Erişim izni vermek için hata iletisindeki yönergeleri uygulayın. Internet Explorer Artırılmış Güvenlik seçeneğinin devre dışı olduğundan emin olun.
  - Bağlayıcı kaydı başarısız olursa bkz. [Uygulama Proxy’si Sorunlarını Giderme](active-directory-application-proxy-troubleshoot.md).  

4. Yükleme tamamlandığında sunucunuza iki yeni hizmet eklenir:

    - **Microsoft AAD Application Proxy Connector** bağlantıyı etkinleştirir
    - **Microsoft AAD Application Proxy Connector Updater**, bağlayıcının yeni sürümlerini düzenli aralıklarla denetleyen ve bağlayıcıyı gereken şekilde güncelleştiren otomatik bir güncelleştirme hizmetidir.

    ![Uygulama Ara Sunucusu Bağlayıcısı hizmetleri - ekran görüntüsü](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Yükleme penceresinde **Son**'a tıklayın.

Yüksek düzeyde kullanılabilirlik sağlamak için en az iki bağlayıcı dağıtmanız gerekir. Daha fazla bağlayıcı dağıtmak için yukarıda belirtilen 2. ve 3. adımı yineleyin. Her bağlayıcı ayrı ayrı kaydedilmelidir.

Bağlayıcıyı kaldırmak isterseniz hem Bağlayıcı hizmetini hem de Updater hizmetini kaldırın. Hizmeti tam olarak kaldırmak için bilgisayarınızı yeniden başlatın.


## Sonraki adımlar

Artık [Uygulama Ara Sunucusu ile uygulamaları yayımlamaya](active-directory-application-proxy-publish.md) hazırsınız.

Ayrı ağlarda veya farklı konumlarda uygulamalarınız varsa farklı bağlayıcıları mantıksal birimler halinde düzenlemek için bağlayıcı gruplarını kullanabilirsiniz. [Uygulama Proxy bağlayıcıları ile çalışma](active-directory-application-proxy-connectors.md) hakkında daha fazla bilgi edinin.



<!--HONumber=Aug16_HO1-->


