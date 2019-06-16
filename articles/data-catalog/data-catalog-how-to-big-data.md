---
title: Azure veri Kataloğu'nda 'büyük veri' veri kaynaklarıyla çalışma
description: Nasıl yapılır makalesi için Azure veri kataloğu kullanarak Azure Blob Depolama, Azure Data Lake ve Hadoop HDFS gibi 'büyük veri' veri kaynaklarıyla desenleri vurgulama.
services: data-catalog
author: JasonWHowell
ms.author: jasonh
ms.assetid: 626d1568-0780-4726-bad1-9c5000c6b31a
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: b6b419d575e2164fc683b8e6b5020572db74d1b4
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "61001750"
---
# <a name="how-to-work-with-big-data-sources-in-azure-data-catalog"></a>Azure veri Kataloğu'nda büyük veri kaynaklarıyla çalışma
## <a name="introduction"></a>Giriş
**Microsoft Azure veri Kataloğu** kayıt ve kurumsal veri kaynakları için bulma sistemi olarak görev yapan tam yönetilen bir bulut hizmetidir. Bu yardımcı kişiler hakkında keşfedin, anlamak ve veri kaynakları ve yardımcı kuruluşların büyük veri dahil olmak üzere, mevcut veri kaynağından daha fazla değer elde kullanacağınızı olur.

**Azure veri Kataloğu** Azure Blog depolama BLOB'ları ve dizinleri yanı sıra Hadoop HDFS dosya ve dizinleri kaydını destekler. Bu veri kaynaklarının yapısını yarı yapılandırılmış büyük esneklik sağlar. Ancak, bunları kaydetme en değerini almak için **Azure veri Kataloğu**, kullanıcıların veri kaynaklarını nasıl düzenlendiği düşünmeniz gerekir.

## <a name="directories-as-logical-data-sets"></a>Mantıksal veri kümesi olarak dizinleri
Büyük veri kaynakları düzenlemek için yaygın dizinleri mantıksal veri kümesi değerlendirilecek modelidir. Üst düzey dizinleri alt bölümleri tanımlayın ve veri içerdikleri depolamak bir veri kümesi tanımlamak için kullanılır.

Bu düzenin bir örnek olabilir:

    \vehicle_maintenance_events
        \2013
        \2014
        \2015
            \01
                \2015-01-trailer01.csv
                \2015-01-trailer92.csv
                \2015-01-canister9635.csv
                ...
    \location_tracking_events
        \2013
        ...

Bu örnekte, mantıksal veri kümesi vehicle_maintenance_events ve location_tracking_events temsil eder. Bu klasörlerin her yıl ve aya göre alt klasörler halinde düzenlenmiş veri dosyalarını içerir. Bu klasörler, yüzlerce veya binlerce dosya potansiyel olarak içerebilir.

Bu düzende, tek tek dosyaları kaydetme **Azure veri Kataloğu** büyük olasılıkla mantıklı değildir. Bunun yerine, verilerle çalışan kullanıcılar için anlamlı veri kümelerini göstermek dizinleri kaydedin.

## <a name="reference-data-files"></a>Başvuru veri dosyaları
Başvuru veri kümeleri, tek tek dosyalar olarak depolamak tamamlayıcı bir desendir. Bu veri kümesi 'küçük' tarafında büyük veri zorlayıcı olabilir ve genellikle bir analitik veri modeli boyutları benzer. Başvuru veri dosyaları, toplu başka bir yerde büyük veri deposunda saklanan veri dosyaları için bağlam sağlamak için kullanılan kayıtları içerir.

Bu düzenin bir örnek olabilir:

    \vehicles.csv
    \maintenance_facilities.csv
    \maintenance_types.csv

Bir analist veya veri Bilimcisi daha büyük bir dizin yapıları, içerdiği veriler ile çalışırken, bu başvuru dosyalardaki veriler yalnızca ada veya Kimliğe göre daha büyük veri kümesinde başvurulan varlıklar için daha ayrıntılı bilgi sağlamak için kullanılabilir.

Bu düzende, tek başvuru veri dosyalarıyla birlikte kaydetmek için mantıklı **Azure veri Kataloğu**. Her dosya, bir veri kümesini temsil eder ve her bir ek açıklama ve ayrı ayrı bulunan.

## <a name="alternate-patterns"></a>Alternatif desenleri
Önceki bölümde açıklanan desenleri için büyük veri deposu düzenlenmiş olabilir yalnızca iki olası yolu olsa da, her farklı bir uygulamasıdır. Nasıl veri kaynaklarınızı, büyük veri kaynaklarıyla kaydederken yapılandırılmıştır bakılmaksızın **Azure veri Kataloğu**, dosyaları ve dizinleri temsil eden değer başkalarına içinde veri kümeleri kaydetmede odaklanmak, Kuruluş. Tüm dosyalar ve dizinler kaydetme, kullanıcıların ihtiyaç duydukları bulmak daha zor hale Kataloğu daraltabilir.

## <a name="summary"></a>Özet
İle veri kaynaklarını kaydederek **Azure veri Kataloğu** bulunmasını ve anlaşılmasını kolaylaştırır. Kaydetme ve büyük veri dosyaları ve dizinleri temsil eden mantıksal veri kümesi yorumlama, ihtiyaç duydukları büyük veri kaynaklarını bulabilir ve kullanıcılara yardımcı olabilir.
