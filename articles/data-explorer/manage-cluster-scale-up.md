---
title: Değişen talepleri karşılamak için bir Azure Veri Gezgini kümenin ölçeğini artırdığınızda
description: Bu makalede, ölçeğini ve değişen isteğe bağlı bir Azure Veri Gezgini kümesinin ölçeğini adımlar açıklanmaktadır.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: 9c3eb82f09c591f313175ef564b1a20075fdcbd4
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537883"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Küme ölçeklendirme-yukarı değişen talepleri karşılamak için yönetme

Bir Azure Veri Gezgini küme ölçeklendirme için iki iş akışı: ölçek büyütme ve [genişleme](manage-cluster-scale-out.md). Bu makalede, küme ölçeği artırma yönetme gösterilmektedir.

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme mutlak doğrulukla tahmin edilemez. Statik küme boyutu için Yardım'ı veya overutilization, hangi hiçbiri idealdir açabilir. Daha iyi bir yaklaşım *ölçek* kapasitesi ve isteğe bağlı olarak değişen CPU kaynakları ekleyerek veya kaldırarak bir küme. 

## <a name="steps-to-scale-up"></a>Ölçeği artırma işleminin adımları

1. Kümenize gidin. Altında **ayarları**seçin **ölçeği**.

    Kullanılabilen SKU'ları listesi gösterilir. Örneğin, aşağıdaki şekilde, yalnızca dört SKU'ları kullanılabilir.

    ![Ölçeği artırma](media/manage-cluster-scale-up/scale-up.png)

    Geçerli SKU oldukları veya kümenin bulunduğu bölgede kullanılabilir değil çünkü bu SKU'ları devre dışı bırakıldı.

1. SKU'nuz değiştirmek için seçin ve istediğiniz SKU'yu seçin **seçin** düğmesi.

> [!NOTE]
> Ölçek büyütme işlemi birkaç dakika sürebilir ve bu sırada, kümenizin askıya alınacaktır. Ölçeği küme performansınızı zarar verebilir unutmayın.

Bir ölçek artırma veya azaltma işlemi, Azure Veri Gezgini kümeniz şimdi yaptık.

Küme ölçeklendirme sorunlarla ilgili yardıma ihtiyacınız varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) Azure portalında.

## <a name="next-steps"></a>Sonraki adımlar

* [Küme ölçeklendirme yönetme](manage-cluster-scale-out.md) çıkış örnek sayısı, belirttiğiniz ölçümlere dayalı dinamik olarak ölçeklendirmeniz mi.

* Bu makaleyi izleyerek kaynak kullanımınızı izleyin: [Azure Veri Gezgini performans, sistem durumu ve ölçümler ile kullanımı izlemenize](using-metrics.md).

