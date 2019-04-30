---
author: rockboyfor
ms.service: virtual-machines
ms.topic: include
origin.date: 10/26/2018
ms.date: 11/26/2018
ms.author: v-yeche
ms.openlocfilehash: 1c3996c3f40da496af0cd795d0873864667a1f19
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62104089"
---
## <a name="use-the-azure-portal"></a>Azure portalı kullanma
1. Yeniden dağıtın ve ardından istediğiniz VM'yi seçin *yeniden* düğmesine *ayarları* dikey penceresi. Görmek için aşağı kaydırmanız gerekebilir **destek ve sorun giderme** aşağıdaki örnekte olduğu gibi 'Yeniden Dağıt' düğmesi içeren bölümü:

    ![Azure VM dikey penceresi](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. İşlemi doğrulamak için şunu seçin *yeniden* düğmesi:

    ![VM dikey penceresinde yeniden dağıtma](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. **Durumu** sanal makinenin değişiklikleri *güncelleştirme* VM yeniden dağıtmak hazırlar aşağıdaki örnekte gösterildiği gibi:

    ![Sanal makine güncelleştiriliyor](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. **Durumu** ardından değişikliklerini *başlangıç* VM üzerinde yeni bir Azure konağına, aşağıdaki örnekte gösterildiği gibi önyükleme gibi:

    ![VM başlatılıyor](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Sanal Makinenin önyükleme işlemi tamamlandıktan sonra **durumu** ardından döndürür *çalıştıran*, VM belirten başarıyla yeniden dağıtıldı:

    ![VM çalışıyor](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

<!-- Update_Description: update meta properties -->