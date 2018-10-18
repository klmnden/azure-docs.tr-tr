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
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 214d4d3d11255e417f3df1e5f6e648b2a30225ea
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49377317"
---
# <a name="transforms-and-jobs"></a>Dönüşümler ve işler

## <a name="overview"></a>Genel Bakış 

Azure Media Services REST API'si (v3) en son sürümünü kodlama ve/veya adlı videoları analiz için yeni bir şablonlu iş akışı kaynak tanıtır bir **dönüştürme**. **Dönüşümler** kodlama veya analying videolar için ortak görevler yapılandırmak için kullanılabilir. Her **dönüştürme** basit bir iş akışı, video veya ses dosyalarını işlemek için görevler açıklanmaktadır. 

**Dönüştürme** tarif nesnedir ve **iş** gerçek isteği, geçerli Azure Media Services için **dönüştürme** belirli bir giriş video veya ses içeriği için. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir. Video kullanarak konumu belirtebilirsiniz: http (s) URL'leri, SAS URL'lerini veya dosyaları yerel olarak veya Azure Blob Depolama alanında bulunan bir yolu. Azure Media Services hesabınızda en fazla 100 dönüşümler sahip ve bu dönüşüm altında işleri göndermek. Ardından iş durumu değişiklikleri gibi olayları kullanarak doğrudan Azure Event Grid bildirim sistemiyle tümleştirmek bildirimleri, abone olabilirsiniz. 

Bu API, Azure Resource Manager tarafından yönetilen bu yana, Resource Manager şablonları oluşturmak ve Media Services hesabınızı dönüşümler dağıtmak için kullanabilirsiniz. Rol tabanlı erişim denetimi aynı zamanda bu API, kaynak düzeyinde dönüşümleri gibi belirli kaynaklara erişim kilitlemenize olanak tanıyan ayarlanabilir.

## <a name="transform-definition"></a>Tanımı dönüştürme

Aşağıdaki tabloda, dönüşümün özelliklerini gösterir ve bunların tanımlarının verir.

|Ad|Tür|Açıklama|
|---|---|---|
|Kimlik|dize|Kaynak için tam kaynak kimliği.|
|ad|dize|Kaynak adı.|
|Properties.Created |dize|UTC tarihi ve saati ne zaman dönüşümü oluşturuldu, buna ' YYYY-AA-ssZ ' biçimi.|
|Properties.Description |dize|Dönüşüm isteğe bağlı ayrıntılı açıklaması.|
|properties.lastModified |dize|UTC tarihi ve saati ne zaman dönüşümü son güncelleştirildi, buna ' YYYY-AA-ssZ ' biçimi.|
|Properties.Outputs |TransformOutput]|Dönüştürme üretmesi gereken bir veya daha fazla TransformOutputs dizisi.|
|type|dize|Kaynak türü.|

Tam tanımı için bkz [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms).

## <a name="job-definition"></a>İş tanımı

Aşağıdaki tabloda işin özelliklerini gösterir ve bunların tanımlarının sağlar.

|Ad|Tür|Açıklama|
|---|---|---|
|Kimlik|dize|Kaynak için tam kaynak kimliği.|
|ad|dize|Kaynak adı.|
|Properties.Created |dize|UTC tarihi ve saati ne zaman dönüşümü oluşturuldu, buna ' YYYY-AA-ssZ ' biçimi.|
|Properties.Description |dize|İşin isteğe bağlı bir ayrıntılı açıklama.|
|properties.lastModified |dize|UTC tarihi ve saati ne zaman dönüşümü son güncelleştirildi, buna ' YYYY-AA-ssZ ' biçimi.|
|Properties.Outputs |JobOutput []: JobOutputAsset] |İş için çıktı.|
|Properties.priority |Öncelik |Öncelikli iş işlenmesi gerekir. Daha yüksek öncelikli işlerin düşük öncelikli işler önce işlenir. Aksi durumda, varsayılan normal kümesidir.
|Properties.State |JobState |İşin geçerli durumu.
|type|dize|Kaynak türü.|

Tam tanımı için bkz [işleri](https://docs.microsoft.com/rest/api/media/jobs).

## <a name="typical-workflow-and-example"></a>Tipik iş akışı ve örnek

1. Dönüşüm oluşturma 
2. Dönüşüm altında işlerini gönderme 
3. Liste dönüşümler 
4. Bu "yemek" gelecekte kullanmayı planlamıyorsanız bir dönüşüm silin. 

Tüm videolarınızı ilk karesine – bir küçük resim olarak çıkarmak istediğinizi varsayalım, uygulamanız gereken adımları şunlardır: 

1. Kural işleme – örneğin tanımlamak, küçük resim olarak videonun ilk çerçeve kullanın 
2. Sonra hizmet giriş olarak sahip olduğunuz her video için hizmet söyleyin: 
    1. Bu videoda, nerede bulacağını ve 
    2. Yazılacağı çıkış – Örneğin, küçük resim görüntüsü 

## <a name="next-steps"></a>Sonraki adımlar

[Stream videoları](stream-files-dotnet-quickstart.md)
