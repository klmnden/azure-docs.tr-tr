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
ms.openlocfilehash: bc5c12d4bb92edaafcc9808da8c48106a6e0cbd5
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2019
ms.locfileid: "59548052"
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

1. Gidin [ https://labs.azure.com ](https://labs.azure.com). Internet Explorer 11 henüz desteklenmediğini unutmayın. 
2. Hizmete laboratuvara kaydolmak için kullandığınız kullanıcı hesabını kullanarak oturum açın. 
3. Erişiminiz olan tüm labs gördüğünüzü onaylayın. 

    ![Tüm laboratuvarları görüntüleme](../media/how-to-use-classroom-lab/all-labs.png)

## <a name="connect-to-the-virtual-machine-in-a-classroom-lab"></a>Bir sınıf laboratuvarındaki sanal makineye bağlanma

1. Henüz başlatılmışsa, sanal Makineyi seçin başlangıç **Başlat** kutucuğundaki. 
2. Erişmek istediğiniz laboratuvarın sanal makinesini temsil eden kutucukta **Bağlan**’ı seçin. 
3. Aşağıdaki adımlardan birini uygulayın: 
   1. İçin **Windows** sanal makineleri Kaydet **RDP** sabit disk dosyası. Sanal makineye bağlanmak için RDP dosyasını açın. Kullanım **kullanıcı adı** ve **parola** makineye oturum açmak için Eğitimci/Profesör alın. 
   3. İçin **Linux** sanal makineler, kopyalama ve SSH bağlantı dizesini kaydedin **sanal makinenize bağlanın** iletişim kutusu. Bir SSH terminalden Bu bağlantı dizesini kullan (gibi [Putty](https://www.putty.org/)) sanal makineye bağlanmak için.

## <a name="stop-the-virtual-machine-in-a-classroom-lab"></a>Bir sınıf laboratuvarındaki sanal makineyi durdurma

Sanal makinenizin durdurmayı seçin **Durdur** kutucuğundaki. Sanal makine durdurulduğunda, kutucuktaki **Başlat** düğmesi etkinleştirilir. 

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelere bakın:

- [Bir yönetici olarak oluşturun ve Laboratuvar hesaplarını yönetme](how-to-manage-lab-accounts.md)
- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
 