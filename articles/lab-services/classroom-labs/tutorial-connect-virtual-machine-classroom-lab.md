---
title: Azure Lab Services’teki bir sınıf laboratuvarına erişme | Microsoft Docs
description: Bu öğreticide, bir profesör tarafından ayarlanan sınıf laboratuvarındaki sanal makinelere erişirsiniz.
services: devtest-lab, lab-services, virtual-machines
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
ms.date: 07/30/2018
ms.author: spelluru
ms.openlocfilehash: dadc90e6a39b9e9689bab0249e6496fdea6f6205
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/02/2018
ms.locfileid: "39450224"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Öğretici: Azure Lab Services’teki bir sınıf laboratuvarına erişme
Bu öğreticide, öğrenci olarak bir sınıf laboratuvarındaki sanal makineye (VM) bağlanırsınız. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Kayıt bağlantısını kullanma 
> * Sanal makineye bağlanma

## <a name="use-the-registration-link"></a>Kayıt bağlantısını kullanma

1. Profesörden/eğitimciden aldığınız **kayıt URL’sine** gidin. 
2. Kaydı tamamlamak için okul hesabınızı kullanarak hizmette oturum açın. 
3. Kaydolduktan sonra, erişimine sahip olduğunuz laboratuvarlar için sanal makineleri gördüğünüzü onaylayın. 
2. Sanal makine hazır hale gelene kadar bekleyin ve ardından **başlatın**.

    ![VM’yi başlatma](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

1. Erişmek istediğiniz laboratuvarın sanal makinesini temsil eden kutucukta **Bağlan**’ı seçin. 

    ![VM’ye bağlanma](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. RDP dosyasını sabit diske kaydedip açın. 
3. Makinede oturum açmak için eğitimcinizden/profesörünüzden aldığınız **kullanıcı adı** ve **parola** bilgisini kullanın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, eğitimcinizden/profesörünüzden aldığınız kayıt bağlantısını kullanarak bir sınıf laboratuvarına eriştiniz.

Laboratuvar sahibi olarak laboratuvarınıza kaydolan kullanıcıları görüntülemek ve VM'lerin kullanımını izlemek isteyebilirsiniz. Bunun nasıl yapılacağını öğrenmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Laboratuvar kullanımını izleme](tutorial-track-usage.md) 