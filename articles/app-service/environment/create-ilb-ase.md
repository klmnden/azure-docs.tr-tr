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
ms.date: 05/28/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 5b05755502ad5836a21080a122d2e1721825f10c
ms.sourcegitcommit: 4cdd4b65ddbd3261967cdcd6bc4adf46b4b49b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66734680"
---
# <a name="create-and-use-an-internal-load-balancer-app-service-environment"></a>Oluşturma ve bir iç yük dengeleyici App Service ortamını kullanma 

Azure App Service ortamı, Azure App Service'in bir Azure sanal ağı (VNet) bir alt ağa dağıtımıdır. Bir App Service Ortamı (ASE) iki şekilde dağıtılabilir: 

- Genellikle Dış ASE olarak adlandırılan durumda, bir dış IP adresi üzerindeki VIP ile.
- Genellikle iç uç nokta bir iç yük dengeleyici (ILB) olduğu için ILB ASE olarak adlandırılan durumda, bir iç IP adresi üzerindeki VIP ile. 

Bu makale bir ILB ASE oluşturma işlemini gösterir. ASE’ye genel bakış için bkz. [App Service Ortamlarına giriş][Intro]. Dış ASE oluşturma hakkında bilgi almak için bkz. [Dış ASE Oluşturma][MakeExternalASE].

## <a name="overview"></a>Genel Bakış 

Bir ASE’yi İnternet’ten erişilebilen bir uç nokta ile ya da sanal ağınızdaki bir IP adresi ile dağıtabilirsiniz. IP adresini bir sanal ağ adresine ayarlamak için, ASE’nin ILB ile dağıtılması gerekir. ASE'nizi bir ILB ile dağıttığınızda, ASE'nizi adını sağlamanız gerekir. Adı ASE'NİZİN etki alanı soneki, ase'deki uygulamalar için kullanılır.  ILB ASE'NİZİN etki alanı soneki olan &lt;ASE adı&gt;. appservicewebsites.net. ILB ASE'de oluşturulan uygulamaların ortak DNS'de konmaz. 

ILB ASE önceki sürümlerinde, HTTPS bağlantıları için bir etki alanı soneki ve varsayılan bir sertifika sağlamak gereklidir. Etki alanı soneki artık ILB ASE oluşturma sırasında toplanan ve bir varsayılan sertifika ayrıca artık toplanır. ILB ASE artık oluşturduğunuzda, varsayılan sertifika Microsoft tarafından sağlanır ve tarayıcı tarafından güvenilir. Özel etki alanı adları, ase'deki uygulamalar ayarlamak ve bu özel etki alanı adlarına sertifikaları ayarlayın. 

Bir ILB ASE ile gibi şeyler yapabilirsiniz:

-   Konak intranet uygulamalarını bulutta bir siteden siteye veya ExpressRoute erişebileceğiniz güvenli bir şekilde.
-   Bir WAF cihazı ile uygulamaları koruma
-   Genel DNS sunucularında listelenmeyen uygulamaları bulutta barındırma.
-   Ön uç uygulamalarınızın güvenli bir şekilde tümleştirilebileceği, İnternet’ten yalıtılmış arka uç uygulamaları oluşturma.

### <a name="disabled-functionality"></a>Devre dışı bırakılan işlevler ###

ILB ASE’yi kullanırken bazı işlemleri yapamazsınız:

-   IP tabanlı SSL kullanma.
-   Belirli uygulamalara IP adresleri atama.
-   Azure portalı üzerinden bir uygulama ile sertifika satın alma ve kullanma. Sertifikaları doğrudan bir sertifika yetkilisinden alabilir ve uygulamalarınızla kullanabilirsiniz. Sertifikaları Azure portalı üzerinden alamazsınız.

## <a name="create-an-ilb-ase"></a>ILB ASE oluşturma ##

ILB ASE oluşturmak için:

1. Azure portalda **Kaynak oluştur** > **Web** > **App Service Ortamı**’nı seçin.

2. Aboneliğinizi seçin.

3. Kaynak grubunu seçin veya oluşturun.

4. App Service ortamınızın adını girin.

5. İç sanal IP türünü seçin.

    ![ASE oluşturma](media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase.png)

6. Ağ seçin

7. Seçin veya bir sanal ağ oluşturun. Burada yeni bir VNet oluşturun, 192.168.250.0/23 adres aralığına ile tanımlanır. ASE değerinden farklı bir kaynak grubunda veya farklı adres aralığı ile bir VNet oluşturmak için Azure sanal ağ oluşturma portalı kullanın. 

8. Boş bir alt ağ oluşturun veya seçin. Bir alt ağ seçmek istiyorsanız, boş ve temsilcisi yok olmalıdır. Alt ağ boyutunu ASE oluşturulduktan sonra değiştirilemez. 256 adres içeren ve en büyük boyutlu ASE’yi işleyebilen ve ölçeklendirme ihtiyaçlarını karşılayabilen `/24` dosya boyutu önerilir. 

    ![ASE ağ][1]

7. Seçin **gözden geçir ve Oluştur** seçip **Oluştur**.

## <a name="create-an-app-in-an-ilb-ase"></a>ILB ASE'de uygulama oluşturma ##

ILB ASE'de uygulama oluşturma işlemi, normalde bir ASE’de uygulama oluşturma işlemiyle aynıdır.

1. Azure portalında **kaynak Oluştur** > **Web** > **Web uygulaması**.

1. Uygulamanın adını girin.

1. Aboneliği seçin.

1. Kaynak grubunu seçin veya oluşturun.

1. Yayımlama, çalışma zamanı yığını ve işletim sistemi seçin.

1. Konum var olan bir ILB ASE olduğu bir konum seçin.  Ayrıca, yeni bir ASE yalıtılmış App Service planı seçerek uygulama oluşturma sırasında oluşturabilirsiniz. Yeni bir ASE oluşturmak istiyorsanız, ASE oluşturulması için istediğiniz bölgeyi seçin.

1. Bir App Service planı seçin ya da oluşturun. 

1. Seçin **gözden geçir ve Oluştur** seçip **Oluştur** hazır olduğunuzda.

### <a name="web-jobs-functions-and-the-ilb-ase"></a>Web işleri, İşlevler ve ILB ASE 

Hem İşlevler hem de web işleri ILB ASE’de desteklenir, ancak portalın bunlarla çalışabilmesi için SCM sitesine ağ erişiminiz olmalıdır.  Başka bir deyişle, tarayıcınız sanal ağ içinde veya sanal ağa bağlı bir konakta olmalıdır. ILB ASE'nizi bitmiyor bir etki alanı adı varsa *appserviceenvironment.net*, tarayıcınızın scm siteniz tarafından kullanılan HTTPS sertifikaya güvenmesi gerekir.

## <a name="dns-configuration"></a>DNS yapılandırması 

Dış VIP kullandığınızda DNS, Azure tarafından yönetilir. ASE’nizde oluşturulan herhangi bir uygulama, genel bir DNS olan Azure DNS'e otomatik olarak eklenir. ILB ASE'de kendi DNS’inizi yönetmeniz gerekir. Bir ILB ASE ile kullanılan etki alanı soneki ASE adına bağlıdır. Etki alanı sonek  *&lt;ASE adı&gt;. appserviceenvironment.net*. Ilb'nizin IP adresi portalında altında olan **IP adresleri**. 

DNS yapılandırmak için:

- için bir bölge oluşturmanız  *&lt;ASE adı&gt;. appserviceenvironment.net*
- işaret eden bu bölgede bir A kaydı oluşturma * ILB IP adresi 
- bir bölgede oluşturma  *&lt;ASE adı&gt;. scm.appserviceenvironment.net* scm adlı
- ILB IP adresine işaret eden scm dilimindeki bir A kaydı oluşturma

## <a name="publish-with-an-ilb-ase"></a>ILB ASE ile yayımlama

Oluşturulan her uygulama için iki uç nokta vardır. ILB ASE'de, sahip olduğunuz *&lt;uygulama adı&gt;.&lt; ILB ASE etki alanı&gt;* ve  *&lt;uygulama adı&gt;.scm.&lt; ILB ASE etki alanı&gt;* . 

SCM site adı sizi Azure portalı içinde **Gelişmiş portal** olarak adlandırılan Kudu konsoluna götürür. Kudu konsolunu kullanarak ortam değişkenlerini görüntüleyebilir, diski keşfedebilir, bir konsolu kullanabilir ve daha fazlasını yapabilirsiniz. Daha fazla bilgi için bkz. [Azure App Service için Kudu konsolu][Kudu]. 

GitHub ve Azure DevOps gibi İnternet tabanlı CI sistemleri, derleme aracısına İnternet’ten erişilebiliyorsa ve aracı ILB ASE ile aynı ağdaysa ILB ASE ile çalışmaya devam eder. Bu nedenle, Azure DevOps örneğinde derleme aracısı ILB ASE ile aynı sanal ağda (alt ağın farklı olması sorun yaratmaz) oluşturulursa Azure DevOps git’ten kod çekip ILB ASE’ye dağıtabilir. Kendi derleme aracınızı oluşturmak istemiyorsanız çekme modeli kullanan bir CI sistemi (Dropbox gibi) kullanmanız gerekir.

Bir ILB ASE’deki uygulamalar için yayımlama uç noktaları, ILB ASE oluşturulurken kullanılan etki alanını kullanır. Bu etki alanı uygulamanın yayımlama profilinde ve uygulamanın portal dikey penceresinde görünür (**Genel Bakış** > **Temel Bilgiler** ve ayrıca **Özellikler**). Bir ILB ASE etki alanı soneki ile varsa  *&lt;ASE adı&gt;. appserviceenvironment.net*ve adlı bir uygulama *mytest*, kullanın *mytest.&lt; ASE adı&gt;. appserviceenvironment.net* FTP ve *mytest.scm.contoso.net* web dağıtımı için.

## <a name="configure-an-ilb-ase-with-a-waf-device"></a>Bir WAF cihazı ile ILB ASE yapılandırma ##

Bir web uygulaması Güvenlik Duvarı (WAF) cihaz, yalnızca internet'e istediğiniz ve rest yalnızca erişilebilir bir vnet'teki tutmak uygulamaları göstermek için ILB ASE ile birleştirebilirsiniz. Bu, başka şeylerin yanında, güvenli çok katmanlı uygulamalar oluşturmanıza olanak sağlar.

ILB ASE'nizi bir WAF cihazıyla yapılandırma hakkında daha fazla bilgi için bkz. [, App Service ortamı ile bir web uygulaması güvenlik duvarı yapılandırma][ASEWAF]. Bu makalede, bir Barracuda sanal gerecinin ASE’nizle nasıl kullanılacağı gösterilir. Bir diğer seçenek ise Azure Application Gateway’in kullanılmasıdır. Application Gateway, arkasına yerleştirilmiş uygulamaların güvenliğini sağlamak için OWASP temel kurallarını kullanır. Application Gateway hakkında daha fazla bilgi için bkz. [Azure web uygulaması güvenlik duvarına giriş][AppGW].

## <a name="ilb-ases-made-before-may-2019"></a>ILB ase Mayıs 2019 önce yapılan

Mayıs 2019 ASE oluşturma sırasında etki alanı soneki ayarlamanızı gerekli önce yapılmış ILB ase. Bunlar ayrıca, etki alanı son ekini temel alarak bir varsayılan sertifikayı karşıya yüklemek gereklidir. Ayrıca, daha eski bir ILB ASE ile çoklu oturum açma uygulamalarla Kudu konsoluna bu ILB ASE'de gerçekleştiremezsiniz. DNS daha eski bir ILB ASE için yapılandırırken, etki alanı sonekiyle başka bir bölgede joker A kaydını ayarlamanız gerekir. 

## <a name="get-started"></a>başlarken ##

* ASE’leri kullanmaya başlamak için bkz. [App Service ortamlarına giriş][Intro]. 

<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
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
