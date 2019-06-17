---
title: Microsoft Azure depolama için .NET ile istemci tarafı şifreleme | Microsoft Docs
description: .NET için Azure depolama istemci kitaplığı, Azure depolama uygulamalarınız için en yüksek güvenlik için istemci tarafı şifreleme ve Azure anahtar kasası ile tümleştirmeyi destekler.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 10/20/2017
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 93386bd1fa3be88cbcdfab3d59ae07d3eb2b046d
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65911930"
---
# <a name="client-side-encryption-and-azure-key-vault-for-microsoft-azure-storage"></a>Microsoft Azure depolama istemci tarafı şifreleme ve Azure anahtar kasası
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Genel Bakış
[.NET için Azure depolama istemci Kitaplığı](/dotnet/api/overview/azure/storage/client) Azure Storage'a yüklemeden ve istemciye indirirken verilerin şifresini çözme önce istemci uygulamalar içinde verilerin şifrelenmesini destekler. Kitaplık ayrıca ile tümleştirmeyi destekler [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) depolama hesabı anahtarı yönetimi için.

İstemci tarafı şifreleme ve Azure anahtar Kasası'nı kullanarak blobları şifreleme işlemi boyunca size yol gösterir bir adım adım öğretici için bkz [Azure anahtar Kasası'nı kullanarak Microsoft Azure depolama blobları şifreleme ve şifre çözme](../blobs/storage-encrypt-decrypt-blobs-key-vault.md).

Java ile istemci tarafı şifreleme için [Java için Microsoft Azure depolama ile istemci tarafı şifreleme](storage-client-side-encryption-java.md).

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Şifreleme ve şifre çözme aracılığıyla Zarf yöntemi
Şifreleme ve şifre çözme işlemleri için zarf teknik izleyin.

### <a name="encryption-via-the-envelope-technique"></a>Şifreleme aracılığıyla Zarf yöntemi
Şifreleme aracılığıyla Zarf tekniği aşağıdaki şekilde çalışır:

1. Azure depolama istemci kitaplığı, bir kerelik kullanım simetrik anahtarı olan bir içerik şifreleme anahtarı (CEK) oluşturur.
2. Kullanıcı verileri, bu CEK kullanılarak şifrelenir.
3. CEK (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) paketlenir. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti veya bir simetrik anahtar ve yerel olarak yönetilebilir veya Azure anahtar Kasalarında depolanan.
   
    Depolama istemcisi kitaplığı kendisini KEK hiçbir zaman erişebilir. Key Vault tarafından sağlanan anahtar sarmalama algoritma kitaplığı çağırır. Kullanıcıların özel sağlayıcıları anahtar sarmalama/çözülme için isterseniz kullanmayı da tercih edebilirsiniz.

4. Şifrelenmiş veriler, ardından Azure depolama hizmetine yüklenir. Bazı ek şifreleme meta verilerle birlikte Sarmalanan anahtar meta verileri (temel, bir blob) olarak depolanan veya şifrelenmiş veriler (kuyruk iletileri ve tablo varlıkları) ile ilişkilendirilmiş.

### <a name="decryption-via-the-envelope-technique"></a>Zarf teknik aracılığıyla şifre çözme
Şifre çözme aracılığıyla Zarf tekniği aşağıdaki şekilde çalışır:

1. İstemci Kitaplığı, kullanıcı anahtar şifreleme anahtarı (KEK) yerel olarak veya Azure anahtar kasalarını yönetme varsayar. Kullanıcı, şifreleme için kullanılan özel anahtarı bilmeniz gerekmez. Bunun yerine, anahtarlar için farklı anahtar tanımlayıcıları gideren bir anahtar çözümleyici ayarlayabilir ve kullanılır.
2. İstemci Kitaplığı, şifrelenmiş veriler service üzerinde depolanan herhangi bir şifreleme materyalin birlikte yükler.
3. Sarmalanmış halden (şifresi) anahtar şifrelemesi kullanarak anahtarı (KEK) daha sonra (CEK) Sarmalanan içerik şifreleme anahtarı var. Burada yine, istemci kitaplığı KEK erişimi yok. Yalnızca özel veya Key Vault sağlayıcısının açma algoritması çağırır.
4. İçerik şifreleme anahtarı (CEK), ardından şifreli kullanıcı verilerin şifresini çözmek için kullanılır.

## <a name="encryption-mechanism"></a>Şifreleme mekanizması
Depolama istemcisi kitaplığı kullanan [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) kullanıcı verilerini şifrelemek için. Özellikle, [Şifre blok zincirleme (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) AES moduyla. Her bir hizmet works biraz farklı şekilde, her biri aşağıda ele alınacaktır.

### <a name="blobs"></a>Bloblar
İstemci Kitaplığı, şu anda yalnızca tüm blobları şifreleme desteklemektedir. Özellikle, kullanıcıların kullanması durumunda şifreleme desteklenir **UploadFrom** yöntemleri veya **OpenWrite** yöntemi. İndirmeler, hem tam hem aralığı indirmeler desteklenen için.

Şifreleme sırasında istemci kitaplığı rastgele başlatma vektörü (IV) rasgele bir içerik şifreleme anahtarı (CEK) 32 bayt ile birlikte 16 bayt oluşturmak ve bu bilgileri kullanarak blob verisi şifreleme Zarf gerçekleştirin. Sarmalanan CEK ve bazı ek şifreleme meta verileri blob hizmetinde şifrelenmiş bir blobu yanı sıra meta veri olarak depolanır.

> [!WARNING]
> Düzenleme veya kendi blob meta verilerini karşıya yükleme, bu meta veriler korunduğundan emin olmak gerekir. Karşıya yüklediğiniz bu meta veriler, sarmalanmış CEK IV ve diğer meta veriler olmadan yeni meta veriler kaybolur ve blob içeriği hiçbir zaman tekrar alınabilir olmaz.
> 
> 

Şifrelenmiş bir blobu indirme içeren tüm blob kullanarak içerik alma **DownloadTo**/**BlobReadStream** kullanışlı yöntemler. Sarmalanan CEK sarmalanmamış ve şifresi çözülmüş veriler kullanıcılara döndürmek için (Bu durumda blob meta veri olarak depolanır) IV ile birlikte kullanılır.

Bir rastgele aralık indiriliyor (**DownloadRange** yöntemleri) az miktarda bir başarıyla istenen şifresini çözmek için kullanılan ek veri alabilmek için kullanıcı tarafından sağlanan aralık ayarlamak şifrelenmiş bir blobu içerir. Aralık.

Tüm türleri blob (blok blobları, sayfa blobları ve ekleme blobları) şifrelenmiş/bu düzeni kullanarak şifresi.

### <a name="queues"></a>Kuyruklar
Kuyruk iletileri herhangi biçimi olamayacağından, istemci kitaplığının metinde başlatma vektörü (IV) ve şifrelenmiş içerik şifreleme anahtarı (CEK) içeren özel bir biçim tanımlar.

Şifreleme sırasında istemci kitaplığı, rastgele bir CEK 32 bayt ile birlikte 16 bayt rastgele bir IV oluşturur ve bu bilgileri kullanarak sıraya ileti metninin Zarf şifreleme gerçekleştirir. Ardından Sarmalanan CEK ve bazı ek şifreleme meta verileri için şifrelenmiş kuyruk iletisi eklenir. (Aşağıda gösterilen) değiştirilmiş bu ileti, hizmette depolanır.

    <MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>

Şifre çözme sırasında sarmalanmış anahtarı ve kuyruk iletiyi ayıklanır ve sarmalanmamış. IV ayrıca ve kuyruk iletiyi ayıklanır ve kuyruk iletisi verilerin şifresini çözmek için sarmalanmış halden anahtarıyla birlikte kullanılır. Bir kuyruk iletisi için 64 KB'lık sınırı, değerlendirmelerde olsa da etkisi yönetilebilir bu nedenle, şifreleme meta verileri (altında 500 bayt), küçük olduğunu unutmayın.

### <a name="tables"></a>Tablolar
İstemci Kitaplığı, varlık özelliklerini şifreleme eklemeyi destekler ve işlemleri değiştirin.

> [!NOTE]
> Birleştirme şu anda desteklenmiyor. Bir özellik alt kümesi daha önce farklı bir anahtarla şifrelenmiş olabilecek olduğundan, yalnızca birleştirme yeni özellikleri ve meta verilerini güncelleştirme veri kaybına yol açar. Özellik başına yeni bir anahtar kullanarak, ikisi için de performansla ilgili nedenlerden dolayı uygun değil veya hizmetten önceden var olan bir varlığa okumak için ek hizmet çağrıları yapma ya da birleştirme gerektirir.
> 
> 

Tablo verileri şifreleme gibi çalışır:  

1. Kullanıcılar şifrelenmiş özelliklerini belirtin.
2. İstemci Kitaplığı, rasgele içerik bir şifreleme anahtarı (CEK) 32 bayt her varlık için birlikte 16 bayt bir rastgele başlatma vektörü (IV) oluşturur ve özellik başına yeni bir IV türetme tarafından şifrelenmiş özellikler Zarf şifreleme gerçekleştirir. Şifrelenmiş özelliği ikili veri olarak depolanır.
3. Sarmalanan CEK ve bazı ek şifreleme meta verileri sonra iki ek ayrılmış özellikleri olarak depolanır. İlk ayrılmış özelliği (_ClientEncryptionMetadata1) IV, sürüm ve Sarmalanan anahtar bilgilerini tutan bir dize özelliğidir. İkinci ayrılmış bir özellik (_ClientEncryptionMetadata2) şifrelenmiş özellikleri hakkında bilgi içeren bir ikili bir özelliktir. Bu ikinci özelliğinde (_ClientEncryptionMetadata2) kendisi şifrelenmiş bilgilerdir.
4. Şifreleme için gereken bu ek ayrılmış özellikleri nedeniyle, kullanıcılar artık 252 yerine yalnızca 250 özel özellikler olabilir. Varlık toplam boyutu 1 MB'tan az olmalıdır.

Yalnızca dize özellikleri şifrelenmiş olduğunu unutmayın. Diğer özellikler şifrelenmesini türlerindeyse, dizeleri dönüştürülmelidir. Şifrelenmiş dizeleri hizmette ikili özellikleri olarak depolanır ve şifre çözme sonra geri dizelere dönüştürülür.

Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenmiş özelliklerini belirtmeniz gerekir. Bu, ya da (TableEntity türetilen POCO varlık için) bir [EncryptProperty] özniteliği ya da bir şifreleme çözümleyici istek seçenekleri belirterek yapılabilir. Bir şifreleme çözümleyici bölüm anahtarını, satır anahtarını ve özellik adını alır ve bu özellik şifrelenmesi gerekip gerekmediğini belirten bir Boole değeri döndüren bir temsilcidir. Şifreleme sırasında istemci kitaplığı, bir özellik için kablo yazılırken şifrelenmesi gerekip gerekmediğine karar vermek için bu bilgileri kullanır. Temsilci özellikleri nasıl şifrelenir etrafında mantıksal olasılığı için de sağlar. (X, örneğin, daha sonra özellik A şifrelemek; Aksi takdirde özellik A ve b şifreleme) Bunu okurken veya varlıkları sorgulayarak bu bilgiyi sağlamak gerekli olduğunu unutmayın.

### <a name="batch-operations"></a>Toplu işlemler
İstemci Kitaplığı, yalnızca bir seçenekler nesnesi (ve bu nedenle bir ilke/KEK) izin verdiğinden toplu işlemde aynı KEK, toplu işlemdeki tüm satırlar boyunca toplu işlem kullanılır. Ancak, istemci kitaplığı dahili olarak yeni rastgele IV ve rastgele CEK her satır toplu işleme oluşturur. Kullanıcılar bu davranışı şifreleme Çözümleyicisi tanımlayarak toplu işlemdeki her işlem için farklı özellikleri şifrelemek de seçebilirsiniz.

### <a name="queries"></a>Sorgular
> [!NOTE]
> Varlıkları şifrelendiği, filtre sorgularını bir şifrelenmiş özellikte çalıştıramazsınız.  Denerseniz, şifrelenmiş veriler şifrelenmemiş veri ile Karşılaştırılacak Service'i denediğiniz çünkü sonuçlar hatalı olacaktır.
> 
> 
> Sorgu işlemleri gerçekleştirmek için sonuç kümesinde tüm anahtarları çözümleyemiyorsa anahtar bir çözümleyici belirtmeniz gerekir. Sorgu sonucuna yer alan bir varlık için bir sağlayıcı çözümlenemezse, istemci kitaplığının bir hata atar. Sunucu tarafı projeksiyonlar gerçekleştirir herhangi bir sorgu için istemci kitaplığının özel şifreleme meta veri özelliklerini (_ClientEncryptionMetadata1 ve _ClientEncryptionMetadata2) varsayılan olarak seçilen sütunlara ekler.

## <a name="azure-key-vault"></a>Azure Key Vault
Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Azure anahtar Kasası'nı kullanarak anahtarları ve gizli anahtarları (örneğin, kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarı, kullanıcılar şifreleyebilirsiniz PFX dosyaları ve parolalar), donanım güvenlik modülleri (HSM'ler) tarafından korunan anahtarları kullanarak. Daha fazla bilgi için [Azure anahtar kasası nedir?](../../key-vault/key-vault-whatis.md).

Depolama istemcisi kitaplığı Key Vault çekirdek kitaplığı anahtarlarını yönetmek için Azure genelinde ortak bir çerçeve sağlamak için kullanır. Kullanıcılar ayrıca anahtar kasası uzantıları kitaplığı kullanılarak ek bir avantaja alır. Uzantıları kitaplığı, basit ve sorunsuz simetrik/RSA yerel ve anahtar bulut sağlayıcılarının yanı sıra toplama ve önbelleğe alma ile kullanışlı işlevsellik sağlar.

### <a name="interface-and-dependencies"></a>Arabirim ve bağımlılıkları
Üç Key Vault paketi vardır:

* Microsoft.Azure.KeyVault.Core IKeyResolver ve ikey değerini içerir. Hiçbir bağımlılık ile küçük bir pakettir. .NET için depolama istemci kitaplığı, bir bağımlılık olarak tanımlar.
* Microsoft.Azure.KeyVault, anahtar kasası REST istemcisi içerir.
* Microsoft.Azure.KeyVault.Extensions şifreleme algoritmaları ve bir RSAKey ve bir SymmetricKey uygulamaları içerir uzantı kodu içerir. Bu, çekirdek ve KeyVault ad alanlarında bağlıdır ve bir toplama Çözümleyicisi (kullanıcılar birden çok anahtar sağlayıcı kullanmak istediğinizde) ve bir önbellek anahtar Çözümleyicisi tanımlamak için işlevsellik sağlar. Kullanıcılar, Azure anahtar kasası anahtarlarını depolamak için veya yerel kullanmak ve bulut şifreleme sağlayıcıları için Key Vault uzantıları kullanmak için kullanmak istiyorsanız, depolama istemcisi kitaplığı bu paketi, doğrudan benzemez olsa da, bu paket ihtiyacınız olacaktır.

Key Vault yüksek değerli ana anahtarları için tasarlanmıştır ve her Key Vault azaltma sınırları bunu aklınızda tasarlanmıştır. Key Vault ile istemci tarafı şifreleme gerçekleştirirken, tercih edilen modeli Key Vault gizli diziler olarak depolanan ve önbelleğe simetrik ana anahtarları yerel olarak kullanmaktır. Kullanıcılar aşağıdakileri yapmanız gerekir:

1. Çevrimdışı bir gizli dizi oluşturma ve anahtar Kasası'na yükleyin.
2. Parolanın temel tanımlayıcısı, şifreleme için gizli anahtarı'nın geçerli sürümü çözümlemek ve bu bilgileri yerel olarak önbelleğe için parametre olarak kullanın. CachingKeyResolver önbelleğe almak için kullanın. kullanıcılara uygulamak için kendi mantığını önbelleğe alma beklenmiyor.
3. Önbelleğe alma çözme, şifreleme ilkesi oluşturulurken girdi olarak kullanın.

Key Vault kullanımıyla ilgili daha fazla bilgi bulunabilir [şifreleme kod örnekleri](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples).

## <a name="best-practices"></a>En iyi uygulamalar
Şifreleme desteği, yalnızca .NET depolama istemci Kitaplığı'nda kullanılabilir. Şu anda Windows Phone ve Windows çalışma zamanı şifrelemeyi desteklemez.

> [!IMPORTANT]
> İstemci tarafı şifreleme kullanırken şu önemli noktalara dikkat edin:
> 
> * Şifrelenmiş bir blobu yazma veya okuma, tüm blob karşıya yükleme komutlarını ve aralığı/tüm blob yükleme komutlarını kullanın. Blok yerleştirme, Put Engellenenler listesi, yazma sayfaları, NET sayfa veya blok ekleme gibi işlemleri protokolü kullanılarak şifrelenmiş bir blobu yazma kaçının; Aksi halde şifrelenmiş bir blobu bozuk ve okunamaz hale.
> * Tablolar için benzer bir kısıtlama yok. Şifrelenmiş özellikler şifreleme meta verileri güncelleştirmeden güncelleştiremezsiniz dikkat edin.
> * Meta veriler üzerinde şifrelenmiş bir blobu ayarlarsanız, şifre çözme için meta verileri ayarlama eklenebilir olmadığından gerekli şifrelemeyle ilgili meta veriler üzerine yazabilir. Bu da anlık görüntüler için geçerlidir; meta veri şifrelenmiş bir blobun anlık görüntüsünü oluşturulurken belirtmekten kaçının. Meta veriler ayarlanmalıdır, çağırdığınızdan emin olun **FetchAttributes** ilk geçerli şifreleme meta verileri ve meta verileri ayarlanırken eşzamanlı yazma sorunu önlemek için yöntemi.
> * Etkinleştirme **RequireEncryption** yalnızca çalışması kullanıcılar için varsayılan istek seçenekleri özelliğinde şifrelenmiş veriler. Aşağıda daha fazla bilgi için bkz.
> 
> 

## <a name="client-api--interface"></a>İstemci API'si / arabirimi
EncryptionPolicy nesne oluşturma sırasında kullanıcılara (IKey uygulama) yalnızca bir anahtar sağlayabilir Çözümleyicisi (IKeyResolver uygulama) veya her ikisini de. IKey anahtar tanımlayıcısı kullanılarak tanımlanır ve sarmalama/çözülme için mantığı sağlayan temel anahtar türü değil. IKeyResolver, bir anahtar şifre çözme işlemi sırasında çözmek için kullanılır. Anahtar tanımlayıcısını verilen bir ikey değerini döndüren bir ResolveKey yöntemi tanımlar. Bu kullanıcılar birden fazla konumda yönetilen birden çok anahtar arasında seçim yapma olanağı sağlar.

* Şifreleme için anahtar her zaman kullanılır ve bir anahtar olmaması bir hataya neden olur.
* Şifre çözme için:
  * Anahtar çözümleyici belirtilmişse anahtarını almak için çağrılır. Çözümleyici belirtildi, ancak anahtar tanımlayıcısı için bir eşleme yok. bir hata oluşturulur.
  * Tanımlayıcısını gerekli anahtar tanımlayıcısı eşleşiyorsa çözümleyici belirtilmedi, ancak belirtilen bir anahtarı anahtar kullanılır. Tanımlayıcı eşleşmiyorsa, bir hata oluşturulur.

Bu makaledeki kod örnekleri, bir şifreleme ilkesi ayarlama ve şifrelenmiş verileri ile çalışma, ancak Azure anahtar kasası ile çalışmaya göstermemiz gerekmez. [Şifreleme örnekleri](https://github.com/Azure/azure-storage-net/tree/master/Samples/GettingStarted/EncryptionSamples) GitHub üzerinde daha ayrıntılı bir uçtan uca senaryo bloblar, kuyruklar ve tablolar için Key Vault tümleştirmesiyle bunların gösterir.

### <a name="requireencryption-mode"></a>RequireEncryption modu
Kullanıcılar, isteğe bağlı olarak burada tüm karşıya yüklemelerden ve şifrelenmelidir işlemi modu etkinleştirebilirsiniz. Bu modda, istemcide bir şifreleme ilkesi olmadan veri yükleme veya hizmette şifrelenmez verileri indirmek için girişimleri başarısız olur. **RequireEncryption** istek seçenekleri nesnenin özellik, bu davranışı denetler. Uygulamanızı Azure Depolama'da depolanan tüm nesneleri şifreler sonra ayarlayabileceğiniz **RequireEncryption** hizmet istemci nesnesi için varsayılan istek seçenekleri özelliği. Örneğin, **CloudBlobClient.DefaultRequestOptions.RequireEncryption** için **true** tüm blob, istemci nesnesi üzerinden gerçekleştirilen işlemleri için şifrelemeyi gerekli kılmak üzere.


### <a name="blob-service-encryption"></a>BLOB hizmeti şifreleme
Oluşturma bir **BlobEncryptionPolicy** nesne ve istek seçenekleri ayarlayın (API başına veya kullanarak, bir istemci düzeyinde **DefaultRequestOptions**). Diğer her şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 BlobEncryptionPolicy policy = new BlobEncryptionPolicy(key, null);

 // Set the encryption policy on the request options.
 BlobRequestOptions options = new BlobRequestOptions() { EncryptionPolicy = policy };

 // Upload the encrypted contents to the blob.
 blob.UploadFromStream(stream, size, null, options, null);

 // Download and decrypt the encrypted contents from the blob.
 MemoryStream outputStream = new MemoryStream();
 blob.DownloadToStream(outputStream, null, options, null);
```

### <a name="queue-service-encryption"></a>Kuyruk hizmeti şifreleme
Oluşturma bir **QueueEncryptionPolicy** nesne ve istek seçenekleri ayarlayın (API başına veya kullanarak, bir istemci düzeyinde **DefaultRequestOptions**). Diğer her şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 QueueEncryptionPolicy policy = new QueueEncryptionPolicy(key, null);

 // Add message
 QueueRequestOptions options = new QueueRequestOptions() { EncryptionPolicy = policy };
 queue.AddMessage(message, null, null, options, null);

 // Retrieve message
 CloudQueueMessage retrMessage = queue.GetMessage(null, options, null);
```

### <a name="table-service-encryption"></a>Tablo hizmeti şifreleme
Bir şifreleme ilkesi oluşturma ve istek seçeneklerini ayarlama ek olarak, ya da belirtmelisiniz bir **EncryptionResolver** içinde **TableRequestOptions**, ya da [EncryptProperty] özniteliği ayarlayın varlık.

#### <a name="using-the-resolver"></a>Çözümleyici kullanma

```csharp
// Create the IKey used for encryption.
 RsaKey key = new RsaKey("private:key1" /* key identifier */);

 // Create the encryption policy to be used for upload and download.
 TableEncryptionPolicy policy = new TableEncryptionPolicy(key, null);

 TableRequestOptions options = new TableRequestOptions()
 {
    EncryptionResolver = (pk, rk, propName) =>
     {
        if (propName == "foo")
         {
            return true;
         }
         return false;
     },
     EncryptionPolicy = policy
 };

 // Insert Entity
 currentTable.Execute(TableOperation.Insert(ent), options, null);

 // Retrieve Entity
 // No need to specify an encryption resolver for retrieve
 TableRequestOptions retrieveOptions = new TableRequestOptions()
 {
    EncryptionPolicy = policy
 };

 TableOperation operation = TableOperation.Retrieve(ent.PartitionKey, ent.RowKey);
 TableResult result = currentTable.Execute(operation, retrieveOptions, null);
```

#### <a name="using-attributes"></a>Öznitelikleri kullanma
Varlık TableEntity uyguluyorsa, yukarıda belirtildiği gibi ardından özelliklerini belirtmek yerine [EncryptProperty] özniteliği ile tasarlanabilir **EncryptionResolver**.

```csharp
[EncryptProperty]
 public string EncryptedProperty1 { get; set; }
```

## <a name="encryption-and-performance"></a>Şifreleme ve performans
Depolama veri sonuçlarınızı ek performans yükünden şifreleme unutmayın. İçerik anahtar ve IV oluşturulmalıdır, içeriği şifrelenir ve ek meta verileri biçimlendirilmiş karşıya ve. Bu ek yükü şifrelenen veri miktarı bağlı olarak değişir. Müşterilerin uygulamalarını geliştirme sırasında performans için her zaman test etmenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar
* [Öğretici: Şifreleme ve şifre çözme Azure anahtar Kasası'nı kullanarak Microsoft Azure depolama BLOB'ları](../blobs/storage-encrypt-decrypt-blobs-key-vault.md)
* İndirme [.NET NuGet paketi için Azure depolama istemci kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage)
* Azure anahtar kasası Nuget'i indir [çekirdek](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Core/), [istemci](https://www.nuget.org/packages/Microsoft.Azure.KeyVault/), ve [uzantıları](https://www.nuget.org/packages/Microsoft.Azure.KeyVault.Extensions/) paketleri  
* Ziyaret [Azure anahtar kasası belgeleri](../../key-vault/key-vault-whatis.md)
