---
title: Azure Log Analytics görünümlerde filtreleri | Microsoft Docs
description: Log Analytics görünümünü bir filtre Veri Görünümü'nde görünümü değiştirmeden belirli bir özellik değeri tarafından filtrelemek kullanıcıların sağlar.  Bu makalede, bir filtre ve özel görünüm için bir tane ekleyin nasıl kullanılacağı açıklanır.
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
ms.topic: conceptual
ms.date: 06/22/2018
ms.author: bwren
ms.component: ''
ms.openlocfilehash: 87cfcefd91652194adc0a5ca4580a8d00ba4d234
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50412763"
---
# <a name="filters-in-log-analytics-views"></a>Log Analytics görünümlerde filtreleri
A **filtre** içinde bir [Log Analytics görüntülemek](log-analytics-view-designer.md) kullanıcılar görünümü değiştirmeden belirli bir özellik değeri tarafından görünümünde verileri filtrelemek olanak sağlar.  Örneğin, görünümünüzü yalnızca belirli bir bilgisayardan veri görünümünü filtrelemek için kullanıcıları veya bilgisayarları kümesini izin verebilir.  Filtre birden çok özellikleri tarafından kullanıcılara izin vermek için tek bir görünümde birden çok filtre oluşturabilirsiniz.  Bu makalede, bir filtre ve özel görünüm için bir tane ekleyin nasıl kullanılacağı açıklanır.

## <a name="using-a-filter"></a>Filtre kullanma
Verileri zaman aralığını üst görünümün görünüm verileri zaman aralığını değiştirebileceğiniz aşağı açılan açmak için tıklayın.

![Filtre örneği](media/log-analytics-view-designer-filters/filters-example-time.png)

Tıklayın **+** görünüm için tanımlanan özel filtreleri kullanarak filtre ekleyin. Açılan veya bir değer yazın ya da filtre için bir değer seçin. Tıklayarak filtreleri eklemeye devam **+**. 


![Filtre örneği](media/log-analytics-view-designer-filters/filters-example-custom.png)

Tüm değerleri için bir filtre kaldırmanız durumunda bu filtre artık uygulanır.


## <a name="creating-a-filter"></a>Bir filtre oluşturma

Filtresi oluşturma **filtreleri** sekmesinde [bir görünümü düzenleme](log-analytics-view-designer.md).  Filtre görünümü için genel ve görünümünde tüm bölümleri geçerlidir.  

![Filtre ayarları](media/log-analytics-view-designer-filters/filters-settings.png)

Aşağıdaki tabloda filtre ayarlarını açıklar.

| Ayar | Açıklama |
|:---|:---|
| Alan Adı | Filtreleme için kullanılan alanın adı.  Bu summarıze alanında eşleşmelidir **değerler için sorgu**. |
| Değerler için sorgu | Kullanıcı için Filtre açılan listeyi doldurmak için çalıştırılacak sorgu.  Bunu kullanmanız gerekir [özetlemek](/azure/kusto/query/summarizeoperator) veya [ayrı](/azure/kusto/query/distinctoperator) ve belirli bir alanla eşleşmelidir için benzersiz değerler sağlayan **alan adı**.  Kullanabileceğiniz [sıralama](/azure/kusto/query/sortoperator) kullanıcıya görüntülenen değerleri sıralamak için. |
| Etiket | Filtre destekleyen sorgularında kullanılıyor ve ayrıca kullanıcıya görüntülenen alanın adı. |

### <a name="examples"></a>Örnekler

Aşağıdaki tablo, birkaç ortak filtrelere örnekleri içerir.  

| Alan Adı | Değerler için sorgu | Etiket |
|:--|:--|:--|
| Bilgisayar   | Sinyal &#124; farklı bilgisayar &#124; bilgisayar asc göre sırala | Bilgisayarlar |
| EventLevelName | Olay &#124; ayrı EventLevelName | Severity |
| Err | Syslog &#124; ayrı Err | Severity |
| SvcChangeType | ConfigurationChange &#124; ayrı svcChangeType | ChangeType |


## <a name="modify-view-queries"></a>Görünüm sorgularını değiştirme

Herhangi bir etkisi olması için bir filtre için görünümünde seçilen değeri filtrelemek için sorguları değiştirmeniz gerekir.  Ardından görünümünde sorgular değiştirmez, kullanıcının seçtiği herhangi bir değeri hiçbir etkisi olmaz.

Bir filtre değeri bir sorguda kullanmak için sözdizimi aşağıdaki gibidir: 

    where ${filter name}  

Örneğin, görünümünüzü döndürür olayları bir sorgu varsa, ve bilgisayarlar olarak adlandırılan bir filtre kullanır, aşağıdakileri kullanabilirsiniz.

    Event | where ${Computers} | summarize count() by EventLevelName

Önem derecesi adlı başka bir filtre eklediyseniz, her iki filtreleri kullanmak için aşağıdaki sorguyu kullanabilirsiniz.

    Event | where ${Computers} | where ${Severity} | summarize count() by EventLevelName

## <a name="next-steps"></a>Sonraki adımlar
* Daha fazla bilgi edinin [görselleştirme bölümleri](log-analytics-view-designer-parts.md) özel görünümünüzü ekleyebilirsiniz.
