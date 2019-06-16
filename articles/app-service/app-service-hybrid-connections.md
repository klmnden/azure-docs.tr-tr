---
title: Karma bağlantılar - Azure App Service | Microsoft Docs
description: Oluşturma ve birbirinden tamamen farklı ağlarda bulunan kaynaklara erişmek için karma bağlantıları kullanın
services: app-service
documentationcenter: ''
author: ccompy
manager: stefsch
editor: ''
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/06/2019
ms.author: ccompy
ms.custom: seodec18
ms.openlocfilehash: 4b125649dee51680625ac5a92b31bdc9f6830529
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67069560"
---
# <a name="azure-app-service-hybrid-connections"></a>Azure App Service karma bağlantılar #

Karma bağlantılar hem bir Azure hizmeti hem de Azure App service'taki bir özelliği olan. Bir hizmet olarak kullanır ve App Service'te kullanılan ötesinde yetenekler vardır. Karma bağlantılar ve App Service dışında kullanımları hakkında daha fazla bilgi için bkz: [Azure geçiş karma bağlantıları][HCService].

App Service içinde karma bağlantılar, diğer alt ağlardaki uygulama kaynaklarına erişmek için kullanılabilir. Bir uygulama uç noktası uygulamanızdan erişim sağlar. Uygulamanıza erişmek alternatif bir özelliği sağlamaz. App Service içinde kullanılan tek bir TCP konak ve bağlantı noktası bileşimi her karma bağlantı ilişkilendirir. Bu karma bağlantı uç noktasını tüm işletim sistemlerinde olabilir ve size sağlanan herhangi bir uygulama, TCP dinleme bağlantı noktası eriştiğiniz anlamına gelir. Karma bağlantılar özelliği, bilmiyorsanız veya uygulama protokolü nedir ve ne eriştiğiniz dikkat edin. Ayrıca, ağ erişimini yalnızca sağlamaktır.  


## <a name="how-it-works"></a>Nasıl çalışır? ##
Karma bağlantılar özelliği, Azure Service Bus geçişi iki giden çağrıları oluşur. Uygulamanızı App Service içinde çalıştığı bir kitaplıktan konak üzerindeki bir bağlantı yoktur. Service Bus geçişi karma Bağlantı Yöneticisi'nden (HCM) bağlantısı yoktur. HCM, erişmeye çalıştığınız kaynak barındırma ağ içinde dağıttığınız bir geçiş hizmetidir. 

İki birleştirilmiş bağlantılarında uygulamanızın HCM diğer tarafında bir sabit konak: bağlantı noktası bileşimi için bir TCP tünel vardır. Bağlantı, güvenlik ve kimlik doğrulama ve yetkilendirme için paylaşılan erişim imzası (SAS) anahtarları için TLS 1.2 kullanır.    

![Karma bağlantı üst düzey Akış Diyagramı][1]

Uygulamanızı yapılandırılmış bir karma bağlantı uç noktası ile eşleşen bir DNS istekte bulunduğunda giden TCP trafiğine karma bağlantı üzerinden yönlendirilir.  

> [!NOTE]
> Başka bir deyişle, her zaman, karma bağlantı için bir DNS adı kullanmayı denemeniz gerekir. Bir IP adresi uç nokta kullanıyorsa, bunun yerine, bazı istemci yazılımını bir DNS arama yapmaz.
>

### <a name="app-service-hybrid-connection-benefits"></a>App Service karma bağlantı avantajları ###

Karma bağlantılar özelliği avantajlar birçok dahil olmak üzere:

- Şirket içi sistemleri ve Hizmetleri güvenli bir şekilde erişebilir.
- Bu özellik, İnternet'ten erişilebilen bir uç nokta gerektirmez.
- Bu hızlı ve kolay ayarlama olur. 
- Her karma bağlantı, güvenlik için yararlı bir tek ana bilgisayar: bağlantı noktası bileşimi ile eşleşir.
- Güvenlik Duvarı boşluklarını normalde gerektirmez. Standart web bağlantı noktaları üzerinden giden tüm bağlantılardır.
- Özelliği, ağ düzeyinde olduğundan, uygulamanız tarafından kullanılan dil ve bitiş noktası tarafından kullanılan teknoloji bağımsızdır.
- Tek bir uygulama birden çok ağ erişim sağlamak için kullanılabilir. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Karma bağlantıları ile yapamayacağınız şeyler ###

Karma bağlantıları ile yapamayacağınız noktalar şunlardır:

- Bir sürücü bağlayın.
- UDP kullanın.
- Dinamik bağlantı noktaları, FTP Pasif modu veya genişletilmiş Pasif modu gibi kullanan erişim TCP tabanlı hizmetler.
- UDP gerektirebilir olduğundan, LDAP, destekler.
- Active Directory etki alanına katılmış bir App Service çalışanı yapamazsınız çünkü destekler.

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Ekleyin ve karma bağlantılar kullanarak uygulamanızı oluşturun ##

Karma bağlantı oluşturmak için Git [Azure portalında] [ portal] ve uygulamanızı seçin. Seçin **ağ** > **karma bağlantı uç noktalarınızı yapılandırın**. Uygulamanız için yapılandırılan karma bağlantıları burada görebilirsiniz.  

![Karma bağlantı ekran listesi][2]

Yeni Karma bağlantı eklemek için seçin **[+] karma bağlantıyı**.  Zaten oluşturduğunuz karma bağlantılar listesini görürsünüz. Bir veya daha fazlası uygulamanıza eklemek için ve ardından istediklerinizi seçin **seçili karma bağlantıyı Ekle**.  

![Karma bağlantı ekran portalı][3]

Yeni Karma bağlantı oluşturma isteyip istemediğinizi seçin **yeni karma bağlantı oluşturma**. Belirtin: 

- Karma bağlantı adı.
- Uç nokta konak adı.
- Uç nokta bağlantı noktası.
- Service Bus ad alanı kullanmak istiyorsunuz.

![Yeni Karma bağlantı iletişim kutusu oluştur ekran görüntüsü][4]

Her karma bağlantı, bir Service Bus ad alanına bağlıdır ve bir Azure bölgesinde her Service Bus ad alanı. Bir Service Bus ad alanı, uygulama ile aynı bölgede kaynaklanan ağ gecikmesini önlemek denemek önemlidir.

Uygulamanızdan karma bağlantınızı kaldırmak istiyorsanız, bunu sağ tıklayıp **Bağlantıyı Kes**.  

Karma bağlantı uygulamanıza eklediğinizde, Ayrıntılar üzerinde yalnızca seçerek görebilirsiniz. 

![Ekran görüntüsü, karma bağlantı ayrıntıları][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Azure geçişi Portalı'nda bir karma bağlantı oluşturma ###

Portal deneyimi açısından ek olarak uygulamanız içinde karma bağlantılar'dan Azure geçişi portalında oluşturabilirsiniz. Karma uygulama hizmeti tarafından kullanılacak bağlantı için bu gerekir:

* İstemci yetkilendirme gerektirir.
* Bir ana bilgisayar: bağlantı noktası bileşimi değeri içeren uç noktası adlı bir meta veri öğesi var.

## <a name="hybrid-connections-and-app-service-plans"></a>Karma bağlantılar ve App Service planları ##

App Service karma bağlantılar, yalnızca temel, standart, Premium ve yalıtılmış fiyatlandırma SKU'ları kullanılabilir. Fiyatlandırma planına bağlı sınırı yoktur.  

| Fiyatlandırma planı | Plana kullanılabilir karma bağlantılar sayısı |
|----|----|
| Temel | 5 |
| Standart | 25 |
| Premium | 200 |
| Yalıtılmış | 200 |

App Service planı kullanıcı Arabirimi kaç karma bağlantılar kullanıldığını gösterir ve hangi uygulamaların tarafından.  

![Ekran görüntüsü, App Service planı özellikleri][6]

Karma bağlantı ayrıntıları görmek için seçin. Uygulama görünümünde gördüğünüz tüm bilgileri görebilirsiniz. Ayrıca, kaç aynı planı uygulamalarında bu karma bağlantıyı kullanan görebilirsiniz.

Bir App Service planında kullanılan karma bağlantı uç noktası sayısına bir sınır yoktur. Kullanılan her karma bağlantı ve bu planı ancak uygulamaları herhangi bir sayıda arasında kullanılabilir. Örneğin, bir App Service planında beş ayrı uygulamalarında kullanılan tek bir karma bağlantı bir karma bağlantı sayılır.

### <a name="pricing"></a>Fiyatlandırma ###

Bir App Service planı SKU'su gereksinimi olan orada yanı sıra, karma bağlantıları kullanarak ek bir maliyeti yoktur. Karma bağlantı tarafından kullanılan her dinleyici için bir ücret yoktur. Dinleyici karma Bağlantı Yöneticisi ' dir. İki karma bağlantı yöneticileri tarafından desteklenen beş karma bağlantılar olsaydı, 10 dinleyicileri olacaktır. Daha fazla bilgi için [Service Bus fiyatlandırma][sbpricing].

## <a name="hybrid-connection-manager"></a>Karma Bağlantı Yöneticisi ##

Karma bağlantılar özelliği, karma bağlantı uç noktasını barındıran ağdaki bir geçiş aracısı gerektirir. Geçiş Aracı, karma Bağlantı Yöneticisi (HCM) olarak adlandırılır. HCM, uygulamanızda indirmesine izin [Azure portalında][portal]seçin **ağ** > **karmabağlantıuçnoktalarınızıyapılandırın**.  

Bu araç, Windows Server 2012 ve daha sonra çalışır. HCM, bir hizmet olarak çalışır ve Azure geçişi bağlantı noktası 443 üzerinden giden bağlar.  

HCM yükledikten sonra aracı için kullanıcı arabirimini kullanmak için HybridConnectionManagerUi.exe çalıştırabilirsiniz. Bu dosya karma Bağlantı Yöneticisi'ni yükleme dizinindedir. Windows 10'da yalnızca arayabilirsiniz *karma Bağlantı Yöneticisi kullanıcı Arabirimi* , arama kutusuna.  

![Karma Bağlantı Yöneticisi'nin ekran görüntüsü][7]

HCM UI başlattığınızda gördüğünüz ilk şey bu HCM örneği ile yapılandırılmış olan tüm karma bağlantıları listeleyen bir tablodur. İlk değişiklik yapmak istiyorsanız, Azure ile kimlik doğrulaması. 

Bir veya daha fazla karma bağlantılar için HCM eklemek için:

1. HCM kullanıcı arabirimini Başlat.
2. Seçin **başka bir karma bağlantı yapılandırma**.
![Ekran görüntüsü yeni karma bağlantılar yapılandırma][8]

1. Kullanılabilir aboneliklerinizle, karma bağlantılarınızı almak için Azure hesabınızla oturum açın. HCM ötesinde Azure hesabınızı kullanmaya devam etmez. 
1. Bir abonelik seçin.
1. HCM geçiş karma bağlantıları seçin.
![Karma bağlantılar'ın ekran görüntüsü][9]

1. **Kaydet**’i seçin.

Şimdi eklediğiniz karma bağlantılar da görebilirsiniz. Ayrıntıları görmek için yapılandırılmış karma bağlantı de seçebilirsiniz.

![Karma bağlantı ayrıntıları ekran görüntüsü][10]

Karma bağlantıları ile yapılandırılmış desteklemek için HCM gerektirir:

- Azure erişiminiz TCP bağlantı noktası 443 üzerinden.
- Karma bağlantı uç noktası TCP erişim.
- Uç nokta ana bilgisayarı ve Service Bus ad alanı DNS göz atmayı yeteneği.

> [!NOTE]
> Azure geçişi için bağlantı üzerinde Web yuvalarını kullanır. Bu özellik yalnızca Windows Server 2012 veya sonraki kullanılabilir. Bu nedenle HCM üzerindeki herhangi bir şey Windows Server 2012'den önceki desteklenmiyor.
>

### <a name="redundancy"></a>Yedeklilik ###

Birden çok karma bağlantılar her HCM destekler. Ayrıca, verilen herhangi bir karma bağlantı, birden çok HCMs tarafından desteklenebilir. Verilen herhangi bir uç noktası için yapılandırılan HCMs arasında trafiği yönlendirmek için varsayılan davranıştır. Yüksek kullanılabilirlik, karma bağlantılarda ağınızdan istiyorsanız, birden çok HCMs ayrı makineler üzerinde çalıştırın. Geçiş hizmeti tarafından HCMs trafiği dağıtmak için kullanılan yük dağıtım algoritmasını rastgele atamadır. 

### <a name="manually-add-a-hybrid-connection"></a>El ile bir karma Bağlantı Ekle ###

Dışında bir HCM örneği belirli bir karma bağlantı için ana bilgisayar için aboneliğinizi etkinleştirmek için bunları ile karma bağlantı için ağ geçidi bağlantı dizesi paylaşın. Ağ geçidi bağlantı dizesi karma bağlantı özelliklerinde gördüğünüz [Azure portalında][portal]. Bu dize kullanmayı tercih **el ile girin** HCM ve ağ geçidi bağlantı dizesini yapıştırın.

![El ile bir karma Bağlantı Ekle][11]

### <a name="upgrade"></a>Yükseltme ###

Düzenli güncelleştirmeler sorunları düzeltin ya da geliştirmeleri sağlamak için karma Bağlantı Yöneticisi için vardır. Yükseltmeler yayınlandığı zaman, popup HCM Arabiriminde gösterilir. Uygulama yükseltme değişiklikleri uygulamak ve HCM yeniden başlatın. 

## <a name="adding-a-hybrid-connection-to-your-app-programmatically"></a>Karma bağlantı programlı olarak uygulamanıza ekleme ##

Aşağıda belirtildiği API'leri, uygulamalarınızı bağlı karma bağlantılar doğrudan yönetmek için kullanılabilir. 

    /subscriptions/[subscription name]/resourceGroups/[resource group name]/providers/Microsoft.Web/sites/[app name]/hybridConnectionNamespaces/[relay namespace name]/relays/[hybrid connection name]?api-version=2016-08-01

Karma bağlantı ile ilişkili bir JSON nesnesi şu şekilde görünür:

    {
      "name": "[hybrid connection name]",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "[location]",
      "properties": {
        "serviceBusNamespace": "[namespace name]",
        "relayName": "[hybrid connection name]",
        "relayArmUri": "/subscriptions/[subscription id]/resourceGroups/[resource group name]/providers/Microsoft.Relay/namespaces/[namespace name]/hybridconnections/[hybrid connection name]",
        "hostName": "[endpoint host name]",
        "port": [port],
        "sendKeyName": "defaultSender",
        "sendKeyValue": "[send key]"
      }
    }

Bu bilgileri yollarından biri olan sayfasından edinebilirsiniz armclient [ARMClient] [ armclient] GitHub projesi. Uygulamanıza önceden mevcut olan karma bağlantı iliştirilirken bir örnek aşağıdadır. Yukarıdaki şemayı gibi her bir JSON dosyası oluşturun:

    {
      "name": "relay-demo-hc",
      "type": "Microsoft.Relay/Namespaces/HybridConnections",
      "location": "North Central US",
      "properties": {
        "serviceBusNamespace": "demo-relay",
        "relayName": "relay-demo-hc",
        "relayArmUri": "/subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myrelay-rg/providers/Microsoft.Relay/namespaces/demo-relay/hybridconnections/relay-demo-hc",
        "hostName": "my-wkstn.home",
        "port": 1433,
        "sendKeyName": "defaultSender",
        "sendKeyValue": "Th9is3is8a82lot93of3774stu887ff122235="
      }
    }

Bu API'yi kullanmak için gönderme anahtar ve geçiş kaynak kimliği gerekir. Filename hctest.json ile bilgilerinizi kaydettiyseniz, karma bağlantıyı uygulamanıza eklemek için şu komutu yürütün: 

    armclient login
    armclient put /subscriptions/ebcidic-asci-anna-nath-rak1111111/resourceGroups/myapp-rg/providers/Microsoft.Web/sites/myhcdemoapp/hybridConnectionNamespaces/demo-relay/relays/relay-demo-hc?api-version=2016-08-01 @hctest.json

## <a name="troubleshooting"></a>Sorun giderme ##

Durumu "Bağlandı", en az bir HCM, karma bağlantısı ile yapılandırıldığında ve Azure ulaşabildiğinden anlamına gelir. Karma bağlantı durumunun söyleyin değil **bağlı**, karma bağlantınız Azure erişimi olan herhangi bir HCM yapılandırılmadı.

Uç nokta DNS adı yerine IP adresi kullanarak belirtildiğinden, istemciler kendi uç noktama bağlanamıyorum birincil nedenidir. Uygulamanızı istenen uç noktası ulaşamıyor ve bir IP adresi kullandıysanız HCM çalıştığı konak üzerinde geçerli bir DNS adı kullanmaya geçiş yapın. Ayrıca DNS adını düzgün HCM çalıştığı konakta çözümler denetleyin. HCM karma bağlantı uç noktasını çalıştığı konak bağlantısı olduğunu doğrulayın.  

App Service'te **tcpping** komut satırı aracını Gelişmiş araçlar (Kudu) Konsolu'ndan çağrılan. Bu araç, bir TCP uç noktasına erişebilir ancak bu, bir karma bağlantı uç noktası erişiminiz varsa söylemez söyleyebilirsiniz. Konsolunda bir karma bağlantı uç noktası karşı aracını kullandığınızda, yalnızca bir ana bilgisayar: bağlantı noktası birleşimini kullanan demektir.  

Uç noktanız için bir komut satırı istemcisi varsa, uygulama konsol bağlantısı test edebilirsiniz. Örneğin, web sunucusu uç noktalarına erişimi curl kullanarak test edebilirsiniz.

## <a name="biztalk-hybrid-connections"></a>BizTalk Karma Bağlantıları ##

Bu özelliğin erken form BizTalk karma bağlantılar çağrıldı. Bu özellik, 31 Mayıs 2018'de, son yaşam oluştu ve işlem ceased. BizTalk karma bağlantılar, tüm uygulamalardan kaldırıldı ve portalı veya API erişilebilir değildir. Ardından bu eski bağlantılar karma Bağlantı Yöneticisi'nde yapılandırılan hala varsa, artık Üretilmiyor durumunu görmek ve son, yaşam deyimi altında görüntüler.

![HCM BizTalk karma bağlantıları][12]


<!--Image references-->
[1]: ./media/app-service-hybrid-connections/hybridconn-connectiondiagram.png
[2]: ./media/app-service-hybrid-connections/hybridconn-portal.png
[3]: ./media/app-service-hybrid-connections/hybridconn-addhc.png
[4]: ./media/app-service-hybrid-connections/hybridconn-createhc.png
[5]: ./media/app-service-hybrid-connections/hybridconn-properties.png
[6]: ./media/app-service-hybrid-connections/hybridconn-aspproperties.png
[7]: ./media/app-service-hybrid-connections/hybridconn-hcm.png
[8]: ./media/app-service-hybrid-connections/hybridconn-hcmadd.png
[9]: ./media/app-service-hybrid-connections/hybridconn-hcmadded.png
[10]: ./media/app-service-hybrid-connections/hybridconn-hcmdetails.png
[11]: ./media/app-service-hybrid-connections/hybridconn-manual.png
[12]: ./media/app-service-hybrid-connections/hybridconn-bt.png

<!--Links-->
[HCService]: https://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: https://portal.azure.com/
[oldhc]: https://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: https://azure.microsoft.com/pricing/details/service-bus/
[armclient]: https://github.com/projectkudu/ARMClient/
