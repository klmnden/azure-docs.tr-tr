---
title: Paylaşılan erişim imzaları (SAS) Azure Depolama'daki kullanarak | Microsoft Docs
description: Bloblar, kuyruklar, tablolar ve dosyalar dahil olmak üzere, Azure depolama kaynaklara temsilci erişimi için paylaşılan erişim imzaları (SAS) kullanmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 04/18/2017
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 2b3c2ed7f2914374ac94783511f2992ae5755967
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67302329"
---
# <a name="using-shared-access-signatures-sas"></a>Paylaşılan erişim imzaları (SAS) kullanma

Paylaşılan erişim imzası (SAS), hesap anahtarınız açığa çıkarmadan depolama hesabınızdaki nesnelere sınırlı erişim diğer istemcilere vermenin bir yolunu sağlar. Bu makalede, biz SAS modeline genel bakış sağlayın, SAS en iyi uygulamaları gözden geçirin ve bazı örneklere göz atacağız.

Burada sunulan olanlar dışında SAS kullanarak ek kod örnekleri için bkz. [. NET'te Azure Blob Depolama ile çalışmaya başlama](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/) ve kullanılabilir diğer örnekler [Azure Kod örnekleri](https://azure.microsoft.com/documentation/samples/?service=storage) kitaplığı. Örnek uygulamaları indirin ve bunları çalıştırın veya github'da koduna göz atın.

## <a name="what-is-a-shared-access-signature"></a>Paylaşılan erişim imzası nedir?

Paylaşılan erişim imzası, depolama hesabınızdaki kaynaklara temsilci erişimi sağlar. Bir SAS ile hesap anahtarlarınızı paylaşmadan depolama hesabınızdaki kaynaklara erişim istemcileri verebilirsiniz. Bu, uygulamalarınızda paylaşılan erişim imzaları kullanmanın anahtar noktasıdır. SAS, hesap anahtarlarınızı tehlikeye atmadan depolama kaynaklarınızı paylaşmanın güvenli bir yoludur.

[!INCLUDE [storage-recommend-azure-ad-include](../../../includes/storage-recommend-azure-ad-include.md)]

[!INCLUDE [storage-account-key-note-include](../../../includes/storage-account-key-note-include.md)]

Bir SAS erişim de dahil olmak üzere, SAS sahip istemciler için verme türü üzerinde ayrıntılı denetim sağlar:

* SAS başlangıç ve sona erme saati de dahil olmak üzere, geçerli aralığı.
* SAS'den izinler. Örneğin, bir blob için bir SAS okuma izni ve bu bloba yazma izinleri, ancak silme izinleri yok.
* İsteğe bağlı bir IP adresi veya IP adresleri Azure depolama SAS kabul eder. Örneğin, kuruluşunuza ait bir IP adresi aralığı belirtebilirsiniz.
* Protokol üzerinden Azure depolama SAS kabul eder. Bu isteğe bağlı bir parametre, HTTPS kullanan istemciler için erişimi kısıtlamak için kullanabilirsiniz.

## <a name="when-should-you-use-a-shared-access-signature"></a>Paylaşılan erişim imzası kullanırken?

Depolama hesabınızın erişim anahtarlarını işlediği değil herhangi bir istemciye depolama hesabınızdaki kaynaklara erişimi sağlamak istediğinizde bir SAS kullanabilirsiniz. Depolama hesabınızın hem ikisi için de hesabınıza yönetici erişimi vermek, bir birincil ve ikincil erişim anahtarı ve içerdiği tüm kaynakları içerir. Bu anahtarların ya da ifşa eden kötü amaçlı veya hatalı kullanım olasılığını hesabınıza açılır. Paylaşılan erişim imzaları, okuma, yazma ve açıkça verilen izinlere göre ve hesap anahtarı için gerek kalmadan, depolama hesabınızdaki verileri silmek istemcilerin güvenli bir yöntem sağlar.

Bir SAS kullanışlı olduğu bir yaygın senaryo burada kullanıcılar okuyup kendi verilerini depolama hesabınıza bir hizmettir. Bir depolama hesabı, kullanıcı verilerini depoladığı bir senaryoda, iki tipik tasarım desenleri vardır:

1. İstemciler, indirin ve kimlik doğrulaması yapan bir ön uç proxy hizmeti aracılığıyla veri yükleyin. Bu ön uç proxy hizmeti için iş kuralları doğrulama sağlayan avantajı olsa da, büyük miktarlarda veri veya yüksek hacimli işlemler için isteğe bağlı şekilde bir hizmet oluşturma pahalı veya zor olabilir.

   ![Senaryo diyagramı: Ön uç proxy hizmeti](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-fe-proxy-service.png)   

1. Basit bir hizmet gerektiği gibi istemcinin kimliğini doğrular ve ardından bir SAS oluşturuyor. SAS istemci aldıktan sonra doğrudan SAS ve SAS tarafından izin verilen zaman aralığı için tanımlanan izinlere sahip depolama hesabı kaynaklarına erişebilirsiniz. SAS, ön uç proxy hizmeti aracılığıyla tüm verileri yönlendirme gereksinimini azaltır.

   ![Senaryo diyagramı: SAS sağlayıcısı hizmeti](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-provider-service.png)   

Çok sayıda gerçek hizmetlerini iki bu yaklaşımların bir karma kullanabilir. Örneğin, bazı veriler işlenir ve diğer veriler olduğundan ve/veya kaydedilmiş doğrudan SAS kullanarak okuma sırasında ön uç Ara sunucusu üzerinden doğrulandı.

Ayrıca, belirli senaryolarda bir kopyalama işleminde kaynak nesne erişim yetkisi vermek için bir SAS'ı kullanmanız gerekir:

* Farklı bir depolama hesabında bulunan başka bir blob için bir blob kopyaladığınızda, kaynak blobun erişim yetkisi vermek için bir SAS kullanmanız gerekir. İsteğe bağlı olarak, hedef blob de erişim yetkisi vermek için bir SAS kullanabilirsiniz.
* Farklı bir depolama hesabında bulunan başka bir dosyaya bir dosya kopyaladığınızda, kaynak dosyaya erişim yetkisi vermek için bir SAS kullanmanız gerekir. Hedef dosya de erişim yetkisi vermek için bir SAS isteğe bağlı olarak kullanabilirsiniz.
* Bir blobu bir dosyaya veya bir blobu bir dosyaya kopyalamanız, kaynak ve hedef nesnelerin aynı depolama hesabında bulunan olsa bile bir SAS kaynak nesnesi erişim yetkisi vermek için kullanmanız gerekir.

## <a name="types-of-shared-access-signatures"></a>Paylaşılan erişim imzaları türleri

İki tür paylaşılan erişim imzası oluşturabilirsiniz:

* **Hizmet SAS.** Hizmet SAS; Blob, Kuyruk, Tablo veya Dosya hizmeti olmak üzere yalnızca bir depolama hizmetindeki kaynağa erişim atar. Bkz: [hizmet SAS oluşturma](https://msdn.microsoft.com/library/dn140255.aspx) ve [hizmeti SAS örneklerini](https://msdn.microsoft.com/library/dn140256.aspx) hizmeti SAS belirteci oluşturma hakkında ayrıntılı bilgi için.
* **Hesap SAS.** Hesap SAS temsilcileri, bir veya daha fazla depolama hizmetindeki kaynaklara erişim. Tüm hizmet SAS ile kullanılabilen işlemleri ayrıca bir hesap SAS kullanılabilir. Ayrıca, hesap SAS ile belirli bir hizmete gibi uygulama işlemlerine erişim yetkilendirebilirsiniz **Get/Set hizmet özellikleri** ve **hizmet istatistikleri alma**. Bununla birlikte hizmet SAS ile izin verilmeyen blob kapsayıcılar, tablolar kuyruklar ve dosya paylaşımları üzerinde okuma, yazma ve silme işlemleri için yetkilendirme yapabilirsiniz. Bkz: [hesap SAS oluşturma](https://msdn.microsoft.com/library/mt584140.aspx) hesap SAS belirteci oluşturma hakkında ayrıntılı bilgi için.

## <a name="how-a-shared-access-signature-works"></a>Paylaşılan erişim imzası nasıl çalışır?

Paylaşılan erişim imzası, bir veya daha fazla depolama kaynaklarını ve özel bir sorgu parametreleri kümesini içeren bir belirteç içeren imzalı bir URI'dir. Belirteç, kaynaklar istemci tarafından erişilebilecek nasıl gösterir. Sorgu parametreleri, imza birini SAS parametreler oluşturulur ve hesap anahtarı ile imzalanmış. Bu imza, depolama kaynağına erişim yetkisi vermek için Azure Depolama tarafından kullanılır.

Kaynak URI gösteren bir SAS URI'sinin bir örnek aşağıda verilmiştir ve SAS belirteci:

![Bir SAS URI'ın bileşenleri](./media/storage-dotnet-shared-access-signature-part-1/sas-storage-uri.png)   

SAS belirteci oluşturma hakkında bir dizedir *istemci* yan (bkz [SAS örnekler](#sas-examples) kod örnekleri için bölüm). Depolama istemci kitaplığı ile oluşturduğunuz bir SAS belirteci gibi herhangi bir şekilde Azure Depolama tarafından izlenmiyor. İstemci tarafında sınırsız sayıda SAS belirteçleri oluşturabilirsiniz.

Bir istemci, bir isteğin bir parçası Azure depolama SAS URI'si sağladığında, hizmet isteği kimlik doğrulaması için geçerli olduğunu doğrulamak için imza ve SAS parametreleri denetler. Hizmet doğrularsa imza geçerli değil ve isteğin yetkilendirilip. Aksi takdirde, istek, hata kodu 403 (Yasak) reddedildi.

## <a name="shared-access-signature-parameters"></a>Paylaşılan erişim imzası parametreleri

Hizmet SAS belirteçleri ve hesap SAS bazı ortak parametreleri içerir ve ayrıca farklı olan birkaç parametre alır.

### <a name="parameters-common-to-account-sas-and-service-sas-tokens"></a>Hesap SAS ortak parametreleri ve hizmet SAS belirteçleri

* **API sürümü** ve isteği yürütmek için kullanılacak depolama hizmeti sürümünü belirten isteğe bağlı bir parametre.
* **Hizmet sürümü** gerekli parametresi isteği yetkilendirmek için kullanılacak depolama hizmeti sürümünü belirtir.
* **Başlangıç zamanı.** Bu, SAS geçerli olacağı süredir. Paylaşılan erişim imzası için başlangıç zamanı isteğe bağlıdır. Başlangıç zamanı belirtilmezse, SAS hemen etkili olur. Başlangıç saati UTC (Eşgüdümlü Evrensel Saat) özel UTC gösterge ile ("Z"), gibi ifade edilmelidir `1994-11-05T13:15:30Z`.
* **Süre sonu.** Bu, sonra SAS artık geçerli olduğu zamandır. En iyi süre sonu için bir SAS belirtin veya bir depolanmış erişim ilkesi ile ilişkilendirebilirsiniz önerilir. Süre sonu UTC (Eşgüdümlü Evrensel Saat) özel UTC gösterge ile ("Z"), gibi ifade edilmelidir `1994-11-05T13:15:30Z` (daha aşağıya bakın).
* **İzinler.** SAS üzerinde belirtilen izinler istemci SAS kullanarak depolama kaynağı karşı gerçekleştirebilirsiniz hangi işlemleri gösterir. Kullanılabilir seçenekler, bir hesap SAS ve hizmet SAS için farklıdır.
* **IP.** Bir IP adresi veya bir IP adresi aralığı dışında Azure belirten isteğe bağlı bir parametre (bakın [yönlendirme oturum yapılandırma durumu](../../expressroute/expressroute-workflows.md#routing-session-configuration-state) Express Route için) içinden isteklerini kabul etmek.
* **Protokol.** Protokolü belirten isteğe bağlı parametresi bir istek için izin verilir. Olası değerler şunlardır: hem HTTPS ve HTTP (`https,http`), bu değer varsayılan değer veya HTTPS yalnızca (`https`). HTTP yalnızca izin verilen bir değer değildir.
* **İmza.** İmza bölümü belirteci olarak belirtilen ve ardından şifreli diğer parametreler oluşturulur. İmza, belirtilen depolama kaynaklarına erişim yetkisi vermek için kullanılır.

### <a name="parameters-for-a-service-sas-token"></a>Hizmet SAS belirteci için parametreleri

* **Depolama kaynağı.** SAS depolama kaynaklarına erişim ile bir hizmeti temsilci seçebilirsiniz şunlardır:
  * Kapsayıcılar ve bloblar
  * Dosya paylaşımları ve dosyalarla
  * Kuyruklar
  * Tabloları ve tablo varlıkları aralığı.

### <a name="parameters-for-an-account-sas-token"></a>Bir hesap SAS belirteci için parametreleri

* **Hizmet veya hizmetleri.** Hesap SAS ise bir erişim bir veya daha fazla depolama hizmetleri için yetkilendirme yapabilirsiniz. Örneğin, temsilciler Blob ve dosya hizmetine erişim hesap SAS oluşturabilirsiniz. Veya tüm dört temsilciler erişim (Blob, kuyruk, tablo ve dosya) Hizmetleri bir SAS oluşturabilirsiniz.
* **Depolama kaynak türleri.** Bir hesap SAS depolama kaynaklarını yerine belirli bir kaynağa bir veya daha fazla sınıfları için geçerlidir. Hesap erişimi devretmek için SAS oluşturabilirsiniz:
  * Hizmet düzeyi API'leri, depolama hesabı kaynağı karşı olarak adlandırılır. Örnekler **Get/Set hizmet özellikleri**, **hizmet istatistikleri alma**, ve **listesi kapsayıcılar/kuyruk/tablolar/paylaşımları**.
  * Her hizmet için kapsayıcı nesneleri karşı çağrılan kapsayıcı düzeyi API'leri: blob kapsayıcıları, kuyruklar, tablolar ve dosya paylaşımları. Örnekler **oluşturma/silme kapsayıcı**, **oluşturma/silme sırası**, **oluşturma/silme tablo**, **oluşturma/silme paylaşımı**ve  **Blobları/dosyalar ve dizinler listesinde**.
  * Nesne düzeyinde API'ler, bloblar, kuyruk iletileri, tablo varlıkları ve dosyaları karşı olarak adlandırılır. Örneğin, **Put Blob**, **sorgu varlığı**, **iletileri alma**, ve **dosyası oluştur**.

## <a name="examples-of-sas-uris"></a>SAS URI örnekleri

### <a name="service-sas-uri-example"></a>Hizmet SAS URI'sini örneği

Bir hizmet sağlayan bir SAS URI'si okuma ve yazma izinleri bir bloba örneği aşağıda verilmiştir. Tablo için SAS nasıl katkı sağladığını anlamanızı için URI her bir parçasının ayırır:

```
https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt?sv=2015-04-05&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D
```

| Ad | SAS bölümü | Açıklama |
| --- | --- | --- |
| Blob URI |`https://myaccount.blob.core.windows.net/sascontainer/sasblob.txt` |Blob adresi. HTTPS kullanarak önemle tavsiye edilir unutmayın. |
| Depolama Hizmetleri sürümü |`sv=2015-04-05` |Depolama Hizmetleri sürüm 2012-02-12 ve daha sonra bu parametre kullanılacak sürümünü gösterir. |
| Başlangıç saati |`st=2015-04-29T22%3A18%3A26Z` |UTC saati belirtilmiş. SAS hemen geçerli olmasını istiyorsanız, başlangıç zamanı atlayın. |
| Süre sonu |`se=2015-04-30T02%3A23%3A26Z` |UTC saati belirtilmiş. |
| Resource |`sr=b` |Bir blobu bir kaynaktır. |
| İzinler |`sp=rw` |SAS'den izinler Read (r) içerir ve yazma (w). |
| IP aralığı |`sip=168.1.5.60-168.1.5.70` |Bir talep kabul edilmeyecektir IP adresi aralığı. |
| Protocol |`spr=https` |Yalnızca HTTPS kullanarak isteklerine izin verilir. |
| İmza |`sig=Z%2FRHIX5Xcg0Mq2rqI3OlWTjEg2tYkboXr1P9ZUXDtkk%3D` |Blob erişim yetkisi vermek için kullanılır. İmza, dize oturum ve SHA256 algoritmasını kullanılarak anahtarı üzerinden hesaplanır ve ardından Base64 kodlama kullanılarak kodlanan bir HMAC değil. |

### <a name="account-sas-uri-example"></a>Hesap SAS URI'sini örneği

Bir hesap SAS belirteci üzerinde güncel aynı genel parametreleri kullanan bir örnek aşağıda verilmiştir. Bu parametreler, yukarıda açıklanan olduğundan, burada açıklanmamıştır. Yalnızca parametreleri, hesap SAS, aşağıdaki tabloda açıklanan özgüdür.

```
https://myaccount.blob.core.windows.net/?restype=service&comp=properties&sv=2015-04-05&ss=bf&srt=s&st=2015-04-29T22%3A18%3A26Z&se=2015-04-30T02%3A23%3A26Z&sr=b&sp=rw&sip=168.1.5.60-168.1.5.70&spr=https&sig=F%6GRVAZ5Cdj2Pw4tgU7IlSTkWgn7bUkkAg8P6HESXwmf%4B
```

| Ad | SAS bölümü | Açıklama |
| --- | --- | --- |
| Kaynak URI'si |`https://myaccount.blob.core.windows.net/?restype=service&comp=properties` |Blob Hizmeti uç noktası, (GET ile çağrıldığında) hizmeti özelliklerini alma veya (KÜMESİYLE çağrıldığında) hizmeti özelliklerini ayarlamanın parametrelere sahip. |
| Hizmetler |`ss=bf` |SAS Blob ve Dosya Hizmetleri için geçerlidir. |
| Kaynak türleri |`srt=s` |SAS hizmet düzeyi işlemlere uygulanır. |
| İzinler |`sp=rw` |Okuma ve yazma işlemleri için erişim izinleri verin. |

Hizmet düzeyi izinleri kısıtlanır düşünüldüğünde, bu SAS erişilebilir işlemleriyle olan **Blob hizmeti özelliklerini almak** (okuma) ve **Blob hizmeti özelliklerini ayarla** (yazma). Ancak, farklı olan bir kaynak URI, aynı SAS belirteci de erişimi devretmek için kullanılabilir **Blob hizmet istatistikleri alma** (okuma).

## <a name="controlling-a-sas-with-a-stored-access-policy"></a>Bir depolanmış erişim ilkesini ile SAS denetleme

Paylaşılan erişim imzası iki biçimlerden birini alabilir:

* **Geçici SAS:** Geçici bir SAS'ı oluşturduğunuzda, başlangıç zamanı, süre sonu ve SAS izinleri tüm SAS URI'de belirtilen (veya ima, burada başlangıç zamanı atlanır durumda). Bu tür bir SAS, hesap SAS ise bir ya da hizmet SAS oluşturulabilir.
* **Depolanmış erişim ilkesini ile SAS:** Bir depolanmış erişim ilkesini bir kaynak kapsayıcı--bir blob kapsayıcısı tanımlanır, tablo, kuyruk, veya dosya paylaşımını--ve biri için kısıtlamalarını yönetmek için kullanılabilir veya daha fazla paylaşılan erişim imzaları. Bir SAS bir depolanmış erişim ilkesini ile ilişkilendirdiğinizde, SAS başlangıç zamanı, süre sonu ve izinleri--depolanmış erişim ilkesini için tanımlanmış kısıtlamalar devralır.

> [!NOTE]
> Şu anda hesap SAS ise bir geçici bir SAS olmalıdır. Erişim için hesap SAS ilkeleri henüz desteklenmemektedir depolanır.

Bir anahtar senaryosu için iki biçim arasındaki fark önemlidir: iptal etme. SAS URI'sini bir URL olduğundan, ilk olarak onu oluşturan kişi bağımsız olarak herkes SAS alır, kullanabilir. Bir SAS yayımlandığını, herkes tarafından kullanılabilir. Bir SAS dört şeylerden biri oluşuncaya kadar işlediği herkese kaynaklarına erişimi verir:

1. SAS üzerinde belirtilen süre sonu ulaşıldı.
2. SAS'den başvurulan depolanmış erişim ilkesini belirtilen sona erme saati (depolanmış erişim ilkesini başvuruluyorsa ve süre sonu belirtiyorsa) ulaşıldı. Bu aralığı sona erdiğinde olduğundan veya bir süre sonu zamanı geçmişte, SAS iptal etmek için bir yol ile depolanmış erişim ilkesini değiştirdiğiniz nedeniyle ortaya çıkabilir.
3. SAS iptal etmek için başka bir yolu olan SAS tarafından başvurulan depolanmış erişim ilkesini silinir. Tam olarak aynı ada sahip bir depolanmış erişim ilkesini yeniden oluşturun, mevcut tüm SAS belirteçlerini yeniden (, değil SAS bitiş saatinin geçtiğini varsayarak) Bu saklı erişim ilkesi ile ilişkilendirilmiş izinleri göre geçerli olacağını unutmayın. SAS iptal amaçlanıyorsa erişim ilkesi gelecekte bir sona erme saati ile yeniden farklı bir ad kullanırsanız emin olun.
4. SAS oluşturmak için kullanılan hesap anahtarı yeniden oluşturuldu. Bir hesap anahtarını yeniden oluşturma tüm uygulama bileşenleri bu anahtarla diğer geçerli hesap anahtarı veya yeniden oluşturuldu yeni hesap anahtarı kullanmak üzere güncelleştirilir kadar yetkilendirmek başarısız olmasına neden olur.

> [!IMPORTANT]
> Paylaşılan erişim imzası URI'si imza oluşturmak için kullanılan hesap anahtarı ile ilişkilidir, ve ilişkili erişim ilkesi (varsa) depolanır. Hiçbir depolanmış erişim ilkesini belirtilirse, paylaşılan erişim imzası iptal etmek için tek yolu hesap anahtarını değiştirmektir.

## <a name="authenticating-from-a-client-application-with-a-sas"></a>Bir SAS ile bir istemci uygulamasında kimlik doğrulaması

Bir SAS elinde bulunan bir istemci, SAS, hesap anahtarlarını sahip olmayan bir depolama hesabına yönelik bir isteği yetkilendirmek için kullanabilirsiniz. Bir SAS bağlantı dizesi ile birlikte veya doğrudan uygun oluşturucu veya yöntemi kullanılır.

### <a name="using-a-sas-in-a-connection-string"></a>SAS kullanarak bir bağlantı dizesi

[!INCLUDE [storage-use-sas-in-connection-string-include](../../../includes/storage-use-sas-in-connection-string-include.md)]

### <a name="using-a-sas-in-a-constructor-or-method"></a>Bir oluşturucu ya da yöntem SAS kullanarak

Bir SAS ile hizmet isteğine yetki verebilir böylece birden fazla Azure depolama istemci kitaplığı oluşturucular ve yöntem aşırı bir SAS parametre sunar.

Örneğin, burada bir SAS URI bir blok blobuna bir başvuru oluşturmak için kullanılır. SAS istek için gereken tek kimlik bilgisi sağlanır. Blok blob başvurusu, daha sonra bir yazma işlemi için kullanılır:

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

## <a name="best-practices-when-using-sas"></a>SAS kullanarak en iyi uygulamalar

Uygulamalarınızda paylaşılan erişim imzaları kullandığınızda iki olası riskleri dikkat etmeniz gerekir:

* Bir SAS sızmış ise, depolama hesabınıza potansiyel olarak tehlikeye atabilir aldığı herkes tarafından kullanılabilir.
* İçin bir SAS sağlanırsa, bir istemci uygulamanın süresi doluyor ve uygulamayı yeni bir SAS, hizmetten alınamıyor, sonra uygulamanın işlevselliğini engelliyordu.

Paylaşılan erişim imzalarını kullanma yönelik aşağıdaki öneriler bu risklerin azaltılmasına yardımcı olur:

1. **Her zaman HTTPS kullanın** oluşturmak ya da SAS'ı dağıtın. SAS HTTP üzerinden geçirilen ve müdahale, bir adam-de-ortadaki adam saldırısı gerçekleştiren bir saldırgan SAS okuma ve yalnızca hedeflenen kullanıcının, olası hassas verileri ödün veya kötü amaçlı kullanıcı tarafından veri bozulması izin verme sahip olarak kullanın.
2. **Saklı erişim ilkeleri, mümkün olduğunda başvurur.** Saklı erişim ilkeleri, depolama hesabı anahtarlarını yeniden oluşturmak zorunda kalmadan izinleri iptal etme seçeneğini sağlar. Sona erme bu kadar çok ileride (veya sonsuz) üzerinde ayarlayın ve geleceğe daha da esnetmenize taşımak için düzenli olarak güncelleştirildiğinden emin olun.
3. **Zaman aşımı değeri ertelenebildiği üzerinde geçici bir SAS kullanın.** Bir SAS tehlikede olsa bile, bu şekilde, bunu yalnızca kısa bir süre için geçerli değil. Bu yöntem, bir depolanmış erişim ilkesini başvuramaz durumunda özellikle önemlidir. Ertelenebildiği geçerlilik sonu süreleri de için karşıya yüklemek için süreyi sınırlamak için bir blob yazılabilir veri miktarını sınırlar.
4. **Gerekirse, SAS otomatik olarak yenileme istemciniz.** İstemciler, SAS sağlayan hizmet kullanılamıyorsa, yeniden denemeler için zaman tanınması SAS süresi dolmadan önce de yenilemelisiniz. Az sayıda süre içinde tamamlanması bekleniyor anında, kısa süreli işlemler için kullanılacak, SAS geliyorsa, ardından bu SAS yenilenmesi için beklendiği gibi gereksiz olabilir. Ancak, düzenli olarak SAS keşfi yapan istemciniz varsa, sona erme olasılığını harekete geçer. Anahtar için SAS kısa süreli olması gerekir (daha önce belirtildiği gibi) husustur olan istemci yenileme erken isteyen emin olmak için yeterli (önce başarılı bir yenileme süresinin dolmasını SAS nedeniyle kesilmemesi).
5. **SAS başlangıç saati ile dikkat edin.** İçin bir SAS için başlangıç zamanı ayarlarsanız **artık**sonra (geçerli saat farklı makinelere göre farklılıkları) nedeniyle saat eğriltme, hataları gözlemlenen aralıklı olarak ilk birkaç dakika. Genel olarak, başlangıç saati'en az 15 dakika önce olacak şekilde ayarlayın. Veya, hangi, hemen tüm durumlarda geçerli hale getirir ayarlamanız gerekmez. Aynı genellikle de--sona erme saati geçerli saat 15 dakikaya kadar herhangi bir istek üzerinde herhangi bir yönde eğriltme gözlemleyin unutmayın. 2012-02-12'den önceki bir REST sürümü kullanan istemciler için bir depolanmış erişim ilkesini başvurmayan bir SAS için süre üst sınırını 1 saat ve başarısız olur daha uzun vadeli belirterek tüm ilkeler var.
6. **Kaynak erişilmesi için belirli olabilir.** En iyi güvenlik uygulaması, gerekli en düşük ayrıcalıkları olan bir kullanıcı sağlamaktır. Bir kullanıcı yalnızca tek bir varlığa yönelik okuma erişimi gerekiyorsa, daha sonra bunları tek bir varlık için okuma erişimi ve tüm varlıklar olmayan okuma/yazma/silme erişimi verin. Ayrıca SAS sunun saldırganın daha az güç olduğundan SAS tehlikedeyse hasarı azaltmak da yardımcı olur.
7. **Hesabınız ile SAS yapılan dahil olmak üzere herhangi bir kullanım için faturalandırılırsınız anlayın.** Bir bloba yazma erişimi sağlayan, bir kullanıcı, 200 GB blob karşıya yüklemek tercih edebilirsiniz. Bunları okuma erişim verdiyseniz, bunların 10 kez indirmek 2 TB çıkış içinde sizin için maliyetler tercih edebilirsiniz. Yeniden olası kötü amaçlı kullanıcıların eylemlerini azaltmaya yardımcı olmak için sınırlı izinler sağlayın. Bu tehdidi azaltmanız (ancak saatinin bitiş saat eğriltme dikkatli olmanız için) kısa süreli SAS'ı kullanın.
8. **SAS kullanarak yazılan veri doğrulayın.** Bir istemci uygulaması, depolama hesabınıza verileri yazdığında, bu verileri ile ilgili sorunlar olabilir aklınızda bulundurun. Uygulamanızın veri doğrulanmış veya kullanıma hazır hale gelmeden önce yetkili gerektiriyorsa, bu doğrulama verileri yazıldıktan sonra ve uygulamanız tarafından kullanılmadan önce gerçekleştirmeniz gerekir. Bu uygulama ayrıca hesabınız için doğru SAS edinilen bir kullanıcı veya sızdırılan SAS kötüye bir kullanıcı tarafından yazılan bozuk ya da kötü amaçlı veri karşı korur.
9. **Her zaman SAS kullanmayın.** Bazen, depolama hesabınıza karşı belirli bir işlemle ilişkili riskleri SAS basıyor. Bu işlemler için iş gerçekleştirdikten sonra depolama hesabınıza Yazar bir orta katman hizmet oluşturma kural doğrulama, kimlik doğrulaması ve denetim. Ayrıca, bazı durumlarda, farklı yollarla erişimi yönetmek basittir. Örneğin, tüm BLOB'ları bir kapsayıcıda genel olarak okunabilir hale getirmek istiyorsanız, genel, kapsayıcı yapabilirsiniz yerine SAS her istemci için erişim sağlama.
10. **Depolama analizi, uygulamanızı izlemek için kullanın.** Kesinti nedeniyle kimlik doğrulama hataları herhangi bir artış SAS sağlayıcısı hizmetinizdeki ya da depolanmış erişim ilkesini yanlışlıkla kaldırılmasına gözlemlemek için günlük kaydını ve ölçümleri kullanabilirsiniz. Bkz: [Azure depolama ekibi blogu](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) ek bilgi için.

## <a name="sas-examples"></a>SAS örnekleri

Aşağıda bazı örnekler paylaşılan erişim imzaları, hesap SAS her iki türdeki ve SAS hizmet.

Bu C# örnekleri çalıştırmak için aşağıdaki NuGet paketlerini projenize başvuru gerekir:

* [.NET için Azure depolama istemci Kitaplığı](https://www.nuget.org/packages/WindowsAzure.Storage), sürüm 6.x veya daha sonra (hesap SAS kullanmak üzere).
* [Azure Configuration Manager](https://www.nuget.org/packages/Microsoft.Azure.ConfigurationManager)

Oluşturma ve bir SAS test gösteren ek örnekler için bkz: [Azure depolama kod örnekleri](https://azure.microsoft.com/documentation/samples/?service=storage).

### <a name="example-create-and-use-an-account-sas"></a>Örnek: Oluşturma ve bir hesap SAS kullanma

Aşağıdaki kod örneği, bir hesap, Blob ve Dosya Hizmetleri için geçerli olan ve istemciye izinlerini okuma, yazma ve liste hizmet düzeyi API'lere erişim izni verir. SAS oluşturur. İstek ile HTTPS yapılması için hesap SAS Protokolü HTTPS için sınırlar.

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

Blob hizmeti için hizmet düzeyi API'lerine erişmek için hesap SAS'ı kullanmak için depolama hesabınız için SAS ve Blob Depolama uç noktası kullanarak Blob istemci nesnesi oluşturun.

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

### <a name="example-create-a-stored-access-policy"></a>Örnek: Bir depolanmış erişim ilkesini oluşturma

Aşağıdaki kod bir depolanmış erişim ilkesini bir kapsayıcı oluşturur. Erişim ilkesi, kapsayıcıya veya bloblarına bir hizmet SAS için sınırlamalar belirlemek için kullanabilirsiniz.

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

### <a name="example-create-a-service-sas-on-a-container"></a>Örnek: Bir kapsayıcı hizmet SAS oluşturma

Aşağıdaki kod bir SAS bir kapsayıcı oluşturur. Varolan bir depolanmış erişim ilkesini adı sağlanmazsa, bu ilke SAS ile ilişkilendirilir. Hiçbir depolanmış erişim ilkesini sağlanırsa, kod kapsayıcısını geçici bir SAS oluşturur.

```csharp
private static string GetContainerSasUri(CloudBlobContainer container, string storedPolicyName = null)
{
    string sasContainerToken;

    // If no stored policy is specified, create a new access policy and define its constraints.
    if (storedPolicyName == null)
    {
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad hoc SAS, and
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
        // It is also possible to specify some constraints on an ad hoc SAS and others on the stored access policy.
        sasContainerToken = container.GetSharedAccessSignature(null, storedPolicyName);

        Console.WriteLine("SAS for blob container (stored access policy): {0}", sasContainerToken);
        Console.WriteLine();
    }

    // Return the URI string for the container, including the SAS token.
    return container.Uri + sasContainerToken;
}
```

### <a name="example-create-a-service-sas-on-a-blob"></a>Örnek: Blob üzerinde hizmet SAS oluşturma

Aşağıdaki kod blob üzerinde bir SAS oluşturur. Varolan bir depolanmış erişim ilkesini adı sağlanmazsa, bu ilke SAS ile ilişkilendirilir. Hiçbir depolanmış erişim ilkesini sağlanırsa, kod blob üzerinde geçici bir SAS oluşturur.

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
        // Note that the SharedAccessBlobPolicy class is used both to define the parameters of an ad hoc SAS, and
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

Paylaşılan erişim imzaları, depolama hesabınıza sınırlı izinlere hesap anahtarı bulunmamalıdır istemcilerine sağlamak için kullanışlıdır. Bu nedenle, Azure depolama kullanan uygulamalar için güvenlik modelinin önemli bir parçası olan. Burada listelenen en iyi uygulamaları izlerseniz, uygulamanızın güvenliğini tehlikeye atmadan depolama hesabınızdaki kaynaklara erişim daha fazla esneklik sağlamak için SAS'ı kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki Adımlar

* [Kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md)
* [Paylaşılan Erişim İmzası ile Erişim için Temsilci Seçme](https://msdn.microsoft.com/library/azure/ee395415.aspx)
* [Tablo ve kuyruk SAS ile tanışın](https://blogs.msdn.com/b/windowsazurestorage/archive/2012/06/12/introducing-table-sas-shared-access-signature-queue-sas-and-update-to-blob-sas.aspx)
