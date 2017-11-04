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
ms.openlocfilehash: f0b148cb9c304c54b47be9601740e7634462d59b
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Azure uygulama hizmeti karma bağlantılar #

## <a name="overview"></a>Genel Bakış ##

Karma bağlantılar, hem Azure hizmetinde, hem de Azure App Service'te bir özellik değil.  Bir hizmet olarak kullanır ve Azure App Service'te işlevden ötesinde özellikler vardır.  Karma bağlantılar ve burada başlayabileceğini Azure App Service dışında kullanımı hakkında daha fazla bilgi edinmek için [Azure geçişi karma bağlantılar][HCService]

Azure App Service içinde karma bağlantılar diğer ağlara uygulama kaynaklara erişim için kullanılabilir. Bir uygulama uç nokta UYGULAMANIZDAN erişmenizi sağlar.  Uygulamanızı erişmek bir alternatif özelliği sağlamaz.  App Service içinde kullanılan gibi her bir karma bağlantı tek bir TCP ana bilgisayarı ve bağlantı noktası bileşimi hatalarla ilintilidir.  Bu karma bağlantı uç noktasının herhangi bir işletim sisteminde olabilir ve belirttiğiniz herhangi bir uygulama bir TCP dinleme bağlantı noktası basarsa anlamına gelir. Karma bağlantılar bilmiyorsanız veya uygulama protokolü nedir veya erişmeye çalıştığınız dikkat edin.  Yalnızca ağ erişim olanağı sunuyoruz.  


## <a name="how-it-works"></a>Nasıl çalışır? ##
Karma bağlantılar özelliği Service Bus geçişi iki giden çağrıları oluşur.  Burada, uygulamanızın App Service içinde çalışan ve Service Bus geçişi karma Bağlantı Yöneticisi'nden (HCM) bağlantı yoktur ana bilgisayardaki bir kitaplıktan bir bağlantı yok.  HCM, erişmeye çalıştığınız kaynak barındırma ağ içinde dağıttığınız bir geçiş hizmetidir. 

İki birleştirilmiş bağlantıları uygulamanızı HCM diğer tarafta bir sabit ana: bağlantı noktası bileşimi için bir TCP tünel vardır.  Bağlantı TLS 1.2 güvenlik ve kimlik doğrulama/yetkilendirme için SAS anahtarları kullanır.    

![Karma bağlantısının üst düzey akış][1]

Uygulamanızı yapılandırılmış bir karma bağlantı uç noktayla eşleşen bir DNS isteğinde bulunduğunda, giden TCP trafiği karma bağlantı üzerinden yönlendirilir.  

> [!NOTE]
> Başka bir deyişle, her zaman karma bağlantınız için bir DNS adı kullanmayı denemeniz gerekir.  Uç nokta bir IP adresi yerine kullanıyorsa, bazı istemci yazılımı bir DNS araması yapmaz.
>
>

Karma bağlantılar iki tür vardır: Azure geçişi ve eski BizTalk karma bağlantılar altında bir hizmet olarak sunulan yeni karma bağlantılar.  Eski BizTalk karma bağlantılar olarak Klasik karma bağlantılar portalda denir.  Bu belgede daha sonra bunları hakkında daha fazla bilgi yoktur.

### <a name="app-service-hybrid-connection-benefits"></a>Uygulama hizmeti karma bağlantısı avantajları ###

Karma bağlantılar özelliği için avantajları vardır dahil olmak üzere:

- Uygulamaları güvenli bir şekilde şirket içi sistemlerde ve hizmetlerde güvenli şekilde erişebilir.
- Bu özellik, Internet'ten erişilebilen bir uç nokta gerektirmez.
- Hızlı ve kolay ayarlayın. 
- Her bir karma bağlantı bir mükemmel güvenlik durum olan tek ana bilgisayar: bağlantı noktası bileşimi için eşleşir.
- Standart web bağlantı noktaları üzerinden giden tüm bağlantıları gibi güvenlik duvarı delik normalde gerektirmez.
- Bu özellik ağ düzeyi olduğundan, uygulamanız tarafından kullanılan dil ve bitiş noktası tarafından kullanılan teknoloji bağımsızdır.
- Tek bir uygulama birden çok ağ erişim sağlamak için kullanılabilir. 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Karma bağlantılar ile yapamayacağı noktalar ###

Karma bağlantılar dahil olmak üzere, yapamayacağınız birkaç şey vardır:

- bir sürücü bağlama
- UDP kullanma
- Dinamik bağlantı noktaları, FTP Pasif modu veya genişletilmiş Pasif modu gibi kullanan hizmetler dayalı TCP erişme
- UDP şekliyle bazen LDAP desteği gerektirir
- Active Directory desteği

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Ekleme ve uygulamanızda karma bağlantı oluşturma ##

Karma bağlantılar, uygulama hizmeti uygulamanızı Azure portalında veya Azure geçiş Azure portalında aracılığıyla oluşturulabilir.  Karma bağlantı ile kullanmak istediğiniz uygulama hizmeti uygulaması aracılığıyla karma bağlantılar oluşturmak önerilir.  Karma bir bağlantı oluşturmak için şu adrese gidin [Azure portal] [ portal] ve uygulamanızı seçin.  Seçin **Ağ > karma bağlantı uç noktalarınızı yapılandırın**.  Buradan, uygulamanız için yapılandırılmış karma bağlantılar görebilirsiniz.  

![Karma bağlantı listesi][2]

Yeni bir karma bağlantı eklemek için karma Bağlantı Ekle'yi tıklatın.  Açılan UI önceden oluşturduğunuz karma bağlantılar listeler.  Bir veya daha fazlası uygulamanıza eklemek için istediğiniz ve isabet olanları tıklatın **Ekle seçili karma bağlantı**.  

![Karma bağlantı portalı][3]

Yeni bir karma bağlantı oluşturmak istiyorsanız, tıklatın **yeni karma bağlantı oluşturmak**.  Buradan, belirttiğiniz: 

- uç nokta adı
- uç noktası ana bilgisayar adı
- uç nokta bağlantı noktası
- Kullanmak istediğiniz hizmet veri yolu ad alanı

![Karma bağlantı oluşturma][4]

Her karma bağlantı için bir hizmet veri yolu ad alanı bağlıdır ve bir Azure bölgesinde her hizmet veri yolu ad alanıdır.  Deneyin ve kopyaladığınızda ağ gecikmesi önlemek amacıyla, uygulamanız ile aynı bölgede bir hizmet veri yolu ad alanını kullanmak önemlidir.

Karma bağlantınız uygulamanızdan kaldırmak istiyorsanız, sağ tıklayın ve seçin **Bağlantıyı Kes**.  

Karma bağlantı, uygulamanızın eklendikten sonra Ayrıntılar üzerinde yalnızca tıklayarak görebilirsiniz. 

![Karma bağlantı ayrıntıları][5]

### <a name="creating-a-hybrid-connection-in-the-azure-relay-portal"></a>Karma bağlantı Azure geçişi Portalı'nda oluşturma ###

Portal deneyimlerden ek olarak, uygulamanızın içinde ayrıca Azure geçişi Portalı'ndan gelen karma bağlantıları oluşturma olanağı vardır. Azure App Service tarafından kullanılan karma bağlantı için sırayla bu iki ölçütü karşılaması gerekir. Aşağıdakileri yapmalıdır:

* İstemci kimlik doğrulaması gerektirir
* Bir meta veri öğesi içeren bir ana bilgisayar: bağlantı noktası birleşimi değeri olarak uç nokta adlı mi

## <a name="hybrid-connections-and-app-service-plans"></a>Karma bağlantılar ve uygulama hizmeti planları ##

Karma bağlantılar, yalnızca temel, standart, Premium ve SKU'ları fiyatlandırması Isolated kullanılabilir.  Fiyatlandırma plana bağlı sınırları vardır.  

> [!NOTE] 
> Yeni Karma bağlantıları Azure geçişte göre yalnızca oluşturabilirsiniz. Yeni BizTalk karma bağlantılar oluşturulamıyor.
>

| Plan fiyatlandırması | Karma bağlantılar planda kullanılabilir sayısı |
|----|----|
| Temel | 5 |
| Standart | 25 |
| Premium | 200 |
| Yalıtılmış | 200 |

Uygulama hizmeti planı kısıtlamaları olduğundan, UI kaç karma bağlantılar kullanıldığını gösterir App Service planı ve hangi uygulamalar tarafından bulunmaktadır.  

![Uygulama Servie plan düzeyi özellikleri][6]

Tıklayarak karma bağlantınız ayrıntılarını görebilirsiniz.  Burada gösterilen özellikleri, uygulama Sergi gördüğünüz tüm bilgileri görebilir ve kaç tane diğer uygulamalardaki aynı uygulama hizmeti planı Bu karma bağlantıyı kullanarak da görebilirsiniz.

Bir uygulama hizmeti planı'nda kullanılabilir karma bağlantı uç sayısına bir sınır olsa da, bu uygulama hizmeti planı uygulamalarda herhangi bir sayıda genelinde kullanılan her karma bağlantı kullanılabilir.  Diğer bir deyişle, bir uygulama hizmeti planında 5 ayrı uygulamalarında kullanılan tek bir karma bağlantı 1 karma bağlantı olarak sayılır.

Karma bağlantılar kullanarak ek bir maliyet yoktur.  Karma bağlantı fiyatlandırma hakkında daha fazla bilgi için lütfen buraya gidin: [Service Bus fiyatlandırma][sbpricing].

## <a name="hybrid-connection-manager"></a>Karma Bağlantı Yöneticisi ##

Karma bağlantılar'ın çalışması için sırayla geçiş aracısı, karma bağlantı uç noktasını barındıran ağdaki gerekir.  Bu geçiş aracısı karma Bağlantı Yöneticisi (HCM) adı verilir.  Bu araç şu adresten yüklenebilir: **Ağ > karma bağlantı uç noktalarınızı yapılandırın** UI içinde uygulamanızdan kullanılabilir [Azure portal][portal].  

Bu araç, Windows server 2012 ve Windows'un sonraki sürümlerinde çalışır.  HCM çalışır bir hizmet olarak bir kez yüklenir.  Bu hizmet üzerinde yapılandırılmış uç tabanlı Azure Service Bus geçişi bağlanır.  HCM bağlantılarından bağlantı noktası 443 üzerinden Azure'a giden.    

HCM yapılandırmak için bir kullanıcı Arabirimi vardır.  HCM yüklendikten sonra karma Bağlantı Yöneticisi'ni yükleme dizininde bulunur HybridConnectionManagerUi.exe çalıştırarak UI'yi kullanıma sunabilirsiniz.  Yazarak Windows 10'da kolayca ulaşıldığında *karma Bağlantı Yöneticisi kullanıcı Arabirimi* , arama kutusuna.  

![Karma bağlantı portalı][7]

HCM kullanıcı Arabirimi başlatıldığında, gördüğünüz ilk tüm HCM'ın bu örneğinin yapılandırılmış karma bağlantıları listeleyen bir tablo şeydir.  Herhangi bir değişiklik yapmak istiyorsanız, Azure kimlik doğrulaması gerekir. 

Bir veya daha fazla karma bağlantılar, HCM eklemek için:

1. HCM kullanıcı arabirimini Başlat
1. Başka bir karma bağlantı yapılandırmak tıklayın ![HCM bir HC ekleme][8]

1. Azure hesabınızla oturum açın
1. Bir abonelik seçin
1. Tıklatın karma bağlantılarında istediğiniz geçiş HCM ![bir HC seçin][9]

1. Kaydet’e tıklayın.

Bu noktada, eklediğiniz karma bağlantılar görürsünüz.  Ayrıca, yapılandırılmış karma bağlantısı tıklatın ve BT hakkındaki ayrıntıları bakın.

![HC ayrıntıları][10]

HCM ile yapılandırılmış karma bağlantılar destekleyebilmesi, onu gerekir:

- TCP bağlantı noktaları 80 ve 443 üzerinden Azure erişimi
- Karma bağlantı uç noktasının TCP erişimi
- Uç noktası ana bilgisayar ve Azure hizmet veri yolu ad DNS göz atmayı yapma yeteneği

HCM, hem yeni karma bağlantılar, hem de eski BizTalk karma bağlantılar destekler.

> [!NOTE]
> Azure geçiş bağlantısı Web yuvalarını kullanır. Bu özellik yalnızca Windows Server 2012 veya daha yeni kullanılabilir. Bu nedenle karma Bağlantı Yöneticisi'ni hiçbir şey Windows Server 2012'den önceki desteklenmiyor.
>

### <a name="redundancy"></a>Yedeklilik ###

Her HCM, birden çok karma bağlantılar destekleyebilir.  Ayrıca, birden çok HCMs tarafından verilen tüm karma bağlantı desteklenebilir.  Varsayılan davranış hepsini bir kez trafiği verilen herhangi bir uç nokta için yapılandırılmış HCMs arasında değil.  Yüksek kullanılabilirlik, karma bağlantılar ağınızdan isterseniz, yalnızca birden çok HCMs ayrı makinelerde örneği oluşturur. 

### <a name="manually-adding-a-hybrid-connection"></a>El ile karma bağlantı ekleme ###

Birisi HCM örneği belirli bir karma bağlantı için ana bilgisayar için aboneliğinizi dışında istiyorsanız, bunları ile karma bağlantı için ağ geçidi bağlantı dizesi paylaşabilirsiniz.  Bu özellikler karma bir bağlantı için gördüğünüz [Azure portal][portal]. Bu dizeyi kullanmak için tıklatın **el ile girin** HCM düğmesine tıklayın ve ağ geçidi bağlantı dizesini yapıştırın.


## <a name="troubleshooting"></a>Sorun giderme ##

Karma bağlantı için bağlantı durumu, en az bir HCM, karma bağlantısının ile yapılandırılmış ve Azure ulaşamadığını anlamına gelir.  Karma bağlantınız için durum söyleyin değil **bağlı**, sonra da karma bağlantınız Azure erişimi HCM yapılandırılmamış.

Uç nokta bir DNS adı yerine bir IP adresi kullanarak belirtilmediğinden, istemciler kendi uç noktasına bağlanamıyor birincil nedenidir.  Uygulamanızı istenen endpoint ulaşamıyor ve bir IP adresi kullandıysanız HCM'ın çalıştırıldığı konak üzerinde geçerli bir DNS adı kullanmaya geçiş yapın.  Denetlenecek diğer DNS adını düzgün HCM çalıştığı konakta çözümler ve HCM karma bağlantı uç noktasına çalıştığı ana bilgisayardan bağlantısı olduğunu noktalardır.  

App Service'te tcpping adlı Gelişmiş araçlar (Kudu) Konsolu'ndan çağrılan bir aracı yoktur.  Bu araç TCP uç noktası erişimi ancak bunu, karma bağlantı uç noktasının erişiminiz varsa söylemez anlayabilirsiniz.  Karma bağlantı uç noktasının karşı konsolunda kullanıldığında, başarılı bir ping işlemi yalnızca bu ana bilgisayar: bağlantı noktası bileşimi kullanan uygulamanız için yapılandırılmış karma bağlantı olduğunu söyler.  

## <a name="biztalk-hybrid-connections"></a>BizTalk Karma Bağlantıları ##

Eski BizTalk karma bağlantılar özelliği devre dışı yeni BizTalk karma bağlantı kapatıldı.  Mevcut BizTalk karma bağlantılarınızı uygulamalarınızı ile kullanmaya devam edebilirsiniz, ancak Azure geçişi kullanan yeni karma bağlantılar için geçirmeniz gerekir.  Yeni hizmet BizTalk sürüm üzerinden avantajları arasında şunlardır:

- Hiçbir ek BizTalk hesabı gereklidir.
- TLS 1.2 BizTalk karma bağlantılar olduğu gibi 1.0 yerine ' dir.
- Bağlantı noktaları 80 ve 443 numaralı IP adresleri yerine Azure ve bir dizi ek diğer ulaşmak için bir DNS adını kullanarak bağlantı noktaları üzerinden iletişim önemlidir.  

Mevcut bir BizTalk karma bağlantıyı uygulamanıza eklemek için uygulamanızı gidin [Azure portal] [ portal] tıklatıp **Ağ > karma bağlantı uç noktalarınızı yapılandırın**.  Klasik karma bağlantılar tabloda tıklatın **Klasik karma Bağlantı Ekle**.  Buradan, BizTalk karma bağlantılar listesini görürsünüz.  


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
