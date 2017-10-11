---
title: "Azure Batch havuzları özel görüntülerden sağlama | Microsoft Docs"
description: "Özel görüntü havuzundan sağlamak için işlem yazılım ve uygulamanız için gereksinim duyduğunuz verileri içeren düğümlerini toplu oluşturabilirsiniz. Özel resimler toplu iş yüklerini çalıştırmak için işlem düğümleri yapılandırmak için etkili bir yoludur."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.openlocfilehash: 3d655766b4f2a5efb0c8c29ffa81a89f84b3e17c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Sanal makinelerin bir havuz oluşturmak için özel bir görüntü kullanın

Azure Batch sanal makinelerin bir havuz oluşturduğunuzda, havuzundaki her işlem düğümü için işletim sistemi sağlayan bir sanal makine (VM) görüntüsü belirtin. Azure Market görüntü veya hazırladığınız özel bir VHD görüntüsü sağlayarak, sanal makinelerin bir havuz oluşturabilirsiniz. Özel görüntü sağladığınızda, her işlem düğümü sağlanan zaman işletim sisteminin nasıl yapılandırıldığına üzerinde kontrol sahibidir. Özel görüntünüzü de uygulamalar ve sağlandığından hemen işlem düğümü üzerinde kullanılabilir olan başvuru verileri içerir.

Özel görüntü kullanarak havuza ait alınırken zaman kazandırır, toplu iş yükünü çalıştırmaya hazır işlem düğümleri. Her zaman Azure Market görüntü kullanır ve bunu sağlandıktan sonra her işlem düğümünde yazılım yüklerseniz olsa da, bu yaklaşım özel görüntü kullanmaktan daha az verimli olabilir. 

Gerek senaryonuz için yapılandırılan özel bir görüntü kullanmak için bazı nedenler şunlardır:

- **İşletim sistemi (OS) yapılandırma** işletim sisteminin özel bir yapılandırma özel görüntü üzerinde gerçekleştirilebilir. 
- **Büyük uygulamalar yükleyin.** Özel bir görüntüde uygulamaları yükleme, sağlandıktan sonra her işlem düğümünde yüklemeden daha daha verimli olur.
- **Önemli miktarda veri kopyalayın.** Özel görüntü verileri kopyaladıysanız, yalnızca bir kez yerine zaman ve bant genişliği tasarrufu her işlem düğümü kopyalanacak gerekir.
- **VM Kurulum işlemi sırasında yeniden başlatın.** Özellikle işlem düğümlerinin sayısını varsa VM yeniden başlatıldığı zaman alan bir işlem olabilir.

## <a name="prerequisites"></a>Ön koşullar

- **Kullanıcı abonelik havuzu ayırma modu kullanılarak oluşturulmuş bir Batch hesabı.** Sanal makine havuzları sağlamak için özel bir görüntü kullanmak için sahip bir kullanıcı abonelik toplu işlem hesabınızı oluşturmak [havuzu ayırma modu](batch-api-basics.md#pool-allocation-mode). Bu modu kullandığınızda Batch havuzları, hesabın bulunduğu aboneliğe ayrılır. Bkz: [hesap](batch-api-basics.md#account) bölümüne [geliştirme büyük ölçekli paralel işlem çözümleri yığın](batch-api-basics.md) toplu işlem hesabı oluşturduğunuzda, Havuz ayırma modunu ayarlama hakkında daha fazla bilgi için.

- **Bir Azure Depolama hesabı.** Özel görüntü kullanarak sanal makineleri havuzu oluşturmak için bir standart, genel amaçlı Azure depolama hesabı aynı abonelikte ve bölgede gerekir. Bir Azure sanal makineden özel görüntü oluşturursanız, burada VM'ın işletim sistemi diski bulunur ve ayrı bir depolama hesabı oluşturmanız gerekmez depolama hesabına görüntü kopyalayacak. 
    
## <a name="prepare-a-custom-image"></a>Özel görüntü hazırlama

Özel görüntü Batch ile kullanmak üzere hazırlamak için Linux veya Windows varolan bir yüklemesini generalize gerekir. Bir işletim sistemi yüklemesi genelleme makine özgü bilgileri kaldırır. Diğer bilgisayarlar veya sanal makineleri yüklü bir görüntü sonucudur.  

> [!IMPORTANT]
> Batch şu anda desteklemiyor Azure kullanarak yönetilen bir havuz sağlamak için görüntüler. Bir havuz sağlamak için kullandığınız özel görüntüyü Azure depolama alanında depolanmalıdır. 
>
> Özel görüntünüzü hazırlık yaparken, aşağıdaki noktaları göz önünde bulundurun:
> - Batch havuzlarını sağlamak için kullandığınız temel işletim sistemi görüntüsünün, özel betik uzantıları gibi önceden yüklenmiş Azure uzantılarına sahip olmadığından emin olun. Görüntü önceden yüklenmiş bir uzantı içeriyorsa Azure, VM dağıtımı sırasında sorunla karşılaşabilir.
> - Batch düğüm aracısı varsayılan geçici sürücüyü beklediğinden, sağladığınız temel işletim sistemi görüntüsünün varsayılan geçici sürücüyü kullandığından emin olun.
>
>

Özel görüntü var olan tüm hazırlanan Windows veya Linux görüntüsü kullanabilirsiniz. Yerel görüntü kullanmak isterseniz, örneğin, sonra görüntüyü aynı abonelik ve toplu işlem hesabı kullanarak olarak bölgesi olan bir Azure depolama hesabı için karşıya yükleme [AzCopy](../storage/storage-use-azcopy.md) veya başka bir karşıya yükleme aracı.

Ayrıca, özel bir görüntü yeni veya var olan bir Azure VM'den hazırlayabilirsiniz. Yeni bir VM oluşturuyorsanız, bir Azure Market görüntüsü için özel görüntünüzü temel görüntü kullanın ve sonra özelleştirebilirsiniz. Temel görüntü özelleştirmek için bir Azure VM oluşturma ve uygulamaları veya veri ekleyin. Ardından, özel görüntü sunmak ve Azure depolamasına kaydetmek için VM genelleştirin. 

Özel görüntü bir Azure sanal makineden hazırlamak için aşağıdaki adımları izleyin:

1. Oluşturma bir **yönetilmeyen** Azure Market görüntüsünden Azure VM. Azure Market her ikisi için görüntüleri içeren [Windows](../virtual-machines/windows/quick-create-portal.md) ve [Linux](../virtual-machines/linux/quick-create-portal.md).
    
    3. adım VM oluşturma işleminin üzerinde seçtiğinizden emin olun **Hayır** için **Depolama: yönetilen diskleri kullanmak** seçeneği. Ayrıca bu depolama hesabı da Azure özel görüntünüzü kaydedeceği olarak sanal makinenin işletim sistemi diski, depolama hesabı adını not edin:

    ![Yönetilmeyen bir VM oluşturun ve depolama hesabı adını not edin](media/batch-custom-images/vm-create-storage.png)
 
2. VM oluşturma işlemi tamamlayın ve Azure tarafından ayrılan bekleyin. Azure portalında çalışır durumda bir VM gösteren bir resim şöyledir:

    ![Market görüntüsünden bir VM oluşturma](media/batch-custom-images/vm-status-running.png)

3. VM çalışmaya başladıktan sonra hatta (Windows için) RDP veya SSH (için Linux) aracılığıyla bağlayın. Gerekli yazılımları yüklemek veya istenen verileri kopyalayın ve VM genelleştirin. Bölümünde açıklanan adımları [VM Generalize](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized.md#generalize-the-vm). 
   
4. Adımlarını izleyin [Azure PowerShell oturum](../virtual-machines/windows/sa-copy-generalized.md#log-in-to-azure-powershell). Azure PowerShell'i yüklemek için bkz: [Azure PowerShell genel bakış](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.2.0). 

5. Ardından, adımları [VM serbest bırakma ve durumu genelleştirilmiş kümesine](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized#deallocate-the-vm-and-set-the-state-to-generalized). 

    Azure portalında VM serbest dikkat edin:

    ![VM serbest emin olun](media/batch-custom-images/vm-status-deallocated.png)

6.  Oluşturun ve Azure Storage kullanarak VM görüntüsü kaydedin [Kaydet AzureRmVMImage](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) PowerShell cmdlet'i. Bölümünde açıklanan yönergeleri izleyin [görüntü oluşturma](../virtual-machines/windows/sa-copy-generalized.md#create-the-image).
    
    VM görüntüsü VM oluşturulduğunda, bu yordamın 1. adımında gösterildiği gibi oluşturulan Azure Storage hesabına kaydedilir. Kaydet-AzureRmVMImage cmdlet'i görüntüye kaydeder **sistem** bu depolama hesabı kapsayıcısında. `-DestinationContainername` Parametre adları bir sanal dizin içinde **sistem** kapsayıcı. `-VHDNamePrefix` Parametresi, blob adı için bir önek belirtir. Bu ön ek blob adı bir tire ile birlikte eklenir. 

    Örneğin, kaydetme AzureRmVMImage aşağıdaki parametrelerle çağırın varsayın:  

        Save-AzureRmVMImage -ResourceGroupName sample-resource-group -Name vm-custom-image -DestinationContainerName batchimages -VHDNamePrefix custom -Path C:\Temp\Images\vm-custom-image.json

    Elde edilen görüntü burada gösterilen blob adı ve konumu kaydedilir:

    ![Sistem kapsayıcısında kaydedilmiş VHD konumu](media/batch-custom-images/vhd-in-vm-storage-account.png)

    > [!NOTE]
    > Bir Azure yönetilmeyen VM birkaç depolama hesapları farklı amaçlar için oluşturur. VM oluşturulduğunda işletim sistemi diski için depolama kapsayıcısı adını not etmediniz, ilişkili depolama hesabı Bul içeren **sistem** kapsayıcı. Gezinmek **sistem** için belirttiğiniz değerleri kullanarak özel görüntü bulmak için kapsayıcı **Kaydet AzureRmVMImage** komutu.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Bir havuzu Portalı'nda özel bir görüntü oluşturun

Özel görüntünüzü kaydettikten sonra konumunu bilmeniz, bu görüntüden Batch havuzu oluşturabilirsiniz. Azure portalından bir havuzu oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında Batch hesabınıza gidin. Bu hesap, özel görüntü içeren depolama hesabıyla aynı abonelikte ve bölgede olması gerekir. 
2. İçinde **ayarları** penceresinde soldaki select **havuzları** menü öğesi.
3. İçinde **havuzları** penceresinde, seçin **Ekle** komutu.
4. Üzerinde **havuzu Ekle** penceresinde, seçin **özel görüntü (Linux/Windows)** gelen **görüntü türü** açılır. Portalda **Özel Görüntü** seçici görüntülenir. Özel görüntünüzü bulunduğu depolama hesabınıza gidin ve istenen VHD kapsayıcıyı seçin. 
5. Doğru seçin **teklif/yayımcı/Sku** özel VHD için.
6. Gerekli ayarları dahil olmak üzere, kalan belirtin **düğüm boyutu**, **ayrılmış düğümleri hedef**, ve **düşük öncelik düğümleri**, yanı sıra tüm isteğe bağlı ayarlar istenen.

    Örneğin, bir Microsoft Windows Server Datacenter 2016 özel görüntü **havuzu Ekle** penceresi, aşağıda gösterildiği gibi görünür:

    ![Özel Windows görüntüsünü havuzu ekleme](media/batch-custom-images/add-pool-custom-image.png)
  
Var olan bir havuzu özel bir görüntüde dayalı olup olmadığını denetlemek için bkz: **işletim sistemi** kaynak Özet bölümündeki özellik **havuzu** penceresi. Havuzu özel bir görüntüsünden oluşturulduysa ayarlanır **özel VM görüntüsü**.

Bir havuzuyla ilişkili tüm özel VHD'leri havuzun üzerinde görüntülenen **özellikleri** penceresi.
 
## <a name="next-steps"></a>Sonraki adımlar

- Toplu ayrıntılı bir bakış için bkz: [geliştirme büyük ölçekli paralel işlem çözümleri yığın](batch-api-basics.md).
- Batch hesabı oluşturma hakkında daha fazla bilgi için bkz: [Azure portalıyla Batch hesabı oluşturma](batch-account-create-portal.md).