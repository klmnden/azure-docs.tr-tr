---
title: "Oluşturma ve bir iç yük dengeleyici ile Azure uygulama hizmeti ortamı kullanma"
description: "Internet yalıtılmış Azure uygulama hizmeti ortamı oluşturulacağı ve kullanılacağı hakkında ayrıntılar"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: cc7bdd7860506c20187dc913b72111824d1737ca
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a>Oluşturma ve bir iç yük dengeleyici ile uygulama hizmeti ortamı kullanma #

 Azure uygulama hizmeti ortamı bir Azure uygulama hizmeti dağıtımı bir Azure sanal ağındaki (VNet) bir alt ağ ile değil. Uygulama hizmeti ortamı (ana) dağıtmak için iki yol vardır: 

- Dış IP adresi üzerinde bir VIP ile genellikle bir dış ana çağrılır.
- Bir iç yük dengeleyici (ILB) iç uç nokta olduğu için bir iç IP adresinde bir VIP ile genellikle bir ILB ana olarak adlandırılır. 

Bu makalede bir ILB ana oluşturulacağını gösterir. Ana üzerinde bir genel bakış için bkz: [uygulama hizmeti ortamları giriş][Intro]. Bir dış ana oluşturmayı öğrenmek için bkz: [bir dış ana oluşturma][MakeExternalASE].

## <a name="overview"></a>Genel Bakış ##

Bir ana internet'ten erişilebilen bir uç nokta veya bir IP adresi ile sanal ağınızda dağıtabilirsiniz. IP adresi için bir sanal ağ adresi ayarlamak için ana bir ILB ile dağıtılması gerekir. Bir ILB ile ana dağıttığınızda, sağlamanız gerekir:

-   Uygulamalarınızı oluştururken kullandığınız kendi etki alanı.
-   HTTPS için kullanılan sertifika.
-   Etki alanınız için DNS yönetimi.

Buna karşılık, sizin gibi şeyler yapabilir:

-   İntranet uygulamalarını bulutta bir siteden siteye veya Azure ExpressRoute VPN aracılığıyla erişebileceğiniz güvenli bir şekilde, ana bilgisayar.
-   Genel DNS sunucuları listelenmeyen ana bilgisayar uygulamalarını bulutta.
-   Ön uç uygulamalarınızı güvenli bir şekilde ile tümleştirebilirsiniz Internet yalıtılmış arka uç uygulamalar oluşturun.

### <a name="disabled-functionality"></a>Devre dışı bırakılan işlevsellik ###

Bir ILB ana kullandığınızda, bunu yapamazsınız bazı şeyler vardır:

-   IP tabanlı SSL kullanın.
-   Belirli uygulamalar için IP adresleri atayın.
-   Satın alma ve Azure portalı üzerinden bir uygulama ile bir sertifika kullanın. Doğrudan bir sertifika yetkilisinden sertifika almak ve bunları uygulamalarınızı ile kullanabilirsiniz. Bunları Azure Portalı aracılığıyla elde edilemiyor.

## <a name="create-an-ilb-ase"></a>Bir ILB ana oluşturma ##

Bir ILB ana oluşturmak için:

1. Azure portalında seçin **yeni** > **Web + mobil** > **uygulama hizmeti ortamı**.

2. Aboneliğinizi seçin.

3. Kaynak grubunu seçin veya oluşturun.

4. Bir VNet oluşturun veya seçin.

5. Varolan bir sanal ağ seçerseniz, ana tutmak için bir alt ağı oluşturmanız gerekir. Bir alt ağ boyutu, ana, gelecekteki büyümesine uyum sağlayacak şekilde büyük ayarladığınızdan emin olun. Dosya boyutu öneririz `/25`, 128 adresi olduğunu ve en büyük ölçekli bir ana işleyebilir. Seçebileceğiniz minimum boyutu bir `/28`. Altyapı gereken sonra bu boyutu en fazla 11 örnekleri için ölçeklendirilebilir.

    * Uygulama hizmeti planlarında varsayılan en fazla 100 örneklerinin gidin.

    * 100 yakın ancak daha hızlı ön uç ölçeklendirme ile ölçeklendirin.

6. Seçin **sanal ağ konumu** > **sanal ağ yapılandırması**. Ayarlama **VIP türü** için **iç**.

7. Bir etki alanı adı girin. Bu etki alanı bu ana içinde oluşturulan uygulamaları için kullanılan adrestir. Bazı kısıtlamalar vardır. Olamaz:

    * NET   

    * azurewebsites.NET

    * p.azurewebsites.NET

    * &lt;asename&gt;. p.azurewebsites.net

   Uygulamalar için kullanılan özel etki alanı adı ve, ana tarafından kullanılan etki alanı adı ile örtüşemez. Bir ILB ana etki alanı adıyla için _contoso.com_, özel etki alanı adları gibi uygulamalar için kullanılamaz:

    * www.contoso.com

    * ABCD.def.contoso.com

    * ABCD.contoso.com

   Uygulamalarınız için özel etki alanı adlarını biliyorsanız, bu özel etki alanı adları ile bir çakışma olmaz ILB ana için bir etki alanı seçin. Bu örnekte, aşağıdakine benzer kullanabilirsiniz *contoso internal.com* , ana etki alanı için sonunda özel etki alanı adları ile çakışmaz çünkü *. contoso.com*.

8. Seçin **Tamam**ve ardından **oluşturma**.

    ![ASE oluşturma][1]

Üzerinde **sanal ağ** dikey penceresinde, var olan bir **sanal ağ yapılandırması** seçeneği. Dış bir VIP veya bir iç VIP seçmek için kullanabilirsiniz. Varsayılan değer **dış**. Seçerseniz **dış**, ana internet'ten erişilebilen bir VIP kullanır. Seçerseniz **iç**, ana sanal ağınızın içinde bir IP adresinde bir ILB ile yapılandırılır.

Siz seçtikten sonra **iç**, daha fazla IP adresi, ana ekleme yeteneği kaldırılır. Bunun yerine, ana etki alanı sağlamanız gerekir. ASE'de bir dış VIP ile ana adını etki alanında o ana oluşturulan uygulamalar için kullanılır.

Ayarlarsanız **VIP türü** için **iç**, ana adınızı etki alanında ana için kullanılmaz. Etki alanını açıkça belirtin. Etki alanınız varsa *contoso.corp.net* ve ana adlı içeren bir uygulama oluşturmak *timereporting*, bu uygulama için timereporting.contoso.corp.net bir URL'dir.


## <a name="create-an-app-in-an-ilb-ase"></a>ILB ASE'de bir uygulama oluşturun ##

Oluşturduğunuz uygulama ASE'de normalde aynı şekilde ILB ASE'de bir uygulama oluşturun.

1. Azure portalında seçin **yeni** > **Web + mobil** > **Web** veya **mobil** veya **API uygulaması**.

2. Uygulama adını girin.

3. Aboneliği seçin.

4. Kaynak grubunu seçin veya oluşturun.

5. Bir uygulama hizmeti planı oluşturun veya seçin. Yeni bir uygulama hizmeti planı oluşturmak istiyorsanız, ana konum olarak seçin. Uygulama hizmeti planınızı oluşturulacak istediğiniz çalışan havuzu seçin. Uygulama hizmeti planı oluşturduğunuzda, ana konum ve çalışan havuzunda seçin. Uygulama adı belirttiğinizde, uygulama adı altında etki alanı için ana etki alanı tarafından değiştirilir.

6. **Oluştur**’u seçin. Uygulama Panonuzda görünmesini istiyorsanız seçin **panoya Sabitle** onay kutusu.

    ![Uygulama hizmeti planı oluşturma][2]

    Altında **uygulama adı**, etki alanı adı, ana etki alanı yansıtacak şekilde güncelleştirilir.

## <a name="post-ilb-ase-creation-validation"></a>POST ILB ana oluşturma doğrulama ##

Bir ILB ana olmayan - ILB ana değerinden biraz farklıdır. Önceden belirtildiği gibi kendi DNS yönetmeniz gereken. Ayrıca, HTTPS bağlantıları için kendi sertifikanızı sağlamanız gerekir.

Etki alanı adı, ana oluşturduktan sonra belirtilen etki alanı gösterir. Yeni bir öğe görünür **ayarı** adlı menüsü **ILB sertifika**. Ana ILB ana etki alanı belirtmeyen bir sertifika ile oluşturulur. Bu sertifika ile ana kullanıyorsanız, tarayıcınızı geçersiz olduğunu söyler. Bu sertifika, HTTPS test kolaylaştırır, ancak ILB ana etki alanınıza bağlı kendi sertifikayı karşıya yüklemek gerekir. Bu adım sertifikanız veya otomatik olarak imzalanan bir sertifika yetkilisinden alınan bağımsız olarak gereklidir.

![ILB ana etki alanı adı][3]

ILB ana geçerli bir SSL sertifikası gerekir. İç sertifika yetkilileri kullanın, bir dış veren bir sertifika satın veya otomatik olarak imzalanan bir sertifika kullanın. SSL sertifikası kaynak bağımsız olarak, aşağıdaki sertifika öznitelikleri doğru şekilde yapılandırılmış olması gerekir:

* **Konu**: *.your-kök-etki-Buraya bu özniteliği ayarlanmalıdır.
* **Konu alternatif adı**: Bu öznitelik her ikisini de içermelidir **kök etki alanı burada .your* ve **.scm.your-kök-etki-burada*. SSL bağlantıları her uygulamayla ilişkili SCM/Kudu siteye kullanma biçiminde bir adresi *your-app-name.scm.your-root-domain-here*.

Convert/SSL sertifikası bir .pfx dosyası olarak Kaydet. .Pfx dosyası, tüm ara içerir ve sertifikaları kök gerekir. Bir parola ile güvenli hale getirin.

Kendinden imzalı bir sertifika oluşturmak istiyorsanız, burada PowerShell komutlarını kullanabilirsiniz. ILB ana etki alanı adınızı yerine kullandığınızdan emin olun *internal.contoso.com*: 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

Sertifika, tarayıcınızın güven zincirindeki bir sertifika yetkilisi tarafından oluşturulmadıysa çünkü bu PowerShell komutlarını oluşturan sertifika tarayıcılar tarafından işaretlenir. Tarayıcınız güvendiği bir sertifika almak için tarayıcınızın zincirinde güven, ticari sertifika yetkilisinden edinin. 

![ILB sertifikayı ayarlayın][4]

Kendi sertifikalarını karşıya yükleme ve erişim test etmek için:

1. Ana oluşturulduktan sonra ana UI gidin. Seçin **ana** > **ayarları** > **ILB sertifika**.

2. ILB sertifika ayarlamak için sertifika .pfx dosyasını seçin ve parolayı girin. Bu adım işlemek için biraz zaman alır. Bir karşıya yükleme işlemi devam ediyor belirten bir ileti görüntülenir.

3. ILB adresi için ana alırsınız. Seçin **ana** > **özellikleri** > **sanal IP adresi**.

4. Ana oluşturulduktan sonra ana bir web uygulaması oluşturun.

5. Bu VNet içinde yoksa, bir VM oluşturun.

    > [!NOTE] 
    > Başarısız veya sorunlara neden olduğundan bu VM ana aynı alt ağda oluşturmak denemeyin.
    >
    >

6. DNS ana etki alanınız için ayarlayın. DNS, etki alanı ile bir joker karakter kullanabilirsiniz. Bazı basit testler yapmak için web uygulaması adı VIP IP adresini ayarlamak için VM üzerinde hosts dosyasını düzenleyin:

    a. ANA etki alanı adı varsa _. ilbase.com_ ve adlı web uygulaması oluşturma _mytestapp_, adresindeki ele _mytestapp.ilbase.com_. Ardından _mytestapp.ilbase.com_ ILB adresine çözümlenemedi. (Windows, _C:\Windows\System32\drivers\etc hosts dosyasıdır\_.)

    b. Web dağıtımı yayımlama veya Gelişmiş konsoluna erişimi test etmek için bir kayıt oluşturmak _mytestapp.scm.ilbase.com_.

7. Bu VM üzerinde bir tarayıcı kullanın ve http://mytestapp.ilbase.com için gidin. (Veya web uygulaması adı, etki alanı ile ne olursa olsun gidin.)

8. Bu VM üzerinde bir tarayıcı kullanın ve https://mytestapp.ilbase.com için gidin. Kendinden imzalı bir sertifika kullanıyorsanız, güvenlik eksikliği kabul edin.

    ILB için IP adresi altında listelenen **IP adreslerini**. Bu liste, gelen yönetim trafiği için dış VIP tarafından kullanılan IP adresleri de vardır.

    ![ILB IP adresi][5]

## <a name="web-jobs-functions-and-the-ilb-ase"></a>Web işleri, İşlevler ve ILB ana ##

Bir ILB ana işlevleri ve web işleri desteklenmektedir, ancak bunlarla çalışmak portal için SCM siteye ağ erişimi olması gerekir.  Başka bir deyişle, tarayıcınızı ya da kullanılıyor veya sanal ağa bağlı bir konak olması gerekir.  

Bir ILB ana Azure işlevleri kullandığınızda, "işlevlerinizi hemen almak yapamıyoruz. bildiren bir hata iletisi alabilirsiniz Lütfen daha sonra yeniden deneyin." İşlevler UI SCM sitenin HTTPS üzerinden yararlanır ve kök sertifika güven tarayıcı zincirindeki olmadığından bu hata oluşur. Web işleri benzer bir sorun vardır. Bu sorunu önlemek için aşağıdakilerden birini yapabilirsiniz:

- Sertifika, güvenilen sertifika deponuza ekleyin. Bu sınır ve Internet Explorer kaldırır.
- Chrome kullanın ve SCM siteye ilk gidin, güvenilmeyen sertifikayı kabul ve Portalı'na gidin.
- Güven, tarayıcı zincirindeki bir ticari sertifikası kullanın.  En iyi seçenek budur.  

## <a name="dns-configuration"></a>DNS yapılandırması ##

Bir dış VIP kullandığınızda, DNS Azure tarafından yönetilir. Ana oluşturulan herhangi bir uygulama, Genel DNS olduğu Azure DNS'ye otomatik olarak eklenir. ILB ASE'de kendi DNS yönetmeniz gerekir. Gibi belirli bir etki alanının _contoso.net_, ILB adresinizi üzerine gelin, DNS'de DNS A kayıtları oluşturmanız gerekir:

- *. contoso.net
- *. scm.contoso.net

Bu ana dışında birden çok şey için ILB ana etki alanı kullandıysanız, tek başına uygulamanızın-adı temelinde DNS yönetmek gerekebilir. Oluşturduğunuzda her yeni uygulama adı, DNS eklemeniz gerekir çünkü bu yöntem bir görevdir. Bu nedenle, ayrılmış bir etki alanı kullanmanızı öneririz.

## <a name="publish-with-an-ilb-ase"></a>Bir ILB ana ile yayımlama ##

Oluşturulan her uygulama için iki uç nokta vardır. ILB ASE'de elinizde  *&lt;uygulama adı >.&lt; ILB ana etki alanı >* ve  *&lt;uygulama adı > .scm.&lt; ILB ana etki alanı >*. 

SCM site adı olarak adlandırılan Kudu konsola alır **Gelişmiş portal**, Azure portalı içinde. Kudu konsol, ortam değişkenleri görüntülemek, disk keşfetmek, bir konsol ve çok daha fazlasını kullanın sağlar. Daha fazla bilgi için bkz: [Kudu Konsolu Azure App Service için][Kudu]. 

Uygulama hizmeti çok kullanıcılı ve bir dış ana yoktur çoklu oturum açma Azure portalı ve Kudu Konsolu arasında. ILB ana için Bununla birlikte, yayımlama kimlik bilgilerinizi Kudu konsola imzalamak için kullanmanız gerekebilir.

Yayımlama uç noktası Internet erişilebilir olmadığı için Internet tabanlı CI sistemler, GitHub ve Visual Studio Team Services gibi bir ILB ana ile çalışmaz. Bunun yerine, Dropbox gibi bir çekme modeli kullanan bir CI sistemi kullanmanız gerekir.

Bir ILB ana uygulamalar için yayımlama uç noktaları ILB ana oluşturulması sırasında etki alanı kullanın. Bu etki alanı uygulamanın yayımlama profili ve uygulamanızın portal dikey penceresinde görünür (**genel bakış** > **Essentials** ve ayrıca **özellikleri**). Alt etki alanı ile bir ILB ana varsa *contoso.net* ve adlı bir uygulama *mytest*, kullanın *mytest.contoso.net* FTP ve *mytest.scm.contoso.net* web dağıtımı için.

## <a name="couple-an-ilb-ase-with-a-waf-device"></a>Bir ILB ana WAF aygıt ile eşleştiği ##

Azure uygulama hizmeti sistemi korumak birçok güvenlik önlemleri sağlar. Ayrıca bir uygulama ele olup olmadığını belirlemek için yardımcı olurlar. En iyi koruma bir web uygulaması için bir web uygulaması güvenlik duvarı ile (WAF) Azure uygulama hizmeti gibi bir ana bilgisayar platformu eşleştiği sağlamaktır. ILB ana ağ yalıtılmış uygulama uç noktası olduğundan, bu tür bir kullanım için uygundur.

ILB ana WAF aygıtla yapılandırma hakkında daha fazla bilgi için bkz: [, uygulama hizmeti ortamınızı ile bir web uygulaması güvenlik duvarı yapılandırma][ASEWAF]. Bu makalede, ana ile Barracuda sanal gereç kullanmayı gösterir. Azure uygulama ağ geçidini kullanan başka bir seçenektir. Uygulama ağ geçidi OWASP çekirdek kuralları arkasında yerleştirilen herhangi bir uygulama güvenliğini sağlamak için kullanır. Uygulama ağ geçidi hakkında daha fazla bilgi için bkz: [Azure web uygulaması güvenlik duvarı giriş][AppGW].

## <a name="get-started"></a>başlarken ##

* ASEs ile çalışmaya başlamak için bkz: [uygulama hizmeti ortamları giriş][Intro].
 
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
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: app-service-app-service-environment-intro.md
[webapps]: ../app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
