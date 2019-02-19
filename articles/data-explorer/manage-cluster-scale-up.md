---
title: Değişen talepleri karşılamak için ölçekleme Azure Veri Gezgini küme
description: Bu makalede, ölçek büyütme ve ölçek azaltma değişen isteğe bağlı bir Azure Veri Gezgini kümesi için adımlar açıklanmaktadır.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 9d265ec7f0ce2030874f38b99b07343f1d4a3f4d
ms.sourcegitcommit: 4bf542eeb2dcdf60dcdccb331e0a336a39ce7ab3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56408655"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Küme ölçeklendirme-yukarı değişen talepleri karşılamak için yönetme

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme % 100 doğrulukla tahmin edilemez. Bir statik küme boyutu eksik kullanımı veya aşırı kullanımı için hangi hiçbiri idealdir açabilir. Daha iyi bir yaklaşım *ölçek* bir kümeye ekleme ve değişen isteğe bağlı kapasite ve CPU kaldırma. Ölçeklendirme için iki iş akışları ölçek büyütme ve ölçek genişletme vardır. Bu makalede küme ölçeği artırma yönetme gösterilmektedir.

1. Kümenize ve altında gidin **ayarları** seçin **ölçeği**.

    Size kullanılabilen SKU'ları bir listesi verilir. Örneğin, aşağıdaki şekilde kullanılabilir tek bir SKU vardır: D14_V2.

    ![Ölçeği artırma](media/manage-cluster-scale-up/scale-up.png)

    Geçerli SKU küme olduğundan D13_V2 devre dışı bırakıldı. L8 ve L16 devre dışı bırakıldı çünkü bunlar kümenin bulunduğu bölgede kullanılamıyor.

1. SKU'nuz değiştirmek için tuşuna basın ve istediğiniz SKU'yu seçin **seçin** düğmesi.

> [!NOTE]
> Ölçeği artırma işlemi birkaç dakika sürebilir. Bu süre boyunca, kümenizi askıya alınır.
>
> Ölçeği, küme performansınızı zarar verebilir.
>

Bir ölçek artırma veya azaltma işlemi, Azure Veri Gezgini kümeniz şimdi gerçekleştirdiğiniz. Bunu da yapabilirsiniz [küme ölçeklendirme](manage-cluster-scale-out.md), belirttiğiniz olan ölçümler temelinde dinamik olarak ölçeklendirmek için otomatik ölçeklendirme, olarak da bilinir.

Küme ölçeklendirme sorunları ile ilgili yardıma ihtiyacınız varsa, bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
