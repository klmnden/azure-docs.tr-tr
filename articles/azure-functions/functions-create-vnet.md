---
title: Azure işlevleri, bir Azure sanal ağı ile tümleştirme
description: Bir işlev bir Azure sanal ağına bağlanma gösteren adım adım öğretici
services: functions
author: alexkarcher-msft
manager: jehollan
ms.service: azure-functions
ms.topic: article
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 749e211c9844f644e04d5135f99d71918d65b66b
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59680363"
---
# <a name="integrate-a-function-app-with-an-azure-virtual-network"></a>Bir işlev uygulaması, bir Azure sanal ağı ile tümleştirme

Bu öğreticide bir Azure sanal ağ kaynaklarına bağlanmak için Azure işlevleri kullanmayı gösterir.

Bu öğretici için size bir özel, olmayan-İnternet'ten erişilemediği, sanal ağ içinde bir VM'de bir WordPress sitesi dağıtacak. Daha sonra erişimi olan bir işlev hem internet hem de VNET dağıtacağız. Sanal ağ içinde dağıtılan bir WordPress sitesi kaynaklara erişmek için bu işlevi kullanacağız.

Sistem birlikte nasıl çalıştığı hakkında daha fazla bilgi için sorun giderme ve Gelişmiş Yapılandırma belgesine bakın [uygulamanızı bir Azure sanal ağ ile tümleştirme](https://docs.microsoft.com/azure/app-service/web-sites-integrate-with-vnet). Premium planı, azure işlevleri, web uygulamaları, aynı barındırma özellikleri sahip, bu nedenle tüm işlevleri ve sınırlamalar söz konusu belgedeki işlevleri için de geçerlidir.

## <a name="topology"></a>Topoloji

 ![VNet tümleştirmesi kullanıcı Arabirimi][1]

## <a name="creating-a-vm-inside-of-a-vnet"></a>Bir sanal ağ içinde bir VM oluşturma

Başlatmak için çalışan bir sanal ağ içinde Wordpress önceden yapılandırılmış bir VM oluşturacağız. 

Bir sanal ağ içinde dağıtılabilir ucuz kaynaklardan biri olduğundan VM üzerinde Wordpress seçildi. Bu senaryo, REST API'leri, App Service ortamları ve diğer Azure Hizmetleri gibi bir sanal ağ içindeki herhangi bir kaynak ile de çalışabilir unutmayın.

1. Azure portalına gidin
2. "Kaynak Oluştur" dikey penceresini açarak yeni bir kaynak ekleyin
3. Arama "[CentOS üzerinde WordPress LEMP7 Max Performance](https://jetware.io/appliances/jetware/wordpress4_lemp7-170526/profile?us=azure)" ve oluşturma dikey penceresini açın 
4. Oluşturma dikey penceresinde, sanal makine aşağıdaki bilgilerle yapılandırın:
    1. Öğreticinin sonunda daha kolay kaynakları temizleme yapmak bu VM için yeni bir kaynak grubu oluşturun. Ben kaynak grubum "İşlevi-VNET-Tutorial" adlı
    1. Sanal makine benzersiz bir ad verin. Benim "VNET-Wordpress" adlı
    1. Size en yakın bölgeyi seçin
    1. Boyut B1s seçin (1 vcpu, 1 GB bellek)
    1. Yönetici hesabı için parola kimlik doğrulaması'nı seçin ve benzersiz adı ve parola girin. Bu öğreticide, gidermeye ihtiyaç duyan sürece VM oturum gerekmez.
    
        ![VM temelleri sekmesi oluştur](./media/functions-create-vnet/create-vm-1.png)

1. Ağ sekmesine Taşı ve aşağıdaki bilgileri girin:
    1.  Yeni sanal ağ oluştur
    1.  İstenen özel adres aralığını ve alt ağ adres aralığı içinde girin. App Service planında kullanabileceğiniz kaç VM alt ağı boyutunu belirler. IP adresleme ve alt ağ oluşturma sizin için yeni olan ise, bir [temellerini anlatan belge](https://support.microsoft.com/en-us/help/164015/understanding-tcp-ip-addressing-and-subnetting-basics). I birkaç makaleyi okumanızı ve mantıklıdır kadar çevrimiçi birkaç video izlerken önerme olasılığınız şekilde, bu senaryoda, IP adresi ve alt ağları önemlidir. 
        1. Bu örnekte, ı 10.10.0.0/16 ağ 10.10.1.0/24 alt ağı ile kullanmak için kabul edildi. Fazladan ve bir /16 kullanmak seçtiğim alt 10.10.0.0/16 ağda hangi alt ağların kullanılabilir hesaplamak kolaydır.
        
        <img src="./media/functions-create-vnet/create-vm-2.png" width="700">

1. Ağ sekmesinde, "None." Genel IP'ye kümesine geri Bu, yalnızca sanal AĞA erişimi olan bir VM dağıtır.
       
    <img src="./media/functions-create-vnet/create-vm-2-1.png" width="700">

7. Bir VM oluşturun. Bu işlem yaklaşık 5 dakika sürer.
8. VM oluşturulduktan sonra ağ sekmesini ziyaret edin ve özel IP adresini daha sonra kullanmak üzere not alın. VM'ye bir genel IP yok.

    ![14]

Şimdi, tamamen sanal ağınızda dağıtılan bir wordpress sitesi olacaktır. Bu site, genel internet'ten erişilebilir değil.

## <a name="create-a-premium-plan-function-app"></a>Bir Premium planı işlev uygulaması oluşturma

Sonraki adım, bir premium planı içinde bir işlev uygulaması oluşturmaktır. Premium planı bir yeni bir adanmış App Service planının tüm ile sunucusuz ölçeklendirme getiren bir tekliftir. Tüketim planı işlev uygulamaları, VNet tümleştirmesi desteklemez.

[!INCLUDE [functions-premium-create](../../includes/functions-premium-create.md)]  

## <a name="connect-your-function-app-to-your-vnet"></a>İşlev uygulamanızı sanal AĞINIZA bağlama

WordPress sitesi barındırma dosyalarını, sanal ağ içinde bir sanal AĞA işlev uygulaması artık bağlanabilirsiniz.

1.  Önceki adımdan işlev uygulaması için Portalı'nda seçin **Platform özellikleri**, ardından **ağ**

    <img src="./media/functions-create-vnet/networking-0.png" width="850">

1.  Seçin **yapılandırmak için tıklayın** altında VNet tümleştirmesi

    ![Ağ özelliği durumu yapılandırma](./media/functions-create-vnet/Networking-1.png)

1. VNET tümleştirmesi sayfasında seçin **VNet Ekle (Önizleme)**

    <img src="./media/functions-create-vnet/networking-2.png" width="600"> 
    
1.  Kullanılacak işlev ve App Service planınız için yeni bir alt ağ oluşturun. Alt ağ boyutunu, app service planınız için eklediğiniz Vm'leri toplam sayısını kısıtlar unutmayın. Sanal AĞINIZA otomatik olarak işlevinizi, sanal makineden farklı bir alt ağda olduğunu bir önemi yoktur dolayısıyla, vnet'te alt ağlar arasındaki trafiği yönlendirir. 
    
    <img src="./media/functions-create-vnet/networking-3.png" width="600">

## <a name="create-a-function-that-accesses-a-resource-in-your-vnet"></a>Sanal ağınızdaki bir kaynağa erişirken bir işlev oluşturma

Biz bu dosyaya erişim ve kullanıcıya hizmet işlevi kullanmak için bunu işlev uygulaması artık sanal ağ ile wordpress sitemizi erişebilirsiniz. Her ikisi de ayarlanmış ve görselleştirmek kolay olduğundan bu örnek için bir wordpress sitesi API ve işlevi bir Proxy çağıran işlevler olarak kullanacağız. Dağıtılan bir sanal ağ içinde herhangi bir API'yi kolayca kullanabilir ve bu API, sanal ağ içinde dağıtılabilir kodu API ile başka bir işlevi çağırır. Bir SQL server, sanal ağ içinde dağıtılan bir mükemmel bir örnektir.

1. Portalda, işlev uygulaması önceki adımda açın.
1. Bir ara sunucu seçerek oluşturma **proxy'ler** > **+**

    <img src="./media/functions-create-vnet/new-proxy.png" width="250">

1. Ara sunucu adını ve rota yapılandırın. Benim bir yol olarak /plant seçtim.
1. Wordpress sitenizin IP önceden doldurun ve arka uç URL'si ayarlayın `http://{YOUR VM IP}/wp-content/themes/twentyseventeen/assets/images/header.jpg`
    
    <img src="./media/functions-create-vnet/create-proxy.png" width="900">

Şimdi, doğrudan yeni bir tarayıcı sekmesi yapıştırarak, arka uç URL'si ziyaret etmeye çalışırsanız, sayfa zaman aşımına gerekir. Wordpress sitenizin yalnızca sanal AĞINIZA ve değil internet'e bağlı olarak bu olması, beklenir. Proxy URL'nizi tarayıcıya yapıştırırsanız, VNET içinde Wordpress sitenizi çekilen güzel tesis resim görmeniz gerekir. 

İşlev uygulamanızı hem Internet hem de sanal AĞINIZA bağlanır. Proxy, genel internet üzerinden bir istek alma ve sanal ağa boyunca bu isteği iletmek için basit bir HTTP proxy olarak görev yapan. Proxy, ardından yanıt, genel internet üzerinden geçirir. 

<img src="./media/functions-create-vnet/plant.png" width="900">

## <a name="next-steps"></a>Sonraki Adımlar

Web Apps üzerinde PV2 planları gibi Premium planda çalışan işlevleri aynı App Service altyapının paylaşın. Bu, tüm belgelerin Web uygulamaları için geçerli olduğunu Premium planı işlevlerinizi anlamına gelir.

1. [Burada işlevleri içindeki ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)
1. [Burada SSS ağ işlevleri okur](./functions-networking-faq.md)
1. [Azure'da sanal ağlar hakkında daha fazla bilgi edinin](../virtual-network/virtual-networks-overview.md)
1. [Daha fazla ağ özellikleri ve App Service ortamları ile denetimi etkinleştir](../app-service/environment/intro.md)
1. [Karma bağlantıları kullanarak güvenlik duvarı değişikliğe gerek kalmadan tek şirket içi kaynaklara bağlanma](../app-service/app-service-hybrid-connections.md)
1. [İşlev proxy'leri hakkında daha fazla bilgi edinin](./functions-proxies.md)

<!--Image references-->
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
