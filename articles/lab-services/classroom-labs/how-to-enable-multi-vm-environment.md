---
title: Azure Lab Services, çoklu VM ortamları etkinleştirme | Microsoft Docs
description: Azure Lab Services'ın bir sınıf laboratuvarına şablonu bir sanal makine içinde birden çok sanal makine ile bir ortam oluşturmayı öğrenin.
services: lab-services
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/18/2019
ms.author: spelluru
ms.openlocfilehash: 6faf32232c42f863bff52fdfb3c0714aee8e9b88
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60702439"
---
# <a name="create-an-environment-with-multiple-vms-inside-a-template-vm-of-a-classroom-lab"></a>Bir sınıf laboratuvarı VM şablon içinde birden çok VM ile bir ortam oluşturun
Azure Lab Services şu anda tek bir şablonda sanal makinede bir laboratuvarı ayarlama ve tek bir kopyasını her kullanıcı için kullanılabilir kılmanıza olanak sağlar. Ancak, güvenlik duvarları veya sunucularını ayarlamak nasıl bir BT sınıfında veren bir Profesör varsa, her biri, Öğrenciler, birden çok sanal makineleri birbirine bir ağ üzerinden konuşabilirsiniz bir ortam ile sağlamanız gerekebilir.

İç içe sanallaştırma, bir laboratuvar şablonu sanal makinenin içindeki bir çoklu VM ortam oluşturmanıza olanak sağlar. Şablon yayımlama Laboratuvardaki her kullanıcı birden çok VM içindeki ayarlanmış bir sanal makineyle sağlar.

## <a name="what-is-nested-virtualization"></a>İç içe sanallaştırma nedir?
İç içe sanallaştırma, bir sanal makine içinde sanal makineler oluşturmanıza olanak sağlar. İç içe sanallaştırma, Hyper-V gerçekleştirilir ve yalnızca Windows Vm'lerinde kullanılabilir.

İç içe sanallaştırma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure'da iç içe sanallaştırma](https://azure.microsoft.com/blog/nested-virtualization-in-azure/)
- [Azure VM'de iç içe sanallaştırmayı etkinleştirme](../../virtual-machines/windows/nested-virtualization.md)

## <a name="use-nested-virtualization-in-azure-lab-services"></a>Azure Lab Services içinde iç içe sanallaştırma kullanma
Önemli adımlar şunlardır:

1. Oluşturma bir **büyük** boyutlandırılmış **Windows** Laboratuvar için şablon makine. 
2. Bağlanmak ve [iç içe sanallaştırmayı etkinleştirmek](../../virtual-machines/windows/nested-virtualization.md).


Aşağıdaki yordamda ayrıntılı adımlar sağlar: 

1. Zaten yoksa, bir laboratuvar hesabı oluşturun. Yönergeler için [Öğreticisi: Azure Lab Services ile bir laboratuvar hesabı ayarlama](tutorial-setup-lab-account.md).
2. [Azure Lab Services web sitesine](https://labs.azure.com) gidin. 
3. **Oturum aç**’ı seçip kimlik bilgilerinizi girin. Azure Lab Services, kuruluş hesaplarını ve Microsoft hesaplarını destekler. 
4. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Laboratuvarınız için bir **ad** belirtin. 
    2. Maksimum belirtin **sanal makinelerin sayısı** Laboratuvardaki. Laboratuvarı oluşturduktan sonra veya varolan bir laboratuvar içindeki VM sayısını decreate veya artırabilirsiniz. Daha fazla bilgi için [bir laboratuvar içindeki sanal makine sayısını güncelleştir](how-to-configure-student-usage.md#update-number-of-virtual-machines-in-lab)
    6. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/tutorial-setup-classroom-lab/new-lab-window.png)
4. **Sanal makine özelliklerini seçin** sayfasında aşağıdaki adımları gerçekleştirin:
    1. Seçin **büyük** laboratuvarda oluşturulacak (VM'ler) sanal makinelerin boyutu. Şu anda yalnızca büyük boyutlu iç içe sanallaştırma destekler.
    2. Bir sanal makine görüntüsü seçin bir **Windows görüntü**. İç içe sanallaştırma yalnızca Windows makinelerde kullanılabilir. 
    3. **İleri**’yi seçin.

        ![VM özellikleri belirtme](../media/how-to-enable-multi-vm-environment/large-windows-vm.png)    
5. **Kimlik bilgilerini ayarla** sayfasında laboratuvardaki tüm VM'ler için varsayılan kimlik bilgilerini belirtin. 
    1. Laboratuvardaki tüm VM'ler için **kullanıcı adını** belirtin.
    2. Kullanıcının **parolasını** belirtin. 

        > [!IMPORTANT]
        > Kullanıcı adını ve parolayı not edin. Bunlar tekrar gösterilmeyecektir.
    3. **Oluştur**’u seçin. 

        ![Kimlik bilgilerini ayarlama](../media/tutorial-setup-classroom-lab/set-credentials.png)
6. **Şablonu yapılandır** sayfasında laboratuvar oluşturma işleminin durumunu görebilirsiniz. Laboratuvar şablonunun oluşturulması 20 dakika sürebilir. 

    ![Şablonu yapılandır](../media/tutorial-setup-classroom-lab/configure-template.png)
7. Şablon yapılandırma işlemleri tamamlandıktan sonra şu sayfayı görürsünüz: 

    ![Tamamlanmış şablon yapılandırma sayfası](../media/tutorial-setup-classroom-lab/configure-template-after-complete.png)
8. Üzerinde **yapılandırma şablonu** sayfasında **Connect** şablona iç içe sanallaştırmayı yapılandırma için VM bağlanmak için. Bu sihirbazdaki adımları tamamladıktan sonra ayrıca daha sonra yapılandırabilirsiniz. 
9. Şablon sanal makine içinde iç içe sanallaştırmayı ayarlayın ve birden çok sanal makine ile bir sanal ağ yapılandırın. Ayrıntılı adım adım yönergeler için bkz: [Azure VM'deki iç içe sanallaştırmayı etkinleştirmek nasıl](../../virtual-machines/windows/nested-virtualization.md). Adımları hızlı bir özeti aşağıda verilmiştir: 
    1. Şablon sanal makinede Hyper-V özelliğini etkinleştirin.
    2. İç içe sanal makineler için internet bağlantısına sahip bir iç sanal ağ Kur ayarlayın
    3. Hyper-V Yöneticisi aracılığıyla sanal makineler oluşturma
    4. Sanal makineler için bir IP adresi atama
10. Şablon sayfasında **İleri**'yi seçin. 
11. **Şablonu yayımla** sayfasında aşağıdaki işlemleri gerçekleştirin. 
    1. Şablon hemen Yayımla ve **Yayımla**.  

        > [!WARNING]
        > Yayımlama işlemini geri alamazsınız. 
    2. Daha sonra yayımlamak istiyorsanız **Sonrası için kaydet**'i seçin. Şablon VM'sini sihirbaz tamamlandıktan sonra yayımlayabilirsiniz. Sihirbaz tamamlandıktan sonra yapılandırma ve yayımlama adımları için [Sınıf laboratuvarlarını yönetme](how-to-manage-classroom-labs.md) makalesindeki [Şablonu yayımlama](how-to-create-manage-template.md#publish-the-template-vm) bölümüne bakın.

        ![Şablonu yayımlama](../media/how-to-enable-multi-vm-environment/publish-template-page.png)
11. Şablonun **yayımlama ilerleme durumunu** görürsünüz. Bu işlemin tamamlanması bir saat sürebilir. 

    ![Şablonu yayımlama - ilerleme](../media/tutorial-setup-classroom-lab/publish-template-progress.png)
12. Şablon başarıyla yayımlandığında aşağıdaki sayfayı görürsünüz. **Done** (Bitti) öğesini seçin.

    ![Şablonu yayımlama - başarılı](../media/tutorial-setup-classroom-lab/publish-success.png)
1. Laboratuvar **panosunu** görürsünüz. 
    
    ![Sınıf laboratuvarı panosu](../media/how-to-enable-multi-vm-environment/dashboard.png)


## <a name="next-steps"></a>Sonraki adımlar

Artık, her bir kullanıcı, çoklu VM ortamında içeren tek bir sanal makine alır. Kullanıcılar laboratuvara ekleme ve kayıt bağlantı şirketlerde öğrenmek için şu makaleye bakın: [Kullanıcılar laboratuvara ekleme](tutorial-setup-classroom-lab.md#add-users-to-the-lab).