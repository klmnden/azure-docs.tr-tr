---
title: Azure Lab Services ile laboratuvar hesabı ayarlama | Microsoft Docs
description: Bu öğreticide, Azure Lab Services ile bir laboratuvar hesabı ayarlarsınız.
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
ms.date: 04/24/2019
ms.author: spelluru
ms.openlocfilehash: 704ad915616e4f860204783462269ec68a6e4e28
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64730631"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Öğretici: Azure Lab Services ile bir laboratuvar hesabı ayarlama
Azure Lab Services’te, bir laboratuvar hesabı, kuruluşunuzdaki laboratuvarların yönetildiği merkezi hesap olarak görev yapar. Laboratuvar hesabınızda, laboratuvar oluşturmak üzere başkalarına izin verin ve laboratuvar hesabı altındaki tüm laboratuvarlara uygulanan ilkeler ayarlayın. Bu öğreticide, laboratuvar yöneticisi olarak bir laboratuvar hesabı oluşturmayı öğrenin. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvar hesabı oluşturma
> * Laboratuvar Oluşturan rolüne kullanıcı ekleme
> * Laboratuvar sahiplerine sunulan Market görüntülerini belirtme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol menüsünde. Seçin **Lab Services** içinde **DEVOPS** bölümü. Yıldız seçerseniz (`*`) yanındaki **Lab Services**, eklenir **Sık Kullanılanlar** sol menüde bölümü. Bir sonraki zamandan ve sonraki sürümlerde, seçtiğiniz **Lab Services** altında **Sık Kullanılanlar**.

    ![Lab Services'i tüm hizmetleri ->](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Üzerinde **Lab Services** sayfasında **Ekle** araç. 

    ![Laboratuvar hesaplarını sayfada Ekle'yi seçin](../media/tutorial-setup-lab-account/add-lab-account-button.png)
4. Üzerinde **Laboratuvar hesabı** sayfasında, aşağıdaki eylemleri gerçekleştirin: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin. 
    5. Mevcut bir seçin **paylaşılan görüntü Galerisi** veya bir tane oluşturabilirsiniz. VM şablonu, başkaları tarafından kullanılabilmeleri de paylaşılan görüntü galerisinde kaydedebilirsiniz. Paylaşılan resim galerileri hakkında ayrıntılı bilgi için bkz. [Azure Lab Services paylaşılan görüntü galerisinde kullanabilecek](how-to-use-shared-image-gallery.md). 
    6. İçin **eş sanal ağ**, Laboratuvar ağı için bir eş sanal ağ (VNet) seçin. Bu hesap içinde oluşturduğunuz laboratuvarlar, seçilen sanal ağa bağlı ve seçili sanal ağ kaynaklara erişime sahip. 
    7. İçin **Laboratuvar konumunu seçmek için izin Laboratuvar oluşturan** alanında, Laboratuvar için bir konum seçmek için laboratuvar creators isteyip istemediğinizi belirtin. Varsayılan olarak, seçenek devre dışıdır. Bunu devre dışı bırakıldığında, Laboratuvar creators oluşturmakta olduğunuz Laboratuvar için bir konum belirtilemez. Labs Laboratuvar hesabına en yakın coğrafi konumda oluşturulur. Etkinleştirildiğinde, Laboratuvar oluşturan bir laboratuvar oluşturma sırasında bir konum seçebilirsiniz. 
    8. **Oluştur**’u seçin. 

        ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Seçin **zil simgesine** araç çubuğunda (**bildirimleri**), dağıtım başarılı oldu ve ardından onaylamak **kaynağa Git**. 

    Alternatif olarak, seçin **Yenile** üzerinde **Laboratuvar hesaplarını** sayfasında ve oluşturduğunuz Laboratuvar hesabını seçin. 

    ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/go-to-lab-account.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için kullanıcının ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olması gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Sınıf laboratuvarı oluşturmak için aynı kullanıcıyı kullanmayı planlıyorsanız bu adımı atlayabilirsiniz. Sınıf laboratuvarı oluşturmak için başka bir kullanıcı hesabı kullanmak istiyorsanız aşağıdaki adımları uygulayın: 

Eğitimcilere, sınıfları için laboratuvar oluşturma ve **Laboratuvar Oluşturan** rolüne bunları ekleme izni sağlamak için:

1. Üzerinde **Laboratuvar hesabı** sayfasında **erişim denetimi (IAM)** seçin **+ Ekle** seçin ve araç **+ rol ataması Ekle** üzerinde araç çubuğu. 

    ![Erişim Denetimi'ne rol ataması Ekle düğmesi](../media/tutorial-setup-lab-account/add-role-assignment-button.png)
1. Üzerinde **rol ataması Ekle** sayfasında **Laboratuvar oluşturan** için **rol**, Laboratuvar Creators role ekleyin ve istediğiniz kullanıcıyı seçin **Kaydet**. 

    ![Laboratuvar oluşturan Ekle](../media/tutorial-setup-lab-account/add-lab-creator.png)

## <a name="specify-marketplace-images-available-to-lab-creators"></a>Market görüntüleri için laboratuvar creators kullanılabilir belirtin
Laboratuvar sahibi olarak laboratuvar oluşturucuların laboratuvar hesabında laboratuvar oluşturmak için kullanacağı Market görüntülerini belirtebilirsiniz. 

1. Sol taraftaki menüden **Market görüntüleri**'ni seçin. Listede varsayılan olarak tüm görüntüler (hem etkin hem devre dışı) yer alır. Yalnızca etkin/devre dışı görüntüleri görmek için en üstteki açılan listeden **Yalnızca etkin**/**Yalnızca devre dışı** seçeneğini belirleyerek listeyi filtreleyebilirsiniz. 
    
    ![Market görüntüleri sayfası](../media/tutorial-setup-lab-account/marketplace-images-page.png)

    Listede görüntülenen Market görüntüleri yalnızca aşağıdaki koşulları karşılayan görüntülerdir:
        
    - Tek bir VM oluşturur.
    - VM'leri sağlamak için Azure Resource Manager'ı kullanır
    - Ek lisans planı satın alınmasını gerektirmez
2. Etkin durumdaki bir Market görüntüsünü **devre dışı bırakmak** için aşağıdaki eylemlerden birini gerçekleştirin: 
    1. Son sütundaki **... (üç nokta)** simgesini ve ardından **Görüntüyü devre dışı bırak**'ı seçin. 

        ![Tek bir görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-one-image.png) 
    2. Listede görüntü adlarının yanında yer alan onay kutularını seçerek bir daha fazla görüntüyü seçin ve **Seçilen görüntüleri devre dışı bırak**'a tıklayın. 

        ![Birden fazla görüntüyü devre dışı bırakma](../media/tutorial-setup-lab-account/disable-multiple-images.png) 
1. Benzer şekilde bir Market görüntüsünü **etkinleştirmek** için aşağıdaki eylemlerden birini gerçekleştirebilirsiniz: 
    1. Son sütundaki **... (üç nokta)** simgesini ve ardından **Görüntüyü etkinleştir**'i seçin. 
    2. Listede görüntü adlarının yanında yer alan onay kutularını seçerek bir daha fazla görüntüyü seçin ve **Seçilen görüntüleri etkinleştir**'e tıklayın. 

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir laboratuvar hesabı oluşturdunuz. Meslek olarak sınıf laboratuvarı oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)

