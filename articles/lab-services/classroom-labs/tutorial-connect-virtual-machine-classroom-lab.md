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
ms.date: 01/17/2019
ms.author: spelluru
ms.openlocfilehash: 3be1da54b16a24ce3c4431dfe26eb778cea5c83d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60694683"
---
# <a name="tutorial-access-a-classroom-lab-in-azure-lab-services"></a>Öğretici: Azure Lab Services içinde bir sınıf laboratuvarına erişim
Bu öğreticide, öğrenci olarak bir sınıf laboratuvarındaki sanal makineye (VM) bağlanırsınız. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Kayıt bağlantısını kullanma 
> * Sanal makineye bağlanma

## <a name="use-the-registration-link"></a>Kayıt bağlantısını kullanma

1. Profesörden/eğitimciden aldığınız **kayıt URL’sine** gidin. Kayıt URL'si tamamladıktan sonra kullanmanız gerekmez. Bunun yerine, URL'yi kullanın: [ https://labs.azure.com ](https://labs.azure.com). Internet Explorer 11 henüz desteklenmediğini unutmayın. 
1. Kaydı tamamlamak için okul hesabınızı kullanarak hizmette oturum açın. 
2. Kaydolduktan sonra, erişimine sahip olduğunuz laboratuvarlar için sanal makineleri gördüğünüzü onaylayın. 
3. Sanal makine hazır hale gelene kadar bekleyin ve ardından **başlatın**. Bu işlem biraz zaman alabilir.  

    ![VM’yi başlatma](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)

## <a name="connect-to-the-virtual-machine"></a>Sanal makineye bağlanma

1. Seçin **Connect** kutucuğuna erişmek istediğiniz Laboratuvar sanal makinesi için. 

    ![VM’ye bağlanma](../media/tutorial-connect-vm-in-classroom-lab/connect-vm.png)
2. Aşağıdaki adımlardan birini uygulayın: 
    1. İçin **Windows** sanal makineleri Kaydet **RDP** sabit disk dosyası. Sanal makineye bağlanmak için RDP dosyasını açın. Kullanım **kullanıcı adı** ve **parola** makineye oturum açmak için Eğitimci/Profesör alın. 
    3. İçin **Linux** kullanabileceğiniz sanal makineleri **SSH** veya **RDP** (etkinse) bunlara bağlanmak için. Daha fazla bilgi için [Linux makineler için Uzak Masaüstü Bağlantısı etkinleştirme](how-to-enable-remote-desktop-linux.md). 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, eğitimcinizden/profesörünüzden aldığınız kayıt bağlantısını kullanarak bir sınıf laboratuvarına eriştiniz.

Laboratuvar sahibi olarak kim Laboratuvarınızı ile kayıtlı olan ve sanal makinelerinin kullanım izleme görüntülemek istiyorsunuz. Laboratuvarı kullanımı izleme hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Laboratuvar kullanımını izleme](tutorial-track-usage.md) 
