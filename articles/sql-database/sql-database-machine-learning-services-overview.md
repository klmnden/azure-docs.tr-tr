---
title: Azure SQL veritabanı (Önizleme) genel bakış machine Learning Hizmetleri (R ile)
description: Bu konu, Azure SQL veritabanı Machine Learning Hizmetleri (R ile) açıklar ve nasıl çalıştığı açıklanmaktadır.
services: sql-database
ms.service: sql-database
ms.subservice: machine-learning-services
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 01/31/2019
ms.openlocfilehash: e2159e7cc59830c3d0d10f1c5b9697ab5b45b666
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824291"
---
# <a name="machine-learning-services-with-r-in-azure-sql-database-preview"></a>Machine Learning Hizmetleri (R ile) Azure SQL veritabanı (Önizleme)

Machine Learning Hizmetleri, Azure SQL veritabanı, veritabanı R betikleri yürütmek için kullanılan bir özelliktir. Bu özellik, yüksek performanslı Tahmine dayalı analiz ve makine öğrenimi için Microsoft R paketleri içerir. İlişkisel veri R betikleri saklı yordamlar, T-SQL betiği içeren R bildirimleri veya R kodunu içeren T-SQL aracılığıyla kullanılabilir.

> [!NOTE]
> Machine Learning Hizmetleri (R ile) Azure SQL veritabanı'nda, şu anda genel Önizleme aşamasındadır. [Önizleme için kaydolun](#signup) aşağıda.

## <a name="what-you-can-do-with-r"></a>R ile yapabilecekleriniz

Gelişmiş analiz ve makine öğrenimi veritabanında sunmak üzere R dilinin gücünü kullanın. Bu yetenek, hesaplamaları ve verilerin bulunduğu işlem, ağ üzerinden veri çekmek için ihtiyacını getirir. Ayrıca, ölçeğinde Gelişmiş analiz sağlamak üzere Kurumsal R paketleri gücünden yararlanın.

Machine Learning Hizmetleri ile Kurumsal R paketleri Microsoft gelen yayılan r, temel bir dağıtım içerir. Microsoft R işlevleri ve algoritmaları, ölçek ve Tahmine dayalı analiz, modelleme, veri görselleştirmeleri ve öncü makine öğrenimi algoritması sunmaya yardımcı programı için tasarlanmıştır.

### <a name="r-packages"></a>R paketleri

En sık kullanılan açık kaynak R paketleri Machine Learning Hizmetleri önceden yüklenmiş durumdadır. Microsoft şu R paketlerini de eklenmiştir:

| R paketi | Açıklama|
|-|-|
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open r Microsoft Gelişmiş dağıtımıdır. Bu, istatistiksel analiz ve veri bilimine yönelik eksiksiz bir açık kaynak platformudur. Bu açık ve %100 R ile uyumlu temel ve Gelişmiş performans ve yeniden üretilebilirliğini için ek özellikler içerir. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | Bu Kitaplığı'nda ölçeklenebilir r işlevleri arasında en yaygın kullanılan için RevoScaleR birincil kitaplığıdır. Veri dönüşümlerini ve işleme, istatistiksel özetleme, Görselleştirme ve modelleme çözümlemeler ve çok sayıda formlar bu kitaplıkları bulunamadı. Ayrıca, bu kitaplıkları olarak işlevleri otomatik olarak iş yükleri kullanılabilir çekirdek sayısı için paralel işleme öbekler halinde düzenlenir ve hesaplama altyapısı tarafından yönetilen veri çubuğunda çalışma olanağı ile dağıtabilirsiniz. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | Metin analizi, görüntü analizi ve yaklaşım analizi için özel modeller oluşturmak için machine learning algoritmaları MicrosoftML ekler. |

Önceden yüklenmiş paketlerine ek olarak şunları yapabilirsiniz [ek paketler yüklemek](sql-database-connect-query-r.md#add-package).

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Önizleme için kaydolun

Genel önizlemeye kaydolmak için aşağıdaki adımları izleyin:

1. Azure aboneliğiniz yoksa, [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

2. Microsoft'ta bir e-posta göndermek [ sqldbml@microsoft.com ](mailto:sqldbml@microsoft.com) genel önizlemeye kaydolmak için. Machine Learning Services (R ile) genel önizleme sürümü, SQL Veritabanı'nda varsayılan olarak etkin değildir.

Programda kaydedildikten sonra Microsoft tarafından yerleşik, veritabanını etkinleştir R için mevcut veya yeni ve genel önizlemeye sunuldu.

Machine Learning Hizmetleri (R ile) SQL veritabanı'nda şu anda yalnızca sanal çekirdek tabanlı satın alma modeli kullanılabilir **genel amaçlı** ve **iş açısından kritik** hizmet katmanları için tek başına ve Elastik havuz dağıtım seçenekleri. Bu ilk genel önizlemeye sunuldu **hiper ölçekli** hizmet katmanı ve **yönetilen örneği** dağıtımlardaki seçim desteklenmez.

Şu an için yalnızca R dili desteklenmektedir. Python desteği yoktur. Önizleme aşağıdaki bölgeler: Batı Avrupa, Kuzey Avrupa, Batı ABD 2, Doğu ABD, Güney Orta ABD, Kuzey Orta ABD, Kanada Orta, Güneydoğu Asya, Hindistan Güney ve Avustralya Güneydoğu başlangıçta kullanılabilir. Ek bölgeler daha sonra eklenecektir.

Machine Learning Hizmetleri ile R üretim iş yükleri için genel Önizleme sırasında kullanmayın.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [anahtar SQL Server Machine Learning Hizmetleri arasındaki farklar](sql-database-machine-learning-services-differences.md)
- Machine Learning Hizmetleri (R ile) Azure SQL veritabanı'nda kullanmayı öğrenmek için bkz: [Hızlı Başlangıç Kılavuzu](sql-database-connect-query-r.md).
- Daha fazla bilgi edinin [SQL Server R dili öğreticiler](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sql-server-r-tutorials)