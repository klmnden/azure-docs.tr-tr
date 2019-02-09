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
ms.date: 02/07/2019
ms.author: spelluru
ms.openlocfilehash: 387e59eccc7dd9b20142bd692a1fe361435d3d57
ms.sourcegitcommit: d1c5b4d9a5ccfa2c9a9f4ae5f078ef8c1c04a3b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55965027"
---
# <a name="how-to-access-a-classroom-lab-in-azure-lab-services"></a>Azure Lab Services’teki bir sınıf laboratuvarına erişme
Bu makalede, bir sınıf laboratuvarına nasıl erişileceği, laboratuvardaki sanal makineye nasıl bağlanılacağı ve sanal makinenin nasıl durdurulacağı açıklanmaktadır. 

## <a name="register-to-a-lab"></a>Bir laboratuvara kaydetme
1. Profesörden/eğitimciden aldığınız **kayıt URL’sine** gidin. 
2. Kaydı tamamlamak için okul hesabınızı kullanarak hizmette oturum açın. 
3. Kaydolduktan sonra, erişimine sahip olduğunuz laboratuvarlar için sanal makineleri gördüğünüzü onaylayın. 
2. Sanal makine hazır hale gelene kadar bekleyin ve ardından **başlatın**. Bu işlem biraz zaman alabilir.  

    ![VM’yi başlatma](../media/tutorial-connect-vm-in-classroom-lab/start-vm.png)


## <a name="view-all-the-classroom-labs"></a>Tüm sınıf laboratuvarlarını görüntüleme
Labs kullanarak kaydettikten sonra aşağıdaki adımları izleyerek tüm sınıf laboratuvarlarını görüntüleyebilirsiniz: 

1. Gidin [ https://labs.azure.com ](https://labs.azure.com). 
2. Hizmete laboratuvara kaydolmak için kullandığınız kullanıcı hesabını kullanarak oturum açın. 
3. Erişiminiz olan tüm labs gördüğünüzü onaylayın. 

    ![Tüm laboratuvarları görüntüleme](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Bir sınıf laboratuvarındaki sanal makineye bağlanma

1. Henüz başlatılmışsa, sanal Makineyi seçin başlangıç **Başlat** kutucuğundaki. 
2. Erişmek istediğiniz laboratuvarın sanal makinesini temsil eden kutucukta **Bağlan**’ı seçin. 
3. Sabit diske (Windows VM için) RDP dosyasını kaydedin ve dosyayı açın. 
4. Makinede oturum açmak için eğitimcinizden/profesörünüzden aldığınız **kullanıcı adı** ve **parola** bilgisini kullanın. 

## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>Bir sınıf laboratuvarındaki sanal makineyi durdurma

Sanal makinenizin durdurmayı seçin **Durdur** kutucuğundaki. Sanal makine durdurulduğunda, kutucuktaki **Başlat** düğmesi etkinleştirilir. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir yönetici olarak oluşturun ve Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
 