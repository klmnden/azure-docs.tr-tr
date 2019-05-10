---
title: Azure Lab Services Laboratuvar hesaplarını yapılandırma | Microsoft Docs
description: Bir laboratuvar hesabı oluşturulduktan sonra yapılandırmayı öğrenin.
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
ms.date: 05/07/2019
ms.author: spelluru
ms.openlocfilehash: ba469c038f04a31a57e798b97b5120bec573feae
ms.sourcegitcommit: 300cd05584101affac1060c2863200f1ebda76b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2019
ms.locfileid: "65414049"
---
# <a name="configure-lab-accounts-in-azure-lab-services"></a>Azure Lab Services Laboratuvar hesaplarını yapılandırma 
Azure Lab Services içinde bir laboratuvar hesabı sınıf laboratuvarlarını gibi yönetilen Laboratuvar türleri için bir kapsayıcıdır. Yönetici Azure Lab Services ile bir laboratuvar hesabı ayarlama ayarlar ve Laboratuvarları hesabı oluşturabilirsiniz Laboratuvar sahipleri erişim sağlar. Bu makalede bir laboratuvar hesabı oluşturun, tüm Laboratuvar hesaplarını görüntülemek veya bir laboratuvar hesabı silme işlemini açıklamaktadır.

## <a name="connect-with-a-peer-virtual-network"></a>Bir eş sanal ağ ile bağlanma
Bir eş ağ Laboratuvar sanal ağdan sanal ağa bir sanal ağa bağlanmak için şu adımları izleyin:

1. Üzerinde **Laboratuvar hesabı** sayfasında **Labs yapılandırma** sol menüsünde.

    ![Laboratuvar yapılandırma sayfası](../media/how-to-manage-lab-accounts/labs-configuration-page.png) 
1. İçin **eş sanal ağ**seçin **etkin** veya **devre dışı bırakılmış**. Varsayılan değer **devre dışı bırakılmış**. Eş sanal ağ'ı etkinleştirmek için aşağıdaki adımları uygulayın: 
    1. **Etkin**’i seçin.
    2. Seçin **VNet** aşağı açılan listeden. 
3. Araç çubuğunda **Kaydet**’i seçin. 

Bu hesap içinde oluşturduğunuz laboratuvarlar seçilen sanal ağa bağlanır. Seçilen sanal ağ içindeki kaynaklara erişebilir. Daha fazla bilgi için [eş sanal ağ içinde Azure Lab Services ile Laboratuvar ağı bağlama](how-to-connect-peer-virtual-network.md).

Bir sanal ağ için seçtiğinizde, **eş sanal ağ** alanı **Laboratuvar konumunu seçmek için izin Laboratuvar oluşturan** seçeneği devre dışıdır. Laboratuvar hesabı Labs'de eş sanal ağ içindeki kaynaklarla bağlanabilmeleri için bir laboratuvar hesabı ile aynı bölgede olmalıdır olmasıdır. 

## <a name="allow-lab-creator-to-pick-location-for-the-lab"></a>Laboratuvar oluşturan Laboratuvar için bir konum seçmek izin ver
Laboratuvar oluşturan aşağıdaki adımları izleyerek labs Laboratuvar hesabı konumunun farklı bir konumda oluşturmak izin ver: 

1. Üzerinde **Laboratuvar hesabı** sayfasında **Labs yapılandırma** sol menüsünde.
2. İçin **Laboratuvar konumunu seçmek için izin Laboratuvar oluşturan**seçin **etkin** Laboratuvar için bir konum seçmek için laboratuvar oluşturan istiyorsanız. Etkinleştirilirse, labs Laboratuvar hesabının bulunduğu aynı konumda otomatik olarak oluşturulur. 
    
    İçin sanal ağ seçtiğinizde bu alan devre dışıdır **eş sanal ağ** alan. Laboratuvar hesabı Labs'de Laboratuvar hesabı eş sanal ağ kaynaklarına erişmesini sağlamak için aynı bölgede olmalıdır olmasıdır. 
1. Araç çubuğunda **Kaydet**’i seçin. 

    ![Laboratuvar konumu ayarı yapılandırın](../media/how-to-manage-lab-accounts/labs-configuration-page-lab-location.png)


## <a name="specify-an-address-range-for-vms-in-the-lab"></a>Bir adres aralığı laboratuvarda VM'ler için belirtin.
Aşağıdaki yordam bir adres aralığı için Vm'leri Laboratuvardaki belirtmek için adım vardır. Daha önce belirttiğiniz aralığın güncelleştirirseniz, değiştirilen adres aralığı değişikliği yaptıktan sonra oluşturulan Vm'lere uygulanır. 

Dikkat etmeniz gereken adres aralığını belirtirken bazı kısıtlamalar şunlardır. 

- Önek değerinden küçük veya 23'a eşit olmalıdır. 
- Bir sanal ağ Laboratuvar hesabına eşlenirse, eşlenen sanal ağ adres aralığından ile sağlanan adres aralığı çakışamaz.

1. Üzerinde **Laboratuvar hesabı** sayfasında **Labs yapılandırma** sol menüsünde.
2. İçin **adres aralığı** alanında, laboratuvarda oluşturulan sanal makineler için adres aralığı belirtin. Adres aralığı içinde classless Inter-Domain yönlendirme (CIDR) gösterimi olmalıdır (örnek: 10.20.0.0/23). Bu adres aralığındaki laboratuvarındaki sanal makinelerde oluşturulur.
3. Araç çubuğunda **Kaydet**’i seçin. 

    ![Adres aralığını yapılandırın](../media/how-to-manage-lab-accounts/labs-configuration-page-address-range.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Bir laboratuvar hesabında sınıf laboratuvarı ayarlamak için kullanıcının ilgili laboratuvar hesabında **Laboratuvar Oluşturan** rolünün üyesi olması gerekir. Laboratuvar hesabını oluşturmak için kullandığınız hesap otomatik olarak bu role eklenir. Sınıf laboratuvarı oluşturmak için aynı kullanıcıyı kullanmayı planlıyorsanız bu adımı atlayabilirsiniz. Sınıf laboratuvarı oluşturmak için başka bir kullanıcı hesabı kullanmak istiyorsanız aşağıdaki adımları uygulayın: 

Eğitimcilere, sınıfları için laboratuvar oluşturma ve **Laboratuvar Oluşturan** rolüne bunları ekleme izni sağlamak için:

1. Üzerinde **Laboratuvar hesabı** sayfasında **erişim denetimi (IAM)**, tıklatıp **+ rol ataması Ekle** araç. 

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
Aşağıdaki makalelere bakın:

- [Laboratuvar sahibi olarak oluşturun ve Laboratuvarları yönetin](how-to-manage-classroom-labs.md)
- [Laboratuvar sahibi olarak ayarlama ve şablonları Yayımlama](how-to-create-manage-template.md)
- [Laboratuvar sahibi olarak yapılandırın ve Laboratuvar kullanımını denetleme](how-to-configure-student-usage.md)
- [Bir laboratuvar kullanıcı olarak sınıf laboratuvarlarına erişim](how-to-use-classroom-lab.md)
