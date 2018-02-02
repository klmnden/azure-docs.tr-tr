---
title: "Azure Batch havuzları özel görüntülerden sağlama | Microsoft Docs"
description: "Özel görüntü havuzundan sağlamak için işlem yazılım ve uygulamanız için gereksinim duyduğunuz verileri içeren düğümlerini toplu oluşturabilirsiniz. Özel resimler toplu iş yüklerini çalıştırmak için işlem düğümleri yapılandırmak için etkili bir yoludur."
services: batch
author: dlepow
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 10/11/2017
ms.author: danlep
ms.openlocfilehash: 63a567e9fdfef8dfceb275953cc0ac606355ea30
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="use-a-managed-custom-image-to-create-a-pool-of-virtual-machines"></a>Sanal makinelerin bir havuz oluşturmak için yönetilen özel görüntü kullanın 

Sanal Makine Yapılandırması'nı kullanarak bir Azure Batch havuzu oluşturduğunuzda, havuzdaki her işlem düğümü için işletim sistemi sağlayan bir VM görüntüsü belirtin. Azure Market görüntüsü olan ya da özel bir görüntü (oluşturulur ve kendiniz yapılandırılmış bir VM görüntüsü) ile sanal makinelerin bir havuz oluşturabilirsiniz. Özel görüntü olmalıdır bir *yönetilen resim* aynı Azure abonelik ve toplu işlem hesabı bölgeye kaynak.

## <a name="why-use-a-custom-image"></a>Özel görüntü neden kullanılır?
Özel görüntü sağladığınızda, işletim sistemi yapılandırması ve işletim sistemi ve veri diskleri kullanılacak türünde denetime sahip olursunuz. Özel görüntünüzü uygulamaları ve bunların sağlanan hemen sonra tüm Batch havuzu düğümlerinde kullanılabilir duruma başvuru verileri içerebilir.

Özel görüntü kullanarak, toplu iş yükünü çalıştırmak için havuzun işlem düğümleri hazırlamak için zaman kazandırır. Bir Azure Market görüntüsünü kullanabilir ve sağladıktan sonra her işlem düğümünde yazılım yükleme sırasında özel görüntü kullanarak daha etkili olabilir.

Senaryonuz için yapılandırılan özel görüntü kullanarak çeşitli avantajları sağlar:

- **İşletim sistemi (OS) yapılandırma**. Özel görüntü işletim sistemi disk üzerinde işletim sisteminin özel bir yapılandırma gerçekleştirebilir. 
- **Yükleme öncesi uygulamalar.** Özel bir görüntü daha etkili ve daha az işletim sistemi diski üzerinde önceden yüklenmiş uygulamalar ile hataya yatkın StartTask kullanarak işlem düğümleri sağladıktan sonra uygulamaları yükleme Daha oluşturabilirsiniz.
- **Yeniden başlatma zamanı Vm'lerinde kaydedin.** Uygulama yüklemesi, genellikle zaman alıcı olan VM yeniden başlatma gerektirir. Uygulamaları yüklenerek yeniden başlatma zaman kazanabilirsiniz. 
- **Çok büyük miktarlarda verinin bir kez kopyalayın.** Yönetilen özel görüntü statik veri parçası için yönetilen bir görüntünün veri diskleri kopyalayarak yapabilirsiniz. Bu yalnızca bir kez yapılması gerektiğini ve veri havuzu her düğüm için kullanılabilir hale getirir.
- **Disk türleri seçeneği.** Bu diskleri veya yapılandırdığınız kendi Linux veya Windows yükleme anlık bir Azure VM yönetilen bir diskten bir VHD'den yönetilen özel bir görüntü oluşturabilirsiniz. İşletim sistemi diski ve veri diski için premium depolama kullanma seçeneğiniz vardır.
- **Havuzları herhangi bir boyuta artar.** Bir havuzu oluşturmak için yönetilen özel görüntü kullandığınızda, havuz, istek herhangi bir boyuta büyüyebilir. Görüntü blob'u VM'ler uyum sağlayacak şekilde VHD'ler kopyalarını gerekmez. 


## <a name="prerequisites"></a>Önkoşullar

- **Yönetilen görüntü kaynağı**. Özel görüntü kullanarak sanal makineleri havuzu oluşturmak için aynı Azure abonelik ve toplu işlem hesabı bölgeye yönetilen görüntü kaynağı oluşturmanız gerekir. Yönetilen bir görüntüsünü hazırlamak seçenekleri için aşağıdaki bölüme bakın.
- **Azure Active Directory (AAD) kimlik doğrulama**. Toplu İstemcisi API AAD kimlik doğrulaması kullanmanız gerekir. AAD için Azure Batch destek bölümlerinde [Active Directory ile kimlik doğrulaması toplu hizmet çözümlerine](batch-aad-auth.md).

    
## <a name="prepare-a-custom-image"></a>Özel görüntü hazırlama
Yönetilen resim VHD yönetilen diskleri olan bir Azure VM veya VM anlık görüntü hazırlayabilirsiniz. 

Görüntünüzü hazırlık yaparken, aşağıdaki noktaları göz önünde bulundurun:

* Batch havuzlarını sağlamak için kullandığınız temel işletim sistemi görüntüsünün, özel betik uzantıları gibi önceden yüklenmiş Azure uzantılarına sahip olmadığından emin olun. Görüntü önceden yüklenmiş bir uzantı içeriyorsa Azure, VM dağıtımı sırasında sorunla karşılaşabilir.
* Sağladığınız temel işletim sistemi görüntüsü varsayılan geçici sürücü kullandığından emin olun. Toplu işlem düğüm Aracısı şu anda varsayılan geçici sürücü bekliyor.
* Bir Batch havuzu tarafından başvurulan yönetilen resim kaynak havuzu ömrü boyunca silinemiyor. Yönetilen görüntü kaynağı silinirse, sonra havuzu herhangi başka kuramaz. 

### <a name="to-create-a-managed-image"></a>Yönetilen bir görüntü oluşturmak için
Tüm mevcut hazırlanan Windows veya Linux işletim sistemi diski, yönetilen bir görüntü oluşturmak için kullanabilirsiniz. Yerel görüntü kullanmak isterseniz, örneğin, ardından yerel disk aynı abonelikte ve bölgede AzCopy veya başka bir karşıya yükleme aracını kullanarak Batch hesabınıza olan bir Azure depolama hesabı için karşıya yükleyin. Yönergeler için bir VHD yüklemek ve yönetilen bir görüntü oluşturmak ayrıntılı adımlar için bkz: [Windows](../virtual-machines/windows/upload-generalized-managed.md) veya [Linux](../virtual-machines/linux/upload-vhd.md) VM'ler.

Ayrıca bir yeni veya var olan Azure VM veya VM anlık görüntü yönetilen görüntüye hazırlayabilirsiniz. 

* Yeni bir VM oluşturuyorsanız, bir Azure Market görüntüsü için yönetilen görüntünüzü temel görüntü kullanın ve sonra özelleştirebilirsiniz. 

* Portalı kullanarak görüntü yakalamak planlıyorsanız, VM ile yönetilen bir disk oluşturulduğundan emin olun. Bir VM oluşturduğunuzda, varsayılan depolama ayar budur.

* VM çalışmaya başladıktan sonra hatta (Windows için) RDP veya SSH (için Linux) aracılığıyla bağlayın. Gerekli yazılımları yüklemek veya istenen verileri kopyalayın ve VM genelleştirin.  

Bir Azure VM generalize ve yönetilen bir görüntü oluşturmak adımlar için yönergeler için bkz: [Windows](../virtual-machines/windows/capture-image-resource.md) veya [Linux](../virtual-machines/linux/capture-image.md) VM'ler.

Görüntüyü bir Batch havuzu oluşturma planınızı nasıl bağlı olarak, aşağıdaki tanımlayıcı için resim gerekir:

* Bir havuz Batch API'lerini kullanarak görüntüsüyle oluşturmayı planlıyorsanız, **kaynak kimliği** biçimidir görüntünün `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage`. 
* Portal kullanmayı planlıyorsanız, **adı** görüntünün. 





## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Bir havuzu Portalı'nda özel bir görüntü oluşturun

Özel görüntünüzü kaydettiğiniz ve kaynak kimliği veya adı bildiğiniz sonra bu görüntüden Batch havuzu oluşturabilirsiniz. Aşağıdaki adımlar, Azure portalından bir havuzu oluşturmak nasıl gösterir.

> [!NOTE]
> Batch API'leri birini kullanarak havuzu oluşturuyorsanız, AAD kimlik doğrulaması için kimlik görüntü kaynağı için izinlere sahip olduğundan emin olun. Bkz: [Active Directory ile kimlik doğrulaması toplu hizmet çözümlerine](batch-aad-auth.md).
>

1. Azure portalında Batch hesabınıza gidin. Bu hesabı aynı abonelikte ve bölgede özel görüntü içeren kaynak grubunu şu şekilde olması gerekir. 
2. İçinde **ayarları** penceresinde soldaki select **havuzları** menü öğesi.
3. İçinde **havuzları** penceresinde, seçin **Ekle** komutu.
4. Üzerinde **havuzu Ekle** penceresinde, seçin **özel görüntü (Linux/Windows)** gelen **görüntü türü** açılır. Gelen **özel VM görüntüsü** açılan listesinde, görüntü adı (kaynak kimliği kısa biçimi) seçin.
5. Doğru seçin **teklif/yayımcı/Sku** özel görüntünüzü için.
6. Gerekli ayarları dahil olmak üzere, kalan belirtin **düğüm boyutu**, **ayrılmış düğümleri hedef**, ve **düşük öncelik düğümleri**, yanı sıra tüm isteğe bağlı ayarlar istenen.

    Örneğin, bir Microsoft Windows Server Datacenter 2016 özel görüntü **havuzu Ekle** penceresi, aşağıda gösterildiği gibi görünür:

    ![Özel Windows görüntüsünü havuzu ekleme](media/batch-custom-images/add-pool-custom-image.png)
  
Var olan bir havuzu özel bir görüntüde dayalı olup olmadığını denetlemek için bkz: **işletim sistemi** kaynak Özet bölümündeki özellik **havuzu** penceresi. Havuzu özel bir görüntüsünden oluşturulduysa ayarlanır **özel VM görüntüsü**.

Bir havuzuyla ilişkili tüm özel resimler havuzun üzerinde görüntülenen **özellikleri** penceresi.
 
## <a name="next-steps"></a>Sonraki adımlar

- Toplu ayrıntılı bir bakış için bkz: [geliştirme büyük ölçekli paralel işlem çözümleri yığın](batch-api-basics.md).
