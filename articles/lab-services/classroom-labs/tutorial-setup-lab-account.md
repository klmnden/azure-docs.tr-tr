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
ms.date: 05/17/2018
ms.author: spelluru
ms.openlocfilehash: 600be7518bc526d3f147bb16377677854b676f63
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823138"
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services"></a>Öğretici: Azure Lab Services ile bir laboratuvar hesabı ayarlama
Azure Lab Services’te, bir laboratuvar hesabı, kuruluşunuzdaki laboratuvarların yönetildiği merkezi hesap olarak görev yapar. Laboratuvar hesabınızda, laboratuvar oluşturmak üzere başkalarına izin verin ve laboratuvar hesabı altındaki tüm laboratuvarlara uygulanan ilkeler ayarlayın. Bu öğreticide, laboratuvar yöneticisi olarak bir laboratuvar hesabı oluşturmayı öğrenin. 

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvar hesabı oluşturma
> * Laboratuvar Oluşturan rolüne kullanıcı ekleme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki ana menüden **Kaynak oluştur**’u seçin.
3. Azure Market’te **Lab Services** ifadesini aratıp açılan listeden **Lab Services** uygulamasını seçin. 
4. Filtrelenen sonuç listesinden **Lab Services (Önizleme)** öğesini seçin. 
1. **Laboratuvar hesabı oluştur** penceresinde **Oluştur**’u seçin.
2. **Laboratuvar hesabı** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin. 
    5. **Oluştur**’u seçin. 

        ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/lab-account-settings.png)
5. Laboratuvar hesabının sayfasını görmüyorsanız, **bildirimler** düğmesini seçin ve sonra bildirimlerdeki **Kaynağa git** düğmesine tıklayın. 

    ![Laboratuvar hesabı oluştur penceresi](../media/tutorial-setup-lab-account/notification-go-to-resource.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Eğitimcilere, sınıfları için laboratuvar oluşturma ve Laboratuvar Oluşturan rolüne bunları ekleme izni sağlamak için:

1. **Laboratuvar Hesabı** sayfasında **Erişim denetimi (IAM)** seçeneğini belirleyin ve araç çubuğunda **+ Ekle** düğmesine tıklayın. 

    ![Laboratuvar hesabı sayfası](../media/tutorial-setup-lab-account/access-control.png)
2. **İzin ekle** sayfasında, **Rol** için **Laboratuvar Oluşturan**’ı seçin, Laboratuvar Oluşturan rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet** seçeneğini belirleyin. 

    ![Laboratuvar Oluşturan rolüne kullanıcı ekleme](../media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir laboratuvar hesabı oluşturdunuz. Meslek olarak sınıf laboratuvarı oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)

