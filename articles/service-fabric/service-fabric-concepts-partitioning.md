---
title: Service Fabric hizmetlerini bölümleme | Microsoft Docs
description: Partition Service Fabric durum bilgisi olan hizmetler açıklar. Bölümler, veri ve işlem birlikte ölçeklendirilebilir için yerel makine üzerinde veri depolama sağlar.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
editor: ''
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: atsenthi
ms.openlocfilehash: 833d87dab59890b9903ea8eecf2334d7dd1c7436
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60711937"
---
# <a name="partition-service-fabric-reliable-services"></a>Partition Service Fabric güvenilir Hizmetleri
Bu makalede, Azure Service Fabric güvenilir Hizmetleri bölümleme temel kavramlar tanıtılmaktadır. Makalesinde kullanılan kaynak kodu de kullanılabilir [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="partitioning"></a>Bölümleme
Bölümleme için Service Fabric benzersiz değil. Aslında, ölçeklenebilir hizmetler oluşturmaya çekirdek deseni olduğu. Daha geniş bir anlamda, biz durumu (veriler) ayırma kavram olarak bölümlendirme hakkında düşünmek ve ölçeklenebilirlik ve performansı artırmak için daha küçük erişilebilir birimler halinde işlem. Bölümleme iyi bilinen bir form [verileri bölümleme][wikipartition], parçalama olarak da bilinir.

### <a name="partition-service-fabric-stateless-services"></a>Partition Service Fabric durum bilgisi olmayan hizmetler
Durum bilgisi olmayan hizmetler için bir hizmet bir veya daha fazla örneğini içeren bir mantıksal birimi olan bir bölüm hakkında değerlendirme yapabilirsiniz. Şekil 1 beş örnek bir bölümü kullanarak bir küme genelinde dağıtılan durum bilgisi olmayan hizmet gösterir.

![Durum bilgisi olmayan hizmet](./media/service-fabric-concepts-partitioning/statelessinstances.png)

Aslında iki tür durum bilgisi olmayan hizmet çözümler vardır. İlk durumuna dışarıdan, örneğin (oturum bilgilerini ve veri depolayan bir Web sitesi için gibi) bir Azure SQL veritabanı'nda kalıcı bir hizmettir. İkinci herhangi bir kalıcı durum yönetmeyin yalnızca Hesaplama Hizmetleri (gibi bir hesaplayıcı veya görüntüyü küçük resim oluşturma) olur.

İçinde çalışması, durum bilgisi olmayan hizmet bölümleme, çok nadir bir senaryodur--ölçeklenebilirlik ve kullanılabilirlik normal olarak elde edilen daha fazla örnek ekleyerek. Durum bilgisi olmayan hizmet örneklerine yönelik birden çok bölüm kullanmayı yalnızca bir kez, özel yönlendirme isteklerini karşılamak için ihtiyacınız andır.

Örneğin, burada kimliklerine sahip kullanıcılar belirli bir aralıktaki belirli hizmet örneği tarafından yalnızca hizmet verilen bir durum düşünün. Gerçekten bölümlenmiş bir arka uç (ör parçalı SQL veritabanı) sahip ve hangi hizmet örneğine veritabanı parçaya--yazma veya diğer hazırlık çalışması içinde gerçekleştirmek denetim istediğinizde ne zaman bir durum bilgisi olmayan hizmet bölümleme, başka bir örnek verilmiştir aynı bölümleme bilgileri gerektiren durum bilgisi olmayan hizmeti, arka uçtaki kullanılır. Bu tür senaryolar farklı şekillerde çözülebilir ve hizmet bölümleme mutlaka gerektirmez.

Bu kılavuzda kalan durum bilgisi olan hizmetler üzerinde odaklanır.

### <a name="partition-service-fabric-stateful-services"></a>Partition Service Fabric durum bilgisi olan hizmetler
Service Fabric, ölçeklenebilir bir durum bilgisi olan hizmetler bölüm durumu (veriler) için birinci sınıf bir yolunu sunarak geliştirme kolaylaştırır. Kavramsal olarak, son derece güvenilirdir aracılığıyla, bir ölçek birimi olarak hakkında bir durum bilgisi olan hizmet ilişkin bir bölüm düşünebilirsiniz [çoğaltmaları](service-fabric-availability-services.md) , dağıtılmış ve bir küme içindeki düğümler arasında dengeli.

Service Fabric durum bilgisi olan hizmetler bağlamında bölümleme belirli hizmet bölüm tam hizmet durumunu değerinin bir bölümü için sorumlu olduğunu belirleme işlemi ifade eder. (Daha önce belirtildiği gibi bir dizi bölümdür [çoğaltmaları](service-fabric-availability-services.md)). Service Fabric hakkında harika bir şey, bunu bölümleri farklı düğümlere yerleştirir olabilir. Bu düğümün kaynak sınırı büyümesine sağlar. Veri büyümesi gerektiğinde gibi bölümlerini büyütmenize ve Service Fabric bölümler düğümleri arasında yeniden dengeler. Bu, donanım kaynakları verimli kullanmaya devam sağlar.

Bir örnek vermek için 5 düğümlü bir küme ve 10 bölümleri ve üç kopyaya hedefi olacak şekilde yapılandırılmış bir hizmet başlayın varsayalım. Bu durumda, Service Fabric dengelemek ve çoğaltmaları--kümede dağıtma ve iki birincil ile bitecek [çoğaltmaları](service-fabric-availability-services.md) düğüm başına.
Artık 10 düğümü kümeye ölçeğini genişletmek gerekiyorsa, Service Fabric birincil yeniden dengelemeniz [çoğaltmaları](service-fabric-availability-services.md) 10 tüm düğümlerde. 5 düğümlerine ölçeği, benzer şekilde, Service Fabric tüm çoğaltmalar 5 düğümler arasında yeniden dengelemeniz.  

Şekil 2 önce ve sonra kümeyi ölçeklendirme 10 bölümler dağılımını gösterir.

![Durum bilgisi olan hizmet](./media/service-fabric-concepts-partitioning/partitions.png)

Sonuç olarak, ölçeği genişletme, istemcilerden gelen istekleri bilgisayara dağıtılır, uygulamanın genel performansını geliştirdik ve öbeklere veri erişim çekişmesini sınırlı olduğundan elde edilir.

## <a name="plan-for-partitioning"></a>Bölümleme planı
Bir hizmet uygulamadan önce her zaman ölçeğini genişletmek için gerekli olan bölümleme stratejisi düşünmelisiniz. Farklı yolu vardır, ancak bunların tümünde uygulama elde etmek için gerekenler üzerinde odaklanın. Bu makalede bağlam için bazı önemli yönlerinden düşünelim.

İlk adım olarak bölümlenmiş gereken durumu yapısını düşünün iyi bir yaklaşımdır.

Basit bir örneği ele alalım. Ülke genelindeki yoklama için bir hizmet oluşturmak için olsaydı, her şehir için bir bölüm içinde ilçe oluşturabilirsiniz. Ardından, bu şehir için karşılık gelen bir bölüme şehirde her kişi için oy saklayabilirsiniz. Şekil 3'te kişiler ve bunların bulunduğu şehir kümesi gösterilmektedir.

![Basit bölüm](./media/service-fabric-concepts-partitioning/cities.png)

Şehirlerin nüfuslarıyla yaygın değiştikçe, çok miktarda veri (örneğin, Seattle) içeren bazı bölümleri ve çok az durumu (örn: Kirkland) ile diğer bölümler bitirebilirsiniz. Bu nedenle durumu eşit miktarda bölümlemeye sahip olmak etkisi nedir?

Örnek yeniden düşünüyorsanız, Seattle için oy tutan bölüm Kirkland değerinden daha fazla trafik alırsınız kolayca görebilirsiniz. Varsayılan olarak, Service Fabric olduğundan her düğüme birincil ve ikincil çoğaltmalarda aynı sayıda hakkında emin sağlar. Bu nedenle, daha az trafik hizmet daha fazla trafik ve diğer hizmet çoğaltmaları barındıran düğümleri bitirebilirsiniz. Tercihen, bir kümedeki böyle sıcak ve soğuk noktalardan kaçınacak şekilde istersiniz.

Bunu önlemek için bir bölümleme açısından bakıldığında iki işlem yapmanız gerekir:

* Böylece tüm bölümler arasında eşit olarak dağıtılmış, durum bölümlemek deneyin.
* Her bir çoğaltma hizmeti için yükü raporlamak. (Bu makalede atın hakkında daha fazla bilgi için [ölçümleri ve yük](service-fabric-cluster-resource-manager-metrics.md)). Service Fabric, bellek veya kayıt sayısı gibi hizmetleri tarafından kullanılan rapor yükleme yeteneği sağlar. Service Fabric, bildirilen ölçümlere göre bazı bölümleri diğerlerinden daha yüksek yüklerini sunan ve böylece hiçbir düğüm genel aşırı daha uygun düğümlerine çoğaltmaları taşıyarak küme yeniden dengeler algılar.

Bazen, belirli bir bölümünde ne kadar veri olacaktır bilemezsiniz. Genel bir öneri her ikisi de--yapmak için bu nedenle ilk olarak, bölümleme stratejisi benimseyerek, verileri eşit olarak bölümleri ve saniye raporlama yük tarafından yayılan.  İlk yöntem, zaman içinde geçici erişim veya yük farklılıkları düzgünleştirme ikinci yardımcı olmanın oylama örnekte açıklanan durumlarda engeller.

Doğru bölüm sayısı ile başlaması için bölüm planlamanın bir diğer unsuru seçmektir.
Service Fabric açısından bakıldığında, hiçbir şey yoktur, senaryonuz için beklenenden daha yüksek bir bölüm sayısı ile başlıyor engeller.
Aslında, en yüksek bölüm sayısı varsayılarak bir geçerli bir yaklaşımdır.

Nadir durumlarda, ilk başta seçtiğiniz çok daha fazla bölüm yapmanızın sonlandırabiliriz. Olaydan sonra bölüm sayısı değiştirilemez gibi aynı hizmet türünün yeni bir hizmet örneği oluşturma gibi bazı gelişmiş bölüm yaklaşımları uygulamak gerekir. İstemci kodunuz korumalıdır istemci-tarafı bilgisini temel alarak doğru hizmet örneğine istekleri yönlendiren bazı istemci tarafı mantığını uygulamak gerekir.

Planlama bölümleme için başka bir mevcut bilgisayar kaynaklarına noktadır. Durumu depolanır ve gerektiğinde, izlemek için bağlıdır:

* Ağ bant genişliği sınırlarını
* Sistem bellek sınırları
* Disk depolama sınırları

Bu nedenle çalıştıran bir kümedeki kaynak kısıtlamaları yaşarsanız ne olur? Yeni gereksinimleri karşılamak için kümedeki yalnızca ölçeklendirebilirsiniz cevaptır.

[Kapasite Planlama Kılavuzu](service-fabric-capacity-planning.md) kümeniz için gerekli kaç düğümleri öğrenmek için yönergeler sunar.

## <a name="get-started-with-partitioning"></a>Bölümlendirme ile çalışmaya başlama
Bu bölümde, hizmetiniz bölümleme ile çalışmaya başlama açıklar.

Service Fabric üç bölüm şemaları seçeneği sunar:

* Aralıklı (UniformInt64Partition da bilinir) bölümleme.
* Bölümleme adı. Genellikle bu modeli kullanan uygulamalar, sınırlanmış kümesinde bucketed veri var. Veri alanlarının adlandırılmış bölüm anahtarlarının kullanılması sık karşılaşılan örneklerden bazıları, bölge, posta kodları, müşteri gruplarına veya diğer iş sınırları olacaktır.
* Singleton bölümleme. Singleton bölümler genellikle hizmet herhangi bir ek yönlendirme gerektirmez kullanılır. Örneğin, durum bilgisi olmayan hizmetler, varsayılan olarak bu bölümleme düzeni kullanın.

Adlandırılmış ve tek bölümleme düzenleri aralıklı bölümlerinin özel biçimler. En yaygın ve kullanışlı bir olduğu gibi varsayılan olarak, bölümleme, Service Fabric kullanmak için Visual Studio şablonları aralıklı. Bu makalenin geri kalanında ranged bölümleme düzeni üzerinde odaklanır.

### <a name="ranged-partitioning-scheme"></a>Aralıklı bölümleme düzeni
Bu, bir tamsayı aralığı (düşük anahtarı ve yüksek anahtar tarafından tanımlanır) ve bölümleri (n) sayısını belirtmek için kullanılır. N bölümler, çakışmayan dizinin alt genel bölüm anahtar aralığı için her sorumlu oluşturur. Örneğin, bir ranged bölümleme düzeni düşük anahtar 0 ile 99 yüksek anahtarına ve sayı 4'ün dört bölüm aşağıda gösterildiği gibi oluşturursunuz.

![Aralık bölümleme](./media/service-fabric-concepts-partitioning/range-partitioning.png)

Veri kümesi içinde benzersiz bir anahtara göre bir karma değer oluşturmak yaygın bir yaklaşımdır. Bazı genel örnekleri anahtarların bir araç kimlik numarası (Toplamıdır), çalışan kimliği veya benzersiz bir dize olacaktır. Bu benzersiz bir anahtar kullanarak, bir karma kod mod, anahtar olarak kullanılacak anahtar aralığı karşılaşırsınız. İzin verilen anahtar aralığın alt ve üst sınırları belirtebilirsiniz.

### <a name="select-a-hash-algorithm"></a>Karma algoritması seçin
Karma önemli bir bölümü, karma algoritması seçmektir. Hedef birbirine yakın (yerleşim yeri hassas karma)--benzer anahtarları grubunda olup bir husustur veya etkinlik problem (karma dağıtım) tüm bölümler arasında dağıtılması gerektiğini, daha yaygın olduğu.

İyi dağıtım karma algoritma özelliklerini şunlardır: işlem kolaydır, birkaç çakışmaları olan ve anahtarları eşit olarak dağıtır. Verimli bir karma algoritması iyi bir örnektir [FNV 1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) karma algoritması.

Genel karma kod algoritması seçenekleri için iyi bir kaynaktır [karma işlevlerini Wikipedia sayfasında](https://en.wikipedia.org/wiki/Hash_function).

## <a name="build-a-stateful-service-with-multiple-partitions"></a>Birden çok bölüm ile durum bilgisi olan hizmet oluşturma
Bir ilk durum bilgisi olan güvenilir hizmet ile birden çok bölüm oluşturalım. Bu örnekte, aynı bölüm içindeki aynı harfi ile başlayan tüm adların saklamak istediğiniz çok basit bir uygulama oluşturacaksınız.

Herhangi bir kod yazmadan önce bölümler ve bölüm anahtarları hakkında düşünmeniz gerekir. 26 bölümleri (için bir tane alfabedeki her harfe), düşük ve yüksek anahtarlar hakkında gerekenler?
Tam anlamıyla harfi her bir bölüm olmasını istiyoruz gibi kendi anahtarını her harf olduğundan 0 düşük anahtarı ve 25 yüksek anahtar kullanabiliriz.

> [!NOTE]
> Gerçekte dağılımı düzensiz olduğu gibi basit bir senaryo budur. "S" veya "M" harflerle başlayan son adları "X" ile başlayan yapılandırılanlardan daha yaygın veya "Y".
> 
> 

1. Açık **Visual Studio** > **dosya** > **yeni** > **proje**.
2. İçinde **yeni proje** iletişim kutusunda, Service Fabric uygulamasını seçin.
3. Proje "AlphabetPartitions" çağırın.
4. İçinde **bir hizmet oluşturma** iletişim kutusunda **durum bilgisi olan** hizmet ve "Alphabet.Processing" çağırın.
5. Bölüm sayısını ayarlayın. ApplicationPackageRoot AlphabetPartitions proje klasöründe bulunan Applicationmanifest.xml dosyasını açın ve aşağıda gösterildiği gibi 26'parametresi Processing_PartitionCount güncelleştirin.
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    Ayrıca aşağıda gösterildiği gibi ApplicationManifest.xml StatefulService öğesinde LowKey ve HighKey özelliklerini güncelleştirmek gerekir.
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. Hizmet tarafından erişilebilir olması bir uç nokta bağlantı noktası (PackageRoot klasöründe bulunur) ServiceManifest.xml bitiş öğesi ekleyerek aşağıda gösterildiği gibi Alphabet.Processing hizmeti için açmak:
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    Artık hizmeti, 26 bölümleri olan bir iç uç nokta için dinleyecek şekilde yapılandırılmıştır.
7. Ardından, geçersiz kılmak ihtiyacınız `CreateServiceReplicaListeners()` işleme sınıfının yöntemi.
   
   > [!NOTE]
   > Bu örnek, bir basit HttpCommunicationListener kullandığınız varsayılır. Güvenilir hizmet iletişimi hakkında daha fazla bilgi için bkz. [güvenilir hizmet iletişim modelini](service-fabric-reliable-services-communication.md).
   > 
   > 
8. Bir çoğaltma dinlediği URL için önerilen Düzen şu biçimdir: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.
    Bu nedenle doğru uç noktaları ve bu düzendeki dinlemek, iletişim dinleyicisini yapılandırmak istiyorsunuz.
   
    Bu adres çoğaltmaya benzersiz olması gerekir, bu hizmetin birden fazla çoğaltma aynı bilgisayarda barındırılabileceği. Bölüm kimliği + çoğaltma kimliği URL'de olan nedeni budur. URL öneki benzersiz olduğu sürece, aynı bağlantı noktasında birden çok adresi üzerinde HttpListener dinleyebilirsiniz.
   
    Ek GUID ikincil çoğaltmaları da salt okunur isteklerini dinlemek burada Gelişmiş bir servis talebi için yoktur. Bu durumda, yeni bir benzersiz adresi birincil ikincil siteden geçiş yaparken adresini yeniden çözümlemek için istemcileri zorlamak için kullanılmadığından emin olmanız gerekir. '+', böylece kullanılabilir tüm konakları üzerinde (IP, FQDN, localhost, vb.) çoğaltma dinler burada adresi olarak kullanılır Aşağıdaki kod örneği gösterilmektedir.
   
    ```CSharp
    protected override IEnumerable<ServiceReplicaListener> CreateServiceReplicaListeners()
    {
         return new[] { new ServiceReplicaListener(context => this.CreateInternalListener(context))};
    }
    private ICommunicationListener CreateInternalListener(ServiceContext context)
    {
   
         EndpointResourceDescription internalEndpoint = context.CodePackageActivationContext.GetEndpoint("ProcessingServiceEndpoint");
         string uriPrefix = String.Format(
                "{0}://+:{1}/{2}/{3}-{4}/",
                internalEndpoint.Protocol,
                internalEndpoint.Port,
                context.PartitionId,
                context.ReplicaOrInstanceId,
                Guid.NewGuid());
   
         string nodeIP = FabricRuntime.GetNodeContext().IPAddressOrFQDN;
   
         string uriPublished = uriPrefix.Replace("+", nodeIP);
         return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInternalRequest);
    }
    ```
   
    Ayrıca, yayımlanmış URL'sini dinleme URL ön ekini biraz farklıdır hatalarının ayıklanabileceğini belirtmekte yarar.
    Dinleme URL'si için HttpListener verilir. Yayımlanmış URL'sini, Service Fabric adlandırma hizmeti bulma için kullanılan hizmet, yayımlanan URL'dir. İstemciler, bulma hizmeti ile bu adresi ister. İstemciler alma adresi gerçek IP veya FQDN düğümünün bağlanmak için olmalıdır. Değerini değiştirmeniz için '+' düğümün IP veya FQDN, yukarıda gösterildiği gibi.
9. Son adım, aşağıda gösterildiği gibi hizmete işleme mantığının eklemektir.
   
    ```CSharp
    private async Task ProcessInternalRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        string output = null;
        string user = context.Request.QueryString["lastname"].ToString();
   
        try
        {
            output = await this.AddUserAsync(user);
        }
        catch (Exception ex)
        {
            output = ex.Message;
        }
   
        using (HttpListenerResponse response = context.Response)
        {
            if (output != null)
            {
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    private async Task<string> AddUserAsync(string user)
    {
        IReliableDictionary<String, String> dictionary = await this.StateManager.GetOrAddAsync<IReliableDictionary<String, String>>("dictionary");
   
        using (ITransaction tx = this.StateManager.CreateTransaction())
        {
            bool addResult = await dictionary.TryAddAsync(tx, user.ToUpperInvariant(), user);
   
            await tx.CommitAsync();
   
            return String.Format(
                "User {0} {1}",
                user,
                addResult ? "successfully added" : "already exists");
        }
    }
    ```
   
    `ProcessInternalRequest` bölüm ve çağrılarını çağırmak için kullanılan sorgu dizesi parametresi değerlerini okur `AddUserAsync` lastname güvenilir sözlüğüne eklenecek `dictionary`.
10. Durum bilgisi olmayan hizmet belirli bir bölüme nasıl çağırabilirsiniz görmek için projeye ekleyelim.
    
    Bu hizmet, lastname bir sorgu dizesi parametresi olarak kabul eder, bölüm anahtarı belirler ve işleme için Alphabet.Processing hizmetine gönderir basit bir web arabirimi olarak görev yapar.
11. İçinde **bir hizmet oluşturma** iletişim kutusunda **durum bilgisi olmayan** hizmet ve aşağıda gösterildiği gibi "Alphabet.Web" çağırın.
    
    ![Durum bilgisi olmayan hizmet ekran görüntüsü](./media/service-fabric-concepts-partitioning/createnewstateless.png).
12. ServiceManifest.xml aşağıda gösterildiği gibi bir bağlantı noktası açmanız Alphabet.WebApi hizmetinin uç noktası bilgileri güncelleştirin.
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. Web sınıfında ServiceInstanceListeners koleksiyonunu döndürülecek gerekir. Basit bir HttpCommunicationListener uygulamak isteyebilirsiniz.
    
    ```CSharp
    protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
    {
        return new[] {new ServiceInstanceListener(context => this.CreateInputListener(context))};
    }
    private ICommunicationListener CreateInputListener(ServiceContext context)
    {
        // Service instance's URL is the node's IP & desired port
        EndpointResourceDescription inputEndpoint = context.CodePackageActivationContext.GetEndpoint("WebApiServiceEndpoint")
        string uriPrefix = String.Format("{0}://+:{1}/alphabetpartitions/", inputEndpoint.Protocol, inputEndpoint.Port);
        var uriPublished = uriPrefix.Replace("+", FabricRuntime.GetNodeContext().IPAddressOrFQDN);
        return new HttpCommunicationListener(uriPrefix, uriPublished, this.ProcessInputRequest);
    }
    ```
14. Artık işleme mantığı uygulamanız gerekir. HttpCommunicationListener çağrıları `ProcessInputRequest` ne zaman bir istek halinde sunulur. Bu nedenle devam edelim ve aşağıdaki kodu ekleyin.
    
    ```CSharp
    private async Task ProcessInputRequest(HttpListenerContext context, CancellationToken cancelRequest)
    {
        String output = null;
        try
        {
            string lastname = context.Request.QueryString["lastname"];
            char firstLetterOfLastName = lastname.First();
            ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    
            ResolvedServicePartition partition = await this.servicePartitionResolver.ResolveAsync(alphabetServiceUri, partitionKey, cancelRequest);
            ResolvedServiceEndpoint ep = partition.GetEndpoint();
    
            JObject addresses = JObject.Parse(ep.Address);
            string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
            UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
            primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
            string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    
            output = String.Format(
                    "Result: {0}. <p>Partition key: '{1}' generated from the first letter '{2}' of input value '{3}'. <br>Processing service partition ID: {4}. <br>Processing service replica address: {5}",
                    result,
                    partitionKey,
                    firstLetterOfLastName,
                    lastname,
                    partition.Info.Id,
                    primaryReplicaAddress);
        }
        catch (Exception ex) { output = ex.Message; }
    
        using (var response = context.Response)
        {
            if (output != null)
            {
                output = output + "added to Partition: " + primaryReplicaAddress;
                byte[] outBytes = Encoding.UTF8.GetBytes(output);
                response.OutputStream.Write(outBytes, 0, outBytes.Length);
            }
        }
    }
    ```
    
    Şimdi arkasını adım adım yol gösterir. Sorgu dizesi parametresi ilk harfini kodu okur `lastname` içine bir karakter. Ardından, onaltılık değerini çıkararak bu harfi için bölüm anahtarı belirler `A` onaltılık değerinin son adlarının ilk harflerini.
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    Bu örnekte, 26 bölümler bölüm başına tek bir bölüm anahtarı ile kullanıyoruz unutmayın.
    Ardından, biz hizmeti bölümü elde `partition` kullanarak bu anahtar için `ResolveAsync` metodunda `servicePartitionResolver` nesne. `servicePartitionResolver` olarak tanımlanır
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    `ResolveAsync` Hizmet URI'si, bölüm anahtarı ve bir iptal belirteci parametreleri olarak yöntemi alır. İşleme hizmeti için URI hizmetidir `fabric:/AlphabetPartitions/Processing`. Ardından, uç nokta bölümünün alın.
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    Son olarak, uç nokta URL'si artı querystring oluşturmamızı ve işleme hizmeti çağırın.
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    İşleme tamamlandığında, biz çıkış geri yazma.
15. Son adım, hizmeti test etmektir. Visual Studio kullanan uygulama parametreleri, yerel ve bulut dağıtımı. Yerel olarak 26 bölümleri olan hizmeti test etmek için güncelleştirmeye gerek duyduğunuz `Local.xml` aşağıda gösterildiği gibi AlphabetPartitions proje ApplicationParameters klasöründe dosya:
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. Dağıtımını tamamladıktan sonra hizmeti ve tüm bölümleri Service Fabric Explorer'da kontrol edebilirsiniz.
    
    ![Service Fabric Explorer ekran görüntüsü](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. Bir tarayıcıda girerek bölümleme mantığı sınayabilirsiniz `http://localhost:8081/?lastname=somename`. Aynı harfi ile başlayan her Soyadı aynı bölümde depolandığını görürsünüz.
    
    ![Tarayıcı ekran görüntüsü](./media/service-fabric-concepts-partitioning/samplerunning.png)

Tüm kaynak kodu örnek [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="reliable-services-and-actor-forking-subprocesses"></a>Reliable Services ve alt işlemden çatal aktör
Service Fabric güvenilir Hizmetleri ve daha sonra reliable actors alt işlemden çatal desteklemiyor. Neden olmadığını desteklenen bir örnektir [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext?view=azure-dotnet) desteklenmeyen bir alt kaydetmek için kullanılamaz ve iptal belirteçlerini yalnızca gönderilen kayıtlı işler; sorunları, her tür gibi kaynaklanan alt işlemden kapatmayın, üst işlemdeki bir iptal belirteci aldıktan sonra hatalar'ı yükseltin. 

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramlarla ilgili daha fazla bilgi için aşağıdakilere bakın:

* [Service Fabric hizmetlerinin kullanılabilirliği](service-fabric-availability-services.md)
* [Service Fabric hizmetlerinin ölçeklenebilirliği](service-fabric-concepts-scalability.md)
* [Kapasite planlaması için Service Fabric uygulamaları](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
