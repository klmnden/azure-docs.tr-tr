---
title: Azure Batch havuzu özel bir görüntüden sağlama | Microsoft Docs
description: Bir toplu işlem yazılım ve uygulamanız için gereken verileri içeren düğümleri sağlamak için özel görüntüden havuz oluşturun. Özel görüntüler, işlem düğümleri Batch iş yüklerinizi çalıştırmak için yapılandırmak için etkili bir yoludur.
services: batch
author: laurenhughes
manager: jeconnoc
ms.service: batch
ms.topic: article
ms.date: 04/15/2019
ms.author: lahugh
ms.openlocfilehash: 233b26b330fabe7da8664114ba1857f74feea4bc
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63764269"
---
# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Sanal makine havuzu oluşturmak için özel görüntü kullanma 

Sanal Makine Yapılandırması'nı kullanarak bir Azure Batch havuzu oluşturduğunuzda, havuzdaki her işlem düğümü işletim sistemi sağlayan bir VM görüntüsü belirtin. Desteklenen Azure Market görüntüsü ile veya özel bir görüntü (oluşturulur ve kendiniz yapılandırılmış bir VM görüntüsü) ile sanal makine havuzu oluşturabilirsiniz. Özel görüntü olmalıdır bir *yönetilen bir görüntü* aynı Azure aboneliği ve Batch hesabı bölgede kaynak.

## <a name="why-use-a-custom-image"></a>Özel bir görüntü neden kullanmalısınız?

Özel bir görüntü sağladığınızda, işletim sistemi yapılandırması ve işletim sistemi ve veri diskleri kullanılacak türü üzerinde denetime sahiptir. Özel görüntü, uygulamaları ve bunların sağlanan hemen sonra tüm Batch havuzu düğümlerinde kullanılabilir hale başvuru verileri içerebilir.

Özel bir görüntü kullanarak, Batch iş yükü çalıştırmak için havuzun işlem düğümleri hazırlamak için zaman kazandırır. Azure Market görüntüsü kullanmak ve sağladıktan sonra her işlem düğümünde yazılım yükleme sırasında özel bir görüntü kullanarak daha verimli olabilir.

Senaryonuz için yapılandırılmış özel bir görüntü kullanarak çeşitli avantajlar sağlayabilir:

- **İşletim sistemi (OS) yapılandırma**. Görüntünün işletim sistemi diskinin yapılandırmasını özelleştirebilirsiniz. 
- **Yükleme öncesi uygulamalar.** Uygulamaların daha verimli ve daha az hata yapmaya açık bir başlangıç görevi kullanarak işlem düğümleri sağladıktan sonra uygulama yüklemenin daha işletim sistemi diskini üzerinde önceden yükleyin.
- **Yeniden başlatma zamanı Vm'lerde kaydedin.** Uygulama yüklemesi, genellikle zaman alıcı olan VM yeniden başlatma gerektirir. Uygulamaları yüklenerek yeniden başlatma zamandan tasarruf edebilirsiniz. 
- **Çok büyük miktarlarda verinin bir kez kopyalayın.** Yönetilen bir özel görüntü statik veri parçası için yönetilen bir görüntünün veri diskleri kopyalayarak olun. Bu yalnızca bir kez gerçekleştirilmesi gerekir ve veri havuzu her düğüm için kullanılabilir hale getirir.
- **Disk türleri seçimi.** İşletim sistemi diski ve veri diski için premium depolama kullanmayı seçebilirsiniz.
- **Havuzlar, büyük ölçüde artar.** Bir havuz oluşturmak için yönetilen bir özel görüntü kullandığınızda, havuz görüntü blob'u VHD'ler kopyalarını gerek kalmadan büyüyebilir. 


## <a name="prerequisites"></a>Önkoşullar

- **Yönetilen bir görüntü kaynağı**. Sanal makinelerin özel bir görüntü kullanarak havuz oluşturma için ya da aynı Azure aboneliği ve Batch hesabı bölgede bir yönetilen bir görüntü kaynağı oluşturmanız gerekir. Sanal makinenin işletim sistemi diski ve isteğe bağlı olarak, bağlı veri diskleri anlık görüntülerden görüntü oluşturulmalıdır. Daha fazla bilgi ve yönetilen bir görüntü hazırlamak için adımları için aşağıdaki bölüme bakın. 
  - Oluşturduğunuz her havuzu için benzersiz bir özel görüntü kullanın.
  - Batch API'lerini kullanarak görüntü ile havuz oluşturma için belirtin **kaynak kimliği** biçimindedir görüntünün `/subscriptions/xxxx-xxxxxx-xxxxx-xxxxxx/resourceGroups/myResourceGroup/providers/Microsoft.Compute/images/myImage`. Portalı kullanmak için **adı** görüntüsü.  
  - Yönetilen bir görüntü kaynağı ölçek büyütme izin vermek için havuz ömrü boyunca var olmalıdır ve havuz silindikten sonra kaldırılabilir.

- **Azure Active Directory (AAD) kimlik doğrulaması**. Batch istemci API'SİNİN AAD kimlik doğrulaması kullanmanız gerekir. AAD için Azure Batch desteği belirtilmiştir [Batch hizmeti çözümlerinin kimliğini Active Directory ile](batch-aad-auth.md).

## <a name="prepare-a-custom-image"></a>Özel bir görüntü hazırlama

Azure'da bir Azure VM'nin işletim sistemi ve veri disklerinin anlık görüntüleri, genelleştirilmiş Azure VM'yi yönetilen disklere sahip veya yüklediğiniz şirket genelleştirilmiş VHD'den yönetilen bir görüntü hazırlayabilirsiniz. Batch havuzları özel bir görüntü ile güvenilir bir şekilde ölçeklendirmek için yönetilen bir görüntü kullanarak bir oluşturmanızı öneririz *yalnızca* ilk yöntem: sanal makinenin disk anlık görüntülerini kullanarak. Bir VM'yi hazırlama, bir anlık görüntüsünü alın ve anlık görüntüden bir görüntü oluşturmak için aşağıdaki adımlara bakın. 

### <a name="prepare-a-vm"></a>Bir VM hazırlama

Görüntü için yeni bir VM oluşturuyorsanız, Batch tarafından yönetilen bir görüntü için temel görüntü olarak desteklenen bir birinci taraf Azure Market görüntüsü kullanın. Yalnızca birinci taraf görüntülerini temel görüntü olarak kullanılabilir. Azure Batch tarafından desteklenen Azure Market görüntü başvuruları, tam bir listesini almak için bkz: [listesi düğüm Aracısı SKU'ları](/rest/api/batchservice/account/listnodeagentskus) işlemi.

> [!NOTE]
> Ek lisans ve satın alma koşulları, temel görüntü olarak içeren bir üçüncü taraf görüntüyü kullanamazsınız. Yönergeler için bu Market görüntüleri hakkında daha fazla bilgi için bkz [Linux](../virtual-machines/linux/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) veya [Windows](../virtual-machines/windows/cli-ps-findimage.md#deploy-an-image-with-marketplace-terms
) VM'ler.


* VM'ye yönetilen disk ile oluşturulduğundan emin olun. Bir VM oluşturduğunuzda, varsayılan depolama ayar budur.
* Sanal makinede özel betik uzantısı gibi Azure uzantıları yüklemeyin. Görüntü önceden yüklenmiş bir uzantı içeriyorsa Azure Batch havuzu dağıtırken sorunlarla karşılaşabilirsiniz.
* Sağladığınız temel işletim sistemi görüntüsünün varsayılan geçici sürücüyü kullandığından emin olun. Batch düğüm Aracısı varsayılan geçici sürücüyü şu anda bekliyor.
* VM çalıştırıldıktan sonra ona (Windows için) RDP veya SSH (Linux için) aracılığıyla bağlanın. Gerekli yazılımları yüklemek veya istenen veri kopyalayın.  

### <a name="create-a-vm-snapshot"></a>VM anlık görüntüsü oluşturma

Anlık görüntü, bir VHD, tam ve salt okunur bir kopyasıdır. Bir sanal makinenin işletim sistemi veya veri disklerinin anlık görüntüsünü oluşturmak için Azure portalı veya komut satırı araçlarını kullanabilirsiniz. Yönergeler için bu adımları ve bir anlık görüntü oluşturmak için seçenekleri görmek için [Linux](../virtual-machines/linux/snapshot-copy-managed-disk.md) veya [Windows](../virtual-machines/windows/snapshot-copy-managed-disk.md) VM'ler.

### <a name="create-an-image-from-one-or-more-snapshots"></a>Bir veya daha fazla anlık görüntülerden görüntü oluşturma

Anlık görüntüden yönetilen bir görüntü oluşturmak için Azure komut satırı gibi araçları kullanın [az görüntü oluşturma](/cli/azure/image) komutu. Bir işletim sistemi disk anlık görüntü ve isteğe bağlı olarak bir veya daha fazla veri diski anlık görüntüleri belirterek bir görüntü oluşturabilirsiniz.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Portalda özel görüntüden havuz oluşturma

Özel görüntünüzü kaydettikten sonra kaynak Kimliğini veya adını biliyorsanız, bu görüntüden bir Batch havuzu oluşturun. Aşağıdaki adımlar Azure portalından bir havuz oluşturma işlemini gösterir.

> [!NOTE]
> Batch API'lerini kullanarak bir havuz oluşturuyorsanız, AAD kimlik doğrulaması için kullandığınız kimlik görüntü kaynağını izni olduğundan emin olun. Bkz: [Batch hizmeti çözümlerinin kimliğini Active Directory ile](batch-aad-auth.md).
>
> Yönetilen bir görüntü için kaynak havuzu ömrü boyunca mevcut olması gerekir. Temel alınan kaynak silinirse, havuz ölçeklendirilemiyor. 

1. Azure portalında Batch hesabınıza gidin. Bu hesap, özel görüntü içeren bir kaynak grubu aynı abonelik ve aynı bölgede olması gerekir. 
2. İçinde **ayarları** penceresi seçin sol taraftaki **havuzları** menü öğesi.
3. İçinde **havuzları** penceresinde **Ekle** komutu.
4. Üzerinde **havuzu Ekle** penceresinde **özel görüntü (Linux/Windows)** gelen **görüntü türü** açılır. Gelen **özel VM görüntüsü** açılır listesinde, görüntü adı (kaynak kimliği kısa biçimi) seçin.
5. Doğru seçin **yayımcı/teklif/Sku** için özel görüntü.
6. Gerekli ayarları dahil olmak üzere, kalan belirtin **düğüm boyutu**, **hedef adanmış düğümler**, ve **düşük öncelikli düğümler**, yanı sıra tüm istenen isteğe bağlı ayarlar.

    Örneğin, bir Microsoft Windows Server Datacenter 2016 özel görüntü **havuzu Ekle** penceresi, aşağıda gösterildiği gibi görünür:

    ![Windows özel görüntüden havuz ekleme](media/batch-custom-images/add-pool-custom-image.png)
  
Mevcut havuzlardan özel bir görüntü tabanlı olup olmadığını denetlemek için bkz. **işletim sistemi** özelliği kaynak Özet bölümünde **havuzu** penceresi. Özel görüntüden havuz oluşturulduysa ayarlanmış **özel VM görüntüsü**.

Bir havuzla ilişkilendirilmiş tüm özel görüntüleri havuzun görüntülenen **özellikleri** penceresi.

## <a name="considerations-for-large-pools"></a>Büyük havuzlar için dikkat edilmesi gerekenler

Vm'leri veya daha fazla özel bir görüntü kullanarak yüzlerce ile havuz oluşturmayı planlıyorsanız, VM anlık görüntüden oluşturulmuş bir görüntüyü kullanmak için yukarıdaki yönergeleri önemlidir.

Ayrıca aşağıdakilere dikkat edin:

- **Boyut sınırları** -özel bir görüntü kullanırken havuz boyutunu 2500 adanmış işlem düğümleri veya 1000 düşük öncelikli düğümler, Batch sınırlar.

  Aynı görüntü (veya birden çok görüntüsü üzerinde aynı temel alınan anlık görüntü tabanlı) kullanıyorsanız birden fazla havuz oluşturmak için toplam işlem düğümleri havuzları, önceki sınırlarını aşamaz. Birden çok tek bir havuz için bir görüntü veya kendi temel alınan anlık görüntü kullanımı önerilmemektedir.

  Havuz yapılandırırsanız sınırları düşebilir [gelen NAT havuzları](pool-endpoint-configuration.md).

- **Yeniden boyutlandırma zaman aşımı** : havuzunuz sabit içeriyorsa, 20-30 dakika gibi bir değere havuzunun resizeTimeout özelliği artırmaz düğüm sayısını otomatik ölçeklendirme. Havuzunuzun hedef boyutunu, zaman aşımı süresi içinde ulaşmaz, başka bir gerçekleştirmek [yeniden boyutlandırma işlemi](/rest/api/batchservice/pool/resize).

  300'den fazla işlem düğümlerinin bir havuzuyla planlıyorsanız, hedef boyutu ulaşmak için birden çok kez havuz yeniden boyutlandırma gerekebilir.

## <a name="considerations-for-using-packer"></a>Packer kullanma konuları

Doğrudan Packer ile yönetilen bir görüntü kaynak oluşturmaya yalnızca kullanıcı aboneliği modunda Batch hesapları ile yapılabilir. Batch hizmeti modu hesapları için bir VHD oluşturun ve sonra VHD için bir yönetilen bir görüntü kaynağı içeri aktarmak gerekir. Havuz ayırma moduna bağlı olarak (kullanıcı aboneliği ve Batch hizmeti), yönetilen bir görüntü kaynağı oluşturmak için adımları farklılık gösterir.

Yönetilen bir görüntü oluşturmak için kullanılan kaynak yaşam süreleri özel görüntüyü başvuran havuzunun var olduğundan emin olun. Bunun yapılmaması havuzu ayırma hataları neden ve/veya hataları yeniden boyutlandırın. 

Görüntüyü ya da temel alınan kaynak kaldırılırsa, hata benzer şekilde alabilirsiniz: `There was an error encountered while performing the last resize on the pool. Please try resizing the pool again. Code: AllocationFailed`. Bu durumda, temel alınan kaynak kaldırılmamıştır emin olun.

Bir VM oluşturmak için Packer kullanma hakkında daha fazla bilgi için bkz. [Packer ile bir Linux görüntüsü oluşturun](../virtual-machines/linux/build-image-with-packer.md) veya [Packer ile Windows görüntü derleme](../virtual-machines/windows/build-image-with-packer.md).

## <a name="next-steps"></a>Sonraki adımlar

- Batch ayrıntılı genel bakış için bkz: [büyük ölçekli paralel işlem çözümleri geliştirme batch'le](batch-api-basics.md).
