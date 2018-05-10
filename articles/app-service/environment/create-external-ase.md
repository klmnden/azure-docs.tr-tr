---
title: Dış Azure uygulama hizmeti ortamı oluşturma
description: Bir uygulama ya da tek başına oluştururken bir uygulama hizmeti ortamının nasıl oluşturulacağı açıklanmaktadır
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
ms.openlocfilehash: debfff03ea9a4de4fb2cd69779d58709a6a3a34f
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="create-an-external-app-service-environment"></a>Bir dış uygulama hizmeti ortamı oluşturun #

Azure App Service Ortamı, Azure App Service’in Azure sanal ağı (VNet) içindeki bir alt ağa dağıtımıdır. Bir App Service ortamı (ASE) iki şekilde dağıtılabilir:

- Genellikle Dış ASE olarak adlandırılan durumda, bir dış IP adresi üzerindeki VIP ile.
- Bir iç yük dengeleyici (ILB) iç uç nokta olduğu için VIP iç IP adresi, genellikle bir ILB ana olarak adlandırılır.

Bu makalede bir dış ana oluşturulacağını gösterir. Ana genel bakış için bkz: [uygulama hizmeti ortamı giriş][Intro]. Bir ILB ana oluşturma hakkında daha fazla bilgi için bkz: [oluşturma ve kullanma bir ILB ana][MakeILBASE].

## <a name="before-you-create-your-ase"></a>Ana oluşturmadan önce ##

Ana oluşturduktan sonra aşağıdaki değiştiremezsiniz:

- Konum
- Abonelik
- Kaynak grubu
- Kullanılan sanal ağ
- Kullanılan alt ağ
- Alt ağ boyutu

> [!NOTE]
> Bir sanal ağı seçin ve bir alt ağ belirtip karşılık Gelecekteki büyümeyi barındırmak için yeterince büyük olduğundan emin olun. Dosya boyutu öneririz `/25` 128 adreslerine sahip.
>

## <a name="three-ways-to-create-an-ase"></a>Bir ana oluşturmak için üç yol ##

Bir ana oluşturmanın üç yolu vardır:

- **Bir uygulama hizmeti planı oluşturma sırasında**. Bu yöntem, tek bir adımda ana ve uygulama hizmeti planı oluşturur.
- **Bir tek başına eylem olarak**. Bu yöntem boş bir ana olduğu tek başına ana, oluşturur. Bu yöntem, bir ana oluşturmak için daha gelişmiş bir işlemdir. Bir ILB ile bir ana oluşturmak için kullanın.
- **Bir Azure Resource Manager şablondan**. Bu yöntem İleri düzey kullanıcılar için kullanılır. Daha fazla bilgi için bkz: [bir ana şablondan oluşturma][MakeASEfromTemplate].

Bir dış ana ana uygulamalarında tüm HTTP/HTTPS trafiğini internet'ten erişilebilen bir IP adresi isabetler anlamına gelen genel bir VIP sahiptir. Bir ILB ile bir ana ana tarafından kullanılan alt ağdan bir IP adresi vardır. ILB ASE'de barındırılan uygulamalar, doğrudan Internet'e açık değildir.

## <a name="create-an-ase-and-an-app-service-plan-together"></a>Bir ana ve bir uygulama hizmeti planı birlikte oluşturma ##

Uygulama hizmeti planı, uygulamaların bir kapsayıcıdır. App Service içinde bir uygulama oluşturduğunuzda, seçin veya bir uygulama hizmeti planı oluşturun. Uygulama hizmeti ortamları uygulama hizmeti planları basılı tutun ve uygulama hizmeti planları uygulamaları tutun.

Bir uygulama hizmeti planı oluştururken bir ana oluşturmak için:

1. İçinde [Azure portal](https://portal.azure.com/)seçin **kaynak oluşturma** > **Web + mobil** > **Web uygulaması**.

    ![Web uygulaması oluşturma][1]

2. Aboneliğinizi seçin. Uygulama ve ana aynı Aboneliklerde oluşturulur.

3. Kaynak grubunu seçin veya oluşturun. Kaynak gruplarıyla ilgili Azure kaynaklarını bir birim olarak yönetebilirsiniz. Kaynak grupları, aynı zamanda, uygulamalarınız için rol tabanlı erişim denetimi kuralları oluşturmak olduğunda yararlıdır. Daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış][ARMOverview].

4. İşletim sisteminizde seçin. 

    * Linux uygulamaları üretim iş yükleri çalışmakta olan bir ana eklemeyin önerdiğimiz bir ana Linux uygulamada barındırma yeni bir önizleme özelliği olduğundan. 
    * Bir ana Linux uygulama ekleme ana önizleme modunda da olacağı anlamına gelir. 

5. Uygulama hizmeti planı seçin ve ardından **Yeni Oluştur**. Linux web uygulamaları ve Windows web uygulamalarını aynı uygulama hizmeti planı'nda olamaz, ancak aynı uygulama hizmeti ortamı'nda olabilir. 

    ![Yeni App Service planı][2]

6. İçinde **konumu** aşağı açılan listesinde, istediğiniz bölgeyi seçin ana oluşturun. Varolan bir ana seçerseniz, yeni bir ana oluşturulmaz. Uygulama hizmeti planı, seçili ana içinde oluşturulur. 

    > [!NOTE]
    > Ana Linux'ta 6 bölgelerde, şu anda etkin yalnızca: **Batı ABD, Doğu ABD, Batı Avrupa, Kuzey Avrupa, Doğu Avustralya, Güneydoğu Asya.** Ana Linux'ta bir önizleme özelliği olduğundan, bu Önizleme önce oluşturmuştunuz bir ana seçmeyin.
    >

7. Seçin **fiyatlandırma katmanı**ve aşağıdakilerden birini seçin **Isolated** SKU'ları fiyatlandırması. İsterseniz bir **Isolated** SKU kartı ve bir ana değil bir konum, yeni bir ana oluşturuldu. konumda. Bir ana oluşturma işlemini başlatmak için **seçin**. **Isolated** SKU yalnızca bir ana ile birlikte kullanılabilir. Ayrıca diğer fiyatlandırma SKU ASE'de dışında kullanamazsınız **Isolated**. 

    * Ana Önizleme Linux'ta için % 50 indirimlidir yalıtılmış (olacaktır iskonto ana için düz masrafları) SKU uygulanır.

    ![Fiyatlandırma katmanı seçimi][3]

8. ANA adını girin. Bu ad adreslenebilir ad uygulamalarınız için kullanılır. ANA adı ise _appsvcenvdemo_, etki alanı adı *. appsvcenvdemo.p.azurewebsites.net*. Adlı bir uygulama oluşturursanız, *mytestapp*, mytestapp.appsvcenvdemo.p.azurewebsites.net adreslenebilir. Adında boşluk kullanamazsınız. Büyük harf karakterler kullanırsanız, etki alanı adı toplam küçük harfli sürümünü adıdır.

    ![Yeni uygulama hizmeti planının adı][4]

9. Azure, sanal ağ ayrıntıları belirtin. Şunlardan birini seçin **Yeni Oluştur** veya **Varolanı seç**. Var olan bir VNet seçmek için seçenek yalnızca seçili bölgede bir VNet varsa kullanılabilir. Seçerseniz **Yeni Oluştur**, sanal ağ için bir ad girin. Bu ada sahip yeni bir Resource Manager Vnet'i oluşturulur. Adres alanı kullanan `192.168.250.0/23` seçili bölgede. Seçerseniz **var olanı Seç**, gerekir:

    a. Birden fazla varsa VNet adres bloğu seçin.

    b. Yeni bir alt ağ adı girin.

    c. Alt ağ boyutunu seçin. *ANA, Gelecekteki büyümeyi barındırmak için büyük bir boyutu seçmek unutmayın.* Öneririz `/25`, 128 adresi olduğunu ve en büyük ölçekli bir ana işleyebilir. Öneririz yok `/28`, örneğin, çünkü yalnızca 16 adresleri kullanılabilir durumdadır. En az yedi adreslerini altyapısını kullanır ve Azure ağ başka bir 5 kullanır. İçinde bir `/28` alt bıraktığınız uygulama hizmeti planı örnekleri için bir ILB ana en dış ana 4 uygulama hizmeti planı örneklerinin ölçekleme ve yalnızca 3.

    d. Alt ağ IP aralığını seçin.

10. Seçin **oluşturma** ana oluşturmak için. Bu işlem, uygulama hizmeti planı ve uygulamayı da oluşturur. ANA uygulama hizmeti planı ve uygulama olduğundan tüm aynı abonelik altında ve ayrıca aynı kaynak grubunda. Ayrı kaynak grubu, ana alması gerekiyorsa veya bir ILB ana ihtiyacınız varsa, tek başına bir ana oluşturmak için aşağıdaki adımları izleyin.

## <a name="create-an-ase-and-a-linux-web-app-using-a-custom-docker-image-together"></a>Bir ana ve özel Docker görüntü birlikte kullanarak bir Linux web uygulaması oluşturma

1. İçinde [Azure portal](https://portal.azure.com/), **kaynak oluşturma** > **Web + mobil** > **kapsayıcıları için Web uygulaması.** 

    ![Web uygulaması oluşturma][7]

2. Aboneliğinizi seçin. Uygulama ve ana aynı Aboneliklerde oluşturulur.

3. Kaynak grubunu seçin veya oluşturun. Kaynak gruplarıyla ilgili Azure kaynaklarını bir birim olarak yönetebilirsiniz. Kaynak grupları, aynı zamanda, uygulamalarınız için rol tabanlı erişim denetimi kuralları oluşturmak olduğunda yararlıdır. Daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış][ARMOverview].

4. Uygulama hizmeti planı seçin ve ardından **Yeni Oluştur**. Linux web uygulamaları ve Windows web uygulamalarını aynı uygulama hizmeti planı'nda olamaz, ancak aynı uygulama hizmeti ortamı'nda olabilir. 

    ![Yeni App Service planı][8]

5. İçinde **konumu** aşağı açılan listesinde, istediğiniz bölgeyi seçin ana oluşturun. Varolan bir ana seçerseniz, yeni bir ana oluşturulmaz. Uygulama hizmeti planı, seçili ana içinde oluşturulur. 

    > [!NOTE]
    > Ana Linux'ta 6 bölgelerde, şu anda etkin yalnızca: **Batı ABD, Doğu ABD, Batı Avrupa, Kuzey Avrupa, Doğu Avustralya, Güneydoğu Asya.** Ana Linux'ta bir önizleme özelliği olduğundan, bu Önizleme önce oluşturmuştunuz bir ana seçmeyin.
    >

6. Seçin **fiyatlandırma katmanı**ve aşağıdakilerden birini seçin **Isolated** SKU'ları fiyatlandırması. İsterseniz bir **Isolated** SKU kartı ve bir ana değil bir konum, yeni bir ana oluşturuldu. konumda. Bir ana oluşturma işlemini başlatmak için **seçin**. **Isolated** SKU yalnızca bir ana ile birlikte kullanılabilir. Ayrıca diğer fiyatlandırma SKU ASE'de dışında kullanamazsınız **Isolated**. 

    * Ana Önizleme Linux'ta için % 50 indirimlidir yalıtılmış (olacaktır iskonto ana için düz masrafları) SKU uygulanır.

    ![Fiyatlandırma katmanı seçimi][3]

7. ANA adını girin. Bu ad adreslenebilir ad uygulamalarınız için kullanılır. ANA adı ise _appsvcenvdemo_, etki alanı adı *. appsvcenvdemo.p.azurewebsites.net*. Adlı bir uygulama oluşturursanız, *mytestapp*, mytestapp.appsvcenvdemo.p.azurewebsites.net adreslenebilir. Adında boşluk kullanamazsınız. Büyük harf karakterler kullanırsanız, etki alanı adı toplam küçük harfli sürümünü adıdır.

    ![Yeni uygulama hizmeti planının adı][4]

8. Azure, sanal ağ ayrıntıları belirtin. Şunlardan birini seçin **Yeni Oluştur** veya **Varolanı seç**. Var olan bir VNet seçmek için seçenek yalnızca seçili bölgede bir VNet varsa kullanılabilir. Seçerseniz **Yeni Oluştur**, sanal ağ için bir ad girin. Bu ada sahip yeni bir Resource Manager Vnet'i oluşturulur. Adres alanı kullanan `192.168.250.0/23` seçili bölgede. Seçerseniz **var olanı Seç**, gerekir:

    a. Birden fazla varsa VNet adres bloğu seçin.

    b. Yeni bir alt ağ adı girin.

    c. Alt ağ boyutunu seçin. *ANA, Gelecekteki büyümeyi barındırmak için büyük bir boyutu seçmek unutmayın.* Öneririz `/25`, 128 adresi olduğunu ve en büyük ölçekli bir ana işleyebilir. Öneririz yok `/28`, örneğin, çünkü yalnızca 16 adresleri kullanılabilir durumdadır. En az yedi adreslerini altyapısını kullanır ve Azure ağ başka bir 5 kullanır. İçinde bir `/28` alt bıraktığınız uygulama hizmeti planı örnekleri için bir ILB ana en dış ana 4 uygulama hizmeti planı örneklerinin ölçekleme ve yalnızca 3.

    d. Alt ağ IP aralığını seçin.

9.  "Kapsayıcı yapılandırın." seçin
    * Özel görüntü adınızı (Azure kapsayıcı kayıt defteri, Docker hub'a ve kendi özel kayıt defteri kullanabilirsiniz) girin. Kendi özel kapsayıcısını kullanmak istemiyorsanız, yalnızca kodunuzu getirin ve yukarıdaki yönergeleri kullanarak Linux'ta uygulama hizmeti ile yerleşik bir görüntü kullanın. 

    ! [Kapsayıcı yapılandırmak] [9]

10. Seçin **oluşturma** ana oluşturmak için. Bu işlem, uygulama hizmeti planı ve uygulamayı da oluşturur. ANA uygulama hizmeti planı ve uygulama olduğundan tüm aynı abonelik altında ve ayrıca aynı kaynak grubunda. Ayrı kaynak grubu, ana alması gerekiyorsa veya bir ILB ana ihtiyacınız varsa, tek başına bir ana oluşturmak için aşağıdaki adımları izleyin.


## <a name="create-an-ase-by-itself"></a>Tek başına bir ana oluşturma ##

Bir ana tek başına oluşturursanız, hiçbir şey var. Boş bir ana hala altyapısı için aylık bir ücret doğurur. Bir ILB ile bir ana oluşturmak veya bir ana kendi kaynak grubu oluşturmak için aşağıdaki adımları izleyin. Ana oluşturduktan sonra uygulama içinde normal işlemi kullanarak oluşturabilirsiniz. Yeni ana konum olarak seçin.

1. Azure Market arama **uygulama hizmeti ortamı**, ya da seçin **kaynak oluşturma** > **Web mobil** > **uygulama Hizmet ortamı**. 

2. ANA adını girin. Bu ad, ana oluşturulan uygulamalar için kullanılır. Adı ise *mynewdemoase*, alt etki alanı adı *. mynewdemoase.p.azurewebsites.net*. Adlı bir uygulama oluşturursanız, *mytestapp*, mytestapp.mynewdemoase.p.azurewebsites.net adreslenebilir. Adında boşluk kullanamazsınız. Büyük harf karakterler kullanırsanız, etki alanı adı toplam küçük harfli sürümünü adıdır. Bir ILB kullanırsanız, ana adınızı alt etki alanı içinde kullanılmaz, ancak bunun yerine ana oluşturma sırasında açıkça belirtilmiştir.

    ![Ana adlandırma][5]

3. Aboneliğinizi seçin. Bu abonelik ayrıca ana tüm uygulamaları kullanan biridir. Başka bir abonelikte bir VNet, ana koymak olamaz.

4. Seçin veya yeni bir kaynak grubu belirtin. Ana için kullanılan kaynak grubunun ağınız için kullanılan adla aynı olması gerekir. Varolan bir sanal ağ seçerseniz, ana için kaynak grubu seçimi, sanal ağınızı yansıtacak şekilde güncelleştirilir. *Resource Manager şablonunu kullanırsanız, VNet kaynak grubundan farklı bir kaynak grubu ile bir ana oluşturabilirsiniz.* Bir şablondan bir ana oluşturmak için bkz: [bir şablondan bir uygulama hizmeti ortamı oluşturma][MakeASEfromTemplate].

    ![Kaynak grubu seçimi][6]

5. VNet ve konumunu seçin. Yeni bir VNet oluşturun veya varolan bir sanal ağ seçin: 

    * Yeni bir VNet seçerseniz, bir ad ve konum belirtebilirsiniz. Bu ana Linux uygulamaları barındırmak istiyorsanız, yalnızca bu 6 bölgeler şu anda desteklenen: **Batı ABD, Doğu ABD, Batı Avrupa, Kuzey Avrupa, Doğu Avustralya, Güneydoğu Asya.** 
    
    * Yeni sanal ağ adres aralığı 192.168.250.0/23 ve varsayılan adlı bir alt ağ vardır. Alt ağ 192.168.250.0/24 tanımlanır. Yalnızca Resource Manager Vnet'i seçebilirsiniz. **VIP türü** seçimi, ana doğrudan (harici) Internet üzerinden erişilebiliyorsa veya bir ILB kullanıyorsa belirler. Bu seçenekler hakkında daha fazla bilgi için bkz: [oluşturma ve kullanma uygulama hizmeti ortamı olan bir iç yük dengeleyici][MakeILBASE]. 

      * Seçerseniz **dış** için **VIP türü**, sistem oluşturulur ile IP tabanlı SSL amaçlar için kaç tane dış IP adreslerini seçebilirsiniz. 
    
      * Seçerseniz **dahili** için **VIP türü**, ana kullanan bir etki alanına belirtmeniz gerekir. Bir ana ortak veya özel adres aralıkları kullanan bir sanal ağ dağıtabilirsiniz. Bir VNet ortak adres aralığı ile kullanmak için sanal ağ önceden oluşturmanız gerekir. 
    
    * Varolan bir sanal ağ seçerseniz, ana oluşturulduğunda, yeni bir alt ağ oluşturulur. *Portalda önceden oluşturulmuş bir alt ağ kullanılamıyor. Resource Manager şablonunu kullanırsanız, var olan bir alt ağ ile bir ana oluşturabilirsiniz.* Bir şablondan bir ana oluşturmak için bkz: [bir şablondan bir uygulama hizmeti ortamı oluşturma][MakeASEfromTemplate].

## <a name="app-service-environment-v1"></a>App Service Ortamı v1 ##

Uygulama hizmeti ortamı'nı (ASEv1) ilk sürümü örneklerini oluşturabilirsiniz. Bu işlemi başlatmak için Market arama **uygulama hizmeti ortamı v1**. Tek başına ana oluşturduğunuz aynı şekilde ana oluşturun. Sona erdiğinde, ASEv1 iki ön uçlar ve iki çalışanları vardır. ASEv1 ile ön uç ve çalışanları yönetmeniz gerekir. Uygulama hizmeti planları oluşturduğunuzda otomatik olarak eklenir. Ön Uçları HTTP/HTTPS uç noktalar olarak davranmasına ve trafik çalışanlarına gönderin. Uygulamalarınızı barındırmak rolleri çalışanlardır. Ana oluşturduktan sonra ön uç ve çalışanları miktarını ayarlayabilirsiniz. 

ASEv1 hakkında daha fazla bilgi için bkz: [uygulama hizmeti ortamı v1 giriş][ASEv1Intro]. Ölçeklendirme ile ilgili daha fazla bilgi için bkz: yönetme ve ASEv1, izleme [bir uygulama hizmeti ortamını yapılandırma][ConfigureASEv1].

<!--Image references-->
[1]: ./media/how_to_create_an_external_app_service_environment/createexternalase-create.png
[2]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreate.png
[3]: ./media/how_to_create_an_external_app_service_environment/createexternalase-pricing.png
[4]: ./media/how_to_create_an_external_app_service_environment/createexternalase-embeddedcreate.png
[5]: ./media/how_to_create_an_external_app_service_environment/createexternalase-standalonecreate.png
[6]: ./media/how_to_create_an_external_app_service_environment/createexternalase-network.png
[7]: ./media/how_to_create_an_external_app_service_environment/createexternalase-createwafc.png
[8]: ./media/how_to_create_an_external_app_service_environment/createexternalase-aspcreatewafc.png
[8]: ./media/how_to_create_an_external_app_service_environment/createexternalase-configurecontainer.png



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
