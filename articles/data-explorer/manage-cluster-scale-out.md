---
title: Değişen talepleri karşılamak için ölçekleme Azure Veri Gezgini küme
description: Bu makalede, Ölçek genişletme ve ölçek-değişen isteğe bağlı bir Azure Veri Gezgini küme içindeki adımları açıklanmaktadır.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 15ef5282e0a073e870f2ac12b5fc442407535770
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408451"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Küme değişen talepleri karşılamak için genişleme yönetme

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme % 100 doğrulukla tahmin edilemez. Bir statik küme boyutu eksik kullanımı veya aşırı kullanımı için hangi hiçbiri idealdir açabilir. Daha iyi bir yaklaşım *ölçek* ekleme ve kaldırma kapasitesini isteğe bağlı olarak değişen bir küme. Ölçeklendirme için iki iş akışları ölçek büyütme ve ölçek genişletme vardır. Bu makalede, ölçeği genişletilen iş akışı açıklanır.

Bu makalede küme ölçek genişletme, otomatik olarak da bilinen ölçeklendirme yönetme gösterilmektedir. Otomatik ölçeklendirme sayesinde genişleme örnek sayısı otomatik olarak önceden tanımlanmış kurallar ve zamanlamaları göre. Aşağıda açıklandığı gibi Azure portalında kümenizin otomatik ölçeklendirme ayarlarını belirleyin.

Kümenize ve altında gidin **ayarları** seçin **ölçeğini**. Altında **yapılandırma**seçin **etkinleştirmek otomatik ölçeklendirme**.

![Otomatik ölçeklendirmeyi etkinleştir](media/manage-cluster-scaling/enable-autoscale.png)

Aşağıdaki grafikte, sonraki birkaç adım akışı gösterilmektedir. Daha fazla ayrıntı grafiğin altına sunuyoruz.

![Ölçek kuralı](media/manage-cluster-scaling/scale-rule.png)

1. Altında **otomatik ölçeklendirme ayarı adı**, gibi bir ad verin *genişleme: önbellek kullanımı*.

1. Altında **ölçek modu**seçin **ölçek dayalı bir ölçüme göre**. Bu mod, dinamik ölçeklendirme sağlar. belirleyebilirsiniz **belirli bir örnek sayısına ölçeklendirin**.

1. Seçin **alınabilecek**.

1. İçinde **ölçek kuralı** bölümünde sağ tarafta, her ayar için değerler sağlayın.

    **Ölçütler**

    | Ayar | Açıklama ve değer |
    | --- | --- | --- |
    | **Zaman toplama** | Gibi bir toplama ölçütü seçin **ortalama**. |
    | **Ölçüm adı** | Ölçeklendirme işlemi, aşağıdakiler gibi temel alınmasını istediğiniz ölçümü seçin **önbellek kullanımı**. |
    | **Zaman dilimi İstatistiği** | Arasında seçim **ortalama**, **Minimum**, **maksimum**, ve **toplam**. |
    | **İşleci** | Uygun bir seçeneği gibi belirleyin **büyüktür veya eşittir**. |
    | **Eşik** | Uygun bir değer seçin. Örneğin, önbellek kullanımı için iyi bir başlangıç noktası % 80'idir. |
    | **Süre (dakika cinsinden)** | Uygun miktarda bir sistemin geri ölçümleri hesaplanırken aramak saati seçin. Varsayılan 10 dakika ile başlayın. |
    |  |  |

    **Eylem**

    | Ayar | Açıklama ve değer |
    | --- | --- | --- |
    | **İşlem** | Ölçek veya ölçek genişletme için uygun seçeneği belirleyin. |
    | **Örnek sayısı** | Düğümleri veya örneklerini eklemek veya bir ölçüm koşul karşılandığında kaldırmak istiyorsanız sayısını seçin. |
    | **Seyrek erişimli (dakika)** | Ölçek işlemleri arasında beklenecek bir uygun zaman aralığı seçin. Varsayılan beş dakika ile başlayın. |
    |  |  |

1. **Add (Ekle)** seçeneğini belirleyin.

1. İçinde **örnek limitleri** bölümünde sol tarafta, her ayar için değerler sağlayın.

    | Ayar | Açıklama ve değer |
    | --- | --- | --- |
    | *En az* | Kümenizi aşağıdaki bağımsız olarak kullanımı Ölçekle örnek sayısı. |
    | *En fazla* | Kümenizi yukarıdaki kullanımı bağımsız olarak ölçeklendirme olmaz örnek sayısı. |
    | *Varsayılan* | Kaynak ölçümlerin okunmasıyla ilgili sorun varsa kullanılan örnekler, varsayılan sayısı. |
    |  |  |

1. **Kaydet**’i seçin.

Bir ölçek genişletme işlemi, Azure Veri Gezgini kümeniz şimdi yapılandırdınız. Bir ölçeklendirme işlemi için başka bir kural ekleyin. Bu, kümenizin ölçeğini dinamik olarak belirttiğiniz ölçümlere göre sağlar.

Bunu da yapabilirsiniz [ölçek kümesi oluşturan](manage-cluster-scale-up.md) bir küme uygun boyutlandırması için.

Küme ölçeklendirme sorunları ile ilgili yardıma ihtiyacınız varsa, bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
