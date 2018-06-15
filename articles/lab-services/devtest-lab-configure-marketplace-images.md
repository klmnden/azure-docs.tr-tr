---
title: Azure DevTest Labs'de Azure Market görüntü ayarlarını yapılandırma | Microsoft Docs
description: Hangi Azure Marketi görüntüleri bir VM Azure DevTest Labs'de oluşturulurken kullanılan yapılandırma
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 804c6af2-17e9-4320-af3a-f454bd398379
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/05/2018
ms.author: spelluru
ms.openlocfilehash: 90df853eeca89e61fa69e6967218c4c18274ac1b
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
ms.locfileid: "33787398"
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Azure DevTest Labs'de Azure Market görüntü ayarlarını yapılandırma
DevTest Labs laboratuvarınızda kullanılacak oluşturma VM'ler Azure Market görüntülerini nasıl yapılandırdığınıza bağlı olarak Azure Market görüntüsünü temel destekler. Bu makalede, varsa, Azure Market görüntülerini olabilecek belirtmek gösterilmiştir VM'ler bir laboratuar ortamında oluşturulurken kullanılır. Bu, ekibinizin yalnızca ihtiyaç duydukları Market görüntülerini erişimi olmasını sağlar. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Bir VM oluşturulurken hangi Azure Market görüntülerini verileceğini seçin
1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin. 
4. Laboratuvar 's dikey penceresinde, seçin **yapılandırma ve ilkeleri**.
5. Laboratuvar'ın üzerinde **yapılandırma ve ilkeleri** altında dikey **sanal makine tabanları**seçin **Market görüntülerini**.
6. Yeni bir VM temeli olarak kullanmak için kullanılabilir olması için tüm tam Azure Market görüntülerini isteyip istemediğinizi belirtin. Seçerseniz **Evet**, laboratuar ortamında aşağıdaki ölçütleri izin verilen karşılayan tüm Azure Marketi görüntüleri sonra:
   
   * Tek bir VM görüntüsü oluşturur **ve**
   * Görüntü sağlama VM'ler, Azure Resource Manager kullanan **ve**
   * Görüntü ek lisans planı satın almayı gerektirmez
     
    Hiçbir görüntü izin verilmesini istediğiniz veya belirtmek istiyorsanız hangi görüntüleri seçin, kullanılabilir **Hayır**.
     
     ![VM'ler için temel görüntü olarak kullanılacak tüm Market görüntülerini izin vermek için seçeneği](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Seçerseniz **Hayır** önceki adıma **izin verilen görüntüleri/seçmek tüm** onay kutusunu etkindir. 
   Liste görünümünde görüntülenen tüm öğelerin seçimini kaldırın veya hızlı bir şekilde seçmek için arama kutusu ile birlikte bu seçeneği kullanın.
   * VM oluşturmak için ayrı ayrı her görüntünün karşılık gelen onay kutusunu işaretleyerek izin vermek istediğiniz Azure Market görüntülerini seçin.
   * Laboratuar ortamında kullanılan tüm Azure Marketi görüntüleri izin vermek istemiyorsanız, hiçbir şey listeden seçin.
   
    ![Hangi Azure Market görüntülerini VM'ler için temel görüntü olarak kullanılabilir belirtebilirsiniz](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Bir VM oluşturulurken Azure Market görüntülerini nasıl verildiğini yapılandırdıktan sonra sıradaki adım olacak [Laboratuvarınızı için bir VM eklemek](devtest-lab-add-vm.md).

