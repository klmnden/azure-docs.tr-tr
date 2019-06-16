---
title: Microsoft Azure Depolama'da Eşzamanlılığı Yönetme
description: Blob, kuyruk, tablo ve Dosya Hizmetleri için eşzamanlılığı yönetme
services: storage
author: jasontang501
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 05/11/2017
ms.author: jasontang501
ms.subservice: common
ms.openlocfilehash: 9e786aed031d528b8ae574444b71753ac538cf47
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64728308"
---
# <a name="managing-concurrency-in-microsoft-azure-storage"></a>Microsoft Azure Depolama'da Eşzamanlılığı Yönetme
## <a name="overview"></a>Genel Bakış
Internet tabanlı modern uygulamalar genellikle görüntüleme ve verileri aynı anda güncelleştiren birden çok kullanıcı sahiptir. Bu, uygulama geliştiricilerin öngörülebilir bir deneyim, özellikle birden çok kullanıcının aynı verileri nerede güncelleştirebilirsiniz senaryoları için son kullanıcılara sağlama hakkında dikkatli düşünme gerektirir. Geliştiriciler genellikle göz önünde bulundurun üç ana veri eşzamanlılık strateji vardır:  

1. İyimser eşzamanlılık – bir güncelleştirme, güncelleştirmenin bir parçası olarak verileri uygulama itibaren değişip değişmediğini doğrulayın gerçekleştiren bir uygulama bu verileri en son okuyun. Wiki sayfası görüntüleme iki kullanıcı aynı sayfaya bir güncelleştirme yaparsanız, ardından wiki platform ikinci güncelleştirme ilk güncelleştirme – üzerine yazmaz ve hem de kullanıcıların kendi güncelleştirme başarılı olup olmadığını anlamak emin olmanız gerekir. Bu strateji, web uygulamalarında sık kullanılır.
2. Kötümser eşzamanlılık – bir güncelleştirme gerçekleştirmek için bir uygulama arayan bir kilidi diğer kullanıcıların kilidi serbest bırakılıncaya kadar veri güncelleştirme engelleyen bir nesne üzerinde olacaktır. Örneğin, burada yalnızca ana güncelleştirmeler gerçekleştirecek bir ana/alt veri çoğaltma senaryosunda ana genellikle özel bir kilit zaman başka hiç kimse güncelleştirebilirsiniz emin olmak için verileri uzun bir süre için tutar.
3. Son yazıcı WINS – başka bir uygulama uygulama bu yana ilk kez güncelleştirilmiş veriler içeriyor, doğrulamadan devam etmek herhangi bir güncelleştirme işlemi izin veren bir yaklaşım, verileri okuyamadı. Bu strateji (veya bir biçimsel stratejisi eksikliği) genellikle burada veri birden çok kullanıcının aynı verilere erişecek hiçbir olasılığı olan bir şekilde bölümlendiğinde kullanılır. Bu da kısa süreli veri akışlarını burada işlenmekte olan yararlı olabilir.  

Bu makalede, Azure depolama platformu bu eşzamanlılık stratejiler üçü için birinci sınıf destek sağlayarak geliştirmeyi nasıl kolaylaştırdığını genel bir bakış sağlar.  

## <a name="azure-storage--simplifies-cloud-development"></a>Azure depolama – bulut geliştirmeyi basitleştirir
Olduğunda garanti eder, güçlü tutarlılık modelini kapsayacak şekilde tasarlandığından iyimser ve kötümser eşzamanlılık için tam destek sağlamak için güncelleyebileceği farklı olmasına rağmen Azure depolama hizmeti, tüm üç strateji destekler. Depolama hizmeti bir veri INSERT tamamlar veya güncelleştirme işlemi tüm ek için verileri en son güncelleştirmeyi görürsünüz erişir. Bir yazma ve güncelleştirilmiş veriler, bu nedenle gelen tutarsızlıkları önlemek için istemci uygulamaları geliştirmeye yönelik karmaşık diğer kullanıcılar tarafından görülebilir, bir kullanıcı tarafından gerçekleştirildiğinde son tutarlılık modelini kullanan depolama platformlar arasında bir gecikme vardır. Son kullanıcıları etkilemeden.  

Uygun eşzamanlılık stratejisi seçmenin yanı sıra geliştiricilere de depolama platformu değişiklikler – özellikle aynı nesneye işlemler arasında nasıl yalıtır bilmeniz gerekir. Azure depolama hizmeti, okuma işlemleri yazma işlemleri tek bir bölüm içinde aynı anda gerçekleşecek izin vermek için anlık görüntü yalıtımı kullanır. Diğer yalıtım düzeyleri farklı olarak, tüm okuma verilerin tutarlı bir anlık görüntüsünü görmesini olsa bile güncelleştirmeleri gerçekleşen – aslında bir güncelleştirme sırasında son kaydedilen değerleri döndürerek işlem işlenmekte olan anlık görüntü yalıtımı garanti eder.  

## <a name="managing-concurrency-in-blob-storage"></a>Blob Depolama'da eşzamanlılığı yönetme
Blob hizmetinde kapsayıcıları ve blobları erişimi yönetmek için her iki iyimser veya kötümser eşzamanlılık modelleriyle kullanmayı tercih. Bir strateji son yazma WINS açıkça belirtmezseniz varsayılandır.  

### <a name="optimistic-concurrency-for-blobs-and-containers"></a>Kapsayıcıları ve blobları için iyimser eşzamanlılık
Depolama hizmeti, depolanan her nesne için bir tanımlayıcı atar. Bu tanımlayıcı, bir nesne üzerinde bir güncelleştirme işlemi gerçekleştirildiğinde güncelleştirilir. Tanımlayıcı, istemciye bir HTTP GET yanıtı HTTP Protokolü içinde tanımlanan ETag (varlık etiketi) üst bilgisini kullanarak bir parçası olarak döndürülür. Böyle bir nesne üzerinde bir güncelleştirme gerçekleştirilirken bir kullanıcının özgün ETag koşullu üstbilgi birlikte gönderebilir belirli bir koşulun yerine bir güncelleştirme emin olmak için yalnızca meydana gelir – bu durumda, depolama hizmeti t gerektiren bir "If-Match" üst durumdur o güncelleştirme isteğinde belirtilen ETag değeri depolama hizmetinde depolanan aynı olduğundan emin olun.  

Bu işlem ana hat aşağıdaki gibidir:  

1. Bir blob Depolama hizmetinden alın, yanıt depolama hizmeti nesnesinin geçerli sürümü tanımlayan bir HTTP ETag üstbilgi değeri içeriyor.
2. Blob güncelleştirdiğinizde, adım 1'de alınan ETag değeri içeren **IF-Match** koşullu hizmetine gönderdiğiniz isteği üstbilgisi.
3. Hizmet isteği ETag değeri blob geçerli ETag değeri ile karşılaştırır.
4. Blob geçerli ETag değeri ETag sürümünden farklı bir sürüm ise **IF-Match** hizmet isteği koşullu üstbilgisinde istemciye 412 hata döndürür. Bu, istemci için başka bir işlem olduğundan, istemci alınan blob güncelleştirdi gösterir.
5. Blob geçerli ETag değeri ETag aynı sürüme ise **IF-Match** koşullu üstbilgisinde isteği, hizmet istenen işlemi gerçekleştirir ve bunu oluşturduğunu göstermek için blob geçerli ETag değeri güncelleştirir Yeni bir sürümü.  

(İstemci depolama kitaplığı 4.2.0 kullanılarak) aşağıdaki C# kod parçacığı oluşturmak nasıl basit bir örneği gösterir. bir **IF-Match AccessCondition** ya da önceden bir blobun özelliklerinden erişilen ETag değere göre Alınan ya da eklenir. Ardından kullanır **AccessCondition** nesne blob güncelleştirdiğinde: **AccessCondition** nesnesi ekler **IF-Match** isteği üstbilgisi. Başka bir işlem blob güncelleştirdi, blob hizmeti bir HTTP 412 (önkoşul başarısız) durum iletisi döndürür. Tam örnek burada indirebilirsiniz: [Azure depolama kullanarak eşzamanlılığı yönetme](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).  

```csharp
// Retrieve the ETag from the newly created blob
// Etag is already populated as UploadText should cause a PUT Blob call
// to storage blob service which returns the ETag in response.
string originalETag = blockBlob.Properties.ETag;

// This code simulates an update by a third party.
string helloText = "Blob updated by a third party.";

// No ETag provided so original blob is overwritten (thus generating a new ETag)
blockBlob.UploadText(helloText);
Console.WriteLine("Blob updated. Updated ETag = {0}",
blockBlob.Properties.ETag);

// Now try to update the blob using the original ETag provided when the blob was created
try
{
    Console.WriteLine("Trying to update blob using original ETag to generate if-match access condition");
    blockBlob.UploadText(helloText,accessCondition:
    AccessCondition.GenerateIfMatchCondition(originalETag));
}
catch (StorageException ex)
{
    if (ex.RequestInformation.HttpStatusCode == (int)HttpStatusCode.PreconditionFailed)
    {
        Console.WriteLine("Precondition failure as expected. Blob's original ETag no longer matches");
        // TODO: client can decide on how it wants to handle the 3rd party updated content.
    }
    else
        throw;
}  
```

Depolama hizmeti için de destek koşullu ek üst bilgilere gibi içerir **If-Modified-Since**, **IF-değiştirilmemiş-Since** ve **If-None-Match** yanı bilgilerin birleşimleri. Daha fazla bilgi için [belirtme koşullu üstbilgileri Blob hizmeti işlemleri için](https://msdn.microsoft.com/library/azure/dd179371.aspx) MSDN'de.  

Aşağıdaki tabloda özetlenmiştir koşullu üst bilgileri gibi kabul kapsayıcısı işlemleri **IF-Match** istek ve yanıtta bir ETag değeri döndürür.  

| İşlem | Kapsayıcı ETag değeri döndürür | Koşullu üstbilgileri kabul eder |
|:--- |:--- |:--- |
| Kapsayıcı oluşturma |Evet |Hayır |
| Kapsayıcı özellikleri alma |Evet |Hayır |
| Kapsayıcı meta verilerini al |Evet |Hayır |
| Kapsayıcı meta verileri ayarlama |Evet |Evet |
| Kapsayıcı ACL alın |Evet |Hayır |
| Kapsayıcı ACL ayarlayın |Evet |Evet (*) |
| Kapsayıcıyı Sil |Hayır |Evet |
| Kira kapsayıcı |Evet |Evet |
| Blobları Listele |Hayır |Hayır |

(*) SetContainerACL tarafından tanımlanan izinlere önbelleğe alınır ve bu izinleri güncelleştirmeleri 30 yaymak için hangi döneminde güncelleştirmeleri tutarlı olması garanti edilmez saniye sürebilir.  

Aşağıdaki tabloda özetlenmiştir koşullu üst bilgileri gibi kabul blob işlemleri **IF-Match** istek ve yanıtta bir ETag değeri döndürür.

| İşlem | ETag değeri döndürür | Koşullu üstbilgileri kabul eder |
|:--- |:--- |:--- |
| Put Blob |Evet |Evet |
| Get Blob |Evet |Evet |
| BLOB özelliklerini alma |Evet |Evet |
| Blob özelliklerini ayarlama |Evet |Evet |
| BLOB meta verilerini al |Evet |Evet |
| BLOB meta verileri ayarlama |Evet |Evet |
| Kira blob'u (*) |Evet |Evet |
| Blob anlık görüntüsü |Evet |Evet |
| Kopya blob'u |Evet |Evet (kaynak ve hedef blob için) |
| Kopyalama Blob durdurma |Hayır |Hayır |
| BLOB silme |Hayır |Evet |
| Blok yerleştirme |Hayır |Hayır |
| Engelleme listesi yerleştirin |Evet |Evet |
| Engelleme listesine alın |Evet |Hayır |
| Sayfa yerleştirin |Evet |Evet |
| Alma sayfası aralıkları |Evet |Evet |

(*) ETag bir blob üzerinde kiralama Blob değiştirmez.  

### <a name="pessimistic-concurrency-for-blobs"></a>Bloblar için kötümser eşzamanlılık
Özel kullanım için bir blob kilitlemek için edinebileceğinizi bir [kira](https://msdn.microsoft.com/library/azure/ee691972.aspx) üzerindeki. Bir kira, kiralama ne kadar süreyle ihtiyacınız belirtin: 15 ila 60 saniye veya özel bir kilit tutarları sonsuz, arasında bu için olabilir. Genişletmek için sınırlı bir kira yenileme yapabilir ve onunla tamamlandığında, herhangi bir kira serbest bırakabilirsiniz. Bu süre dolduğunda blob hizmetine otomatik olarak sınırlı kira serbest bırakır.  

Kira bir yazma kilididir dahil olmak üzere desteklenmek, farklı eşitleme stratejileri etkinleştirme / yazma, okuma, özel paylaşılan / özel okuma ve yazma paylaşılan / özel okuyun. Bir kira var. burada depolama hizmeti özel yazma işlemlerini (put, ayarlama ve silme işlemleri) uygular. okuma işlemleri için özel kullanım olanağını sağlar ancak bir kira kimliği tüm istemci uygulamaları kullanır ve aynı anda yalnızca bir istemci olduğundan emin olmak Geliştirici gerektiren bir Geçerli bir kira kimliği Okuma işlemleri bir kira kimliği sonuç paylaşılan okumalar içermez.  

Aşağıdaki C# kod parçacığı, 30 saniye olarak bir blob için bir özel kira alınıyor, blobun içeriğini güncelleştirme ve ardından kira serbest bir örnek gösterilmektedir. Blob hizmeti, zaten varsa geçerli bir kira blob üzerinde yeni bir kira çalıştığınızda, bir "HTTP (409) çakışma" durumu sonucunu döndürür. Aşağıdaki kod parçacığında bir **AccessCondition** depolama hizmeti blob güncelleştirmek için bir istek yaptığında kiralama bilgilerini kapsülleyen nesne.  Tam örnek burada indirebilirsiniz: [Azure depolama kullanarak eşzamanlılığı yönetme](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

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

Bir yazma işlemi kiralanmış blob kiralama kimliği geçmeden çalışırsanız, istek 412 bir hata ile başarısız olur. Çağırmadan önce kiralama süresi gerçekleştiriyorsanız **UploadText** yöntem, ancak yine de kira Kimliğini sağlamanız geçmek için istek ile de başarısız bir **412** hata. Kiralama bitiş süreleri ve kira kimliklerini yönetme hakkında daha fazla bilgi için bkz. [kira blob'u](https://msdn.microsoft.com/library/azure/ee691972.aspx) REST belgeleri.  

Aşağıdaki blob işlemleri kiraları kötümser eşzamanlılık yönetmek için kullanabilirsiniz:  

* Put Blob
* Get Blob
* BLOB özelliklerini alma
* Blob özelliklerini ayarlama
* BLOB meta verilerini al
* BLOB meta verileri ayarlama
* BLOB silme
* Blok yerleştirme
* Engelleme listesi yerleştirin
* Engelleme listesine alın
* Sayfa yerleştirin
* Alma sayfası aralıkları
* Anlık görüntü blobu - kiralama varsa, isteğe bağlı kira kimliği
* Kopya blob'u - hedef blob üzerinde kiralama var olup kira Kimliğini sağlamanız gerekir.
* Kopyalama Blob durdurma - sonsuz bir kira üzerinde hedef blob zaten varsa gerekli kimliği kiralama
* Kira blob'u  

### <a name="pessimistic-concurrency-for-containers"></a>Kapsayıcılar için kötümser eşzamanlılık
Kira kapsayıcılarına bloblar olarak desteklenmesi aynı eşitleme stratejileri etkinleştirme (özel yazma / okuma, özel bir yazma paylaşılan / özel okuma ve yazma paylaşılan / özel okuma) ancak farklı olarak BLOB Depolama hizmeti yalnızca olmama üzerinde zorlar silme işlemleri. Etkin bir kiralama içeren bir kapsayıcıya silmek için bir istemci etkin bir kiralama Kimliğiyle silme isteği içermelidir. Kira kimliği dahil olmak üzere diğer tüm kapsayıcı işlemleri kiralanmış bir kapsayıcı başarılı durumda olduklarını paylaşılan operations. Güncelleştirme (put veya grup) veya okuma işlemleri olmama gerekiyorsa ardından geliştiriciler bir istemci aynı anda yalnızca bir geçerli bir kira kimliği tüm istemcilerin bir kira kimliği ve, kullandığınızdan emin olun  

Aşağıdaki kapsayıcı işlemleri kiraları kötümser eşzamanlılık yönetmek için kullanabilirsiniz:  

* Kapsayıcıyı Sil
* Kapsayıcı özellikleri alma
* Kapsayıcı meta verilerini al
* Kapsayıcı meta verileri ayarlama
* Kapsayıcı ACL alın
* Kapsayıcı ACL ayarlayın
* Kira kapsayıcı  

Daha fazla bilgi için bkz.  

* [Blob hizmeti işlemleri için koşullu üst bilgilerini belirtme](https://msdn.microsoft.com/library/azure/dd179371.aspx)
* [Kira kapsayıcı](https://msdn.microsoft.com/library/azure/jj159103.aspx)
* [Kira blob'u](https://msdn.microsoft.com/library/azure/ee691972.aspx)

## <a name="managing-concurrency-in-the-table-service"></a>Tablo hizmetinde eşzamanlılığı yönetme
Tablo hizmeti, iyimser eşzamanlılık varlıklar, açıkça iyimser eşzamanlılık denetimlerinin gerçekleştirmek için seçtiğiniz gerekir blob hizmeti farklı olarak çalıştığınız varsayılan davranış olarak denetler kullanır. Tablo ve blob hizmetleri arasında herhangi bir fark ile blob hizmeti kapsayıcıları ve blobları eşzamanlılık yönetebilirsiniz ise varlıkları eşzamanlılık davranışını yalnızca yönetebileceğiniz bir yerdir.  

Tablo hizmetinde bir varlık döndürür aldığınızda ETag değeri, iyimser eşzamanlılığı kullanın ve tablo Depolama hizmetinden alınan başka bir işlem bir varlık değiştirdiği, kontrol etmek için kullanabilirsiniz. Bu işlem ana hat aşağıdaki gibidir:  

1. Tablo Depolama hizmetinden varlık almak, yanıt depolama hizmetinde bu varlıkla ilişkili geçerli tanımlayıcı tanımlayan bir ETag değeri içeriyor.
2. Varlık güncelleştirdiğinizde, zorunlu olarak 1. adımda aldığınız ETag değeri içeren **IF-Match** hizmetine gönderdiğiniz isteği üstbilgisi.
3. Hizmet isteği ETag değeri varlığın geçerli ETag değerine sahip karşılaştırır.
4. Varlığın geçerli ETag değeri, zorunlu olarak ETag farklıdır, **IF-Match** hizmet istek üstbilgisinde istemciye 412 hata döndürür. Bu, istemci için başka bir işlem olduğundan, istemci alınan varlık güncelleştirdi gösterir.
5. Varlığın geçerli ETag değeri ETag zorunlu olarak aynı olduğunda **IF-Match** istekteki üstbilgi veya **IF-Match** üst bilgiyi içeren joker karakter (*), hizmet gerçekleştirir İstenen işlem ve varlık, güncelleştirildiğini göstermek için geçerli ETag değeri güncelleştirir.  

Blob hizmeti, tablo hizmeti eklemek istemci gerektiğini unutmayın bir **IF-Match** güncelleştirme isteği başlığı. Ancak, bir koşulsuz kullanmaya zorlamak mümkün müdür (son yazıcı WINS stratejisini) güncelleştirmek ve istemci ayarlarsa eşzamanlılık denetimlerinin atlama **IF-Match** joker karakter (*) istekteki üstbilgi.  

Aşağıdaki C# kod parçacığı ya da önceden oluşturulmuş veya güncelleştirilmiş e-posta adresi olan alınan bir müşteri varlığı gösterir. İlk ekleme veya müşteri nesnesi ETag değeri işlemi depoları alma ve değiştirme işlemini yürütüldüğünde örnek aynı nesne örneği kullandığından, otomatik olarak ETag değeri etkinleştirme hizmete geri tablo hizmetine gönderir Eşzamanlılık ihlalleri için denetleyin. Tablo depolama varlıkta güncelleştirmiştir. başka bir işlem hizmeti bir HTTP 412 (önkoşul başarısız) durum iletisi döndürür.  Tam örnek burada indirebilirsiniz: [Azure depolama kullanarak eşzamanlılığı yönetme](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114).

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

Eşzamanlılık denetimi açıkça devre dışı bırakmak, ayarlamalısınız **ETag** özelliği **çalışan** nesnesine "*" değiştirme işlemini yürütmeden önce.  

```csharp
customer.ETag = "*";  
```

Tablo varlık işlemleri ETag değerlerini kullanma, aşağıdaki tabloda özetlenmiştir:

| İşlem | ETag değeri döndürür | IF-Match istek üst bilgisi gerektirir |
|:--- |:--- |:--- |
| Varlıkları sorgulayın |Evet |Hayır |
| Varlık Ekle |Evet |Hayır |
| Varlık güncelleştir |Evet |Evet |
| Varlık Birleştir |Evet |Evet |
| Varlığı Sil |Hayır |Evet |
| Varlığı değiştirme veya ekleme |Evet |Hayır |
| INSERT veya birleştirme varlık |Evet |Hayır |

Unutmayın **Ekle veya Değiştir varlık** ve **Ekle ya da birleştirme varlık** işlemleri yapıp *değil* tabloya bir ETag değeri göndermeyin herhangi bir eşzamanlılık denetimlerinin gerçekleştirilemiyor hizmeti.  

Genel tablolar kullanan geliştiricilerin ölçeklenebilir uygulamalar geliştirirken üzerinde iyimser eşzamanlılık yararlanmalıdır. Kötümser kilitleme gerekirse tabloları erişen her tablo için atanmış bir blob atayın ve tablosunda işletim önce blob üzerinde kiralama geçirmeye çalışan olduğunda bir yaklaşım geliştiriciler alabilir. Bu yaklaşım, tüm veri erişim yolları tablosunda işletim önce kiralayabilir emin olmak için uygulama gerekmez. Ayrıca, en düşük kira süresini ölçeklenebilirlik için dikkat gerektiren 15 saniye olan unutmamalısınız.  

Daha fazla bilgi için bkz.  

* [Varlıklar üzerinde işlemler](https://msdn.microsoft.com/library/azure/dd179375.aspx)  

## <a name="managing-concurrency-in-the-queue-service"></a>Kuyruk hizmetindeki eşzamanlılığı yönetme
Burada birden çok istemciye iletileri kuyruktan alıyor, hangi eşzamanlılık kuyruğa alma hizmeti, bir konudur bir senaryodur. Kuyruktan bir ileti alındığında, yanıt iletisi ve ileti silmek için gerekli bir üstten alma girişi değer içeriyor. Kuyruktan ileti otomatik olarak silinmez, ancak bunu alındıktan sonra visibilitytimeout parametresi tarafından belirtilen zaman aralığı boyunca diğer istemcilere görünür durumda. İletiyi alır istemci ileti işlendikten sonra TimeNextVisible tarafından belirtilen süreden önce öğenin hesaplanan yanıtın visibilitytimeout parametre değerine göre silmesi beklenir. Visibilitytimeout değerini TimeNextVisible değerini belirlemek için ileti alınır zaman eklenir.  

Kuyruk hizmeti için iyimser veya kötümser eşzamanlılık desteğine sahip değil ve bu nedenle istemciler bir kuyruktan alınan iletileri işleme iletileri bir kere etkili olacak şekilde işlenir emin olmanız gerekir. Bir son yazıcı WINS stratejisi SetQueueServiceProperties, SetQueueMetaData, SetQueueACL ve UpdateMessage güncelleştirme işlemleri için kullanılır.  

Daha fazla bilgi için bkz.  

* [Kuyruk hizmeti REST API'si](https://msdn.microsoft.com/library/azure/dd179363.aspx)
* [İletileri alma](https://msdn.microsoft.com/library/azure/dd179474.aspx)  

## <a name="managing-concurrency-in-the-file-service"></a>Dosya hizmeti içinde eşzamanlılığı yönetme
Dosya hizmeti, iki farklı protokol uç noktası – SMB ve REST kullanılarak erişilebilir. REST hizmeti İyimser kilitleme veya kötümser kilitleme desteği yoktur ve son yazıcı WINS strateji tüm güncelleştirmeleri takip eder. Dosya sistemi kilitleme mekanizmaları kötümser kilitleme gerçekleştirme olanağı dahil olmak üzere paylaşılan dosyaları – erişimi yönetmek için dosya paylaşımlarına bağlama SMB istemcileri yararlanabilirsiniz. Bir SMB istemcisinden bir dosyayı açtığında dosya erişim ve Paylaşım bu belirtir modu. "Yazma" veya "Okuma/yazma" bir dosya paylaşımı modu yanı sıra, "None" dosya erişimi ayar dosyası kapatılana kadar bir SMB istemcisinden tarafından kilitlenen dosyasında neden olur. Bir SMB istemcisinden dosya kilitli olduğu bir dosya REST işlem denenirse REST hizmeti SharingViolation hata koduyla, 409 (Çakışma) durum kodu döndürür.  

Bir SMB istemcisinden Sil için'bir dosya açıldığında, bu dosyada açık tanıtıcıları diğer SMB istemci kadar Silinmeyi Bekleyen kapalı olarak dosyayı işaretler. Bir dosya silme gibi bekleyen işaretlenmiş, ancak bu dosya üzerinde herhangi bir REST işlemi SMBDeletePending hata koduyla, 409 (Çakışma) durum kodu döndürür. SMB istemcisi dosyayı kapatmadan önce bekleyen silme bayrağını kaldırmak üzere mümkün olduğundan, 404 (bulunamadı) durum kodu döndürülmez. Diğer bir deyişle, durum kodu 404 (bulunamadı), yalnızca dosya kaldırıldığında beklenmektedir. Durumu Sil bekleyen bir SMB dosya olarak kullanılırken, dosyaları Listele sonuçları dahil edilmez olduğunu unutmayın. Ayrıca, REST Dosya Sil ve REST dizini silme işlemleri atomik olarak uygulanır ve bir bekleyen silme durumda neden unutmayın.  

Daha fazla bilgi için bkz.  

* [Dosya Yönetimi kilitler](https://msdn.microsoft.com/library/azure/dn194265.aspx)  

## <a name="summary-and-next-steps"></a>Summary ve sonraki adımlar
Microsoft Azure depolama hizmeti geliştiricilerin tehlikeye veya eşzamanlılık ve bunlar için uygulanacak gelmiş veri tutarlılığı gibi temel tasarım varsayımları yeniden başlatılmasına gerek kalmadan en karmaşık çevrimiçi uygulamalar ihtiyaçlarını karşılamak için tasarlanan verildi.  

Bu blogda başvurulan tam bir örnek uygulama için:  

* [Azure depolama - örnek uygulaması kullanarak eşzamanlılığı yönetme](https://code.msdn.microsoft.com/Managing-Concurrency-using-56018114)  

Bkz. Azure depolama hakkında daha fazla bilgi için:  

* [Microsoft Azure depolama giriş sayfası](https://azure.microsoft.com/services/storage/)
* [Azure Depolama’ya giriş](storage-introduction.md)
* Başlarken depolama [Blob](../blobs/storage-dotnet-how-to-use-blobs.md), [tablo](../../cosmos-db/table-storage-how-to-use-dotnet.md), [kuyrukları](../storage-dotnet-how-to-use-queues.md), ve [dosyaları](../storage-dotnet-how-to-use-files.md)
* Depolama mimarisi – [Azure Depolama: Güçlü tutarlılık ile yüksek oranda kullanılabilir bulut depolama hizmeti](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)

