---
title: Microsoft Azure Depolama'da Eşzamanlılığı Yönetme
description: Blob, kuyruk, tablo ve Dosya Hizmetleri için eşzamanlılık yönetme
services: storage
documentationcenter: ''
author: jasontang501
manager: tadb
editor: tysonn
ms.assetid: cc6429c4-23ee-46e3-b22d-50dd68bd4680
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: jasontang501
ms.openlocfilehash: 937cca66a0af0674b868e6a87681adbea330e91c
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2018
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Microsoft Azure Depolama'da Eşzamanlılığı Yönetme
## <a name="overview"></a>Genel Bakış
Modern Internet tabanlı uygulamalar genellikle görüntüleme ve verileri aynı anda güncelleştirme birden çok kullanıcı sahiptir. Bu, uygulama geliştiricilerin kendi son kullanıcıları için özellikle birden çok kullanıcı aynı verileri nerede güncelleştirebilirsiniz senaryoları için öngörülebilir bir deneyim sağlamaya nasıl dikkatlice düşünün gerektirir. Geliştiriciler genellikle düşünün üç ana veri eşzamanlılık stratejisi vardır:  

1. İyimser eşzamanlılık – bir güncelleştirme, güncelleştirme bir parçası olarak veri uygulama itibaren değişip değişmediğini doğrulayın gerçekleştiren bir uygulama bu verileri son okuyun. Bir wiki sayfa görüntüleme iki kullanıcı aynı sayfaya bir güncelleştirme yaparsanız Örneğin, ardından wiki platform ikinci güncelleştirmeyi ilk güncelleştirmesi – üzerine değildir ve her iki kullanıcı kendi güncelleştirme başarılı olup olmadığını anlamak emin olmalısınız. Bu strateji, web uygulamaları en sık kullanılır.
2. Eşzamanlılık – bir güncelleştirme gerçekleştirmek için bir uygulama arayan bir kilit diğer kullanıcıların kilidi serbest kadar verileri güncelleştirme engelleyen bir nesne üzerinde olur. Örneğin, yalnızca ana güncelleştirmeleri nerede gerçekleştirecek ana/bağımlı veri çoğaltma senaryoda asıl genellikle özel bir kilit kimsenin güncelleştirebilirsiniz emin olmak için verileri zamanında uzun bir süre için tutar.
3. Son yazıcı WINS – başka bir uygulama verileri bu yana uygulama ilk güncelleştirdi, doğrulamadan devam etmek tüm güncelleştirme işlemlerine izin veren bir yaklaşım verilerini okur. Bu strateji (veya bir resmi stratejisi eksikliği) genellikle veri birden çok kullanıcı aynı verilere erişecek hiçbir olasılığı olan yolu burada bölümlenmiş kullanılır. Bu da kısa süreli veri akışlarını burada işlenen yararlı olabilir.  

Bu makalede bu eşzamanlılık stratejileri üçü için birinci sınıf destek sağlayan Azure Storage platform geliştirme nasıl basitleştirir genel bir bakış sağlar.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure Storage – bulut geliştirme basitleştirir
Depolama hizmetinin veri ekleme kaydeder veya güncelleştirme işlemi tüm daha fazla için verileri en son güncelleştirmeyi görürler erişir garanti bir güçlü tutarlılık modelini kapsayacak şekilde tasarlandığından iyimser ve kötümser eşzamanlılık için tam destek sağlamak yeteneğini farklı olmasına rağmen Azure depolama hizmeti tüm üç stratejileri destekler. Bir kullanıcı tarafından ve güncelleştirilmiş verileri, böylece tutarsızlıklar son kullanıcıların etkilemesini önlemek için istemci uygulamaları geliştirme karmaşıklaştırarak diğer kullanıcılar tarafından görülebilir yazma gerçekleştirildiğinde nihai tutarlılık modelini kullanan depolama platformları arasında bir gecikme vardır.  

Uygun eşzamanlılık stratejisi seçmenin yanı sıra geliştiriciler ayrıca bir depolama platform değişiklikler – özellikle de aynı nesneye işlemleri arasında nasıl yalıtır bilmeniz gerekir. Azure depolama hizmeti anlık görüntü yalıtımı yazma işlemlerini tek bir bölüm içinde eşzamanlı olarak gerçekleşecek şekilde okuma işlemleri izin vermek için kullanır. Diğer yalıtım düzeyi farklı olarak, tüm okuma verilerin tutarlı bir anlık görüntü gördüğünüz bile güncelleştirmeleri yaşanan – temelde son kabul edilen değerlerin bir güncelleştirme sırasında döndürerek işlem işleniyor sırada anlık görüntü yalıtımı garanti eder.  

## <a name="managing-concurrency-in-blob-storage"></a>Blob depolama alanına eşzamanlılık yönetme
BLOB'ları ve blob hizmetinde kapsayıcıları erişimi yönetmek için ya da iyimser veya kötümser eşzamanlılık modelleriyle kullanmayı tercih edebilirsiniz. Bir strateji son yazma WINS açıkça belirtmezseniz varsayılandır.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>BLOB'ları ve kapsayıcıları için iyimser eşzamanlılık
Depolama Birimi hizmeti depolanan her nesne için bir tanımlayıcı atar. Bu tanımlayıcı, bir nesne üzerinde her bir güncelleştirme işlemi gerçekleştirildiğinde güncelleştirilir. Tanımlayıcı, istemci HTTP Protokolü içinde tanımlanan ETag (varlık etiketi) üstbilgisi kullanarak bir HTTP GET yanıtında bir parçası olarak döndürülür. Böyle bir nesnenin bir güncelleştirmesi gerçekleştiren kullanıcı koşullu üstbilgi birlikte özgün ETag gönderebilir belirli bir koşulun yerine, bir güncelleştirme emin olmak için yalnızca meydana gelir – bu durumda, depolama birimi hizmeti t gerektiren bir "If-Match" üstbilgisi durumdur o güncelleştirme isteğinde belirtilen ETag değeri depolama hizmetinde depolanan aynı olduğundan emin olun.  

Bu işlem anahat aşağıdaki gibidir:  

1. Bir blob Depolama hizmetinden alın, yanıt nesnesinin geçerli sürümü, depolama hizmetindeki tanımlayan bir HTTP ETag üstbilgi değeri içeriyor.
2. Blob'ı güncelleştirdiğinizde, adım 1'de alınan ETag değeri içeren **IF-Match** hizmete gönderme isteği koşullu üstbilgi.
3. Hizmet isteği ETag değeri blob geçerli ETag değeri ile karşılaştırır.
4. BLOB geçerli ETag değeri ETag sürümünden farklı bir sürüm ise **IF-Match** hizmet isteği koşullu üstbilgisinde istemciye 412 hata döndürür. Bu, istemciye, istemcinin alınan bu yana başka bir işlem blob güncelleştirdi gösterir.
5. BLOB geçerli ETag değeri ETag ile aynı sürüme ise **IF-Match** hizmet isteği koşullu üstbilgisinde istenen işlem gerçekleştirir ve yeni bir sürümü oluşturdu göstermek için blob geçerli ETag değeri güncelleştirir.  

(İstemci depolama kitaplığı 4.2.0 kullanarak) aşağıdaki C# kod parçacığını nasıl oluşturulacağını basit bir örneği gösterir bir **IF-Match AccessCondition** , daha önce kopya alınan eklenmiş veya silinmiş bir blob özelliklerinden erişilen ETag değere göre. Daha sonra kullanır **AccessCondition** nesne blob güncelleştirdiğinde: **AccessCondition** nesnesi ekler **IF-Match** isteği üstbilgisi. Başka bir işlem blob güncelleştirdi, blob hizmeti bir HTTP 412 (önkoşul başarısız oldu) durum iletisi döndürür. Tam örnek buradan indirebilirsiniz: [Azure Storage kullanarak eşzamanlılık yönetme](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

```csharp
// Retrieve the ETag from the newly created blob
// Etag is already populated as UploadText should cause a PUT Blob call
// to storage blob service which returns the etag in response.
string orignalETag = blockBlob.Properties.ETag;

// This code simulates an update by a third party.
string helloText = "Blob updated by a third party.";

// No etag, provided so orignal blob is overwritten (thus generating a new etag)
blockBlob.UploadText(helloText);
Console.WriteLine("Blob updated. Updated ETag = {0}",
blockBlob.Properties.ETag);

// Now try to update the blob using the orignal ETag provided when the blob was created
try
{
    Console.WriteLine("Trying to update blob using orignal etag to generate if-match access condition");
    blockBlob.UploadText(helloText,accessCondition:
    AccessCondition.GenerateIfMatchCondition(orignalETag));
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
    {
        Console.WriteLine("Precondition failure as expected. Blob's orignal etag no longer matches");
        // TODO: client can decide on how it wants to handle the 3rd party updated content.
    }
    else
        throw;
}  
```

Depolama hizmeti için de destek ek koşullu üstbilgileri gibi içerir **If-Modified-Since**, **IF-Unmodified-Since** ve **If-None-Match** birleşimleri bunların yanı sıra. Daha fazla bilgi için bkz: [belirtme koşullu üstbilgileri Blob hizmeti işlemleri için](http://msdn.microsoft.com/library/azure/dd179371.aspx) konusuna bakın.  

Aşağıdaki tabloda koşullu üstbilgileri gibi kabul kapsayıcı işlemleri özetlenmektedir **IF-Match** istek ve yanıtta bir ETag değeri döndürür.  

| İşlem | Kapsayıcı ETag değeri döndürür | Koşullu üstbilgileri kabul eder |
|:--- |:--- |:--- |
| Kapsayıcı oluşturma |Evet |Hayır |
| Kapsayıcı özellikleri Al |Evet |Hayır |
| Kapsayıcı meta verileri alma |Evet |Hayır |
| Kapsayıcı meta verileri ayarlama |Evet |Evet |
| Kapsayıcı ACL Al |Evet |Hayır |
| Kapsayıcı ACL ayarlayın |Evet |Evet (*) |
| Kapsayıcısını silmek |Hayır |Evet |
| Kira kapsayıcısı |Evet |Evet |
| Liste BLOB'ları |Hayır |Hayır |

(*) SetContainerACL tarafından tanımlanan izinleri önbelleğe alınır ve bu izinleri güncelleştirmeleri 30 yaymak için hangi dönemde güncelleştirmeleri tutarlı olması garanti edilmez saniye sürebilir.  

Aşağıdaki tabloda koşullu üstbilgileri gibi kabul blobu işlemleri özetlenmektedir **IF-Match** istek ve yanıtta bir ETag değeri döndürür.

| İşlem | ETag değeri döndürür | Koşullu üstbilgileri kabul eder |
|:--- |:--- |:--- |
| Put Blob |Evet |Evet |
| BLOB alma |Evet |Evet |
| BLOB özelliklerini alma |Evet |Evet |
| Blob özelliklerini ayarlama |Evet |Evet |
| Get Blob Metadata |Evet |Evet |
| Set Blob Metadata |Evet |Evet |
| Kira blob'u (*) |Evet |Evet |
| Snapshot Blob |Evet |Evet |
| BLOB kopyalama |Evet |Evet (kaynak ve hedef blob için) |
| Kopya Blob Durdur |Hayır |Hayır |
| BLOB Sil |Hayır |Evet |
| Blok yerleştirme |Hayır |Hayır |
| Engelleme listesi yerleştirme |Evet |Evet |
| Engelleme listesi al |Evet |Hayır |
| Sayfa yerleştirin |Evet |Evet |
| Sayfa aralıklarını alma |Evet |Evet |

(*) Kira blob'u blob üzerinde ETag değiştirmez.  

### <a name="pessimistic-concurrency-for-blobs"></a>Eşzamanlılık BLOB'lar için
Özel kullanım için bir blob kilitlemek için elde edebilirsiniz bir [kira](http://msdn.microsoft.com/library/azure/ee691972.aspx) üzerindeki. Bir kira aldığınızda, kira ne kadar süreyle ihtiyacınız belirtin: 15 ila 60 saniye veya özel bir kilit tutar sonsuz arasındaki bu için olabilir. Genişletmek için sınırlı bir kira yenileme ve ile işiniz bittiğinde, tüm kira serbest bırakabilirsiniz. Bunlar sona erdiğinde blob hizmeti otomatik olarak sınırlı kira serbest bırakır.  

Kira farklı eşitleme stratejileri desteklenmesi özel yazma dahil olmak üzere etkinleştir / okuma, özel yazma paylaşılan / özel okuma ve yazma paylaşılan / özel okuyun. Bir kira var. burada Depolama Birimi Hizmeti (put, ayarlayın ve silme işlemleri) yazma gerektirir tüm istemci uygulamaları bir kira kimliği ve yalnızca bir istemci aynı anda kullandığınızdan emin olmak Geliştirici exclusivity okuma işlemleri için sağlama ancak geçerli bir kira kimliğe sahip özel zorlar. Bir kira kimliği sonucu paylaşılan okuma içermez okuma işlemleri.  

Aşağıdaki C# kod parçacığını bir blob 30 saniye için özel bir kira alınırken, blob içeriği güncelleştirme ve kira serbest bırakma bir örneği gösterir. Blob hizmeti zaten varsa geçerli bir kira blob üzerindeki yeni bir kira almaya çalıştığında, bir "HTTP (409) çakışma" durum sonucunu döndürür. Aşağıdaki kod parçacığını kullanan bir **AccessCondition** blob depolama hizmetindeki güncelleştirmek için bir istek yaptığında, kiralama bilgileri yalıtan nesne.  Tam örnek buradan indirebilirsiniz: [Azure Storage kullanarak eşzamanlılık yönetme](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
// Acquire lease for 15 seconds
string lease = blockBlob.AcquireLease(TimeSpan.FromSeconds(15), null);
Console.WriteLine("Blob lease acquired. Lease = {0}", lease);

// Update blob using lease. This operation will succeed
const string helloText = "Blob updated";
var accessCondition = AccessCondition.GenerateLeaseCondition(lease);
blockBlob.UploadText(helloText, accessCondition: accessCondition);
Console.WriteLine("Blob updated using an exclusive lease");

//Simulate third party update to blob without lease
try
{
    // Below operation will fail as no valid lease provided
    Console.WriteLine("Trying to update blob without valid lease");
    blockBlob.UploadText("Update without lease, will fail");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
        Console.WriteLine("Precondition failure as expected. Blob's lease does not match");
    else
        throw;
}  
```

Bir yazma işlemi kiralık blob kira kimliği geçmeden çalışırsanız, istek 412 bir hata ile başarısız olur. Çağırmadan önce kira süresi dolarsa unutmayın **UploadText** yöntem, ancak hala kira kimliği geçişi, istek ile de başarısız bir **412** hata. Kira bitiş zamanları ve kira kimlikleri yönetme hakkında daha fazla bilgi için bkz: [kira blob'u](http://msdn.microsoft.com/library/azure/ee691972.aspx) REST belgeleri.  

Aşağıdaki blobu işlemleri kiraları eşzamanlılık yönetmek için kullanabilirsiniz:  

* Put Blob
* BLOB alma
* BLOB özelliklerini alma
* Blob özelliklerini ayarlama
* Get Blob Metadata
* Set Blob Metadata
* BLOB Sil
* Blok yerleştirme
* Engelleme listesi yerleştirme
* Engelleme listesi al
* Sayfa yerleştirin
* Sayfa aralıklarını alma
* Anlık görüntü Blob - kira kimliği bir kira varsa, isteğe bağlı
* BLOB - bir kira hedef blob üzerindeki var olup olmadığını kimliği gerekli kira kopyalama
* Sonsuz bir kira hedef blob üzerindeki var olup olmadığını kira kimliği durdurma kopyalama Blob - gerekli
* Kira blob'u  

### <a name="pessimistic-concurrency-for-containers"></a>Eşzamanlılık kapsayıcıları için
Kiraları kapsayıcılarında BLOB olarak desteklenmesi aynı eşitleme stratejileri etkinleştirme (özel yazma / okuma, özel yazma paylaşılan / özel okuma ve yazma paylaşılan / özel okuma) ancak BLOB Depolama hizmetinin yalnızca silme işlemlerini exclusivity zorlar. Etkin bir kira kapsayıcıyla silmek için bir istemci silme isteği etkin kira Kimliğiyle eklemeniz gerekir. Kira kimliği dahil olmak üzere diğer tüm kapsayıcı işlemleri kiralık bir kapsayıcıda başarılı durumda olduklarını işlemleri paylaşılan. Güncelleştirme (put veya kümesi) veya okuma işlemleri exclusivity gerekliyse, ardından geliştiriciler aynı anda bir istemci geçerli bir kira kimliğe sahip yalnızca bir kira kimliği ve, tüm istemcilerin kullanın emin olmalısınız  

Aşağıdaki kapsayıcı işlemleri kiraları eşzamanlılık yönetmek için kullanabilirsiniz:  

* Kapsayıcısını silmek
* Kapsayıcı özellikleri Al
* Kapsayıcı meta verileri alma
* Kapsayıcı meta verileri ayarlama
* Kapsayıcı ACL Al
* Kapsayıcı ACL ayarlayın
* Kira kapsayıcısı  

Daha fazla bilgi için bkz.  

* [Blob hizmeti işlemleri için koşullu üstbilgileri belirtme](http://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Kira kapsayıcısı](http://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Kira blob'u ](http://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Tablo hizmetinde eşzamanlılık yönetme
Tablo hizmeti iyimser eşzamanlılık burada açıkça iyimser eşzamanlılık denetimleri gerçekleştirmek için seçtiğiniz gerekir blob hizmeti aksine varlıklarla çalışırken varsayılan davranış olarak denetler kullanır. Diğer tablo ve blob hizmetleri arasında blob hizmetiyle kapsayıcılar ve bloblar eşzamanlılığı yönetebilir ancak varlıklar eşzamanlılık davranışını yalnızca yönetebilir farktır.  

İyimser eşzamanlılık kullanın ve tablo Depolama hizmetinden alınan bu yana başka bir işlem varlıkta değişiklik olmadığını denetlemek için tablo hizmeti bir varlık döndürdüğünde aldığınız ETag değeri kullanabilirsiniz. Bu işlem anahat aşağıdaki gibidir:  

1. Bir varlık tablo Depolama hizmetinden alın, yanıt depolama hizmetindeki varlık ile ilişkili geçerli bir tanımlayıcı tanımlayan bir ETag değeri içeriyor.
2. Varlık güncelleştirdiğinizde, zorunlu olarak 1. adımda alınan ETag değeri içeren **IF-Match** hizmete gönderme isteği üstbilgisi.
3. Hizmet isteği ETag değeri varlık geçerli ETag değeri ile karşılaştırır.
4. Varlık geçerli ETag değeri zorunlu ETag farklı ise **IF-Match** hizmet istek üstbilgisinde istemciye 412 hata döndürür. Bu, istemciye, istemcinin alınan bu yana başka bir işlem varlık güncelleştirdi gösterir.
5. Varlık geçerli ETag değeri ETag zorunlu ile aynı olduğunda **IF-Match** istek üstbilgisinde veya **IF-Match** üst bilgiyi içeren joker karakter (*), hizmet istenen işlem gerçekleştirir ve güncelleştirilmiş göstermek için varlık geçerli ETag değeri güncelleştirir.  

Blob hizmeti, dahil etmek istemci tablo hizmeti gerektirdiğini Not bir **IF-Match** güncelleştirme isteği üstbilgisi. Ancak, bir koşulsuz zorlama mümkündür (son yazıcı WINS stratejisi) güncelleştirin ve istemci ayarlar eşzamanlılık denetimleri atlanmasına **IF-Match** joker karakter (*) istekteki üstbilgi.  

Aşağıdaki C# kod parçacığını ya da daha önce oluşturulmuş veya güncelleştirilmiş kendi e-posta adresi alınan bir müşteri varlığı gösterir. İlk ekleme işlemi depoları ETag değeri veya müşteri nesnesindeki ve değiştirme işlemi yürüttüğünde örnek aynı nesne örneğini kullandığından, otomatik olarak ETag değeri eşzamanlılık ihlali denetlemek hizmet etkinleştirme geri tablo hizmetine gönderir. Başka bir işlem tablo depolama varlıkta güncelleştirdi, hizmeti bir HTTP 412 (önkoşul başarısız oldu) durum iletisi döndürür.  Tam örnek buradan indirebilirsiniz: [Azure Storage kullanarak eşzamanlılık yönetme](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

```csharp
try
{
    customer.Email = "updatedEmail@contoso.org";
    TableOperation replaceCustomer = TableOperation.Replace(customer);
    customerTable.Execute(replaceCustomer);
    Console.WriteLine("Replace operation succeeded.");
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == 412)
        Console.WriteLine("Optimistic concurrency violation – entity has changed since it was retrieved.");
    else
        throw;
}  
```

Açıkça eşzamanlılık denetimi devre dışı bırakmak için ayarlamalısınız **ETag** özelliği **çalışan** nesnesine "*" değiştirme işlemini yürütmeden önce.  

```csharp
customer.ETag = "*";  
```

Aşağıdaki tabloda, tablo varlık işlemleri ETag değerleri kullanma özetlenmektedir:

| İşlem | ETag değeri döndürür | IF-Match istek üstbilgisi gerektirir |
|:--- |:--- |:--- |
| Sorgu varlıklar |Evet |Hayır |
| Varlık ekleme |Evet |Hayır |
| Varlık güncelleştir |Evet |Evet |
| Varlık birleştirme |Evet |Evet |
| Varlığı silme |Hayır |Evet |
| Ekleme veya değiştirme varlık |Evet |Hayır |
| INSERT veya birleştirme varlık |Evet |Hayır |

Unutmayın **ekleme veya değiştirme varlık** ve **ekleme veya birleştirme varlık** işlemleri yapıp *değil* tablo hizmetine bir ETag değeri göndermeyin her eşzamanlılık denetim gerçekleştirilemiyor.  

Genel tabloları kullanarak geliştiricilerin ölçeklenebilir uygulamalar geliştirirken üzerinde iyimser eşzamanlılık yararlanmalıdır. Kötümser kilitleme gerekirse tabloları erişen her tablo için atanmış bir blob atayın ve tabloda işletim önce blob üzerindeki bir kira yararlanmaya çalışan olduğunda bir yaklaşım geliştiriciler alabilir. Bu yaklaşım, tüm veri erişim yolları tabloda işletim önce kira elde emin olmak için uygulama gerektirir. Ayrıca, ölçeklenebilirlik için dikkat gerektiren 15 saniye minimum kira süresi olan unutmamalısınız.  

Daha fazla bilgi için bkz.  

* [Varlıklar üzerinde işlemler](http://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Kuyruk hizmetinde eşzamanlılık yönetme
Hangi eşzamanlılık Çağrısı hizmetindeki bir sorun olduğu bir senaryo, burada birden çok istemciye iletilerin bir kuyruktan alıyor ' dir. Kuyruktan bir ileti alındığında, yanıt iletisi ve ileti silmek için gerekli bir pop giriş değeri içeriyor. İletiyi kuyruktan otomatik olarak silinmez, ancak bunu alındıktan sonra visibilitytimeout parametresi tarafından belirtilen zaman aralığı için diğer istemcilere görünür değil. İletiyi alır istemci ileti işlendikten sonra TimeNextVisible tarafından belirtilen süreden önce hesaplanır yanıt öğesinin visibilitytimeout parametre değeri temel alınarak silme bekleniyor. Visibilitytimeout değerini TimeNextVisible değerini belirlemek için hangi ileti alınır zaman eklenir.  

Sıra Hizmeti iyimser veya kötümser eşzamanlılık desteği yok ve bir ıdempotent şekilde işlenen iletileri bir sıradan alınan iletileri işleme neden istemcileri bu için emin olmalısınız. Bir son yazıcı WINS stratejisi SetQueueServiceProperties, SetQueueMetaData, SetQueueACL ve UpdateMessage gibi güncelleştirme işlemleri için kullanılır.  

Daha fazla bilgi için bkz.  

* [Kuyruk hizmeti REST API'si](http://msdn.microsoft.com/library/azure/dd179363.aspx)
* [İletileri alma](http://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Dosya hizmeti eşzamanlılık yönetme
Dosya hizmeti, iki farklı protokol uç noktalarını – SMB ve REST kullanarak erişilebilir. REST hizmeti İyimser kilitleme veya kötümser kilitleme desteği yoktur ve tüm güncelleştirmeleri bir son yazıcı WINS stratejisi izler. Dosya paylaşımları bağlama SMB istemcileri paylaşılan dosyaları – kötümser kilitleme gerçekleştirme yeteneğini de dahil olmak üzere erişimi yönetmek için dosya sistemi kilitleme mekanizmaları yararlanabilirsiniz. SMB istemcisi bir dosyayı açtığında dosya erişim ve Paylaşım belirtir modu. Dosya erişimi seçeneği "Yazma" veya "Okuma/yazma" bir dosya paylaşımı modu birlikte "None" dosya kapatılana kadar bir SMB istemci tarafından kilitleniyor dosyasında neden olur. SMB istemci kilitli dosyanın bulunduğu bir dosya REST işleminin denenmesi durumunda REST hizmeti hata kodu SharingViolation durum koduyla 409 (Çakışma) döndürür.  

SMB istemcisi silme için bir dosya açıldığında, bu dosyada açık tanıtıcıların bekleyen silme diğer tüm SMB istemci kadar kapalı olarak dosyayı işaretler. Bir dosya bekleyen silme olarak işaretlenmiş, ancak bu dosya üzerinde herhangi bir REST işlemi hata kodu SMBDeletePending durum koduyla 409 (Çakışma) döndürür. Dosyayı kapatmadan önce bekleyen silme bayrağı kaldırmak SMB istemcisi için olası olduğundan durum kodu 404 (bulunamadı) döndürülmez. Diğer bir deyişle, durum kodu 404 (bulunamadı), yalnızca dosya kaldırıldığında beklenir. Delete durumu bekleyen bir SMB dosya olarak kullanılırken, liste dosyaları sonuçlarında katılmaz olduğunu unutmayın. Ayrıca, REST Dosya Sil ve REST dizin silme işlemlerini otomatik olarak uygulanır ve bekleyen silme durumunda yol açmamasını unutmayın.  

Daha fazla bilgi için bkz.  

* [Dosya Yönetimi kilitler](http://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Özet ve sonraki adımlar
Microsoft Azure depolama hizmeti, geliştiricilerin tehlikeye veya için verilen alın gelmiş eşzamanlılık ve veri tutarlılığı gibi temel tasarım varsayımları zorlanıyor zorlamadan en karmaşık çevrimiçi uygulamalar ihtiyaçlarını karşılamak üzere tasarlanmıştır.  

Bu Web Günlüğü'ndeki başvurulan tam örnek uygulama için:  

* [Azure Storage - örnek uygulaması kullanarak eşzamanlılık yönetme](http://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Azure Storage bakın hakkında daha fazla bilgi için:  

* [Microsoft Azure depolama giriş sayfası](https://azure.microsoft.com/services/storage/)
* [Azure Depolama’ya giriş](storage-introduction.md)
* Başlarken depolama [Blob](../blobs/storage-dotnet-how-to-use-blobs.md), [tablo](../../cosmos-db/table-storage-how-to-use-dotnet.md), [sıraları](../storage-dotnet-how-to-use-queues.md), ve [dosyaları](../storage-dotnet-how-to-use-files.md)
* Depolama mimarisi – [Azure Storage: güçlü tutarlılık sahip yüksek oranda kullanılabilir bulut depolama hizmeti](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

