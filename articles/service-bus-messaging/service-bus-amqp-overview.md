---
title: AMQP 1.0 Azure Service Bus içinde genel bakış | Microsoft Docs
description: Gelişmiş Message Queuing Protokolü (AMQP) 1.0 Azure kullanma hakkında bilgi edinin.
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: ''
ms.assetid: 0e8d19cc-de36-478e-84ae-e089bbc2d515
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/30/2018
ms.author: sethm
ms.openlocfilehash: 6d2dffd22ecfc0aaf6e338567d5cf107a2c07383
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
ms.locfileid: "28926606"
---
# <a name="amqp-10-support-in-service-bus"></a>Hizmet veri yolu AMQP 1.0 desteği
Azure hizmet veri yolu bulut hizmeti ve şirket içi [Windows Server için hizmet veri yolu (hizmet veri yolu 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) Gelişmiş Message Queuing Protokolü (AMQP) 1.0 destekler. AMQP, platformlar arası, açık bir standart protokol kullanan karma uygulamalar oluşturmanıza olanak sağlar. Farklı diller ve çerçeveler kullanılarak oluşturulan ve farklı işletim sistemlerinde çalışan bileşenleri kullanan uygulamalar oluşturabilirsiniz. Tüm bu bileşenleri Service Bus ve sorunsuz bir şekilde bağlayabilirsiniz yapılandırılmış iş iletileri verimli bir şekilde ve tam bir güvenilirlik, exchange.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Giriş: AMQP 1.0 nedir ve neden önemlidir?
Geleneksel olarak, ileti odaklı ara yazılım ürünleri istemci uygulamaları ve aracıları arasındaki iletişim için özel protokoller kullanmış. Bu, belirli bir satıcının Mesajlaşma Aracısı seçtikten sonra o satıcının kitaplıkları istemci uygulamalarınızı bu Aracısı bağlanmak için kullanması gerektiğini anlamına gelir. Farklı bir ürün uygulamaya taşıma bağlı tüm uygulamaları kod değişikliklerini gerektirdiğinden bu satıcıya, bağımlılığı derecesini sonuçlanır. 

Ayrıca, farklı satıcılardan Mesajlaşma aracıları bağlanma hassas. Bu, genellikle iletilerin bir sistemden taşımak ve kendi özel ileti biçim arasında çeviri için uygulama düzeyi köprüleme gerektirir. Bu ortak bir gereksinimdir; Örneğin, ne zaman yeni bir birleştirilmiş arabirimde çok eski farklı sistemleri sağlayın veya gerekir bir birleşme izleyerek BT sistemleri tümleştirme.

Yazılım endüstrisinin hızlı taşınan bir iştir; Yeni programlama dilleri ve uygulama çerçeveleri bazen bewildering hızı sunulur. Benzer şekilde, BT sistemlerinin gereksinimleri zamanla gelişmesi ve geliştiriciler, en son platform özelliklerinden yararlanmak istersiniz. Ancak, bazen seçili Mesajlaşma satıcı bu platformu desteklemez. Mesajlaşma protokolleri özel olduğundan, bu yeni platformlar için kitaplıkları sağlar başkalarına mümkün değil. Bu nedenle, ağ geçitleri veya ileti ürünü kullanmaya devam etmek etkinleştirmeniz köprüleri oluşturma gibi yaklaşımlar kullanmanız gerekir.

Geliştirme, Gelişmiş Message Queuing Protokolü (AMQP) 1.0 sorunlardan motive. JP Morgan olan, en finansal hizmetler firmalarından gibi ileti odaklı Ara ağır kullanıcılarının Chase, kaynaklanan. Hedef basit: farklı diller, çerçeveler ve işletim sistemleri kullanılarak oluşturulan bileşenleri kullanan ileti tabanlı uygulamalar oluşturmak mümkün kılan bir açık standart Mesajlaşma Protokolü oluşturmak için tüm en iyi türünün bileşenleri Üreticiler aralığından kullanma.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 Teknik Özellikleri
AMQP 1.0 ileti uygulamalarını güçlü, platformlar arası oluşturmak için kullanabileceğiniz bir verimli, güvenilir ve hat düzeyinde Mesajlaşma protokolüdür. Basit bir hedef Protokolü vardır: iletileri iki taraf arasında güvenli, güvenilir ve verimli aktarım mekanizması tanımlamak için. İletilerini kendileri heterojen göndericiler ile alıcılar tam uygunluğunu yapılandırılmış iş iletileri Exchange sağlayan bir taşınabilir veri gösterimi kullanılarak kodlanır. En önemli özelliklerin özeti aşağıdadır:

* **Verimli**: AMQP 1.0 olan bir bağlantıya dayalı bir ikili Protokolü yönergeleri ve iş iletileri için kodlama, aktarılan üzerine kullanan protokol. Ağ ve bağlı bileşenleri kullanımını en üst düzeye çıkarmak için Gelişmiş Akış denetimi düzenleri içerir. Bu, protokol verimlilik, esneklik ve birlikte çalışabilirlik arasında bir denge için tasarlanmıştır belirtti.
* **Güvenilir**: AMQP 1.0 protokolü yangın ve unut için güvenilir, gelen güvenilirlik garanti aralıklı tam olarak değiştirilebilmesi için iletilerini sağlar-kere teslim onaylanır.
* **Esnek**: AMQP 1.0 olan farklı topoloji desteklemek için kullanılan esnek bir protokoldür. Aynı protokol istemci istemci, istemci aracısı ve Aracısı Aracısı iletişimi için kullanılabilir.
* **Aracısı modeli bağımsız**: AMQP 1.0 belirtimi yapmaz gereksinimlere aracısı tarafından kullanılan bir ileti modeli. Başka bir deyişle, kolayca varolan Mesajlaşma aracıları için AMQP 1.0 desteği eklemek mümkündür.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 olan bir standart (büyük harf ait olan ')
AMQP 1.0 Uluslararası bir standart olan ISO ve IEC ISO/IEC 19464:2014 olarak onaylandı.

AMQP 1.0 çekirdek Grup 20'den fazla şirketleri tarafından 2008 beri geliştirilmekte açıldı teknolojisi üreticiler ve son kullanıcı firmalarından. Bu süre boyunca kullanıcı firmalarından gerçek iş gereksinimleri katkısı ve teknoloji satıcıları bu gereksinimleri karşılamak için Protokolü gelişim göstermiştir. İşlem boyunca, kendi uygulamalarını arasında birlikte çalışabilirlik doğrulamak için bunlar işbirliği Atölyeleri satıcılar katılan.

Ekim 2011 terfi, yapılandırılmış bilgi standartları (OASIS) ve OASIS AMQP 1.0 standart için kuruluşunuzdaki teknik komitesi geçti geliştirme iş Ekim 2012'de serbest bırakıldı. Aşağıdaki firmalarından içinde teknik komitesi standart geliştirme sırasında katılmış:

* **Teknoloji satıcılar**: Axway yazılım, Huawei teknolojileri, IIT yazılım, INETCO sistemleri, Kaazing, Microsoft, Mitre Corporation, Primeton teknolojileri, devam eden yazılım, Red Hat, SITA, yazılım AG, Solace sistemleri, VMware, WSO2, Zenika.
* **Kullanıcı firmalarından**: banka, Amerika, kredi Suisse, Alman Boerse, Goldman Sachs, JPMorgan Chase.

Açık standartlar yaygın olarak alıntı avantajlarından bazıları şunlardır:

* Daha az olasılığı satıcı kilit açma
* Birlikte çalışabilirlik
* Geniş kullanılabilirliğini kitaplıkları ve araçları
* Kullanım dışı kalma karşı koruma
* Bilgili personel kullanılabilirliği
* Alt ve yönetilebilir riski

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 ve hizmet veri yolu
Azure hizmet veri yolu AMQP 1.0 desteği Service Bus queuing yararlanan şimdi ve yayımlama/aracılı Mesajlaşma özellikleri verimli bir ikili protokolünü kullanarak platformları aralığından abonelik anlamına gelir. Ayrıca, diller, çerçevelerinin ve işletim sistemlerinin bir karışımını kullanılarak oluşturulan bileşenlerden oluşan uygulamalar oluşturabilir.

Aşağıdaki şekilde standart Java ileti hizmeti (JMS) API ve Windows üzerinde çalışan .NET istemcileri kullanılarak yazılmış Linux üzerinde çalışan Java istemcileri AMQP 1.0 kullanarak Service Bus aracılığıyla iletileri değiş örnek dağıtımı gösterilmektedir.

![][0]

**Şekil 1: platformlar arası Service Bus ve AMQP 1.0 kullanarak ileti gösteren örnek dağıtım senaryosu**

Şu anda, Service Bus ile çalışmak için aşağıdaki istemci kitaplıklarından bilinmektedir:

| Dil | Kitaplık |
| --- | --- |
| Java |Apache Qpid Java ileti hizmeti (JMS) istemcisi<br/>IIT yazılım SwiftMQ Java istemcisi |
| C |Apache Qpid Proton-C |
| PHP |Apache Qpid Proton-PHP |
| Python |Apache Qpid Proton-Python |
| C# |AMQP .net Lite |

**Şekil 2: Tablo AMQP 1.0 istemci kitaplıkları**

## <a name="summary"></a>Özet
* AMQP 1.0, platformlar arası, karma uygulamalar oluşturmak için kullanabileceğiniz bir açık, güvenilir Mesajlaşma protokolüdür. AMQP 1.0 bir OASIS standardıdır.
* AMQP 1.0 desteği artık Windows Server için hizmet veri yolu (hizmet veri yolu 1.1) yanı sıra Azure Service Bus içinde kullanılabilir. Fiyatlandırma varolan protokolleri aynıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi hazır mısınız? Aşağıdaki bağlantıları ziyaret edin:

* [.NET gelen hizmet veri yolu AMQP ile kullanma]
* [Java'dan hizmet veri yolu AMQP ile kullanma]
* [Bir Azure Linux VM'de Proton-C Apache Qpid yükleme]
* [Windows Server için hizmet veri yolu AMQP]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[.NET gelen hizmet veri yolu AMQP ile kullanma]: service-bus-amqp-dotnet.md
[Java'dan hizmet veri yolu AMQP ile kullanma]: service-bus-amqp-java.md
[Bir Azure Linux VM'de Proton-C Apache Qpid yükleme]: service-bus-amqp-apache.md
[Windows Server için hizmet veri yolu AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
