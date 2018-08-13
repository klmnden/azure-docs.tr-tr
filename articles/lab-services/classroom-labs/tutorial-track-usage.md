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
ms.date: 07/23/2018
ms.author: spelluru
ms.openlocfilehash: 710d157dcf4c6d060e59bcfbb69455e2ddc91bdd
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39450139"
---
# <a name="tutorial-track-usage-of-a-lab-in-azure-lab-service"></a>Öğretici: Azure Lab Services’teki bir laboratuvarın kullanımını izleme
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
4. **Kullanıcılar** sekmesini seçin. Laboratuvarınıza kayıtlı öğrencileri görürsünüz. **Kayıt bağlantısı**'nı seçin, bağlantıyı kopyalayın ve henüz laboratuvarınıza kaydolmamış yeni öğrencilere gönderin. 

    ![Kayıtlı kullanıcılar](../media/tutorial-track-usage/registered-users.png)

## <a name="view-the-usage-of-vms-in-the-lab"></a>Laboratuvarınızdaki VM'lerin kullanımını görüntüleme 

1. Soldaki menüden **Sanal makineler**'i seçin. 
2. VM'lerin durumunu ve VM'lerin çalışma süresini gördüğünüzden emin olun. Öğrenci VM'sinde sizin harcadığınız süre, son sütunda gösterilen kullanım süresine dahil edilmez. 

    ![VM kullanımı](../media/tutorial-track-usage/vm-usage.png)

## <a name="manage-student-vms"></a>Öğrenci VM'lerini yönetme 
Farenizi sanal makine listesindeki bir sıranın üzerine getirdiğinizde aşağıdaki görevlerle ilgili denetimleri görürsünüz: 

- Bir VM’ye bağlanma
- VM başlatma
- VM durdurma
- VM silme

![VM denetimleri](../media/tutorial-track-usage/vm-controls.png) 



## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide laboratuvara kaydolan kullanıcıları bulmayı, laboratuvardaki VM'lerin kullanımını izlemeyi ve laboratuvardaki VM'leri yönetmeyi öğrendiniz.

Sınıf laboratuvarları hakkında daha fazla bilgi edinmek için bkz. [Nasıl yapılır kılavuzları](how-to-manage-lab-accounts.md).
