---
title: "Service Fabric Hizmetleri bölümleme | Microsoft Docs"
description: "Service Fabric durum bilgisi olan hizmetler bölüm açıklar. Bölümler, veri ve işlem birlikte Genişletilebilir şekilde yerel makinede veri depolama sağlar."
services: service-fabric
documentationcenter: .net
author: msfussell
manager: timlt
editor: 
ms.assetid: 3b7248c8-ea92-4964-85e7-6f1291b5cc7b
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/30/2017
ms.author: msfussell
ms.openlocfilehash: 3c1e80305cb65f41a6981b99f69e8b87f89599ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="partition-service-fabric-reliable-services"></a>Bölüm Service Fabric güvenilir hizmetler
Bu makalede Azure Service Fabric güvenilir hizmetler bölümlendirme, temel kavramlar tanıtılmaktadır. Makalede kullanılan kaynak kodunu da kullanılabilir [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="partitioning"></a>Bölümleme
Bölümleme için Service Fabric benzersiz değil. Aslında, ölçeklenebilir hizmetler oluşturmaya bir çekirdek desenini değil. Daha geniş anlamda, biz durumu (veri) ayırma kavram olarak bölümlendirme hakkında düşünün ve ölçeklenebilirliği ve performansı artırmak için daha küçük erişilebilir birimlerine işlem. Bölümlendirme, iyi bilinen bir form [veri bölümlendirme][wikipartition], parçalama olarak da bilinir.

### <a name="partition-service-fabric-stateless-services"></a>Bölüm Service Fabric durum bilgisi olmayan hizmetler
Durum bilgisi olmayan hizmetler için bir hizmet bir veya daha fazla örneklerini içeren bir birimi olan bir bölüm hakkında düşünebilirsiniz. Şekil 1 bir durum bilgisi olmayan hizmetin bir bölüm kullanarak bir kümede dağıtılmış beş örneğiyle gösterir.

![Durum bilgisiz hizmeti](./media/service-fabric-concepts-partitioning/statelessinstances.png)

Gerçekten durum bilgisiz hizmet çözümlerine iki tür vardır. İlk durumuna harici olarak örneğin (gibi verilere ve oturum bilgilerini depolayan bir Web sitesi) bir Azure SQL veritabanında kalıcı bir hizmettir. İkinci herhangi kalıcı durumunu yönetme değil yalnızca Hesaplama Hizmetleri (örneğin, bir hesap makinesi veya görüntü küçük resim oluşturma) olur.

İçinde bir durum bilgisi olmayan hizmetin bölümlendirme, çok nadir bir senaryo--ölçeklenebilirlik bir durumdur ve kullanılabilirlik normalde elde fazla örneğe ekleyerek. Özel yönlendirme isteklerini karşılamak ihtiyacınız olduğunda, durum bilgisiz hizmet örnekleri için birden çok bölüm isteyip istemediğinize karar yalnızca bir kez gösterilecektir.

Örnek olarak, burada belirli bir aralıkla kimlikleri kullanıcılar yalnızca belirli bir hizmet örneği tarafından sunulması bir durum düşünün. Gerçekten bölümlenmiş arka uç (ör parçalı SQL veritabanı) sahip olduğunuzda ve hangi hizmet örneği için veritabanı parça--yazma veya arka kullanılan aynı bölümleme bilgi gerektirir durum bilgisiz hizmeti içindeki diğer hazırlık işlemleri gerçekleştirmesi denetlemek istediğinizde bir durum bilgisi olmayan hizmetin ne zaman bölüm, başka bir örnek verilmiştir. Bu senaryo türlerini farklı şekillerde çözülebilir ve hizmet bölümleme gerektirmeyebilecek.

Bu kılavuzda kalan durum bilgisi olan hizmetler odaklanır.

### <a name="partition-service-fabric-stateful-services"></a>Bölüm Service Fabric durum bilgisi olan hizmetler
Service Fabric bölümü durumu (veri) için birinci sınıf bir yol sunarak ölçeklenebilir durum bilgisi olan hizmetler geliştirmek kolaylaştırır. Üzerinden yüksek oranda güvenilir bir ölçek birimi olarak bir durum bilgisi olan hizmet bölüm hakkında kavramsal olarak, düşünebilirsiniz [çoğaltmaları](service-fabric-availability-services.md) , dağıtılmış ve bir kümedeki düğümleri arasında dengeli.

Service Fabric durum bilgisi olan hizmetleri bağlamında bölümleme belirli bir hizmet bölümü hizmetinin tam durumunu bir kısmı için sorumlu olduğunu belirleme işlemi ifade eder. (Önce belirtildiği gibi bir bölüm kümesidir [çoğaltmaları](service-fabric-availability-services.md)). Service Fabric hakkında önemli bir şey, bu bölümler farklı düğümlerde yerleştirir ' dir. Bu düğümün kaynak sınırına ulaşması sağlar. Veri büyüme gereksinimlerine göre bölümleri büyümesine ve Service Fabric bölümleri düğümleri arasında yeniden dengeler. Bu donanım kaynaklarının sürekli verimli kullanılmasını sağlar.

Bir örnek vermek için 5 düğümlü bir küme ve 10 bölümler ve üç çoğaltmaları hedefi olacak biçimde yapılandırılmış bir hizmet başlayın söyleyin. Bu durumda, Service Fabric dengelemek ve çoğaltmalar, küme üzerinde--dağıtır ve iki birincil ile bitecek [çoğaltmaları](service-fabric-availability-services.md) düğüm başına.
Şimdi 10 düğümler kümeye ölçeğini gerekiyorsa, Service Fabric birincil yeniden dengelemeniz [çoğaltmaları](service-fabric-availability-services.md) tüm 10 düğümleri arasında. 5 düğümlerine ölçeği, benzer şekilde, Service Fabric tüm çoğaltmalar 5 düğümleri arasında yeniden dengelemeniz.  

Şekil 2 önce ve sonra küme ölçeklendirme 10 bölümleri dağılımını gösterir.

![Durum bilgisi olan hizmet](./media/service-fabric-concepts-partitioning/partitions.png)

Sonuç olarak, genişleme istemcilerinden gelen istekleri bilgisayarlar arasında dağıtılır, uygulamanın genel performansı geliştirildi ve veri öbekleri erişim Çekişme sınırlı olduğundan elde edilir.

## <a name="plan-for-partitioning"></a>Bölümleme planlama
Bir hizmet uygulamadan önce her zaman genişletmek için gereken bölümleme stratejisine düşünmelisiniz. Farklı yolu vardır, ancak bunların tümünün uygulama elde etmek gerekenler üzerinde odaklanın. Bu makalede bağlam için şimdi daha önemli yönlerinden bazıları göz önünde bulundurun.

İlk adım olarak bölümlenmiş gerekiyor durumu yapısı hakkındaki görüşlerinizi iyi bir yaklaşımdır.

Basit bir örnek atalım. Countywide yoklama için bir hizmet oluşturmak için olsaydı, her şehir için bir bölüm ilçe oluşturabilirsiniz. Ardından, bu Şehir karşılık gelen bir bölüme şehirde her kişi için oy saklayabilirsiniz. Şekil 3, kişiler ve bunlar bulunduğu şehir kümesi gösterilmektedir.

![Basit bölüm](./media/service-fabric-concepts-partitioning/cities.png)

Şehir popülasyonunu yaygın değiştikçe, çok miktarda veri (örneğin, Seattle) içeren bazı bölümleri ve çok az durumu (örneğin Kirkland) ile diğer bölümleri ile bitirebilirsiniz. Bu nedenle durumu eşit miktarda bölümlemeye sahip olmak etkisi nedir?

Örnek hakkında yeniden düşünüyorsanız, Seattle için oy tutan bölüm Kirkland'den daha fazla trafik alırsınız kolayca görebilirsiniz. Varsayılan olarak, Service Fabric olduğunu birincil ve ikincil çoğaltmaları her düğümde aynı sayıda hakkında emin olur. Bu nedenle, daha az trafik hizmet daha fazla trafik ve diğerleri hizmet çoğaltmaları tutun düğümleriyle bitirebilirsiniz. Tercihen şöyle sıcak ve soğuk noktalar bir kümede önlemek isteyebilirsiniz.

Bu durumu önlemek için bir bölümleme açısından iki işlem yapmanız gerekir:

* Böylece tüm bölümleri arasında eşit olarak dağıtılır durumu bölümlemek deneyin.
* Her bir çoğaltma hizmeti için yükü raporlamak. (Bu makalede hakkında daha fazla bilgi için kontrol [ölçümleri ve yük](service-fabric-cluster-resource-manager-metrics.md)). Service Fabric bellek miktarı veya kayıt sayısı gibi hizmetleri tarafından tüketilen rapor yükleme yeteneği sağlar. Bildirilen ölçülerine bağlı olarak, Service Fabric bazı bölümleri diğerlerinden daha yüksek yüklerini hizmet veren ve böylece hiçbir düğümü genel aşırı daha uygun düğümlerine çoğaltmaları taşıyarak küme yeniden dengeler algılar.

Bazı durumlarda, ne kadar veri belli bir bölüm olacak bilemezsiniz. Her ikisi de--yapmak için genel bir öneri olacak şekilde ilk olarak, bölümleme stratejisine kabul ederek, veri eşit bölümleri ve saniye raporlama yük tarafından yayar.  İlk yöntem oylama örnekte, zaman içinde erişim ya da yük geçici farklılıkları düzgünleştirme ikinci yardımcı sırasında açıklanan durumlarda engeller.

Bölüm planlama başka bir boyut doğru bölüm sayısı başından itibaren seçmektir.
Service Fabric açısından bakıldığında, hiçbir şey yoktur senaryonuz için beklenenden daha yüksek bir bölüm sayısı ile başlıyor engeller.
Aslında, en çok bölüm sayısı varsayarak bir geçerli bir yaklaşımdır.

Nadir durumlarda, ilk başta seçtiğiniz olandan daha fazla bölüm gerek yukarı bitebilir. Olaydan sonra bölüm sayısı değiştirilemez olarak aynı hizmet türüne ilişkin yeni bir hizmet örneği oluşturma gibi bazı gelişmiş bölüm yaklaşımlar uygulamak gerekir. İstekleri, istemci kodunuzun korumalıdır istemci-tarafı bilgisini temel alarak doğru hizmet örneği yönlendirir bazı istemci-tarafı mantığı uygulamanız gerekir.

Planlama bölümleme için başka bir mevcut bilgisayar kaynaklarına noktadır. Erişilen ve depolanacak durumu gereksinimleriniz değiştikçe izleyin bağlıdır:

* Ağ bant genişliği sınırları
* Sistem bellek sınırları
* Disk depolama sınırları

Bu nedenle çalıştıran bir kümedeki kaynak kısıtlamaları içine çalıştırırsanız ne olur? Yeni gereksinimleri karşılamak için Küme yalnızca ölçeklendirebilirsiniz cevaptır.

[Kapasite Planlama Kılavuzu](service-fabric-capacity-planning.md) kümenizi gereken kaç düğümleri belirleme için yönergeler sunar.

## <a name="get-started-with-partitioning"></a>Bölümlendirme ile çalışmaya başlama
Bu bölümde, hizmetiniz bölümlendirme ile çalışmaya başlama açıklar.

Service Fabric üç bölümleme şeması seçeneği sunar:

* (Uniformınt64partition da bilinir) bölümleme aralıklı.
* Bölümleme adı. Bu model genelde kullanan uygulamalar, sınırlı kümesinde bucketed veri içeriyor. Adlandırılmış bölüm anahtarları olarak kullanılan veri alanlarının sık karşılaşılan örnekleri, bölge, posta kodları, müşteri grupları veya diğer iş sınırları olacaktır.
* Tek bölümleme. Hizmeti herhangi bir ek yönlendirme gerekmediğinde singleton bölümler genellikle kullanılır. Örneğin, durum bilgisi olmayan hizmetler varsayılan olarak bu bölümleme düzenini kullanın.

Adlandırılmış ve özel formları aralıklı bölümlerinin tek bölümleme şemaları geçerlidir. En yaygın ve kullanışlı bir olduğu gibi varsayılan olarak, bölümleme, Service Fabric kullanmak için Visual Studio şablonları aralıklı. Bu makalenin sonraki bölümlerinde ranged bölümleme düzenini temel odaklanır.

### <a name="ranged-partitioning-scheme"></a>Bölümleme düzeni aralıklı
Bu, (düşük anahtarı ve yüksek anahtar tarafından tanımlanır) bir tamsayı aralığı ve bölümler (n) sayısını belirtmek için kullanılır. N bölümler, her bir çakışmayan alt aralığı genel bölüm anahtarı aralığının için sorumlu oluşturur. Örneğin, ranged bölümleme düzeni düşük anahtar 0 ile 99 yüksek anahtarı ve 4 sayısını dört bölüm, aşağıda gösterildiği gibi oluşturursunuz.

![Bölümleme aralığı](./media/service-fabric-concepts-partitioning/range-partitioning.png)

Veri kümesi içinde benzersiz bir anahtar göre bir karma oluşturma ortak bir yaklaşımdır. Bazı ortak anahtarları örnekleri bir araç kimlik numarası (Toplamıdır), bir çalışan kimliği veya benzersiz bir dize olabilir. Bu benzersiz anahtarı'nı kullanarak, bir karma kod mod, anahtar olarak kullanılacak anahtar aralığı karşılaşırsınız. İzin verilen anahtar aralığının üst ve alt sınırları belirtebilirsiniz.

### <a name="select-a-hash-algorithm"></a>Bir karma algoritması seçin
Karma önemli bir bölümü, karma algoritma seçme. Hedef birbirine yakın (yere göre hassas karma)--benzer anahtarları grubunda olup bir husustur veya etkinlik kapsamlı tüm bölümler (dağıtım karma) dağıtılması, daha yaygın olduğu.

İyi dağıtım karma algoritması özelliklerini şunlardır: işlem kolaydır, birkaç çakışmaları sahiptir ve anahtarları eşit dağıtır. Verimli karma algoritması iyi bir örnektir [FNV-1](https://en.wikipedia.org/wiki/Fowler%E2%80%93Noll%E2%80%93Vo_hash_function) karma algoritması.

Genel karma kodu algoritması seçenekleri için iyi bir kaynaktır [karma işlevlerini Wikipedia sayfasında](http://en.wikipedia.org/wiki/Hash_function).

## <a name="build-a-stateful-service-with-multiple-partitions"></a>Durum bilgisi olan bir hizmet birden çok bölüm ile derleme
İlk güvenilir durum bilgisi olan hizmet birden çok bölüm oluşturalım. Bu örnekte, aynı bölüm aynı harfiyle başlayan tüm soyadlarını saklamak istediğiniz çok basit bir uygulama oluşturacaksınız.

Kod yazmadan önce bölümler ve bölüm anahtarlarını hakkında düşünmek gerekir. 26 bölümleri (bir her harf alfabedeki için), düşük ve yüksek anahtarlar hakkında gerekenler?
Tam anlamıyla harf başına bir bölüm olmasını istiyoruz gibi kendi anahtarını her harfi olduğu gibi biz 0 düşük anahtar ve 25 yüksek anahtar kullanabilirsiniz.

> [!NOTE]
> Gerçekte dağıtım eşit olacak şekilde basitleştirilmiş bir senaryo budur. Harfler "S" veya "M" ile başlayan son adları "X" ile başlayan olanları daha yaygın veya "Y".
> 
> 

1. Açık **Visual Studio** > **dosya** > **yeni** > **proje**.
2. İçinde **yeni proje** iletişim kutusunda, Service Fabric uygulaması seçin.
3. Proje "AlphabetPartitions" çağırın.
4. İçinde **bir hizmet oluşturma** iletişim kutusunda, seçin **durum bilgisi olan** hizmet ve aşağıdaki görüntüde gösterildiği gibi "Alphabet.Processing" çağırın.
       ![Visual Studio'da yeni hizmet iletişim kutusu][1]

  <!--  ![Stateful service screenshot](./media/service-fabric-concepts-partitioning/createstateful.png)-->

5. Bölüm sayısı ayarlayın. AlphabetPartitions proje ApplicationPackageRoot klasöründe bulunan Applicationmanifest.xml dosyasını açın ve parametre Processing_PartitionCount 26 için aşağıda gösterildiği gibi güncelleştirin.
   
    ```xml
    <Parameter Name="Processing_PartitionCount" DefaultValue="26" />
    ```
   
    Ayrıca aşağıda gösterildiği gibi ApplicationManifest.xml StatefulService öğesinde LowKey ve HighKey özelliklerini güncelleştirmeniz gerekir.
   
    ```xml
    <Service Name="Processing">
      <StatefulService ServiceTypeName="ProcessingType" TargetReplicaSetSize="[Processing_TargetReplicaSetSize]" MinReplicaSetSize="[Processing_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[Processing_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    ```
6. Hizmet tarafından erişilebilir olması bir uç nokta bağlantı noktası yukarı ServiceManifest.xml (PackageRoot klasöründe bulunur) uç noktası öğesinin ekleyerek aşağıda gösterildiği gibi Alphabet.Processing hizmeti açın:
   
    ```xml
    <Endpoint Name="ProcessingServiceEndpoint" Port="8089" Protocol="http" Type="Internal" />
    ```
   
    Hizmet 26 bölümlerle iç uç nokta dinlemek için yapılandırılmıştır.
7. Ardından, geçersiz kılmanız gerekir `CreateServiceReplicaListeners()` işleme sınıfının yöntemi.
   
   > [!NOTE]
   > Bu örnek için basit bir HttpCommunicationListener kullandığınızı varsayar. Güvenilir hizmet iletişimi hakkında daha fazla bilgi için bkz: [güvenilir hizmet iletişim modelini](service-fabric-reliable-services-communication.md).
   > 
   > 
8. Bir çoğaltma dinlediği URL için önerilen bir desen aşağıdaki biçimdir: `{scheme}://{nodeIp}:{port}/{partitionid}/{replicaid}/{guid}`.
    Bu nedenle doğru uç noktaları ve bu deseni dinlemek için iletişim dinleyicisini yapılandırmak istiyorsunuz.
   
    Bu adres için çoğaltma benzersiz olması gerekir böylece bu hizmetin birden fazla çoğaltma aynı bilgisayarda barındırılan. Bölüm kimliği + çoğaltma kimliği URL'de olan nedeni budur. URL öneki benzersiz olduğu sürece HttpListener birden çok adresi aynı bağlantı noktasında dinler.
   
    Ek GUID burada ikincil çoğaltmaların salt okunur istekleri için de dinlemek Gelişmiş bir olay yok. Bu durumda, yeni bir benzersiz adresi birincil ikincil geçiş sırasında yeniden adresini çözümlemek için istemcileri zorlamak için kullanılmadığından emin olmak istersiniz. '+' böylece kullanılabilir tüm konakları üzerinde (IP, FQDM, localhost, vb.) çoğaltma dinler burada adresi olarak kullanılır Aşağıdaki kod örneği gösterir.
   
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
   
    Ayrıca, yayımlanan URL dinleme URL önekten biraz farklıdır dikkate değerdir.
    Dinleme URL HttpListener için verilir. Yayımlanan Service Fabric adlandırma hizmet bulma için kullanılan hizmetine yayımlanan URL'dir. İstemciler, bu bulma hizmeti aracılığıyla bu adres için sorar. İstemcilerinin aldığından adresi gerçek IP veya FQDN düğümün bağlanmak için olmalıdır. Değiştirmeniz gereken şekilde '+' düğümün IP veya yukarıda gösterildiği gibi FQDN ile.
9. Aşağıda gösterildiği gibi hizmet işleme mantığı eklemek için son adım olacaktır.
   
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
                addResult ? "sucessfully added" : "already exists");
        }
    }
    ```
   
    `ProcessInternalRequest`bölüm ve çağrıları çağırmak için kullanılan sorgu dizesi parametresi değerleri okur `AddUserAsync` lastname güvenilir sözlüğe eklemek için `dictionary`.
10. Durum bilgisi olmayan bir hizmeti belirli bir bölüm nasıl çağırabilirsiniz görmek için projeye ekleyelim.
    
    Bu hizmet, lastname bir sorgu dizesi parametresi olarak kabul eder, bölüm anahtarı belirler ve işleme Alphabet.Processing hizmetine gönderir bir basit bir web arabirimi olarak görev yapar.
11. İçinde **bir hizmet oluşturma** iletişim kutusunda, seçin **Stateless** hizmet ve aşağıda gösterildiği gibi "Alphabet.Web" çağırın.
    
    ![Durum bilgisiz hizmet ekran görüntüsü](./media/service-fabric-concepts-partitioning/createnewstateless.png).
12. ServiceManifest.xml aşağıda gösterildiği gibi bir bağlantı noktası açmak amacıyla Alphabet.WebApi hizmet uç noktası bilgileri güncelleştirin.
    
    ```xml
    <Endpoint Name="WebApiServiceEndpoint" Protocol="http" Port="8081"/>
    ```
13. Web sınıfında ServiceInstanceListeners koleksiyonu döndürülecek gerekir. Yeniden basit HttpCommunicationListener uygulamak seçebilirsiniz.
    
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
14. Şimdi işleme mantığı uygulamanız gerekir. HttpCommunicationListener çağrıları `ProcessInputRequest` bir isteği ne zaman devreye girer. Bu nedenle devam edelim ve aşağıdaki kodu ekleyin.
    
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
    
    Şimdi arkasını adım adım yol gösterir. Sorgu dizesi parametresi ilk harfini kodu okur `lastname` bir char içine. Ardından, on altılık değeri çıkararak bu harfi için bölüm anahtarı belirler `A` son adları ilk harfi onaltılık değeri.
    
    ```CSharp
    string lastname = context.Request.QueryString["lastname"];
    char firstLetterOfLastName = lastname.First();
    ServicePartitionKey partitionKey = new ServicePartitionKey(Char.ToUpper(firstLetterOfLastName) - 'A');
    ```
    
    Bu örnekte, 26 bölümler bölüm başına tek bölüm anahtarına sahip kullanıyoruz unutmayın.
    Ardından, biz hizmeti bölüm elde `partition` kullanarak bu anahtar için `ResolveAsync` yöntemi `servicePartitionResolver` nesnesi. `servicePartitionResolver`olarak tanımlanır
    
    ```CSharp
    private readonly ServicePartitionResolver servicePartitionResolver = ServicePartitionResolver.GetDefault();
    ```
    
    `ResolveAsync` Hizmet URI'si, bölüm anahtarı ve bir iptal belirteci parametre olarak yöntemi alır. İşleme hizmeti için URI hizmetidir `fabric:/AlphabetPartitions/Processing`. Ardından, bölümün bitiş noktası alın.
    
    ```CSharp
    ResolvedServiceEndpoint ep = partition.GetEndpoint()
    ```
    
    Son olarak, şu uç nokta URL'si artı querystring yapı ve işleme hizmeti çağırın.
    
    ```CSharp
    JObject addresses = JObject.Parse(ep.Address);
    string primaryReplicaAddress = (string)addresses["Endpoints"].First();
    
    UriBuilder primaryReplicaUriBuilder = new UriBuilder(primaryReplicaAddress);
    primaryReplicaUriBuilder.Query = "lastname=" + lastname;
    
    string result = await this.httpClient.GetStringAsync(primaryReplicaUriBuilder.Uri);
    ```
    
    İşlem tamamlandığında, biz çıkış geri yazma.
15. Son adım hizmetin test etmektir. Visual Studio kullanan uygulama parametreleri, yerel ve bulut dağıtımı. Yerel olarak 26 bölümleri hizmetiyle sınamak için güncelleştirmeniz gerekir `Local.xml` dosya AlphabetPartitions proje ApplicationParameters klasöründe aşağıda gösterildiği gibi:
    
    ```xml
    <Parameters>
      <Parameter Name="Processing_PartitionCount" Value="26" />
      <Parameter Name="WebApi_InstanceCount" Value="1" />
    </Parameters>
    ```
16. Dağıtımını tamamladıktan sonra hizmet ve tüm bölümleri Service Fabric Explorer'da kontrol edebilirsiniz.
    
    ![Service Fabric Explorer ekran görüntüsü](./media/service-fabric-concepts-partitioning/sfxpartitions.png)
17. Bir tarayıcıda girerek bölümleme mantığı sınayabilirsiniz `http://localhost:8081/?lastname=somename`. Aynı harfle başlayan her Soyadı aynı bölümde depolandığını görürsünüz.
    
    ![Tarayıcı ekran görüntüsü](./media/service-fabric-concepts-partitioning/samplerunning.png)

Tüm kaynak kodu örnek [GitHub](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started/tree/classic/Services/AlphabetPartitions).

## <a name="next-steps"></a>Sonraki adımlar
Service Fabric kavramları hakkında daha fazla bilgi için aşağıdakilere bakın:

* [Service Fabric hizmetlerin kullanılabilirliğini](service-fabric-availability-services.md)
* [Service Fabric Hizmetleri ölçeklenebilirliği](service-fabric-concepts-scalability.md)
* [Kapasite planlama için Service Fabric uygulamaları](service-fabric-capacity-planning.md)

[wikipartition]: https://en.wikipedia.org/wiki/Partition_(database)

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png