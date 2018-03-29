---
title: Paylaşılan erişim imzaları (SAS) Azure storage'da kullanma | Microsoft Docs
description: BLOB, kuyruklar, tablolar ve dosyaları da dahil olmak üzere Azure Storage kaynaklarına erişimi devretmek için paylaşılan erişim imzaları (SAS) kullanmayı öğrenin.
services: storage
documentationcenter: ''
author: craigshoemaker
manager: jeconnoc
editor: tysonn
ms.assetid: 46fd99d7-36b3-4283-81e3-f214b29f1152
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 04/18/2017
ms.author: cshoe
ms.openlocfilehash: d3f8b3261f9e2e86dbcaa41b92111545abeffe54
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2018
---
# <a name="using-shared-access-signatures-sas"></a>Paylaşılan erişim imzaları (SAS) kullanma

Paylaşılan erişim imzası (SAS) hesap anahtarınızı sokmadan diğer istemcilere depolama hesabındaki nesnelere sınırlı erişim vermek için bir yol sağlar. Bu makalede, biz SAS modeline genel bakış sağlar, SAS en iyi uygulamaları incelemek ve bazı örneklere bakın.

Burada sunulan olanlar SAS kullanarak ek kod örnekleri için bkz: [.NET içinde Azure Blob Storage ile çalışmaya başlama](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) ve diğer örnekleri kullanılabilir [Azure Kod örnekleri](https://azure.microsoft.com/documentation/samples/?service=storage) kitaplığı. Örnek uygulamaları indir ve bunları çalıştırma veya github'daki kod göz atın.

## <a name="what-is-a-shared-access-signature"></a>Paylaşılan erişim imzası nedir?
Paylaşılan erişim imzası temsilci depolama hesabınızdaki kaynaklara erişim sağlar. Bir SAS ile hesap anahtarlarınızı paylaşmadan depolama hesabınızdaki kaynaklara erişim istemcileri verebilirsiniz. Bu, uygulamalarınızda paylaşılan erişim imzaları kullanmanın anahtar noktasıdır. SAS, hesap anahtarlarınızı tehlikeye atmadan depolama kaynaklarınızı paylaşmanın güvenli bir yoludur.

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

Bir SAS dahil olmak üzere, SAS olan istemcilere vermek erişim türünü üzerinde ayrıntılı denetim sağlar:

* SAS başlangıç saati ve sona erme saati dahil geçerli aralık.
* SAS tarafından verilen izinler. Örneğin, bir blob için bir SAS okuma izni ve o blob yazma izinleri, ancak izinleri silmemeniz.
* İsteğe bağlı bir IP adresi veya IP adresleri Azure Storage SAS kabul eder. Örneğin, kuruluşunuza ait bir IP adresi aralığı belirtebilirsiniz.
* SAS kabul edecek Azure Storage protokolü. Bu isteğe bağlı parametre HTTPS kullanan istemciler için erişimi kısıtlamak için kullanabilirsiniz.

## <a name="when-should-you-use-a-shared-access-signature"></a>Ne zaman bir paylaşılan erişim imzası kullanmalısınız?
Depolama hesabınızın erişim anahtarlarını işlediği olmayan herhangi bir istemciye depolama hesabınızdaki kaynaklara erişimi sağlamak istediğinizde bir SAS kullanabilirsiniz. Depolama hesabınızı hem her ikisi de yönetici hesabınıza erişim bir birincil ve ikincil erişim anahtarını ve içerdiği tüm kaynaklar içerir. Bu anahtarları birini gösterme kötü amaçlı veya ihmalkar kullanma olasılığını hesabınıza açar. Paylaşılan erişim imzaları okuma, yazma ve açıkça verilen izinler göre ve hesap anahtarı için gerek kalmadan depolama hesabınızdaki veri silmek istemcilerin güvenli bir alternatif sunar.

Bir SAS yararlı olduğu yaygın bir senaryo, kullanıcıların okuma ve kendi veri depolama hesabınıza yazma bir hizmettir. Bir depolama hesabı kullanıcı verilerini depoladığı bir senaryoda, iki tipik tasarım modeli vardır:

1. İstemciler, indirin ve kimlik doğrulaması yapan bir ön uç proxy hizmeti aracılığıyla veri yükleyin. Bu ön uç proxy hizmeti için iş kuralları doğrulanması izin vererek avantajı olsa da, büyük miktarlarda veri veya yüksek hacimli işlemleri için isteğe bağlı eşleşecek şekilde ölçeklenebilen bir hizmet oluşturuluyor pahalı ya da zor olabilir.

  ![Senaryo diyagramı: ön uç proxy hizmeti](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png)   

1. Basit bir hizmeti istemci gerektiği şekilde doğrular ve ardından bir SAS oluşturur. SAS istemci aldıktan sonra doğrudan SAS tarafından ve SAS tarafından izin verilen aralığı için tanımlanan izinlerle depolama hesabı kaynaklarına erişebilir. SAS ön uç proxy hizmeti üzerinden tüm verileri yönlendirme gereksinimini azaltır.

  ![Senaryo diyagramı: SAS sağlayıcı hizmeti](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png)   

Pek çok gerçek dünya hizmet bu iki yaklaşımdan karma kullanabilir. Örneğin, bazı veriler işlenen ve diğer verileri kaydedilen ve/veya SAS kullanarak doğrudan okuma sırasında ön uç proxy doğrulanabilir.

Ayrıca, belirli senaryolarda bir kopyalama işleminde kaynak nesne kimlik doğrulaması için bir SAS kullanmanız gerekir:

* Farklı depolama hesabında bulunduğu başka bir blob için bir blob kopyaladığınızda, kaynak blob kimliğini doğrulamak için bir SAS kullanmanız gerekir. Hedef blob de kimlik doğrulaması için bir SAS isteğe bağlı olarak kullanabilirsiniz.
* Farklı depolama hesabında bulunduğu başka bir dosyaya bir dosya kopyaladığınızda, kaynak dosya kimliğini doğrulamak için bir SAS kullanmanız gerekir. Hedef dosya de kimlik doğrulaması için bir SAS isteğe bağlı olarak kullanabilirsiniz.
* Bir blobu bir dosyaya veya bir blobu bir dosyaya kopyaladığınızda, aynı depolama hesabında kaynak ve hedef nesneler bulunurlar olsa bile, SAS kaynak nesnesinin kimliğini doğrulamak için kullanmanız gerekir.

## <a name="types-of-shared-access-signatures"></a>Paylaşılan erişim imzaları türleri
İki tür paylaşılan erişim imzası oluşturabilirsiniz:

* **Service SAS.** Hizmet SAS; Blob, Kuyruk, Tablo veya Dosya hizmeti olmak üzere yalnızca bir depolama hizmetindeki kaynağa erişim atar. Bkz: [hizmet SAS oluşturma](https://msdn.microsoft.com/library/dn140255.aspx) ve [hizmet SAS örnekler](https://msdn.microsoft.com/library/dn140256.aspx) hizmet SAS belirteci oluşturma hakkında ayrıntılı bilgi.
* **Hesap SAS.** Hesap SAS Temsilciler, bir veya daha fazla depolama hizmetindeki kaynaklara erişim. Tüm hizmet SAS kullanılabilir işlemlerini de hesap SAS kullanılabilir. Buna ek olarak, hesap SAS ile belirli bir hizmeti gibi uygulama işlemlerine erişim devredebilirsiniz **Get/Set hizmet özellikleri** ve **hizmeti istatistikleri almak**. Bununla birlikte hizmet SAS ile izin verilmeyen blob kapsayıcılar, tablolar kuyruklar ve dosya paylaşımları üzerinde okuma, yazma ve silme işlemleri için yetkilendirme yapabilirsiniz. Bkz: [bir hesap SAS oluşturma](https://msdn.microsoft.com/library/mt584140.aspx) hesap SAS belirteci oluşturma hakkında ayrıntılı bilgi.

## <a name="how-a-shared-access-signature-works"></a>Paylaşılan erişim imzası nasıl çalışır?
Paylaşılan erişim imzası bir veya daha fazla depolama kaynaklarına işaret ve özel bir sorgu parametreleri kümesini içeren bir belirteç içeren imzalı bir URI değil. İstemci tarafından kaynaklara nasıl erişebilir belirteci gösterir. Sorgu parametreleri, imza birini SAS parametrelerinden oluşturulur ve hesap anahtarı ile imzalanmış. Bu imza Azure Storage ile SAS kimlik doğrulaması için kullanılır.

Kaynak URI'si gösteren bir SAS URI'sini bir örneği burada verilmiştir ve SAS belirteci:

![SAS URI'sini bileşenleri](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png)   

SAS belirteci oluşturmak üzerinde bir dizedir *istemci* yan (bkz [SAS örnekler](#sas-examples) bölüm kod örnekleri için). Depolama istemcisi kitaplığı ile oluşturduğunuz bir SAS belirteci Örneğin, Azure Storage tarafından herhangi bir şekilde izlenmez. SAS belirteci sınırsız sayıda istemci tarafında oluşturabilirsiniz.

Bir istemci, SAS URI'sini Azure depolama alanına bir isteğin bir parçası sağladığında, hizmet isteği kimlik doğrulaması için geçerli olduğunu doğrulamak için imza ve SAS parametreleri denetler. Hizmet doğrularsa imza geçerli değil ve isteğin kimliği. Aksi takdirde, istek 403 (Yasak) hata koduyla reddedildi.

## <a name="shared-access-signature-parameters"></a>Paylaşılan erişim imzası parametreleri
Hizmet SAS belirteci ve hesap SAS bazı ortak parametreler içerir ve ayrıca farklı birkaç parametre alır.

### <a name="parameters-common-to-account-sas-and-service-sas-tokens"></a>Hesap SAS ortak parametreler ve hizmet SAS belirteci
* **API sürümü** isteğin yürütülmesi için kullanılacak depolama hizmeti sürümünü belirten isteğe bağlı bir parametre.
* **Hizmet sürümünü** istek kimliğini doğrulamak için kullanılacak depolama hizmeti sürümünü belirten gerekli bir parametre.
* **Başlangıç zamanı.** Hangi SAS geçerli hale geldiği tarih budur. Paylaşılan erişim imzası için başlangıç saati isteğe bağlıdır. Bir başlangıç saati atlanırsa, SAS hemen etkili olur. Başlangıç saati UTC (Eşgüdümlü Evrensel Saat), bir özel UTC göstergesi ("Z") ile örneğin ifade edilmesi gerekir `1994-11-05T13:15:30Z`.
* **Sona erme saati.** Bu, sonra SAS artık geçerli olmayan zamandır. En iyi uygulamalar için bir SAS bir sona erme saati belirtin veya bir saklı erişim ilkesi ile ilişkilendirebilirsiniz öneririz. Sona erme saati UTC (Eşgüdümlü Evrensel Saat), bir özel UTC göstergesi ("Z") ile örneğin ifade edilmesi gerekir `1994-11-05T13:15:30Z` (daha aşağıya bakın).
* **İzinler.** SAS belirtilen izinlerini istemci SAS kullanarak depolama kaynağı karşı gerçekleştirebilirsiniz hangi işlemleri gösterir. İzinleri hesap SAS ve hizmet SAS için farklıdır.
* **IP.** Bir IP adresi veya bir IP adresi aralığı Azure dışında belirten isteğe bağlı bir parametre (bölümüne bakın [yönlendirme oturum yapılandırma durumu](../../expressroute/expressroute-workflows.md#routing-session-configuration-state) hızlı rota için) isteklerini kabul etmek üzere.
* **Protokol.** Protokol belirten isteğe bağlı parametresi bir istek için izin verilir. Olası değerler şunlardır: HTTPS ve HTTP (`https,http`), olan varsayılan değeri veya HTTPS yalnızca (`https`). HTTP yalnızca izin verilen değeri olmadığını unutmayın.
* **İmza.** İmza bölümü belirteci olarak belirtilen ve ardından şifrelenmiş diğer parametreler oluşturulur. SAS kimlik doğrulaması için kullanılır.

### <a name="parameters-for-a-service-sas-token"></a>Hizmet SAS belirteci için parametreler
* **Depolama kaynağı.** Bir hizmeti ile erişim devredebilirsiniz depolama kaynaklarını SAS şunlardır:
  * Kapsayıcılar ve bloblar
  * Dosya paylaşımları ve dosyaları
  * Kuyruklar
  * Tabloları ve tablo varlıkları aralıkları.

### <a name="parameters-for-an-account-sas-token"></a>Bir hesap SAS belirteci için parametreler
* **Hizmet veya hizmetleri.** Hesap SAS bir veya daha fazla depolama hizmetleri için erişim devredebilirsiniz. Örneğin, temsilciler Blob ve dosya hizmete erişim hesap SAS oluşturabilirsiniz. Veya dört temsilciler erişimi (Blob, kuyruk, tablo ve dosya) Hizmetleri SAS oluşturabilirsiniz.
* **Depolama kaynak türleri.** Bir hesap SAS depolama kaynaklarını, yerine belirli bir kaynak bir veya daha fazla sınıfları için geçerlidir. Hesap erişimi devretmek için SAS oluşturabilirsiniz:
  * Hizmet düzeyi API'leri karşı depolama hesabı kaynağı olarak adlandırılır. Örnekler **Get/Set hizmet özellikleri**, **alma hizmeti istatistikleri**, ve **kapsayıcıları/sıraları/tablolar/paylaşımları**.
  * Her hizmet için kapsayıcı nesneleri karşı çağrılan kapsayıcı düzeyi API'leri: blob kapsayıcılar, kuyruklar, tablolar ve dosya paylaşımları. Örnekler **oluşturma/silme kapsayıcı**, **oluşturma/silme sıra**, **oluşturma/silme tablo**, **oluşturma/silme paylaşımı**, ve **listesi BLOB'lar/dosyaları ve dizinleri**.
  * BLOB, kuyruk iletileri, tablo varlıkları ve dosyaları karşı adlı nesne düzeyinde API'leri. Örneğin, **Put Blob**, **sorgu varlık**, **iletileri almak**, ve **dosyası oluştur**.

## <a name="examples-of-sas-uris"></a>SAS URI'ler örnekleri

### <a name="service-sas-uri-example"></a>Hizmet SAS URİ'si örneği

Bir hizmet sağlar SAS URI'sini okuma ve yazma izinlerine bir blobu bir örneği burada verilmiştir. Tablo için SAS nasıl katkıda bulunan anlamak için URI her parçası sekme ayırır:

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| Ad | SAS bölümü | Açıklama |
| --- | --- | --- |
| BLOB URI'si |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |Blob adresi. HTTPS kullanarak önerilir unutmayın. |
| Depolama Hizmetleri sürümü |`sv=2015-04-05` |Depolama Hizmetleri sürüm 2012-02-12 ve daha sonra bu parametre hangi sürümün kullanılacağını gösterir. |
| Başlangıç zamanı |`st=2015-04-29T22%3A18%3A26Z` |UTC saati belirtilmiş. SA'ları hemen geçerli olmasını istiyorsanız, başlangıç saati atlayın. |
| Süre sonu |`se=2015-04-30T02%3A23%3A26Z` |UTC saati belirtilmiş. |
| Kaynak |`sr=b` |Bir blob kaynaktır. |
| İzinler |`sp=rw` |SAS tarafından verilen izinler ve yazma (w) Read (r) içerir. |
| IP aralığı |`sip=168.1.5.60-168.1.5.70` |Bir isteği kabul edilecek IP adresleri aralığı. |
| Protokol |`spr=https` |Yalnızca HTTPS kullanarak isteklerine izin verilir. |
| İmza |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |Blob erişimde kimlik doğrulaması için kullanılır. İmza dize oturum ve SHA256 algoritmasını kullanarak anahtarı üzerinden hesaplanır ve Base64 kodlama kullanılarak kodlanan bir HMAC değil. |

### <a name="account-sas-uri-example"></a>Hesap SAS URI'sini örneği

Hesap SAS belirteci aynı ortak parametreleri kullanan bir örneği burada verilmiştir. Bu parametreler, yukarıda açıklanan olduğundan, burada açıklanmamaktadır. Yalnızca parametreleri, hesap SAS aşağıdaki tabloda açıklanan özgüdür.

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| Ad | SAS bölümü | Açıklama |
| --- | --- | --- |
| Kaynak URI'si |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |Blob Hizmeti uç noktası, (GET ile çağrıldığında) hizmeti özelliklerini alma veya (KÜMESİYLE çağrıldığında) hizmet özelliklerini ayarlama parametrelere sahip. |
| Hizmetler |`ss=bf` |SAS Blob ve dosya hizmetlere uygular |
| Kaynak türleri |`srt=s` |SAS hizmet düzeyi işlemleri için geçerlidir. |
| İzinler |`sp=rw` |Okuma ve yazma işlemleri için erişim izinleri verin. |

İzinler hizmet düzeyini sınırlı olduğuna, erişilebilir bu SAS ile işlemleridir **Blob hizmeti özellik alma** (okuma) ve **Blob hizmeti özelliklerini ayarla** (yazma). Ancak, farklı olan bir kaynak URI, aynı SAS belirteci da erişimi devretmek için kullanılabilecek **Blob hizmeti istatistikleri almak** (okuma).

## <a name="controlling-a-sas-with-a-stored-access-policy"></a>Bir SAS depolanmış erişim ilkesi ile denetleme
Paylaşılan erişim imzası iki biçimlerden birini gerçekleştirebilirsiniz:

* **Geçici SAS:** , başlangıç zamanı, bitiş zamanı, geçici bir SAS oluşturduğunuzda ve SAS izinlerini tüm SAS URI'sini belirtilen (veya zımni başlangıç saati atlanmış olduğu durumda). Bu tür bir SAS hesap SAS veya hizmet SAS olarak oluşturulabilir.
* **Saklı erişim ilkesi ile SAS:** depolanmış erişim ilkesi bir kaynak kapsayıcı--bir blob kapsayıcısını tanımlanır, tablo, kuyruk, veya dosya paylaşımı--ve bir veya daha fazla paylaşılan erişim imzaları kısıtlamalarını yönetmek için kullanılan. Bir SAS depolanmış erişim ilkesi ile ilişkilendirdiğinizde, SAS başlangıç saati, sona erme saati ve izinleri--depolanmış erişim ilkesi için tanımlı kısıtlamalar devralır.

> [!NOTE]
> Şu anda hesap SAS geçici bir SAS olmalıdır. Depolanmış. erişim ilkeleri SA hesabı için henüz desteklenmiyor.

İki tür arasındaki farkı bir anahtar senaryo için önemlidir: iptal. SAS URI'sini bir URL olduğundan, kimin ilk oluşturulduğu bağımsız olarak SAS alacağı herhangi bir kişi, kullanabilirsiniz. Bir SAS yayımlandığını, herkes tarafından kullanılabilir. Bir SAS kaynaklarına erişimi dört özelliklerinden biri işlem yapılana kadar işlediği herkese verir:

1. SAS belirtilen sona erme saati ulaşıldı.
2. SAS tarafından başvurulan depolanmış erişim ilkesinde belirtilen sona erme saati (depolanmış erişim ilkesi başvurulduğunda ve bir süre sonu zamanı belirtiyorsa) ulaşıldı. Bu aralığı sona erdiğinde olduğundan veya bir sona erme saati SAS iptal etmenin bir yolu geçmişte depolanmış erişim ilkesiyle değiştirdiğinden ortaya çıkabilir.
3. SAS iptal etmek için başka bir yol olduğu SAS tarafından başvurulan depolanmış erişim ilkesi silinir. Tam olarak aynı ada sahip depolanmış erişim ilkesi oluşturun, var olan tüm SAS belirteci yeniden (, SAS bitiş saati olmayan geçti varsayılarak) Bu saklı erişim ilkesiyle ilişkili izinleriyle geçerli olacağını unutmayın. SAS iptal amaçlanıyorsa, erişim ilkesi gelecekte bir sona erme saati ile yeniden oluşturmanız, farklı bir ad kullandığınızdan emin olun.
4. SAS oluşturmak için kullanılan hesap anahtar yeniden oluşturulacak. Hesap anahtarı yeniden oluşturuluyor diğer geçerli hesap anahtarı veya yeni yeniden hesap anahtarı kullanmak üzere güncelleştirilmiş kadar kimlik doğrulaması bu anahtarı kullanan tüm uygulama bileşenleri neden olur.

> [!IMPORTANT]
> Paylaşılan erişim imzası URI imzayı oluşturmak için kullanılan hesap anahtarı ile ilişkilidir, ve ilişkili erişim ilkesi (varsa) depolanır. Hiçbir depolanmış erişim ilkesi belirtilirse, paylaşılan erişim imzası iptal etmek için yalnızca hesap anahtarı değiştirmek için yoludur.

## <a name="authenticating-from-a-client-application-with-a-sas"></a>Bir istemci uygulamadan bir SAS ile kimlik doğrulaması
Bir SAS elinde olan bir istemci isteği hesabı anahtarları sahip olmayan bir depolama hesabı karşı kimlik doğrulaması yapmak için SAS kullanabilirsiniz. SAS bağlantı dizesi ile birlikte veya doğrudan uygun Oluşturucusu veya yöntemi kullanılır.

### <a name="using-a-sas-in-a-connection-string"></a>Bir SAS bağlantı dizesiyle
[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a>SAS Oluşturucusu veya yöntemi kullanarak
Hizmet SAS ile isteğine doğrulanabilmesi birkaç Azure Storage istemci kitaplığı oluşturucular ve yöntemi aşırı bir SAS parametre sunar.

Örneğin, burada bir SAS URI'sini blok blob başvurusu oluşturmak için kullanılır. SAS istek için gereken tek kimlik bilgileri sağlar. Blok blob başvurusu daha sonra bir yazma işlemi için kullanılır:

```csharp
string sasUri = "https://storagesample.blob.core.windows.net/sample-container/" +
    "sampleBlob.txt?sv=2015-07-08&sr=b&sig=39Up9JzHkxhUIhFEjEH9594DJxe7w6cIRCg0V6lCGSo%3D" +
    "&se=2016-10-18T21%3A51%3A37Z&sp=rcw";

CloudBlockBlob blob = new CloudBlockBlob(new Uri(sasUri));

// Create operation: Upload a blob with the specified name to the container.
// If the blob does not exist, it will be created. If it does exist, it will be overwritten.
try
{
    MemoryStream msWrite = new MemoryStream(Encoding.UTF8.GetBytes(blobContent));
    msWrite.Position = 0;
    using (msWrite)
    {
        await blob.UploadFromStreamAsync(msWrite);
    }

    Console.WriteLine("Create operation succeeded for SAS {0}", sasUri);
    Console.WriteLine();
}
catch (StorageException e)
{
    if (e.RequestInformation.HttpStatusCode == 403)
    {
        Console.WriteLine("Create operation failed for SAS {0}", sasUri);
        Console.WriteLine("Additional error information: " + e.Message);
        Console.WriteLine();
    }
    else
    {
        Console.WriteLine(e.Message);
        Console.ReadLine();
        throw;
    }
}

```

## <a name="best-practices-when-using-sas"></a>SAS kullanırken en iyi uygulamalar
Paylaşılan erişim imzaları uygulamalarınızda kullandığınızda iki olası riskleri bilmeniz gerekir:

* Bir SAS sızmasını varsa, depolama hesabınız tehlikeye atabilir aldığı herkes tarafından kullanılabilir.
* Bir SAS için sağladıysanız istemci uygulamanın süresi dolduktan ve uygulama hizmetinizden yeni SAS alamıyor sonra uygulamanın işlevselliğini engelliyordu.

Paylaşılan erişim imzaları kullanma aşağıdaki öneriler bu riskleri azaltmaya yardımcı:

1. **Her zaman HTTPS kullanmak** oluşturmak veya bir SAS dağıtmak için. Bir SAS ise ve HTTP üzerinden geçirilen ele man-in--middle saldırısı gerçekleştiren bir saldırgan SAS okuyabilmesini ve yalnızca hedeflenen kullanıcının, olası hassas verileri tehlikeye veya veri bozulması kötü niyetli bir kullanıcı tarafından izin vermeyi olabilir olarak kullanın.
2. **Başvuru erişim ilkeleri, mümkün olduğunda depolanır.** Saklı erişim ilkeleri depolama hesabı anahtarlarını yeniden oluşturmak zorunda kalmadan izinler seçeneği sunar. Sona erme tarihini bu kadar çok gelecekte (veya sonsuz) üzerinde ayarlayabilir ve geleceğe uzağa taşımak için düzenli olarak güncelleştirilen emin olun.
3. **Zaman aşımı değeri yakın dönemde üzerinde geçici bir SAS kullanın.** Bir SAS tehlikede olsa bile bu şekilde, onu yalnızca kısa bir süre için geçerli değil. Bu yöntem, depolanmış erişim ilkesi başvuramaz, özellikle önemlidir. Yakın dönemde zaman aşımı değeri de bir blobu için karşıya yüklemek için zaman sınırlayarak yazılabilir veri miktarını sınırlar.
4. **Gerekirse, SAS otomatik olarak yenilemek istemciler vardır.** İstemcileri iyi dolmasına SAS SAS sağlayan hizmet kullanılamıyorsa, yeniden deneme zaman izin vermek üzere yenilemelisiniz. Az sayıda sona erme süresi içinde tamamlanması beklenen hemen, kısa süreli işlemleri için kullanılacak, SAS istediyseniz, ardından bu SAS yenilenmesi beklendiği gibi gereksiz olabilir. Ancak, düzenli olarak SAS aracılığıyla istek yapıyor istemciniz varsa, sona erme olasılığını oyuna gelir. (Daha önce belirtildiği gibi) kısa süreli olarak SAS gereksinimini dengelemek için anahtar dikkat etmeniz gereken istemci yenileme erken istediğini sağlamak için gereken ile (önce başarılı yenileme zaman aşımına uğramak SAS nedeniyle kesintisi yaşamamak için) yeterli.
5. **SAS başlangıç tarihine sahip dikkatli olun.** Bir SAS için başlangıç zamanı ayarlarsanız **şimdi**, ardından (farklı makinelerde göre geçerli saat farklılıkları) nedeniyle saat eğriltme, hataları gözlenen zaman zaman için ilk az dakika. Genel olarak, en az 15 dakika geçmiş olması için başlangıç saatini ayarlayın. Ya da hiç, bunu hemen tüm durumlarda geçerli yapacak ayarlamanız gerekmez. Aynı genellikle de--sona erme saati geçerli saati, 15 dakika içinde herhangi bir istek üzerinde herhangi bir yönde eğme gözlemlemek unutmayın. Bir REST sürüm 2012-02-12 önce kullanan istemciler için bir saklı erişim ilkesi başvurmayan bir SAS için en uzun süre 1 saat, başarısız olur daha uzun vadeli belirterek tüm ilkeleri ise.
6. **Erişilecek kaynakla belirli olabilir.** En iyi güvenlik uygulaması, en düşük gerekli ayrıcalıklara bir kullanıcı sağlamaktır. Bir kullanıcı yalnızca tek bir varlık için okuma erişimi gerekiyorsa, daha sonra bunları tek bir varlık için okuma erişimi ve tüm varlıklar için değil okuma/yazma/silme erişim verin. Bu da SAS saldırgan elinizde daha az güç olduğundan SAS aşılıp aşılmadığını hasarı azaltmak yardımcı olur.
7. **Hesabınız ile SAS yapılan dahil olmak üzere tüm kullanım için Fatura edilecek anlayın.** Bir blob yazma erişimi sağlarsanız, 200 GB blob karşıya yüklemek bir kullanıcı seçebilirsiniz. Okuma erişimi verdiniz varsa, bunlar 10 kez indirmek 2 TB çıkış sizin için maliyet seçebilirsiniz. Yeniden, kötü amaçlı kullanıcılar olası eylemleri azaltmaya yardımcı olmak için sınırlı izinleri sağlar. Bu tehdidi azaltmak (ancak saatin son saat eğriltme oluşturduğunu için) kısa süreli SAS kullanın.
8. **SAS kullanarak yazılan veriler doğrulayın.** Bir istemci uygulaması depolama hesabınıza veri yazarken, verileri sorunları olabilir aklınızda bulundurun. Uygulamanız bu verileri doğrulanmış veya kullanıma hazır hale gelmeden önce yetkili gerektiriyorsa, bu doğrulama verileri yazıldıktan sonra ve uygulamanız tarafından kullanılan önce gerçekleştirmeniz gerekir. Bu yöntem aynı zamanda, hesabınıza düzgün SAS alınan bir kullanıcı tarafından veya bir sızan SAS yararlanmasını bir kullanıcı tarafından yazılan bozuk veya kötü amaçlı veriler korur.
9. **Her zaman SAS kullanmayın.** Bazen, depolama hesabınız karşı belirli bir işlemle ilişkili riskleri SAS avantajlarından daha ağır basar. Bu tür işlemler için iş gerçekleştirildikten sonra depolama hesabınıza Yazar bir orta katman hizmet oluşturma kuralı doğrulama, kimlik doğrulaması ve denetim. Ayrıca, bazen onu diğer yollarla erişimi yönetmek daha kolaydır. Örneğin, tüm BLOB'ları bir kapsayıcıda herkes tarafından okunabilir yapmak istiyorsanız, ortak, kapsayıcı yapabilirsiniz yerine bir SAS her istemci için erişim sağlama.
10. **Storage Analytics uygulamanızı izlemek için kullanın.** Herhangi bir kesinti nedeniyle kimlik doğrulama hataları aşırı SAS sağlayıcısı hizmetiniz veya depolanmış erişim ilkesi yanlışlıkla kaldırılmasını izlemek için günlüğe kaydetme ve ölçümleri'ni kullanabilirsiniz. Bkz: [Azure depolama ekibi blogu](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) ek bilgi için.

## <a name="sas-examples"></a>SAS örnekleri
Aşağıda bazı örnekler her iki tür paylaşılan erişim imzalar, hesap SAS ve SAS hizmet.

Bu C# örnekleri çalıştırmak için aşağıdaki NuGet paketlerini projenize başvuru gerekir:

* [.NET için Azure Storage istemci Kitaplığı](http://www.nuget.org/packages/WindowsAzure.Storage), sürüm 6.x ya da daha sonra (SA hesabı kullanmak üzere).
* [Azure Configuration Manager](http://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager)

Oluşturma ve bir SAS test Göster ek örnekler için bkz: [depolama için Azure Kod örnekleri](https://azure.microsoft.com/documentation/samples/?service=storage).

### <a name="example-create-and-use-an-account-sas"></a>Örnek: Oluşturma ve hesap SAS kullanma
Aşağıdaki kod örneğinde hesap Blob ve Dosya Hizmetleri için geçerlidir ve istemciye izinleri okuma, yazma ve liste hizmet düzeyi API'lere erişim izni verir SAS oluşturur. İstek ile HTTPS yapılmalıdır şekilde hesap SAS Protokolü HTTPS için sınırlar.

```csharp
static string GetAccountSASToken()
{
    // To create the account SAS, you need to use your shared key credentials. Modify for your account.
    const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

    // Create a new access policy for the account.
    SharedAccessAccountPolicy policy = new SharedAccessAccountPolicy()
        {
            Permissions = SharedAccessAccountPermissions.Read | SharedAccessAccountPermissions.Write | SharedAccessAccountPermissions.List,
            Services = SharedAccessAccountServices.Blob | SharedAccessAccountServices.File,
            ResourceTypes = SharedAccessAccountResourceTypes.Service,
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Protocols = SharedAccessProtocol.HttpsOnly
        };

    // Return the SAS token.
    return storageAccount.GetSharedAccessSignature(policy);
}
```

Blob hizmeti için hizmet düzeyi API'leri erişmek için hesap SAS kullanmak için depolama hesabınız için SAS ve Blob storage uç kullanarak bir Blob istemci nesnesi oluşturun.

```csharp
static void UseAccountSAS(string sasToken)
{
    // Create new storage credentials using the SAS token.
    StorageCredentials accountSAS = new StorageCredentials(sasToken);
    // Use these credentials and the account name to create a Blob service client.
    CloudStorageAccount accountWithSAS = new CloudStorageAccount(accountSAS, "account-name", endpointSuffix: null, useHttps: true);
    CloudBlobClient blobClientWithSAS = accountWithSAS.CreateCloudBlobClient();

    // Now set the service properties for the Blob client created with the SAS.
    blobClientWithSAS.SetServiceProperties(new ServiceProperties()
    {
        HourMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        MinuteMetrics = new MetricsProperties()
        {
            MetricsLevel = MetricsLevel.ServiceAndApi,
            RetentionDays = 7,
            Version = "1.0"
        },
        Logging = new LoggingProperties()
        {
            LoggingOperations = LoggingOperations.All,
            RetentionDays = 14,
            Version = "1.0"
        }
    });

    // The permissions granted by the account SAS also permit you to retrieve service properties.
    ServiceProperties serviceProperties = blobClientWithSAS.GetServiceProperties();
    Console.WriteLine(serviceProperties.HourMetrics.MetricsLevel);
    Console.WriteLine(serviceProperties.HourMetrics.RetentionDays);
    Console.WriteLine(serviceProperties.HourMetrics.Version);
}
```

### <a name="example-create-a-stored-access-policy"></a>Örnek: bir saklı erişim ilkesi oluşturma
Aşağıdaki kod bir kapsayıcıda depolanmış erişim ilkesi oluşturur. Erişim ilkesi, kapsayıcıyı veya bloblarını hizmet SAS için sınırlamalar belirlemek için kullanabilirsiniz.

```csharp
private static async Task CreateSharedAccessPolicyAsync(CloudBlobContainer container, string policyName)
{
    // Create a new shared access policy and define its constraints.
    // The access policy provides create, write, read, list, and delete permissions.
    SharedAccessBlobPolicy sharedPolicy = new SharedAccessBlobPolicy()
    {
        // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
        // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
        SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
        Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.List |
            SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create | SharedAccessBlobPermissions.Delete
    };

    // Get the container's existing permissions.
    BlobContainerPermissions permissions = await container.GetPermissionsAsync();

    // Add the new policy to the container's permissions, and set the container's permissions.
    permissions.SharedAccessPolicies.Add(policyName, sharedPolicy);
    await container.SetPermissionsAsync(permissions);
}
```

### <a name="example-create-a-service-sas-on-a-container"></a>Örnek: bir kapsayıcıda hizmet SAS oluşturma
Aşağıdaki kod bir kapsayıcıda SAS oluşturur. Varolan bir depolanmış erişim ilkesi adı sağlanmazsa, bu ilkeyi SAS ile ilişkilidir. Hiçbir depolanmış erişim ilkesi sağlanırsa, kodu bir geçici SAS kapsayıcısını oluşturur.

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocPolicy = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.List
        };

        // Generate the shared access signature on the container, setting the constraints directly on the signature.
        sasContainerToken = container.GetSharedAccessSignature(adHocPolicy, null);

        Console.WriteLine("SAS for blob container (ad hoc): {0}", sasContainerToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the container. In this case, all of the constraints for the
        // shared access signature are specified on the stored access policy, which is provided by name.
        // It is also possible to specify some constraints on an ad-hoc SAS and others on the stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

### <a name="example-create-a-service-sas-on-a-blob"></a>Örnek: bir blob üzerindeki hizmet SAS oluşturma
Aşağıdaki kod, bir blob üzerindeki bir SAS oluşturur. Varolan bir depolanmış erişim ilkesi adı sağlanmazsa, bu ilkeyi SAS ile ilişkilidir. Hiçbir depolanmış erişim ilkesi sağlanırsa, kod blob üzerindeki bir geçici SAS oluşturur.

```csharp
private static string GetBlobSasUri(CloudBlobContainer container, string blobName, string policyName = null)
{
    string sasBlobToken;

    // Get a reference to a blob within the container.
    // Note that the blob may not exist yet, but a SAS can still be created for it.
    CloudBlockBlob blob = container.GetBlockBlobReference(blobName);

    if (policyName == null)
    {
        // Create a new access policy and define its constraints.
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad-hoc SAS, and
        // to construct a shared access policy that is saved to the container's shared access policies.
        SharedAccessBlobPolicy adHocSAS = new SharedAccessBlobPolicy()
        {
            // When the start time for the SAS is omitted, the start time is assumed to be the time when the storage service receives the request.
            // Omitting the start time for a SAS that is effective immediately helps to avoid clock skew.
            SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24),
            Permissions = SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Create
        };

        // Generate the shared access signature on the blob, setting the constraints directly on the signature.
        sasBlobToken = blob.GetSharedAccessSignature(adHocSAS);

        Console.WriteLine("SAS for blob (ad hoc): {0}", sasBlobToken);
        Console.WriteLine();
    }
    else
    {
        // Generate the shared access signature on the blob. In this case, all of the constraints for the
        // shared access signature are specified on the container's stored access policy.
        sasBlobToken = blob.GetSharedAccessSignature(null, policyName);

        Console.WriteLine("SAS for blob (stored access policy): {0}", sasBlobToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

## <a name="conclusion"></a>Sonuç
Paylaşılan erişim imzalar, depolama hesabınıza sınırlı izinlere hesap anahtarı olmamalıdır istemcilere sağlamak için yararlıdır. Bu nedenle, Azure Storage kullanarak herhangi bir uygulama için güvenlik modelinin önemli bir parçası olan. Burada listelenen en iyi uygulamaları izlerseniz, uygulamanızın güvenliği tehlikeye atmadan depolama hesabınızdaki kaynaklarına erişim konusunda daha fazla esneklik sağlamak için SAS'ı kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar
* [Paylaşılan erişim imzası, bölüm 2: Oluşturma ve bir SAS Blob storage'ı kullanma](../blobs/storage-dotnet-shared-access-signature-part-2.md)
* [Kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md)
* [Paylaşılan Erişim İmzası ile Erişim için Temsilci Seçme](http://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Tablo ve kuyruk SAS Tanıtımı](http://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
