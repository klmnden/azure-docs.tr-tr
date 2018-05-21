---
title: Bir Azure uygulama hizmeti ortamı'nı kullanma
description: Oluşturma, yayımlama ve uygulamaları Azure uygulama hizmeti ortamı ölçeklendirme
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
ms.openlocfilehash: 66ef20616df77dc809a79e516a53133a80759dc7
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="use-an-app-service-environment"></a>Bir uygulama hizmeti ortamı'nı kullanma #

## <a name="overview"></a>Genel Bakış ##

Azure uygulama hizmeti ortamı bir Azure uygulama hizmeti dağıtımı bir müşterinin Azure sanal ağındaki bir alt ağ ile değil. Aşağıdakilerden oluşur:

- **Ön Uçları**: ön uçlar olduğu bir uygulama hizmeti ortamı'nda (ana) HTTP/HTTPS sonlandırır şunlardır.
- **Çalışanlar**: uygulamalarınızı ana bilgisayar kaynakları çalışanlardır.
- **Veritabanı**: ortamı tanımlayan bilgileri veritabanında tutar.
- **Depolama**: depolama birimini müşteri yayımlanan uygulamaları barındırmak için kullanılır.

> [!NOTE]
> Uygulama hizmeti ortamı iki sürümü vardır: ASEv1 ve ASEv2. ASEv1 içinde kullanabilmek için önce kaynakların yönetmeniz gerekir. Yapılandırma ve ASEv1 yönetme hakkında bilgi edinmek için [bir uygulama hizmeti ortamı v1 yapılandırma][ConfigureASEv1]. Bu makalenin geri kalanında ASEv2 odaklanır.
>
>

Uygulama erişimi için bir harici veya dahili VIP ile bir ana (ASEv1 ve ASEv2) dağıtabilirsiniz. Bir dış VIP dağıtımla genellikle bir dış ana olarak adlandırılır. Bir iç yük dengeleyici (ILB) kullandığından iç sürüm ILB ana adı verilir. ILB ana hakkında daha fazla bilgi için bkz: [oluşturma ve kullanma bir ILB ana][MakeILBASE].

## <a name="create-a-web-app-in-an-ase"></a>ASE'de bir web uygulaması oluşturma ##

ASE'de bir web uygulaması oluşturmak için aynı süreci normalde oluşturduğunuzda, ancak bazı küçük farklar ile kullanın. Yeni bir uygulama hizmeti planı oluşturduğunuzda:

- Uygulamanızı dağıtmak bir coğrafi konum seçmek yerine bir ana konumunuzu seçin.
- ASE'de oluşturulan tüm uygulama hizmeti planları, fiyatlandırma katmanı bir Isolated olması gerekir.

Bir ana yoksa, bir'ndaki yönergeleri izleyerek oluşturabilirsiniz [uygulama hizmeti ortamı oluşturma][MakeExternalASE].

ASE'de bir web uygulaması oluşturmak için:

1. Seçin **kaynak oluşturma** > **Web + mobil** > **Web uygulaması**.

2. Web uygulaması için bir ad girin. ASE'de zaten bir uygulama hizmeti planı seçtiyseniz, uygulama için etki alanı adı ana etki alanı adını yansıtır.

    ![Web uygulaması adı seçimi][1]

3. Bir abonelik seçin.

4. Yeni bir kaynak grubu için bir ad girin veya seçin **var olanı kullan** ve aşağı açılan listeden birini seçin.

5. İşletim sisteminizi seçin. 

    * Linux uygulamaları üretim iş yükleri çalışmakta olan bir ana eklemeyin önerdiğimiz bir ana Linux uygulamada barındırma yeni bir önizleme özelliği olduğundan. 
    * Bir ana Linux uygulama ekleme ana önizleme modunda da olacağı anlamına gelir. 

5. Var olan bir uygulama hizmeti planı, ana seçin veya aşağıdaki adımları izleyerek yeni bir tane oluşturun:

    a. Seçin **Yeni Oluştur**.

    b. Uygulama hizmeti planınız için adı girin.

    c. Ana seçin **konumu** aşağı açılan liste. Bir ana Linux uygulamada barındırma yalnızca etkin 6 bölgelerde, şu anda: **Batı ABD, Doğu ABD, Batı Avrupa, Kuzey Avrupa, Doğu Avustralya, Güneydoğu Asya.** 

    d. Seçin bir **Isolated** fiyatlandırma katmanı. Seçin **seçin**.

    e. **Tamam**’ı seçin.
    
    ![Yalıtılmış fiyatlandırma katmanları][2]

    > [!NOTE]
    > Linux web uygulamaları ve Windows web uygulamalarını aynı uygulama hizmeti planı'nda olamaz, ancak aynı uygulama hizmeti ortamı'nda olabilir. 
    >

6. **Oluştur**’u seçin.

## <a name="how-scale-works"></a>Nasıl çalışır ölçeklendirme ##

Her uygulama hizmeti uygulaması bir uygulama hizmeti planında çalışır. Kapsayıcı ortamları uygulama hizmeti planları basılı tutun ve uygulama hizmeti planları uygulamaları barındıracak modelidir. Bir uygulama ölçeklendirdiğinizde uygulama hizmeti planı ölçeklendirme ve dolayısıyla tüm uygulamalar aynı planda ölçeklendirin.

Bir uygulama hizmeti planı ölçeklendirdiğinizde ASEv2 içinde gereken altyapıyı otomatik olarak eklenir. Altyapı eklenirken ölçeklendirme işlemleri için gecikme süresini yoktur. Oluşturabilir veya uygulama hizmeti planınızı ölçeklendirin önce ASEv1 içinde gereken altyapıyı eklenmesi gerekir. 

Uygulama hizmeti multitenant ölçeklendirme, bir kaynak havuzu desteklemek kullanıma hazır olduğundan genellikle hemen uygulanır. Bir ana böyle bir arabellek yoktur ve kaynakları gerek ayrılır.

ASE'de, en fazla 100 örnekleri ölçeklendirebilirsiniz. Bu 100 örnekleri arasında birden çok uygulama hizmeti planları dağıtılmış veya tüm tek tek uygulama hizmeti planında olabilir.

## <a name="ip-addresses"></a>IP adresleri ##

Uygulama hizmeti bir uygulama için ayrılmış bir IP adresi ayırma yeteneği vardır. Bir IP tabanlı SSL yapılandırdıktan sonra bu özellik açıklandığı gibi kullanılabilir [Azure web uygulamaları için var olan özel bir SSL sertifikası bağlama][ConfigureSSL]. Ancak, ASE'de, önemli bir özel durum yoktur. Bir ILB ana bir IP tabanlı SSL için kullanılacak ek IP adresleri eklenemiyor.

ASEv1 içinde kullanabilmek için önce IP adreslerini kaynaklar olarak ayırması gerekmez. Çok kullanıcılı uygulama hizmeti gibi ASEv2, bunları uygulamanızdan kullanın. Her zaman bir yedek adres yok ASEv2 içinde en çok 30 IP adresi. Bir adresi her zaman kullanıma hazır olmasını sağlamak her birini kullanın, başka bir eklenir. Gecikme IP ekleme engelleyen başka bir IP adresi ayırmak için bir kez gerektiğinde hızlı art arda giderir.

## <a name="front-end-scaling"></a>Ön uç ölçeklendirme ##

ASEv2 içinde çalışanları, uygulama hizmeti planları ölçeklendirdiğinizde tarafından otomatik olarak desteklemek için eklenir. Her ana iki ön uçlar ile oluşturulur. Ayrıca, ön otomatik olarak ölçeklendirme her 15 örnekleri için bir ön uç hızında uygulama hizmeti planlarında sona erer. 15 örneği varsa, örneğin, daha sonra üç ön uçlar sahip. 30 örneklerine ölçeklendirme, sonra dört ön uçları vb. vardır.

Ön Uçları sayısı fazlasıyla yeterlidir çoğu için senaryolar olmalıdır. Ancak, çıkışı daha hızlı bir oranda ölçeklendirebilirsiniz. Her beş örnekleri için bir ön olarak düşük sonuna oranı değiştirebilirsiniz. Oranını değiştirmek bir ücret yoktur. Daha fazla bilgi için bkz: [Azure uygulama hizmeti fiyatlandırması][Pricing].

HTTP/HTTPS uç noktası ana için ön uç kaynaklardır. Varsayılan ön uç yapılandırma ile ön uç başına bellek kullanımı sürekli olarak yaklaşık yüzde 60'tır. Müşteri iş yükleri bir ön uç üzerinde çalışmıyor. Anahtar ölçeklendirmek için olan bir ön uç için öncelikle HTTPS trafiği tarafından yönlendirilen CPU faktördür.

## <a name="app-access"></a>Uygulama erişimi ##

Dış ASE'de uygulamaları oluştururken kullanılan etki alanı, çok kullanıcılı uygulama hizmeti farklıdır. ANA adını içerir. Bir dış ana oluşturma hakkında daha fazla bilgi için bkz: [uygulama hizmeti ortamı oluşturma][MakeExternalASE]. Bir dış ana etki alanı adıyla benzer *.&lt; asename&gt;. p.azurewebsites.net*. Örneğin, ana adlı, _dış ana_ ve adlı bir uygulama ana bilgisayar _contoso_ ana, bunu aşağıdaki URL'lere ulaşana içeren:

- contoso.external-ase.p.azurewebsites.net
- contoso.scm.external-ase.p.azurewebsites.net

URL contoso.scm.external ase.p.azurewebsites.net Kudu konsoluna erişmesini veya web kullanarak uygulamanızı yayımlamak için dağıtmak için kullanılır. Kudu Konsolu hakkında daha fazla bilgi için bkz: [Kudu Konsolu Azure App Service için][Kudu]. Kudu konsol, web kullanıcı Arabirimi sağlar hata ayıklama, dosyalar, dosyalar ve çok daha fazlasını düzenleme için.

ILB ASE'de dağıtım sırasında etki alanı belirler. Bir ILB ana oluşturma hakkında daha fazla bilgi için bkz: [oluşturma ve kullanma bir ILB ana][MakeILBASE]. Etki alanı adı belirtirseniz, _ılb ase.info_, o ana uygulamalarda uygulama oluşturma sırasında bu etki alanı kullanın. Adlı uygulama için _contoso_, URL'ler:

- contoso.ilb ase.info
- contoso.SCM.ilb ase.info

## <a name="publishing"></a>Yayımlama ##

Çok kullanıcılı olarak App Service ile ASE'de ile yayımlayabilirsiniz:

- Web dağıtımı.
- FTP.
- Sürekli Tümleştirme.
- Kudu konsolunda sürükleyip yeniden açın.
- Visual Studio, Eclipse ya da Intellij Idea gibi bir IDE.

Bir dış ana ile yayımlama bu seçeneklerin tümü aynı şekilde davranır. Daha fazla bilgi için bkz: [Azure App Service'te dağıtım][AppDeploy]. 

Ana yayımlama ile ilgili bir ILB ana farktır. Bir ILB ana ile yayımlama uç noktalar yalnızca ILB ile tüm büyük/küçük harf kullanılabilir. Özel IP ana alt ağdaki sanal ağda ILB açıktır. ILB için ağ erişimi yoksa, o ana tüm uygulamaların yayımlanamıyor. İçinde belirtildiği gibi [oluşturma ve kullanma bir ILB ana][MakeILBASE], DNS sisteminde uygulamalar için yapılandırmanız gerekir. SCM uç noktasının dahildir. Bunlar düzgün tanımlanmamış, yayımlanamıyor. Ayrıca, IDE doğrudan yayımlamak için ILB için ağ erişimi olması gerekir.

Yayımlama uç noktası Internet erişilebilir olmadığı için Internet tabanlı CI sistemler, GitHub ve Visual Studio Team Services gibi bir ILB ana ile çalışmaz. Bunun yerine, Dropbox gibi çekme modeli kullanan bir CI sistemi kullanmanız gerekir.

Bir ILB ASE’deki uygulamalar için yayımlama uç noktaları, ILB ASE oluşturulurken kullanılan etki alanını kullanır. Uygulamanın yayımlama profili ve uygulamanızın portal dikey görebilirsiniz (içinde **genel bakış** > **Essentials** ve ayrıca **özellikleri**). 

## <a name="pricing"></a>Fiyatlandırma ##

SKU adlı fiyatlandırma **Isolated** yalnızca ASEv2 ile kullanmak için oluşturuldu. ASEv2 içinde barındırılan tüm uygulama hizmeti planları SKU fiyatlandırma Isolated içinde ' dir. Yalıtılmış uygulama hizmeti planı hızları, bölge başına farklılık gösterebilir. 

Uygulama hizmeti planları için fiyat yanı sıra ana kendisi için bir düz oranı yoktur. Düz oranı, ana boyutunu değiştirmez ve 1 ek oranını ölçeklendirme varsayılan bir ana altyapısının için ödenen her 15 uygulama hizmeti planı örnekleri için ön uç.  

Her 15 uygulama hizmeti planı örnekleri için 1 ön uç varsayılan ölçek oranını yeterince hızlı değilse, hangi ön uç eklenir veya ön uç boyutunu oranında ayarlayabilirsiniz.  Oranı veya boyutunu ayarladığınızda, varsayılan olarak ekleneceği olmayan ön uç çekirdekleri ücret ödersiniz.  

Örneğin, 10 ölçek oranı ayarlarsanız, ön uç her 10 örnekleri için uygulama hizmeti planlarında eklenir. Ücret her 15 örnekleri için bir ön uç ölçek oranını kapsar. 10 ölçek oranını ile 10 uygulama hizmeti planı örnekleri için eklenen üçüncü ön uç için ücret ödemeniz. Otomatik olarak eklendiğinden 15 örnekleri ulaştığında için ödeme gerekmez.

2 Çekirdek için ön uç boyutunu ayarlanmış, ancak oranı ayarlama ek çekirdek için ücret ödersiniz.  Bir ana 2 ön uç, boyutu 2 Çekirdek ön uç için artar, 2 fazladan çekirdek için ödeme böylece bile otomatik ölçeklendirme eşiğin altına oluşturulur.

Daha fazla bilgi için bkz: [Azure uygulama hizmeti fiyatlandırması][Pricing].

## <a name="delete-an-ase"></a>Bir ana Sil ##

Bir ana silmek için: 

1. Kullanım **silmek** en üstündeki **uygulama hizmeti ortamı** dikey. 

2. Silmek istediğinizi onaylamak için ana adını girin. Bir ana sildiğinizde, tüm içeriğin içindeki silin. 

    ![Ana silme][3]

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
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../app-service-deploy-local-git.md
[ASEWAF]: app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
