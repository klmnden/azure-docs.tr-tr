---
title: Azure'da uygulamanızı test etme | Microsoft Docs
description: Bir laboratuar ortamında bir dosya paylaşımı oluşturma ve yerel makinenize ve Laboratuvar sanal makineler'de bağlama dosya paylaşımına masaüstü/web uygulamaları dağıtmanıza ve bunları test öğrenin.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.author: spelluru
ms.openlocfilehash: f8c57b9e1fabbd04a7d9c92484b0f52f074c2577
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2019
ms.locfileid: "65872500"
---
# <a name="test-your-app-in-azure"></a>Azure'da uygulamanızı test etme 
Bu makalede adımları uygulamanızı test etmek için Azure DevTest Labs'i kullanarak sağlar. İlk olarak, bir laboratuvar içindeki bir dosya paylaşımı ayarlama ve yerel geliştirme makineniz ve bir laboratuvar içindeki bir VM üzerinde bir sürücü olarak bağlayın. Ardından, böylece laboratuvara VM'de uygulamayı çalıştırabilir, dosya paylaşımına uygulamanızı dağıtmak için Visual Studio 2019 kullanın.  

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Önkoşullar 
1. [Bir Azure aboneliği oluşturun](https://azure.microsoft.com/free/) yoksa zaten varsa, ve oturum [Azure portalında](https://portal.azure.com).
2. Yönergeleri [bu makalede](devtest-lab-create-lab.md) Azure DevTest Labs'i kullanarak Laboratuvar oluşturmak için. Böylece, kolayca, yeniden oturum açtığınızda bulabilirsiniz Laboratuvar panonuza sabitleyin. Azure DevTest Labs, Azure içinde kaynaklar, hızlı bir şekilde Harcamaları ve maliyeti denetleme oluşturmanıza olanak sağlar. DevTest Labs hakkında daha fazla bilgi için bkz: [genel bakış](devtest-lab-overview.md). 
3. Konusundaki yönergeleri izleyerek Laboratuvar kaynak grubunda bir Azure depolama hesabı oluşturma [depolama hesabı oluşturma](../storage/common/storage-create-storage-account.md) makalesi. Üzerinde **depolama hesabı oluşturma** sayfasında **var olanı kullan** için **kaynak grubu**seçip **Laboratuvar kaynak grubu**. 
4. Konusundaki yönergeleri izleyerek, Azure depolamada bir dosya paylaşımı oluşturma [Azure dosyaları'nda bir dosya paylaşımı oluşturma](../storage/files/storage-how-to-create-file-share.md) makalesi. 

## <a name="mount-the-file-share-on-your-local-machine"></a>Yerel makinenizde dosya paylaşımını bağlama
1. Yerel makinenizde betikten kullanın [kalıcı Azure dosya paylaşımı kimlik bilgileri Windows](../storage/files/storage-how-to-use-files-windows.md#persisting-azure-file-share-credentials-in-windows) bölümünü [Windows ile Azure dosya paylaşımını kullanma](../storage/files/storage-how-to-use-files-windows.md) makalesi. 
2. Ardından, `net use` makinenizde dosya paylaşımını bağlamak için komutu. Örnek komut aşağıdaki gibidir: Komutu çalıştırmadan önce Azure depolama adı ve dosya paylaşım adı belirtin. 

    `net use Z: \\<YOUR AZURE STORAGE NAME>.file.core.windows.net\<YOUR FILE SHARE NAME> /persistent:yes`

## <a name="create-a-vm-in-the-lab"></a>Laboratuvarda VM oluşturma
1. Üzerinde **dosya paylaşımı** sayfasında **kaynak grubu** üstteki içerik haritası menüsünde. Gördüğünüz **kaynak grubu** sayfası. 
    
    ![İçerik haritası menüsünde kaynak grubunu seçin](media/test-app-in-azure/select-resource-group-bread-crump.png)
2. Üzerinde **kaynak grubu** sayfasında **Laboratuvar** DevTest Labs'de oluşturulan.

    ![Laboratuvarı seçme](media/test-app-in-azure/select-devtest-lab-in-resource-group.png)
3. Üzerinde **DevTest Labs** seçin, Laboratuvar için sayfa **+ Ekle** araç. 

    ![Laboratuvar düğmesi ekleme](media/test-app-in-azure/add-button-in-lab.png)
4. Üzerinde **temel seçin** sayfasında, arama **smalldisk**seçip **[smalldisk] Windows Server 2016 veri merkezi**. 

    ![Küçük bir disk Windows server'ı seçin](media/test-app-in-azure/choose-small-disk-windows-server.png)
5. Üzerinde **sanal makine** sayfasında, belirtin **sanal makine adı**, **kullanıcı adı**, **parola**seçip **oluştur** .    
    
    ![Sanal makine sayfası oluşturma](media/test-app-in-azure/create-virtual-machine-page.png)    

## <a name="mount-the-file-share-on-your-vm"></a>Sanal makinenizde dosya paylaşımını bağlama
1. Sanal makine başarıyla oluşturulduktan sonra seçin **sanal makine** listeden.    

    ![Laboratuvar sanal makinesi seçin](media/test-app-in-azure/select-lab-vm.png)
2. Seçin **Connect** VM'ye bağlanmak için araç çubuğunda. 
3. [Azure PowerShell'i yükleme](/powershell/azure/install-az-ps).
4. Dosya Paylaşımı bölümüne bağlama'ndaki yönergeleri izleyin. 

## <a name="publish-your-app-from-visual-studio"></a>Uygulamanızı Visual Studio'dan yayımlama
Bu bölümde, uygulamanızı Visual Studio'dan test VM'si bulut yayımlayın.

1. Visual Studio 2019'ı kullanarak bir masaüstü/web uygulaması oluşturun.
2. Uygulamanızı oluşturun.
3. Uygulamanızı yayımlamak için projenizde sağ **Çözüm Gezgini**seçip **Yayımla**. 
4. İçinde **yayımlama sihirbazını**, girin **sürücü** dosya paylaşımınızı eşlenir.

    **Masaüstü uygulaması:**

    ![Masaüstü uygulaması](media/test-app-in-azure/desktop-app.png)

    **Web uygulaması:**

    ![Web uygulaması](media/test-app-in-azure/web-app.png)

1. Seçin **sonraki** yayımlama iş akışını tamamlamak ve **son**. Sihirbaz adımlarını tamamladığınızda, Visual Studio uygulamanızı oluşturur ve dosya paylaşımınızı yayımlar. 


## <a name="test-the-app-on-your-test-vm-in-the-lab"></a>Test sanal makinenizin üzerinde uygulamayı laboratuar ortamında test etme

1. Laboratuvar VM için sanal makine sayfasına gidin. 
2. Seçin **Başlat** VM durumu Durduruldu ise başlatmak için araç çubuğunda. Başlatma ve durdurma her zaman önlemek sanal Makineniz için otomatik başlatma ve Otomatik kapatma ilkeleri ayarlayabilirsiniz. 
3. **Bağlan**’ı seçin.

    ![Sanal makine sayfası](media/test-app-in-azure/virtual-machine-page.png)
4. Sanal makinede başlatın **dosya Gezgini**seçip **bu PC** dosya paylaşımınızı bulunacak.

    ![VM üzerinde paylaşımı bulma](media/test-app-in-azure/find-share-on-vm.png)

    > [!NOTE]
    > Dosya paylaşımınızın sanal makinenizde yerel makinenizde bulamıyor veya varsa herhangi bir nedenle, bu çalıştırarak yeniden bağlamak `net use` komutu. Bulabilirsiniz `net use` komutunu **Connect** Sihirbazı'nın, **dosya paylaşımı** Azure portalında.
1. Dosya Paylaşımı'nı açın ve Visual Studio'dan dağıtılan uygulamayı gördüğünüzü onaylayın. 

    ![VM üzerinde paylaşımı açma](media/test-app-in-azure/open-file-share.png)

    Artık erişmek ve uygulamanızı Azure'da oluşturulan VM testin test edin.

## <a name="next-steps"></a>Sonraki adımlar
Sanal makineleri bir laboratuar ortamında kullanma hakkında bilgi edinmek için aşağıdaki makalelere bakın. 

- [VM'yi bir laboratuvara ekleme](devtest-lab-add-vm.md)
- [Bir laboratuvar sanal makinesi yeniden başlatın](devtest-lab-restart-vm.md)
- [Bir laboratuvar sanal makinesi yeniden boyutlandırma](devtest-lab-resize-vm.md)
