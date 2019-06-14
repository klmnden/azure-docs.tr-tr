---
title: Azure hizmet veri yolu AMQP 1.0 genel bakış | Microsoft Docs
description: Advanced Message Queuing Protocol (AMQP) 1.0 azure'da kullanma hakkında bilgi edinin.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 0e8d19cc-de36-478e-84ae-e089bbc2d515
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 34829482e570354c1ab1e1fd6cec0c96b993cd83
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60403924"
---
# <a name="amqp-10-support-in-service-bus"></a>Hizmet veri yolu AMQP 1.0 desteği
Azure Service Bus bulut hizmeti ve şirket içi [Windows Server için hizmet veri yolu (hizmet veri yolu 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) Gelişmiş ileti sıraya alma Protokolü (AMQP) 1.0 desteği. AMQP, platformlar arası, açık standart protokolü kullanılarak karma uygulamalar oluşturmanıza olanak sağlar. Farklı diller ve çerçeveler kullanılarak oluşturulur ve farklı işletim sistemleri üzerinde çalışan bileşenlerini kullanarak uygulama oluşturabilirsiniz. Tüm bu bileşenler, Service Bus ve sorunsuz bir şekilde bağlanabilir, verimli bir şekilde ve tam uygunlukta yapılandırılmış iş mesaj alışverişi.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Giriş: AMQP 1.0 nedir ve neden önemlidir?
Geleneksel olarak, iletiye yönelik ara yazılım ürünleri, istemci uygulamaları ile aracıları arasındaki iletişim için özel protokoller kullandınız. Bu, belirli bir satıcının Mesajlaşma Aracısı seçtikten sonra satıcının kitaplıkları bu aracı, istemci uygulamalarınıza bağlanmak için kullanmanız gerekir, anlamına gelir. Bir uygulama için farklı bir ürünü taşıma bağlı tüm uygulamaları içinde kod değişiklikleri gerektirdiğinden bu düzeyde bir satıcıyı bağımlılığa sonuçlanır. 

Ayrıca, farklı satıcılardan Mesajlaşma aracıları bağlamak zor aynıdır. Bu, genellikle iletilerin bir sistemden diğerine taşımak için ve kendi özel ileti biçim arasında çeviri yapmak için uygulama düzeyi köprüleme gerektirir. Bu sık karşılaşılan bir gereksinimdir; Örneğin, ne zaman gerekir yeni birleşik bir arabirim için eski bağımsız sistemleri sağladığınız veya bir birleşme izleyerek BT sistemlerini tümleştirin.

Yazılım sektöründe hızlı ilerleyen bir iştir; Yeni programlama dilleri ve uygulama çerçeveleri bazen bewildering bir hızda kullanıma sunulmuştur. Benzer şekilde, BT sistemlerinin gereksinimleri zamanla gelişmesinin ve geliştiriciler, en son platform özelliklerinden yararlanmak istiyorsanız. Ancak, bazen seçili ileti satıcı bu platformları desteklemez. Mesajlaşma protokolleri özel olduğundan, başkalarının yeni şu platformlar için kitaplıkları sağlamak mümkün değildir. Bu nedenle, ağ geçitleri veya Mesajlaşma ürünü kullanmaya devam etmenize olanak sağlayan köprüleri oluşturma gibi bir yaklaşım kullanmanız gerekir.

Geliştirme, Advanced Message Queuing Protocol (AMQP) 1.0 sorunlardan motive. JP Morgan kimin gibi en finansal hizmet şirketleri, iletiye yönelik ara yazılım ağır kullanıcılarıdır kestirmeden sonuca gitme, kutucuğun kaynağı. Hedef basit: farklı dilleri, çerçeveler ve işletim sistemleri, kullanılarak oluşturulan bileşenlerini kullanarak ileti tabanlı uygulamalar oluşturmak yapılabilir bir Mesajlaşma açık standart protokolü oluşturmak için tüm türünün en iyisi çeşitli bileşenlerini kullanma Üreticiler.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 Teknik Özellikler
AMQP 1.0 Mesajlaşma uygulamaları sağlam, platformlar arası oluşturmak için kullanabileceğiniz bir verimli, güvenilir, hat düzeyinde bir Mesajlaşma protokolüdür. Basit bir hedef Protokolü vardır: iki taraflar arasında iletileri güvenli, güvenilir ve verimli aktarım mekanizması tanımlamak için. İletileri heterojen göndericiler ve alıcılar tam uygunlukta, yapılandırılmış iş mesaj alışverişi sağlayan bir taşınabilir veri gösterimi kullanılarak kodlanır. En önemli özelliklerin özeti aşağıda verilmiştir:

* **Verimli**: AMQP 1.0 protokol yönergeleri ve iş iletileri için ikili kodlama kullanan üzerinden aktarılan bir bağlantıya dayalı bir protokoldür. Bu, ağ ve bağlı bileşenleri kullanımını en üst düzeye çıkarmak için Gelişmiş Akış denetimi düzenleri içerir. Bu, Protokolü verimliliği, esneklik ve birlikte çalışabilirlik arasında bir denge için tasarlanmış belirtti.
* **Güvenilir**: Bir dizi Başlat ve unut için güvenilir, gelen güvenilirlik Garantisi ile tam olarak değiştirilmek üzere iletileri AMQP 1.0 protokol sağlar-bir kez teslim onaylanır.
* **Esnek**: AMQP 1.0 farklı topolojileri desteklemek için kullanılan esnek bir protokoldür. Aynı protokol, istemci ve istemci, istemci aracısı ve aracısı için aracı iletişimi için kullanılabilir.
* **Model Aracısı bağımsız**: AMQP 1.0 belirtimi, gereksinimlere aracısı tarafından kullanılan bir Mesajlaşma modeli yapmaz. Başka bir deyişle, var olan bir Mesajlaşma aracıları için AMQP 1.0 desteğine kolayca eklemek mümkündür.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 olan bir standart (büyük'ın ')
AMQP 1.0 bir uluslararası standart olan ISO ve ISO/IEC 19464:2014 olarak IEC tarafından onaylandı.

AMQP 1.0, bir çekirdek Grup 20'den fazla şirketler tarafından 2008 geliştirilmekte oluştu teknoloji tedarikçileri hem son kullanıcı firmalar. Bu süre boyunca kullanıcı firmaları gerçek dünyadaki iş gereksinimlerine olmuş ve teknoloji satıcıları bu gereksinimleri karşılamak için protokol gelişim göstermiştir. İşlem boyunca, uygulamalar arasında birlikte çalışabilirliği doğrulamak için bunlar etiketlerde atölyeler satıcıları katılan.

Ekim 2011'in geliştirme çalışması için ilerletme, yapılandırılmış bilgi standartları (OASIS) ve OASIS AMQP 1.0 standart için bir kuruluştaki bir teknik komitesi geçirileceğini Ekim 2012'de yayınlanmıştır. Aşağıdaki şirketleri teknik komitesi standart geliştirilmesi sırasında aracısından:

* **Teknoloji satıcıları**: Axway yazılım, Huawei teknolojileri, IIT yazılım, INETCO sistemleri, Kaazing, Microsoft, Mitre Corporation, Primeton teknolojileri, ilerleme durumunu Software, Red Hat, SITA, yazılım AG, sistemleri, VMware, WSO2, Zenika Solace.
* **Kullanıcı firmaları**: Amerika, kredi Suisse Deutsche Boerse, Goldman, banka Sachs, JPMorgan kestirmeden sonuca gitme.

Açık standartlar yaygın olarak alıntı avantajlarından bazıları şunlardır:

* Daha az olasılığı satıcı kilit açma
* Birlikte çalışabilirlik
* Geniş kullanılabilirliğini kitaplıkları ve araçları
* Kullanım dışı kalma karşı koruma
* Bilgili personeli kullanılabilirliği
* Alt ve yönetilebilir risk

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 ve Service Bus
Azure hizmet veri yolu AMQP 1.0 desteğine, Service Bus queuing yararlanarak artık ve yayımlama/aracılı Mesajlaşma özelliklerinin verimli bir ikili protokolü kullanılarak platformlar aralığından abonelik anlamına gelir. Ayrıca, dillerin, çerçevelerin ve işletim sistemlerinin bir karışımını kullanılarak oluşturulan bileşenlerden oluşan bir uygulama oluşturabilirsiniz.

Aşağıdaki şekilde, standart Java mesaj hizmeti (JMS) API ve Windows üzerinde çalışan .NET istemcileri kullanılarak yazılmış, Linux üzerinde çalışan Java istemcileri'nı AMQP 1.0 kullanarak Service Bus aracılığıyla iletileri değiş örnek bir dağıtım gösterilmektedir.

![][0]

**Şekil 1: Platformlar arası Mesajlaşma Service Bus ve AMQP 1.0 kullanarak gösteren örnek dağıtım senaryosu**

Şu anda, Service Bus ile çalışmak için aşağıdaki istemci kitaplıklardan bilinmektedir:

| Dil | Kitaplık |
| --- | --- |
| Java |Apache Qpid Java mesaj hizmeti (JMS) istemcisi<br/>IIT yazılım SwiftMQ Java istemci |
| C |Apache Qpid Proton-C |
| PHP |Apache Qpid Proton-PHP |
| Python |Apache Qpid Proton-Python |
| C# |AMQP .NET Lite |

**Şekil 2: AMQP 1.0 istemci kitaplıkları, tablo**

## <a name="summary"></a>Özet
* AMQP 1.0 platformlar arası karma uygulamalar oluşturmak için kullanabileceğiniz bir açık, güvenilir bir Mesajlaşma protokolüdür. AMQP 1.0 bir OASIS standart'tır.
* Windows Server için hizmet veri yolu (hizmet veri yolu 1.1) yanı sıra Azure hizmet veri yolu AMQP 1.0 desteği artık kullanılabilir. Fiyatlandırma mevcut protokolleri aynıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi hazır mısınız? Aşağıdaki bağlantıları ziyaret edin:

* [.NET Service Bus ile AMQP kullanma]
* [Java Service Bus ile AMQP kullanma]
* [Bir Azure Linux VM'de Proton-C Apache Qpid yükleme]
* [Windows Server için hizmet veri yolu AMQP]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[.NET Service Bus ile AMQP kullanma]: service-bus-amqp-dotnet.md
[Java Service Bus ile AMQP kullanma]: service-bus-amqp-java.md
[Bir Azure Linux VM'de Proton-C Apache Qpid yükleme]: service-bus-amqp-apache.md
[Windows Server için hizmet veri yolu AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
