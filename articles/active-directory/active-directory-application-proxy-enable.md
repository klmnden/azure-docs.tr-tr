<properties
    pageTitle="Azure AD Uygulama Ara Sunucusunu Etkinleştirme | Microsoft Azure"
    description="Klasik Azure portalındaki Uygulama Ara Sunucusunu kapatıp ters ara sunucuya ilişkin Bağlayıcıları yükleyin."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="StevenPo"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="06/01/2016"
    ms.author="kgremban"/>

# Azure portalında Uygulama Ara Sunucusunu etkinleştirme

Bu makale, Azure AD'deki bulut dizininiz için Microsoft Azure AD Uygulama Ara Sunucusunu etkinleştirmenize kılavuzluk eder. Özel ağınıza Uygulama Ara Sunucusu Bağlayıcısını nasıl yükleyeceğiniz de açıklanmaktadır. Bu, ağınızla ara sunucu hizmetiniz arasında bağlantı kurmanızı sağlar. Ayrıca, bu Bağlayıcıyı Microsoft Azure AD kiracı aboneliğinize de kaydedeceksiniz. Uygulama Ara Sunucusu ile neler yapabileceğinizi öğrenmek istiyorsanız [Şirket içi uygulamalara güvenli uzaktan erişim sağlama](active-directory-application-proxy-get-started.md) hakkında daha fazla bilgi edinin.

Azure AD Uygulaması Ara Sunucusu'nu etkinleştirmeye ilişkin bu kılavuzda yer alan adımları tamamladıktan sonra şirket içi uygulamalarınızı uzaktan erişim için yayımlamaya hazır olacaksınız.

> [AZURE.NOTE] Uygulama Ara Sunucusu özelliğini, yalnızca Azure Active Directory'nin Premium veya Basic sürümüne yükseltmeniz halinde kullanabilirsiniz. Daha fazla bilgi için bkz. [Azure Active Directory sürümleri](active-directory-editions.md).

## Uygulama Ara Sunucusu önkoşulları
Uygulama Ara Sunucusu hizmetlerini etkinleştirip kullanabilmeniz için şunlara sahip olmanız gerekir:

- [Microsoft Azure AD temel veya premium aboneliği](active-directory-editions.md) ve genel yöneticisi olduğunuz bir Azure AD dizini.
- Uygulama Ara Sunucusu Bağlayıcısı'nı yükleyebileceğiniz; Windows Server 2012 R2, Windows 8.1 veya sonraki sürümlerini çalıştıran bir sunucu. Sunucu, buluttaki Uygulama Ara Sunucusu hizmetlerine HTTPS istekleri gönderir ve yayımlamak istediğiniz uygulamalara yönelik bir HTTPS bağlantısının olmasını gerektirir.
- Yolda bir güvenlik duvarı varsa Bağlayıcının, Uygulama Ara Sunucusuna HTTPS (TCP) istekleri yapabilmesi için güvenlik duvarının açık olduğundan emin olun. Bağlayıcı, bu bağlantı noktalarını üst düzey etki alanlarının bir parçası olan şu alt etki alanlarıyla birlikte kullanır: *msappproxy.net* ve *servicebus.windows.net*. Şu bağlantı noktalarının **tümünü** **giden** trafiğine açtığınızdan emin olun:

  	| Bağlantı Noktası Numarası | Açıklama |
  	| --- | --- |
  	| 80 | Güvenlik doğrulaması için giden HTTP trafiğini etkinleştirmek üzere kullanılır. |
  	| 443 | Azure AD'ye yönelik kullanıcı kimlik doğrulamasını etkinleştirmek için kullanılır. (Yalnızca Bağlayıcı kayıt işlemi için gereklidir.) |
  	| 10100-10120 | Ara sunucuya geri gönderilen LOB HTTP yanıtlarını etkinleştirmek için kullanılır |
  	| 9352, 5671 | Gelen istekler için Azure hizmetiyle Bağlayıcı arasındaki iletişimi etkinleştirmek için kullanılır. |
  	| 9350 | İsteğe bağlı olarak, gelen istekler konusunda daha iyi performans sağlamak için kullanılır |
  	| 8080 | Bağlayıcı önyükleme sırasını ve Bağlayıcı otomatik güncelleştirmesini etkinleştirmek için kullanılır |
  	| 9090 | Bağlayıcı kaydını etkinleştirmek için kullanılır (Yalnızca Bağlayıcı kayıt işlemi için gereklidir) |
  	| 9091 | Bağlayıcı güven sertifikası için otomatik yenilemeyi etkinleştirmek üzere kullanılır |

Güvenlik duvarınız kaynak kullanıcılar için trafiği zorunlu kılarsa Ağ Hizmeti olarak çalışan Windows hizmetlerinden gelen trafik için bu bağlantı noktalarını açın. Ayrıca, NT Authority\System için bağlantı noktası 8080'i etkinleştirdiğinizden emin olun.


## 1. Adım: Azure AD'de Uygulama Ara Sunucusunu etkinleştirme
1. [Klasik Azure portalında](https://manage.windowsazure.com/) yönetici olarak oturum açın.
2. Active Directory'ye gidip Uygulama Ara Sunucusunu etkinleştirmek istediğiniz dizini seçin.

    ![Active Directory - simge](./media/active-directory-application-proxy-enable/ad_icon.png)

3. Dizin sayfasında **Yapılandır**'ı seçip **Uygulama Ara Sunucusu** için aşağı kaydırın.
4. **Bu Dizin için Uygulama Ara Sunucusu Hizmetlerini Etkinleştir** seçeneğini **Etkin** olarak değiştirin.

    ![Uygulama Ara Sunucusunu etkinleştirme](./media/active-directory-application-proxy-enable/app_proxy_enable.png)

5. **Şimdi indir**'i seçin. **Azure AD Uygulama Ara Sunucusu Bağlayıcısı İndirme** sayfasına yönlendirileceksiniz. Lisans koşullarını okuyup kabul edin ve Uygulama Ara Sunucusu Bağlayıcısı'na ilişkin Windows Installer dosyasını (.exe) kaydetmek için **İndir**'e tıklayın.

## 2. Adım: Bağlayıcıyı yükleme ve kaydetme
1. Yukarıdaki önkoşullara göre hazırladığınız sunucuda *AADApplicationProxyConnectorInstaller.exe* öğesini çalıştırın.
2. Yüklemek için sihirbazdaki yönergeleri uygulayın.
3. Yükleme sırasında, Bağlayıcıyı Azure AD kiracınızın Uygulama Ara Sunucusuna kaydetmeniz istenir.

  - Azure AD genel yönetici kimlik bilgilerinizi sağlayın. Genel yönetici kiracınız, Microsoft Azure kimlik bilgilerinizden farklı olabilir.
  - Bağlayıcıyı kaydeden yöneticinin, Uygulama Ara Sunucusu hizmetini etkinleştirdiğiniz dizinde olduğundan emin olun. (Örneğin, kiracı etki alanı contoso.com ise yöneticinin admin@contoso.com veya o etki alanındaki başka bir diğer ad olması gerekir.)
  - **IE Artırılmış Güvenlik Yapılandırması** Azure AD Bağlayıcısını yüklediğiniz sunucuda **Etkin** olarak ayarlandıysa kayıt ekranı engellenebilir. Bu durumda, erişim izni vermek için hata iletisindeki yönergeleri uygulayın. Internet Explorer Artırılmış Güvenlik seçeneğinin devre dışı olduğundan emin olun.
  - Bağlayıcı kaydı başarısız olursa bkz. [Uygulama Ara Sunucusu Sorunlarını Giderme](active-directory-application-proxy-troubleshoot.md).  

4. Yükleme tamamlandığında, sunucunuza aşağıda gösterildiği gibi iki yeni hizmet eklenir.

    - **Microsoft AAD Application Proxy Connector** bağlantıyı etkinleştirir
    - **Microsoft AAD Application Proxy Connector Updater**, Bağlayıcının yeni sürümlerini düzenli aralıklarla denetleyen ve Bağlayıcıyı gereken şekilde güncelleştiren otomatik bir güncelleştirme hizmetidir.

    ![Uygulama Ara Sunucusu Bağlayıcısı hizmetleri - ekran görüntüsü](./media/active-directory-application-proxy-enable/app_proxy_services.png)

5. Yüklemeyi tamamlamak için yükleme penceresinde **Son**'a tıklayın.

Artık [Uygulama Ara Sunucusu ile uygulamaları yayımlamaya](active-directory-application-proxy-publish.md) hazırsınız.

Yüksek düzeyde kullanılabilirlik sağlamak için en az bir ek Bağlayıcı dağıtmanız gerekir. Daha fazla Bağlayıcı dağıtmak için yukarıda belirtilen 2. ve 3. adımı yineleyin. Her Bağlayıcı ayrı ayrı kaydedilmelidir.

Bağlayıcı kaldırmak istiyorsanız hem Bağlayıcı hizmetini hem de Güncelleştirici hizmetini kaldırın ve ardından hizmeti bilgisayarınızdan tamamen kaldırmak için bilgisayarınızı yeniden başlatın.


## Sonraki adımlar

- [Uygulama Ara Sunucusu ile uygulama yayımlama](active-directory-application-proxy-publish.md)
- [Kendi etki alanı adınızı kullanarak uygulama yayımlama](active-directory-application-proxy-custom-domains.md)
- [Çoklu oturum açmayı etkinleştirme](active-directory-application-proxy-sso-using-kcd.md)
- [Uygulama Ara Sunucusu ile ilgili sorunları giderme](active-directory-application-proxy-troubleshoot.md)

En yeni haberler ve güncelleştirmeler için [Uygulama Ara Sunucusu bloguna](http://blogs.technet.com/b/applicationproxyblog/) göz atın



<!---HONumber=Jun16_HO2-->


