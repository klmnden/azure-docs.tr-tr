---
title: Azure Otomasyonu hesabı ile Log Analytics arasındaki bağlantıyı kaldırma
description: Bu makalede, Azure Otomasyonu hesabınızdan bir Log Analytics çalışma alanı bağlantısının nasıl genel bir bakış sağlar.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 08/01/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 1328ce8c306188c32bce5385f58f118a63c08deb
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39426542"
---
# <a name="how-to-unlink-your-automation-account-from-a-log-analytics-workspace"></a>Otomasyon hesabınızdan bir Log Analytics çalışma alanı bağlantısının nasıl

Azure Otomasyonu, runbook işlerinin tüm otomasyon hesaplarınız genelinde izleme yalnızca desteklemek için Log Analytics ile tümleştirilir ancak Log Analytics temelinde bağımlı olan aşağıdaki çözümlerden içeri aktardığınızda de gereklidir:

* [Güncelleştirme yönetimi](../operations-management-suite/oms-solution-update-management.md)
* [Değişiklik İzleme](../log-analytics/log-analytics-change-tracking.md)
* [Vm'leri çalışma saatleri dışında başlatma/durdurma](automation-solution-vm-management.md)

Log Analytics ile Otomasyon hesabınızı tümleştirmek istediğiniz karar verirseniz, doğrudan Azure portalından hesabınızı kesebilir.  Devam etmeden önce öncelikle daha önce bahsedilen çözümleri kaldırmanız gerekir, bu işlem devam etmeden gelen Aksi takdirde engellenir. Kaldırmak için gerekli adımları anlamak için alınan belirli çözüm makalesini gözden geçirin.

Bu çözümleri kaldırdıktan sonra Otomasyon hesabının bağlantısını kaldırmak için aşağıdaki adımları gerçekleştirebilirsiniz.

> [!NOTE]
> Azure SQL izleme çözümünün önceki sürümleri dahil olmak üzere bazı çözümler Otomasyon varlıklarından oluşturmuş olabilir ve ayrıca çalışma alanının bağlantısı kaldırılıyor önce kaldırılması gerekebilir.

## <a name="unlink-workspace"></a>Çalışma alanının bağlantısını Kaldır

1. Azure portalından Otomasyon hesabınızı açın ve sayfa seçin üzerinde Otomasyon hesabı **bağlantılı çalışma** bölümünde **ilgili kaynakları** soldaki.

1. Bağlantıyı kaldır çalışma sayfasında tıklayın **çalışma alanının bağlantısını Kaldır**.

   ![Çalışma sayfası bağlantısını Kaldır](media/automation-unlink-from-log-analytics/automation-unlink-workspace-blade.png).

   Devam etmek istediğinizi doğrulayan bir ileti alacaksınız.

1. Azure Otomasyonu hesabı Log Analytics çalışma alanınızın bağlantısını dener, ancak altında ilerleme durumunu izleyebilirsiniz **bildirimleri** menüsünde.

İsteğe bağlı olarak, güncelleştirme yönetimi çözümünü kullandıysanız, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Güncelleştirme zamanlamaları - her oluşturduğunuz güncelleştirme dağıtımlarının eşleşen adlara sahip)

* Çözüm için - oluşturulan karma çalışan grupları her benzer şekilde machine1.contoso.com_9ceb8108 - 26 c 9-4051-b6b3-227600d715c8 için olarak adlandırılır).

İsteğe bağlı olarak, yoğun olmayan saatlerde çözüm sırasında Vm'leri başlatma/durdurma kullandıysanız, çözümü kaldırdıktan sonra artık gerekli olmayan aşağıdaki öğeleri kaldırmak isteyebilirsiniz.

* Başlatma ve durdurma VM runbook zamanlama
* VM runbook'ları durdurun ve başlatın
* Değişkenler

## <a name="next-steps"></a>Sonraki adımlar

Log Analytics ile tümleştirmek için Otomasyon hesabınızı yeniden yapılandırmak için bkz: [Otomasyon iş durumunu ve iş akışları Log Analytics'e iletme](automation-manage-send-joblogs-log-analytics.md).