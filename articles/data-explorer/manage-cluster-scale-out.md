---
title: Bir Azure Veri Gezgini kümesini ölçeklendirme
description: Ölçeği genişletme ve ölçeklendirmek için adımlar bu makalede bir Azure Veri Gezgini kümesinde göre değişen isteğe bağlı.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: 882f44683bbdc7f4eb49ff4912ca7a33187afbf8
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537898"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Küme değişen talepleri karşılamak için genişleme yönetme

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme mutlak doğrulukla tahmin edilemez. Statik küme boyutu için Yardım'ı veya overutilization, hangi hiçbiri idealdir açabilir.

Daha iyi bir yaklaşım *ölçek* ekleme ve kaldırma kapasitesini isteğe bağlı olarak değişen bir küme. Ölçeklendirme için iki iş akışı: ölçek büyütme ve ölçek genişletme. Bu makalede, ölçeği genişletilen iş akışı açıklanır.

Bu makalede, küme ölçek genişletme, otomatik olarak da bilinen ölçeklendirme yönetme gösterilmektedir. Otomatik ölçeklendirme, otomatik olarak önceden tanımlanmış kurallar ve zamanlamaları göre örnek sayısını ölçeklendirmek sağlar. Bu makalede açıklanan Azure portalında kümenizin otomatik ölçeklendirme ayarlarınızı belirtin.

## <a name="steps-to-configure-autoscale"></a>Otomatik ölçeklendirme yapılandırma adımları

Azure portalında Veri Gezgini küme kaynağınıza gidin. Altında **ayarları** başlığı seçin **ölçeğini**. Üzerinde **yapılandırma** sekmesinde **etkinleştirmek otomatik ölçeklendirme**.

   ![Otomatik ölçeklendirmeyi etkinleştir](media/manage-cluster-scaling/enable-autoscale.png)

Aşağıdaki grafikte, sonraki birkaç adım akışı gösterilmektedir. Daha ayrıntılı bilgi grafiği izleyin.

1. İçinde **otomatik ölçeklendirme ayarı adı** kutusunda, gibi bir ad verin *genişleme: önbellek kullanımı*. 

   ![Ölçek kuralı](media/manage-cluster-scaling/scale-rule.png)

2. İçin **ölçek modu**seçin **ölçek dayalı bir ölçüme göre**. Bu mod, dinamik ölçeklendirme sağlar. Belirleyebilirsiniz **belirli bir örnek sayısına ölçeklendirin**.

3. Seçin **+ alınabilecek**.

4. İçinde **ölçek kuralı** bölümünde sağ tarafta, her ayar için değerler sağlayın.

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

5. **Add (Ekle)** seçeneğini belirleyin.

6. İçinde **örnek limitleri** bölümünde sol tarafta, her ayar için değerler sağlayın.

    | Ayar | Açıklama ve değer |
    | --- | --- |
    | **En az** | Kümenizi aşağıdaki bağımsız olarak kullanımı Ölçekle örnek sayısı. |
    | **En fazla** | Kümenizi yukarıdaki kullanımı bağımsız olarak ölçeklendirme olmaz örnek sayısı. |
    | **Varsayılan** | Varsayılan örnek sayısı. Kaynak ölçümlerin okunmasıyla ile ilgili sorun varsa, bu ayar kullanılır. |
    |  |  |

7. **Kaydet**’i seçin.

Bir ölçek genişletme işlemi, Azure Veri Gezgini kümeniz şimdi yapılandırdınız. Bir ölçeklendirme işlemi için başka bir kural ekleyin. Bu yapılandırma, dinamik olarak belirttiğiniz ölçümlere göre kümenizi ölçeklendirme sağlar.

Küme ölçeklendirme sorunlarla ilgili yardıma ihtiyacınız varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) Azure portalında.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini performansını, sistem durumu ve kullanım ölçümleri ile izleme](using-metrics.md)

* [Küme ölçeği artırma yönetme](manage-cluster-scale-up.md) bir küme uygun boyutlandırması için.
