---
title: Azure DevTest Labs'de Azure Market görüntü ayarlarını yapılandırma | Microsoft Docs
description: Azure DevTest Labs'de bir VM oluşturulurken kullanılabilir hangi Azure Market görüntülerini yapılandırma
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
ms.openlocfilehash: d0375713c4881c0b73b91fc07bda3ceac2dbc620
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60201899"
---
# <a name="configure-azure-marketplace-image-settings-in-azure-devtest-labs"></a>Azure DevTest Labs'de Azure Market görüntü ayarlarını yapılandırma
DevTest Labs Laboratuvarınızı kullanılacak oluşturma Vm'leri Azure Market görüntüleri nasıl yapılandırdığınıza bağlı olarak, Azure Market görüntüleri göre destekler. Bu makalede, varsa, Azure Market görüntüleri olabilecek belirtmek gösterilmektedir sanal makineleri bir laboratuar ortamında oluşturulurken kullanılan. Bu, ekibiniz yalnızca ihtiyaç duydukları Market görüntüleri erişimi olduğunu sağlar. 

## <a name="select-which-azure-marketplace-images-are-allowed-when-creating-a-vm"></a>Hangi Azure Market görüntüleri bir VM oluşturulurken izin seçin
1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. Seçin **tüm hizmetleri**ve ardından **DevTest Labs** listeden.
3. İstenen Laboratuvar labs listesinden seçin. 
4. Laboratuvar dikey penceresinde seçin **yapılandırması ve ilkelerini**.
5. Laboratuvar'ın **yapılandırması ve ilkelerini** altındaki dikey penceresinde **sanal makine tabanları**seçin **Market görüntüleri**.
6. Yeni bir sanal makinenin temel olarak kullanmak için kullanılabilir olması için tüm tam Azure Market görüntüleri isteyip istemediğinizi belirtin. Seçerseniz **Evet**, aşağıdaki tüm ölçütleri laboratuvarda izin karşılayan tüm Azure Market görüntüleri sonra:
   
   * Tek bir VM görüntüsü oluşturur **ve**
   * Görüntünün sağlama için sanal makineleri, Azure Resource Manager kullanır **ve**
   * Görüntünün bir fazladan lisans planı satın alınıyor gerektirmez
     
     Hiçbir görüntü izin verilmesi için istediğiniz veya belirtmek istiyorsanız hangi görüntüleri seçin, kullanılabilir **Hayır**.
     
     ![VM'ler için temel görüntü olarak kullanılacak tüm Market görüntüleri izin vermek için seçeneği](./media/devtest-lab-configure-marketplace-images/allow-all-marketplace-images.png)
7. Seçerseniz **Hayır** önceki adıma **izin verilen görüntüleri/seçin tüm** onay kutusunu etkin. 
   Hızlı bir şekilde seçin veya liste görünümünde görüntülenen tüm öğelerin seçimini kaldır için arama kutusuna birlikte bu seçeneği kullanın.
   * VM oluşturmak için tek tek her görüntünün karşılık gelen onay kutusunu işaretleyerek izin vermek istediğiniz Azure Market görüntülerini seçin.
   * Tüm Azure Market görüntüleri laboratuvarda kullanılacak izin vermek istemiyorsanız, hiçbir şey listeden seçin.
   
     ![Hangi Azure Market görüntüleri VM'ler için temel görüntü olarak kullanılabilir belirtebilirsiniz.](./media/devtest-lab-configure-marketplace-images/select-marketplace-images.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Azure Market görüntüleri bir VM oluşturulurken nasıl verildiğini yapılandırdıktan sonra sonraki adım olarak [bir VM, laboratuvara ekleme](devtest-lab-add-vm.md).

