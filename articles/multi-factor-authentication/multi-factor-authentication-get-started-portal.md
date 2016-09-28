<properties 
    pageTitle="Azure Multi-Factor Authentication Sunucusu için kullanıcı portalını dağıtma"
    description="Bu, nasıl Azure MFA ve kullanıcı portalını kullanmaya başlayacağınızı açıklayan Azure Multi-factor authentication sayfasıdır."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/15/2016"
    ms.author="kgremban"/>


# Azure Multi-Factor Authentication Sunucusu için kullanıcı portalını dağıtma

Kullanıcı Portalı yöneticinin Azure Multi-Factor Authentication Kullanıcı Portalı’nı yüklemesine ve yapılandırmasına olanak tanır. Kullanıcı Portalı, kullanıcıların Azure Multi-Factor Authentication’a kaydolmasını ve hesaplarını korumalarını sağlayan bir IIS web sitesidir. Bir kullanıcı, sonraki oturum açışı sırasında telefon numarasını, PIN’ini değiştirebilir ya da Azure Multi-Factor Authentication’ı atlayabilir.

Kullanıcılar kendi normal kullanıcı adı ve parolalarını kullanarak Kullanıcı Portalı’ndan oturum açar ve kendi kimlik doğrulamalarını tamamlamak için bir Azure Multi-Factor Authentication çağrısını tamamlar veya güvenlik sorularını yanıtlar. Kullanıcı kaydına izin veriliyorsa, kullanıcı ilk kez Kullanıcı Portalı’nda oturum açtığında kendi telefon numarasını ve PIN’ini yapılandırır.

Kullanıcı Portalı Yöneticileri yeni kullanıcı eklemek ve mevcut kullanıcıları güncelleştirmek üzere ayarlanabilir ve izin verilebilir.

<center>![Kurulum](./media/multi-factor-authentication-get-started-portal/install.png)</center>

## Azure Multi-Factor Authentication Sunucusu ile aynı sunucuda kullanıcı portalını dağıtma

Aşağıdaki ön koşullar Kullanıcı Portalı’nı Azure Multi-Factor Authentication Sunucusu ile aynı sunucuya yüklemek için gereklidir.

- asp.net ve IIS 6 metatabanı uyumluluğu (IIS 7 ya da üst sürümü için) dahil IIS yüklenmelidir.
- Oturum açmış kullanıcının, varsa bilgisayar ve Etki Alanı yönetici hakları olması gerekir.  Bunun nedeni hesabın Active Directory güvenlik grupları oluşturmak için izin gerektirmesidir.

### Azure Multi-Factor Authentication Sunucusu için kullanıcı portalını dağıtmak için

1. Azure Multi-Factor Authentication Sunucusu’nda: soldaki menüde Kullanıcı Portalı simgesine tıklayın ve Kullanıcı Portalı’nı Yükle düğmesine tıklayın
1. İleri'ye tıklayın.
1. İleri'ye tıklayın.
1. Bilgisayar bir etki alanına katıldıysa ve Kullanıcı Portalı ile Azure Multi-Factor Authentication hizmeti arasındaki hizmeti güvenli hale getirmek üzere Active Directory yapılandırması tamamlanmamışsa, Active Directory adımı görüntülenir. Otomatik olarak bu yapılandırmayı tamamlamak için İleri düğmesine tıklayın.
1. İleri'ye tıklayın.
1. İleri'ye tıklayın.
1. Kapat’a tıklayın.
1. Herhangi bir bilgisayarda web tarayıcısını açın ve Kullanıcı Portalı’nın yüklendiği URL'ye gidin (örn. https://www.publicwebsite.com/MultiFactorAuth). Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.

<center>![Kurulum](./media/multi-factor-authentication-get-started-portal/portal.png)</center>

## Azure Multi-Factor Authentication Sunucusu Kullanıcı Portalı’nı Farklı Sunucuda dağıtma

Azure Multi-Factor Authentication Uygulamasını kullanmak için, uygulamanın Kullanıcı Portalı ile başarıyla iletişim kurabilmesini sağlamak amacıyla aşağıdakiler gereklidir:

Donanım ve yazılım gereksinimleri için lütfen Donanım ve Yazılım Gereksinimleri’ne bakın.

- Azure Multi-Factor Authentication Sunucusu’nun v6.0 ya da üst sürümünü kullanıyor olmalısınız.
- Kullanıcı Portalı’nın, Microsoft® Internet Information Services (IIS) 6.x, IIS 7.x veya üst sürümünü çalıştıran bir İnternete yönelik web sunucusunda yüklü olması gerekir.
- IIS 6.x kullanırken, ASP.NET v2.0.50727’nin yüklü, kayıtlı ve İzinli olarak ayarlandığından emin olun.
- IIS 7.x ya da üst sürümünü kullanırken gerekli rol hizmetleri ASP.NET ve IIS 6 Metatabanı Uyumluluğu’nu içerir.
- Kullanıcı Portalı bir SSL sertifikası ile güvenli hale getirilmelidir.
- Azure Multi-Factor Authentication Web Hizmeti SDK’sı, Azure Multi-Factor Authentication Sunucusu’nun yüklendiği sunucuda IIS 6.x, IIS 7.x ya da üs sürümünde yüklenmelidir.
- Azure Multi-Factor Authentication Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir.
- Kullanıcı Portalı SSL üzerinden Azure Multi-Factor Authentication Web Hizmeti SDK’sına bağlanabilmelidir.
- Kullanıcı Portalı, “PhoneFactor Admins” adlı bir güvenlik grubunun üyesi olan hizmetin kimlik bilgilerini kullanarak Azure Multi-Factor Authentication Web Hizmeti SDK’sının kimliğini doğrulayabilmelidir. Azure Multi-Factor Authentication Sunucusu etki alanı ile birleşik bir sunucuda çalışıyorsa, bu hizmet hesabı ve grubu Active Directory’de yer alır. Bir etki alanı ile birleştirilmediyse, bu hizmet hesabı ve grubu yerel olarak Azure Multi-Factor Authentication Sunucusu’nda yer alır.

Azure Multi-Factor Authentication Sunucusu dışında bir sunucuya kullanıcı portalını yüklemek aşağıdaki üç adımı gerektirir:

1. Web hizmeti SDK’sını yükleme
2. Kullanıcı portalını yükleme
3. Azure Multi-Factor Authentication Sunucusu’nda Kullanıcı Portalı Ayarlarını yapılandırma


### Web hizmeti SDK’sını yükleme

Azure Multi-Factor Authentication Web Hizmeti SDK’sı Azure Multi-Factor Authentication Sunucusu’nda halihazırda yüklü değilse, bu sunucuya gidin ve Azure Multi-Factor Authentication Sunucusu’nu açın. Web Hizmeti SDK’sı simgesine tıklayın, Web Hizmeti SDK’sını yükle düğmesine... tıklayın ve sunulan yönergeleri izleyin. Web Hizmeti SDK’sı bir SSL sertifikası ile güvenli hale getirilmelidir. Kendinden imzalı bir sertifika bu amaç doğrultusunda kabul edilebilir, ancak SSL bağlantısı başlatıldığında bu sertifikaya güvenmesi için, Kullanıcı Portalı web sunucusundaki Yerel Bilgisayar hesabının “Güvenilen Kök Sertifika Yetkilileri” deposuna aktarılmalıdır.

<center>![Kurulum](./media/multi-factor-authentication-get-started-portal/sdk.png)</center>

### Kullanıcı portalını yükleme

Kullanıcı portalını ayrı bir sunucuda yüklemeden önce, aşağıdakilere dikkat edin:

- İnternet'e yönelik web sunucusunda bir web tarayıcısı açmak ve web.config dosyasına girilen Web hizmeti SDK’sının URL’sine gitmek faydalıdır. Tarayıcı web hizmetine başarıyla gidebilirse, sizden kimlik bilgilerinizi ister. Aynen dosyada göründüğü gibi web.config dosyasına girilen parola girilen kullanıcı adını ve parolayı girin. Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.
- Kullanıcı Portalı web sunucusunun önünde ters proxy ya da güvenlik duvarı yer alıyorsa ve SSL boşaltma gerçekleştiriyorsa, Kullanıcı Portalı’nın https yerine http kullanabilmesi için, Kullanıcı Portalı web.config dosyasını düzenleyebilir ve aşağıdaki anahtarı <appSettings> bölümüne ekleyebilirsiniz. <add key="SSL_REQUIRED" value="false"/>

#### Kullanıcı portalını yüklemek için

1. Azure Multi-Factor Authentication Sunucusu’nda Windows Gezgini'ni açın ve Azure Multi-Factor Authentication Sunucusu’nun yüklü olduğu klasöre gidin (örneğin, :\Program Files\Multi-Factor Authentication Server). Kullanıcı Portalı’nın yüklü olduğu sunucu için uygun şekilde MultiFactorAuthenticationUserPortalSetup yükleme dosyasının 32-bit veya 64-bit sürümünü seçin. Yükleme dosyasını İnternet’e yönelik sunucuya kopyalayın.
2. İnternet’e yönelik sunucuda, kurulum dosyası yönetici haklarıyla çalıştırılmalıdır. Bunu yapmanın en kolay yolu, yönetici olarak bir komut istemi açmak ve yükleme dosyasının kopyalandığı konuma gitmektir.
3. MultiFactorAuthenticationUserPortalSetup64 yükleme dosyasını çalıştırın, isterseniz Site’yi ve Sanal Dizin’i değiştirin.
4. Kullanıcı Portalı yüklenmesi tamamlandıktan, sonraki C:\inetpub\wwwroot\MultiFactorAuth (veya sanal dizin adını temel alarak uygun dizin) gidin ve web.config dosyasını düzenleyin.
5. USE_WEB_SERVICE_SDK anahtarını bulun ve değeri false iken true olarak değiştirin. WEB_SERVICE_SDK_AUTHENTICATION_USERNAME ve WEB_SERVICE_SDK_AUTHENTICATION_PASSWORD anahtarlarını bulun ve değerleri PhoneFactor Admins güvenlik grubunun bir üyesi olan hizmet hesabının kullanıcı adı ve parolası olarak ayarlayın (Yukarıdaki Gereksinimler bölümüne bakın). Satırın sonundaki tırnak işaretlerinin arasına, (value=””/>) Kullanıcı Adı ve Parolayı girdiğinizden emin olun. Tam kullanıcı adı kullanmanız önerilir (örneğin, etki alanı\kullanıcı adı veya makine\kullanıcı adı) kullanmak için önerilir.
6. pfup_pfwssdk_PfWsSdk ayarını bulun ve “http://localhost:4898/PfWsSdk.asmx” değerini Azure Multi-Factor Authentication Sunucusu’nda çalışan Web Hizmeti SDK’sının URL’si ile değiştirin (örneğin, https://computer1.domain.local/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx). Bu bağlantı için SSL kullanılması nedeniyle, SSL sertifikası sunucu adı için kullanılacağı ve kullanılan URL’nin sertifikadaki adla eşleşmesi gerektiği için Web Hizmeti SDK’sına IP adresi ile değil sunucu adıyla başvurmalısınız. Sunucu, İnternet’e yönelik sunucudan gelen bir IP adresini çözümlemezse, Azure Multi-Factor Authentication Sunucusu’nun adını bu IP adresine eşlemek için bu sunucudaki hosts dosyasına bir giriş ekleyin. Değişiklikler yapıldıktan sonra web.config dosyasını kaydedin.
7. Kullanıcı Portalı’nın altında yüklendiği web sitesi (örneğin Varsayılan Web Sitesi) halihazırda ortak olarak imzalanmış bir sertifikayla bağlanmadıysa, henüz yüklü değilse sertifikayı sunucuya yükleyin, IIS Yöneticisi’ni açın ve sertifikayı web sitesine bağlayın.
8. Herhangi bir bilgisayarda web tarayıcısını açın ve Kullanıcı Portalı’nın yüklendiği URL'ye gidin (örn. https://www.publicwebsite.com/MultiFactorAuth). Sertifika uyarısı ya da hatası görüntülenmediğinden emin olun.



## Azure Multi-Factor Authentication Sunucusu’nda kullanıcı portalı ayarlarını yapılandırma
Artık portal yüklendiğine göre, portal ile çalışmak için Azure Multi-Factor Authentication Sunucusu’nu yapılandırmalısınız.

Azure Multi-Factor Authentication Sunucusu kullanıcı portalı için çeşitli seçenekler sunar.  Aşağıdaki tabloda bu seçeneklerin ve ne için kullanıldıklarının açıklamasının bir listesi verilmiştir.

Kullanıcı Portalı Ayarları|Açıklama|
:------------- | :------------- |
Kullanıcı Portalı URL’si| Portalın barındırıldığı URL’yi girmenizi sağlar.
Birincil kimlik doğrulama| Portalda oturum açarken kullanılacak kimlik doğrulama türünü belirtmenizi sağlar.  Windows, Radius veya LDAP kimlik doğrulaması.
Kullanıcıların oturum açmasına izin verir|Kullanıcıların, Kullanıcı portalı oturum açma sayfasında kullanıcı adı ve parola girmesini sağlar.  Bu seçilmezse, kutuları gri görünür.
Kullanıcı kaydına izin ver|Onları telefon numarası gibi ek bilgi isteyen bir kurulum ekranına getirerek kullanıcıların multi-factor authentication’a kaydolmasını sağlar.  Yedek telefon numarası istemi kullanıcıların ikincil bir telefon numarası belirtmesine izin verir.  Üçüncü taraf OATH belirteci istemi kullanıcıların bir üçüncü tarafa OATH belirteci belirtmesine izin verir.
Kullanıcıların Bir Kerelik geçiş başlatmasına izin ver| Bu, kullanıcıların Bir Kerelik geçiş başlatmalarına izin verir.  Kullanıcı bunu ayarlarsa, kullanıcının sonraki oturum açışında etkili olur.  Atlama (saniye) kullanıcıya 300 saniye olan varsayılan değeri değiştirebilecekleri bir kutu sağlar.  Aksi halde, bir kerelik atlama yalnızca 300 saniye geçerlidir.
Kullanıcıların yöntemi seçmesine izin ver| Kullanıcıların kendi birincil bağlantı kurma yöntemini belirtmesine olanak tanır.  Bu telefon araması, SMS, mobil uygulama veya OATH belirteci olabilir.
Kullanıcıların dili seçmesine izin ver|  Kullanıcının telefon araması, SMS, mobil uygulama veya OATH belirteci için kullanılan dili seçmesine olanak sağlar.
Kullanıcıların mobil uygulama etkinleştirmesine izin ver| Kullanıcıların sunucuyla birlikte kullanılan mobil uygulama etkinleştirme işlemini tamamlamak üzere bir etkinleştirme kodu seçmesini sağlar.  Ayrıca bunu etkinleştirebilecekleri cihaz sayısını da ayarlayabilirsiniz.  1 ile 10 arasında.
Geri dönüş için güvenlik sorularını kullan|Multi-factor authentication başarısız olursa, güvenlik soruları kullanmanıza olanak sağlar.  Başarılı bir şekilde yanıtlanması gereken güvenlik sorusu sayısını belirtebilirsiniz.
Kullanıcıların üçüncü taraf OATH belirteci ilişkilendirmesine izin ver| Kullanıcıların üçüncü taraf OATH belirteci ilişkilendirmesine izin verir.
Geri dönüş için OATH belirtecini kullan|Multi-factor authentication’nın başarısız olması durumunda, bir OATH belirteci kullanımına izin verir.  Dakika cinsinden oturum zaman aşımı süresini de belirtebilirsiniz.
Günlü kaydını etkinleştir|Kullanıcı portalında günlük kaydını etkinleştirir.  Günlük dosyaları şu adreste bulunabilir: C:\Program Files\Multi-Factor Authentication Server\Logs.

Bu ayarların çoğu, etkinleştirildiklerinde ve kullanıcı, kullanıcı portalında oturum açtığında görünür.

![Kullanıcı portalı ayarları](./media/multi-factor-authentication-get-started-portal/portalsettings.png)



### Azure Multi-Factor Authentication Sunucusu’nda kullanıcı portalı ayarlarını yapılandırmak için




1. Azure Multi-Factor Authentication Sunucusu’nda Kullanıcı Portalı simgesine tıklayın. Ayarlar sekmesinde, Kullanıcı Portalı URL'si metin kutusuna Kullanıcı Portalı URL'sini girin. Bu URL, e-posta işlevselliği etkinleştirildiğinde, Azure Multi-Factor Authentication Sunucusu’na aktarıldıkları zaman kullanıcılara gönderilen e-postalara eklenir.
2. Kullanıcı Portalı'nda kullanmak istediğiniz ayarları seçin. Örneğin, kullanıcıların kendi kimlik doğrulama yöntemlerini denetlemesine izin vermek için, seçim yapabilecekleri yöntemlerle birlikte Kullanıcıların yöntemi seçmesine izin ver seçeneğinin seçili olduğundan emin olun.
3. Görüntülenen ayarları anlamaya ilişkin yardım için sağ üst köşedeki Yardım bağlantısına tıklayın.

<center>![Kurulum](./media/multi-factor-authentication-get-started-portal/config.png)</center>


## Yöneticiler sekmesi
Bu sekme yalnızca yönetici ayrıcalıklarına sahip olacak kullanıcıları eklemenizi sağlar.  Bir yönetici eklerken, aldıkları izinleri ayrıntılı olarak ayarlayabilirsiniz.  Bu şekilde, yöneticiye yalnızca gerekli izinleri verdiğinizden emin olabilirsiniz.  Ekle düğmesine tıklamanız ve ardından, kullanıcıyı izinlerini seçmeniz ve son olarak Ekle'ye tıklamanız geçerlidir.

![Kullanıcı portalı yöneticileri](./media/multi-factor-authentication-get-started-portal/admin.png)


## Güvenlik Soruları
Bu sekme, Geri dönüş için güvenlik sorularını kullan seçeneği seçili ise, kullanıcıların yanıtları sağlaması gereken güvenlik sorularını belirtmenizi sağlar.  Azure Multi-Factor Authenticaton Sunucusu kullanabileceğiniz varsayılan sorularla birlikte gelir.  Ayrıca, soruların sırasını değiştirebilir ay da kendi sorularınızı ekleyebilirsiniz.  Kendi sorularınızı eklerken, bu soruların görünmesini istediğiniz dili de belirtebilirsiniz.

![Kullanıcı portalı güvenlik soruları](./media/multi-factor-authentication-get-started-portal/secquestion.png)


## Geçen Oturumlar

## SAML
SAML kullanarak bir kimlik sağlayıcısından gelen talepleri kabul etmek için kullanıcı portalını ayarlamanıza olanak sağlar.  Oturum zaman aşımını belirtebilir, doğrulama sertifikasını ve Oturumu kapatma yönlendirme URL’sini belirtebilirsiniz.

![SAML](./media/multi-factor-authentication-get-started-portal/saml.png)

## Güvenilen IP'ler
Bu sekme, bir kullanıcı bu adreslerden birinden oturum açarsa, multi-factor authentication’ın atlanacağı şekilde, eklenebilecek tek bir IP adresi ya da IP adresleri aralığı belirtmenize olanak tanır.

![Kullanıcı portalı güvenilen IP'leri](./media/multi-factor-authentication-get-started-portal/trusted.png)

## Self Servis Kullanıcı Kaydı
Kullanıcılarının oturum açmasını ve kaydolmasını istiyorsanız, Kullanıcıların oturum açmasına izin ver ve Kullanıcı kaydına izin ver seçeneklerini seçmelisiniz. Seçtiğiniz ayarların kullanıcı oturum açma deneyimini etkileyeceğini unutmayın.

Örneğin, bir kullanıcı Kullanıcı Portalı’nda oturum açtığında ve Oturum Aç düğmesine tıkladığında, Azure Multi-Factor Authentication Kullanıcı Kurulumu sayfasına yönlendirilir.  Azure Multi-Factor Authentication’ı nasıl yapılandırdığınıza bağlı olarak, kullanıcı kendi kimlik doğrulama yöntemini seçebilir.  

Sesli Arama kimlik doğrulama yöntemini seçerse ya da bu yöntemi kullanmak üzere önceden yapılandırıldıysa, sayfa kullanıcıdan birincil telefonu numarasını ve varsa dahili numarayı girmesini ister.  Ayrıca kullanıcının yedek telefon numarası girmesine de izin verilir.  

![Kullanıcı portalı güvenilen IP'leri](./media/multi-factor-authentication-get-started-portal/backupphone.png)

Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN’i girmesini de ister.  Kendi telefon numaralarını ve PIN’i (varsa) girdikten sonra, kullanıcı Kimlik Doğrulaması için şimdi Beni Ara düğmesine tıklar.  Azure Multi-Factor Authentication kullanıcının birincil telefon numarasına bir telefon araması yapar.  Kullanıcı aramayı yanıtlamalı ve PIN’ini girmeli (varsa) ve self servis kayıt işleminin sonraki adımına geçmek için # tuşuna basmalıdır.   

Kullanıcı SMS Metni kimlik doğrulama yöntemini seçerse ya da bu yöntemi kullanmak üzere önceden yapılandırıldıysa, sayfa kullanıcıdan birincil telefonu numarasını ister.  Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN’i girmesini de ister.  Kendi telefon numarasını ve PIN’ini (varsa) girdikten sonra, kullanıcı Kimlik Doğrulaması için şimdi Bana SMS gönder düğmesine tıklar.  Azure Multi-Factor Authentication kullanıcının mobil uygulamasına bir SMS kimlik doğrulaması yapar.  Kullanıcı tek seferlik parolayı (OTP) içeren bir SMS almalı ve bu SMS’i bu OTP’nin yanı sıra kendi PIN’ini (varsa) ile yanıtlayarak self servis kayıt işleminin sonraki adımına geçmelidir.

![Kullanıcı portalı SMS](./media/multi-factor-authentication-get-started-portal/text.png)   

Kullanıcı Mobil uygulama kimlik doğrulama yöntemini seçerse veya bu yöntemi kullanmak için önceden yapılandırılmış ise, sayfa cihazlarına Azure Multi-Factor Authentication uygulamasını yüklemek ve bir etkinleştirme kodu oluşturmak için kullanıcıya sorar.  Azure Multi-Factor Authentication uygulamasını yükledikten sonra, kullanıcı Etkinleştirme Kodu Oluştur düğmesine tıklar.    

>[AZURE.NOTE]Azure Multi-Factor Authentication uygulamasını kullanmak için, kullanıcının kendi cihazı için anında iletme bildirimlerini etkinleştirmesi gerekir.

Böylece sayfada bir barkod resmi ile birlikte, etkinleştirme kodu ve bir URL görüntülenir.  Kimlik doğrulaması sırasında kullanıcının PIN kullanması gerekiyorsa, sayfa kullanıcıdan PIN’i girmesini de ister.  Kullanıcı Azure Multi-Factor Authentication uygulamasına etkinleştirme kodunu ve URL’yi girer ya da barkod resmini taramak için barkod tarayıcısını kullanır ve Etkinleştir düğmesine tıklar.    

Etkinleştirme tamamlandıktan sonra, kullanıcı Şimdi Kimliğimi Doğrula düğmesine tıklar.  Azure Multi-Factor Authentication kullanıcının mobil uygulamasına bir kimlik doğrulaması yapar.  Kullanıcı PIN’ini girmeli (varsa) ve self servis kayıt işleminin sonraki adımına geçmek için mobil uygulamasında Kimliği Doğrula düğmesini basmalıdır.  


Yöneticiler Azure Multi-Factor Authentication Sunucusu’nu güvenlik soruları ve yanıtları toplamak üzere yapılandırmışsa, kullanıcı Güvenlik Soruları sayfasına yönlendirilir.  Kullanıcı dört güvenlik sorusu seçmeli ve seçtiği sorulara yanıt vermelidir.    

![Kullanıcı portalı güvenlik soruları](./media/multi-factor-authentication-get-started-portal/secq.png)  

Kullanıcı self servis kayıt işlemi artık tamamlanmış ve kullanıcı Kullanıcı Portalı’nda oturum açmıştır.  Kullanıcılar, yöneticileri izin vermişse, gelecekte istedikleri zaman telefon numarası, PIN, kimlik doğrulama yöntemi ve güvenlik sorularını değiştirmek için Kullanıcı Portalı’na dönebilirler.



<!--HONumber=Sep16_HO3-->


