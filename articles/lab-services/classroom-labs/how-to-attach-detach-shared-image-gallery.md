---
title: Ekleme veya bir Azure Lab Services paylaşılan görüntü galerisinde ayırma | Microsoft Docs
description: Azure Lab Services laboratuvarda bir paylaşılan görüntü Galerisine ekleme hakkında bilgi edinin.
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
ms.openlocfilehash: de4e9fb4b15f4c346926fe46f23255c668204c2e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65413884"
---
# <a name="attach-or-detach-a-shared-image-gallery-in-azure-lab-services"></a>Ekleme veya bir Azure Lab Services paylaşılan görüntü galerisinde ayırma
Öğretmenler/Laboratuvar Yönetimi, bir Azure şablonu VM görüntüsü kaydedebilir [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md) bu başkaları tarafından yeniden kullanılabilir. İlk adım, Laboratuvar Yöneticisi var olan bir paylaşılan görüntü Galerisine Laboratuvar hesabına ekler. Paylaşılan görüntü Galerisi bağlandıktan sonra labs Laboratuvar hesabında oluşturulan görüntüleri paylaşılan görüntü Galerisine kaydedebilirsiniz. Diğer Öğretmenler, bu görüntü, kendi sınıfları için bir şablon oluşturmak için paylaşılan bir görüntü galerisinden seçebilirsiniz. 

Bu makalede ekleme veya bir laboratuvar hesabı paylaşılan görüntü Galerisine ayırma gösterilmektedir. 

## <a name="configure-at-the-time-of-lab-account-creation"></a>Laboratuvar hesap oluşturma sırasında yapılandırma
Bir laboratuvar hesabı oluştururken bir paylaşılan görüntü Galerisi Laboratuvar hesabına ekleyebilirsiniz. Aşağı açılan listeden var olan bir paylaşılan görüntü Galerisine seçin veya yeni bir tane oluşturun. Oluşturma ve paylaşılan görüntü Galerisi Laboratuvar hesaba iliştirebilir için **Yeni Oluştur**, Galeri için bir ad girin ve girin **Tamam**. 

![Paylaşılan görüntü Galerisi Laboratuvar hesap oluşturma sırasında yapılandırma](../media/how-to-use-shared-image-gallery/new-lab-account.png)

## <a name="configure-after-the-lab-account-is-created"></a>Bir laboratuvar hesabı oluşturulduktan sonra yapılandırma
Bir laboratuvar hesabı oluşturulduktan sonra aşağıdaki görevleri gerçekleştirebilirsiniz:

- Oluşturma ve paylaşılan görüntü Galerisine ekleme
- Paylaşılan görüntü Galerisi Laboratuvar hesaba iliştirebilir.
- Bir laboratuvar hesabı paylaşılan görüntü galerisinden Ayır

## <a name="create-and-attach-a-shared-image-gallery"></a>Oluşturma ve paylaşılan görüntü Galerisine ekleme
1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Seçin **tüm hizmetleri** sol menüsünde. Seçin **Lab Services** içinde **DEVOPS** bölümü. Yıldız seçerseniz (`*`) yanındaki **Lab Services**, eklenir **Sık Kullanılanlar** sol menüde bölümü. Bir sonraki zamandan ve sonraki sürümlerde, seçtiğiniz **Lab Services** altında **Sık Kullanılanlar**.

    ![Lab Services'i tüm hizmetleri ->](../media/tutorial-setup-lab-account/select-lab-accounts-service.png)
3. Görmek için laboratuvar hesabınızı seçin **Laboratuvar hesabı** sayfası. 
4. Seçin **paylaşılan görüntü Galerisi** seçin ve soldaki menüden **+ Oluştur** araç.  

    ![Paylaşılan görüntü Galerisi düğme oluşturma](../media/how-to-use-shared-image-gallery/new-shared-image-gallery-button.png)
5. İçinde **Oluştur paylaşılan görüntü Galerisi** penceresinde girin bir **adı** ve galerinin girin **Tamam**. 

    ![Paylaşılan görüntü Galerisi penceresi oluştur](../media/how-to-use-shared-image-gallery/create-shared-image-gallery-window.png)

    Azure Lab Services paylaşılan görüntü Galerisi oluşturur ve Laboratuvar hesabınıza bağlı. Bu Laboratuvar hesabında oluşturulan tüm labs ekli paylaşılan görüntü Galerisine erişebilirsiniz. 

    ![Eklenen resmin Galerisi](../media/how-to-use-shared-image-gallery/image-gallery-in-list.png)

    Alt bölmede resimleri paylaşılan görüntü galerisinde bakın. Bu yeni galerisinde resim yok. Galeride görüntü karşıya yüklediğinizde bu sayfasında görebilirsiniz.     

    Ekli paylaşılan görüntü Galerisi'ndaki tüm görüntüler varsayılan olarak etkindir. Etkinleştirebilir veya bunları listeden ve kullanarak seçilen görüntüler devre dışı **seçilen görüntüler etkinleştirme** veya **seçilen görüntüler devre dışı** düğmesi.

## <a name="attach-an-existing-shared-image-gallery"></a>Var olan bir paylaşılan görüntü Galerisine ekleme
Aşağıdaki yordamı, bir laboratuvar hesabı için var olan bir paylaşılan görüntü Galerisine ekleme işlemini göstermektedir. 

1. Üzerinde **Laboratuvar hesabı** sayfasında **paylaşılan görüntü Galerisi** seçin ve soldaki menüden **iliştirme** araç. 

    ![Paylaşılan görüntü Galerisi - düğmesi ekleme](../media/how-to-use-shared-image-gallery/sig-attach-button.png)
5. Üzerinde **var olan bir paylaşılan görüntü Galerisine ekleme** seçin sayfasında ve paylaşılan görüntü Galerisi **Tamam**.

    ![Mevcut bir galeriyi seçin](../media/how-to-use-shared-image-gallery/select-image-gallery.png)
6. Aşağıdaki ekranı görürsünüz: 

    ![Galerim listesinde](../media/how-to-use-shared-image-gallery/my-gallery-in-list.png)
    
    Bu örnekte, vardır hiçbir görüntü paylaşılan görüntü galerisinde henüz.

    Azure Lab Services kimlik laboratuvara bağlı paylaşılan görüntü Galerisine katkıda bulunan olarak eklenir. Bu, Öğretmenler / sanal makine olarak kaydetmek için BT yöneticisi, paylaşılan görüntü Galerisine görüntüler. Bu Laboratuvar hesabında oluşturulan tüm labs ekli paylaşılan görüntü Galerisine erişebilirsiniz. 

    Ekli paylaşılan görüntü Galerisi'ndaki tüm görüntüler varsayılan olarak etkindir. Etkinleştirebilir veya bunları listeden ve kullanarak seçilen görüntüler devre dışı **seçilen görüntüler etkinleştirme** veya **seçilen görüntüler devre dışı** düğmesi. 

## <a name="save-an-image-to-the-shared-image-gallery"></a>Paylaşılan görüntü Galerisine bir görüntüyü kaydedin
Paylaşılan görüntü Galerisi eklendikten sonra bir laboratuvar hesabı yöneticisi veya bir Öğretmen kaydedebilir veya diğer Öğretmen tarafından yeniden kullanılabilir, böylece paylaşılan görüntü Galerisine bir görüntü yükleyin. Paylaşılan görüntü Galerisine bir görüntü yüklemek için yönergeler için bkz: [paylaşılan görüntü Galerisi genel bakış](../../virtual-machines/windows/shared-images.md). 

> [!NOTE]
> Sınıf Laboratuvarlarını kullanıcı arabirimi (UI) curently, paylaşılan bir görüntü Galerisine bir laboratuvar görüntüsü kaydediliyor desteklemiyor. 

## <a name="detach-a-shared-image-gallery"></a>Paylaşılan görüntü Galerisi Ayır
Tek bir paylaşılan görüntü Galerisi laboratuvara eklenebilir. Başka bir paylaşılan görüntü Galerisi eklemek istiyorsanız, önce yeni bir ekleme geçerli bir ayırma. Laboratuvarınızı paylaşılan görüntü galerisinden ayırmak için seçin **ayırma** araç çubuğunda ve ayırma işlemi onaylayın. 

![Laboratuvar hesabı paylaşılan görüntü galerisinden Ayır](../media/how-to-use-shared-image-gallery/detach.png)

## <a name="next-steps"></a>Sonraki adımlar
Paylaşılan görüntü Galerisine bir laboratuvar görüntüsünü kaydetmek veya bir VM oluşturmak için paylaşılan bir görüntü galerisinden bir görüntü kullanma hakkında bilgi edinmek için [paylaşılan görüntü Galerisi kullanmayı](how-to-use-shared-image-gallery.md).

Resim galerileri genel olarak paylaşılan hakkında daha fazla bilgi için bkz: [paylaşılan görüntü Galerisi](../../virtual-machines/windows/shared-image-galleries.md).
