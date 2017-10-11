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
ms.date: 04/22/2017
ms.author: ccompy
ms.openlocfilehash: fef9e7b280387934cb093f51b4c8aa134a3b87e7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="azure-app-service-hybrid-connections"></a>Azure uygulama hizmeti karma bağlantılar #

## <a name="overview"></a>Genel Bakış ##

Karma bağlantılar, hem Azure hizmetinde, hem de Azure App Service'te bir özellik değil.  Bir hizmet olarak kullanın ve Azure App Service'te işlevden ötesinde özellikler vardır.  Karma bağlantılar ve burada başlayabileceğini Azure App Service dışında kullanımı hakkında daha fazla bilgi edinmek için [Azure geçişi karma bağlantılar][HCService]

Azure App Service içinde karma bağlantılar diğer ağlara uygulama kaynaklara erişim için kullanılabilir. Bir uygulama uç nokta UYGULAMANIZDAN erişmenizi sağlar.  Uygulamanızı erişmek bir alternatif özelliği sağlamaz.  App Service içinde kullanılan gibi her bir karma bağlantı tek bir TCP ana bilgisayarı ve bağlantı noktası bileşimi hatalarla ilintilidir.  Bu karma bağlantı uç noktasının herhangi bir işletim sisteminde olabilir ve herhangi bir uygulama bir TCP dinleme bağlantı noktası devreyi sağlanan anlamına gelir. Karma bağlantılar, bilmiyorsanız veya uygulama protokolü nedir veya erişmeye çalıştığınız dikkat edin.  Ayrıca, ağ erişimi yalnızca sağlamaktır.  


## <a name="how-it-works"></a>Nasıl çalışır? ##
Karma bağlantılar özelliği Service Bus geçişi iki giden çağrıları oluşur.  Burada, uygulamanızın app service içinde çalışan ve sonra Service Bus geçişi karma bağlantı Manager(HCM) arasında bağlantı yok ana bilgisayardaki bir kitaplıktan bir bağlantı yok.  HCM ağ barındırma içinde dağıttığınız bir geçiş hizmetidir 

İki birleştirilmiş bağlantılarında uygulamanızı sabit ana: bağlantı noktası bileşimi için bir TCP tünel HCM diğer tarafta sahiptir.  Bağlantı TLS 1.2 güvenlik ve kimlik doğrulama/yetkilendirme için SAS anahtarları kullanır.    

![][1]

Uygulamanızı yapılandırma karma bağlantı uç noktayla eşleşen bir DNS isteğinde bulunduğunda, giden TCP trafiğine karma bağlantı yönlendirilir.  

> [!NOTE]
> Başka bir deyişle, her zaman karma bağlantınız için bir DNS adı kullanmayı denemeniz gerekir.  Uç nokta bir IP adresi yerine kullanıyorsa, bazı istemci yazılımı bir DNS araması yapmaz.
>
>

Karma bağlantılar, Azure geçiş altında bir hizmet olarak sunulan yeni karma bağlantılar ve eski BizTalk karma bağlantılar iki tür vardır.  Eski BizTalk karma bağlantılar olarak Klasik karma bağlantılar portalda denir.  Bu belgede daha sonra bunları hakkında daha fazla bilgi yoktur.

### <a name="app-service-hybrid-connection-benefits"></a>Uygulama hizmeti karma bağlantı avantajları ###

Karma bağlantılar yeteneği dahil olmak üzere avantajları vardır

- Uygulamaları güvenli bir şekilde şirket içi sistemleri ve Hizmetleri ile güvenli bir şekilde erişebilir
- Bu özellik Internet erişilebilir uç gerektirmez
- hızlı ve kolay ayarlamak  
- Her bir karma bağlantı bir mükemmel güvenlik durum olan tek ana bilgisayar: bağlantı noktası birleşimi ile eşleşir
- Standart web bağlantı noktaları üzerinden giden tüm bağlantıları gibi normalde güvenlik duvarı delik gerektirmez
- özellik anlamına da ağ düzeyi olduğundan, uygulamanız tarafından kullanılan dil ve bitiş noktası tarafından kullanılan teknoloji bağımsızdır
- tek bir uygulama birden çok ağ erişim sağlamak için kullanılabilir 

### <a name="things-you-cannot-do-with-hybrid-connections"></a>Karma bağlantılar ile yapamayacağı noktalar ###

Karma bağlantılar ile yapamayacağınız birkaç şey vardır ve bunlar şunları içerir:

- bir sürücü bağlama
- UDP kullanma
- FTP Pasif modu veya genişletilmiş Pasif modu gibi dinamik bağlantı noktaları kullanan hizmetler dayalı TCP erişme
- UDP şekliyle bazen LDAP desteği gerektirir
- Active Directory desteği

## <a name="adding-and-creating-a-hybrid-connection-in-your-app"></a>Ekleme ve uygulamanızda karma bağlantı oluşturma ##

Karma bağlantılar, uygulama portalı üzerinden veya karma bağlantı hizmet portalından oluşturulabilir.  Uygulamanız ile kullanmak istediğiniz karma bağlantılar oluşturmak için uygulama portalını kullanma önerilir.  Karma bir bağlantı oluşturmak için Git [Azure portal] [ portal] ve uygulamanız için kullanıcı Arabirimi uygulamasına gidin.  Seçin **Ağ > karma bağlantı uç noktalarınızı yapılandırın**.  Buradan, uygulamanızı yapılandırılmış karma bağlantılar görebilirsiniz.  

![][2]

Yeni bir karma bağlantı eklemek için karma Bağlantı Ekle'yi tıklatın.  Açılan UI önceden oluşturduğunuz karma bağlantılar listeler.  Bir veya daha fazlası uygulamanıza eklemek için istediğiniz ve isabet olanları tıklatın **Ekle seçili karma bağlantı**.  

![][3]

Yeni bir karma bağlantı oluşturmak istiyorsanız, tıklatın **yeni karma bağlantı oluşturmak**.  Buradan belirtin: 

- uç nokta adı
- uç noktası ana bilgisayar adı
- uç nokta bağlantı noktası
- kullanmak istediğiniz servicebus ad alanı

![][4]

Her karma bağlantı için hizmet veri yolu ad alanı bağlıdır ve bir Azure bölgesinde her hizmet veri yolu ad alanıdır.  Deneyin ve kopyaladığınızda ağ gecikmesi önlemek amacıyla, uygulamanız ile aynı bölgede bir hizmet veri yolu ad alanı kullanmak önemlidir.

Karma bağlantınız uygulamanızdan kaldırmak istiyorsanız, sağ tıklayın ve seçin **Bağlantıyı Kes**.  

Karma bağlantı, web uygulamanızın eklendikten sonra Ayrıntılar üzerinde yalnızca tıklayarak görebilirsiniz.  

![][5]

## <a name="hybrid-connections-and-app-service-plans"></a>Karma bağlantılar ve uygulama hizmeti planları ##

Şimdi yapabileceğiniz yalnızca karma bağlantılar, yeni karma bağlantılar gerçekleştirilir.  Yalnızca temel, standart, Premium ve SKU'ları fiyatlandırması Isolated kullanılabilir.  Fiyatlandırma plana bağlı sınırları vardır.  

| Plan fiyatlandırması | Karma bağlantılar planda kullanılabilir sayısı |
|----|----|
| Temel | 5 |
| Standart | 25 |
| Premium | 200 |
| Yalıtılmış | 200 |

Uygulama hizmeti planı kısıtlamaları olduğundan UI kaç karma bağlantılar kullanıldığını gösterir App Service planı ve hangi uygulamalar tarafından bulunmaktadır.  

![][6]

Gibi uygulama görünümüyle ayrıntıları karma bağlantınız tıklayarak görebilirsiniz.  Burada gösterilen özellikleri'nde uygulama görünüme sahip tüm bilgileri görebilirsiniz ancak de aynı uygulama hizmeti planında kaç diğer uygulamalar bu karma bağlantı kullandığını görebilirsiniz.

![][7]

Bir uygulama hizmeti planı'nda kullanılabilir karma bağlantı uç sayısına bir sınır olsa da, bu uygulama hizmeti planı uygulamalarda herhangi bir sayıda genelinde kullanılan her karma bağlantı kullanılabilir.  My uygulama hizmeti planı'nda 5 ayrı uygulamalarında kullandım 1 karma bağlantı varsa, yine yalnızca 1 karma bağlantı olduğunu söylemek için olmasıdır.

Karma bağlantılar'yalnızca temel, standart, Premium veya yalıtılmış SKU içinde kullanılabilir olan ötesinde ek bir maliyet yoktur.  Karma bağlantı fiyatlandırma hakkında ayrıntılar lütfen buraya gidin: [Service Bus fiyatlandırma][sbpricing].

## <a name="hybrid-connection-manager"></a>Karma Bağlantı Yöneticisi ##

Sırayla çalışması karma bağlantılar için bir geçiş aracısı, karma bağlantı uç noktasını barındıran ağdaki gerekir.  Bu geçiş aracısı karma Bağlantı Yöneticisi (HCM) adı verilir.  Bu araç şu adresten yüklenebilir: **Ağ > karma bağlantı uç noktalarınızı yapılandırın** UI içinde uygulamanızdan kullanılabilir [Azure portal][portal].  

Bu araç, Windows server 2008 R2 ve Windows'un sonraki sürümlerinde çalışır.  HCM çalışır bir hizmet olarak bir kez yüklenir.  Bu hizmet üzerinde yapılandırılmış uç tabanlı Azure servicebus geçiş bağlanır.  HCM bağlantılarından 80 ve 443 numaralı bağlantı noktalarına giden.    

HCM yapılandırmak için bir kullanıcı Arabirimi vardır.  HCM yüklendikten sonra karma Bağlantı Yöneticisi'ni yükleme dizininde bulunur HybridConnectionManagerUi.exe çalıştırarak UI'yi kullanıma sunabilirsiniz.  Yazarak Windows 10'da kolayca ulaşıldığında *karma Bağlantı Yöneticisi kullanıcı Arabirimi* , arama kutusuna.  

HCM kullanıcı Arabirimi başlatıldığında, gördüğünüz ilk tüm HCM'ın bu örneğinin yapılandırılmış karma bağlantıları listeleyen bir tablo şeydir.  Herhangi bir değişiklik yapmak istiyorsanız Azure kimlik doğrulaması gerekir. 

Bir veya daha fazla karma bağlantılar, HCM eklemek için:

1. HCM kullanıcı arabirimini Başlat
1. Başka bir karma bağlantı yapılandırmak tıklayın![][8]

1. Azure hesabınızla oturum açın
1. Bir abonelik seçin
1. Geçiş için bu HCM istediğiniz karma bağlantılar'ı tıklatın![][9]

1. Kaydet’e tıklayın.

Bu noktada eklediğiniz karma bağlantılar görürsünüz.  Ayrıca yapılandırılmış karma bağlantısında tıklatın ve bağlantı ayrıntılarını bakın.

![][10]

HCM ile yapılandırılmış karma bağlantılar destekleyebilmesi, onu gerekir:

- TCP bağlantı noktaları 80 ve 443 üzerinden Azure erişimi
- Karma bağlantı uç noktasının TCP erişimi
- DNS yapılabilmesi aramalarına uç noktası ana bilgisayar ve azure servicebus ad alanı

HCM, hem yeni karma bağlantılar, hem de eski BizTalk karma bağlantılar destekler.

### <a name="redundancy"></a>Yedeklilik ###

Her HCM, birden çok karma bağlantılar destekleyebilir.  Ayrıca, herhangi bir belirtilen karma bağlantıyı birden çok HCMs tarafından desteklenebilir.  Varsayılan davranış hepsini bir kez trafiği verilen herhangi bir uç nokta için yapılandırılmış HCMs arasında değil.  Yüksek kullanılabilirlik, karma bağlantılar ağınızdan isterseniz, yalnızca birden çok HCMs ayrı makinelerde örneği oluşturur. 

### <a name="manually-adding-a-hybrid-connection"></a>El ile karma bağlantı ekleme ###

Birisi verilen karma bağlantı için bir HCM örneği barındırmak için aboneliğinizi dışında istiyorsanız, bunları ile karma bağlantı için ağ geçidi bağlantı dizesi paylaşabilirsiniz.  Bu karma bir bağlantı özelliklerinde görebilirsiniz [Azure portal][portal]. Bu dizeyi kullanmak için tıklatın **el ile yapılandırmanız** HCM düğmesine tıklayın ve ağ geçidi bağlantı dizesini yapıştırın.


## <a name="troubleshooting"></a>Sorun giderme ##

Ne zaman karma bağlantınız ile çalışan bir uygulama ayarlanır ve yapılandırılmış Bu karma bağlantısı olan en az bir HCM yoktur ardından durum söyleyin **bağlı** Portalı'nda.  Değil dediği varsa **bağlı** uygulamanızı çalışmıyor veya sizin HCM Azure 80 veya 443 bağlantı noktalarında giden bağlanamıyor anlamına gelir.  

Uç nokta bir DNS adı yerine bir IP adresi kullanarak belirtilmediğinden, istemciler kendi uç noktasına bağlanamıyor birincil nedenidir.  Uygulamanızı istenen endpoint ulaşamıyor ve bir IP adresi kullandıysanız HCM'ın çalıştırıldığı konak üzerinde geçerli bir DNS adı kullanmaya geçiş yapın.  Denetlenecek diğer DNS adını düzgün HCM çalıştığı konakta çözümler ve HCM karma bağlantı uç noktasına çalıştığı ana bilgisayardan bağlantısı olduğunu noktalardır.  

App Service'te tcpping adlı Konsolu'ndan çağrılan bir aracı yoktur.  Bu araç TCP uç noktası erişimi ancak bir karma bağlantı uç noktasının erişiminiz varsa söylemez anlayabilirsiniz.  Karma bağlantı uç noktasının karşı konsolunda kullanıldığında, başarılı bir ping işlemi yalnızca bu ana bilgisayar: bağlantı noktası bileşimi kullanan uygulamanız için yapılandırılmış karma bağlantı olduğunu söyler.  

## <a name="biztalk-hybrid-connections"></a>BizTalk Karma Bağlantıları ##

Eski BizTalk karma bağlantılar özelliği devre dışı başka BizTalk karma bağlantı oluşturmaları kapatıldı.  Önceden var olan BizTalk karma bağlantılarınızı uygulamalarınızı ile kullanmaya devam edebilirsiniz ancak yeni hizmet geçirmeniz gerekir.  Yeni hizmet BizTalk sürüm üzerinden avantajları arasında şunlardır:

- hiçbir ek BizTalk hesabı gereklidir
- TLS 1.2 BizTalk karma bağlantılar olduğu gibi 1.0 yerine olduğu
- Bağlantı noktaları 80 ve 443 numaralı IP adresleri yerine Azure ve bir dizi ek diğer ulaşmak için bir DNS adını kullanarak bağlantı noktaları üzerinden iletişim önemlidir.  

Uygulamanızı Git BizTalk karma bağlantı uygulamanıza eklemek için [Azure portal] [ portal] tıklatıp **Ağ > karma bağlantı uç noktalarınızı yapılandırın**.  Klasik karma bağlantıları tabloda tıklatın **Klasik karma Bağlantı Ekle**.  Buradan, BizTalk karma bağlantılar listesini görürsünüz.  


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

