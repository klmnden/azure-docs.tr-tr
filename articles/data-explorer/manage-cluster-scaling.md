---
title: Değişen talepleri karşılamak için ölçekleme Azure Veri Gezgini küme
description: Bu makalede, Ölçek genişletme ve ölçek-değişen isteğe bağlı bir Azure Veri Gezgini küme içindeki adımları açıklanmaktadır.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: bd3be916587ab884ea5579234a6189af54b53797
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2018
ms.locfileid: "50209216"
---
# <a name="manage-cluster-scaling-to-accommodate-changing-demand"></a>Küme, değişen talepleri karşılamak için ölçekleme yönetme

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme % 100 doğrulukla tahmin edilemez. Bir statik küme boyutu eksik kullanımı veya aşırı kullanımı için hangi hiçbiri idealdir açabilir. Daha iyi bir yaklaşım *ölçek* ekleme ve kaldırma kapasitesini isteğe bağlı olarak değişen bir küme. Bu makalede küme ölçeklendirme yönetme gösterilmektedir.

Kümenize ve altında gidin **ayarları** seçin **ölçeğini**. Altında **yapılandırma**seçin **etkinleştirmek otomatik ölçeklendirme**.

![Otomatik ölçeklendirmeyi etkinleştir](media/manage-cluster-scaling/enable-autoscale.png)

Aşağıdaki grafikte, sonraki birkaç adım akışı gösterilmektedir. Daha fazla ayrıntı grafiğin altına sunuyoruz.

![Ölçek kuralı](media/manage-cluster-scaling/scale-rule.png)

1. Altında **otomatik ölçeklendirme ayarı adı**, gibi bir ad verin *genişleme: önbellek kullanımı*.

1. Altında **ölçek modu**seçin **ölçek dayalı bir ölçüme göre**. Bu mod, dinamik ölçeklendirme sağlar. belirleyebilirsiniz **belirli bir örnek sayısına ölçeklendirin**.

1. Seçin **alınabilecek**.

1. İçinde **ölçek kuralı** bölümünde sağ tarafta, her ayar için değerler sağlayın.

    | Ayar | Açıklama ve değer |
    | --- | --- | --- |
    | **Zaman toplama** | Gibi bir toplama ölçütü seçin **ortalama**. |
    | **Ölçüm adı** | Ölçeklendirme işlemi, aşağıdakiler gibi temel alınmasını istediğiniz ölçümü seçin **önbellek kullanımı**. |
    | **Zaman dilimi İstatistiği** | Arasında seçim **ortalama**, **Minimum**, **maksimum**, ve **toplam**. |
    | **İşleci** | Uygun bir seçeneği gibi belirleyin **büyüktür veya eşittir**. |
    | **Eşik** | Uygun bir değer seçin. Örneğin, önbellek kullanımı için iyi bir başlangıç noktası % 80'idir. |
    | **Süresi** | Uygun miktarda bir sistemin geri ölçümleri hesaplanırken aramak saati seçin. Varsayılan on dakika ile başlayın. |
    | **İşlem** | Ölçeklendirme veya ölçeği genişletmek için uygun seçeneği belirleyin. |
    | **Örnek sayısı** | Düğümleri veya örneklerini eklemek veya bir ölçüm koşul karşılandığında kaldırmak istiyorsanız sayısını seçin. |
    | **Seyrek erişimli (dakika)** | Ölçek işlemleri arasında beklenecek bir uygun zaman aralığı seçin. Varsayılan beş dakika ile başlayın. |
    |  |  |

1. **Add (Ekle)** seçeneğini belirleyin.

1. İçinde **örnek limitleri** bölümünde sol tarafta, her ayar için değerler sağlayın.

    | Ayar | Açıklama ve değer |
    | --- | --- | --- |
    | *En az* | Bu kümenizi aşağıdaki kullanımı bağımsız olarak ölçeği örnekleri sayısıdır. |
    | *En fazla* | Kümenizi yukarıdaki kullanımı bağımsız olarak ölçeği örnekleri sayısıdır. |
    | *Varsayılan* | Kaynak ölçümlerin okunmasıyla ilgili bir sorun varsa kullanılan örnekler, varsayılan sayısı. |
    |  |  |

1. **Kaydet**’i seçin.

Bir ölçek genişletme işlemi, Azure Veri Gezgini kümeniz şimdi yapılandırdınız. Bir ölçeklendirme işlemi için başka bir kural ekleyin. Bu, kümenizin ölçeğini dinamik olarak belirttiğiniz kullanım ölçümlerine bağlı sağlar.

Küme ölçeklendirme sorunları ile ilgili yardıma ihtiyacınız varsa, Lütfen bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).