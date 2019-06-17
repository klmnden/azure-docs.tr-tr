---
title: Azure DevTest labs'deki bir laboratuvara Laboratuvar veya VM sildiğiniz | Microsoft Docs
description: Bu makalede bir laboratuvar veya VM bir laboratuar ortamında nasıl silineceği gösterilmektedir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2018
ms.author: spelluru
ms.openlocfilehash: 9634c70566aba21bdd28ee016c9fa94464ec9c1b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62127421"
---
# <a name="delete-a-lab-or-vm-in-a-lab-in-azure-devtest-labs"></a>Azure DevTest labs'deki bir laboratuvara, Laboratuvar veya VM silme
Bu makalede bir laboratuvar veya VM bir laboratuar ortamında nasıl silineceği gösterilmektedir.

## <a name="delete-a-lab"></a>Bir laboratuvar Sil
DevTest Labs örnek bir kaynak grubundan silme işleminin DevTest Labs hizmeti aşağıdaki eylemleri gerçekleştirir: 

- Laboratuvar oluşturma sırasında otomatik olarak oluşturulan tüm kaynakları otomatik olarak silinir. Kaynak grubu silinmez. Bu kaynak grubu tüm kaynakları el ile oluşturduysanız, bunları hizmet silmez. 
- Bu VM'ler ile ilişkili Laboratuvar ve kaynak grupları içindeki tüm sanal makineleri otomatik olarak silinir. Bir laboratuar ortamında bir VM oluşturduğunuzda, hizmet kaynakları (disk, ağ arabirimi, genel IP adresi, vb.) ayrı bir kaynak grubu içinde VM oluşturur. Bu kaynak grubunda ek kaynakları el ile oluşturursanız, ancak, DevTest Labs hizmeti bu kaynakları ve kaynak grubunu silmez. 

Bir laboratuvar silmek için aşağıdaki eylemleri gerçekleştirin: 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm kaynak** sol taraftaki menüden **DevTest Labs** türünün hizmet ve Laboratuvar seçin.

    ![Laboratuvarınızı seçin](media/devtest-lab-delete-lab-vm/select-lab.png)
3. Üzerinde **DevTest Labs** sayfasında **Sil** araç. 

    ![Sil düğmesi](media/devtest-lab-delete-lab-vm/delete-button.png)
4. Üzerinde **onay** want **adı** Laboratuvar ve select **Sil**. 

    ![Onayla](media/devtest-lab-delete-lab-vm/confirm-delete.png)
5. İşlemin durumunu görmek için seçin **bildirimleri** simgesine (zil). 

    ![Bildirimler](media/devtest-lab-delete-lab-vm/delete-status.png)

 
## <a name="delete-a-vm-in-a-lab"></a>Laboratuvarda VM silme
Laboratuvarda VM silersem, Laboratuvar oluşturma sırasında oluşturulan bazı (Tümü değil) kaynakları silinir. Aşağıdaki kaynaklar silinmez: 

-   Anahtar kasası ana kaynak grubunda
-   Kullanılabilirlik kümesi, yük dengeleyici, VM kaynak grubu içinde genel IP adresi. Bu kaynaklar bir kaynak grubunda birden çok VM tarafından paylaşılır. 

Sanal makine ve ağ arabiriminin VM ile ilişkili disk silinir. 

Laboratuvarda bir VM'yi silmek için aşağıdaki eylemleri gerçekleştirin: 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm kaynak** sol taraftaki menüden **DevTest Labs** türünün hizmet ve Laboratuvar seçin.

    ![Laboratuvarınızı seçin](media/devtest-lab-delete-lab-vm/select-lab.png)
3. Seçin **... (üç nokta)**  VM'ler ve seçim listesi içinde VM için **Sil**. 

    ![VM menüsünde Sil](media/devtest-lab-delete-lab-vm/delete-vm-menu-in-list.png)
4. Üzerinde **onay** iletişim kutusunda **Tamam**. 
5. İşlemin durumunu görmek için seçin **bildirimleri** simgesine (zil). 

Bir sanal makineden silinemedi **sanal makine sayfasında**seçin **Sil** aşağıdaki görüntüde gösterildiği gibi araç çubuğundan:

![VM VM sayfasından silin.](media/devtest-lab-delete-lab-vm/delete-from-vm-page.png) 


## <a name="next-steps"></a>Sonraki adımlar
Bir laboratuvar oluşturmak istiyorsanız, aşağıdaki makalelere bakın: 

- [Laboratuvar oluşturma](devtest-lab-create-lab.md)
- [Bir VM'yi laboratuvara ekleme](devtest-lab-add-vm.md)