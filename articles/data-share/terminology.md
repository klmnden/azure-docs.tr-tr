---
title: Azure veri paylaşımı Önizleme terminolojisi
description: Azure veri paylaşımı Önizleme terminolojisi
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 4e7db84666b9d3786c3fc25e3653d24d0b95f2e4
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789234"
---
# <a name="azure-data-share-preview-concepts"></a>Azure veri paylaşımı Önizleme kavramları 

Azure veri paylaşımı Önizleme veri paylaşma ile ilgili bazı yeni terimlerle tanıtır. Bu makalede, sık kullanılan bazı görebileceğiniz hizmet kullanılan terimler açıklanmaktadır. 

## <a name="data-provider"></a>Veri sağlayıcısı

Bir veri sağlayıcısı veri tüketicileriyle paylaşan kuruluştur. Genellikle veri sağlayıcısı sahibi veya bir veri sorumlusu olabilir. Veri sağlayıcıları, çeşitli veri türlerine paylaşmak istiyorsunuz. Örnek bir veri sağlayıcısı paylaşmak isteyebileceğiniz veri noktası satış veya zaman serisi verileri gibi ham verileri verilebilir. Bir veri sağlayıcısı ayrıca analiz ve Öngörüler zaten içeren önceden işlenen, seçkin veri paylaşmak isteyebilirsiniz. 

## <a name="data-consumer"></a>Veri tüketicisi 

Veri tüketicisi, verileri bir veri sağlayıcısı'ndan alma kuruluştur. Veri tüketicisi, Öngörüler, kullanıcıların kendi verilerini paylaşılan verilerle birleştirmek isteyen. Bazı durumlarda, veri tüketici zaten işlenmiş olan verileri alıyor olabilir. 

## <a name="data-share"></a>Veri paylaşımı

Bir veri paylaşımı, tek bir varlık olarak paylaşılan veri kümeleri grubudur. Veri kümeleri, Azure veri paylaşımı tarafından desteklenen Azure veri kaynakları bir dizi olabilir. Şu anda Azure veri paylaşımı, Azure Blob Depolama ve Azure Data Lake Store destekler. 

## <a name="share-subscription"></a>Abonelik paylaşımı 

Bir veri tüketici veri sağlayıcısı veri paylaşımı davetini kabul ettiğinde paylaşan bir abonelik oluşturulur. Veri sağlayıcıları, etkin paylaşımı abonelikleri giderek görüntüleyebilirsiniz **gönderilen paylaşımları** Azure verilerini hesabı paylaşmak ve seçerek **paylaşmak abonelikleri**.

Bir veri tüketici giderek bunların etkin paylaşımı aboneliğinin geçerli olup olmadığını denetlemek **alınan paylaşımları** ve bunların alınan paylaşımları durumunu görüntüleme. 

## <a name="snapshot"></a>Anlık Görüntü

Bunlar veri Paylaşım daveti kabul ettiğinizde, bir anlık görüntü veri tüketici tarafından oluşturulabilir. Bunlar daveti kabul ettiğinizde, kendileriyle paylaşılan verilerin tam bir anlık görüntüsünü tetikleme yapılmasını sağlayabilirsiniz. Anlık görüntü verileri, veri tüketici anlık görüntü oluşturulan bir noktada bir kopyasıdır. 

İki çeşit anlık görüntü - tam ve artımlı vardır. Tam bir anlık görüntü veri paylaşımı içinde tüm verileri içerir. Artımlı anlık görüntüsünü güncelleştirilen/son anlık görüntünün tetiklendi bu yana eklenmiş tüm verileri içerir. 

## <a name="snapshot-settings-in-azure-data-share"></a>Azure veri paylaşımı anlık görüntüsü ayarları
 
Bir veri sağlayıcısı, veri paylaşımı için bir anlık görüntü ayarını etkinleştirebilirsiniz. Bu ayar, veri tüketicileri oluşunca artımlı güncelleştirmeleri almasını sağlar. Veri sağlayıcısı paylaşılmış olan veri güncelleştirmeleri almak için veri tüketicilerinin istiyorsanız bu ayarı etkinleştirilmelidir. 

Bu ayar bir veri sağlayıcısı sağlar, yinelenme aralığı seçilebilir. Yinelenme aralığı, günlük veya saatlik olabilir. 

Veri tüketicisi için önce yeni bir anlık görüntü oluşturulan bu yana değişmiş verileri içeren artımlı güncelleştirmeleri almak için bu anlık görüntü zamanlamayı katılımı seçeneği vardır. 

## <a name="invitation"></a>Davet

Bir veri sağlayıcısı, veri paylaşımı birden çok alıcıya davet edebilirsiniz. Bunlar veri paylaşımına alıcılar ekleyerek bunu yapabilirsiniz. Bir veri paylaşımı oluşturulduktan sonra davetleri da eklenebilir. 

Bunu gönderildikten sonra bir veri sağlayıcısı davet silebilirsiniz. Bir veri sağlayıcısı daveti kabul ettikten sonra silerse, veri tüketici hala etkin paylaşımı aboneliğiniz olduğunu unutmayın. Veri sağlayıcısı davet siler ve henüz kabul, veri tüketici kabul etmek mümkün olmayacaktır. 

## <a name="recipient"></a>Alıcı

Bir alıcı bir veri paylaşımı için bir davet alan kişidir. Genellikle, bir veri sağlayıcısı alıcılar oluşturdukları veri paylaşımına ekler. Davetiye alıcısı daveti kabul ettikten sonra bir veri tüketici haline gelir.  

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya başlayın öğrenmek için devam [verilerinizi paylaşmak](share-your-data.md) öğretici.

