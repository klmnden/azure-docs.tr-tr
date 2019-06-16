---
title: Azure Data Lake depolama Gen2 hiyerarşik Namespace
description: Azure Data Lake depolama Gen2 hiyerarşik ad alanı kavramını açıklar
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: normesta
ms.reviewer: jamesbak
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: e0d888db5f8de137783a3f9282ca7f85d8a30fc3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64939451"
---
# <a name="azure-data-lake-storage-gen2-hierarchical-namespace"></a>Azure Data Lake depolama Gen2 hiyerarşik ad alanı

Nesne depolama ölçek ve Fiyatlar, dosya sistem performansını sağlamak Azure Data Lake depolama Gen2 izin veren bir anahtar eklenmesi mekanizmasıdır bir **hiyerarşik ad alanı**. Bu koleksiyonu nesneleri/dosyaları içinde bir hesap ve iç içe geçmiş alt dizinlerin bir hiyerarşiye bilgisayarınızdaki dosya sistemi düzenlenir aynı şekilde düzenlenmesine olanak sağlar. Etkin hiyerarşik ad alanı ile bir depolama hesabı ölçeklenebilirlik ve maliyet açısından uygunluğunu nesne depolama, analiz altyapıları ve çerçeveleri için tanıdık dosya sistemi sematiğini ile sağlayabilen haline gelir.

## <a name="the-benefits-of-the-hierarchical-namespace"></a>Hiyerarşik ad alanı avantajları

Hiyerarşik ad alanı blob verilerinde uygulamak dosya sistemleri ile ilişkili aşağıdaki faydaları şunlardır:

- **Atomik bir dizini düzenleme:** Nesne depoları, yol kesimleri göstermek için nesne adlarında eğik çizgi (/) ekleme bir kuralı benimseyen dizin sıradüzeni yaklaşık. Bu kural nesnelerini düzenleme için çalışırken, kuralı taşıma, yeniden adlandırma veya dizinleri silme gibi eylemler için hiçbir Yardım sağlar. Gerçek dizin uygulamaları dizin düzeyinde görevleri elde etmek için tek tek bloblar milyonlarca potansiyel olarak işlemesi gerekir. Bunun aksine, hiyerarşik ad alanı, tek bir giriş (üst dizini) güncelleştirerek bu görevleri işler.

    Çarpıcı Bu iyileştirme, çok büyük veri analizi çerçeveleri için özellikle önemlidir. Hive, Spark vb. gibi araçlar genellikle geçici bir konuma çıkışını yazmak ve sonuç projenin konumu yeniden adlandırın. Hiyerarşik ad alanı bu yeniden adlandırma genellikle kendi analiz işlem daha uzun sürebilir. Daha düşük işlem gecikme süresi maliyetinizi (TCO) analizi iş yükleri için düşük maliyetli eşittir.

- **Tanıdık arabirimi stili:** Dosya sistemleri, geliştiriciler ve kullanıcılar tarafından de anlaşılabilir. Data Lake depolama Gen2 tarafından kullanıma sunulan dosya sistemi arabirimi büyük ve küçük bilgisayarlar tarafından kullanılan aynı paradigma olduğu gibi buluta taşıdığınızda, yeni bir depolama paradigma öğrenmek için gerek yoktur.

Nesne depoları daha önce bir hiyerarşik ad alanı desteklenen henüz nedeniyle bir hiyerarşik ad alanı ölçek sınırları biridir. Ancak, Data Lake depolama Gen2 hiyerarşik ad alanı doğrusal olarak ölçeklenen ve veri kapasitesi veya performansı düşmez.

## <a name="when-to-enable-the-hierarchical-namespace"></a>Hiyerarşik ad alanı etkinleştirme zamanı

Hiyerarşik ad alanı dizinleri yönetmek dosya sistemleri için tasarlanmış depolama iş yükleri için etkinleştirmeniz önerilir. Bu, öncelikle analiz işleme için tüm iş yüklerini içerir. Hiyerarşik ad alanı sağlayarak yüksek düzeyde kuruluş gerektiren veri kümeleri de yararlı olacaktır.

Hiyerarşik ad alanı etkinleştirme nedenlerini TCO analizi tarafından belirlenir. Genel olarak bakıldığında, iş yükü gecikme nedeniyle depolama hızlandırma geliştirmeleri işlem kaynakları için daha az zaman gerektirir. Birçok iş yükleri için gecikme süresi, hiyerarşik ad alanı tarafından etkinleştirilen atomik directory işleme nedeniyle iyileştirilebilir. Çok sayıda iş yükü > %85 toplam maliyeti, bilgi işlem kaynağını temsil eder ve toplam sahip olma maliyeti tasarrufu için önemli miktarda iş yükü gecikme çağrılarda bile büyüklükteki bir azalma karşılık gelmektedir. Burada depolama maliyetlerini artırır hiyerarşik ad alanı etkinleştiriliyor durumlarda bile TCO'yu hala nedeniyle daha az işlem maliyetleri düşürdü.

## <a name="when-to-disable-the-hierarchical-namespace"></a>Hiyerarşik ad alanı devre dışı bırakmak ne zaman

Bazı nesne deposu iş yükleri hiyerarşik ad alanı etkinleştirerek hiçbir avantajı elde değil. Yedeklemeler, resim depolama ve nesne kuruluş depolandığı ayrı olarak nesnelerinin kendileri diğer uygulamalar (örneğin: ayrı bir veritabanı içinde).

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](./data-lake-storage-quickstart-create-account.md)
