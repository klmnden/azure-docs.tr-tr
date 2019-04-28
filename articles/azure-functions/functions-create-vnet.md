---
title: Azure işlevleri, bir Azure sanal ağı ile tümleştirin
description: Bir işlev bir Azure sanal ağına bağlama gösteren adım adım öğretici
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: article
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 96ab479d3373eb6e575a00898f7007a4df252e39
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62125686"
---
# <a name="integrate-a-function-app-with-an-azure-virtual-network"></a>Bir işlev uygulaması, bir Azure sanal ağı ile tümleştirme

Bu öğreticide bir Azure sanal ağındaki kaynaklara bağlanmak için Azure işlevleri kullanmayı gösterir.

Bu öğretici için şu internet'ten erişilebilir değil bir sanal ağdaki bir VM'de bir WordPress sitesi dağıtacaksınız. Biz ardından erişimi olan bir işlev hem internet hem de sanal ağ için dağıtacaksınız. Sanal ağ içinde dağıtılan bir WordPress sitesi kaynaklara erişmek için bu işlevi kullanacağız.

Sorun giderme ve Gelişmiş Yapılandırma sistemi birlikte nasıl çalıştığı hakkında daha fazla bilgi için bkz. [uygulamanızı bir Azure sanal ağı ile tümleştirme](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet). Tüm işlevleri ve sınırlamalar söz konusu makaledeki işlevler uygulamak için Premium planı, azure işlevleri web apps ile aynı barındırma özellikleri vardır.

## <a name="topology"></a>Topoloji

 ![Sanal ağ tümleştirmesi için kullanıcı Arabirimi][1]

## <a name="create-a-vm-inside-a-virtual-network"></a>Bir sanal ağ içinde bir VM oluşturma

Başlamak için bir sanal ağ içinde çalıştırılan WordPress önceden yapılandırılmış bir VM oluşturacağız. 

Sanal ağ içinde dağıtılabilir ucuz kaynaklardan biri olduğundan VM üzerinde WordPress seçtik. Bu senaryo ayrıca REST API'leri, App Service ortamları ve diğer Azure Hizmetleri gibi bir sanal ağdaki herhangi bir kaynak ile çalışabilir unutmayın.

1. Azure portalına gidin.
2. Açarak yeni bir kaynak ekleyin **kaynak Oluştur** dikey penceresi.
3. Arama "[CentOS üzerinde WordPress LEMP7 Max Performance](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure)" ve oluşturma dikey penceresini açın. 
4. Üzerinde **Temelleri** sekmesinde, aşağıdaki bilgileri içeren bir VM yapılandırın:
    1. Öğreticinin sonunda daha kolay kaynakları temizleme yapmak bu VM için yeni bir kaynak grubu oluşturun. Örneğin, burada "İşlevi-VNET-Tutorial" kullanıyoruz.
    1. Sanal makine benzersiz bir ad verin. Örnek olarak "VNET-Wordpress" kullanıyoruz.
    1. Size en yakın bölgeyi seçin.
    1. Boyut B1s seçin (1 vCPU, 1 GB bellek).
    1. Yönetici hesabı için parola kimlik doğrulaması'nı seçin ve benzersiz adı ve parola girin. Bu öğreticide, gidermeye ihtiyaç duyan sürece VM oturum gerekmez.
    
        ![Bir VM oluşturmak için sekmesinde temelleri](./media/functions-create-vnet/create-vm-1.png)

1. Taşı **ağ** sekmesini ve aşağıdaki bilgileri girin:
    1.  Yeni bir sanal ağ oluşturun.
    1.  Özel bir adres aralığı ve bir alt ağ adres aralığı içinde girin. App Service planında kullanabileceğiniz kaç VM alt ağı boyutunu belirler. IP adresleme ve alt ağ oluşturma sizin için yeni başladıysanız var. bir [temel kavramları kapsar belge](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). Bu nedenle birkaç makalelerini okuyun ve mantıklıdır kadar birkaç çevrimiçi videoları öneririz, bu senaryoda, IP adresi ve alt ağları önemlidir. 
    
        Bu örnekte, biz 10.10.0.0/16 ağ 10.10.1.0/24 alt ağı ile kullanmayı edilmesiyle. Biz açıdan ve bir /16 kullanarak alt ağ 10.10.0.0/16 ağda hangi alt ağların kullanılabilir hesaplamak kolaydır.
        
        <img src="./media/functions-create-vnet/create-vm-2.png" width="700">

1. Yeniden **ağ** sekmesinde, genel IP kümesine **hiçbiri**. Bu adım yalnızca sanal ağa erişimi olan bir VM dağıtır.
       
    <img src="./media/functions-create-vnet/create-vm-2-1.png" width="700">

7. Bir VM oluşturun. İşlem yaklaşık 5 dakika sürer.
8. VM oluşturulduktan sonra Git, **ağ** sekme ve özel IP adresini daha sonra kullanmak üzere not alın. VM'ye bir genel IP yok.

    ![14]

Artık tamamen sanal ağınızda dağıtılan bir WordPress sitesi var. Bu site, genel internet'ten erişilebilir değil.

## <a name="create-a-function-app-in-a-premium-plan"></a>Bir Premium planında bir işlev uygulaması oluşturma

Sonraki adım, bir Premium planı içinde bir işlev uygulaması oluşturmaktır. Bir Premium planı, ayrılmış bir App Service planının tüm avantajları ile sunucusuz ölçeklendirme getirir. Tüketim planı oluşturulan işlev uygulamaları, sanal ağ tümleştirmesi desteklemez.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

## <a name="connect-your-function-app-to-your-virtual-network"></a>İşlev uygulamanızı sanal ağınıza bağlama

WordPress sitesi barındırma dosyalarından sanal ağınızdaki bir sanal ağa işlev uygulaması şimdi bağlanabilirsiniz.

1.  Önceki adımdan işlev uygulaması için portalında **Platform özellikleri**. Ardından **ağ**.

    <img src="./media/functions-create-vnet/networking-0.png" width="850">

1.  Seçin **yapılandırmak için buraya tıklayın** altında **VNet tümleştirmesi**.

    ![Bir ağ özelliğini yapılandırma durumu](./media/functions-create-vnet/Networking-1.png)

1. Sanal ağ tümleştirmesi sayfasında **VNet Ekle (Önizleme)**.

    <img src="./media/functions-create-vnet/networking-2.png" width="600"> 
    
1.  İşlev ve kullanmak için App Service planı için yeni bir alt ağ oluşturun. Alt ağ boyutunu, App Service planınız için eklediğiniz VM toplam sayısını kısıtlar unutmayın. Sanal ağınıza otomatik olarak işlevinizi, sanal makineden farklı bir alt ağda olduğunu fark etmez şekilde, sanal ağ içindeki alt ağlar arasındaki trafiği yönlendirir. 
    
    <img src="./media/functions-create-vnet/networking-3.png" width="600">

## <a name="create-a-function-that-accesses-a-resource-in-your-virtual-network"></a>Sanal ağınızda bir kaynağa erişirken bir işlev oluşturma

İşlev uygulaması, artık sanal ağ ile WordPress sitemizi erişebilirsiniz. Bu nedenle bu dosyaya erişim ve kullanıcıya hizmet için işlev kullanılacak ekleyeceğiz. Hem ayarlamak ve görselleştirmek kolay olduğundan bu örnek için bir WordPress sitesi API ve bir proxy çağıran işlevin kullanacağız. 

Dağıtılan bir sanal ağ içindeki herhangi bir API, bir kolayca kullanabilirsiniz. Sanal ağınızda dağıtılan API API çağrıları yapan kod ile başka bir işlev de kullanabilirsiniz. Sanal ağınızda dağıtılan bir SQL Server örneği mükemmel bir örnektir.

1. Portalda, önceki adımdaki işlev uygulamasını açın.
1. Bir ara sunucu seçerek oluşturma **proxy'leri** > **+**.

    <img src="./media/functions-create-vnet/new-proxy.png" width="250">

1. Ara sunucu adını ve rota yapılandırın. Bu örnek, kullanır "/ bitki" bir yolu olarak.
1. WordPress sitenizin IP önceden doldurun ve ayarlama **arka uç URL'si** için `http://{YOUR VM IP}/wp-content/themes/twentyseventeen/assets/images/header.jpg`
    
    <img src="./media/functions-create-vnet/create-proxy.png" width="900">

Şimdi, doğrudan yeni bir tarayıcı sekmesi yapıştırarak, arka uç URL'sini ziyaret denerseniz, sayfa zaman aşımına gerekir. WordPress sitenizin yalnızca, sanal ağınızın ve değil internet'e bağlı olduğundan budur. Proxy URL'nizi tarayıcıya yapıştırın, sanal ağınızda tesis resme (WordPress sitenizi çekilen) görmeniz gerekir. 

İşlev uygulamanızı hem internet hem de sanal ağınıza bağlı. Proxy, genel internet üzerinden bir istek alma ve sanal ağa boyunca bu isteği iletmek için basit bir HTTP proxy olarak görev yapan. Proxy, ardından yanıt, genel internet üzerinden geçirir. 

<img src="./media/functions-create-vnet/plant.png" width="900">

## <a name="next-steps"></a>Sonraki adımlar

Premium planda çalışan işlevleri aynı App Service altyapının PremiumV2 planları üzerinde web apps'te olarak paylaşın. Web apps için tüm belgeleri Premium planı işlevleriniz için geçerlidir.

* [İşlevler içindeki ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)
* [SSS ağ işlevleri okur](./functions-networking-faq.md)
* [Azure'daki sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
* [Daha fazla ağ özellikleri ve App Service ortamları ile denetimi etkinleştir](../app-service/environment/intro.md)
* [Karma bağlantılar kullanarak güvenlik duvarı değişiklikleri olmadan tek şirket içi kaynaklara bağlanma](../app-service/app-service-hybrid-connections.md)
* [İşlev proxy'leri hakkında daha fazla bilgi edinin](./functions-proxies.md)

<!--Image references -->
[1]: ./media/functions-create-vnet/topology.png
[2]: ./media/functions-create-vnet/create-function-app.png
[3]: ./media/functions-create-vnet/create-app-service-plan.png
[4]: ./media/functions-create-vnet/configure-vnet.png
[5]: ./media/functions-create-vnet/create-vm-1.png
[6]: ./media/functions-create-vnet/create-vm-2.png
[7]: ./media/functions-create-vnet/create-vm-2-1.png
[8]: ./media/functions-create-vnet/networking-1.png
[9]: ./media/functions-create-vnet/networking-2.png
[10]: ./media/functions-create-vnet/networking-3.png
[11]: ./media/functions-create-vnet/new-proxy.png
[12]: ./media/functions-create-vnet/create-proxy.png
[14]: ./media/functions-create-vnet/vm-networking.png
