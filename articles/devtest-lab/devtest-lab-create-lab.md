---
title: "Azure DevTest Labs'de laboratuvar oluşturma | Microsoft Belgeleri"
description: "Sanal makineler için Azure DevTest Labs'de bir laboratuvar oluşturma"
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
ms.date: 05/30/2017
ms.author: tarcher
ms.translationtype: HT
ms.sourcegitcommit: fff84ee45818e4699df380e1536f71b2a4003c71
ms.openlocfilehash: 265a968f902f53c7561c8c7e937f8eacfdb37167
ms.contentlocale: tr-tr
ms.lasthandoff: 08/01/2017

---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma
Azure DevTest Labs'deki bir laboratuvar, Sanal Makineler (VM'ler) gibi kaynaklardan oluşan grupları kapsayan ve limitlerin yanı sıra kotalar belirleyerek söz konusu kaynakları daha iyi yönetmenizi sağlayan bir altyapıdır. Bu makalede, Azure portalını kullanarak laboratuvar oluşturma işlemi ayrıntılı olarak açıklanmıştır.

## <a name="prerequisites"></a>Ön koşullar
Laboratuvar oluşturmak için aşağıdakilere ihtiyacınız vardır:

* Azure aboneliği. Azure satın alma seçenekleri hakkında bilgi edinmek için bkz. [Azure'ı satın alma](https://azure.microsoft.com/pricing/purchase-options/) veya [Ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/). Laboratuvarı oluşturmak için aboneliğin sahibi olmanız gerekir.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma adımları
Aşağıdaki adımlar, Azure portal kullanarak Azure DevTest Labs’de nasıl bir laboratuvar oluşturulacağını göstermektedir. 

1. [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Sol taraftaki ana menüden **Diğer Hizmetler**'i (listenin en alt kısmında) seçin.

    ![Diğer hizmetler menü seçeneği](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. Mevcut hizmetler listesinden **DevTest Labs**'i seçin.
1. **DevTest Labs** dikey penceresinde, **Ekle**'yi seçin.
   
    ![Laboratuvar ekleme](./media/devtest-lab-create-lab/add-lab-button.png)

1. **DevTest Lab oluştur** dikey penceresinde:
   
    1. Yeni laboratuvar için bir **Laboratuvar Adı** girin.
    2. Laboratuvarla ilişkilendirilecek **Abonelik** seçimini yapın.
    3. Laboratuvarın depolanacağı bir **Konum** seçin.
    4. **Otomatik kapatma**’yı seçerek laboratuvarın tüm VM’lerinin otomatik olarak kapatılmasını etkinleştirmek isteyip istemediğinizi belirleyin ve buna yönelik parametreleri tanımlayın. VM'nin otomatik olarak kapatılmasını istediğiniz zamanı belirtmenizi sağlayan otomatik kapanma özelliği temel olarak maliyet tasarrufu sağlayan bir özelliktir. [Azure DevTest Labs'deki bir laboratuvara yönelik tüm ilkeleri yönetme](./devtest-lab-set-lab-policy.md#set-auto-shutdown) makalesinde açıklanan adımları uygulayarak laboratuvarı oluşturduktan sonra otomatik kapanma ayarlarını değiştirebilirsiniz.
    5. Laboratuvar kısayolunun portal panosunda gösterilmesini istiyorsanız **Panoya Sabitle**’yi seçin.
    6. Yapılandırma otomasyonu yönelik Azure Resource Manager şablonlarını almak için **Otomasyon seçenekleri**’ni seçin. 
    7. **Oluştur**’u seçin. **Oluştur** seçeneği belirlendikten sonra **DevTest Labs** dikey penceresi görüntülenir. **Bildirim** alanını gözlemleyerek laboratuvar oluşturma işleminin durumunu izleyebilirsiniz. İşlem tamamlandığında, laboratuvar listesinde yeni oluşturulan laboratuvarı görmek için sayfayı yenileyin.  
    
    ![Laboratuvar dikey penceresi oluşturma](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarınızı oluşturduktan sonra dikkate alınması gereken sonraki adımlardan birkaçı şu şekildedir:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md).
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md).
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md).
* [VM'leriniz için özel yapılar oluşturma](devtest-lab-artifact-author.md).
* [Yapı içeren bir VM'yi laboratuvara ekleme](https://azure.microsoft.com/resources/videos/how-to-create-vms-with-artifacts-in-a-devtest-lab/).


