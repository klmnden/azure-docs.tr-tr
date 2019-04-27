---
title: Azure - dış App Service ortamı oluşturma
description: Bir uygulama ya da tek başına oluştururken bir App Service ortamı oluşturma açıklanır
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 94dd0222-b960-469c-85da-7fcb98654241
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: eef13c5a4e3757b0eafd77c0915717175c2dbd8c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60769109"
---
# <a name="create-an-external-app-service-environment"></a>Bir dış App Service ortamı oluşturma

Azure App Service Ortamı, Azure App Service’in Azure sanal ağı (VNet) içindeki bir alt ağa dağıtımıdır.

> [!NOTE]
> Bir sanal IP (App Service ortamı başvurmak için kullanılan VIP), her bir App Service ortamı yok.

Bir App Service Ortamı (ASE) iki şekilde dağıtılabilir:

- Genellikle Dış ASE olarak adlandırılan durumda, bir dış IP adresi üzerindeki VIP ile.
- İç uç nokta bir iç yük dengeleyici (ILB) olduğu için iç IP adresi üzerindeki VIP ile genellikle bir ILB ASE olarak adlandırılan.

Bu makalede, dış ASE oluşturma işlemini gösterir. ASE genel bakış için bkz. [App Service ortamı giriş][Intro]. ILB ASE oluşturma hakkında daha fazla bilgi için bkz: [oluşturma ve kullanma ILB ASE][MakeILBASE].

## <a name="before-you-create-your-ase"></a>ASE'NİZİN oluşturmadan önce

ASE'yi oluşturduktan sonra aşağıdaki değiştiremezsiniz:

- Location
- Abonelik
- Kaynak grubu
- Kullanılan sanal ağ
- Kullanılan alt ağ
- Alt ağ boyutu

> [!NOTE]
> Bir VNet seçin ve bir alt ağ belirtin, gelecekteki büyümeye ve ölçeklendirme ihtiyaçlarını karşılayacak kadar büyük olduğundan emin olun. Boyutu öneririz `/24` 256 adreslerine sahip.
>

## <a name="three-ways-to-create-an-ase"></a>Bir ASE oluşturma üç yolu

Bir ASE oluşturma üç yolu vardır:

- **Bir App Service planı oluşturulurken**. Bu yöntem, tek bir adımda ASE ile App Service planı oluşturur.
- **Tek başına bir eylem olarak**. Bu yöntem, tek başına bir ASE, boş bir ASE olduğu oluşturur. Bu yöntem, ASE oluşturmak için daha gelişmiş bir işlemdir. Bir ASE, ILB ile oluşturmak için kullanın.
- **Bir Azure Resource Manager şablonundan**. Bu yöntem, İleri düzey kullanıcılar için kullanılır. Daha fazla bilgi için [şablondan ASE oluşturma][MakeASEfromTemplate].

Dış ASE ase'deki uygulamalar için tüm HTTP/HTTPS trafiğini İnternet'ten erişilebilen bir IP adresi ulaştığını anlamına gelir. bir genel VIP sahiptir. Bir ASE, ILB ile ASE tarafından kullanılan alt ağdan bir IP adresi vardır. ILB ASE'de barındırılan uygulamalar, doğrudan internet'e kullanıma sunulmaz.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Birlikte bir ASE ve App Service planı oluşturma

App Service planı, uygulamaların bir kapsayıcıdır. App Service'te bir uygulama oluşturduğunuzda, seçin veya bir App Service planı oluşturun. App Service ortamları App Service planları basılı tutun ve uygulamaları App Service planları tutun.

Bir App Service planını oluştururken bir ASE oluşturmak için:

1. İçinde [Azure portalında](https://portal.azure.com/)seçin **kaynak Oluştur** > **Web + mobil** > **Web uygulaması**.

    ![Web uygulaması oluşturma][1]

2. Aboneliğinizi seçin. Uygulama ve ASE aynı abonelik içinde oluşturulur.

3. Kaynak grubunu seçin veya oluşturun. Kaynak grupları ile ilgili Azure kaynaklarını bir birim olarak yönetebilirsiniz. Kaynak grupları, ayrıca, uygulamalarınız için rol tabanlı erişim denetimi kuralları oluştur olduğunda yararlıdır. Daha fazla bilgi için [Azure Resource Manager'a genel bakış][ARMOverview].

4. (Windows, Linux ve Docker) işletim sisteminizi seçin. 

5. App Service planı seçin ve ardından **Yeni Oluştur**. Linux web uygulamaları ve Windows web uygulamaları aynı App Service planında olamaz, ancak aynı App Service Ortamı'nda olabilir. 

    ![Yeni App Service planı][2]

6. İçinde **konumu** aşağı açılan listesinde, istediğiniz bölgeyi seçin, ASE oluşturma. Mevcut bir ASE seçerseniz, yeni bir ASE oluşturulmaz. App Service planı, seçtiğiniz ASE'de oluşturulur. 

7. Seçin **fiyatlandırma katmanı**ve birini **yalıtılmış** fiyatlandırma SKU'ları. Seçerseniz bir **yalıtılmış** SKU kartı ve bir ASE değil bir konum, yeni bir ASE oluşturulduğu konumda. ASE oluşturma işlemini başlatmak için **seçin**. **Yalıtılmış** SKU yalnızca ASE ile birlikte kullanılabilir. Ayrıca herhangi bir fiyatlandırma SKU bir ASE'de dışında kullanamazsınız **yalıtılmış**. 

    ![Fiyatlandırma katmanı seçimi][3]

8. ASE'NİZİN adını girin. Bu ad, adreslenebilir adında uygulamalarınız için kullanılır. Ase'nin adı ise _appsvcenvdemo_, etki alanı adı *. appsvcenvdemo.p.azurewebsites.net*. Adlı bir uygulama oluşturursanız *mytestapp*, mytestapp.appsvcenvdemo.p.azurewebsites.net adreslenebilir. Ad boşluk kullanamazsınız. Büyük harf karakterler kullanırsanız, etki alanı adı toplam küçük harfli sürümünü adıdır.

    ![Yeni App Service planı adı][4]

9. Azure sanal ağ ayrıntılarını belirtin. Şunlardan birini seçin **Yeni Oluştur** veya **var olanı Seç**. Seçili bölgesinde bir sanal ağınız varsa var olan bir sanal ağ seçme seçeneği kullanılabilir. Seçerseniz **Yeni Oluştur**, sanal ağ için bir ad girin. Bu ada sahip yeni bir Resource Manager sanal ağı oluşturulur. Adres alanı kullanan `192.168.250.0/23` seçili bölgesinde. Seçerseniz **var olanı Seç**, gerekir:

    a. Birden fazla aboneliğiniz varsa, sanal ağ adres bloğu seçin.

    b. Yeni bir alt ağ adı girin.

    c. Alt ağ boyutunu seçin. *Unutmayın, ase'nizin gelecekteki büyümeye uyum sağlamak için büyük bir boyut seçin.* Öneririz `/25`, 128 adres içeren ve bir en büyük boyutlu ASE'yi işleyebilen. Önermemekteyiz `/28`, örneğin, çünkü yalnızca 16 adresleri kullanılabilir. Altyapı en az yedi adresleri ve başka bir 5 Azure ağ kullanır. İçinde bir `/28` alt kaldığını App Service planı, ILB ASE için örnekleri en dış ASE için 4 App Service planı örneği, ölçekleme ve yalnızca 3.

    d. Alt ağ IP aralığı seçin.

10. Seçin **Oluştur** ASE oluşturma. Bu işlem ayrıca App Service planı ve bir uygulama oluşturur. ASE, App Service planı ve app: aynı abonelik altında tümünü ve ayrıca aynı kaynak grubunda. Ayrı bir kaynak grubu, ASE'NİZİN erişmesi gerekiyorsa veya ILB ASE gerekiyorsa, tek başına bir ASE oluşturma adımlarını izleyin.

## <a name="create-an-ase-and-a-linux-web-app-using-a-custom-docker-image-together"></a>Bir ASE ve özel bir Docker görüntüsü birlikte kullanarak bir Linux web uygulaması oluşturma

1. İçinde [Azure portalında](https://portal.azure.com/), **kaynak Oluştur** > **Web + mobil** > **kapsayıcılar için Web uygulaması.** 

    ![Web uygulaması oluşturma][7]

1. Aboneliğinizi seçin. Uygulama ve ASE aynı abonelik içinde oluşturulur.

1. Kaynak grubunu seçin veya oluşturun. Kaynak grupları ile ilgili Azure kaynaklarını bir birim olarak yönetebilirsiniz. Kaynak grupları, ayrıca, uygulamalarınız için rol tabanlı erişim denetimi kuralları oluştur olduğunda yararlıdır. Daha fazla bilgi için [Azure Resource Manager'a genel bakış][ARMOverview].

1. App Service planı seçin ve ardından **Yeni Oluştur**. Linux web uygulamaları ve Windows web uygulamaları aynı App Service planında olamaz, ancak aynı App Service Ortamı'nda olabilir. 

    ![Yeni App Service planı][8]

1. İçinde **konumu** aşağı açılan listesinde, istediğiniz bölgeyi seçin, ASE oluşturma. Mevcut bir ASE seçerseniz, yeni bir ASE oluşturulmaz. App Service planı, seçtiğiniz ASE'de oluşturulur. 

1. Seçin **fiyatlandırma katmanı**ve birini **yalıtılmış** fiyatlandırma SKU'ları. Seçerseniz bir **yalıtılmış** SKU kartı ve bir ASE değil bir konum, yeni bir ASE oluşturulduğu konumda. ASE oluşturma işlemini başlatmak için **seçin**. **Yalıtılmış** SKU yalnızca ASE ile birlikte kullanılabilir. Ayrıca herhangi bir fiyatlandırma SKU bir ASE'de dışında kullanamazsınız **yalıtılmış**. 

    ![Fiyatlandırma katmanı seçimi][3]

1. ASE'NİZİN adını girin. Bu ad, adreslenebilir adında uygulamalarınız için kullanılır. Ase'nin adı ise _appsvcenvdemo_, etki alanı adı *. appsvcenvdemo.p.azurewebsites.net*. Adlı bir uygulama oluşturursanız *mytestapp*, mytestapp.appsvcenvdemo.p.azurewebsites.net adreslenebilir. Ad boşluk kullanamazsınız. Büyük harf karakterler kullanırsanız, etki alanı adı toplam küçük harfli sürümünü adıdır.

    ![Yeni App Service planı adı][4]

1. Azure sanal ağ ayrıntılarını belirtin. Şunlardan birini seçin **Yeni Oluştur** veya **var olanı Seç**. Seçili bölgesinde bir sanal ağınız varsa var olan bir sanal ağ seçme seçeneği kullanılabilir. Seçerseniz **Yeni Oluştur**, sanal ağ için bir ad girin. Bu ada sahip yeni bir Resource Manager sanal ağı oluşturulur. Adres alanı kullanan `192.168.250.0/23` seçili bölgesinde. Seçerseniz **var olanı Seç**, gerekir:

    a. Birden fazla aboneliğiniz varsa, sanal ağ adres bloğu seçin.

    b. Yeni bir alt ağ adı girin.

    c. Alt ağ boyutunu seçin. *Unutmayın, ase'nizin gelecekteki büyümeye uyum sağlamak için büyük bir boyut seçin.* Öneririz `/25`, 128 adres içeren ve bir en büyük boyutlu ASE'yi işleyebilen. Önermemekteyiz `/28`, örneğin, çünkü yalnızca 16 adresleri kullanılabilir. Altyapı en az yedi adresleri ve başka bir 5 Azure ağ kullanır. İçinde bir `/28` alt kaldığını App Service planı, ILB ASE için örnekleri en dış ASE için 4 App Service planı örneği, ölçekleme ve yalnızca 3.

    d. Alt ağ IP aralığı seçin.

1.  "Container Yapılandır" seçeneğini belirleyin
    * (Azure Container Registry, Docker Hub ve kendi özel kayıt defterinizi kullanabilirsiniz), özel görüntü adı girin. Kendi özel kapsayıcınızı kullanmak istemiyorsanız, yalnızca kodunuzu getirin ve yukarıdaki yönergeleri kullanarak Linux'ta App Service ile yerleşik görüntü kullanın. 

    ![Kapsayıcı yapılandırma][9]

1. Seçin **Oluştur** ASE oluşturma. Bu işlem ayrıca App Service planı ve bir uygulama oluşturur. ASE, App Service planı ve app: aynı abonelik altında tümünü ve ayrıca aynı kaynak grubunda. Ayrı bir kaynak grubu, ASE'NİZİN erişmesi gerekiyorsa veya ILB ASE gerekiyorsa, tek başına bir ASE oluşturma adımlarını izleyin.


## <a name="create-an-ase-by-itself"></a>Tek başına bir ASE oluşturma

Bir ASE tek başına oluşturursanız, hiçbir şey var. Boş bir ASE, hala altyapısı için aylık olarak ücretlendirilir. Bir ASE, ILB ile oluşturma veya kendi kaynak grubunda bir ASE oluşturmak için aşağıdaki adımları izleyin. ASE'yi oluşturduktan sonra uygulama içinde normal işlem kullanarak oluşturabilirsiniz. Yeni ASE'nizi konum olarak seçin.

1. Azure Market'te arama **App Service ortamı**, ya da seçin **kaynak Oluştur** > **Web mobil** > **uygulama Hizmet ortamı**. 

1. ASE'NİZİN adını girin. Bu ad, ASE içinde oluşturulan uygulamalar için kullanılır. Adı ise *mynewdemoase*, alt etki alanı adı *. mynewdemoase.p.azurewebsites.net*. Adlı bir uygulama oluşturursanız *mytestapp*, mytestapp.mynewdemoase.p.azurewebsites.net adreslenebilir. Ad boşluk kullanamazsınız. Büyük harf karakterler kullanırsanız, etki alanı adı toplam küçük harfli sürümünü adıdır. Bir ILB kullanırsanız, ASE adınız alt etki alanı içinde kullanılmaz, ancak bunun yerine ASE oluşturma sırasında açıkça belirtilir.

    ![ASE adlandırma][5]

1. Aboneliğinizi seçin. Bu abonelik ayrıca ASE içindeki tüm uygulamalar kullanan biridir. Başka bir abonelikte bir vnet'e ASE'nizi konulamaz.

1. Seçin veya yeni bir kaynak grubu belirtin. ASE'niz için kullanılan kaynak grubunu, sanal ağ için kullanılan hizmet örneğiyle aynı olmalıdır. Mevcut bir sanal ağı seçerseniz ASE'niz için kaynak grubu seçimi, ağınızın yansıtacak şekilde güncelleştirilir. *Resource Manager şablonu kullanıyorsanız, VNet kaynak grubundan farklı bir kaynak grubu ile bir ASE oluşturabilirsiniz.* Şablondan ASE oluşturma için bkz: [bir şablondan bir App Service ortamı oluşturma][MakeASEfromTemplate].

    ![Kaynak grubu seçimi][6]

1. VNet ve konumu seçin. Yeni bir VNet oluşturun veya mevcut bir VNet seçin: 

    * Yeni bir VNet seçerseniz, bir ad ve konum belirtebilirsiniz. 
    
    * Yeni sanal ağ adres aralığı 192.168.250.0/23 ve varsayılan adlı bir alt ağ vardır. Alt ağ 192.168.250.0/24 tanımlanır. Yalnızca Resource Manager sanal ağı seçebilirsiniz. **VIP türü** seçim ASE'NİZİN doğrudan internet'ten (Dış) erişilebilen bir ILB kullanıyorsa veya belirler. Bu seçenekler hakkında daha fazla bilgi için bkz. [oluşturma ve kullanma App Service ortamı ile iç yük dengeleyici][MakeILBASE]. 

      * Seçerseniz **dış** için **VIP türü**, sistem oluşturulur ile IP tabanlı SSL amaçları için kaç dış IP adreslerini seçebilirsiniz. 
    
      * Seçerseniz **dahili** için **VIP türü**, ASE'nizi kullanan etki alanı belirtmeniz gerekir. Bir ASE genel veya özel adres aralıkları kullanan bir Vnet'e dağıtabilirsiniz. Ortak adres aralığı ile bir sanal ağ kullanmak için önceden sanal ağ oluşturmanız gerekir. 
    
    * Mevcut bir sanal ağı seçerseniz, ASE'yi oluşturulduğunda yeni bir alt ağ oluşturulur. *Portalda, önceden oluşturulmuş bir alt ağ kullanamazsınız. Resource Manager şablonu kullanıyorsanız, var olan bir alt ağ ile bir ASE oluşturabilirsiniz.* Şablondan ASE oluşturma için bkz: [bir şablondan bir App Service ortamı oluşturma][MakeASEfromTemplate].

## <a name="app-service-environment-v1"></a>App Service Ortamı v1

App Service ortamı (ASEv1) ilk sürümü örneklerini yine de oluşturabilirsiniz. İşlemini başlatmak için markette Ara **App Service ortamı v1**. ASE'i ASE tek başına oluşturduğunuz aynı şekilde oluşturun. Tamamlandığında, iki ön uç ve iki çalışan, ASEv1 sahiptir. ASEv1 ile ön uçlar ve çalışanlardan yönetmeniz gerekir. App Service planlarınızda oluşturduğunuzda, bunlar otomatik olarak eklenir. Ön uçlar HTTP/HTTPS uç noktaları olarak davranır ve çalışanlar için trafiği göndermek. Çalışanlar, uygulamaları barındıran rollerdir. ASE'yi oluşturduktan sonra ön uçlar ve çalışanlardan miktarını ayarlayabilirsiniz. 

ASEv1 hakkında daha fazla bilgi için bkz: [App Service ortamı v1 giriş][ASEv1Intro]. Ölçeklendirme hakkında daha fazla bilgi için bkz: yönetme ve ASEv1, izleme [bir App Service ortamını yapılandırma][ConfigureASEv1].

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png
[7]: ./media/how_to_create_an_external_app_service_environment/createexternalase-createwafc.png
[8]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreatewafc.png
[9]: ./media/how_to_create_an_external_app_service_environment/createexternalase-configurecontainer.png



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
