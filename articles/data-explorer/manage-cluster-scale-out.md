---
title: Değişen talepleri karşılamak için ölçekleme Azure Veri Gezgini küme
description: Ölçeği genişletme ve ölçeklendirmek için adımlar bu makalede bir Azure Veri Gezgini kümesinde göre değişen isteğe bağlı.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 9b54bf182f23eceb47c392059ff52c04bf0a8aed
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755080"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Küme değişen talepleri karşılamak için genişleme yönetme

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme mutlak doğrulukla tahmin edilemez. Statik küme boyutu için Yardım'ı veya overutilization, hangi hiçbiri idealdir açabilir.

Daha iyi bir yaklaşım *ölçek* ekleme ve kaldırma kapasitesini isteğe bağlı olarak değişen bir küme. Ölçeklendirme için iki iş akışı: ölçek büyütme ve ölçek genişletme. Bu makalede, ölçeği genişletilen iş akışı açıklanır.

Bu makalede, küme ölçek genişletme, otomatik olarak da bilinen ölçeklendirme yönetme gösterilmektedir. Otomatik ölçeklendirme, otomatik olarak önceden tanımlanmış kurallar ve zamanlamaları göre örnek sayısını ölçeklendirmek sağlar. Bu makalede açıklanan Azure portalında kümenizin otomatik ölçeklendirme ayarlarınızı belirtin.

Kümenize gidin. Altında **ayarları**seçin **ölçeğini**. Altında **yapılandırma**seçin **etkinleştirmek otomatik ölçeklendirme**.

![Otomatik ölçeklendirmeyi etkinleştir](media/manage-cluster-scaling/enable-autoscale.png)

Aşağıdaki grafikte, sonraki birkaç adım akışı gösterilmektedir. Daha fazla ayrıntı Grafiği ' dir.

![Ölçek kuralı](media/manage-cluster-scaling/scale-rule.png)

1. İçinde **otomatik ölçeklendirme ayarı adı** kutusunda, gibi bir ad verin *genişleme: önbellek kullanımı*.

1. İçin **ölçek modu**seçin **ölçek dayalı bir ölçüme göre**. Bu mod, dinamik ölçeklendirme sağlar. Belirleyebilirsiniz **belirli bir örnek sayısına ölçeklendirin**.

1. Seçin **+ alınabilecek**.

1. İçinde **ölçek kuralı** bölümünde sağ tarafta, her ayar için değerler sağlayın.

    **Ölçütler**

    | Ayar | Açıklama ve değer |
    | --- | --- |
    | **Zaman toplama** | Gibi bir toplama ölçütü seçin **ortalama**. |
    | **Ölçüm adı** | Ölçeklendirme işlemi, aşağıdakiler gibi temel alınmasını istediğiniz ölçümü seçin **önbellek kullanımı**. |
    | **Zaman dilimi İstatistiği** | Arasında seçim **ortalama**, **Minimum**, **maksimum**, ve **toplam**. |
    | **İşleci** | Uygun bir seçeneği gibi belirleyin **büyüktür veya eşittir**. |
    | **Eşik** | Uygun bir değer seçin. Örneğin, önbellek kullanımı için yüzde 80'i iyi bir başlangıç noktası ' dir. |
    | **Süre (dakika cinsinden)** | Uygun miktarda bir sistemin geri ölçümleri hesaplanırken aramak saati seçin. Varsayılan 10 dakika ile başlayın. |
    |  |  |

    **Eylem**

    | Ayar | Açıklama ve değer |
    | --- | --- |
    | **İşlem** | Ölçeklendirme veya ölçeği genişletmek için uygun seçeneği belirleyin. |
    | **Örnek sayısı** | Düğümleri veya örneklerini eklemek veya bir ölçüm koşul karşılandığında kaldırmak istiyorsanız sayısını seçin. |
    | **Seyrek erişimli (dakika)** | Ölçek işlemleri arasında beklenecek bir uygun zaman aralığı seçin. Varsayılan beş dakika ile başlayın. |
    |  |  |

1. **Add (Ekle)** seçeneğini belirleyin.

1. İçinde **örnek limitleri** bölümünde sol tarafta, her ayar için değerler sağlayın.

    | Ayar | Açıklama ve değer |
    | --- | --- |
    | **En az** | Kümenizi aşağıdaki bağımsız olarak kullanımı Ölçekle örnek sayısı. |
    | **En fazla** | Kümenizi yukarıdaki kullanımı bağımsız olarak ölçeklendirme olmaz örnek sayısı. |
    | **Varsayılan** | Varsayılan örnek sayısı. Kaynak ölçümlerin okunmasıyla ile ilgili sorun varsa, bu ayar kullanılır. |
    |  |  |

1. **Kaydet**’i seçin.

Bir ölçek genişletme işlemi, Azure Veri Gezgini kümeniz şimdi yapılandırdınız. Bir ölçeklendirme işlemi için başka bir kural ekleyin. Bu yapılandırma, dinamik olarak belirttiğiniz ölçümlere göre kümenizi ölçeklendirme sağlar.

Ayrıca [küme ölçeği artırma yönetme](manage-cluster-scale-up.md) bir küme uygun boyutlandırması için.

Küme ölçeklendirme sorunlarla ilgili yardıma ihtiyacınız varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) Azure portalında.
