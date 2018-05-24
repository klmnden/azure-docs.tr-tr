---
title: Azure DevTest Labs’de bir özel laboratuvara erişme | Microsoft Docs
description: Bu öğreticide, Azure DevTest Labs kullanılarak oluşturulan laboratuvara erişir, sanal makineler talep eder, sanal makineleri kullanır ve sanal makine talebini geri alırsınız.
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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: be4bde6bd320e8af7cd3119ff4ccdabd942963ca
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
ms.locfileid: "34361847"
---
# <a name="tutorial-access-a-custom-lab-in-azure-devtest-labs"></a>Öğretici: Azure DevTest Labs’de bir özel laboratuvara erişme
Bu öğreticide, [Öğretici: Özel laboratuvar oluşturma](tutorial-create-custom-lab.md) başlıklı öğreticide oluşturulan özel laboratuvarı kullanırsınız.

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Özel laboratuvarda sanal makine (VM) talep etme
> * VM’ye bağlanma
> * Sanal makine talebini geri alma

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="access-the-lab"></a>Laboratuvara erişme

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Soldaki menüden **Tüm kaynaklar**’ı seçin. 
3. Kaynak türü için **DevTest Labs**’i seçin. 
4. Laboratuvarı seçin. 

    ![Laboratuvarı seçme](./media/tutorial-use-custom-lab/search-for-select-custom-lab.png)

## <a name="claim-a-vm"></a>Sanal makine talep etme

1. **Talep edilebilir sanal makineler** listesinde **...** (üç nokta) ve sonra **Makineyi talep et**’i seçin.

    ![Sanal makineyi talep etme](./media/tutorial-use-custom-lab/claim-virtual-machine.png)
1. **Sanal makinelerim** listesinde sanal makineyi gördüğünüzü onaylayın.

    ![Sanal makinem](./media/tutorial-use-custom-lab/my-virtual-machines.png)

## <a name="connect-to-the-vm"></a>VM’ye bağlanma

1. Listeden sanal makinenizi seçin. Sanal makineniz için **Sanal Makine sayfasını** görürsünüz. Araç çubuğunda **Bağlan**’ı seçin.

    ![Sanal makineye bağlanma](./media/tutorial-use-custom-lab/connect-button.png)
2. İndirilen **RDP** dosyasını sabit diskinize kaydedin ve sanal makineye bağlanmak için bunu kullanın. Önceki bölümde sanal makine oluşturulurken belirttiğiniz kullanıcı adını ve parolayı belirtin. 

## <a name="unclaim-the-vm"></a>Sanal makine talebini geri alma
Sanal makineyi kullandıktan sonra şu adımları izleyerek sanal makine talebini geri alın: 

1. Sanal makine sayfasında, araç çubuğundan **Talebi Geri Al**’ı seçin. 

    ![Sanal makine talebini geri alma](./media/tutorial-use-custom-lab/unclaim-vm-menu.png)
1. Sanal makine talebi geri alınmadan önce sanal makine kapatılır. 

    ![Talebi geri alma durumu](./media/tutorial-use-custom-lab/unclaim-status.png) 
1. Talebi geri alma işlemi bittikten sonra, en alt kısımdaki **Talep edilebilir sanal makineler** listesinde sanal makineyi görürsünüz. 
    
## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, Azure DevTest Labs kullanılarak oluşturulan bir özel laboratuvara nasıl erişeceğiniz ve bu laboratuvarı nasıl kullanacağınız gösterildi. Özel laboratuvardaki sanal makinelere erişme ve bu sanal makineleri kullanma hakkında daha fazla bilgi için bkz. 

> [!div class="nextstepaction"]
> [Nasıl yapılır: Özel laboratuvardaki sanal makineleri kullanma](devtest-lab-add-vm.md)

