---
title: Service Fabric güvenilir hizmeti programlama modeline genel bakış | Microsoft Docs
description: Service Fabric'in Reliable Services programlama modeline hakkında bilgi edinin ve hizmetlerinizi yazmaya başlayın.
services: Service-Fabric
documentationcenter: .net
author: masnider
manager: chackdan
editor: vturecek; mani-ramaswamy
ms.assetid: 0c88a533-73f8-4ae1-a939-67d17456ac06
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 3/9/2018
ms.author: masnider
ms.openlocfilehash: 1789c7489e58df09dccfde3e7ab106ef54b5c1ae
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58666197"
---
# <a name="reliable-services-overview"></a>Reliable Services özelliğine genel bakış
Azure Service Fabric, yazma ve durum bilgisiz ve durum bilgisi olan Reliable Services yönetme kolaylaştırır. Bu konu şunları içerir:

* Durum bilgisiz ve durum bilgisi olan hizmetler için Reliable Services programlama modeli.
* Seçenekler bir güvenilir hizmet yazarken yapmanız gerekir.
* Bazı senaryolar ve örnekler Reliable Services kullanmak ne zaman ve nasıl yazılır.

Reliable Services, Service Fabric üzerinde kullanılabilir programlama modelleri biridir. Diğer bir Reliable Services model üzerinde sanal aktör programlama modeli sağlar Reliable Actor programlama modeli var. Reliable Actors programlama modeli hakkında daha fazla bilgi için bkz. [Service Fabric Reliable Actors hizmetine giriş](service-fabric-reliable-actors-introduction.md).

Service Fabric Hizmetleri, sağlama ve dağıtım yükseltme ve silme ile ömrünü aracılığıyla yöneten [Service Fabric uygulama yönetimini](service-fabric-deploy-remove-applications.md).

## <a name="what-are-reliable-services"></a>Reliable Services nedir?
Reliable Services uygulamanızı önemli express yardımcı olması için bir basit, güçlü, üst düzey programlama modeli sağlar. Güvenilir programlama modeli Hizmetleri olursunuz:

* Rest API'leri programlama Service fabric'in erişim. Service Fabric olarak modellenir hizmetlerinin aksine [Konuk yürütülebilir dosyaları](service-fabric-guest-executables-introduction.md), Reliable Services get Service Fabric API'leri rest doğrudan kullanın. Bu hizmetleri sağlar:
  * sistemi sorgulama
  * küme içindeki varlıklar hakkında rapor durumu
  * Yapılandırma ve kod değişiklikleri ile ilgili bildirimleri alma
  * bulma ve diğer hizmetlerle iletişim kurma
  * (isteğe bağlı olarak) [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)
  * ...ve erişim diğer birçok yetenekleri için birinci sınıf bir programlama modeli çeşitli programlama dillerinde çekmenize onların.
* Kendi kodunuzu çalıştıran basit bir model programlama modellerini olduğunuz gibi görünüyor. Kodunuz, iyi tanımlanmış bir giriş noktası ve kolayca yönetilen yaşam döngüsü vardır.
* Bir iletişim takılabilir modeli. HTTP ile gibi tercih ettiğiniz bir aktarım kullanın [Web API](service-fabric-reliable-services-communication-webapi.md), WebSockets, özel TCP protokolleri ya da başka bir şey. Reliable Services Giden kutusu seçenekleri kullanabilirsiniz veya kendi sağlayabilir bazı harika sağlar.
* Durum bilgisi olan hizmetler için Reliable Services programlama modeli kullanarak, hizmet içinde durumunu tutarlı ve güvenilir bir şekilde depolamak sağlar [güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md). Güvenilir koleksiyonlar kullanan herkese tanıdık gelecektir yüksek kullanılabilirliğe sahip ve güvenilir koleksiyon sınıfları daha basit bir dizi olan C# koleksiyonları. Geleneksel olarak, hizmetleri güvenilir durum yönetimi için dış sistemler gerekli. Güvenilir koleksiyonlar ile aynı yüksek kullanılabilirlik ve güvenilirlik, yüksek oranda kullanılabilir bir dış mağazalardan beklenir karşılaşmışsınızdır yanındaki işlem durumunuzu depolayabilirsiniz. İşlem ve çalışması için durum birlikte bulundurmak için bu modeli gecikme süresi de artırır.

## <a name="what-makes-reliable-services-different"></a>Reliable Services farklı yapan nedir?
Service Fabric güvenilir hizmetler önce yazılmış hizmetlerinden farklıdır. Service Fabric, güvenilirlik, kullanılabilirlik, tutarlılık ve ölçeklenebilirlik sağlar.

* **Güvenilirlik** - hizmet kalır yukarı güvenilir olmayan ortamlarda bile makineleriniz nerede başarısız veya ağ karşılaşıp karşılaşmadığını veya burada Hizmetleri hata ve kilitlenme karşılaştığınız veya başarısız durumda. Durum bilgisi olan hizmetler için durumu, ağ veya diğer hatalar varsa bile korunur.
* **Kullanılabilirlik** -hizmetiniz, erişilebilir ve hızlı. Service Fabric, istenen numaranızı kopyalarını çalıştırma tutar.
* **Ölçeklenebilirlik** - Hizmetleri belirli donanım ayrılmış ve büyütün veya eklenmesi veya kaldırılmasını donanım veya diğer kaynaklar ile gerektiği gibi Daralt. Hizmetleri kolayca bölümlenmiş (durum bilgisi olan durumda özellikle) hizmetini ölçeklendirebilirsiniz sağlamak ve kısmi hata işleme. Hizmetleri oluşturulabilir ve müşteri isteklerine yanıt olarak dinamik olarak etkinleştirme çalışmaya gerektiği şekilde başlar için daha fazla örnek kod aracılığıyla silindi varsayalım. Son olarak, Service Fabric Hizmetleri basit olmasını önerir. Service Fabric hizmetleri gerektiren veya ayrılmış tüm işletim sistemi örnekleri veya hizmetinin tek bir örneği için işlemler yerine tek bir işlem içinde sağlanacak binlerce sağlar.
* **Tutarlılık** -bu hizmette depolanan tüm bilgileri tutarlı olmasını güvence altına alınabilir. Bu hizmet içinde birden çok güvenilir koleksiyonlar arasında bile geçerlidir. İşlemsel olarak kararlı bir şekilde bir hizmet içinde koleksiyonlar arasında değişiklik yapılabilir.

## <a name="service-lifecycle"></a>Hizmet yaşam döngüsü
Reliable Services, durum bilgisi olan veya durum bilgisi olmayan, hizmeti olup olmadığını hızlı bir şekilde kodunuzda takın ve çalışmaya başlama olanak tanıyan basit bir yaşam döngüsü sağlar.  Hizmetinizi çalışır almak için uygulamanız gereken yalnızca bir veya iki yöntem vardır.

* **CreateServiceReplicaListeners/Createserviceınstancelisteners** -burada kullanmak istediği iletişimi stack(s) hizmeti tanımlayan bu yöntemidir. Aşağıdaki gibi iletişim yığın [Web API](service-fabric-reliable-services-communication-webapi.md), olan uç noktaları ya da (nasıl istemciler hizmete erişmek) hizmeti için uç nokta tanımlar. Ayrıca, görünen iletileri hizmet kodunu geri kalanı ile etkileşimini tanımlar.
* **RunAsync** -hizmetinizin iş mantığını çalıştığı ve burada, hizmet ömrü boyunca çalışması gereken herhangi bir arka plan görevi hız kazandırın, bu yöntem kullanılır. Sağlanan iptal belirtecini bu çalışmanın ne zaman durması gerektiğini sinyaldir. Örneğin, hizmet iletileri güvenilir bir sıra dışında çekme ve bunları işlemeniz gerekiyorsa, bu iş nerede olacağını budur.

Reliable services ilk kez öğrendiğiniz okumaya devam edin! Reliable services yaşam döngüsü için ayrıntılı bilgileri arıyorsanız, attıktan [bu makalede](service-fabric-reliable-services-lifecycle.md).

## <a name="example-services"></a>Örnek Hizmetleri
Bu programlama modeli bilerek parçaların birbirine nasıl uyduğunu görmek için iki farklı Hizmetleri hızlı bir göz atalım.

### <a name="stateless-reliable-services"></a>Güvenilir durum bilgisi olmayan hizmetler
Durum bilgisi olmayan hizmet tek yerdir çağrıları arasında hizmetinde saklanan durum yoktur. Mevcut olan herhangi bir durumu tamamen atılabilir ve eşitleme, çoğaltma, Kalıcılık veya yüksek kullanılabilirlik gerektirmez.

Örneğin, bellek ve tüm hüküm ve aynı anda gerçekleştirilecek işlemleri alır bir hesap makinesi göz önünde bulundurun.

Bu durumda, `RunAsync()` (C#) veya `runAsync()` (Java) hiçbir arka plan görev hizmeti yapmak için gereken işleme olduğundan hizmet boş olabilir. Hesaplayıcı hizmet oluşturulduğunda döndürür bir `ICommunicationListener` (C#) veya `CommunicationListener` (Java) (örneğin [Web API](service-fabric-reliable-services-communication-webapi.md)) bazı bağlantı noktası üzerinde dinleyen bir uç noktası'kurmak açılır. Bu uç noktaları için farklı olan hesaplamayı yöntemleri kancaları (örnek: "Ekleyin. (n1, n2)") Makinesi'ni Genel API tanımlayın.

Bir istemciden bir çağrı yapıldığında, uygun yöntemi çağrılır ve hesaplayıcı hizmet sağlanan veriler üzerinde işlem gerçekleştirir ve sonucu döndürür. Bu, herhangi bir durumu depolamaz.

Herhangi bir iç durum depolamayın Bu örnek hesaplayıcı basitleştirir. Ancak, çoğu Hizmetleri gerçekten durum bilgisiz değildir. Bunun yerine, başka bir deposunda durumlarına federasyonda. (Örneğin, bir yedekleme deposu veya cache oturum durumu tutmayı kullanan herhangi bir web uygulamasını durum bilgisiz değildir.)

Sık kullanılan bir örnek olarak bir ön uç web uygulaması için genel kullanıma yönelik API sunan Service Fabric durum bilgisi olmayan hizmetler kullanılan nasıl. Ön uç hizmeti daha sonra bir kullanıcı isteğini tamamlamak için durum bilgisi olan hizmetler hakkında konuşuyor. Bu durumda, istemcilerden gelen çağrıları burada durum bilgisi olmayan hizmet dinlediği bir bilinen bağlantı noktası için 80 gibi yönlendirilir. Bu durum bilgisi olmayan hizmet çağrısı alır ve çağrı güvenilir bir tarafın olup olmadığını ve hangi hizmet belirler için yönlendirilir.  Ardından, durum bilgisi olmayan hizmet doğru bölüm durum bilgisi olan hizmet çağrısı iletir ve bir yanıt bekler. Durum bilgisi olmayan hizmet bir yanıtı aldığında, özgün istemciye yanıt verir. Böyle bir hizmet içinde örneklerimizi örneğidir [ C# ](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)  /  [Java](https://github.com/Azure-Samples/service-fabric-java-getting-started). Bu örneklerdeki bu düzeni yalnızca bir örnektir, vardır diğerleri de diğer örnekleri.

### <a name="stateful-reliable-services"></a>Durum bilgisi olan Reliable Services
Durum bilgisi olan hizmet durumu işlevi için tutarlı ve hizmetin mevcut tutulan kısmı olmalıdır biridir. Sürekli olarak aldığı güncelleştirmelerini temel alarak belirli bir değerin yuvarlanmış ortalamasını hesaplar bir hizmeti kullanmayı düşünün. Bunu yapmak için ihtiyaç duyduğu işlem ve geçerli ortalama gelen isteklerin geçerli kümesi olmalıdır. Durum bilgisi olan (örneğin, bir Azure blob veya tablo deposunu Bugün) bir dış depolama bilgilerini depolar alır ve işler herhangi bir hizmeti. Yalnızca durumuna dış durum deposunda tutar.

Dış depo olduğundan bu durum için ne güvenilirlik, kullanılabilirlik, ölçeklenebilirlik ve tutarlılık sağlar hizmetlerin çoğu bugün durumlarına harici olarak depolar. Service Fabric'te Hizmetleri durumlarına harici olarak depolamak gerekmez. Service Fabric hizmet kodunu hem de hizmet durumu için bu gereksinimlerin üstlenir.

Resimleri işleyen bir hizmet yazılacak istediğimizi varsayalım. Hizmeti bunu yapmak için bu görüntüyü üzerinde gerçekleştirmek için bir görüntü ve bir dizi dönüşümleri alır. (Şimdi onu bir Webapı olduğunu varsayın) Bu hizmet iletişim dinleyicisini kullanıma sunan bir API gibi sağladığı döndürür `ConvertImage(Image i, IList<Conversion> conversions)`. Bir isteği aldığında, hizmet depolar bir `IReliableQueue`ve istek izleyebilmek için istemci bazı kimliğini döndürür.

Bu hizmette `RunAsync()` daha karmaşık olabilir. Hizmet bir döngü içinde olan kendi `RunAsync()` tanesi istekleri çeker `IReliableQueue` ve istenen dönüştürmeleri gerçekleştirir. Sonuçları depolanan bir `IReliableDictionary` ne zaman istemci geri gelir, dönüştürülen resimlerinin alabilirsiniz. Bir görüntü başarısız olsa bile emin olmak için kayıp değilse, bu güvenilir hizmet sıranın dışında çekme, dönüştürmeler gerçekleştirmek ve sonucu tek bir işlemde tüm depolar. Bu durumda, ileti kuyruktan silinir ve dönüştürmeler tamamlandığında sonuçları Sonuç sözlükte depolanır. Alternatif olarak, hizmet sıradaki görüntü çekme ve hemen sonra bir uzak depoda saklayabilirsiniz. Bu durumu yönetmek için hizmette olduğunu miktarını azaltır, ancak uzak depoda yönetmek için gerekli meta verileri tutmak hizmet beri arttıkça karmaşıklığı sahiptir. Ortada bir şey başarısız olursa her iki yaklaşım ile istek işlenmeyi bekleyen kuyruğunda kalır.

Bu hizmet hakkında dikkat edilecek bir normal bir .NET hizmetine gibi ses şeydir! Tek fark, kullanılan veri yapıları olan (`IReliableQueue` ve `IReliableDictionary`) Service Fabric tarafından sağlanır ve yüksek oranda güvenilir, kullanılabilir ve tutarlı.

## <a name="when-to-use-reliable-services-apis"></a>Güvenilir hizmetler API'lerini kullanmak ne zaman
Ardından uygulama hizmeti gereksinimlerinizi niteleyen aşağıdakilerden herhangi biri, güvenilir hizmetler API'lerini dikkate almanız gerekir:

* Hizmetinizin kodunuzun (ve isteğe bağlı olarak durumu) yüksek oranda kullanılabilir ve güvenilir olması
* İşlem garanti arasında birden çok birimi durumunun (örneğin, sipariş ve sipariş satır öğeleri) gerekir.
* Uygulamanızın durumunu, doğal olarak güvenilir bir sözlük ve Kuyruklar modellenebilir.
* Uygulama kodu veya durumu düşük gecikme süreli okuma ve yazma işlemleri ile yüksek oranda kullanılabilir olması gerekir.
* Ayrıntı düzeyi, işlemler ve eşzamanlılık, bir veya daha fazla güvenilir koleksiyonlar kontrol etmek uygulamanız gerekir.
* Duyuruları yönetmek veya hizmetiniz için bölümleme düzeni denetlemek istiyorsunuz.
* Kodunuzu bir ücretsiz iş parçacıklı çalışma zamanı ortamı gerekir.
* Dinamik olarak oluşturmak veya güvenilir bir sözlük veya kuyruklar veya çalışma zamanında tüm hizmetleri yok etmek uygulamanız gerekir.
* Program aracılığıyla denetlemek Service Fabric tarafından sağlanan yedekleme ve geri yükleme özellikleri hizmetinizin durumu için gerekir.
* Değişiklik geçmişi durumu onun birim korumak uygulamanız gerekir.
* Geliştirme veya üçüncü taraf geliştirilen, özel durum sağlayıcılarının kullanmak istiyorsunuz.

## <a name="next-steps"></a>Sonraki adımlar
* [Reliable Services hızlı başlangıç](service-fabric-reliable-services-quick-start.md)
* [Güvenilir koleksiyonlar](service-fabric-reliable-services-reliable-collections.md)
* [Reliable Actors programlama modeli](service-fabric-reliable-actors-introduction.md)
