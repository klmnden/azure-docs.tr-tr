---
title: Azure SQL veritabanı Machine Learning Hizmetleri ile R (Önizleme) genel bakış
description: Bu makalede Azure SQL veritabanı Machine Learning Hizmetleri (R ile) ve nasıl çalıştığını açıklar.
services: sql-database
ms.service: sql-database
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: dphansen
ms.author: davidph
ms.reviewer: carlrab
manager: cgronlun
ms.date: 03/01/2019
ms.openlocfilehash: fe472b8a19b45d7f7b00a8f858c9179d6ee51999
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65951587"
---
# <a name="azure-sql-database-machine-learning-services-with-r-preview"></a>Azure SQL veritabanı Machine Learning Hizmetleri ile R (Önizleme)

Machine Learning Hizmetleri, Azure SQL veritabanı, veritabanı R betikleri yürütmek için kullanılan bir özelliktir. Bu özellik, yüksek performanslı Tahmine dayalı analiz ve makine öğrenimi için Microsoft R paketleri içerir. İlişkisel veri R betikleri saklı yordamlar, T-SQL betiği içeren R bildirimleri veya R kodunu içeren T-SQL aracılığıyla kullanılabilir.

> [!IMPORTANT]
> Azure SQL veritabanı Machine Learning Hizmetleri (R ile) şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>
> Tek veritabanları ve sanal çekirdek tabanlı satın alma modeli kullanarak elastik havuzları için genel önizlemede kullanılabilir **genel amaçlı** ve **iş açısından kritik** hizmet katmanları. Bu ilk genel önizlemeye sunuldu **hiper ölçekli** hizmet katmanı ve **yönetilen örnek** dağıtım seçeneği desteklenmez. Şu an için yalnızca R dili desteklenmektedir. Python desteği yoktur.
>
> Önizleme aşağıdaki bölgelerde kullanılabilir: Batı Avrupa, Kuzey Avrupa, Batı ABD 2, Doğu ABD, Güney Orta ABD, Kuzey Orta ABD, Kanada Orta, Güneydoğu Asya, Hindistan Güney ve Avustralya Güneydoğu.
>
> [Önizleme için kaydolun](#signup) aşağıda.

## <a name="what-you-can-do-with-r"></a>R ile yapabilecekleriniz

Gelişmiş analiz ve makine öğrenimi veritabanında sunmak üzere R dilinin gücünü kullanın. Bu yetenek, hesaplamaları ve verilerin bulunduğu işlem, ağ üzerinden veri çekmek için ihtiyacını getirir. Ayrıca, Gelişmiş analiz ölçekte sunmak için Kurumsal R paketleri gücünden yararlanabilirsiniz.

Machine Learning Hizmetleri ile Kurumsal R paketleri Microsoft gelen yayılan r, temel bir dağıtım içerir. Microsoft R işlevleri ve algoritmaları, ölçek ve Tahmine dayalı analiz, modelleme, veri görselleştirmeleri ve öncü makine öğrenimi algoritması sunmaya yardımcı programı için tasarlanmıştır.

### <a name="r-packages"></a>R paketleri

En sık kullanılan açık kaynaklı R paketleri Machine Learning Hizmetleri önceden yüklenmiş durumdadır. Microsoft şu R paketlerini de eklenmiştir:

| R paketi | Açıklama|
|-|-|
| [Microsoft R Open](https://mran.microsoft.com/rro) | Microsoft R Open r Microsoft Gelişmiş dağıtımıdır. İstatistiksel analiz ve veri bilimine yönelik eksiksiz bir açık kaynak platformu olduğu. Bu açık ve %100 R ile uyumlu temel ve Gelişmiş performans ve yeniden üretilebilirliğini için ek özellikler içerir. |
| [RevoScaleR](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-revoscaler) | Bu Kitaplığı'nda ölçeklenebilir r işlevleri arasında en yaygın kullanılan için RevoScaleR birincil kitaplığıdır. Veri dönüşümlerini ve işleme, istatistiksel özetleme, Görselleştirme ve modelleme çözümlemeler ve çok sayıda formlar bu kitaplıkları bulunamadı. Ayrıca, bu kitaplıkları olarak işlevleri otomatik olarak iş yükleri kullanılabilir çekirdek sayısı için paralel işleme öbekler halinde düzenlenir ve hesaplama altyapısı tarafından yönetilen veri çubuğunda çalışma olanağı ile dağıtabilirsiniz. |
| [MicrosoftML (R)](https://docs.microsoft.com/sql/advanced-analytics/r/ref-r-microsoftml) | Metin analizi, görüntü analizi ve yaklaşım analizi için özel modeller oluşturmak için machine learning algoritmaları MicrosoftML ekler. |

Önceden yüklenmiş paketlerine ek olarak şunları yapabilirsiniz [ek paketler yüklemek](sql-database-machine-learning-services-add-r-packages.md).

<a name="signup"></a>

## <a name="sign-up-for-the-preview"></a>Önizleme için kaydolun

Genel önizlemeye kaydolmak için aşağıdaki adımları izleyin:

1. Azure aboneliğiniz yoksa, [hesap oluşturma](https://azure.microsoft.com/free/) başlamadan önce.

2. Microsoft'ta bir e-posta göndermek [ sqldbml@microsoft.com ](mailto:sqldbml@microsoft.com) genel önizlemeye kaydolmak için. Machine Learning Services (R ile) genel önizleme sürümü, SQL Veritabanı'nda varsayılan olarak etkin değildir.

Programa kayıtlı olduğunuz sonra Microsoft tarafından yerleşik, veritabanını etkinleştir R için mevcut veya yeni ve genel önizlemeye sunuldu.

R ile Machine Learning Hizmetleri için üretim iş yükü, genel Önizleme sırasında önerilmez.

## <a name="next-steps"></a>Sonraki adımlar

- Bkz: [anahtar SQL Server Machine Learning Hizmetleri arasındaki farklar](sql-database-machine-learning-services-differences.md).
- R sorguyu Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) kullanmayı öğrenin için bkz: [Hızlı Başlangıç Kılavuzu](sql-database-connect-query-r.md).
- Bazı basit R betikleriyle çalışmaya başlamak için bkz. [oluşturma ve Azure SQL veritabanı Machine Learning Hizmetleri (Önizleme) içinde basit R betikleri çalıştırma](sql-database-quickstart-r-create-script.md).
