---
title: Dönüştüren ve işleri Azure Media Services | Microsoft Docs
description: Media Services kullanırken kurallar veya videolarınızı işlemeye belirtimleri tanımlamak için bir dönüşüm oluşturmanız gerekir. Bu makalede dönüştürme nedir ve nasıl kullanılacağını hakkında genel bir bakış sağlar.
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
ms.openlocfilehash: b755e0573098d3dbed1bea18a40af634be609f76
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
ms.locfileid: "34272089"
---
# <a name="transforms-and-jobs"></a>Dönüşümler ve işleri

## <a name="overview"></a>Genel Bakış 

Azure Media Services REST API (v3) en son sürümünü kodlama ve/veya adlı videoları, çözümleme için yeni bir şablonlu iş akışı kaynak tanıtır bir **dönüştürme**. **Dönüşümleri** kodlama veya analying videolar için ortak görevler yapılandırmak için kullanılabilir. Her **dönüştürme** basit bir iş akışı, video ve ses dosyalarını işlemek için görevleri açıklar. 

**Dönüştürme** nesnesidir tarif ve **iş** , uygulamak için Azure Media Services için gerçek isteği **dönüştürme** belirli bir giriş ses veya video içerik. **İş** giriş videosunun konumu ve çıktının konumu gibi bilgileri belirtir. Video kullanmanın konum belirtebilirsiniz: http (s) URL'ler, SAS URL'leri veya dosyaları yerel olarak veya Azure Blob Depolama alanında bulunan bir yolu. Azure Media Services hesabınızı en fazla 100 dönüşümler sahip ve bu dönüşümler altında işleri göndermek. Sonra iş durumu değişiklikleri gibi olayları doğrudan Azure olay kılavuz bildirim sistemiyle tümleştirme bildirimleri kullanarak abone olabilirsiniz. 

Bu API Azure Resource Manager tarafından yönetilen olduğundan, Resource Manager şablonları oluşturmak ve Media Services hesabınızı dönüşümler dağıtmak için kullanabilirsiniz. Rol tabanlı erişim denetimi de bu API kaynak düzeyinde dönüşümler gibi belirli kaynaklara erişim kilitlemenize olanak tanıyan ayarlanabilir.

## <a name="transform-definition"></a>Tanımı dönüştürmesi

Aşağıdaki tabloda dönüşümün özelliklerini gösterir ve tanımlarını sağlar.

|Ad|Tür|Açıklama|
|---|---|---|
|Kimlik|dize|Kaynak için tam kaynak kimliği.|
|ad|dize|Kaynağın adı.|
|Properties.Created |dize|UTC tarihi ve saati zaman dönüştürme oluşturuldu, buna ' YYYY-MM-: ssZ ' biçimi.|
|Properties.Description |dize|İsteğe bağlı ayrıntılı bir açıklaması Dönüştür.|
|properties.lastModified |dize|UTC tarihi ve saati zaman dönüştürme en son güncelleştirilen, buna ' YYYY-MM-: ssZ ' biçimi.|
|Properties.Outputs |TransformOutput]|Dönüştürme oluşturması bir veya daha fazla TransformOutputs dizisi.|
|type|dize|Kaynak türü.|

Tam, bkz. [dönüştüren](https://docs.microsoft.com/rest/api/media/transforms).

## <a name="job-definition"></a>İş tanımı

Aşağıdaki tabloda, işin özelliklerini gösterir ve tanımlarını sağlar.

|Ad|Tür|Açıklama|
|---|---|---|
|Kimlik|dize|Kaynak için tam kaynak kimliği.|
|ad|dize|Kaynağın adı.|
|Properties.Created |dize|UTC tarihi ve saati zaman dönüştürme oluşturuldu, buna ' YYYY-MM-: ssZ ' biçimi.|
|Properties.Description |dize|İsteğe bağlı ayrıntılı bir açıklaması iş.|
|properties.lastModified |dize|UTC tarihi ve saati zaman dönüştürme en son güncelleştirilen, buna ' YYYY-MM-: ssZ ' biçimi.|
|Properties.Outputs |JobOutput []: JobOutputAsset] |İş için çıkarır.|
|Properties.priority |Öncelik |Öncelik iş işlenmesi. Yüksek öncelikli işler düşük öncelikli işler önce işlenir. Aksi durumda, varsayılan normal olarak ayarlanmış.
|Properties.State |JobState |İşin geçerli durumu.
|type|dize|Kaynak türü.|

Tam, bkz. [işleri](https://docs.microsoft.com/rest/api/media/jobs).

## <a name="typical-workflow-and-example"></a>Normal iş akışı ve örnek

1. Bir dönüşüm oluşturun 
2. Dönüşüm altında işleri gönderme 
3. Liste dönüşümler 
4. Bu "tarif" gelecekte kullanmayı planlamıyorsanız bir dönüşüm silin. 

Tüm videolarınızı ilk çerçeve küçük resim – olarak ayıklamak istediğinizi varsayalım adımlar şunlardır: 

1. Videonun ilk çerçevesi küçük resim olarak kullanmak, kural işleme için– örneğin tanımlayın 
2. Ardından, hizmet giriş olarak sahip her video için hizmet söyleyin: 
    1. Bu görüntü, nerede bulacağını ve 
    2. Çıktı – Örneğin, küçük resim yazılacağı 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Akış video dosyaları](stream-files-dotnet-quickstart.md)
