---
title: Service Fabric güvenilir hizmeti programlama modeline genel bakış | Microsoft Docs
description: Service Fabric'ın güvenilir hizmet programlama modeli hakkında bilgi edinin ve kendi Hizmetleri yazma çalışmaya başlayın.
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: timlt
editor: vturecek; mani-ramaswamy
ms.assetid: 0c88a533-73f8-4ae1-a939-67d17456ac06
ms.service: Service-Fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: masnider;
ms.openlocfilehash: 66e3939a59f09650ce76488c38eb46699ab9f63f
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="reliable-services-overview"></a>Reliable Services özelliğine genel bakış
Azure Service Fabric, yazma ve durum bilgisiz ve durum bilgisi olan güvenilir hizmetler yönetme basitleştirir. Bu konu şunları içerir:

* Durum bilgisiz ve durum bilgisi olan hizmetler için Reliable Services programlama modeli.
* Güvenilir bir hizmet yazılırken yapmak zorunda seçenekleri.
* Bazı senaryolar ve örnekler Reliable Services kullanmak ne zaman ve nasıl yazılır.

Güvenilir hizmetler biridir programlama modelleri Service Fabric üzerinde kullanılabilir. Diğeri aktör sanal bir programlama modelini Reliable Services model üzerinde sağlayan güvenilir aktör programlama modeli. Reliable Actors programlama modeli hakkında daha fazla bilgi için bkz: [Service Fabric Reliable Actors giriş](service-fabric-reliable-actors-introduction.md).

Service Fabric aracılığıyla Hizmetleri'nden, sağlama ve dağıtım yükseltme ve silme, aracılığıyla ömrü yönetir [Service Fabric uygulaması Yönetimi](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Güvenilir hizmetler nelerdir?
Güvenilir hizmetler, uygulamanız için önemli olan express yardımcı olması için bir basit, güçlü, üst düzey programlama modeli sağlar. Güvenilir programlama modeli Hizmetleri ile şunları alırsınız:

* API programlama Service Fabric kalan erişim. Service Fabric olarak Modellenen Hizmetleri aksine [Konuk yürütülebilir dosyalar](service-fabric-guest-executables-introduction.md), güvenilir hizmetler almak Service Fabric API rest doğrudan kullanmak. Bu hizmetleri sağlar:
  * sistemi sorgulama
  * küme içindeki varlıklar hakkında rapor durumu
  * Yapılandırma ve kodun değişiklikler hakkında bildirim almak
  * bulma ve diğer hizmetleri ile iletişim
  * (isteğe bağlı) kullanmak [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md)
  * ...ve erişim diğer birçok yetenekleri için birinci sınıf bir programlama modeli çeşitli programlama dillerinde çekmenize onların.
* Kendi kodunuzu çalıştırmak için basit bir model için kullanılan modellerini programlama gibi görünüyor. Kodunuzu bir iyi tanımlanmış giriş noktası ve kolayca yönetilebilir yaşam döngüsü vardır.
* Takılabilir iletişim modelini. HTTP ile gibi tercih ettiğiniz taşıması kullanan [Web API](service-fabric-reliable-services-communication-webapi.md), WebSockets, özel TCP protokolleri ya da başka bir şey. Güvenilir hizmetler, Giden kutusu seçeneklerini kullanabilirsiniz ya da kendi sağlayabilir bazı harika sağlar.
* Durum bilgisi olan hizmetler için güvenilir hizmetler programlama modeli kullanarak durumunuza sağ hizmetinizi iç tutarlı ve güvenilir bir şekilde depolamak sağlar [güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md). C# koleksiyonları kullanan herkes için tanıdık gelecektir yüksek oranda kullanılabilir ve güvenilir koleksiyon sınıfları basit bir dizi güvenilir koleksiyonlarıdır. Geleneksel olarak, dış sistemler güvenilir durum yönetimi için gereken hizmetleri. Güvenilir koleksiyonlarla aynı yüksek kullanılabilirlik ve güvenilirlik yüksek oranda kullanılabilir dış depoları beklenir gelen yanında, işlem durumunuza depolayabilirsiniz. Hesaplama ve çalışması için gereken durumu birlikte bulundurmak çünkü bu modeli gecikme süresi de geliştirir.

Bu genel bir bakış güvenilir hizmetler için Microsoft Virtual Academy videosunu izleyin. <center>
<a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=HhD9566yC_4106218965">
<img src="./media/service-fabric-reliable-services-introduction/ReliableServicesVid.png" WIDTH="360" HEIGHT="244" />
</a>
</center>

## <a name="what-makes-reliable-services-different"></a>Güvenilir hizmetler farklı ne yapar?
Service Fabric güvenilir Hizmetleri'nde önce yazılmış Hizmetleri'nden farklıdır. Service Fabric, güvenilirlik, kullanılabilirlik, tutarlılık ve ölçeklenebilirlik sağlar.

* **Güvenilirlik** - hizmet kalır yukarı güvenilir olmayan ortamlarda bile makinelerinizi başarısız veya ağ sorunları isabet olduğu veya nerede Hizmetleri kendilerini hataları ve kilitlenme karşılaşırsanız veya başarısız durumda. Durum bilgisi olan hizmetler için ağ veya diğer hataları varlığında olsa bile, durumu korunur.
* **Kullanılabilirlik** -, erişilebilir ve esnek bir hizmettir. Service Fabric kopyaları çalışan istenen numaranızı tutar.
* **Ölçeklenebilirlik** - Hizmetleri belirli donanım ayrılmış ve büyütür veya eklenmesi veya kaldırılması donanım veya diğer kaynakları aracılığıyla gerektiği gibi küçültür. Hizmetleri kolayca bölümlenmiş (durum bilgisi olan durumda özellikle) hizmetini ölçeklendirebilirsiniz emin olmak için ve işlemek kısmi hatalar. Hizmetleri oluşturulabilir ve müşteri isteklerine yanıt olarak dinamik olarak çalışmaya gerektiği gibi başlar için daha fazla örnekler etkinleştirme kodu aracılığıyla silinmiş söyleyin. Son olarak, Service Fabric Hizmetleri basit olmasını önerir. Service Fabric Hizmetleri tek bir işlem yerine gerektiren veya ayrılması tüm işletim sistemi örneği veya hizmet tek bir örneğine işlemleri içinde sağlanacak binlerce sağlar.
* **Tutarlılık** -bu hizmetinde depolanan tüm bilgileri tutarlı olmasını güvence altına alınabilir. Bu hizmet içinde birden çok güvenilir koleksiyonları arasında bile geçerlidir. İşlemsel olarak atomik bir biçimde bir hizmet kapsamındaki koleksiyonları arasında değişiklik yapılabilir.

## <a name="service-lifecycle"></a>Hizmet yaşam döngüsü
Durum bilgisi olan veya durum bilgisiz hizmetinizi olup olmadığını belirtir, hızlı bir şekilde kodunuzda takın ve çalışmaya başlama sağlayan basit bir yaşam döngüsü güvenilir hizmetler sağlar.  Hizmetinizi çalışır almak için uygulamanız gereken yalnızca bir veya iki yöntem vardır.

* **CreateServiceReplicaListeners/CreateServiceInstanceListeners** - This method is where the service defines the communication stack(s) that it wants to use. Aşağıdaki gibi iletişim yığın [Web API](service-fabric-reliable-services-communication-webapi.md), ne dinleme bitiş noktasını veya (nasıl istemcileri hizmete erişmek) hizmeti için uç noktalar tanımlar değil. Görüntülenen iletileri servis kodunu geri kalanı ile nasıl etkileşim tanımlar.
* **RunAsync** -hizmetiniz, iş mantığını çalıştığı ve burada bu hizmetin ömrü boyunca çalışması gerektiğini herhangi bir arka plan görevi devre dışı kazandırın bu yöntemidir. Sağlanan iptal belirteci zaman bu iş durması gerektiğini sinyaldir. Örneğin, hizmet iletileri güvenilir bir sıra dışında çekmek ve bunları işlemek gerekirse, bu iş yeri olur budur.

İlk kez güvenilir hizmetler öğrendiğiniz okumaya devam edin! Güvenilir hizmetler yaşam döngüsü için ayrıntılı bilgileri arıyorsanız, üzerinden için head [bu makalede](service-fabric-reliable-services-lifecycle.md).

## <a name="example-services"></a>Örnek Hizmetleri
Bu programlama modeli bilerek bir hızlı parçaların nasıl bir araya getireceğinizi görmek için iki farklı hizmet bakalım.

### <a name="stateless-reliable-services"></a>Durum bilgisiz güvenilir hizmetler
Bir durum bilgisi olmayan hizmetin bir yerdir hizmet içinde çağrıları arasında tutulan bir durum yok. Mevcut herhangi bir durum tamamen atılabilir ve eşitleme, çoğaltma, sürdürme veya yüksek kullanılabilirlik gerektirmez.

Örneğin, belleğe sahip değil ve tüm koşulları ve aynı anda gerçekleştirmek için işlemleri alan bir hesap makinesi göz önünde bulundurun.

Bu durumda, `RunAsync()` (C#) veya `runAsync()` (Java) hiçbir arka plan görevi hizmet yapmak için gereken işleme olduğundan hizmet boş olabilir. Hesaplayıcı hizmet oluşturulduğunda döndürür bir `ICommunicationListener` (C#) veya `CommunicationListener` (Java) (örneğin [Web API](service-fabric-reliable-services-communication-webapi.md)), açılan bazı bağlantı noktası üzerinde dinleyen bir uç noktası. Bu dinleme bitiş noktası için farklı hesaplama yöntemleri kancalarını (örnek: "Ekle (n1, n2)") hesap makinesi genel API'si tanımlayın.

Bir istemciden bir çağrısı yapıldığında uygun yöntemi çağrılır ve hesap makinesi hizmetinin sağlanan veriler üzerinde işlemler gerçekleştirir ve sonucu döndürür. Herhangi bir durum depolamak değil.

Herhangi bir iç durum depolamayın Bu örnek hesaplayıcı kolaylaştırır. Ancak çoğu Hizmetleri gerçekten durum bilgisiz değil. Bunun yerine, bunlar başka bir deposunda durumlarına hale getirmesini sağlayarak. (Örneğin, bir yedekleme deposu veya önbelleği oturum durumu tutmayı güvenen herhangi bir web uygulamasına durum bilgisiz değildir.)

Yaygın bir örneği durum bilgisi olmayan hizmetler kullanılan nasıl Service Fabric bir ön uç web uygulaması için genel kullanıma yönelik API gösteren gibidir. Ön uç hizmeti sonra bir kullanıcı isteği tamamlamak için durum bilgisi olan hizmetlere konuşmaları. Bu durumda, istemcilerden gelen çağrıları burada durum bilgisi olmayan hizmetin dinleme yaptığı bir bilinen bağlantı noktasına, 80 gibi yönlendirilir. Bu durum bilgisiz hizmet çağrıyı alır ve çağrı güvenilir bir tarafın olup olmadığını ve hangi hizmet belirler için hedeflenen.  Ardından, durum bilgisiz hizmet durum bilgisi olan hizmetin doğru bölüm çağrısı iletir ve yanıt bekler. Durum bilgisiz hizmeti bir yanıtı aldığında, özgün istemciye yanıt verir. Bu tür bir hizmet, bizim örneklerimizi örneğidir [C#](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started) / [Java](https://github.com/Azure-Samples/service-fabric-java-getting-started/tree/master/Actors/VisualObjectActor/VisualObjectWebService). Bu örnekte bu deseni yalnızca bir örneği, vardır başkalarının diğer örneklerinde.

### <a name="stateful-reliable-services"></a>Durum bilgisi olan güvenilir hizmetler
Durum bilgisi olan hizmet durumu işlevi için tutarlı ve hizmet için sırada mevcut tutulan kısmı olmalıdır biridir. Sürekli olarak çalışırken ortalama aldığı güncelleştirmelerine göre belirli bir değerin hesaplar bir hizmeti kullanmayı düşünün. Bunu yapmak için işlem ve geçerli ortalama için gereken gelen istekleri geçerli kümesi olmalıdır. Alır, işler ve (örneğin, bir Azure blob veya tablo deposu Bugün) bir dış deposunda bilgileri depolayan herhangi durum bilgisi olan hizmetidir. Yalnızca durumuna dış durum deposunda saklar.

Dış deponun için bu duruma güvenilirlik, kullanılabilirlik, ölçeklenebilirlik ve tutarlılık sağladıkları olduğundan çoğu Hizmetleri bugün durumlarına harici olarak depolar. Service Fabric içinde Hizmetleri durumlarına harici olarak depolamak için gerekmez. Service Fabric hizmeti kodunu ve hizmet durumu için bu gereksinimleri ilgilenir.

Resimlerini işleyen bir hizmet yazma istiyoruz varsayalım. Hizmeti bunu yapmak için bu görüntüyü gerçekleştirmek için bir görüntü ve bir dizi dönüşümleri alır. (Şimdi onu bir Webapı olduğunu varsayın) bu hizmeti iletişimi dinleyici düzenlemenizi sağlayan bir API ister döndürür `ConvertImage(Image i, IList<Conversion> conversions)`. Bir istek aldığında, hizmet depolar bir `IReliableQueue`ve böylece istek izleyebilirsiniz istemciye bazı kimliğini döndürür.

Bu hizmette `RunAsync()` daha karmaşık olabilir. Hizmet bir döngü içinde sahip kendi `RunAsync()` dışı istekleri çeker `IReliableQueue` ve istenen dönüşümleri gerçekleştirir. Sonuçları depolanan bir `IReliableDictionary` ne zaman istemci geri gelir böylece dönüştürülen resimlerinin elde edebilirsiniz. Bir şey görüntü başarısız olsa bile emin olmak için kaybı olmadığından, bu güvenilir hizmet sıranın dışında çekme, dönüşümlerini gerçekleştirmek ve tümünü tek bir işlemde sonucu depolamak. Bu durumda, ileti kuyruktan silinir ve yalnızca dönüşümleri tamamlandığında sonuçları Sonuç sözlükte depolanır. Alternatif olarak, hizmet görüntünün sıradaki çekme ve hemen bir uzak depolama alanında depolar. Bu durumu yönetmek için hizmetin sahip miktarını azaltır, ancak Uzak Depolama yönetmek için gerekli meta verileri tutmak hizmet itibaren artar karmaşıklık sahiptir. Bir şey ortadaki başarısız olursa ya da bir yaklaşım ile istek işlenmeyi bekleyen kuyruğunda kalır.

Bu hizmet hakkında dikkat edilecek bir gibi normal bir .NET hizmetine sesleri şeydir! Yalnızca kullanılan veri yapıları farktır (`IReliableQueue` ve `IReliableDictionary`) Service Fabric tarafından sağlanan ve yüksek oranda güvenilir, kullanılabilir ve tutarlı.

## <a name="when-to-use-reliable-services-apis"></a>Güvenilir hizmetler API'ları kullanma zamanı
Aşağıdakilerden birini uygulama hizmeti gereksinimlerinizi niteleyen bile güvenilir hizmetler API'leri düşünmeniz gerekir:

* Hizmetinizin kodunu istediğiniz (ve isteğe bağlı olarak durumu) yüksek oranda kullanılabilir ve güvenilir olması için
* İşlem garanti durumu (örneğin, siparişler ve satır öğeleri sipariş) birden çok birimi arasında gerekir.
* Uygulamanızın durumu doğal olarak güvenilir sözlükler ve Kuyruklar modellenebilir.
* Uygulamaların kod ya da durum düşük gecikme süresi okuma ve yazma işlemleri ile yüksek oranda kullanılabilir olması gerekir.
* Eşzamanlılık veya uygulaması yapılan işlemler kesinliği bir veya daha fazla güvenilir koleksiyonlarındaki denetlemek uygulamanız gerekir.
* Duyuruları yönetmek veya hizmetinizi bölümleme düzenini denetlemek istediğinizde.
* Kodunuzu bir ücretsiz iş parçacıklı çalışma zamanı ortamı gerekir.
* Dinamik olarak oluşturmak veya güvenilir sözlükler veya sıralar veya çalışma zamanında tüm hizmetleri yok etmek uygulamanız gerekir.
* Program aracılığıyla sağlanan Service Fabric yedekleme denetlemek ve geri yükleme özellikleri hizmetinizin durumu için gerekir.
* Kendi durumu birimleri için değişiklik geçmişini korumak uygulamanız gerekir.
* Geliştirme veya üçüncü taraf geliştirilmiş, özel durum sağlayıcılarının kullanmak istiyor.

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenilir hizmetler hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
* [Güvenilir koleksiyonları](service-fabric-reliable-services-reliable-collections.md)
* [Reliable Actors programlama modeli](service-fabric-reliable-actors-introduction.md)
