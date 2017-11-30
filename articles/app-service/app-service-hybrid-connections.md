---
title: "Azure uygulama hizmeti karma bağlantılar | Microsoft Docs"
description: "Oluşturma ve farklı ağlarda kaynaklara erişmek için karma bağlantılar kullanın"
services: app-service
documentationcenter: 
author: ccompy
manager: stefsch
editor: 
ms.assetid: 66774bde-13f5-45d0-9a70-4e9536a4f619
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: ccompy
ms.openlocfilehash: 677642e4e97523ed71ff5857ae27263743dca535
ms.sourcegitcommit: cfd1ea99922329b3d5fab26b71ca2882df33f6c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/30/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Azure uygulama hizmeti karma bağlantılar #

Karma bağlantılar Azure hizmetinde ve Azure App Service'te bir özellik değil. Bir hizmet olarak kullanır ve App Service içinde kullanılan ötesinde özellikler vardır. Karma bağlantılar ve uygulama hizmeti dışında kullanımı hakkında daha fazla bilgi için bkz: [Azure geçişi karma bağlantılar][HCService].

App Service içinde karma bağlantılar diğer ağlara uygulama kaynaklara erişim için kullanılabilir. Bir uygulama uç nokta uygulamanızdan erişmenizi sağlar. Uygulamanızı erişmek bir alternatif özelliği sağlamaz. App Service içinde kullanılan gibi her bir karma bağlantı tek bir TCP ana bilgisayarı ve bağlantı noktası bileşimi hatalarla ilintilidir. Bu karma bağlantı uç noktasının herhangi bir işletim sisteminde olabilir ve belirttiğiniz herhangi bir uygulama bir TCP dinleme bağlantı noktası erişme anlamına gelir. Karma bağlantılar özelliği bilmiyorsanız veya uygulama protokolü nedir veya erişmeye çalıştığınız dikkat edin. Ayrıca, ağ erişimi yalnızca sağlamaktır.  


## <a name="how-it-works"></a>Nasıl çalışır? ##
Karma bağlantılar Özelliği Azure Service Bus geçişi iki giden çağrıları oluşur. Uygulamanızı App Service içinde çalıştığı bir kitaplıktan ana bilgisayardaki bir bağlantı yok. Service Bus geçişi karma Bağlantı Yöneticisi'nden (HCM) bağlantısı yoktur. HCM, erişmeye çalıştığınız kaynak barındırma ağ içinde dağıttığınız bir geçiş hizmetidir. 

İki birleştirilmiş bağlantıları uygulamanızı HCM diğer tarafta bir sabit ana: bağlantı noktası bileşimi için bir TCP tünel vardır. Bağlantı TLS 1.2 güvenlik ve kimlik doğrulama ve yetkilendirme için paylaşılan erişim imzası (SAS) anahtarları kullanır.    

![Karma bağlantısının üst düzey Akış Diyagramı][1]

Uygulamanızı yapılandırılmış bir karma bağlantı uç noktayla eşleşen bir DNS isteğinde bulunduğunda, giden TCP trafiği karma bağlantı üzerinden yönlendirilir.  

> [!NOTE]
> Başka bir deyişle, her zaman karma bağlantınız için bir DNS adı kullanmayı denemeniz gerekir. Uç nokta bir IP adresi yerine kullanıyorsa, bazı istemci yazılımı bir DNS araması yapmaz.
>
>

Karma bağlantılar özelliği iki tür vardır: Service Bus geçişi kapsamında bir hizmeti ve eski Azure BizTalk Services karma bağlantılar sunulan karma bağlantılar. İkinci olan Klasik karma bağlantılar portalda gösteriyor. Bu makalenin sonraki bölümlerinde bunlarla ilgili daha fazla bilgi bulunmaktadır.

### <a name="app-service-hybrid-connection-benefits"></a>Uygulama hizmeti karma bağlantısı avantajları ###

Karma bağlantılar özelliği için avantajları vardır dahil olmak üzere:

- Şirket içi sistemlerde ve hizmetlerde güvenli bir şekilde erişebilir.
- Bu özellik, internet'ten erişilebilen bir uç nokta gerektirmez.
- Hızlı ve kolay ayarlayın. 
- Her karma bağlantı için güvenlik yararlı, tek konak: bağlantı noktası birleşimi ile eşleşir.
- Güvenlik Duvarı delik normalde gerektirmez. Standart web bağlantı noktaları üzerinden giden tüm bağlantılardır.
- Bu özellik ağ düzeyi olduğundan, uygulamanız tarafından kullanılan dil ve bitiş noktası tarafından kullanılan teknoloji bağımsızdır.
- Tek bir uygulama birden çok ağ erişim sağlamak için kullanılabilir. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Karma bağlantılar ile yapamayacağı noktalar ###

Karma bağlantılar dahil olmak üzere, yapamayacağınız birkaç şey vardır:

- Bir sürücü bağlama.
- UDP kullanma.
- FTP Pasif modu veya genişletilmiş Pasif modu gibi dinamik bağlantı noktaları kullanan TCP tabanlı hizmetler erişme.
- Bazen UDP gerektirdiğinden LDAP, destekleme.
- Active Directory destekleme.

## <a name="add-and-create-hybrid-connections-in-your-app"></a>Ekleme ve uygulamanızda karma bağlantıları oluşturma ##

Uygulama hizmeti uygulamanızı Azure portalında veya Azure geçiş Azure portalında aracılığıyla karma bağlantılar oluşturabilirsiniz. Karma bağlantı ile kullanmak istediğiniz uygulama hizmeti uygulaması aracılığıyla karma bağlantılar oluşturmanızı öneririz. Karma bir bağlantı oluşturmak için şu adrese gidin [Azure portal] [ portal] ve uygulamanızı seçin. Seçin **ağ** > **karma bağlantı uç noktalarınızı yapılandırın**. Buradan, uygulamanız için yapılandırılmış karma bağlantılar görebilirsiniz.  

![Karma bağlantı ekran listesi][2]

Yeni bir karma bağlantı eklemek için seçin **karma Bağlantı Ekle**.  Önceden oluşturduğunuz karma bağlantılar listesini görürsünüz. Bir veya daha fazlası uygulamanıza eklemek için ve ardından olanları seçin **Ekle seçili karma bağlantı**.  

![Karma bağlantının ekran portalı][3]

Yeni bir karma bağlantı oluşturmak isteyip istemediğinizi seçin **yeni karma bağlantı oluşturmak**. Belirtin: 

- Uç nokta adı.
- Uç noktası ana bilgisayar adı.
- Uç nokta bağlantı noktası.
- Kullanmak istediğiniz hizmet veri yolu ad alanı.

![Yeni Karma bağlantı iletişim kutusu, ekran oluşturma][4]

Her karma bağlantı için bir hizmet veri yolu ad alanı bağlıdır ve bir Azure bölgesinde her hizmet veri yolu ad alanıdır. Kopyaladığınızda ağ gecikmesi önlemek için uygulamanız ile aynı bölgede bir hizmet veri yolu ad alanı kullanmaya çalıştığınızda önemlidir.

Karma bağlantınız uygulamanızdan kaldırmak istiyorsanız, sağ tıklatın ve seçin **Bağlantıyı Kes**.  

Karma bağlantı, uygulamanızın eklendiğinde, ayrıntıları üzerinde yalnızca seçerek görebilirsiniz. 

![Ekran görüntüsü, karma bağlantıları ayrıntıları][5]

### <a name="create-a-hybrid-connection-in-the-azure-relay-portal"></a>Karma bağlantı Azure geçişi Portalı'nda oluşturma ###

Portal deneyimlerden ek olarak, uygulamanızın içinde Azure geçişi Portalı'ndan gelen karma bağlantıları oluşturabilirsiniz. Karma bir App Service tarafından kullanılacak bağlantı için aşağıdakileri yapmalıdır:

* İstemci kimlik doğrulaması gerektirir.
* Bir ana bilgisayar: bağlantı noktası birleşimi değer olarak içeren uç nokta adlı bir meta veri öğesi var.

## <a name="hybrid-connections-and-app-service-plans"></a>Karma bağlantılar ve uygulama hizmeti planları ##

Karma bağlantılar özellik, yalnızca temel, standart, Premium ve SKU'ları fiyatlandırması Isolated kullanılabilir. Fiyatlandırma plana bağlı sınırları vardır.  

> [!NOTE] 
> Yeni Karma bağlantıları Azure geçişte göre yalnızca oluşturabilirsiniz. Yeni BizTalk karma bağlantılar oluşturulamıyor.
>

| plan fiyatlandırması | Karma bağlantılar planda kullanılabilir sayısı |
|----|----|
| Temel | 5 |
| Standart | 25 |
| Premium | 200 |
| Yalıtılmış | 200 |

Uygulama hizmeti planı, kaç tane karma bağlantılar kullanılan gösterdiğine dikkat edin ve hangi uygulamaların tarafından.  

![Uygulama hizmeti ekran plan özellikleri][6]

Ayrıntıları görmek için karma bağlantıyı seçin. Uygulama Sergi gördüğünüz tüm bilgileri görebilirsiniz. Aynı planında kaç diğer uygulamalar bu karma bağlantıyı kullanarak da görebilirsiniz.

Bir uygulama hizmeti planında kullanılabilir karma bağlantı uç sayısına bir sınır yoktur. Kullanıldığında, her karma bağlantı ancak, bu planında uygulamaları herhangi bir sayıda kullanılabilir. Örneğin, bir uygulama hizmeti planında beş ayrı uygulamalarında kullanılan tek bir karma bağlantı bir karma bağlantı olarak sayılır.

Karma bağlantılar kullanarak ek bir maliyet yoktur. Ayrıntılar için bkz [Service Bus fiyatlandırma][sbpricing].

## <a name="hybrid-connection-manager"></a>Karma Bağlantı Yöneticisi ##

Karma bağlantılar özellik geçiş aracısı, karma bağlantı uç noktasını barındıran ağdaki gerektirir. Bu geçiş aracısı karma Bağlantı Yöneticisi (HCM) adı verilir. HCM, uygulamanızda indirebilir [Azure portal][portal]seçin **ağ** > **karmabağlantıuçnoktalarınızıyapılandırın**.  

Bu araç, Windows Server 2012 ve daha sonra çalışır. Yüklendiğinde, HCM yapılandırılmış uç noktalarda temel Service Bus geçişi bağlandığı bir hizmet olarak çalışır. HCM bağlantılarından bağlantı noktası 443 üzerinden Azure'a giden.    

HCM yükledikten sonra kullanıcı Arabirimi aracını kullanmak için HybridConnectionManagerUi.exe çalıştırabilirsiniz. Karma Bağlantı Yöneticisi'ni yükleme dizininde dosyasıdır. Windows 10'da yalnızca arayabilirsiniz *karma Bağlantı Yöneticisi kullanıcı Arabirimi* , arama kutusuna.  

![Karma Bağlantı Yöneticisi'nin ekran görüntüsü][7]

HCM UI başlattığınızda gördüğünüz ilk HCM'ın bu örneğinin yapılandırılmış olan tüm karma bağlantılar listeleyen bir tablo şeydir. Herhangi bir değişiklik yapmak istiyorsanız, ilk Azure kimlik doğrulaması. 

Bir veya daha fazla karma bağlantılar, HCM eklemek için:

1. HCM UI başlatın.
1. Seçin **başka bir karma bağlantıyı yapılandırma**.
![Yeni karma bağlantılar yapılandırma ekran görüntüsü][8]

1. Azure hesabınızla oturum açın.
1. Bir abonelik seçin.
1. Geçiş için HCM istediğiniz karma bağlantılar'ı seçin.
![Karma bağlantılar ekran görüntüsü][9]

1. **Kaydet**’i seçin.

Eklediğiniz karma bağlantılar artık görebilirsiniz. Ayrıntıları görmek için yapılandırılmış karma bağlantı öğesini de seçebilirsiniz.

![Karma bağlantı ayrıntılarının ekran görüntüsü][10]

Karma ile yapılandırılmış bağlantılarını desteklemek için HCM gerektirir:

- 80 ve 443 numaralı bağlantı noktaları üzerinden TCP erişim Azure.
- Karma bağlantı uç noktasının TCP erişim.
- Uç noktası ana bilgisayar ve hizmet veri yolu ad DNS göz atmayı yeteneği.

Yeni karma bağlantılar ve BizTalk karma bağlantılar HCM destekler.

> [!NOTE]
> Azure geçiş bağlantısı Web yuvalarını kullanır. Bu özellik yalnızca Windows Server 2012 veya sonraki kullanılabilir. Bu nedenle HCM hiçbir şey Windows Server 2012'den önceki desteklenmiyor.
>

### <a name="redundancy"></a>Yedeklilik ###

Her HCM, birden çok karma bağlantılar destekleyebilir. Ayrıca, birden çok HCMs tarafından verilen tüm karma bağlantı desteklenebilir. Verilen herhangi bir uç nokta için yapılandırılmış HCMs arasında trafiği yönlendirmek için varsayılan davranıştır. Yüksek kullanılabilirlik, karma bağlantılar ağınızdan istiyorsanız, birden çok HCMs ayrı makinelerde çalıştırın. 

### <a name="manually-add-a-hybrid-connection"></a>El ile karma Bağlantı Ekle ###

Birisi dışında HCM örneği belirli bir karma bağlantı için ana bilgisayar için aboneliğinizi etkinleştirmek için bunları ile karma bağlantı için ağ geçidi bağlantı dizesi paylaşır. Bu özellikler karma bir bağlantı için gördüğünüz [Azure portal][portal]. Bu dizeyi kullanmak için **el ile girin** HCM ve ağ geçidi bağlantı dizesini yapıştırın.


## <a name="troubleshooting"></a>Sorun giderme ##

"Bağlı" durumunu en az bir HCM Bu karma bağlantı ile yapılandırılmış ve Azure ulaşamadığını anlamına gelir. Karma bağlantı durumunun söyleyin değil **bağlı**, karma bağlantınız Azure erişimi HCM yapılandırılmamış.

Uç nokta bir DNS adı yerine bir IP adresi kullanarak belirtilmediğinden, istemciler kendi uç noktasına bağlanamıyor birincil nedenidir. Uygulamanızı istenen endpoint ulaşamıyor ve bir IP adresi kullandıysanız HCM'ın çalıştırıldığı konak üzerinde geçerli bir DNS adı kullanmaya geçiş yapın. Ayrıca DNS adını düzgün HCM çalıştığı ana bilgisayarda çözer denetleyin. HCM karma bağlantı uç noktasına çalıştığı konaktan bağlantısı olduğunu doğrulayın.  

App Service içinde tcpping Aracı'nı Gelişmiş araçlar (Kudu) konsolundan çağrılabilir. Bu araç TCP uç noktası erişimi ancak bunu, karma bağlantı uç noktasının erişiminiz varsa söylemez anlayabilirsiniz. Karma bağlantı uç noktasının karşı konsolunda Aracı'nı kullandığınızda, yalnızca bir ana bilgisayar: bağlantı noktası bileşimini kullanır onaylayan.  

## <a name="biztalk-hybrid-connections"></a>BizTalk Karma Bağlantıları ##

Eski BizTalk karma bağlantılar özelliği için yeni BizTalk karma bağlantı kapatıldı. Mevcut BizTalk karma bağlantılarınızı uygulamalarınızı ile kullanmaya devam edebilirsiniz, ancak Azure geçişi kullanan yeni karma bağlantılar için geçirmeniz gerekir. Yeni hizmet BizTalk sürüm üzerinden avantajları arasında şunlardır:

- Hiçbir ek BizTalk hesabı gereklidir.
- TLS sürüm 1.2 sürümü 1.0 yerine hazır.
- İletişim 80 ve 443 numaralı bağlantı noktaları üzerinden ve IP adreslerini yerine Azure ve ek bağlantı noktası aralığını ulaşmak için bir DNS adı kullanır.  

Mevcut bir BizTalk karma bağlantıyı uygulamanıza eklemek için uygulamanızı gidin [Azure portal][portal]seçip **ağ** > **Yapılandır Karma bağlantı uç noktalarınızı**. Klasik karma bağlantılar tablosunu seçin **Klasik karma Bağlantı Ekle**. Ardından, BizTalk karma bağlantılar listesini görebilirsiniz.  


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

<!--Links-->
[HCService]: http://docs.microsoft.com/azure/service-bus-relay/relay-hybrid-connections-protocol/
[portal]: http://portal.azure.com/
[oldhc]: http://docs.microsoft.com/azure/biztalk-services/integration-hybrid-connection-overview/
[sbpricing]: http://azure.microsoft.com/pricing/details/service-bus/
