---
title: "Azure DevTest Labs&quot;de laboratuvar oluşturma | Microsoft Belgeleri"
description: "Sanal makineler için Azure DevTest Labs&quot;de bir laboratuvar oluşturma"
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: f2e607924f17b42bba73672a2d24257e672e1134


---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma
## <a name="prerequisites"></a>Ön koşullar
Laboratuvar oluşturmak için aşağıdakilere ihtiyacınız vardır:

* Azure aboneliği. Azure satın alma seçenekleri hakkında bilgi edinmek için bkz. [Azure'ı satın alma](https://azure.microsoft.com/pricing/purchase-options/) veya [Ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/). Laboratuvarı oluşturmak için aboneliğin sahibi olmanız gerekir.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma adımları
Aşağıdaki adımlar, Azure portal kullanarak Azure DevTest Labs’de nasıl bir laboratuvar oluşturulacağını göstermektedir. 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
2. **More services**’i (Daha fazla hizmet’i) seçip ardından listeden **DevTest Labs**’i seçin.
3. **DevTest Labs** dikey penceresinde, **Ekle**'yi seçin.
   
    ![Laboratuvar ekleme](./media/devtest-lab-create-lab/add-lab-button.png)
4. **DevTest Lab oluştur** dikey penceresinde:
   
   1. Yeni laboratuvar için bir **Laboratuvar Adı** girin.
   2. Laboratuvarla ilişkilendirilecek **Abonelik** seçimini yapın.
   3. Laboratuvarın depolanacağı bir **Konum** seçin.
   4. **Otomatik kapatma**’yı seçerek laboratuvarın tüm VM’lerinin otomatik olarak kapatılmasını etkinleştirmek isteyip istemediğinizi belirleyin ve buna yönelik parametreleri tanımlayın.
   5. **Depolama türü**’nü seçerek laboratuvarın VM'lerine yönelik depolama disk türünü belirtin. 
   6. **Oluştur**'u seçin.
      
      ![Laboratuvar dikey penceresi oluşturma](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarınızı oluşturduktan sonra dikkate alınması gereken sonraki adımlardan birkaçı şu şekildedir:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md).
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md).
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md).
* [VM'leriniz için özel yapılar oluşturma](devtest-lab-artifact-author.md).
* [Yapı içeren bir VM'yi laboratuvara ekleme](devtest-lab-add-vm-with-artifacts.md).




<!--HONumber=Nov16_HO2-->


