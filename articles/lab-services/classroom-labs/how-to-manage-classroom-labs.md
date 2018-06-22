---
title: Sınıf labs Azure Laboratuvar Hizmetleri'nde yönetme | Microsoft Docs
description: Sınıf Laboratuvar oluşturulacağı ve yapılandırılacağı hakkında bilgi edinin, tüm sınıf labs, kayıt Laboratuvar kullanıcıyla bağlantı ya da bir laboratuvar silme palaşma görüntüleyin.
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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 311e58f01fac6d7786992b3c11e4b1b7c02ca838
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36304524"
---
# <a name="manage-classroom-labs-in-azure-lab-services"></a>Sınıf labs Azure Laboratuvar Hizmetleri'nde yönetme 
Bu makalede, oluşturma ve sınıf laboratuvarı yapılandırmak, tüm sınıf labs görüntülemek veya bir sınıf Laboratuvar silme açıklar.

## <a name="prerequisites"></a>Önkoşullar
Bir laboratuvar hesabı sınıf laboratuvarda ayarlamak için bir üyesi olmanız **Laboratuvar Creator** Laboratuvar hesabı rolünde. Bir laboratuvar sahibi bir kullanıcı Laboratuvar Creator rolüne aşağıdaki makaledeki adımları kullanarak ekleyebilirsiniz: [Laboratuvar Creator rolüne bir kullanıcı ekleme](tutorial-setup-lab-account.md#add-a-user-to-the-lab-creator-role).

## <a name="create-a-classroom-lab"></a>Sınıf laboratuvarı oluşturma

1. [Azure Lab Services web sitesine](https://labs.azure.com) gidin.
2. **Yeni Laboratuvar** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. Sınıf laboratuvarı için bir **ad** belirtin. 
    2. Sınıfta kullanmayı planladığınız sanal makinenin **boyutunu** seçin.
    3. Sanal makineyi oluşturmak için kullanılacak **görüntüyü** seçin.
    4. Belirtin **varsayılan kimlik bilgileri** oturum açmayı laboratuvara sanal makineler için kullanılacak.
    7. **Kaydet**’i seçin.

        ![Sınıf laboratuvarı oluşturma](../media/how-to-manage-classroom-labs/new-lab-window.png)
1. Gördüğünüz **Pano** Laboratuvar için. 
    
    ![Sınıf Laboratuvar Panosu](../media/how-to-manage-classroom-labs/classroom-lab-home-page.png)

## <a name="configure-usage-policy"></a>Kullanım ilkesini yapılandırma

1. **Kullanım ilkesi**’ni seçin. 
2. **Kullanım ilkesi**, ayarlar bölümüne, laboratuvarı kullanmasına izin verilen **kullanıcı sayısını** girin.
3. **Kaydet**’i seçin. 

    ![Kullanım ilkesi](../media/how-to-manage-classroom-labs/usage-policy-settings.png)

## <a name="set-up-the-template"></a>Şablonu ayarlama
Laboratuvardaki şablon, tüm kullanıcıların sanal makinelerinin oluşturulduğu bir temel sanal makine görüntüsüdür. Şablon sanal makinesini, tam olarak laboratuvar kullanıcılarına sağlamak istediklerinizle yapılandırılacak şekilde ayarlayın. Laboratuvar kullanıcılarının görebileceği bir ad ve açıklama belirtebilirsiniz. VM şablonu örneklerini Laboratuvar kullanıcılarınız için kullanılabilir hale getirmek şablonu yayımlayın.  

### <a name="set-template-title-and-description"></a>Set şablon başlığı ve açıklama
1. **Şablon** bölümünde, şablon için **Düzenle**’yi (kalem simgesi) seçin. 
2. **Kullanıcı görünümü** penceresinde, şablon için bir **başlık** girin.
3. Şablon için **açıklama** girin.
4. **Kaydet**’i seçin.

    ![Sınıf laboratuvarı açıklaması](../media/how-to-manage-classroom-labs/lab-description.png)

### <a name="set-up-the-template-vm"></a>VM şablonu oluşturun
 VM şablonu için bağlanmak ve, Öğrenciler kullanıma önce tüm gerekli yazılımı yükleyin. 

1. Şablon sanal makine hazır olana kadar bekleyin. Hazır olduğunda **Başlat** düğmesi etkinleştirilmesi gerekir. VM başlatmak için **Başlat**.

    ![VM şablonu Başlat](../media/tutorial-setup-classroom-lab/start-template-vm.png)
1. VM'ye bağlanmak için **Bağlan**ve yönergeleri izleyin. 

    ![VM şablonu için Bağlan](../media/tutorial-setup-classroom-lab/connect-template-vm.png)
1. Öğrenciler için laboratuvar (örneğin, Visual Studio, Azure Storage Gezgini, vb.) yapmak gerekli olan yazılımı yükleyin. 
2. Bağlantısını kes (Uzak Masaüstü oturumunuzu kapatın) VM şablonundan. 
3. **Durdur** seçerek VM şablonu **durdurmak**. 

    ![VM şablonu Durdur](../media/tutorial-setup-classroom-lab/stop-template-vm.png)


### <a name="publish-the-template"></a>Yayın şablonu 
Bir şablon yayımladığınızda, Azure Laboratuvar Hizmetleri şablonunu kullanarak laboratuar ortamında VM'ler oluşturur. Bu işlemde oluşturulan sanal makine sayısı, laboratuvarda izin verilen maksimum kullanıcı sayısıyla aynıdır. Laboratuvarın kullanım ilkesinde bu maksimum değeri ayarlayabilirsiniz. Tüm sanal makineler, şablonla aynı yapılandırmaya sahiptir. 

1. Seçin **Yayımla** içinde **şablonu** bölümü. 

    ![VM şablonunu yayımlama](../media/tutorial-setup-classroom-lab/public-access.png)
1. İçinde **Yayımla** penceresinde, seçin **yayımlanan** seçeneği. 
2. Şimdi, seçtiğiniz **Yayımla** düğmesi. Bu işlem bazı alabilir kaç Vm'lerinde bağlı olarak zaman oluşturuluyor, laboratuara izin verilen kullanıcı sayısı ile aynı olduğu.
    
    > [!IMPORTANT]
    > Bir şablon genel olarak kullanılabilir olduğunda, şablonun erişimi özel erişim olarak değiştirilemez. 
4. Geçiş **sanal makineleri** sayfasında ve bulunan beş sanal makineleri gördüğünüzü onaylayın **atanmamış** durumu. Bu sanal makineleri için Öğrenciler henüz atanmadı. 

    ![Sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines.png)
5. Sanal makineleri oluşturulana kadar bekleyin. Bunlar bulunmalıdır **durduruldu** durumu. Öğrenci VM başlatmak, VM'ye bağlanın, VM durdurun ve bu sayfadaki VM silin. Bu sayfada Başlat ya da sanal makineleri Başlat, Öğrenciler olanak tanır. 

    ![Durdurulmuş durumdaki sanal makineler](../media/tutorial-setup-classroom-lab/virtual-machines-stopped.png)


## <a name="send-registration-link-to-students"></a>Öğrencilere kayıt bağlantısı gönderme

1. Geçiş **Pano** görünümü. 
2. **Kullanıcı kaydı** kutucuğunu seçin.

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/dashboard-user-registration-link.png)
1. **Kullanıcı kaydı** iletişim kutusunda **Kopyala** düğmesini seçin. Bağlantı, panoya kopyalanır. Bağlantıyı bir e-posta düzenleyiciye yapıştırın ve öğrenciye e-posta gönderin. 

    ![Öğrenci kaydı bağlantısı](../media/tutorial-setup-classroom-lab/registration-link.png)
2. Üzerinde **kullanıcı kaydı** iletişim kutusunda **Kapat**. 

## <a name="view-all-classroom-labs"></a>Tüm sınıf labs görüntüleyin
1. Gidin [Azure Laboratuvar Hizmetleri portalı](https://labs.azure.com).
2. Seçili Laboratuvar hesaptaki tüm labs gördüğünüzü onaylayın. 

    ![Tüm Laboratuvarları](../media/how-to-manage-classroom-labs/all-labs.png)
3. Açılan listenin en üstünde farklı Laboratuvar hesabı seçmek için kullanın. Seçili Laboratuvar hesabı Labs'de bakın. 

## <a name="delete-a-classroom-lab"></a>Bir sınıf Laboratuvar Sil
1. Laboratuvar için kutucuğa köşedeki üç noktaya (...) seçin. 

    ![Laboratuvarı seçme](../media/how-to-manage-classroom-labs/select-three-dots.png)
2. **Sil**’i seçin. 

    ![Düğme silme](../media/how-to-manage-classroom-labs/delete-button.png)
3. Üzerinde **Delete Laboratuvar** iletişim kutusunda **silmek**. 

    ![Sil iletişim kutusu](../media/how-to-manage-classroom-labs/delete-lab-dialog-box.png)
 
## <a name="manage-student-vms"></a>Öğrenci sanal makineleri yönetme
Öğrenciler kasa kaydı'nı kullanarak Azure Laboratuvar Services ile bağladıktan sonra bunları için üzerinde Öğrenciler için atanan sanal makineleri görmek sağlanan **sanal makineleri** sekmesi. 

![Öğrenciler için atanan sanal makineler](../media/how-to-manage-classroom-labs/virtual-machines-students.png)

Bunu Öğrenci VM üzerinde aşağıdaki görevleri gerçekleştirebilirsiniz: 

- VM VM çalışıyorsa durdurun. 
- VM VM durdurulmuşsa başlatın. 
- VM’ye bağlanın. 
- VM silin. 

## <a name="next-steps"></a>Sonraki adımlar
Azure Lab Services kullanarak bir laboratuvarı ayarlamaya başlama:

- [Bir sınıf laboratuvarı ayarlama](how-to-manage-classroom-labs.md)
- [Laboratuvar ayarlama](../tutorial-create-custom-lab.md)
