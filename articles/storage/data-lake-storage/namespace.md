---
title: Azure Data Lake Storage Gen2 Önizleme hiyerarşik Namespace
description: Azure Data Lake Storage Gen2 Önizleme için hiyerarşik ad alanı kavramını açıklar
services: storage
author: jamesbak
manager: twooley
ms.service: storage
ms.topic: article
ms.date: 06/27/2018
ms.author: jamesbak
ms.component: data-lake-storage-gen2
ms.openlocfilehash: 9b41ca1eedcf69b23557c079e018d69de9fb907c
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37114376"
---
# <a name="azure-data-lake-storage-gen2-preview-hierarchical-namespace"></a>Azure Data Lake Storage Gen2 Önizleme hiyerarşik ad alanı

Nesne depolama ölçek ve fiyatlarını dosya sistemi performansı sağlamak üzere Azure Data Lake depolama Gen2 Önizleme izin veren bir anahtar eklenmesi mekanizmadır bir **hiyerarşik ad alanı**. Bu nesneleri/dosya dizinleri ve iç içe alt dizinleri bir hiyerarşiye dosya sistemi, bilgisayarınızdaki düzenlenmiştir aynı şekilde düzenlenmesine için bir hesap içinde koleksiyonunu sağlar. Etkin hiyerarşik ad alanı ile Data Lake Storage Gen2 analytics motoru ve çerçeveler hakkında bilginiz dosya sistemi sematiğini ile nesne depolama düşük maliyet ve ölçeklenebilirlik sağlar.

## <a name="the-benefits-of-the-hierarchical-namespace"></a>Hiyerarşik ad alanı yararları

> [!NOTE]
> Azure Data Lake Storage Gen2 genel Önizleme sırasında aşağıda listelenen özelliklerden bazıları, kullanılabilirliklerini farklılık gösterebilir. Yeni özellikler ve bölgeler Önizleme programı sırasında yayımlanan gibi bu bilgileri bizim ayrılmış Yammer Grup duyurulacaktır.  

Aşağıdaki faydaları hiyerarşik ad blob verilerin üzerine uygulamak dosya sistemleri ile ilişkilidir:

- **Atomik dizin işleme:** nesne depoları, eğik çizgi (/) nesne adı yol kesimleri belirtmek için ek bir kuralı'nu benimseme dizin hiyerarşisi yaklaşık. Bu kural nesneleri düzenlemek için çalışırken, kuralı taşıma, yeniden adlandırma veya dizinleri silme gibi eylemleri için hiçbir desteği sağlar. Gerçek dizinleri uygulamaları olası dizin düzeyinde görevleri elde etmek için tek tek bloblar milyonlarca işlemesi gerekir. Bunun aksine, hiyerarşik ad alanı, tek bir giriş (üst dizini) güncelleştirerek bu görevleri işler. 

    Bu çarpıcı iyileştirme birçok büyük veri analizi çerçeveyi özellikle önemlidir. Hive, Spark, vb. gibi araçlar genellikle çıkış geçici konuma yazma ve iş sonuç konumda yeniden adlandırın. Hiyerarşik ad bu yeniden adlandırma genellikle kendi analiz işlem daha uzun sürebilir. Daha düşük iş gecikmesi daha düşük sahip olma maliyeti (TCO) analytics iş yükleri için eşittir.

- **Tanıdık arabirimi stili:** dosya sistemleri iyi geliştiriciler ve kullanıcılar tarafından anlaşılmasını. Data Lake Storage Gen2 tarafından kullanıma sunulan dosya sistemi arabirimi, büyük ve küçük bilgisayarlar tarafından kullanılan aynı örnektir gibi buluta taşıdığınızda, yeni bir depolama kip öğrenmek için gerek yoktur.

Nesne depoları geçmişte hiyerarşik ad alanları desteklenmiyor olduğunu nedenlerinden hiyerarşik ad alanları ölçek sınırlı biridir. Ancak, Data Lake Storage Gen2 hiyerarşik ad alanı doğrusal olarak ölçeklendirir ve veri kapasitesi veya performans düşebilir değil.

## <a name="when-to-enable-the-hierarchical-namespace"></a>Zaman hiyerarşik ad alanı etkinleştirmek için

Hiyerarşik ad kapatma dizinleri işlemek dosya sistemleri için tasarlanmış depolama iş yükleri için önerilir. Bu, öncelikle analytics işlenmek üzere tüm iş yükleri içerir. Kuruluş yüksek derecede gerektiren veri kümeleri, ayrıca hiyerarşik ad alanı etkinleştirerek yararlı olacaktır.

Hiyerarşik ad alanı etkinleştirme nedenleri TCO çözümlemesi tarafından belirlenir. Genel olarak bakıldığında, iş yükü gecikme depolama hızlandırma nedeniyle yenilikleri işlem kaynakları için daha az zaman gerektirir. Birçok iş yükleri için gecikme süresini hiyerarşik ad alanı tarafından etkinleştirilmiş atomik dizin düzenlemesi nedeniyle iyileştirilebilir. Birçok iş yükü, toplam maliyeti % > 85 işlem kaynağını temsil eder ve böylece bile uygun azalma iş yükü gecikme TCO tasarruf için önemli miktarda karşılık gelir. Burada hiyerarşik ad alanı etkinleştirme depolama maliyetleri artırır durumlarda bile TCO hala nedeniyle daha az işlem maliyetleri düşürdü.

## <a name="when-to-disable-the-hierarchical-namespace"></a>Zaman hiyerarşik ad alanı devre dışı bırakmak için

Bazı nesne deposu iş yükleri bir fayda hiyerarşik ad alanı etkinleştirerek kazanabilir değil. Bu iş yükleri örnekleri arasında yedeklemeler, görüntü depolama ve nesne kuruluş depolandığı ayrı ayrı nesneleri kendilerini diğer uygulamaları (*ör*, ayrı bir veritabanında).

> [!NOTE]
> Önizleme sürümü ile hiyerarşik ad etkinleştirirseniz var. veri veya işlemleri Blob ve Data Lake Storage Gen2 REST API'leri arasında hiçbir birlikte çalışabilirliği Bu işlevselliği Önizleme sırasında eklenir.

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](./quickstart-create-account.md)