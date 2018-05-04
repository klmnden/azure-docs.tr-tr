---
title: Azure Analysis Services modelleri için zaman uyumsuz yenileme | Microsoft Docs
description: Zaman uyumsuz yenileme REST API'sini kullanarak kod öğrenin.
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 04/12/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: d1862c5ed83033eb8de74459f26260864c646dfa
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2018
---
# <a name="asynchronous-refresh-with-the-rest-api"></a>REST API ile zaman uyumsuz Yenile
REST çağrıları destekleyen herhangi bir programlama dili kullanarak, Azure Analysis Services tablolu modeller zaman uyumsuz veri yenileme işlemleri gerçekleştirebilir. Bu sorgu genişleme için salt okunur çoğaltmalarını eşitlenmesi içerir. 

Veri yenileme işlemleri veri birimi, bölümler, vb. kullanarak en iyi duruma getirilmesi düzeyi dahil çeşitli Etkenler bir dizi bağlı olarak biraz zaman alabilir. Bu işlemler geleneksel kullanarak gibi varolan yöntemleriyle çağrılan [ZEL](https://docs.microsoft.com/sql/analysis-services/tabular-model-programming-compatibility-level-1200/introduction-to-the-tabular-object-model-tom-in-analysis-services-amo) (tablo nesne modeli) [PowerShell](https://docs.microsoft.com/sql/analysis-services/powershell/analysis-services-powershell-reference) cmdlet'lerini veya [TMSL](https://docs.microsoft.com/sql/analysis-services/tabular-model-scripting-language-tmsl-reference) (tablo modeli Dil) komut dosyası. Ancak, bu yöntemleri genellikle güvenilir olmayan, uzun süre çalışan HTTP bağlantılarını gerektirebilir.

Azure Analysis Services için REST API zaman uyumsuz olarak gerçekleştirilebilmesi veri yenileme işlemlerini sağlar. REST API kullanarak HTTP bağlantılarını uzun süre çalışan istemci uygulamaları gerekli değildir. Otomatik yeniden denemeler ve toplu yürütme gibi güvenilirlik için yerleşik diğer özellikleri de vardır.

## <a name="base-url"></a>Temel URL

Taban URL'si şu biçimdedir:

```
https://<rollout>.asazure.windows.net/servers/<serverName>/models/<resource>/
```

Örneğin, Batı ABD Azure bölgesinde bulunan myserver adlı bir sunucuya AdventureWorks adlı model göz önünde bulundurun. Sunucu adı şudur:

```
asazure://westus.asazure.windows.net/myserver 
```

Bu sunucu adı için temel URL şöyledir:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/ 
```

Temel URL'yi kullanarak, kaynakları ve işlemleri aşağıdaki parametrelere göre eklenebilen: 

![Zaman uyumsuz Yenile](./media/analysis-services-async-refresh/aas-async-refresh-flow.png)

- Biten bir şey **s** koleksiyonudur.
- İle biten bir şey **()** bir işlevdir.
- Başka bir şey bir kaynak/nesnesidir.

Örneğin, yenilemeler koleksiyonda POST fiil yenileme işlemi gerçekleştirmek için kullanabilirsiniz:

```
https://westus.asazure.windows.net/servers/myserver/models/AdventureWorks/refreshes
```

## <a name="authentication"></a>Kimlik Doğrulaması

Tüm çağrılar yetkilendirme üst geçerli bir Azure Active Directory (OAuth 2) belirteçle kimliğinin doğrulanması gerekir ve aşağıdaki gereksinimleri karşılaması gerekir:

- Belirteç, bir kullanıcı belirteci ya da bir uygulama hizmet sorumlusu olmalıdır.
- Belirteç kümesine doğru hedef kitle olmalıdır `https://*.asazure.windows.net`.
- Kullanıcı veya uygulama istenen arama yapmak için yeterli izinleri sunucuda veya model olmalıdır. İzin düzeyi, model veya sunucu üzerindeki yönetim grubu içinde rolleri tarafından belirlenir.

    > [!IMPORTANT]
    > Şu anda **sunucu yöneticisinin** rolü izinleri gerekli.

## <a name="post-refreshes"></a>POST /refreshes

Yenileme işlemi gerçekleştirmek için yeni bir yenileme öğesi koleksiyona eklemek için /refreshes koleksiyonda POST fiil kullanın. Konum üstbilgisi yanıt yenileme kimliği içerir. İstemci uygulaması çıkarın ve durumu zaman uyumsuz olduğu için gerekirse daha sonra denetleyin.

Bir model için bir defada yalnızca bir yenileme işlemi kabul edildi. Geçerli çalışan bir yenileme işlemi ve başka bir gönderilir, 409 çakışma HTTP durum kodu döndürülür.

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
Parametreleri belirterek gerekli değildir. Varsayılan uygulanır.

|Ad  |Tür  |Açıklama  |Varsayılan  |
|---------|---------|---------|---------|
|Tür     |  Enum       |  İşlem türü. Türleri ile TMSL hizalanır [Yenile komut](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/refresh-command-tmsl) türleri: tam, clearValues, hesaplama, dataOnly, otomatik, eklemek ve birleştirin.       |   Otomatik      |
|CommitMode     |  Enum       |  Nesneleri yığınlardaki veya yalnızca tamamlandığında tamamlanan olup olmayacağını belirler. Modları içerir: varsayılan, işlem, partialBatch.  |  işlem       |
|MaxParallelism     |   Int      |  Bu değer paralel işleme komutları çalıştırmak için iş parçacığı sayısını belirler. Bu değer hizalı TMSL ayarlanabilir MaxParallelism özelliğiyle [Sequence komutu](https://docs.microsoft.com/sql/analysis-services/tabular-models-scripting-language-commands/sequence-command-tmsl) veya başka yöntemler kullanarak.       | 10        |
|retryCount    |    Int     |   İşlemi başarısız olmadan önce yeniden deneme sayısını gösterir.      |     0    |
|Nesneler     |   Dizi      |   İşlenecek nesne dizisi. Her bir nesne içerir: "Tablo" tüm tablo ya da "Tablo" ve "Bölüm" bir bölüm işleme sırasında işlerken. Herhangi bir nesnenin belirtilmezse, tüm modeli yenilenir. |   İşlem tüm modeli      |

CommitMode partialBatch için eşittir. Büyük veri ilk bir yük, bunu saat sürebilir olduğunda kullanılır. Bir veya daha fazla toplu başarıyla uyguladıktan sonra yenileme işlemi başarısız olursa, başarılı bir şekilde kaydedilmiş toplu kaydedilmiş kalır (bunu başarıyla tamamlanan toplu geri döner değil).

> [!NOTE]
> Zaman yazma MaxParallelism değeri toplu iş boyutu Dur, ancak bu değeri değiştirebilirsiniz.

## <a name="get-refreshesrefreshid"></a>GET /refreshes/\<refreshId >

Yenileme işlemi durumunu denetlemek için yenileme kimliğine göre GET fiil kullanın Yanıt gövdesi bir örneği burada verilmiştir. İşlemi sürüyor, ise **devam ediyor** durumunda döndürülür.

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

Geçmiş yenileme işlemleri bir model için bir listesini almak için GET fiil /refreshes koleksiyonda kullanın. Yanıt gövdesi bir örneği burada verilmiştir. 

> [!NOTE]
> Zaman yazma, son 30 gün yenileme işlemlerinin depolanan ve döndürdü, ancak bu sayıyı değiştirebilirsiniz.

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

Devam eden yenileme işlemini iptal etmek için yenileme kimliğine göre DELETE fiil kullanın

## <a name="post-sync"></a>POST Sync

Yenileme işlemleri gerçekleştirilecek sorgu genişleme için yinelemelerle yeni verileri eşitlemek gerekli olabilir. Bir model için bir eşitleme işlemi gerçekleştirmek için POST fiil Sync işlevini kullanın. Konum üstbilgisi yanıt eşitleme işlemi kimliği içerir.

## <a name="get-sync-status"></a>Sync durumunu Al

Bir eşitleme işlemi durumunu denetlemek için işlem kimliği bir parametre olarak Geçiren GET fiil kullanın. Yanıt gövdesi bir örneği burada verilmiştir:

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

İçin değerler `syncstate`:

- 0: çoğaltılıyor. Veritabanı dosyalarını bir hedef klasöre çoğaltılır.
- 1: yeniden. Veritabanı salt okunur sunucu örnekleri üzerinde rehydrated.
- 2: tamamlandı. Eşitleme işlemi başarıyla tamamlandı.
- 3: başarısız oldu. Eşitleme işlemi başarısız oldu.
- 4: sonlandırılıyor. Eşitleme işlemi tamamlandı ancak temizleme adımları gerçekleştiriyor.

## <a name="code-sample"></a>Kod örneği

İşte, başlamanıza C# kod örneği [github'da RestApiSample](https://github.com/Microsoft/Analysis-Services/tree/master/RestApiSample).

### <a name="to-use-the-code-sample"></a>Kod örneğini kullanmak için

1.  Kopyalama veya deposuna yükleyin. RestApiSample çözümü açın.
2.  Satırı bulun **istemci. BaseAddress =...** ve sağlamak, [ana URL](#base-url).

Kod örneği kullanabilirsiniz etkileşimli oturum açma, kullanıcı adı/parola veya [hizmet sorumlusu](#service-principal).

#### <a name="interactive-login-or-usernamepassword"></a>Etkileşimli oturum açma veya kullanıcı adı/parola

Bu form kimlik doğrulaması, bir Azure uygulama API gerekli izinleri atanmış oluşturulabilir gerektirir. 

1.  Azure portalında tıklatın **kaynak oluşturma** > **Azure Active Directory** > **uygulama kayıtlar**  >   **Yeni uygulama kaydı**.

    ![Yeni uygulama kaydı](./media/analysis-services-async-refresh/aas-async-app-reg.png)


2.  İçinde **oluşturma**, bir ad yazın, seçin **yerel** uygulama türü. İçin **yeniden yönlendirme URI'si**, girin **urn: ietf:wg:oauth:2.0:oob**ve ardından **oluşturma**.

    ![Ayarlar](./media/analysis-services-async-refresh/aas-async-app-reg-name.png)

3.  Uygulamanızı seçin ve ardından kopyalayın ve kaydedin **uygulama kimliği**.

    ![Uygulama Kimliğini kopyalayın](./media/analysis-services-async-refresh/aas-async-app-id.png)

4.  İçinde **ayarları**, tıklatın **gerekli izinleri** > **Ekle**.

    ![API erişimi ekler](./media/analysis-services-async-refresh/aas-async-add.png)

5.  İçinde **bir API seçin**, türü **Azure Analysis Services** Ara kutusuna ve ardından seçin.

    ![API seçin](./media/analysis-services-async-refresh/aas-async-select-api.png)

6.  Seçin **okuma ve yazma tüm modelleri**ve ardından **seçin**. Her ikisi de seçildiğinde tıklatın **Bitti** izinleri eklemek için. Yaymak için birkaç dakika sürebilir.

    ![Okuma seçin ve tüm modelleri yazma](./media/analysis-services-async-refresh/aas-async-select-read.png)

7.  Kod örneğinde, bulma **UpdateToken()** yöntemi. Bu yöntem içeriğini inceleyin.
8.  Bul **ClientID dize =...** ve ardından girin **uygulama kimliği** 3. adımdan kopyalanır.
9.  Örnek uygulamayı çalıştırın.

#### <a name="service-principal"></a>Hizmet sorumlusu

Bkz: [oluşturma hizmet sorumlusu - Azure portalında](../azure-resource-manager/resource-group-create-service-principal-portal.md) ve [için sunucu yöneticisi rolünün bir hizmet sorumlusu ekleme](analysis-services-addservprinc-admins.md) bir hizmet sorumlusu ayarlamak ve Azure AS gerekli izinler atama hakkında daha fazla bilgi için . Adımları tamamladıktan sonra aşağıdaki ek adımları tamamlayın:

1.  Kod örneğinde, bulma **dize yetkilisi =...** , yerine **ortak** kuruluşunuzun ile Kiracı kimliği.
2.  Açıklama/ClientCredential sınıfı ki nesne örneğini oluşturmak için kullanılan şekilde açıklamadan çıkarın. Olun \<uygulama kimliği > ve \<uygulama anahtarı > değerleri güvenli bir şekilde erişilen veya sertifika tabanlı kimlik doğrulaması için hizmet asıl adı kullanın.
3.  Örnek uygulamayı çalıştırın.


## <a name="see-also"></a>Ayrıca bkz.

[Örnekler](analysis-services-samples.md)   
[REST API](https://docs.microsoft.com/rest/api/analysisservices/servers)   


