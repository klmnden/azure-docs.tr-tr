---
title: App Service ortamı - Azure iç yük dengeleyici oluşturma
description: İnternet’ten yalıtılmış bir Azure App Service Ortamı oluşturma ve kullanma ayrıntıları
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 06/12/2018
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 01d982d91d772ccfd468ccdac6391f971be4f43b
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546551"
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Bir App Service Ortamı ile iç yük dengeleyici oluşturma ve kullanma #

 Azure App Service Ortamı, Azure App Service’in Azure sanal ağı (VNet) içindeki bir alt ağa dağıtımıdır. Bir App Service Ortamı (ASE) iki şekilde dağıtılabilir: 

- Genellikle Dış ASE olarak adlandırılan durumda, bir dış IP adresi üzerindeki VIP ile.
- Genellikle iç uç nokta bir iç yük dengeleyici (ILB) olduğu için ILB ASE olarak adlandırılan durumda, bir iç IP adresi üzerindeki VIP ile. 

Bu makale bir ILB ASE oluşturma işlemini gösterir. ASE’ye genel bakış için bkz. [App Service Ortamlarına giriş][Intro]. Dış ASE oluşturma hakkında bilgi almak için bkz. [Dış ASE Oluşturma][MakeExternalASE].

## <a name="overview"></a>Genel Bakış ##

Bir ASE’yi İnternet’ten erişilebilen bir uç nokta ile ya da sanal ağınızdaki bir IP adresi ile dağıtabilirsiniz. IP adresini bir sanal ağ adresine ayarlamak için, ASE’nin ILB ile dağıtılması gerekir. ASE’nizi bir ILB ile dağıttığınızda şu bilgileri sağlamanız gerekir:

-   Uygulamalarınızı oluştururken kullandığınız kendi etki alanınız.
-   HTTPS için kullanılan sertifika.
-   Etki alanınızın DNS yönetimi.

Buna karşılık, şunun gibi şeyler yapabilirsiniz:

-   Siteden siteye veya Azure ExpressRoute VPN aracılığıyla erişebildiğiniz intranet uygulamalarını bulutta güvenli bir şekilde barındırma.
-   Genel DNS sunucularında listelenmeyen uygulamaları bulutta barındırma.
-   Ön uç uygulamalarınızın güvenli bir şekilde tümleştirilebileceği, İnternet’ten yalıtılmış arka uç uygulamaları oluşturma.

### <a name="disabled-functionality"></a>Devre dışı bırakılan işlevler ###

ILB ASE’yi kullanırken bazı işlemleri yapamazsınız:

-   IP tabanlı SSL kullanma.
-   Belirli uygulamalara IP adresleri atama.
-   Azure portalı üzerinden bir uygulama ile sertifika satın alma ve kullanma. Sertifikaları doğrudan bir sertifika yetkilisinden alabilir ve uygulamalarınızla kullanabilirsiniz. Sertifikaları Azure portalı üzerinden alamazsınız.

## <a name="create-an-ilb-ase"></a>ILB ASE oluşturma ##

ILB ASE oluşturmak için:

1. Azure portalında **kaynak Oluştur** > **Web** > **App Service ortamı**.

2. Aboneliğinizi seçin.

3. Kaynak grubunu seçin veya oluşturun.

4. Bir VNet seçin veya oluşturun.

5. Varolan bir sanal ağı seçerseniz, ASE’yi tutmak için bir alt ağ oluşturmanız gerekir. Alt ağ boyutunu, ASE’nizin gelecekteki her türlü büyümesine uyum sağlayacak kadar büyük ayarladığınızdan emin olun. 256 adres içeren ve en büyük boyutlu ASE’yi işleyebilen ve ölçeklendirme ihtiyaçlarını karşılayabilen `/24` dosya boyutu önerilir. 

6. Seçin **sanal ağ/konum** > **sanal ağ yapılandırması**. **VIP Türü**’nü **İç** olarak ayarlayın.

7. Etki alanı adı girin. Bu etki alanı, bu ASE içinde oluşturulan uygulamalar için kullanılır. Bazı kısıtlamalar vardır. Şunlar olamaz:

    * net   

    * azurewebsites.net

    * p.azurewebsites.net

    * &lt;asename&gt;.p.azurewebsites.net

   Yapabilecekleriniz [mevcut bir DNS adını uygulamanıza eşleme][customdomain]. Uygulamalar için kullanılan özel etki alanı adı ve ASE’niz için kullanılan etki alanı adı örtüşemez. Etki alanı adı _contoso.com_ olan bir ILB ASE ile uygulamalarınız için şunlar gibi özel etki alanı adlarını kullanamazsınız:

   * www\.contoso.com

   * abcd.def.contoso.com

   * abcd.contoso.com

   Uygulamalarınızın özel etki alanı adlarını biliyorsanız, ILB ASE için bu özel etki alanı adları ile çakışmayacak bir etki alanı seçin. Bu örnekte, *.contoso.com* ile sona eren özel etki alanı adlarıyla çakışmayacağından, ASE’nizin etki alanı için *contoso-internal.com* gibi bir şey kullanabilirsiniz.

8. **Tamam**’ı ve ardından **Oluştur**’u seçin.

    ![ASE oluşturma][1]

**Sanal Ağ** dikey penceresinde bir **Sanal Ağ Yapılandırması** seçeneği bulunur. Bu seçeneği kullanarak Dış VIP veya İç VIP seçebilirsiniz. Varsayılan seçenek **Dış**’tır. **Dış**’ı seçerseniz ASE’niz İnternet'ten erişilebilen bir VIP kullanır. **İç**’i seçerseniz ASE’niz sanal ağınızın içindeki bir IP adresinde bulunan ILB ile yapılandırılır.

**İç**’i seçtikten sonra ASE’nize daha fazla IP adresi ekleyemezsiniz. Bunun yerine, ASE’nin etki alanını sağlamanız gerekir. Dış VIP kullanılan bir ASE'de, ASE’nin adı bu ASE’de oluşturulan uygulamaların etki alanında kullanılır.

**VIP Türü**’nü **İç** olarak ayarlarsanız, ASE adınız ASE etki alanında kullanılmaz. Etki alanını açıkça belirtin. Etki alanınız varsa *contoso.corp.net* ve ASE adlı bir uygulama oluşturduğunuzda *timereporting*, bu uygulamanın URL'si timereporting.contoso.corp.NET şeklinde olur.


## <a name="create-an-app-in-an-ilb-ase"></a>ILB ASE'de uygulama oluşturma ##

ILB ASE'de uygulama oluşturma işlemi, normalde bir ASE’de uygulama oluşturma işlemiyle aynıdır.

1. Azure portalında **kaynak Oluştur** > **Web + mobil** > **Web uygulaması**.

1. Uygulamanın adını girin.

1. Aboneliği seçin.

1. Kaynak grubunu seçin veya oluşturun.

1. İşletim sisteminizi seçin. 

    * Özel bir Docker kapsayıcısı kullanarak bir Linux uygulaması oluşturmak istiyorsanız, [buradaki][linuxapp] yönergeleri izleyerek kendi kapsayıcınızı kullanıma sunabilirsiniz. 

1. Bir App Service planı seçin ya da oluşturun. Yeni bir App Service planı oluşturmak istiyorsanız, konum olarak ASE’nizi seçin. App Service planınızın oluşturulmasını istediğiniz çalışan havuzunu seçin. App Service planını oluştururken, ASE’nizi konum olarak seçin ve çalışan havuzunu belirleyin. Uygulamanın adını belirttiğinizde, uygulama adının altındaki etki alanı ASE’nizin etki alanı ile değiştirilir.

1. **Oluştur**’u seçin. Uygulamanızın Panonuzda görünmesini istiyorsanız belirleyin **panoya Sabitle** onay kutusu.

    ![App Service planı oluşturma][2]

    **Uygulama Adı** altında, etki alanı adı ASE’nizin etki alanını yansıtacak şekilde güncelleştirilir.

## <a name="post-ilb-ase-creation-validation"></a>ILB sonrası ASE oluşturmayı doğrulama ##

Bir ILB ASE, ILB olmayan ASE’den biraz farklıdır. Daha önce belirtildiği gibi, kendi DNS’inizi yönetmeniz gerekir. Ayrıca, HTTPS bağlantıları için kendi sertifikanızı sağlamanız gerekir.

ASE’yi oluşturduktan sonra etki alanı adında belirttiğiniz etki alanı gösterilir. Yeni bir öğe görünür **ayarı** adlı menüsü **ILB sertifikası**. ASE, ILB ASE etki alanını belirtmeyen bir sertifika ile oluşturulur. ASE’yi bu sertifika ile kullanırsanız, tarayıcınız sertifikanın geçersiz olduğunu söyler. Bu sertifika HTTPS’yi test etmeyi kolaylaştırır, ancak ILB ASE etki alanınıza bağlı kendi sertifikanızı karşıya yüklemeniz gerekir. Bu adım, sertifikanızın otomatik olarak imzalanmış veya bir sertifika yetkilisinden alınmış olmasına bakılmaksızın gereklidir.

![ILB ASE etki alanı adı][3]

ILB ASE’nizin geçerli bir SSL sertifikası olmalıdır. İç sertifika yetkililerini kullanın, harici bir verenden sertifika satın alın ya da otomatik olarak imzalanan bir sertifika kullanın. SSL sertifikasının kaynağından bağımsız olarak, aşağıdaki sertifika özniteliklerinin doğru şekilde yapılandırılması gerekir:

* **Konu**: Bu öznitelik *.your-root-domain-here olarak ayarlanmalıdır.
* **Konu alternatif adı**: Bu öznitelik her ikisini de içermelidir **.your-root-domain-here* ve **.Your-kök-etki-burada*. Her bir uygulamayla ilişkili SCM/Kudu sitesiyle kurulan SSL bağlantıları *your-app-name.scm.your-root-domain-here* biçiminde bir adres kullanır.

SSL sertifikasını .pfx dosyası olarak dönüştürün/kaydedin. .pfx dosyası, tüm ara ve kök sertifikaları içermelidir. Bir parola ile güvenli hale getirin.

Otomatik olarak imzalanan bir sertifika oluşturmak isterseniz, burada PowerShell komutlarını kullanabilirsiniz. *internal.contoso.com* yerine ILB ASE etki alanı adınızı kullandığınızdan emin olun: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "*.internal-contoso.com","*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Bu PowerShell komutlarının oluşturduğu sertifika, tarayıcınızın güven zincirinde olmayan bir sertifika yetkilisi tarafından oluşturulduğu için tarayıcılar tarafından işaretlenir. Tarayıcınızın güvendiği bir sertifika almak için, sertifikayı tarayıcınızın güven zincirindeki ticari sertifika yetkilisinden edinin. 

![ILB sertifikası ayarlama][4]

Kendi sertifikalarınızı yüklemek ve erişimi test etmek için:

1. ASE oluşturulduktan sonra ASE kullanıcı arabirimine gidin. **ASE** > **Ayarlar** > **ILB Sertifikası**’nı seçin.

1. ILB sertifikasını ayarlamak için sertifika .pfx dosyasını seçin ve parolayı girin. Bu adımın işlenmesi biraz sürebilir. Karşıya yükleme işleminin devam ettiğini belirten bir ileti görüntülenir.

1. ASE’nizin ILB adresini alın. **ASE** > **Özellikler** > **Sanal IP Adresi**’ni seçin.

2. ASE oluşturulduktan sonra ASE'NİZDE bir uygulama oluşturun.

3. Bu sanal ağ içinde yoksa bir VM oluşturun.

    > [!NOTE] 
    > Başarısız olacağından veya sorunlara yol açacağından, bu VM’yi ASE ile aynı sanal ağ içinde oluşturmaya çalışmayın.
    >

4. ASE etki alanınızın DNS’ini ayarlayın. DNS’inizde etki alanınızla birlikte bir joker karakter kullanabilirsiniz. Bazı basit testler yapmak için uygulama adını VIP IP adresine ayarlamak için VM üzerindeki konak dosyalarını düzenleyin:

    a. ASE'NİZİN etki alanı adı varsa _. ilbase.com_ adlı bir uygulama oluşturduğunuzda _mytestapp_, konumunda ele _mytestapp.ilbase.com_. Daha sonra ILB adresini çözümlemek için _mytestapp.ilbase.com_ adresini ayarlarsınız. (Windows hosts dosyası altındadır _C:\Windows\System32\drivers\etc\\_.)

    b. Web dağıtımı yayımlamayı veya gelişmiş konsola erişimi test etmek için bir _mytestapp.scm.ilbase.com_ kaydı oluşturun.

5. Veya bu sanal makinedeki bir tarayıcıyı kullanıp https://mytestapp.ilbase.com sayfasına gidin. (Veya uygulama adınız ile bir etki alanınız ne olursa olsun gidin.)

6. Veya bu sanal makinedeki bir tarayıcıyı kullanıp https://mytestapp.ilbase.com sayfasına gidin. Otomatik olarak imzalanan sertifika kullanıyorsanız, güvenlik eksikliğini kabul edin.

    ILB’nizin IP adresi, **IP adresleri** altında listelenir. Bu listede ayrıca dış VIP tarafından ve gelen yönetim trafiği için kullanılan IP adresleri bulunur.

    ![ILB IP adresi][5]

## <a name="web-jobs-functions-and-the-ilb-ase"></a>Web işleri, İşlevler ve ILB ASE ##

Hem İşlevler hem de web işleri ILB ASE’de desteklenir, ancak portalın bunlarla çalışabilmesi için SCM sitesine ağ erişiminiz olmalıdır.  Başka bir deyişle, tarayıcınız sanal ağ içinde veya sanal ağa bağlı bir konakta olmalıdır.  

Azure İşlevleri’ni bir ILB ASE’de kullandığınızda şöyle bir hata iletisi alabilirsiniz: "Şu anda işlevlerinizi alamıyoruz. Lütfen daha sonra tekrar deneyin." Bu hata, İşlevler kullanıcı arabirimi HTTPS üzerinden SCM sitesini kullandığından ve kök sertifika, tarayıcının güven zincirinde olmadığından oluşur. Web işleri de benzer bir sorun içerir. Bu sorunu önlemek için aşağıdakilerden birini yapabilirsiniz:

- Sertifikayı güvenilir sertifika deponuza ekleyin. Bu Microsoft Edge ve Internet Explorer engellemesini kaldırır.
- Chrome kullanın ve ilk olarak SCM sitesine gidin, güvenilmeyen sertifikayı kabul edip portala gidin.
- Tarayıcınızın güven zincirinde olan ticari bir sertifika kullanın.  En iyi seçenek budur.  

## <a name="dns-configuration"></a>DNS yapılandırması ##

Dış VIP kullandığınızda DNS, Azure tarafından yönetilir. ASE’nizde oluşturulan herhangi bir uygulama, genel bir DNS olan Azure DNS'e otomatik olarak eklenir. ILB ASE'de kendi DNS’inizi yönetmeniz gerekir. _contoso.net_ gibi belirli bir etki alanı için DNS’inizde aşağıdakiler için ILB adresini işaret eden DNS A kayıtları oluşturmanız gerekir:

- *.contoso.net
- *.scm.contoso.net

ILB ASE etki alanınız bu ASE’nin dışında birden çok amaç için kullanılıyorsa, DNS’i uygulama adı bazında yönetmeniz gerekebilir. Oluşturduğunuz her yeni uygulama adını DNS’inize eklemeniz gerektiği için bu yöntem zorludur. Bu nedenle, ayrılmış bir etki alanı kullanmanız önerilir.

## <a name="publish-with-an-ilb-ase"></a>ILB ASE ile yayımlama ##

Oluşturulan her uygulama için iki uç nokta vardır. ILB ASE'de *&lt;uygulama adı>.&lt;ILB ASE Etki Alanı>* ve *&lt;uygulama adı>.scm.&lt; ILB ASE Etki Alanı>* vardır. 

SCM site adı sizi Azure portalı içinde **Gelişmiş portal** olarak adlandırılan Kudu konsoluna götürür. Kudu konsolunu kullanarak ortam değişkenlerini görüntüleyebilir, diski keşfedebilir, bir konsolu kullanabilir ve daha fazlasını yapabilirsiniz. Daha fazla bilgi için bkz. [Azure App Service için Kudu konsolu][Kudu]. 

Çok kiracılı App Service ve bir Dış ASE’de, Azure portalı ile Kodu konsolu arasında çoklu oturum açma mevcuttur. Ancak, ILB ASE için Kudu konsolunda oturum açarken yayımlama kimlik bilgilerinizi kullanmanız gerekir.

GitHub ve Azure DevOps gibi İnternet tabanlı CI sistemleri, derleme aracısına İnternet’ten erişilebiliyorsa ve aracı ILB ASE ile aynı ağdaysa ILB ASE ile çalışmaya devam eder. Bu nedenle, Azure DevOps örneğinde derleme aracısı ILB ASE ile aynı sanal ağda (alt ağın farklı olması sorun yaratmaz) oluşturulursa Azure DevOps git’ten kod çekip ILB ASE’ye dağıtabilir. Kendi derleme aracınızı oluşturmak istemiyorsanız çekme modeli kullanan bir CI sistemi (Dropbox gibi) kullanmanız gerekir.

Bir ILB ASE’deki uygulamalar için yayımlama uç noktaları, ILB ASE oluşturulurken kullanılan etki alanını kullanır. Bu etki alanı uygulamanın yayımlama profilinde ve uygulamanın portal dikey penceresinde görünür (**Genel Bakış** > **Temel Bilgiler** ve ayrıca **Özellikler**). Alt etki alanı *contoso.net* olan bir ILB ASE’niz ve *mytest* adlı bir uygulamanız varsa, FTP için *mytest.contoso.net* ve web dağıtımı için *mytest.scm.contoso.net* kullanın.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Bir WAF cihazı ile ILB ASE’yi eşleştirme ##

Azure App Service, sistemi koruyan çok sayıda güvenlik önlemi sağlar. Ayrıca bir uygulamanın ele geçirilip geçirilmediğini belirlemeye yardımcı olur. Bir web uygulaması için en iyi koruma, Azure App Service gibi bir barındırma platformunu bir web uygulaması güvenlik duvarı (WAF) ile eşleştirmektir. ILB ASE ağdan yalıtılmış bir uygulama uç noktası olduğundan, bu tür bir kullanıma uygundur.

ILB ASE’nizi bir WAF cihazıyla yapılandırma hakkında daha fazla bilgi için bkz. [Bir web uygulaması güvenlik duvarını App Service ortamınızla yapılandırma][ASEWAF]. Bu makalede, bir Barracuda sanal gerecinin ASE’nizle nasıl kullanılacağı gösterilir. Bir diğer seçenek ise Azure Application Gateway’in kullanılmasıdır. Application Gateway, arkasına yerleştirilmiş uygulamaların güvenliğini sağlamak için OWASP temel kurallarını kullanır. Application Gateway hakkında daha fazla bilgi için bkz. [Azure web uygulaması güvenlik duvarına giriş][AppGW].

## <a name="get-started"></a>başlarken ##

* ASE’leri kullanmaya başlamak için bkz. [App Service ortamlarına giriş][Intro].
 

<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/security-overview.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
[customdomain]: ../app-service-web-tutorial-custom-domain.md
[linuxapp]: ../containers/app-service-linux-intro.md
