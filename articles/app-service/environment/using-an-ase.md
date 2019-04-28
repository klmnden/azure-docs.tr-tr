---
title: App Service ortamı - Azure'ı kullanın
description: Nasıl oluşturmasına, yayımlamasına ve bir Azure App Service ortamında uygulamaları ölçeklendirme
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: a22450c4-9b8b-41d4-9568-c4646f4cf66b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: d536e9d14edfa17e890480c07951eccb70e9eb9a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61228367"
---
# <a name="use-an-app-service-environment"></a>Bir App Service ortamını kullanma #

## <a name="overview"></a>Genel Bakış ##

Azure App Service ortamı, Azure App Service'in bir müşterinin Azure sanal ağdaki bir alt ağa dağıtımıdır. Şunlardan oluşur:

- **Ön uçlar**: Ön uçlar HTTP/HTTPS bir App Service ortamında (ASE) burada sonlandıran ' dir.
- **Çalışanları**: Çalışanlar, uygulamalarınızı barındırmak kaynaklardır.
- **Veritabanı**: Veritabanı ortamı tanımlayan bilgileri tutar.
- **Depolama**: Depolama, müşteri tarafından yayımlanan uygulamaları barındırmak için kullanılır.

> [!NOTE]
> App Service ortamının iki sürümü vardır: ASEv1 ve ASEv2. ASEv1'de kullanabilmeniz için önce kaynakları yönetmeniz gerekir. Yapılandırma ve ASEv1 yönetme konusunda bilgi almak için bkz: [bir App Service ortamı v1 yapılandırma][ConfigureASEv1]. Bu makalenin geri kalanında ASEv2 üzerinde odaklanır.
>
>

Uygulama erişimi için bir dış veya iç VIP ile ASE (ASEv1 ve ASEv2) dağıtabilirsiniz. Dış VIP dağıtım genellikle dış ASE olarak adlandırılır. İç yük dengeleyici (ILB) kullandığından, dahili sürüm ILB ASE çağrılır. ILB ASE hakkında daha fazla bilgi için bkz: [oluşturma ve kullanma ILB ASE][MakeILBASE].

## <a name="create-an-app-in-an-ase"></a>Bir ASE'de uygulama oluşturma ##

Bir ASE'de uygulama oluşturma için aynı süreci normalde oluşturduğunuzda, ancak birkaç küçük farklılıkla kullanın. Yeni bir App Service planı oluşturduğunuzda:

- Uygulamanızı dağıtmak coğrafi bir konum seçmek yerine bir ASE konumunuzu seçin.
- Bir ASE içinde oluşturulan tüm App Service planları bir yalıtılmış fiyatlandırma katmanında olmanız gerekir.

Bir ASE yoksa, yönergeleri izleyerek bir tane oluşturabilirsiniz [bir App Service ortamı oluşturma][MakeExternalASE].

Bir ASE'de uygulama oluşturma için:

1. Seçin **kaynak Oluştur** > **Web + mobil** > **Web uygulaması**.

2. Uygulama için bir ad girin. Bir ASE'de App Service planı zaten seçili değilse, uygulama etki alanı adını ASE'nin etki alanı adı yansıtır.

    ![Uygulama adı seçimi][1]

1. Bir abonelik seçin.

1. Yeni bir kaynak grubu için bir ad girin veya seçin **var olanı kullan** ve aşağı açılan listeden birini seçin.

1. İşletim sisteminizi seçin. 

    * Linux uygulamaları şu anda üretim iş yükleri çalıştıran ASE eklemeyin öneririz ASE bir Linux uygulaması barındıran yeni bir önizleme özelliği olduğundan. 
    * Bir ASE ile bir Linux uygulaması ekleme ASE önizleme modunda da olacağı anlamına gelir. 

1. ASE'NİZDE var olan bir App Service planı seçin veya aşağıdaki adımları izleyerek yeni bir tane oluşturun:

    a. Seçin **Yeni Oluştur**.

    b. App Service planınız için adı girin.

    c. ASE'NİZDE seçin **konumu** aşağı açılan listesi. Bir ASE bir Linux uygulaması barındırma yalnızca 6 bölgede şu anda etkindir: **Batı ABD, Doğu ABD, Batı Avrupa, Kuzey Avrupa, Doğu Avustralya, Güneydoğu Asya.** 

    d. Seçin bir **yalıtılmış** fiyatlandırma katmanı. Seçin **seçin**.

    e. **Tamam**’ı seçin.
    
    ![Yalıtılmış fiyatlandırma katmanları][2]

    > [!NOTE]
    > Linux uygulamaları ve Windows uygulamaları aynı App Service planında olamaz, ancak aynı App Service Ortamı'nda olabilir. 
    >

2. **Oluştur**’u seçin.

## <a name="how-scale-works"></a>Nasıl çalıştığını ölçeklendirme ##

Her App Service uygulaması, App Service planı içinde çalışır. Kapsayıcı ortamları App Service planları tutun ve uygulamaları App Service planları barındıracak modelidir. Bir uygulamayı ölçeklendirme, ölçeği App Service planı ve bu nedenle tüm uygulamaları aynı plana ölçeklendirin.

Bir App Service planı ölçeklediğinizde ASEv2 gerekli altyapı otomatik olarak eklenir. Altyapı eklenirken ölçeklendirme işlemleri için gecikme süresini yoktur. ASEv1'de oluşturabilir veya App Service planınızın ölçeğini önce gerekli altyapı eklenmesi gerekir. 

Çok kiracılı App Service, ölçeklendirme, bir kaynak havuzu desteklemek hazır olduğundan genellikle hemen uygulanır. Bir ASE böyle bir arabellek yoktur ve kaynakları üzerinde gerek ayrılır.

Bir ASE'de, 100 örneğe kadar ölçeklendirebilirsiniz. Bu 100 örneğe veya birden fazla App Service planları arasında dağıtılmış tüm tek tek App Service planında olabilir.

## <a name="ip-addresses"></a>IP adresleri ##

App Service, uygulama için ayrılmış bir IP adresi ayırmayı özelliğine sahiptir. Bir IP tabanlı SSL yapılandırdıktan sonra bu özellik anlatıldığı gibi kullanılabilir [mevcut bir özel SSL sertifikasını Azure App Service'e bağlama][ConfigureSSL]. Ancak, bir ASE'de, önemli bir istisna vardır. Bir ILB ASE IP tabanlı SSL için kullanılacak ek IP adresleri eklenemiyor.

ASEv1'de kullanabilmeniz için önce IP adresleri kaynakları ayırmanız gerekir. Çok kiracılı App Service gibi ASEv2 ', bunları uygulamanızdan kullanın. Her zaman bir yedek adres yok ASEv2 en fazla 30 IP adresi. Böylece her zaman bir adres kullanım için kullanıma hazır her birini kullanın, başka bir eklenir. Gecikme ekleme IP engelleyen başka bir IP adresi ayırmak için gerekli olduğu bir saate sayfayı hızlı bir şekilde ele alır.

## <a name="front-end-scaling"></a>Ön uç ölçeklendirme ##

ASEv2 içinde çalışanları App Service planlarınızda ölçeklediğinizde tarafından otomatik olarak desteklemek için eklenir. ASE her iki ön uçlar ile oluşturulur. Ayrıca, ön otomatik olarak bir hızda her 15 örnekleri için bir ön uç ölçeği genişletme, App Service planlarında sona erer. 15 örnekleriniz varsa, örneğin, sonra üç ön uçlar sizde. 30 örneğe genişletme, ardından dört ön uçlar vb. vardır.

Ön uçlar sayısı yeterli çoğu fazlasını senaryoları olmalıdır. Ancak, daha hızlı bir fiyat karşılığında ölçeği genişletebilirsiniz. Beş her örnek için bir ön olarak düşük sonuna oranı değiştirebilirsiniz. Oran değiştirmek bir ücret yoktur. Daha fazla bilgi için [Azure App Service fiyatlandırma][Pricing].

HTTP/HTTPS uç noktası ASE için ön uç kaynaklardır. Varsayılan ön uç yapılandırması ile ön uç başına bellek kullanımını sürekli olarak yaklaşık yüzde 60'tır. Müşteri iş yüklerinin bir ön uç üzerinde çalıştırmayın. Anahtar saygı ölçeklendirme ile ön uç için öncelikle HTTPS trafiğini temelli CPU faktördür.

## <a name="app-access"></a>Uygulama erişimi ##

Bir dış ASE'de uygulama oluştururken kullanılan etki alanı, çok kiracılı App Service farklıdır. Bu, ase'nin adı içerir. Dış ASE oluşturma hakkında daha fazla bilgi için bkz. [bir App Service ortamı oluşturma][MakeExternalASE]. Dış ASE etki alanı adıyla benzer *.&lt; asename&gt;. p.azurewebsites.net*. Örneğin, ASE'nizi adındaki _dış ase_ ve adlı bir uygulamanın ana bilgisayar _contoso_ ASE, bunu aşağıdaki URL'lere ulaşana içeren:

- contoso.external-ase.p.azurewebsites.net
- contoso.scm.external-ase.p.azurewebsites.net

URL contoso.scm.external ase.p.azurewebsites.net Kudu konsoluna erişmesini veya web kullanarak uygulamanızı yayımlamak için dağıtmak için kullanılır. Kudu konsolunu hakkında daha fazla bilgi için bkz: [Azure App Service için Kudu Konsolu][Kudu]. Kudu konsolunu, web kullanıcı Arabirimi sunar hata ayıklama, karşıya dosya yükleme, dosyaları ve daha fazlasını düzenlemek için.

ILB ASE'de, dağıtım sırasında etki alanı belirler. ILB ASE oluşturma hakkında daha fazla bilgi için bkz. [oluşturma ve kullanma ILB ASE][MakeILBASE]. Etki alanı adı belirtirseniz _ılb ase.info_, o ase'deki uygulamalar uygulama oluşturma sırasında bu etki alanı kullanın. Adlı bir uygulama için _contoso_, URL'ler:

- contoso.ilb ase.info
- contoso.SCM.ilb ase.info

## <a name="publishing"></a>Yayımlama ##

Çok kiracılı olarak App Service ile bir ASE'de, ile yayımlayabilirsiniz:

- Web dağıtımı.
- FTP.
- Sürekli Tümleştirme.
- Kudu konsolunda sürükleyip yeniden açın.
- Visual Studio, Eclipse veya Intellij Idea gibi bir IDE.

Dış ASE ile yayımlama bu seçeneklerin tümü aynı şekilde davranır. Daha fazla bilgi için [Azure App Service'te dağıtım][AppDeploy]. 

ILB ASE ile ilgili en önemli fark yayımlama var. Bir ILB ASE ile yayımlama uç noktaları yalnızca ILB ile tüm büyük/küçük harf kullanılabilir. ILB ASE alt ağdaki sanal ağdaki bir özel IP açıktır. ILB ağ erişimi yoksa, bu ASE üzerinde herhangi bir uygulamayı yayımlayamazsınız. Belirtilen [oluşturma ve kullanma ILB ASE][MakeILBASE], DNS, sistemdeki uygulamalar için yapılandırmanız gerekir. Bu, SCM uç noktasının içerir. Bunlar düzgün tanımlanmamışsa, yayımlanamıyor. Ayrıca, IDE'ler doğrudan yayımlamak için ILB ağ erişiminiz olması gerekir.

Yayımlama uç nokta Internet erişilebilir olmadığı için kullanıma hazır, GitHub ve Azure DevOps gibi Internet tabanlı CI sistemleri bir ILB ASE ile çalışmaz. Azure DevOps için bu sorunu, ILB ulaşabilecekleri iç ağınızda şirket içinde barındırılan bir sürüm aracı yükleyerek çalışabilirsiniz. Alternatif olarak, Dropbox gibi çekme modeli kullanan bir CI sistemi kullanabilirsiniz.

Bir ILB ASE’deki uygulamalar için yayımlama uç noktaları, ILB ASE oluşturulurken kullanılan etki alanını kullanır. Uygulamanın yayımlama profilinde ve uygulamanın portal dikey penceresinde görebilirsiniz (içinde **genel bakış** > **Essentials** ve ayrıca **özellikleri**). 

## <a name="pricing"></a>Fiyatlandırma ##

Fiyatlandırma SKU adı verilen **yalıtılmış** yalnızca ASEv2 ile kullanmak için oluşturuldu. İçinde ASEv2 barındırılan tüm App Service planları yalıtılmış fiyatlandırma SKU'su ' dir. Yalıtılmış App Service planı ücretler bölgeye göre farklılık gösterebilir. 

Ek olarak, App Service planları için fiyat, ASE kendisi için bir sabit ücretle yoktur. Sabit fiyat ile ASE'nizi boyutunu değiştirmez ve ek 1 oranını ölçeklendirme varsayılan konumunda ASE altyapı için ödeme ön uç için her 15 App Service planı örneği.  

Her 15 App Service planı örneği için 1 ön uç varsayılan ölçek oranını yeterince hızlı değilse, hangi ön uç eklenir veya ön uç boyutu oranında ayarlayabilirsiniz.  Oran veya boyutu ayarladığınızda, varsayılan olarak eklenebilir olmayan ön uç çekirdekleri için ödeme yaparsınız.  

Örneğin, 10 ölçek oranı ayarlarsanız, ön uç App Service planlarınızda 10 her örnek için eklenir. Sabit Ücret, her 15 örnekleri için bir ön uç ölçeği oranını kapsar. 10 ile bir ölçek oranı, 10 App Service planı örneği için eklenen üçüncü bir ön uç için ücret ödemeniz. Otomatik olarak eklendiğinden 15 örnekleri ulaştığında için ödeme yapmak zorunda kalmazsınız.

2 Çekirdek için ön uç boyutu ayarlanmış, ancak oranı ayarlama ek çekirdekler için ödeme yaparsınız.  Bir ASE 2 ön uç, böylece fazladan 2 Çekirdek için ödeyeceğiniz 2 Çekirdek ön uç boyutu artar, otomatik ölçeklendirme eşiğin altına ile oluşturulur.

Daha fazla bilgi için [Azure App Service fiyatlandırma][Pricing].

## <a name="delete-an-ase"></a>Bir ASE Sil ##

Bir ASE silmek için: 

1. Kullanım **Sil** en üstündeki **App Service ortamı** dikey penceresi. 

1. ASE'NİZİN silmek istediğinizi onaylamak için adı girin. Bir ASE sildiğinizde tüm içerik içindeki silin. 

    ![ASE silme][3]

<!--Image references-->
[1]: ./media/using_an_app_service_environment/usingase-appcreate.png
[2]: ./media/using_an_app_service_environment/usingase-pricingtiers.png
[3]: ./media/using_an_app_service_environment/usingase-delete.png


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
[Functions]: ../../azure-functions/index.yml
[Pricing]: https://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: https://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../deploy-local-git.md
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
