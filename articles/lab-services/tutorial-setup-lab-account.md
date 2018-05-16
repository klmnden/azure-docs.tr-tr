---
title: Azure Lab Services ile laboratuvar hesabı ayarlama | Microsoft Docs
description: Bu öğreticide, Azure Lab Services (önceki adıyla DevTest Labs) ile bir laboratuvar hesabı ayarlarsınız.
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
ms.date: 04/26/2018
ms.author: spelluru
ms.openlocfilehash: 5af348bfdccb9392948af962cf582e33ebd40872
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-set-up-a-lab-account-with-azure-lab-services-formerly-azure-devtest-labs"></a>Öğretici: Azure Lab Services (önceki adıyla DevTest Labs) ile bir laboratuvar hesabı ayarlama
Bu öğreticide, Azure Lab Services ile laboratuvar hesabı oluşturmak için laboratuvar yöneticisi olarak hareket edersiniz. Ardından eğitimcilere, bu laboratuvar hesabında sınıfları için laboratuvar oluşturma izni sağlarsınız. Eğitimci, [Azure Lab Services web sitesini](https://labs.azure.com) kullanarak bir sınıf için laboratuvar ayarlayabilir.   

Bu öğreticide, aşağıdaki eylemleri gerçekleştireceksiniz:

> [!div class="checklist"]
> * Laboratuvar hesabı oluşturma
> * Laboratuvar Oluşturan rolüne kullanıcı ekleme

## <a name="prerequisites"></a>Ön koşullar

- Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
- Azure Lab Services şu anda geçitli önizleme aşamasında. Bir laboratuvar hesabı oluşturmak için [önizlemeye kaydolun](https://aka.ms/azlabspreviewsignup).

## <a name="create-a-lab-account"></a>Laboratuvar hesabı oluşturma
Aşağıdaki adımlar, Azure portalını kullanarak Azure Lab Services ile nasıl bir laboratuvar hesabı oluşturulacağını göstermektedir. 

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol taraftaki ana menüden **Kaynak oluştur**’u (listenin en üstünde) seçin, **Geliştirici araçları**’nın üzerine gelin ve **Lab Services (önizleme)** seçeneğine tıklayın.
1. **Laboratuvar hesabı oluştur** penceresinde **Oluştur**’u seçin.
2. **Laboratuvar hesabı** penceresinde aşağıdaki eylemleri gerçekleştirin: 
    1. **Laboratuvar hesabı adı** için bir ad girin. 
    2. Laboratuvar hesabı oluşturmak istediğiniz **Azure aboneliğini** seçin.
    3. **Kaynak grubu** için, **Yeni oluştur**’u seçip kaynak grubu için bir ad girin.
    4. **Konum** için, laboratuvar hesabının oluşturulmasını istediğiniz bir konumu/bölgeyi seçin. 
    5. **Oluştur**’u seçin. 

        ![Laboratuvar hesabı oluştur penceresi](./media/tutorial-setup-lab-account/lab-account-settings.png)
5. Laboratuvar hesabının sayfasını görmüyorsanız, **bildirimler** düğmesini seçin ve sonra bildirimlerdeki **Kaynağa git** düğmesine tıklayın. 

    ![Laboratuvar hesabı oluştur penceresi](./media/tutorial-setup-lab-account/notification-go-to-resource.png)    
6. Aşağıdaki **laboratuvar hesabı** sayfasını görürsünüz:

    ![Laboratuvar hesabı sayfası](./media/tutorial-setup-lab-account/lab-account-page.png)

## <a name="add-a-user-to-the-lab-creator-role"></a>Laboratuvar Oluşturan rolüne kullanıcı ekleme
Eğitimcilere, sınıfları için laboratuvar oluşturma ve Laboratuvar Oluşturan rolüne bunları ekleme izni sağlamak için:

1. **Laboratuvar Hesabı** sayfasında **Erişim denetimi (IAM)** seçeneğini belirleyin ve araç çubuğunda **+ Ekle** düğmesine tıklayın. 

    ![Laboratuvar hesabı sayfası](./media/tutorial-setup-lab-account/access-control.png)
2. **İzin ekle** sayfasında, **Rol** için **Laboratuvar Oluşturan**’ı seçin, Laboratuvar Oluşturan rolüne eklemek istediğiniz kullanıcıyı seçin ve **Kaydet** seçeneğini belirleyin. 

    ![Laboratuvar Oluşturan rolüne kullanıcı ekleme](./media/tutorial-setup-lab-account/add-user-to-lab-creator-role.png)


## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir laboratuvar hesabı oluşturdunuz. Meslek olarak sınıf laboratuvarı oluşturma hakkında bilgi edinmek için sonraki öğreticiye ilerleyin:

> [!div class="nextstepaction"]
> [Bir sınıf laboratuvarı ayarlama](tutorial-setup-classroom-lab.md)

