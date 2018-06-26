---
title: Azure günlük analizi görünümlerinde filtreleri | Microsoft Docs
description: Günlük analizi görünümünde bir filtre kullanıcıların Görünüm değiştirmeden görünümünde verileri belirli bir özellik değeri tarafından filtre olanak tanır.  Bu makalede bir filtre ve özel bir görünüm için bir tane ekleyin nasıl kullanılacağı açıklanmaktadır.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ce41dc30-e568-43c1-97fa-81e5997c946a
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/22/2018
ms.author: bwren
ms.openlocfilehash: 21b54f60286e25c410b9d51de8be122c450080d3
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/25/2018
ms.locfileid: "36752784"
---
# <a name="filters-in-log-analytics-views"></a>Günlük analizi görünümlerinde filtreleri
A **filtre** içinde bir [günlük analizi görüntülemek](log-analytics-view-designer.md) kullanıcıların Görünüm değiştirmeden görünümünde verileri belirli bir özellik değeri tarafından filtre olanak sağlar.  Örneğin, yalnızca belirli bir bilgisayardan veri görünümünü filtrelemek için görünümün kullanıcılar veya bilgisayarlar kümesi izin verebilir.  Birden çok özelliğe göre filtre yapmalarına izin vermek için tek bir görünümde birden çok filtre oluşturabilirsiniz.  Bu makalede bir filtre ve özel bir görünüm için bir tane ekleyin nasıl kullanılacağı açıklanmaktadır.

## <a name="using-a-filter"></a>Bir filtre kullanarak
Görünüm veri zaman aralığını değiştirebileceğiniz aşağı açılan açmak için bir görünüm üstündeki veri zaman aralığı'nı tıklatın.

![Filtre örneği](media/log-analytics-view-designer/filters-example-time.png)

Tıklatın **+** tanımlanan özel filtreler görünümünü kullanarak bir filtre eklemek için. Bir değer girin ve açılan bir filtrenin değeri ya da seçin. Tıklayarak filtreleri eklemeye devam **+**. 


![Filtre örneği](media/log-analytics-view-designer/filters-example-custom.png)

Tüm filtre değerleri kaldırırsanız, bu filtre artık uygulanır.


## <a name="creating-a-filter"></a>Bir filtre oluşturma

Filtresi oluşturma **filtreleri** sekmesinde [bir görünümü düzenleme](log-analytics-view-designer.md).  Filtre görünüm için geneldir ve tüm bölümleri görünümünde uygular.  

![Filtre ayarları](media/log-analytics-view-designer/filters-settings.png)

Aşağıdaki tabloda bir filtre için ayarları açıklanır.

| Ayar | Açıklama |
|:---|:---|
| Alan Adı | Filtreleme için kullanılan alanın adı.  Bu özetleme alanında eşleşmelidir **değerleri için sorgu**. |
| Değerleri için sorgu | Filtre açılan kullanıcı için doldurmak için çalıştırılacak sorgu.  Bu da kullanmalıdır [özetlemek](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/summarize-operator) veya [ayrı](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/distinct-operator) belirli bir alan ve eşleşmelidir için benzersiz değerler sağlamak için **alan adı**.  Kullanabileceğiniz [sıralama](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/sort-operator) kullanıcıya gösterilen değerleri sıralamak için. |
| Etiket | Filtre destekleme sorgularda kullanılan ve ayrıca kullanıcıya görüntülenen alanın adı. |

### <a name="examples"></a>Örnekler

Aşağıdaki tabloda ortak filtreleri birkaç örnek verilmiştir.  

| Alan Adı | Değerleri için sorgu | Etiket |
|:--|:--|:--|
| Bilgisayar   | Sinyal &#124; farklı bilgisayar &#124; bilgisayar asc göre sırala | Bilgisayarlar |
| EventLevelName | Olay &#124; ayrı EventLevelName | Severity |
| Önem düzeyi | Syslog &#124; farklı önem düzeyi | Severity |
| SvcChangeType | ConfigurationChange &#124; ayrı svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Görünüm sorguları değiştirme

Herhangi bir etkisi olması için bir filtre için herhangi bir sorgu görünümünde seçilen değerleri üzerinde filtreleme değiştirmeniz gerekir.  Herhangi bir sorgu görünümünde değiştirmeyin, kullanıcının seçtiği herhangi bir değeri hiçbir etkisi olmaz.

Sorguda bir filtre değeri kullanmak için sözdizimi aşağıdaki gibidir: 

    where ${filter name}  

Örneğin, görünümünüze bir sorgu döndürür olayları varsa ve bilgisayarlar olarak adlandırılan bir filtre kullanır, aşağıdakileri kullanabilirsiniz.

    Event | where ${Computers} | summarize count() by EventLevelName

Önem derecesi adlı başka bir filtre eklediyseniz, her iki filtreleri kullanmak için aşağıdaki sorguyu kullanabilirsiniz.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinmek [görselleştirme bölümleri](log-analytics-view-designer-parts.md) özel görünümünüzde ekleyebilirsiniz.