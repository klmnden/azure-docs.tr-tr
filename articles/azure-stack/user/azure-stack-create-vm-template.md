---
title: Bu öğreticide, Azure Stack şablon kullanarak VM oluşturma | Microsoft Docs
description: ASDK predfined şablon ve GitHub özel bir şablon kullanarak VM oluşturmak için nasıl kullanılacağını açıklar.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.custom: mvc
ms.date: 08/15/2018
ms.author: sethm
ms.reviewer: ''
ms.openlocfilehash: 5026a7a753ec744d281266b2fb30a70a66a7f9db
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2018
ms.locfileid: "42139666"
---
# <a name="tutorial-create-a-vm-using-a-community-template"></a>Öğretici: topluluk şablonu kullanarak bir VM oluşturma
Bir Azure Stack operatörü veya kullanıcı olarak kullanarak bir VM oluşturabilirsiniz [özel GitHub hızlı başlangıç şablonları](https://github.com/Azure/AzureStack-QuickStart-Templates) Azure Stack marketini bilgisayarından el ile dağıtma yerine.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Azure Stack hızlı başlangıç şablonları hakkında bilgi edinin 
> * Bir özel GitHub şablon kullanarak VM oluşturma
> * Minikube başlatın ve bir uygulama yükleyin

## <a name="learn-about-azure-stack-quickstart-templates"></a>Azure Stack hızlı başlangıç şablonları hakkında bilgi edinin
Azure Stack hızlı başlangıç şablonları depolanır [genel AzureStack hızlı başlangıç şablonları depo](https://github.com/Azure/AzureStack-QuickStart-Templates) GitHub üzerinde. Bu depo, Microsoft Azure Stack geliştirme Seti'ni (ASDK) ile test edilmiştir Azure Resource Manager dağıtım şablonlarını içerir. Azure Stack'i değerlendirin ve ASDK ortamınızda kolaylaştırmak için bunları kullanabilirsiniz. 

Zaman içinde çok sayıda GitHub kullanıcı 400'den fazla dağıtım şablonları büyük bir koleksiyonda kaynaklanan depoya katkıda buluna. Azure Stack için çeşitli ortamların nasıl dağıtabilirsiniz daha iyi anlamak için harika bir başlangıç noktası depodur. 

>[!IMPORTANT]
> Bu şablonların bazılarını, topluluk üyeleri tarafından ve Microsoft tarafından oluşturulur. Her şablon, Microsoft değil, sahibi tarafından bir lisans sözleşmesi altında lisanslanır. Microsoft, bu şablonları için sorumlu değildir ve güvenlik, uyumluluk veya performansı incelemez. Topluluk Şablonları herhangi bir Microsoft destek programı veya hizmeti altında desteklenmez ve kullanıma sunulan "Herhangi bir garanti olmaksızın olduğu gibi".

Azure Resource Manager şablonlarınızı github'a katkıda bulunmak istiyorsanız, katkılarınız için yapmalısınız [azure hızlı başlangıç şablonları depo](https://github.com/Azure/AzureStack-QuickStart-Templates).

GitHub deposunu ve kendisine katkıda bulunma hakkında daha fazla bilgi için bkz: [depoya ait Benioku dosyasını](https://github.com/Azure/AzureStack-QuickStart-Templates/blob/master/README.md). 


## <a name="create-a-vm-using-a-custom-github-template"></a>Bir özel GitHub şablon kullanarak VM oluşturma
Bu örnek öğreticide [101-vm-linux-minikube](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-linux-minikube) Azure Stack Hızlı Başlangıç şablonu AzureStack Minikube kubenetes kümesini yönetmek için çalışan bir Ubuntu 16.04 sanal makineye dağıtmak için kullanılır.

Minikube Kubernetes yerel olarak çalıştırılmasına olanak sağlayan bir araçtır. Minikube Kubernetes deneyin veya onunla geliştirmek isteyen kullanıcılar için bir VM içinde tek düğümlü bir Kubernetes kümesi günlük olarak çalıştırır. Bu basit, tek bir düğüm Kubernetes kümesi üzerinde bir Linux VM çalıştırma destekler. Bu tam olarak işlevsel bir Kubernetes kümesi çalıştıran almak için kullanılabilecek en hızlı ve anlaşılır en yoludur. Bu hizmet sayesinde geliştiriciler, geliştirme ve yerel makinelerde Kubernetes tabanlı uygulama dağıtımlarını test etmek. Bu mimari, Minikube VM hem ana hem de aracı düğümü bileşenlerinin yerel olarak çalıştırır:
- API sunucusu, Zamanlayıcı ve etcd sunucusu gibi ana düğümü bileşenlerinin LocalKube adlı tek bir Linux işlemde çalıştırılır.
- Tam olarak normal bir aracı düğüm üzerinde çalışır gibi aracı düğümü bileşenlerinin docker kapsayıcıları içinde çalışır. Bir uygulama dağıtım açısından fark yoktur bir Minikube veya normal Kubernetes kümesinde uygulama dağıtıldığında.

Bu şablon, aşağıdaki bileşenleri yükler:

- Ubuntu 16.04 LTS VM
- [Docker CE](https://download.docker.com/linux/ubuntu) 
- [Kubectl](https://storage.googleapis.com/kubernetes-release/release/v1.8.0/bin/linux/amd64/kubectl)
- [Minikube](https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64)
- xFCE4
- xRDP

> [!IMPORTANT]
> Ubuntu VM görüntüsü (Ubuntu Server 16.04 LTS Bu örnekte) zaten Azure Stack marketini için bu adımları başlamadan önce eklenmesi gerekir.

1.  Tıklayın **+ yeni** > **özel** > **şablon dağıtımı**.

    ![](media/azure-stack-create-vm-template/1.PNG) 

2. Tıklayın **şablonu Düzen**.

   ![](media/azure-stack-create-vm-template/2.PNG) 

3.  Tıklayın **Hızlı Başlangıç şablonu**.

       ![](media/azure-stack-create-vm-template/3.PNG)

4. Seçin **101-vm-linux-minikube** kullanarak kullanılabilir şablonlardan **bir şablon seçin** açılan listeler ve ardından **Tamam**.  

   ![](media/azure-stack-create-vm-template/4.PNG)

5. Aksi takdirde bunu yapmak veya tamamlandığında, tıklayın JSON şablonu değişiklikler yapmasını istiyorsanız **Kaydet** düzenleme şablonu iletişim kutusunu kapatmak için.

   ![](media/azure-stack-create-vm-template/5.PNG) 

6.  Tıklayarak **parametreleri**doldurun veya kullanılabilir alanları gerektiği gibi değiştirin ve ardından **Tamam**. Aboneliği kullanın, oluşturun veya mevcut bir kaynak grubu adı seçin ve ardından seçin **Oluştur** şablon dağıtımını başlatmak için.

       ![](media/azure-stack-create-vm-template/6.PNG)

7. Aboneliği kullanın, oluşturun veya mevcut bir kaynak grubu adı seçin ve ardından seçin **Oluştur** şablon dağıtımını başlatmak için.

   ![](media/azure-stack-create-vm-template/7.PNG)

8. Kaynak grubu dağıtımı özel şablon tabanlı VM oluşturmak için birkaç dakika sürer. Yükleme durumu ve kaynak grubu özelliklerini bildirimler aracılığıyla izleyebilirsiniz. 

   ![](media/azure-stack-create-vm-template/8.PNG)

   >[!NOTE]
   > Dağıtım tamamlandığında, VM çalıştırırsınız. 

## <a name="start-minikube-and-install-an-application"></a>Minikube başlatın ve bir uygulama yükleyin
Linux sanal makinesi başarıyla oluşturuldu, size minikube başlatmak ve uygulama yüklemek oturum açabilir. 

1. Dağıtım tamamlandıktan sonra tıklayın **Connect** Linux VM'ye bağlanmak için kullanılan genel IP adresini görüntülemek için. 

   ![](media/azure-stack-create-vm-template/9.PNG)

2. Yükseltilmiş bir komut isteminden çalıştırın **mstsc.exe** Uzak Masaüstü Bağlantısı açın ve önceki adımda bulunan Linux sanal makinenin genel IP adresine bağlanın. XRDP için oturum açmanız istendiğinde, sanal makine oluştururken belirttiğiniz kimlik bilgilerini kullanın.

   ![](media/azure-stack-create-vm-template/10.PNG)

3. Terminal öykünücü açın ve minikube başlatmak için aşağıdaki komutları girin:

    >    `sudo minikube start --vm-driver=none`
    >   
    >    `sudo minikube addons enable dashboard`
    >    
    >    `sudo minikube dashboard --url`

   ![](media/azure-stack-create-vm-template/11.PNG)

4. Web tarayıcısı açın ve kubernetes Panosu adresini ziyaret edin. Tebrikler, artık minikube kullanarak tam olarak çalışan bir kubernetes yüklemesine var!

   ![](media/azure-stack-create-vm-template/12.PNG)

5. Bir örnek uygulamasını dağıtmak istiyorsanız, kubernetes resmi belge sayfasını ziyaret edin, önceden bir yukarıda oluşturduğunuz olarak "Minikube küme oluşturma" bölümüne atlayın. Yalnızca bölümüne "Node.js uygulamanızı oluşturma" en hızlı https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube/.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Azure Stack hızlı başlangıç şablonları hakkında bilgi edinin 
> * Bir özel GitHub şablon kullanarak VM oluşturma
> * Minikube başlatın ve bir uygulama yükleyin

