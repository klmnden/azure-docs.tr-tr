---
title: Değişen talepleri karşılamak için ölçekleme Azure Veri Gezgini küme
description: Bu makalede, Ölçek genişletme ve ölçek-değişen isteğe bağlı bir Azure Veri Gezgini küme içindeki adımları açıklanmaktadır.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 213a49d87d5e9f801bb17604a322c231a4e3dee2
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735819"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Küme ölçek yönetmek değişen talepleri karşılamak

Bir küme uygun şekilde boyutlandırma Azure Veri Gezgini performansı için kritik öneme sahiptir. Ancak, isteğe bağlı olarak bir küme % 100 doğrulukla tahmin edilemez. Bir statik küme boyutu eksik kullanımı veya aşırı kullanımı için hangi hiçbiri idealdir açabilir. Daha iyi bir yaklaşım *ölçek* bir kümeye ekleme ve değişen isteğe bağlı kapasite ve CPU kaldırma. Bu makalede, küme ölçeğinde yönetme işlemini göstermektedir ayarlama.

Kümenize ve altında gidin **ayarları** seçin **ölçeği**.

Ardından kullanılabilen SKU'ların listesini almanız. Etkin kartları listesinden seçebilirsiniz. Örneğin aşağıdaki şekilde D14_vs seçilebilir yalnızca bir SKU yoktur.

![Ölçeği artırma](media/manage-cluster-scale-up/scale-up.png)

Bu kümenin geçerli SKU olduğundan D13_v2 devre dışı bırakıldı. L8 ve L16 devre dışı bırakıldı çünkü bunlar kümenin bulunduğu bölgede kullanılamıyor.

SKU'SUNDA bir SKU tıklamanız yeterlidir değiştirmek için tıklayın ve kullanmak istediğiniz **seçin** düğmesi.

[!NOTE] Ölçeği artırma işlemi birkaç dakika sürebilir ve bu sırada, kümenizin askıya alınacaktır. Ölçeği küme performansınızı zarar verebilir unutmayın.

Küme ölçeklendirme sorunları ile ilgili yardıma ihtiyacınız varsa, bir destek isteği açın [Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).