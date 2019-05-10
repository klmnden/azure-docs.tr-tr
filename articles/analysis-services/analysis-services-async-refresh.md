---
title: Azure Analysis Services modellerine yönelik zaman uyumsuz yenileme | Microsoft Docs
description: REST API kullanarak zaman uyumsuz yenileme kod öğrenin.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 63b64df457af5b7d3d2bd5901f73d89ccd3c913a
ms.sourcegitcommit: 8fc5f676285020379304e3869f01de0653e39466
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/09/2019
ms.locfileid: "65506965"
---
# <a name="asynchronous-refresh-with-the-rest-api"></a>REST API ile zaman uyumsuz yenileme

REST çağrılarını destekleyen herhangi bir programlama dilini kullanarak, Azure Analysis Services tablosal Modellerinizi zaman uyumsuz veri yenileme işlemleri gerçekleştirebilirsiniz. Bu, eşitleme için sorgu genişleme salt okunur çoğaltmaların içerir. 

Veri yenileme işlemleri, veri hacmi, bölümler, vb. kullanarak iyileştirmesi düzeyi dahil olmak üzere bir dizi etkene bağlı olarak biraz zaman alabilir. Bu işlem geleneksel olarak kullanma gibi mevcut yöntemleri ile çağrılabilir [TOM](https://docs.microsoft.com/sql/analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo) (tablolu nesne modeli) [PowerShell](https://docs.microsoft.com/sql/analysis-services/powershell/analysis-services-powershell-reference) cmdlet'lerini veya [TMSL](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) (Tablosal Model Komut dosyası dili). Ancak, bu yöntemler genellikle güvenilir olmayan, uzun süren HTTP bağlantıları gerektirebilir.

Azure Analysis Services için REST API zaman uyumsuz olarak gerçekleştirilebilmesi veri yenileme işlemleri sağlar. REST API kullanarak istemci uygulamalarından uzun süren HTTP bağlantıları gerekli değildir. Güvenilirlik, otomatik yeniden denemeler ve toplu işleme gibi diğer yerleşik özellikler vardır.

## <a name="base-url"></a>Temel URL

Temel URL'si şu biçimdedir:

```
https://<rollout>.asazure.windows.net/servers/<serverName>/models/<resource>/
```

Örneğin, adlı myserver, Batı ABD Azure bölgesinde bulunan bir sunucuda AdventureWorks adlı bir model göz önünde bulundurun. Sunucu adı şöyledir:

```
asazure://westus.asazure.windows.net/myserver 
```

Bu sunucu adı temel URL'si şudur:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/ 
```

Temel URL'ı kullanarak, kaynak ve işlem aşağıdaki parametreleri temel alarak eklenerek: 

![Zaman uyumsuz yenileme](./media/analysis-services-async-refresh/aas-async-refresh-flow.png)

- İle biten bir şey **s** koleksiyonudur.
- İle biten herhangi bir şey **()** bir işlevdir.
- Başka bir kaynak/nesnesidir.

Örneğin, bir yenileme işlemi gerçekleştirmeye yenilemeleri koleksiyonunda POST edimi kullanabilirsiniz:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/refreshes
```

## <a name="authentication"></a>Kimlik Doğrulaması

Tüm çağrıları ile geçerli bir Azure Active Directory (OAuth 2) belirteci yetkilendirme üst bilgisinde kimliğinin doğrulanması gerekir ve aşağıdaki gereksinimleri karşılaması gerekir:

- Belirteç, bir kullanıcı belirteci ya da bir uygulama hizmet sorumlusu olması gerekir.
- Belirteç kümesine doğru hedef kitleye olmalıdır `https://*.asazure.windows.net`.
- Kullanıcı veya uygulama istenen çağrı yapmak için yeterli izinleri sunucu veya model olmalıdır. İzin düzeyi rolleri model veya sunucu üzerindeki yönetim grubu tarafından belirlenir.

    > [!IMPORTANT]
    > Şu anda **Sunucu Yöneticisi** rolü izinleri gereklidir.

## <a name="post-refreshes"></a>POST /refreshes

Bir yenileme işlemi gerçekleştirmek için yeni bir yenileme öğe koleksiyona eklemek için /refreshes koleksiyonunda POST edimi kullanın. Yanıt Location üst bilgisini yenileme kimliğini içerir İstemci uygulama, kesin ve durumunu zaman uyumsuz olduğundan gerekirse daha sonra denetleyin.

Bir model için bir kerede yalnızca bir yenileme işlemi kabul edilir. Varsa geçerli çalışan bir yenileme işlemi ve başka gönderilen 409 çakışma HTTP durum kodu döndürülür.

Gövde aşağıdakine benzeyebilir:

```
{
    "Type": "Full",
    "CommitMode": "transactional",
    "MaxParallelism": 2,
    "RetryCount": 2,
    "Objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer"
        },
        {
            "table": "DimDate"
        }
    ]
}
```

### <a name="parameters"></a>Parametreler

Parametreleri belirterek, gerekli değildir. Varsayılan olarak uygulanır.

| Ad             | Tür  | Açıklama  |Varsayılan  |
|------------------|-------|--------------|---------|
| `Type`           | Sabit listesi  | İşlem türü. Türleri ile TMSL hizalanır [Yenile komut](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl) türleri: tam, clearValues, dataOnly, otomatik, hesaplama ve birleştirin. Ekleme türü desteklenmiyor.      |   Otomatik      |
| `CommitMode`     | Sabit listesi  | Nesneleri yalnızca tamamlandığında veya toplu kaydedilmiş olup olmayacağını belirler. Modları içerir: varsayılan, işlem partialBatch.  |  işlem       |
| `MaxParallelism` | Int   | Bu değer, iş parçacıkları, paralel olarak işleme komutları çalıştırmak en fazla sayısını belirler. Bu değer hizalı TMSL ayarlanabilir MaxParallelism özelliği ile [Sequence komutu](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/sequence-command-tmsl) veya diğer yöntemleri kullanarak.       | 10        |
| `RetryCount`     | Int   | İşlem başarısız olmadan önce yeniden denenme sayısını gösterir.      |     0    |
| `Objects`        | Dizi | İşlenecek nesne dizisi. Her bir nesne içerir: "Tablo" tüm tablo ya da "Tablo" ve "Bölüm" bölümü işlenirken işlerken. Nesne yok belirtilirse, modelin tamamını yenilenir. |   İşlem tüm modeli      |

CommitMode partialBatch için eşittir. Büyük veri kümelerinin ilk bir yük, bunu saat kadar sürebilir olduğunda kullanılır. Bir veya daha fazla toplu uyguladıktan sonra yenileme işlemi başarısız olursa, başarılı bir şekilde kaydedilmiş toplu kaydedilmiş kalır (bunu başarıyla kaydedilmiş toplu geri döner değil).

> [!NOTE]
> Yazma sırasında toplu iş boyutu MaxParallelism değerdir, ancak bu değeri değiştirebilirsiniz.

## <a name="get-refreshesrefreshid"></a>GET /refreshes/\<refreshId >

Bir yenileme işlemi durumunu denetlemek için yenileme kimliğini temel GET fiili kullanın Yanıt gövdesi bir örnek aşağıda verilmiştir. İşlem devam ediyor, olursa **Inprogress** durumu döndürülür.

```
{
    "startTime": "2017-12-07T02:06:57.1838734Z",
    "endTime": "2017-12-07T02:07:00.4929675Z",
    "type": "full",
    "status": "succeeded",
    "currentRefreshType": "full",
    "objects": [
        {
            "table": "DimCustomer",
            "partition": "DimCustomer",
            "status": "succeeded"
        },
        {
            "table": "DimDate",
            "partition": "DimDate",
            "status": "succeeded"
        }
    ]
}
```

## <a name="get-refreshes"></a>/Refreshes Al

Bir model için geçmiş yenileme işlemleri bir listesini almak için GET fiili /refreshes koleksiyonunda kullanın. Yanıt gövdesi bir örnek aşağıda verilmiştir. 

> [!NOTE]
> Yazma sırasında son 30 gün yenileme işlemleri depolanan ve döndürdü, ancak bu sayıyı değiştirebilirsiniz.

```
[
    {
        "refreshId": "1344a272-7893-4afa-a4b3-3fb87222fdac",
        "startTime": "2017-12-09T01:58:04.76",
        "endTime": "2017-12-09T01:58:12.607",
        "status": "succeeded"
    },
    {
        "refreshId": "474fc5a0-3d69-4c5d-adb4-8a846fa5580b",
        "startTime": "2017-12-07T02:05:48.32",
        "endTime": "2017-12-07T02:05:54.913",
        "status": "succeeded"
    }
]
```

## <a name="delete-refreshesrefreshid"></a>DELETE /refreshes/\<refreshId >

Devam eden yenileme işlemi iptal etmek için yenileme kimliğini temel silme fiili kullanın

## <a name="post-sync"></a>POST Sync

Yenileme işlemlerini gerçekleştirilecek yinelemeler için sorgu ölçeği genişletme ile yeni verileri eşitlemek gerekli olabilir. Bir model için bir eşitleme işlemi gerçekleştirmek için POST edimi Sync işlevini kullanın. Yanıt Location üst bilgisini eşitleme işlemi kimliğini içerir

## <a name="get-sync-status"></a>Sync durumunu Al

Eşitleme işlemi durumunu denetlemek için işlem kimliği bir parametre olarak geçirerek GET fiili kullanın. Yanıt gövdesi bir örnek aşağıda verilmiştir:

```
{
    "operationId": "cd5e16c6-6d4e-4347-86a0-762bdf5b4875",
    "database": "AdventureWorks2",
    "UpdatedAt": "2017-12-09T02:44:26.18",
    "StartedAt": "2017-12-09T02:44:20.743",
    "syncstate": 2,
    "details": null
}
```

Değerleri `syncstate`:

- 0: Çoğaltılıyor. Veritabanı dosyaları hedef klasöre çoğaltılır.
- 1: Dolduruluyor. Veritabanı salt okunur server örnekleri üzerinde rehydrated.
- 2: Tamamlandı. Eşitleme işlemi başarıyla tamamlandı.
- 3: Başarısız. Eşitleme işlemi başarısız oldu.
- 4: Sonlandırılıyor. Eşitleme işlemi tamamlandı ancak temizleme adımları gerçekleştiriyor.

## <a name="code-sample"></a>Kod örneği

İşte başlamanıza yardımcı olur, C# kod örneği [github'da RestApiSample](https://github.com/Microsoft/Analysis-Services/tree/master/RestApiSample).

### <a name="to-use-the-code-sample"></a>Kod örneği kullanmak için

1.  Kopyalayın veya depo indirin. RestApiSample çözümü açın.
2.  Satırı bulun **istemci. BaseAddress =...** ve belirtin, [ana URL](#base-url).

Kod örneği kullanır [hizmet sorumlusu](#service-principal) kimlik doğrulaması.

### <a name="service-principal"></a>Hizmet sorumlusu

Bkz: [hizmet sorumlusu - Azure portal'ı oluşturma](../active-directory/develop/howto-create-service-principal-portal.md) ve [sunucu yöneticisi rolüne hizmet sorumlusu ekleme](analysis-services-addservprinc-admins.md) bir hizmet sorumlusu ayarlamak ve Azure AS gerekli izinler atama hakkında daha fazla bilgi . Adımları tamamladıktan sonra aşağıdaki ek adımları tamamlayın:

1.  Kod örneğinde, bulma **dize yetkilisi =...** , değiştirin **ortak** kuruluşunuzun ile Kiracı kimliği.
2.  Yorum ClientCredential sınıf kimlik bilgileri nesnesinin örneğini oluşturmak için kullanılan şekilde açıklamasını kaldırın. Olun \<uygulama kimliği > ve \<uygulama anahtarı > değerleri güvenli bir şekilde erişilebilir veya sertifika tabanlı kimlik doğrulaması için hizmet sorumluları kullanın.
3.  Örnek uygulamayı çalıştırın.


## <a name="see-also"></a>Ayrıca bkz.

[Örnekler](analysis-services-samples.md)   
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)   


