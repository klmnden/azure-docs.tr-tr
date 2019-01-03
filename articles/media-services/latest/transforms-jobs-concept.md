---
title: Dönüşümler ve işler Azure Media Services | Microsoft Docs
description: Media Services hizmetini kullanırken, dönüşüm kurallar veya videolarınızı işlemek için özellikleri tanımlamak için oluşturmanız gerekir. Bu makalede, dönüştürme nedir ve nasıl kullanılacağını hakkında genel bir bakış sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 12/20/2018
ms.author: juliako
ms.openlocfilehash: 95079813cf3ade41d17393168116e4767ca26e99
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2018
ms.locfileid: "53742788"
---
# <a name="transforms-and-jobs"></a>Dönüşümler ve İşler
 
Kullanım [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms) kodlama veya videoları analiz için ortak görevler yapılandırmak için. Her **dönüştürme** bir tarif veya bir iş akışı, video veya ses dosyalarını işlemek için görevler açıklanmaktadır. 

A [iş](https://docs.microsoft.com/rest/api/media/jobs) uygulamak için Azure Media Services için fiili istek **dönüştürme** belirli bir giriş video veya ses içeriği için. **İş** konumun video giriş ve çıkış konumunu gibi bilgileri belirtir. Giriş video kullanarak konumu belirtebilirsiniz: HTTPS URL'leri, SAS URL'lerini veya [medya Hizmetleri varlıklar](https://docs.microsoft.com/rest/api/media/assets).  

## <a name="typical-workflow"></a>Tipik iş akışı

1. Dönüşüm oluşturma 
2. Dönüşüm altında işlerini gönderme 
3. Liste dönüşümler 
4. Gelecekte kullanmayı planlamıyorsanız bir dönüşüm silin. 

Tüm videolarınızı ilk karesine – bir küçük resim olarak çıkarmak istediğinizi varsayalım, uygulamanız gereken adımları şunlardır: 

1. Tarif veya videolarınızı işlemek için bir kural tanımlama - "ilk çerçeve video küçük resmi kullan". 
2. Her video için hizmet söyleyin: 
    1. Bu video nerede bulacağını,  
    2. Çıktı küçük resim görüntüsü yazmak yer. 

A **dönüştürme** tarif sonra (adım 1) oluşturun ve bu tarif (Adım 2) kullanarak göndermek yardımcı olur.

## <a name="transforms"></a>Dönüştürmeler

Media Services hesabınızı v3 API kullanarak doğrudan ya da yayımlanan SDK'ları kullanarak dönüştürmeler oluşturabilirsiniz. Media Services hesabınızı oluşturmak ve dağıtmak için Resource Manager şablonları kullanabilmeniz için API Azure Resource Manager tarafından yönetilen Media Services v3 dönüştürür. Rol tabanlı erişim denetimi, dönüşümler erişimi kilitlemek için kullanılabilir.

### <a name="transform-definition"></a>Tanımı dönüştürme

Aşağıdaki tabloda, dönüşümün özelliklerini gösterir ve bunların tanımlarının verir.

|Ad|Açıklama|
|---|---|
|Kimlik|Kaynak için tam kaynak kimliği.|
|ad|Kaynak adı.|
|Properties.Created |UTC tarihi ve saati ne zaman dönüşümü oluşturuldu, buna ' YYYY-AA-ssZ ' biçimi.|
|Properties.Description |Dönüşüm isteğe bağlı ayrıntılı açıklaması.|
|properties.lastModified |UTC tarihi ve saati ne zaman dönüşümü son güncelleştirildi, buna ' YYYY-AA-ssZ ' biçimi.|
|Properties.Outputs |Dönüştürme üretmesi gereken bir veya daha fazla TransformOutputs dizisi.|
|type|Kaynak türü.|

Tam tanımı için bkz [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms).

Yukarıda açıklandığı gibi bir dönüştürme tarif veya videolarınızı işlemek için kural oluşturmanıza yardımcı olur. Tek bir dönüştürme, birden fazla kural uygulayabilirsiniz. Örneğin, bir dönüştürme belirli bir bit hızı anda bir MP4 dosyasına her video kodlanmış ve bir küçük resim videonun ilk çerçevesinden oluşturulmasını belirtebilirsiniz. Dönüştürme, dahil etmek istediğiniz her bir kural için bir TransformOutput girdi eklersiniz.

> [!NOTE]
> Dönüşümler tanım güncelleştirme işlemi desteklerken, bir değişiklik yapmadan önce tüm devam eden işleri tamamlandığından emin olmak için ilgileniriz. Genellikle, açıklamayı değiştirmek için mevcut bir dönüştürme güncelleştirin veya temel alınan TransformOutputs önceliklerini değiştirmek. Ardından tarif yeniden yazmak isterseniz, yeni bir dönüştürme oluşturursunuz.

## <a name="jobs"></a>İşler

Dönüşüm oluşturulduktan sonra Media Services API'leri ve yayımlanan SDK'ları hiçbirini kullanarak işleri gönderebilirsiniz. İşlerin durumunu ve ilerleme durumunu izleme olayları Event Grid ile tarafından alınabilir. Daha fazla bilgi için [EventGrid kullanarak olayları izleme](job-state-events-cli-how-to.md ).

### <a name="jobs-definition"></a>İş tanımı

Aşağıdaki tabloda, işleri'nin özelliklerini gösterir ve bunların tanımlarının verir.

|Ad|Açıklama|
|---|---|
|id|Kaynak için tam kaynak kimliği.|
|ad   |Kaynak adı.|
|properties.correlationData |Müşteri, iş ve JobOutput durumu bir parçası olarak döndürülen bağıntı veri (sabit) değişiklik bildirimleri sağlanır. Daha fazla bilgi için [olay şemaları](media-services-event-schemas.md)<br/><br/>Özelliği, Media Services'ın çok kiracılı kullanım için de kullanılabilir. Kiracı kimlikleri işleri koyabilirsiniz. |
|Properties.Created |UTC tarihi ve saati ne zaman iş oluşturuldu, buna ' YYYY-AA-ssZ ' biçimi.|
|Properties.Description |İsteğe bağlı müşteri tarafından sağlanan iş açıklaması.|
|Properties.input|İş için giriş.|
|properties.lastModified    |UTC tarihi ve saati ne zaman işin en son güncelleştirildi, buna ' YYYY-AA-ssZ ' biçimi.|
|Properties.Outputs|İş için çıktı.|
|Properties.priority    |Öncelikli iş işlenmesi gerekir. Daha yüksek öncelikli işlerin düşük öncelikli işler önce işlenir. Aksi durumda, varsayılan normal kümesidir.|
|Properties.State   |İşin geçerli durumu.|
|type   |Kaynak türü.|

Tam tanımı için bkz [işleri](https://docs.microsoft.com/rest/api/media/jobs).

> [!NOTE]
> İş tanımı bir güncelleştirme işlemi desteklerken, iş gönderildikten sonra değiştirilebilir özellikler yalnızca açıklama ve öncelik ' dir. Ayrıca, bir değişiklik önceliği yalnızca proje yine de kuyruğa alınmış bir durumda ise etkili olur. İş işleme başladı ya da sona önceliğini değiştirme etkisi yoktur.

### <a name="pagination"></a>Sayfalandırma

Sayfalandırma işleri, Media Services v3 sürümünde desteklenir.

> [!TIP]
> Koleksiyon listeleme ve belirli bir sayfa bağımlı olmadan her zaman sonraki bağlantısını kullanmanız gerekir.

Sorgu yanıtına fazla öğe içeriyorsa, hizmet döndürür bir "\@odata.nextLink" sonraki sonuç sayfasını alınacağı özellik. Bu kullanılabilir sonuç kümesinin tamamı aracılığıyla sayfası. Sayfa boyutunu yapılandıramazsınız. 

İşleri oluşturduysanız veya çalışırken disk belleği koleksiyonu sildi (Bu değişiklikleri indirilmedi koleksiyonu içinde parçasıysa.) değişiklikler döndürülen sonuçlarda yansıtılır 

Aşağıdaki C# örnek hesabında, işlere numaralandırma gösterir.

```csharp            
List<string> jobsToDelete = new List<string>();
var pageOfJobs = client.Jobs.List(config.ResourceGroup, config.AccountName, "Encode");

bool exit;
do
{
    foreach (Job j in pageOfJobs)
    {
        jobsToDelete.Add(j.Name);
    }

    if (pageOfJobs.NextPageLink != null)
    {
        pageOfJobs = client.Jobs.ListNext(pageOfJobs.NextPageLink);
        exit = false;
    }
    else
    {
        exit = true;
    }
}
while (!exit);

```

Diğer örnekler için bkz [işler - liste](https://docs.microsoft.com/rest/api/media/jobs/list)


## <a name="next-steps"></a>Sonraki adımlar

[Stream videoları](stream-files-dotnet-quickstart.md)
