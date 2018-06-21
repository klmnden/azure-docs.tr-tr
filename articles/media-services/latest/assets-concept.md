---
title: Azure Media Services varlıkları | Microsoft Docs
description: Bu makalede varlıklar nelerdir ve Azure Media Services tarafından nasıl kullanıldıkları hakkında bir açıklama sağlar.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 03/19/2018
ms.author: juliako
ms.openlocfilehash: 61555eb6cca6995215ce43051abbda9aa43539ec
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36284847"
---
# <a name="assets"></a>Varlıklar

Bir **varlık** dijital dosyaları (video, ses, görüntüler, küçük resim koleksiyonları, metin parçaları ve kapalı açıklamalı alt yazı dosyaları dahil) ve bu dosyalar hakkındaki meta verileri içerir. Dijital dosyalar bir varlığa karşıya yükledikten sonra medya kodlama ve iş akışları akış Hizmetleri'nde kullanılabilir.

Bir varlık, bir blob kapsayıcısında eşlenmiş [Azure depolama hesabı](storage-account-concept.md) ve varlık içindeki dosyalara kapsayıcıdaki blok blobları olarak depolanır. Depolama SDK istemcileri kullanarak kapsayıcılarında varlık dosyaları ile etkileşim kurabilir.

Azure Media Services hesabı genel amaçlı v2 kullandığında Blob katmanları destekler (GPv2) depolama. GPv2 ile soğutma dosyaları veya soğuk depolama taşıyabilirsiniz. Soğuk depolama (örneğin, bunlar kodlanmıştır.) sonra artık gerekmediğinde kaynak dosyalarını arşivlemek için uygundur

Media Services v3 iş giriş varlıklar veya http (s) URL'leri oluşturulabilir. İşiniz için bir giriş olarak kullanılabilen bir varlık oluşturmak için bkz: [yerel bir dosyadan bir iş girişi oluşturma](job-input-from-local-file-how-to.md).

Ayrıca, bilgiyi [Media Services depolama hesaplarında](storage-account-concept.md) ve [dönüşümler ve işleri](transform-concept.md).

## <a name="asset-definition"></a>Varlık tanımı

Aşağıdaki tabloda, varlığın özelliklerini gösterir ve tanımlarını sağlar.

|Ad|Tür|Açıklama|
|---|---|---|
|Kimlik|dize|Kaynak için tam kaynak kimliği.|
|ad|dize|Kaynağın adı.|
|properties.alternateId |dize|Varlık alternatif kimliği.|
|properties.assetId |dize|Varlık Kimliği|
|Properties.Container |dize|Varlık blob kapsayıcısının adı.|
|Properties.Created |dize|Varlık oluşturma tarihi.|
|Properties.Description |dize|Varlık açıklaması.|
|properties.lastModified |dize|Son değiştirilme tarihi varlığın.|
|properties.storageAccountName |dize|Depolama hesabı adı.|
|properties.storageEncryptionFormat |AssetStorageEncryptionFormat |Varlık şifreleme biçimi. Bir None veya MediaStorageEncryption.|
|type|dize|Kaynak türü.|

Tam, bkz. [varlıklar](https://docs.microsoft.com/rest/api/media/assets).

## <a name="filtering-ordering-paging"></a>Filtreleme, sıralama, disk belleği

Media Services varlıkları için aşağıdaki OData sorgu seçeneklerini destekler: 

* $filter 
* $orderby 
* $top 
* $skiptoken 

### <a name="filteringordering"></a>Filtreleme ve sıralama

Aşağıdaki tabloda, bu seçenekler için varlık özellikleri nasıl uygulanabilir gösterilmektedir: 

|Ad|Filtre|Sıra|
|---|---|---|
|Kimlik|Desteklediği Özel Uygulamalar:<br/>Eşittir<br/>Şu değerden fazla:<br/>Küçüktür|Desteklediği Özel Uygulamalar:<br/>Artan<br/>Azalan|
|ad|||
|properties.alternateId |Desteklediği Özel Uygulamalar:<br/>Eşittir||
|properties.assetId |Desteklediği Özel Uygulamalar:<br/>Eşittir||
|Properties.Container |||
|Properties.Created|Desteklediği Özel Uygulamalar:<br/>Eşittir<br/>Şu değerden fazla:<br/>Küçüktür|Desteklediği Özel Uygulamalar:<br/>Artan<br/>Azalan|
|Properties.Description |||
|properties.lastModified |||
|properties.storageAccountName |||
|properties.storageEncryptionFormat | ||
|type|||

Aşağıdaki C# örnek filtreleri oluşturma tarihi:

```csharp
var odataQuery = new ODataQuery<Asset>("properties/created lt 2018-05-11T17:39:08.387Z");
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName, odataQuery);
```

### <a name="pagination"></a>Sayfalandırma

Sayfa numaralandırma her dört etkin sıralamalar için desteklenir. 

Bir sorgu yanıtı birçok (şu anda üzerinden 1000) içeriyorsa, öğeler, hizmet döndüren bir "\@odata.nextLink" sonraki sonuç sayfasını alınacağı özellik. Bu kullanılabilir tüm sonuç kümesini aracılığıyla sayfasına. Sayfa boyutunu kullanıcı tarafından yapılandırılabilir değildir. 

Varlıklar oluşturduysanız veya disk belleği koleksiyonu aracılığıyla sırasında silinmiş (Bu değişiklikleri indirilmedi koleksiyonu içinde parçasıysa.) değişiklikler döndürülen sonuçlarda yansıtılır 

Aşağıdaki C# örnek hesaptaki tüm varlıklar aracılığıyla listeleme gösterilmektedir.

```csharp
var firstPage = await MediaServicesArmClient.Assets.ListAsync(CustomerResourceGroup, CustomerAccountName);

var currentPage = firstPage;
while (currentPage.NextPageLink != null)
{
    currentPage = await MediaServicesArmClient.Assets.ListNextAsync(currentPage.NextPageLink);
}
```

Diğer örnekler için bkz: [varlıklar - liste](https://docs.microsoft.com/rest/api/media/assets/list)


## <a name="storage-side-encryption"></a>Depolama tarafı şifreleme

Varlıklarınızı REST korumak için depolama Tarafı Şifrelemesi tarafından varlıklar şifrelenmelidir. Aşağıdaki tabloda, depolama tarafı şifreleme Media Services ile nasıl çalıştığı gösterilmektedir:

|şifreleme seçeneği|Açıklama|Media Services v2|Media Services v3|
|---|---|---|---|
|Depolama şifrelemesi medya Hizmetleri|Medya Hizmetleri tarafından yönetilen AES-256 şifrelemesi, anahtar|Desteklenen<sup>(1)</sup>|Desteklenmeyen<sup>(2)</sup>|
|[Rest verileri için depolama hizmeti şifrelemesi](https://docs.microsoft.com/azure/storage/common/storage-service-encryption)|Sunucu tarafı şifreleme Azure Storage tarafından sunulan anahtar Azure veya müşteri tarafından yönetilen|Desteklenen|Desteklenen|
|[İstemci tarafı şifreleme depolama](https://docs.microsoft.com/azure/storage/common/storage-client-side-encryption)|Azure depolama, anahtar kasası müşteri tarafından yönetilen anahtar tarafından sunulan istemci tarafı şifreleme|Desteklenmiyor|Desteklenmiyor|

<sup>1</sup> sırada Media Services, içeriği işlenmesini desteklemez, temiz/herhangi bir biçimde şifreleme olmadan, bunun nedenle önerilmez.

<sup>2</sup> Media Services'ın v3 depolama şifreleme (AES 256 şifreleme), yalnızca Media Services v2 ile varlıklarınızı oluşturulduğunda için geriye dönük uyumluluk desteklenir. Var olan depolama ile v3 çalışır anlamı varlıklar şifrelenmiş ancak yenilerini oluşturulmasına izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Bir dosyayı akışa alma](stream-files-dotnet-quickstart.md)
