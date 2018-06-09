---
title: Bu öğreticide, bir Azure yığın şablonu kullanarak bir VM oluşturma | Microsoft Docs
description: ASDK predfined şablonu ve GitHub özel bir şablon kullanarak bir VM oluşturmak için nasıl kullanılacağını açıklar.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 06/07/2018
ms.author: brenduns
ms.reviewer: ''
ms.openlocfilehash: e772dc41ce2cb77a03b91515cae35ffc48f5dbc3
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/08/2018
ms.locfileid: "35238504"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Öğretici: bir topluluk şablonu kullanarak bir VM oluşturma
Bir Azure yığın işleci veya kullanıcı olarak kullanarak bir VM oluşturabilirsiniz [özel GitHub hızlı başlangıç şablonlarını](https://github.com/Azure/AzureStack-QuickStart-Templates) bir Azure yığın Marketi'nden el ile dağıtma yerine.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure yığın hızlı başlangıç şablonları hakkında bilgi edinin 
> * Özel bir GitHub şablonu kullanarak bir VM oluşturma
> * Minikube başlatın ve bir uygulama yükleyin

## <a name="learn-about-azure-stack-quickstart-templates"></a>Azure yığın hızlı başlangıç şablonları hakkında bilgi edinin
Azure yığın hızlı başlangıç şablonları depolanır [genel AzureStack hızlı başlangıç şablonlarını depo](https://github.com/Azure/AzureStack-QuickStart-Templates) github'da. Bu depo, Microsoft Azure yığın Geliştirme Seti (ASDK) ile test edilmiştir Azure Resource Manager dağıtım şablonları içerir. Azure yığın değerlendirmek ve ASDK ortamı kullanmak kolaylaştırmak için bunları kullanabilirsiniz. 

Zaman içinde çok sayıda GitHub kullanıcı büyük 400'den fazla dağıtım şablonları koleksiyonunda kaynaklanan depoya katkıda. Çeşitli ortamlar için Azure yığın nasıl dağıtabileceğiniz daha iyi anlamak için harika bir başlangıç noktası depodur. 

>[!IMPORTANT]
> Bu şablonların bazıları, topluluk üyeleri tarafından ve Microsoft tarafından oluşturulur. Her şablon bir lisans sözleşmesi kapsamında Microsoft'un değil, sahibi tarafından lisanslanmıştır. Microsoft, bu şablonları sorumlu değildir ve güvenlik, uyumluluk veya performansı incelemez. Topluluk Şablonları herhangi bir Microsoft destek programı veya hizmeti altında desteklenmez ve kullanılabilir hale getirilir "Hiçbir garanti olduğu gibi".

Azure Resource Manager şablonlarınızı GitHub için katkıda istiyorsanız, katkılarınız için olmalısınız [azure hızlı başlangıç şablonlarını depo](https://github.com/Azure/AzureStack-QuickStart-Templates).

GitHub depo ve ona katkıda bulunma hakkında daha fazla bilgi için bkz: [depodaki'nın Benioku dosyasını](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md). 


## <a name="create-a-vm-using-a-custom-github-template"></a>Özel bir GitHub şablonu kullanarak bir VM oluşturma
Bu örnek öğreticideki [101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) Azure yığın hızlı başlatma şablonunu AzureStack Minikube kubenetes kümeyi yönetmek için çalışan bir Ubuntu 16.04 sanal makineyi dağıtmak için kullanılır.

Minikube Kubernetes yerel olarak çalıştırmak kolaylaştıran bir araçtır. Minikube Kubernetes deneyin veya onunla geliştirmek isteyen kullanıcılar için bir VM içinde tek düğümlü bir Kubernetes küme günlük çalıştırır. Bu basit, tek bir düğüm Kubernetes küme üzerinde bir Linux VM çalıştıran destekler. Bu çalışan tam olarak işlevsel bir Kubernetes küme almak için hızlı ve en doğrudan iletme yoludur. Geliştiricilerin geliştirmek ve kendi yerel makinede Kubernetes tabanlı uygulama dağıtımlarını test olanak tanır. Mimari, Minikube VM hem ana hem de aracı düğümü bileşenlerini yerel olarak çalışır:
- API sunucunuz, Zamanlayıcı ve etcd sunucu gibi ana düğüm bileşenleri LocalKube adlı tek bir Linux işlem içinde çalıştırılır.
- Tam olarak normal bir aracı düğümünde çalıştırılır olarak Aracısı düğümü bileşenlerinin docker kapsayıcılarda çalıştırılır. Bir uygulama dağıtım açısından fark yoktur uygulama Minikube ya da normal Kubernetes küme dağıtıldığında.

Bu şablon, aşağıdaki bileşenleri yükler:

- Ubuntu 16.04 LTS VM
- [Docker CE](https://download.docker.com/linux/ubuntu) 
- [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
- [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
- xFCE4
- xRDP

> [!IMPORTANT]
> Ubuntu VM görüntüsü (Ubuntu Server 16.04 LTS Bu örnekte) zaten Azure yığın Market adımları başlamadan önce eklenmiş olan gerekir.

1.  Tıklatın **+ yeni** > **özel** > **şablon dağıtımı**.

    ![](media/azure-stack-create-vm-template/1.PNG) 

2. Tıklatın **Düzen şablonu**.

   ![](media/azure-stack-create-vm-template/2.PNG) 

3.  Tıklatın **hızlı başlatma şablonunu**.

       ![](media/azure-stack-create-vm-template/3.PNG)

4. Seçin **101-vm-linux-minikube** kullanarak kullanılabilir şablonlardan **bir şablon seçin** açılır liste ve ardından **Tamam**.  

   ![](media/azure-stack-create-vm-template/4.PNG)

5. Aksi durumda bunun veya tamamlandığında, tıklayın JSON şablonu değişiklikler yapmak istiyorsanız, **kaydetmek** Düzen şablonu iletişim kutusunu kapatmak için.

   ![](media/azure-stack-create-vm-template/5.PNG) 

6.  Tıklayın **parametreleri**doldurun veya kullanılabilir alanlar gerektiği gibi değiştirin ve ardından **Tamam**. Abonelik kullanmak, oluşturun veya varolan bir kaynak grubu adı seçin ve ardından seçin **oluşturma** şablon dağıtımını başlatmak için.

       ![](media/azure-stack-create-vm-template/6.PNG)

7. Abonelik kullanmak, oluşturun veya varolan bir kaynak grubu adı seçin ve ardından seçin **oluşturma** şablon dağıtımını başlatmak için.

   ![](media/azure-stack-create-vm-template/7.PNG)

8. Kaynak grubu dağıtım özel şablon tabanlı VM oluşturmak için birkaç dakika sürer. Bildirimler arasında ve kaynak grubu özelliklerinden yükleme durumunu izleyebilirsiniz. 

   ![](media/azure-stack-create-vm-template/8.PNG)

   >[!NOTE]
   > Dağıtım tamamlandığında VM'nin çalışır. 

## <a name="start-minikube-and-install-an-application"></a>Minikube başlatın ve bir uygulama yükleyin
Linux VM başarıyla oluşturuldu, size minikube başlatmak ve bir uygulama yüklemek için oturum açabilir. 

1. Dağıtım tamamlandıktan sonra **Bağlan** Linux VM'ye bağlanmak için kullanılan genel IP adresini görüntüleyin. 

   ![](media/azure-stack-create-vm-template/9.PNG)

2. Yükseltilmiş bir komut isteminden çalıştırın **mstsc.exe** Uzak Masaüstü Bağlantısı açın ve önceki adımda bulunan Linux VM ortak IP adresine bağlanın. XRDP için oturum açmanız istendiğinde VM oluşturulurken belirtilen kimlik bilgilerini kullanın.

   ![](media/azure-stack-create-vm-template/10.PNG)

3. Terminal öykünücüsü açın ve minikube başlatmak için komutları girin:

    >    `sudo minikube start --vm-driver=none`
    >   
    >    `sudo minikube addons enable dashboard`
    >    
    >    `sudo minikube dashboard --url`

   ![](media/azure-stack-create-vm-template/11.PNG)

4. Web tarayıcısını açın ve kubernetes Pano adresini ziyaret edin. Tebrikler, artık minikube kullanarak tam olarak çalışan kubernetes yüklemesi vardır.

   ![](media/azure-stack-create-vm-template/12.PNG)

5. Örnek bir uygulama dağıtmak istiyorsanız, kubernetes resmi belgeleri sayfasını ziyaret edin, biri yukarıdaki oluşturmuş gibi "Minikube küme oluşturma" bölümüne atlayın. Yalnızca bölüme "Node.js uygulamanızı oluşturma" konumundaki atlamak https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure yığın hızlı başlangıç şablonları hakkında bilgi edinin 
> * Özel bir GitHub şablonu kullanarak bir VM oluşturma
> * Minikube başlatın ve bir uygulama yükleyin

