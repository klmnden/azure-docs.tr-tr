---
title: Bir Azure Veri Gezgini kümesini ölçeklendirme
description: Ölçeği genişletme ve ölçeklendirmek için adımlar bu makalede bir Azure Veri Gezgini kümesinde göre değişen isteğe bağlı.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: 29bfcc42462a667850f0b2e1bbda3d29cd1597ab
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571523"
---
# <a name="manage-cluster-horizontal-scaling-to-accommodate-changing-demand"></a>Küme değişen talepleri karşılamak için yatay ölçeklendirmeyi yönetme

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme mutlak doğrulukla tahmin edilemez. Statik küme boyutu için Yardım'ı veya overutilization, hangi hiçbiri idealdir açabilir.

Daha iyi bir yaklaşım *ölçek* ekleme ve kaldırma kapasitesini isteğe bağlı olarak değişen bir küme. Ölçeklendirme için iki iş akışı vardır: 
* Yatay ölçeklendirme, olarak da adlandırılır ve ölçeklendirme.
* Dikey ölçeklendirme, olarak da adlandırılır yukarı ve aşağı ölçeklendirme.

Bu makalede, yatay ölçeklendirme iş akışını açıklar.

Yatay ölçeklendirme, örnek sayısını otomatik olarak önceden tanımlanmış kurallar ve zamanlamaları göre ölçeklendirmenize olanak tanır. Bu makalede açıklanan Azure portalında kümenizin otomatik ölçeklendirme ayarlarınızı belirtin.

## <a name="steps-to-configure-horizontal-scaling"></a>Yatay ölçeklendirme yapılandırma adımları

Azure portalında Veri Gezgini küme kaynağınıza gidin. Altında **ayarları** başlığı seçin **ölçeğini**. 

İstenen otomatik ölçeklendirme yönteminizi seçin: **El ile ölçeklendirmenin**, **otomatik ölçeklendirme en iyi duruma getirilmiş** veya **özel otomatik ölçeklendirme**.

### <a name="manual-scale"></a>El ile ölçeklendirme

El ile ölçek kümesi oluşturma ile varsayılan ayardır. Küme anlamına otomatik olarak değişmez statik küme kapasitesi vardır. Çubuğunu kullanarak statik kapasiteyi seçebilir ve sonraki ayarı bir kümenin ölçeğini değiştirir kadar değiştirmez.

   ![El ile ölçeklendirmenin yöntemi](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale"></a>En iyi duruma getirilmiş otomatik ölçeklendirme

En iyi duruma getirilmiş otomatik ölçeklendirme için önerilen otomatik ölçeklendirme yöntemdir. En iyi duruma getirilmiş otomatik ölçeklendirme yapılandırma adımları:

1. En iyi duruma getirilmiş otomatik ölçeklendirme seçeneği ve daha düşük bir sınır ve küme örneklerini miktarı için üst sınır seçin, sonra da bu sınırlar arasında otomatik ölçeklendirme yapılır.
2. Kaydet’e tıklayın.

   ![En iyi duruma getirilmiş otomatik ölçeklendirme yöntemi](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

En iyi duruma getirilmiş otomatik ölçeklendirme mekanizması tıklayarak çalışmaya başlar ve olduktan sonra eylemleri küme etkinlik günlüğünde görünür olur. Bu otomatik ölçeklendirme yöntemi, maliyetleri ve küme performansı iyileştirme: küme için aynı ve daha düşük maliyetler performans bırakın, ölçeği açma işleminin durumunu almak başlar ve küme için durumunu almak başlar overutilization, iyi durumda olduğundan emin olmak için genişletilmiş olur

### <a name="custom-autoscale"></a>Özel bir otomatik ölçeklendirme

Özel ölçeklendirme yöntemi sayesinde kümenizi dinamik olarak belirttiğiniz ölçümlere göre ölçeklendirin. Aşağıdaki grafikte, flow ve özel otomatik ölçeklendirme yapılandırma adımları gösterilmektedir. Daha ayrıntılı bilgi grafiği izleyin.

1. İçinde **otomatik ölçeklendirme ayarı adı** kutusunda, gibi bir ad verin *genişleme: önbellek kullanımı*. 

   ![Ölçek kuralı](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

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

Bir ölçek genişletme işlemi, Azure Veri Gezgini kümeniz şimdi yapılandırdınız. Bir ölçeklendirme işlemi için başka bir kural ekleyin. Küme ölçeklendirme sorunlarla ilgili yardıma ihtiyacınız varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) Azure portalında.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gezgini performansını, sistem durumu ve kullanım ölçümleri ile izleme](using-metrics.md)
* [Küme dikey olarak ölçeklendirmeyi yönetmeniz](manage-cluster-vertical-scaling.md) bir küme uygun boyutlandırması için.
