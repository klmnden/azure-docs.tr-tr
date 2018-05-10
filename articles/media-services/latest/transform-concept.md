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
ms.openlocfilehash: d256d87548d54951cb77beffb88bba26a1a3de49
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="transforms-and-jobs"></a>Dönüşümler ve işleri

## <a name="overview"></a>Genel Bakış 

Azure Media Services REST API (v3) en son sürümünü kodlama ve/veya adlı videoları, çözümleme için yeni bir şablonlu iş akışı kaynak tanıtır bir **dönüştürme**. **Dönüşümleri** kodlama veya analying videolar için ortak görevler yapılandırmak için kullanılabilir. Her **dönüştürme** basit bir iş akışı, video ve ses dosyalarını işlemek için görevleri açıklar. 

**Dönüştürme** nesnesidir tarif ve **iş** , uygulamak için Azure Media Services için gerçek isteği **dönüştürme** belirli bir giriş ses veya video içerik. **İş** konumun video giriş ve çıkış konumunu gibi bilgileri belirtir. Video kullanmanın konum belirtebilirsiniz: http (s) URL'ler, SAS URL'leri veya dosyaları yerel olarak veya Azure Blob Depolama alanında bulunan bir yolu. Azure Media Services hesabınızı en fazla 100 dönüşümler sahip ve bu dönüşümler altında işleri göndermek. Sonra iş durumu değişiklikleri gibi olayları doğrudan Azure olay kılavuz bildirim sistemiyle tümleştirme bildirimleri kullanarak abone olabilirsiniz. 

Bu API Azure Resource Manager tarafından yönetilen olduğundan, Resource Manager şablonları oluşturmak ve Media Services hesabınızı dönüşümler dağıtmak için kullanabilirsiniz. Rol tabanlı erişim denetimi de bu API kaynak düzeyinde dönüşümler gibi belirli kaynaklara erişim kilitlemenize olanak tanıyan ayarlanabilir.

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
