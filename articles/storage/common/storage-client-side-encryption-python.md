---
title: Microsoft Azure depolama için istemci tarafı şifreleme Python ile | Microsoft Docs
description: Python için Azure Storage istemci kitaplığı, Azure Storage uygulamalarınız için en yüksek güvenlik için istemci tarafı Şifreleme destekler.
services: storage
documentationcenter: python
author: lakasa
manager: jahogg
editor: tysonn
ms.assetid: f9bf7981-9948-4f83-8931-b15679a09b8a
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/11/2017
ms.author: lakasa
ms.openlocfilehash: c925b41d1654bd5c9b40438c4b6b9f402ec4bac2
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2018
ms.locfileid: "29742651"
---
# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Microsoft Azure depolama için istemci tarafı şifreleme Python ile
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Genel Bakış
[Python için Azure Storage istemci Kitaplığı](https://pypi.python.org/pypi/azure-storage) Azure depolama alanına yüklemek ve istemciye indirirken verilerin şifresini çözmek önce istemci uygulamalar içinde verilerin şifrelenmesi destekler.

> [!NOTE]
> Azure depolama Python kitaplığı önizlemede değil.
> 
> 

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Şifreleme ve şifre çözme Zarf teknik aracılığıyla
Şifreleme ve şifre çözme işlemleri Zarf teknik izleyin.

### <a name="encryption-via-the-envelope-technique"></a>Zarf teknik aracılığıyla şifreleme
Zarf teknik aracılığıyla şifreleme aşağıdaki şekilde çalışır:

1. Azure storage istemci kitaplığı simetrik anahtar bir kerelik kullan bir içerik şifreleme anahtarı (CEK) oluşturur.
2. Kullanıcı verileri bu CEK kullanılarak şifrelenir.
3. CEK (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) paketlenir. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti ya da yerel olarak yönetilen bir simetrik anahtar olabilir.
   Depolama istemcisi kitaplığı kendisini KEK hiçbir zaman erişebilir. Kitaplık KEK tarafından sağlanan anahtar kaydırma algoritması çağırır. Kullanıcıların özel sağlayıcıları anahtar kaydırma/açmak için isterseniz kullanmayı da seçebilirsiniz.
4. Şifrelenmiş veriler daha sonra Azure Storage hizmetine yüklenir. Bazı ek şifreleme meta verilerle birlikte Sarmalanan anahtarın meta veriler (hakkında bir blob) olarak depolanır veya şifrelenmiş verilerle (iletileri kuyruğa ve tablo varlıkları) değiştirilmiş.

### <a name="decryption-via-the-envelope-technique"></a>Zarf teknik aracılığıyla şifre çözme
Şifre çözme Zarf teknik aracılığıyla aşağıdaki şekilde çalışır:

1. Kullanıcı anahtar şifreleme anahtarı (KEK) yerel olarak yönetiyor istemci kitaplığı varsayar. Kullanıcı, şifreleme için kullanılan özel anahtarını bilmesi gerekmez. Bunun yerine, farklı anahtar tanımlayıcıları anahtarları çözümler, bir anahtar çözümleyici ayarlayabilir ve kullanılır.
2. İstemci Kitaplığı hizmette depolanan tüm şifreleme malzeme birlikte şifrelenmiş verileri yükler.
3. Sarmalanmamış (şifresi) anahtarı şifreleme kullanarak anahtarı (KEK) Sarmalanan içerik şifreleme anahtarı (CEK) olduğundan. Burada yeniden istemci kitaplığı KEK erişimi yok. Yalnızca özel sağlayıcının açma algoritması da çağırır.
4. İçerik şifreleme anahtarı (CEK), ardından şifrelenmiş kullanıcı verileri şifrelemek için kullanılır.

## <a name="encryption-mechanism"></a>Şifreleme mekanizması
Depolama istemcisi kitaplığı kullanır [AES](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) kullanıcı verilerini şifrelemek için. Özellikle, [Şifre blok zincirleme (CBC)](http://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) AES moduyla. Her bir hizmet works biraz farklı şekilde Burada bunların her birini aşağıdakiler ele alınacaktır.

### <a name="blobs"></a>Bloblar
İstemci Kitaplığı şu anda yalnızca tüm blobları şifreleme desteklemektedir. Kullanıcıların kullandığınızda Şifreleme özellikle, desteklenen **oluşturma*** yöntemleri. Yüklemeleri, hem tam hem aralık yüklemeleri desteklenir ve her iki paralelleştirme karşıya yükleme ve indirme için kullanılabilir.

Şifreleme sırasında istemci kitaplığının rastgele başlatma vektörü (IV), 32 bayt rasgele bir içerik şifreleme anahtarı (CEK) ile birlikte 16 bayt oluşturmak ve bu bilgileri kullanarak blob verisi Zarf şifreleme gerçekleştirin. Sarmalanan CEK ve bazı ek şifreleme meta veriler şifrelenmiş bir blobu hizmetinin yanı sıra meta verileri blob olarak depolanır.

> [!WARNING]
> Düzenleme veya kendi meta veriler blob'a karşıya yükleme, bu meta veriler korunduğundan emin olmak gerekir. Bu meta veriler olmadan yeni meta veriler karşıya yükleme, Sarmalanan CEK, IV ve diğer meta veriler kaybolur ve blob içeriğinin hiçbir zaman tekrar alınabilir.
> 
> 

Şifrelenmiş bir blobu indirme içerir kullanarak tüm blob içerik alma **almak*** kullanışlı yöntemler. Sarmalanan CEK sarılmamış ve kullanıcılara şifresi çözülmüş veriler döndürmek için (Bu durumda blob meta verileri depolanır) IV ile birlikte kullanılır.

Rastgele bir aralığı indirme (**almak*** aralığı parametreleri yöntemleriyle geçirilen) çok küçük miktarda başarıyla İstenen aralık şifresini çözmek için kullanılan ek veri alabilmek için kullanıcılar tarafından sağlanan aralığı ayarlama şifrelenmiş bir blobu içerir.

Blok blobları ve sayfa BLOB'ları yalnızca şifrelenmiş şifresi/bu şeması kullanarak olabilir. Aynı zamanda şifreleme eklemek için BLOB'ları şu anda desteği yoktur.

### <a name="queues"></a>Kuyruklar
İletileri kuyruğa herhangi biçimi olabileceği için istemci kitaplığının başlatma vektörü (IV) ve şifrelenmiş içerik şifreleme anahtarı (CEK) ileti metnini içeren özel bir biçim tanımlar.

Şifreleme sırasında istemci kitaplığı 16 bayt rastgele CEK 32 bayt yanı sıra rasgele bir IV oluşturur ve bu bilgileri kullanarak sıraya ileti metninin Zarf şifreleme gerçekleştirir. Sarmalanan CEK ve bazı ek şifreleme meta veriler şifrelenmiş kuyruk iletisini eklenir. (Aşağıda gösterilen) bu değiştirilmiş ileti hizmette depolanır.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

Şifre çözme sırasında Sarmalanan anahtar sırası iletiden ayıklanır ve sarılmamış. IV ayrıca kuyruk iletiden ayıklanan ve kuyruk iletisi verilerin şifresini çözmek için sarmalanmamış anahtarı ile birlikte kullanılan. Bir kuyruk iletisi 64 KB sınırını doğru sayım sırasında etkisi yönetilebilir olması şifreleme meta verileri (altında 500 bayt), küçük olduğunu unutmayın.

### <a name="tables"></a>Tablolar
İstemci Kitaplığı varlık özellikleri şifrelenmesi için INSERT destekler ve değiştirme işlemlerini.

> [!NOTE]
> Birleştirme şu anda desteklenmiyor. Bir alt özellikler kümesini daha önce farklı bir anahtar kullanılarak şifrelenmiş olabilecek olduğundan, sadece yeni özellikleri birleştirme ve meta verilerini güncelleştirme veri kaybına neden olur. Ya da birleştirme önceden var olan varlık hizmetinden okumak için fazladan hizmeti çağrıları yapma gerektirir veya yeni bir anahtar özellik başına kullanarak, her ikisi de performansı artırmak için uygun değildir.
> 
> 

Tablo verileri şifreleme gibi çalışır:

1. Kullanıcıları şifrelenmesi için özellikleri belirtin.
2. İstemci Kitaplığı rastgele başlatma vektörü (IV) yanı sıra rasgele içerik bir şifreleme anahtarı (CEK) 32 bayt her varlık için 16 bayt oluşturur ve yeni bir IV özellik başına türetme tarafından şifrelenmesi için ayrı ayrı özellikler Zarf şifreleme gerçekleştirir. Şifrelenmiş özelliği ikili veri olarak depolanır.
3. Sarmalanan CEK ve bazı ek şifreleme meta verileri iki ek ayrılmış özellikleri olarak depolanır. İlk ayrılmış özelliği (\_ClientEncryptionMetadata1) IV, sürüm ve Sarmalanan anahtar hakkında bilgi içeren bir dize özelliğidir. İkinci ayrılmış özelliği (\_ClientEncryptionMetadata2) şifrelenmiş özellikler hakkında bilgi içeren bir ikili özelliğidir. Bu ikinci özellik bilgileri (\_ClientEncryptionMetadata2) kendisi şifrelenmiş olduğunu.
4. Şifreleme için gereken bu ek ayrılmış özellikleri nedeniyle, kullanıcılar artık 252 yerine yalnızca 250 özel özelliklere sahip olabilir. Varlık toplam boyutu 1 MB'tan az olması gerekir.
   
   Yalnızca dize özellikleri şifrelenmiş olduğunu unutmayın. Diğer özelliklerin türleri, şifreli olarak varsa, bunlar dizelere dönüştürülmesi gerekir. Şifrelenmiş dizelerin hizmette ikili özellikleri olarak depolanır ve bunlar geri dizeleri (ham dizeleri, EntityProperties EdmType.STRING türüyle değil) için şifre çözme sonra dönüştürülür.
   
   Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenecek özelliklerini belirtmeniz gerekir. Bu da bu özellikleri EdmType.STRING türü kümesine TableEntity nesneleriyle depolayarak yapılabilir ve true olarak ayarlandığında veya tableservice nesnesinde encryption_resolver_function ayarı şifreleyebilirsiniz. Bir şifreleme çözümleyici bölüm anahtarı, satır anahtarını ve özellik adını alır ve bu özellik şifrelenmesi gerekip gerekmediğini belirten bir Boole değeri döndüren bir işlevdir. Şifreleme sırasında istemci kitaplığı, bir özellik için kablo yazılırken şifrelenmesi gerekip gerekmediğine karar vermek için bu bilgileri kullanır. Temsilci özellikler nasıl şifrelenir geçici mantığı olasılığı için de sağlar. (Örneğin, X ise ardından özelliği A şifrelemek; Aksi takdirde özellikleri A ve b şifreleme) Bu bilgileri okurken veya varlıkları sorgulamak için gerekli olmadığını göz önünde bulundurun.

### <a name="batch-operations"></a>Toplu işlemleri
Toplu işteki tüm satırlar için bir şifreleme ilkesi uygulanır. İstemci Kitaplığı dahili olarak toplu işleme yeni rastgele IV ve satır başına rastgele CEK oluşturur. Kullanıcılar, şifreleme Çözümleyicisi Bu davranış tanımlayarak toplu işlemdeki her işlem için farklı özellikleri şifrelemek de seçebilirsiniz.
Bir toplu bir bağlam Yöneticisi olarak tableservice batch() yöntemle oluşturduysanız, tableservice's şifreleme ilkesi toplu otomatik olarak uygulanır. Bir toplu açıkça Oluşturucu çağırarak oluşturulursa, şifreleme ilkesi parametre olarak geçirilen ve sol toplu ömrü boyunca değiştirilmemiş.
Toplu iş şifreleme ilkesi (varlıklar tableservice's Şifreleme İlkesi'ni kullanarak toplu iş yürütme sırasında şifrelenmez) kullanarak toplu içine eklenen gibi varlıkları şifrelenmesini unutmayın.

### <a name="queries"></a>Sorgular
> [!NOTE]
> Varlıkları şifrelendiği, filtre sorgularını bir şifrelenmiş özellikte çalıştırılamıyor.  Denerseniz, şifrelenmiş veriler şifrelenmemiş verilerle karşılaştırmak hizmet çalışırken çünkü sonuçlar hatalı olacaktır.
> 
>
Sorgu işlemleri gerçekleştirmek için sonuç kümesindeki tüm anahtarları çözümleyebildiğini anahtar bir çözümleyici belirtmeniz gerekir. Sorgu sonucunda bulunan bir varlık için bir sağlayıcı çözümlenemezse, istemci kitaplığının bir hata durum oluşturur. Sunucu tarafı tahminleri gerçekleştirir herhangi bir sorgu için istemci kitaplığının özel şifreleme meta veri özelliklerini ekler (\_ClientEncryptionMetadata1 ve \_ClientEncryptionMetadata2) varsayılan olarak seçili sütun.

> [!IMPORTANT]
> İstemci tarafı şifreleme kullanırken bu önemli noktalara dikkat edin:
> 
> * Okuma veya yazma şifrelenmiş bir blobu, tüm blob karşıya yükleme komutlarını ve aralığı/bütün blob yükleme komutlarını kullanın. Put bloğu, Put engelleme listesi, yazma sayfaları veya Temizle sayfalar gibi Protokolü işlemleri kullanılarak şifrelenmiş bir blobu yazma kaçının; Aksi takdirde şifrelenmiş bir blobu bozuk ve okunamaz olun.
> * Tablolar için benzer bir kısıtlama var. Şifrelenmiş özellikler şifreleme meta verisini güncelleştirmeden yükseltmemeye dikkatli olun.
> * Meta veriler üzerinde şifrelenmiş bir blobu ayarlarsanız, meta verileri ayarlama eklenebilir olmadığından şifre çözme için gerekli şifrelemeyle ilgili meta veriler üzerine yazabilir. Bu da anlık görüntüler için geçerlidir; meta veri anlık görüntüsünü şifrelenmiş bir blobu oluşturulurken belirtmekten kaçının. Meta veriler ayarlanmalıdır, çağırdığınızdan emin olun **get_blob_metadata** önce geçerli şifreleme meta verilerini almak ve meta veri ayarlanırken eşzamanlı yazma önlemek için yöntem.
> * Etkinleştirme **require_encryption** bayrağı yalnızca şifrelenmiş verileri ile çalışması gereken kullanıcılar için hizmet nesnesi. Aşağıda daha fazla bilgi için bkz.
> 
> 

Depolama istemcisi kitaplığı sağlanan KEK ve anahtar Çözümleyicisi aşağıdaki arabirimi uygulamak için bekliyor. [Azure anahtar kasası](https://azure.microsoft.com/services/key-vault/) Python KEK Yönetimi Beklemede ve tamamlandığında bu kitaplığa tümleşik desteği.

## <a name="client-api--interface"></a>İstemci API / arabirim
Bir depolama birimi hizmeti (yani blockblobservice) oluşturulduktan sonra kullanıcı bir şifreleme ilkesi oluşturan alanları değerleri atayabilirsiniz: key_encryption_key, key_resolver_function ve require_encryption. Kullanıcılar, yalnızca bir KEK sağlayabilir yalnızca Çözümleyici veya her ikisi de. key_encryption_key anahtar tanımlayıcısını kullanarak tanımlanır ve kaydırma/açmak için mantığı sağlayan temel anahtar türü değil. key_resolver_function bir anahtar şifre çözme işlemi sırasında çözmek için kullanılır. Anahtar tanımlayıcısını verilen geçerli bir KEK döndürür. Bu kullanıcılar birden fazla konumda yönetilen birden çok anahtar arasında seçim yapma olanağı sağlar.

KEK başarıyla verileri şifrelemek için aşağıdaki yöntemleri uygulamanız gerekir:

* wrap_key(cek): kullanıcının tercih ettiğiniz bir algoritma kullanarak belirtilen CEK (bayt) sarmalar. Sarmalanan anahtar döndürür.
* get_key_wrap_algorithm(): Anahtarları sarmalamak için kullanılan algoritma döndürür.
* get_kid(): Returns the string key id for this KEK.
  KEK başarıyla verilerin şifresini çözmek için aşağıdaki yöntemleri uygulamanız gerekir:
* unwrap_key (cek, algoritma): dize belirtilen algoritmasını kullanarak belirtilen CEK sarmalanmamış biçiminde döndürür.
* get_kid(): Returns a string key id for this KEK.

Anahtar çözümleyici en az, anahtar kimliği verilen yukarıdaki arabirimini uygulayan karşılık gelen KEK döndüren bir yöntem uygulamalıdır. Yalnızca bu hizmeti nesnesi key_resolver_function özelliğine atanacak bir yöntemdir.

* Şifreleme için bir anahtarın olmaması bir hataya neden olur ve anahtarı her zaman kullanılır.
* Şifre çözme için:
  
  * Anahtar çözümleyici anahtarını almak için belirtilmişse çağrılır. Çözümleyici belirtildi, ancak anahtar tanımlayıcısı için bir eşleme yok, bir hata oluşturulur.
  * Gerekli anahtar tanımlayıcısı tanıtıcısını eşleşmesi durumunda çözümleyici belirtilmedi, ancak belirtilen bir anahtar, anahtar kullanılır. Tanımlayıcı eşleşmiyorsa, bir hata oluşturulur.
    
    Azure.storage.samples şifreleme örneklerinde <fix URL>BLOB, kuyruklar ve tablolar için daha ayrıntılı bir uçtan uca senaryoyu göstermektedir.
      Örnek uygulamaları KEK ve anahtar çözümleyici örnek dosyalarında sırasıyla KeyWrapper ve KeyResolver sağlanır.

### <a name="requireencryption-mode"></a>RequireEncryption modu
Kullanıcıların isteğe bağlı olarak bir çalışma modu, burada tüm ve indirmelere şifrelenmelidir sağlayabilirsiniz. Bu modda, istemcide bir şifreleme ilkesi olmadan verileri karşıya yükleme veya hizmette şifrelenmez veri indirme girişimleri başarısız olur. **Require_encryption** bayrağı hizmet nesnesi üzerinde bu davranışı denetler.

### <a name="blob-service-encryption"></a>BLOB hizmeti şifreleme
Şifreleme İlkesi alanlarında blockblobservice nesne üzerinde ayarlayın. Şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set the KEK and key resolver on the service object.
my_block_blob_service.key_encryption_key = kek
my_block_blob_service.key_resolver_funcion = key_resolver.resolve_key

# Upload the encrypted contents to the blob.
my_block_blob_service.create_blob_from_stream(container_name, blob_name, stream)

# Download and decrypt the encrypted contents from the blob.
blob = my_block_blob_service.get_blob_to_bytes(container_name, blob_name)
```

### <a name="queue-service-encryption"></a>Kuyruk hizmeti şifreleme
Şifreleme İlkesi alanlarında queueservice nesne üzerinde ayarlayın. Şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Set the KEK and key resolver on the service object.
my_queue_service.key_encryption_key = kek
my_queue_service.key_resolver_funcion = key_resolver.resolve_key

# Add message
my_queue_service.put_message(queue_name, content)

# Retrieve message
retrieved_message_list = my_queue_service.get_messages(queue_name)
```

### <a name="table-service-encryption"></a>Tablo hizmeti şifreleme
Bir şifreleme ilkesi oluşturma ve istek seçeneklerini ayarlama ek olarak, ya da belirtmeniz gerekir bir **encryption_resolver_function** üzerinde **tableservice**, veya üzerinde EntityProperty şifrele özniteliğini ayarlayın.

### <a name="using-the-resolver"></a>Çözücü kullanma

```python
# Create the KEK used for encryption.
# KeyWrapper is the provided sample implementation, but the user may use their own object as long as it implements the interface above.
kek = KeyWrapper('local:key1') # Key identifier

# Create the key resolver used for decryption.
# KeyResolver is the provided sample implementation, but the user may use whatever implementation they choose so long as the function set on the service object behaves appropriately.
key_resolver = KeyResolver()
key_resolver.put_key(kek)

# Define the encryption resolver_function.
def my_encryption_resolver(pk, rk, property_name):
    if property_name == 'foo':
        return True
    return False

# Set the KEK and key resolver on the service object.
my_table_service.key_encryption_key = kek
my_table_service.key_resolver_funcion = key_resolver.resolve_key
my_table_service.encryption_resolver_function = my_encryption_resolver

# Insert Entity
my_table_service.insert_entity(table_name, entity)

# Retrieve Entity
# Note: No need to specify an encryption resolver for retrieve, but it is harmless to leave the property set.
my_table_service.get_entity(table_name, entity['PartitionKey'], entity['RowKey'])
```

### <a name="using-attributes"></a>Öznitelikleri kullanma
Yukarıda belirtildiği gibi bir özelliği bir EntityProperty nesnesinde depolamak ve şifrele alanını ayarlamak için şifreleme işaretlenebilir.

```python
encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)
```

## <a name="encryption-and-performance"></a>Şifreleme ve performans
Depolama veri sonuçlarınızda ek performans yükü şifreleme unutmayın. IV ve içerik anahtarı oluşturulmuş olması gerekir, içeriği şifrelenmelidir ve ek meta veri biçimlendirilmiş ve karşıya gerekir. Bu ek yükü Şifrelenmekte veri miktarı bağlı olarak değişir. Müşterilerin kendi uygulamalarında geliştirme sırasında performansı için her zaman sınamanızı öneririz.

## <a name="next-steps"></a>Sonraki adımlar
* Karşıdan [Java Pypı paket için Azure Storage istemci kitaplığı](https://pypi.python.org/pypi/azure-storage)
* Karşıdan [Python için Azure Storage istemci kitaplığı kaynak kodu github'dan](https://github.com/Azure/azure-storage-python)
