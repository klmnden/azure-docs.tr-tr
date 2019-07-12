---
title: Değişen talepleri karşılamak için bir Azure Veri Gezgini kümenin ölçeğini artırdığınızda
description: Bu makalede, ölçeğini ve değişen isteğe bağlı bir Azure Veri Gezgini kümesinin ölçeğini adımlar açıklanmaktadır.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: dc9ca8bb592e699d19835efeafb91e81408ae297
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571536"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Küme ölçeklendirme-yukarı değişen talepleri karşılamak için yönetme

Bir Azure Veri Gezgini küme ölçeklendirme için iki iş akışı vardır:
1. [Yatay ölçeklendirme](manage-cluster-horizontal-scaling.md), olarak da bilinir ve ölçeklendirme.
2. Dikey ölçeklendirme, olarak da adlandırılır yukarı ve aşağı ölçeklendirme.

Bu makalede, küme dikey ölçeklendirme yönetme gösterilmektedir.

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme mutlak doğrulukla tahmin edilemez. Statik küme boyutu için Yardım'ı veya overutilization, hangi hiçbiri idealdir açabilir. Daha iyi bir yaklaşım *ölçek* kapasitesi ve isteğe bağlı olarak değişen CPU kaynakları ekleyerek veya kaldırarak bir küme. 

## <a name="steps-to-configure-vertical-scaling"></a>Dikey ölçeklendirme yapılandırma adımları

1. Kümenize gidin. Altında **ayarları**seçin **ölçeği**.

    Kullanılabilen SKU'ları listesi gösterilir. Örneğin, aşağıdaki şekilde, yalnızca dört SKU'ları kullanılabilir.

    ![Ölçeği artırma](media/manage-cluster-vertical-scaling/scale-up.png)

    Geçerli SKU oldukları veya kümenin bulunduğu bölgede kullanılabilir değil çünkü bu SKU'ları devre dışı bırakıldı.

1. SKU'nuz değiştirmek için seçin ve istediğiniz SKU'yu seçin **seçin** düğmesi.

> [!NOTE]
> Dikey ölçeklendirme işlemi birkaç dakika sürebilir ve bu sırada, kümenizin askıya alınacaktır. Ölçeği küme performansınızı zarar verebilir unutmayın.

Bir ölçek artırma veya azaltma işlemi, Azure Veri Gezgini kümeniz şimdi yaptık.

Küme ölçeklendirme sorunlarla ilgili yardıma ihtiyacınız varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) Azure portalında.

## <a name="next-steps"></a>Sonraki adımlar

* [Küme yatay ölçeklendirmeyi yönetmeniz](manage-cluster-horizontal-scaling.md) çıkış örnek sayısı, belirttiğiniz ölçümlere dayalı dinamik olarak ölçeklendirmeniz mi.

* Bu makaleyi izleyerek kaynak kullanımınızı izleyin: [Azure Veri Gezgini performans, sistem durumu ve ölçümler ile kullanımı izlemenize](using-metrics.md).

