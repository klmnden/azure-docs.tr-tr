---
title: Değişen talepleri karşılamak için bir Azure Veri Gezgini kümenin ölçeğini artırdığınızda
description: Bu makalede, ölçeğini ve değişen isteğe bağlı bir Azure Veri Gezgini kümesinin ölçeğini adımlar açıklanmaktadır.
author: radennis
ms.author: radennis
ms.reviewer: orspodek
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 565953c8e0c6f9765d5eeb16a9fa18c3e79b8370
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59044944"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Küme ölçeklendirme-yukarı değişen talepleri karşılamak için yönetme

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme mutlak doğrulukla tahmin edilemez. Statik küme boyutu için Yardım'ı veya overutilization, hangi hiçbiri idealdir açabilir.

Daha iyi bir yaklaşım *ölçek* kapasitesi ve isteğe bağlı olarak değişen CPU kaynakları ekleyerek veya kaldırarak bir küme. Ölçeklendirme için iki iş akışı: ölçek büyütme ve ölçek genişletme. Bu makalede, küme ölçeği artırma yönetme gösterilmektedir.

1. Kümenize gidin. Altında **ayarları**seçin **ölçeği**.

    Kullanılabilen SKU'ları listesi gösterilir. Örneğin, aşağıdaki şekilde, yalnızca dört SKU'ları kullanılabilir.

    ![Ölçeği artırma](media/manage-cluster-scale-up/scale-up.png)

    Geçerli SKU oldukları veya kümenin bulunduğu bölgede kullanılabilir değil çünkü bu SKU'ları devre dışı bırakıldı.

1. SKU'nuz değiştirmek için seçin ve istediğiniz SKU'yu seçin **seçin** düğmesi.

> [!NOTE]
> Ölçek büyütme işlemi birkaç dakika sürebilir ve bu sırada, kümenizin askıya alınacaktır. Ölçeği küme performansınızı zarar verebilir unutmayın.

Bir ölçek artırma veya azaltma işlemi, Azure Veri Gezgini kümeniz şimdi yaptık. Ayrıca [küme ölçeklendirme yönetme](manage-cluster-scale-out.md) çıkış örnek sayısı, belirttiğiniz ölçümlere dayalı dinamik olarak ölçeklendirmeniz mi.

Küme ölçeklendirme sorunlarla ilgili yardıma ihtiyacınız varsa [bir destek isteği açın](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) Azure portalında.
