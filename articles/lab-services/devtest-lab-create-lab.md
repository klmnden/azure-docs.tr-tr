---
title: Azure DevTest Labs'de laboratuvar oluşturma | Microsoft Belgeleri
description: Sanal makineler için Azure DevTest Labs'de bir laboratuvar oluşturma
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: spelluru
ms.openlocfilehash: c54b97bdf69908f32015631a9e527c6e289d1d2a
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60202457"
---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma
Azure DevTest Labs'deki bir laboratuvar, Sanal Makineler (VM'ler) gibi kaynaklardan oluşan grupları kapsayan ve limitlerin yanı sıra kotalar belirleyerek söz konusu kaynakları daha iyi yönetmenizi sağlayan bir altyapıdır. Bu makalede, Azure portalını kullanarak laboratuvar oluşturma işlemi ayrıntılı olarak açıklanmıştır.

## <a name="prerequisites"></a>Önkoşullar
Laboratuvar oluşturmak için aşağıdakilere ihtiyacınız vardır:

* Azure aboneliği. Azure satın alma seçenekleri hakkında bilgi edinmek için bkz. [Azure'ı satın alma](https://azure.microsoft.com/pricing/purchase-options/) veya [Ücretsiz bir aylık deneme](https://azure.microsoft.com/pricing/free-trial/). Laboratuvarı oluşturmak için aboneliğin sahibi olmanız gerekir.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Azure DevTest Labs'de laboratuvar oluşturma adımları
Aşağıdaki adımlar, Azure portal kullanarak Azure DevTest Labs’de nasıl bir laboratuvar oluşturulacağını göstermektedir. 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. Sol taraftaki ana menüden **Tüm Hizmetler**'i (listenin en üst kısmında) seçin. Seçin * (yıldız yanındaki) **DevTest Labs** içinde **DEVOPS** bölümü. Bu eylem ekler **DevTest Labs** sol gezinti menüsüne böylece sonraki kolayca erişebilir. 

    ![Tüm hizmetleri - DevTest Labs seçin](./media/devtest-lab-create-lab/all-services-select.png)
2. Şimdi, seçtiğiniz **DevTest Labs** sol gezinti menüsünde. Seçin **Ekle** araç. 
   
    ![Laboratuvar ekleme](./media/devtest-lab-create-lab/add-lab-button.png)
1. Üzerinde **DevTest Lab Oluştur** sayfasında, aşağıdaki eylemleri gerçekleştirin: 
    1. Girin bir **adı** Laboratuvar için.
    2. Laboratuvarla ilişkilendirilecek **Abonelik** seçimini yapın.
    3. Girin bir **kaynak grubu adı** Laboratuvar için. 
    4. Seçin bir **konumu** Laboratuvarın depolanacağı.
    4. **Otomatik kapatma**’yı seçerek laboratuvarın tüm VM’lerinin otomatik olarak kapatılmasını etkinleştirmek isteyip istemediğinizi belirleyin ve buna yönelik parametreleri tanımlayın. VM'nin otomatik olarak kapatılmasını istediğiniz zamanı belirtmenizi sağlayan otomatik kapanma özelliği temel olarak maliyet tasarrufu sağlayan bir özelliktir. [Azure DevTest Labs'deki bir laboratuvara yönelik tüm ilkeleri yönetme](./devtest-lab-set-lab-policy.md#set-auto-shutdown) makalesinde açıklanan adımları uygulayarak laboratuvarı oluşturduktan sonra otomatik kapanma ayarlarını değiştirebilirsiniz.
    1. Laboratuvarda oluşturacağınız tüm kaynaklara eklenecek özel etiketler oluşturmak istiyorsanız **Etiketler** için **AD** ve **DEĞER** bilgilerini girin. Laboratuvarları kategorilere göre yönetmek ve düzenlemek istiyorsanız etiketler faydalı olacaktır. Laboratuvarı oluşturduktan sonra etiket ekleme dahil olmak üzere etiketler hakkında daha fazla bilgi için bkz. [Laboratuvara etiket ekleme](devtest-lab-add-tag.md).
    6. Yapılandırma otomasyonu yönelik Azure Resource Manager şablonlarını almak için **Otomasyon seçenekleri**’ni seçin. 
    7. **Oluştur**’u seçin. **Bildirim** alanını gözlemleyerek laboratuvar oluşturma işleminin durumunu izleyebilirsiniz. 
    
        ![DevTest Labs laboratuvar bölümü oluşturma](./media/devtest-lab-create-lab/create-devtestlab-blade.png)
    8. Tamamlandığında, seçin **kaynağa Git** bildirim. Alternatif olarak, yenileme **DevTest Labs** Laboratuvar listesinde yeni oluşturulan laboratuvarı görmek için sayfayı.  Laboratuvar listesinde seçin. Laboratuvarınız için giriş sayfasını görürsünüz. 

        ![Laboratuvar için giriş sayfası](./media/devtest-lab-create-lab/lab-home-page.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
Laboratuvarınızı oluşturduktan sonra dikkate alınması gereken sonraki adımlardan birkaçı şu şekildedir:

* [Laboratuvara erişimin güvenliğini sağlama](devtest-lab-add-devtest-user.md)
* [Laboratuvar ilkeleri belirleme](devtest-lab-set-lab-policy.md)
* [Laboratuvar şablonu oluşturma](devtest-lab-create-template.md)
* [VM'leriniz için özel yapıtlar oluşturma](devtest-lab-artifact-author.md)
* [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)

