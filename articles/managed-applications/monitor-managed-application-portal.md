---
title: Yönetilen bir uygulama izlemek için Azure portalını kullanma | Microsoft Docs
description: Kullanılabilirlik ve Uyarılar için yönetilen bir uygulama izlemek için Azure portalını kullanmayı gösterir.
services: managed-applications
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 10/04/2018
ms.author: tomfitz
ms.openlocfilehash: ea9f55d7c6002aaba2fd4fdc2a76b93f98e1d6b0
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/05/2018
ms.locfileid: "48811528"
---
# <a name="monitor-a-deployed-instance-of-a-managed-application"></a>Dağıtılan bir yönetilen uygulama örneğini izleyin

Azure aboneliğinizde bir yönetilen uygulamayı dağıttıktan sonra uygulama durumunu denetlemek isteyebilirsiniz. Bu makalede, durumunu denetlemek için Azure portalında seçenekler gösterilmektedir. Yönetilen bir uygulamada kaynakların kullanılabilirliğini izleyebilirsiniz. Ayrıca, ayarlamak ve Uyarıları görüntüleyebilirsiniz.

## <a name="view-resource-health"></a>Kaynak durumunu görüntüleme

1. Yönetilen uygulama örneğinizi seçin.

   ![Yönetilen uygulama seçin](./media/monitor-managed-application-portal/select-managed-application.png)

1. Seçin **kaynak durumu**.

   ![Kaynak durumu seçin](./media/monitor-managed-application-portal/select-resource-health.png)

1. Yönetilen bir uygulamada kaynakların kullanılabilirliğini görüntüleyin.

   ![Kaynak durumunu görüntüleme](./media/monitor-managed-application-portal/view-health.png)

## <a name="view-alerts"></a>Uyarıları görüntüleme

1. Seçin **uyarılar**.

   ![Uyarıları seçin](./media/monitor-managed-application-portal/select-alerts.png)

1. Yapılandırılmış uyarı kuralları varsa gündeme gelmiş uyarılar hakkındaki bilgileri görebilirsiniz.

   ![Uyarıları görüntüleme](./media/monitor-managed-application-portal/view-alerts.png)

1. Uyarı kuralları eklemek için seçin **+ yeni uyarı kuralı**.

   ![Uyarı oluşturma](./media/monitor-managed-application-portal/create-new-alert.png)

Yönetilen uygulama örneğinizi veya yönetilen uygulamayı kaynakları için uyarılar oluşturabilirsiniz. Uyarılar oluşturma hakkında daha fazla bilgi için bkz: [Microsoft azure'da uyarılara genel bakış](../monitoring-and-diagnostics/monitoring-overview-alerts.md).

## <a name="next-steps"></a>Sonraki adımlar

* Yönetilen uygulama örnekleri için bkz. [örnek projeler için Azure yönetilen uygulamalar](sample-projects.md).
* Bir yönetilen uygulamayı dağıtmak için bkz. [Dağıt hizmet Kataloğu uygulaması Azure Portalı aracılığıyla](deploy-service-catalog-quickstart.md).