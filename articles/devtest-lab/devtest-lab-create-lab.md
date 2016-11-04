---
title: Azure DevTest Labs'de laboratuvar oluşturma | Microsoft Docs
description: Sanal makineler için Azure DevTest Labs'de bir laboratuvar oluşturma
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: ''

ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2016
ms.author: tarcher

---
# Azure DevTest Labs'de laboratuvar oluşturma
## Ön koşullar
Laboratuvar oluşturmak için aşağıdakilere ihtiyacınız vardır:

* Azure aboneliği. Azure satın alma seçenekleri hakkında bilgi edinmek için bkz. [Azure'ı satın alma](https://azure.microsoft.com/pricing/purchase-options/) veya [Ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/). Laboratuvarı oluşturmak için aboneliğin sahibi olmanız gerekir.

## Azure DevTest Labs'de laboratuvar oluşturma adımları
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

## Sonraki adımlar
Laboratuvarınızı oluşturduktan sonra dikkate alınması gereken sonraki adımlardan birkaçı şu şekildedir:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md).
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md).
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md).
* [VM'leriniz için özel yapılar oluşturma](devtest-lab-artifact-author.md).
* [Yapı içeren bir VM'yi laboratuvara ekleme](devtest-lab-add-vm-with-artifacts.md).

<!--HONumber=Sep16_HO3-->


