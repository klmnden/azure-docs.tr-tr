---
title: Azure Lab Services’teki bir laboratuvarın kullanımını izleme | Microsoft Docs
description: Bu öğreticide laboratuvar oluşturan/sahibi olarak laboratuvarınızın kullanımını izleyeceksiniz.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: e2831191905da1b9e0ad55131be9eaa7aa13950e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60777631"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Öğretici: Azure Laboratuvar servisi laboratuvarda kullanımını izleyin
Bu öğreticide bir laboratuvar oluşturanın/sahibinin bir laboratuvarın kullanımını nasıl izleyebileceği gösterilmektedir.

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvarınıza kayıtlı kullanıcıları görüntüleme
> * Laboratuvarınızdaki VM'lerin kullanımını görüntüleme
> * Öğrenci VM'lerini yönetme 


## <a name="view-users-registered-with-the-lab"></a>Laboratuvara kayıtlı kullanıcıları görüntüleme

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. 
2. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler.
3. **Laboratuvarlarım** sayfasında kullanımını izlemek istediğiniz laboratuvarı seçin. 
4. Seçin **kullanıcılar** sol menüde veya **kullanıcılar** Döşe. Laboratuvarınıza kayıtlı öğrencileri görürsünüz. **Kayıt bağlantısı**'nı seçin, bağlantıyı kopyalayın ve henüz laboratuvarınıza kaydolmamış yeni öğrencilere gönderin. 

    ![Kayıtlı kullanıcılar](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Laboratuvarınızdaki VM'lerin kullanımını görüntüleme 

1. Soldaki menüden **Sanal makineler**'i seçin. 
2. VM'lerin durumunu ve VM'lerin çalışma süresini gördüğünüzden emin olun. Laboratuvar sahibi bir öğrenci üzerinde harcadığı zamanı son sütununda gösterilen kullanım zamana karşı VM sayılmaz. 

    ![VM kullanımı](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Öğrenci VM'lerini yönetme 
Fare bir sanal makinenin üzerinde listedeki satıra getirin (önceki bölümde görüntüde gösterildiği gibi) aşağıdaki görevleri gerçekleştirmek için denetimleri görmeniz: 

- Bir VM’ye bağlanma
- VM başlatma
- VM durdurma
- VM silme


![VM denetimleri](../media/tutorial-track-usage/vm-controls.png)

Araç çubuğu düğmeleri, başlatma, durdurma veya VM silme için de kullanabilirsiniz. 



## <a name="next-steps"></a>Sonraki adımlar
Sınıf laboratuvarlarını hakkında daha fazla bilgi için altında makalelerine bakın [nasıl yapılır kılavuzları](how-to-manage-lab-accounts.md).
