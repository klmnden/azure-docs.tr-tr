---
title: Microsoft Azure depolama için Python ile istemci tarafı şifreleme | Microsoft Docs
description: Python için Azure depolama istemci kitaplığı, Azure depolama uygulamalarınız için en yüksek güvenlik için istemci tarafı Şifreleme destekler.
services: storage
author: tamram
ms.service: storage
ms.devlang: python
ms.topic: article
ms.date: 05/11/2017
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: d04c1e137a190b01554106c041853aa2fd6786d7
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65146907"
---
# <a name="client-side-encryption-with-python-for-microsoft-azure-storage"></a>Microsoft Azure depolama istemci tarafı şifreleme ile Python
[!INCLUDE [storage-selector-client-side-encryption-include](../../../includes/storage-selector-client-side-encryption-include.md)]

## <a name="overview"></a>Genel Bakış
[Python için Azure depolama istemci Kitaplığı](https://pypi.python.org/pypi/azure-storage) Azure Storage'a yüklemeden ve istemciye indirirken verilerin şifresini çözme önce istemci uygulamalar içinde verilerin şifrelenmesini destekler.

> [!NOTE]
> Azure depolama Python kitaplığı Önizleme aşamasındadır.
> 
> 

## <a name="encryption-and-decryption-via-the-envelope-technique"></a>Şifreleme ve şifre çözme aracılığıyla Zarf yöntemi
Şifreleme ve şifre çözme işlemleri için zarf teknik izleyin.

### <a name="encryption-via-the-envelope-technique"></a>Şifreleme aracılığıyla Zarf yöntemi
Şifreleme aracılığıyla Zarf tekniği aşağıdaki şekilde çalışır:

1. Azure depolama istemci kitaplığı, bir kerelik kullanım simetrik anahtarı olan bir içerik şifreleme anahtarı (CEK) oluşturur.
2. Kullanıcı verileri, bu CEK kullanılarak şifrelenir.
3. CEK (anahtar şifreleme anahtarı (KEK) kullanılarak şifrelenir) paketlenir. KEK anahtar bir tanımlayıcıyla tanımlanır ve asimetrik anahtar çifti ya da yerel olarak yönetilen bir simetrik anahtar.
   Depolama istemcisi kitaplığı kendisini KEK hiçbir zaman erişebilir. Kitaplık KEK tarafından sağlanan anahtar sarmalama algoritması çağırır. Kullanıcıların özel sağlayıcıları anahtar sarmalama/çözülme için isterseniz kullanmayı da tercih edebilirsiniz.
4. Şifrelenmiş veriler, ardından Azure depolama hizmetine yüklenir. Bazı ek şifreleme meta verilerle birlikte Sarmalanan anahtar meta verileri (temel, bir blob) olarak depolanan veya şifrelenmiş veriler (kuyruk iletileri ve tablo varlıkları) ile ilişkilendirilmiş.

### <a name="decryption-via-the-envelope-technique"></a>Zarf teknik aracılığıyla şifre çözme
Şifre çözme aracılığıyla Zarf tekniği aşağıdaki şekilde çalışır:

1. İstemci Kitaplığı, kullanıcı anahtar şifreleme anahtarı (KEK) yerel olarak yönetildiğinden emin varsayar. Kullanıcı, şifreleme için kullanılan özel anahtarı bilmeniz gerekmez. Bunun yerine, farklı anahtar tanımlayıcıları anahtarları için çözümler, önemli bir çözümleyici ayarlayabilir ve kullanılır.
2. İstemci Kitaplığı, şifrelenmiş veriler service üzerinde depolanan herhangi bir şifreleme materyalin birlikte yükler.
3. Sarmalanmış halden (şifresi) anahtar şifrelemesi kullanarak anahtarı (KEK) daha sonra (CEK) Sarmalanan içerik şifreleme anahtarı var. Burada yine, istemci kitaplığı KEK erişimi yok. Yalnızca özel sağlayıcının açma algoritması da çağırır.
4. İçerik şifreleme anahtarı (CEK), ardından şifreli kullanıcı verilerin şifresini çözmek için kullanılır.

## <a name="encryption-mechanism"></a>Şifreleme mekanizması
Depolama istemcisi kitaplığı kullanan [AES](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) kullanıcı verilerini şifrelemek için. Özellikle, [Şifre blok zincirleme (CBC)](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation#Cipher-block_chaining_.28CBC.29) AES moduyla. Her bir hizmet works biraz farklı şekilde, her biri aşağıda ele alınacaktır.

### <a name="blobs"></a>Bloblar
İstemci Kitaplığı, şu anda yalnızca tüm blobları şifreleme desteklemektedir. Özellikle, kullanıcıların kullanması durumunda şifreleme desteklenir **oluşturma*** yöntemleri. İndirmeler, hem tam hem aralığı indirmeler desteklenir ve paralelleştirme hem karşıya yükleme ve indirme için kullanılabilir.

Şifreleme sırasında istemci kitaplığı rastgele başlatma vektörü (IV) rasgele bir içerik şifreleme anahtarı (CEK) 32 bayt ile birlikte 16 bayt oluşturmak ve bu bilgileri kullanarak blob verisi şifreleme Zarf gerçekleştirin. Sarmalanan CEK ve bazı ek şifreleme meta verileri blob hizmetinde şifrelenmiş bir blobu yanı sıra meta veri olarak depolanır.

> [!WARNING]
> Düzenleme veya kendi blob meta verilerini karşıya yükleme, bu meta veriler korunduğundan emin olmak gerekir. Bu meta veriler olmadan yeni meta verilerini karşıya yükleme, sarmalanmış CEK, IV ve diğer meta veriler kaybolur ve blob içeriği hiçbir zaman tekrar alınabilir olmaz.
> 
> 

Şifrelenmiş bir blobu indirme içeren tüm blob kullanarak içerik alma **alma*** kullanışlı yöntemler. Sarmalanan CEK sarmalanmamış ve şifresi çözülmüş veriler kullanıcılara döndürmek için (Bu durumda blob meta veri olarak depolanır) IV ile birlikte kullanılır.

Bir rastgele aralık indiriliyor (**alma*** aralığı parametrelere sahip yöntemleri geçirilen) az miktarda bir için başarıyla kullanılabilir ek veri alabilmek için kullanıcı tarafından sağlanan aralık ayarlamak şifrelenmiş bir blobu içerir. İstenen bir aralıkla şifresini çözemez.

Blok blobları ve sayfa blobları yalnızca şifrelenmiş şifresi/bu düzeni kullanarak olabilir. Aynı zamanda blobları şifreleme eklemek için şu anda desteği yoktur.

### <a name="queues"></a>Kuyruklar
Kuyruk iletileri herhangi biçimi olamayacağından, istemci kitaplığının metinde başlatma vektörü (IV) ve şifrelenmiş içerik şifreleme anahtarı (CEK) içeren özel bir biçim tanımlar.

Şifreleme sırasında istemci kitaplığı, rastgele bir CEK 32 bayt ile birlikte 16 bayt rastgele bir IV oluşturur ve bu bilgileri kullanarak sıraya ileti metninin Zarf şifreleme gerçekleştirir. Ardından Sarmalanan CEK ve bazı ek şifreleme meta verileri için şifrelenmiş kuyruk iletisi eklenir. (Aşağıda gösterilen) değiştirilmiş bu ileti, hizmette depolanır.

```
<MessageText>{"EncryptedMessageContents":"6kOu8Rq1C3+M1QO4alKLmWthWXSmHV3mEfxBAgP9QGTU++MKn2uPq3t2UjF1DO6w","EncryptionData":{…}}</MessageText>
```

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
3. Sarmalanan CEK ve bazı ek şifreleme meta verileri sonra iki ek ayrılmış özellikleri olarak depolanır. İlk özellik ayrılmış (\_ClientEncryptionMetadata1) IV, sürüm ve Sarmalanan anahtar bilgilerini tutan bir dize özelliği. İkinci özellik ayrılmış (\_ClientEncryptionMetadata2) şifrelenir özellikleri hakkında bilgileri tutan ikili bir özelliktir. Bu ikinci özellik bilgileri (\_ClientEncryptionMetadata2), kendisi şifrelenir.
4. Şifreleme için gereken bu ek ayrılmış özellikleri nedeniyle, kullanıcılar artık 252 yerine yalnızca 250 özel özellikler olabilir. Varlık toplam boyutu 1 MB'tan az olmalıdır.

   Yalnızca dize özellikleri şifrelenmiş olduğunu unutmayın. Diğer özellikler şifrelenmesini türlerindeyse, dizeleri dönüştürülmelidir. Şifrelenmiş dizeleri hizmette ikili özellikleri olarak depolanır ve bunlar geri dizeleri (ham dize, EntityProperties EdmType.STRING türüyle değil) şifre çözme sonra dönüştürülür.

   Şifreleme İlkesi yanı sıra, tablolar için kullanıcıların şifrelenmiş özelliklerini belirtmeniz gerekir. Bu, ya da bu özellikleri TableEntity türü kümesine EdmType.STRING nesneleriyle depolayarak yapılabilir ve true olarak ayarlanmış veya tableservice nesnede encryption_resolver_function ayarlama şifreleme. Bir şifreleme çözümleyici bölüm anahtarını, satır anahtarını ve özellik adını alır ve bu özellik şifrelenmesi gerekip gerekmediğini belirten bir Boole değeri döndüren bir işlevdir. Şifreleme sırasında istemci kitaplığı, bir özellik için kablo yazılırken şifrelenmesi gerekip gerekmediğine karar vermek için bu bilgileri kullanır. Temsilci özellikleri nasıl şifrelenir etrafında mantıksal olasılığı için de sağlar. (X, örneğin, daha sonra özellik A şifrelemek; Aksi takdirde özellik A ve b şifreleme) Bunu okurken veya varlıkları sorgulayarak bu bilgiyi sağlamak gerekli olduğunu unutmayın.

### <a name="batch-operations"></a>Toplu işlemler
Bir şifreleme ilkesi toplu işteki tüm satırlar için geçerlidir. İstemci Kitaplığı dahili olarak yeni rastgele IV ve rastgele CEK her satır toplu işleme oluşturur. Kullanıcılar bu davranışı şifreleme Çözümleyicisi tanımlayarak toplu işlemdeki her işlem için farklı özellikleri şifrelemek de seçebilirsiniz.
Bir batch tableservice batch() yöntemi aracılığıyla bir bağlam Yöneticisi olarak oluşturulursa, toplu tableservice'nın şifreleme ilkesi otomatik olarak uygulanır. Bir batch açıkça bir oluşturucu çağırarak oluşturulursa, şifreleme ilkesi parametre olarak geçen ve toplu işlem ömrü boyunca sol değiştirilmemiş.
Toplu işin şifreleme ilkesi (varlıklar tableservice'nın Şifreleme İlkesi'ni kullanarak toplu işleme sırasında şifrelenmez) kullanarak batch içine eklenen olarak varlıkları şifrelenmesini unutmayın.

### <a name="queries"></a>Sorgular
> [!NOTE]
> Varlıkları şifrelendiği, filtre sorgularını bir şifrelenmiş özellikte çalıştıramazsınız.  Denerseniz, şifrelenmiş veriler şifrelenmemiş veri ile Karşılaştırılacak Service'i denediğiniz çünkü sonuçlar hatalı olacaktır.
> 
> 
> Sorgu işlemleri gerçekleştirmek için sonuç kümesinde tüm anahtarları çözümleyemiyorsa anahtar bir çözümleyici belirtmeniz gerekir. Sorgu sonucuna yer alan bir varlık için bir sağlayıcı çözümlenemezse, istemci kitaplığının bir hata atar. Sunucu tarafı projeksiyonlar gerçekleştirir herhangi bir sorgu için özel şifreleme meta veri özelliklerini istemci kitaplığı ekler (\_ClientEncryptionMetadata1 ve \_ClientEncryptionMetadata2) varsayılan olarak seçili sütunları.
> 
> [!IMPORTANT]
> İstemci tarafı şifreleme kullanırken şu önemli noktalara dikkat edin:
> 
> * Şifrelenmiş bir blobu yazma veya okuma, tüm blob karşıya yükleme komutlarını ve aralığı/tüm blob yükleme komutlarını kullanın. Blok yerleştirme, engelleme listesine yerleştirin, yazma sayfaları veya Temizle sayfaları gibi işlemleri protokolü kullanılarak şifrelenmiş bir blobu yazma kaçının; Aksi halde şifrelenmiş bir blobu bozuk ve okunamaz hale.
> * Tablolar için benzer bir kısıtlama yok. Şifrelenmiş özellikler şifreleme meta verileri güncelleştirmeden güncelleştiremezsiniz dikkat edin.
> * Meta veriler üzerinde şifrelenmiş bir blobu ayarlarsanız, şifre çözme için meta verileri ayarlama eklenebilir olmadığından gerekli şifrelemeyle ilgili meta veriler üzerine yazabilir. Bu da anlık görüntüler için geçerlidir; meta veri şifrelenmiş bir blobun anlık görüntüsünü oluşturulurken belirtmekten kaçının. Meta veriler ayarlanmalıdır, çağırdığınızdan emin olun **get_blob_metadata** ilk geçerli şifreleme meta verileri ve meta verileri ayarlanırken eşzamanlı yazma sorunu önlemek için yöntemi.
> * Etkinleştirme **require_encryption** bayrağı yalnızca şifrelenmiş verileri ile çalışma kullanıcılar için hizmet nesnesi. Aşağıda daha fazla bilgi için bkz.

Depolama istemcisi kitaplığı bekliyor. sağlanan KEK ve anahtar Çözümleyicisi aşağıdaki arabirim uygulamak için. [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) Python KEK yönetim Beklemede ve tamamlandığında bu kitaplığa tümleştirilecektir desteği.

## <a name="client-api--interface"></a>İstemci API'si / arabirimi
Depolama hizmeti nesnesi (yani blockblobservice) oluşturulduktan sonra kullanıcı bir şifreleme ilkesi oluşturan alanları için değerler atayabilir: key_encryption_key, key_resolver_function ve require_encryption. Kullanıcılar, yalnızca bir KEK sağlayabilir yalnızca bir çözümleyici veya her ikisi de. key_encryption_key anahtar tanımlayıcısı kullanılarak tanımlanır ve sarmalama/çözülme için mantığı sağlayan temel anahtar türü değil. key_resolver_function, bir anahtar şifre çözme işlemi sırasında çözmek için kullanılır. Anahtar tanımlayıcısını verilen geçerli bir KEK döndürür. Bu kullanıcılar birden fazla konumda yönetilen birden çok anahtar arasında seçim yapma olanağı sağlar.

KEK başarıyla verileri şifrelemek için aşağıdaki yöntemleri uygulamanız gerekir:

* wrap_key(cek): Kullanıcının seçtiği bir algoritma kullanarak belirtilen CEK (bayt) sarmalar. Sarmalanan anahtarını döndürür.
* get_key_wrap_algorithm(): Anahtarları sarmalamak için kullanılan algoritma döndürür.
* get_kid(): Dize anahtar kimliği için bu KEK döndürür.
  KEK başarıyla verilerin şifresini çözmek için aşağıdaki yöntemleri uygulamanız gerekir:
* unwrap_key (cek, algoritması): Dize olarak belirtilen algoritmasını kullanarak belirtilen CEK sarmalanmış halden biçiminde döndürür.
* get_kid(): Dize anahtar kimliği için bu KEK döndürür.

En az anahtar Çözümleyici, verilen bir anahtar kimlik yukarıdaki arabirimini uygulayan karşılık gelen KEK döndüren bir yöntem uygulaması gerekir. Hizmet nesnesi key_resolver_function özelliğine atanacak yalnızca bu yöntem kullanılır.

* Şifreleme için anahtar her zaman kullanılır ve bir anahtar olmaması bir hataya neden olur.
* Şifre çözme için:

  * Anahtar çözümleyici belirtilmişse anahtarını almak için çağrılır. Çözümleyici belirtildi, ancak anahtar tanımlayıcısı için bir eşleme yok. bir hata oluşturulur.
  * Tanımlayıcısını gerekli anahtar tanımlayıcısı eşleşiyorsa çözümleyici belirtilmedi, ancak belirtilen bir anahtarı anahtar kullanılır. Tanımlayıcı eşleşmiyorsa, bir hata oluşturulur.

    Azure.storage.samples şifreleme örneklerinde, BLOB'lar, kuyruklar ve tablolar için daha ayrıntılı bir uçtan uca senaryoyu göstermektedir.
      Örnek uygulamaları KEK ve anahtar çözümleyici örnek dosyaları sırasıyla KeyWrapper ve KeyResolver sağlanır.

### <a name="requireencryption-mode"></a>RequireEncryption modu
Kullanıcılar, isteğe bağlı olarak burada tüm karşıya yüklemelerden ve şifrelenmelidir işlemi modu etkinleştirebilirsiniz. Bu modda, istemcide bir şifreleme ilkesi olmadan veri yükleme veya hizmette şifrelenmez verileri indirmek için girişimleri başarısız olur. **Require_encryption** bayrağı hizmet nesnesi üzerinde bu davranışını denetler.

### <a name="blob-service-encryption"></a>BLOB hizmeti şifreleme
Şifreleme İlkesi alanları blockblobservice nesnesi üzerinde ayarlayın. Diğer her şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

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
Şifreleme İlkesi alanları queueservice nesnesi üzerinde ayarlayın. Diğer her şey istemci kitaplığı tarafından dahili olarak ele alınacaktır.

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
Bir şifreleme ilkesi oluşturma ve istek seçeneklerini ayarlama ek olarak, ya da belirtmelisiniz bir **encryption_resolver_function** üzerinde **tableservice**, veya şifreleme özniteliğini ayarlayın EntityProperty.

### <a name="using-the-resolver"></a>Çözümleyici kullanma

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
Yukarıda belirtildiği gibi bir özellik EntityProperty nesneyi depolanması ve şifreleme alanını ayarlamak için şifreleme işaretlenebilir.

```python
encrypted_property_1 = EntityProperty(EdmType.STRING, value, encrypt=True)
```

## <a name="encryption-and-performance"></a>Şifreleme ve performans
Depolama veri sonuçlarınızı ek performans yükünden şifreleme unutmayın. İçerik anahtar ve IV oluşturulmalıdır, içeriği şifrelenir ve ek meta veri biçimlendirilmiş karşıya ve. Bu ek yükü şifrelenen veri miktarı bağlı olarak değişir. Müşterilerin uygulamalarını geliştirme sırasında performans için her zaman test etmenizi öneririz.

## <a name="next-steps"></a>Sonraki adımlar
* İndirme [Java Pypı paketi için Azure depolama istemci kitaplığı](https://pypi.python.org/pypi/azure-storage)
* İndirme [Python için Azure depolama istemci kitaplığı kaynak kodunu github'dan](https://github.com/Azure/azure-storage-python)
