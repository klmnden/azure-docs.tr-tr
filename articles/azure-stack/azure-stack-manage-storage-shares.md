---
title: Azure yığın depolama kapasitesi yönetme | Microsoft Docs
description: İzleme ve kullanılabilir depolama alanı için Azure yığınına yönetme.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: b0e694e4-3575-424c-afda-7d48c2025a62
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: da6bb00d7538c1a26e1ed4be29d3c882aa378e9e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
ms.locfileid: "34077420"
---
# <a name="manage-storage-capacity-for-azure-stack"></a>Azure yığın depolama kapasitesi yönetme

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bu makaledeki bilgiler Azure yığın bulut işleci İzleyicisi yardımcı olur ve bunların Azure yığın dağıtımına depolama kapasitesini yönetin. Bir alt kümesi için kullanılacak Azure yığın dağıtımına toplam depolama kapasitesini Azure yığın depolama altyapısını ayırır **depolama hizmetleri**. Depolama Hizmetleri dağıtımı düğümlerine karşılık gelen birimleri paylaşımlarında kiracının veri depolayın.

Bulut operatörü çalışmak için depolama sınırlı miktarda sahip. Depolama alanı miktarını uygulamanız çözümü tarafından tanımlanır. Çözümünüzü bir çok düğümlü çözümünü kullandığınızda OEM satıcınıza veya Azure yığın Geliştirme Seti yüklediğiniz donanım tarafından sağlanır.

Azure yığın depolama kapasitesi genişletme desteklemediğinden önemlidir [İzleyici](#monitor-shares) verimli işlemleri emin olmak için kullanılabilir depolama alanı korunur.  

Bir paylaşım kalan boş kapasitesi sınırlı hale geldiğinde planladığınız [alanını yönetmek](#manage-available-space) paylaşımları kapasite tükenmesini engellemek için.

Kapasite yönetmek için Seçenekler şunlardır:
- Kapasiteyi geri kazanmak
- Bir kapsayıcı geçirme

Bir paylaşım kullanılan, % 100 depolama olduğunda, artık bu paylaşım için işlevleri hizmet. Geri yükleme işlemleri paylaşım için Yardım almak için Microsoft Destek'e başvurun.

## <a name="understand-volumes-and-shares-containers-and-disks"></a>Birimler ve paylaşımlar, kapsayıcıları ve diskleri anlama
### <a name="volumes-and-shares"></a>Birim ve paylaşımların
*Depolama birimi hizmeti* kullanılabilir depolama alanı Kiracı verileri tutmak için ayrılan ayrı ve eşit birime bölümler. Birimlerin sayısı, Azure yığın dağıtımda düğüm sayısını eşittir:

- Bir dört düğümlü dağıtım dört birim yok. Her birim tek bir paylaşımı vardır. Kaldırılan veya düzgün çalışmayan bir düğüm ise, bir çok düğümlü dağıtım paylaşımları sayısı daha azdır değil.
- Azure yığın Geliştirme Seti kullanırsanız, tek bir birim üzerinde tek bir paylaşım yok.

Depolama hizmet paylaşımları depolama hizmetleri özel kullanım için olduğundan, doğrudan değiştirmek, Ekle veya herhangi bir dosya paylaşımlarında kaldırmanız gerekir. Yalnızca depolama hizmetleri bu birimlerde depolanan dosyalar üzerinde çalışması gerekir.

Birimleri paylaşımlarında Kiracı verilerini tutun. Kiracı veri sayfa BLOB'ları içeren blok ekleme BLOB'ları, tabloları, kuyrukları, veritabanları ve ilgili meta verileri depolar. Depolama nesneleri (BLOB'lar vb.) ayrı ayrı içinde tek bir paylaşım bulunduğundan, her nesnenin en büyük boyutu bir paylaşımı boyutunu aşamaz. Yeni nesneler en büyük boyutu bu yeni bir nesne oluşturulduğunda, bu bir paylaşım kullanılmayan alanı kalan kapasite bağlıdır.

Bir paylaşım olduğunda düşük boş alan ve eylemlere [geri](#reclaim-capacity) alanı başarılı veya kullanılabilir değil, Azure yığın bulut operatörü olabilir [geçirmek](#migrate-a-container-between) bir paylaşım başka bir blob kapsayıcıları.

- Kapsayıcılar ve bloblar hakkında daha fazla bilgi için bkz: [Blob storage](azure-stack-key-features.md#blob-storage) anahtar özellikler ve Azure yığınında kavramları.
- Kiracı kullanıcılar Azure yığınında blob storage ile nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure yığın depolama hizmetleri](/azure/azure-stack/user/azure-stack-storage-overview#azure-stack-storage-services).


### <a name="containers"></a>Kapsayıcılar
Kiracı kullanıcılar sonra blob verilerini depolamak için kullanılan kapsayıcı oluşturun. BLOB'ları yerleştirmek için hangi kapsayıcısında kullanıcı karar olsa da, depolama birimi hizmeti bir algoritma kapsayıcı koymak için hangi birimde belirlemek için kullanır. Algoritma, genellikle en fazla kullanılabilir alan birimi seçer.  

Bir blob bir kapsayıcıda yerleştirilir sonra o blob fazla alan kullanması için büyüyebilir. Yeni BLOB'ları ve varolan ekledikçe BLOB'lar büyüme, bu kapsayıcı küçültür tutan birim kullanılabilir alanı.  

Kapsayıcıları için tek bir paylaşım sınırlı değildir. Bir kapsayıcıda birleşik blob veri kullanımı % 80 veya daha fazla kullanılabilir alan büyürken kapsayıcı girer *taşma* modu. Taşma modundayken, bu kapsayıcıda oluşturulan yeni BLOB yeterli alana sahip farklı bir birime ayrılır. Zaman içinde bir kapsayıcı taşma modunda birden çok birimlere dağıtılır BLOB'ları olabilir.

% 80 ve birim kullanılabilir alanı % 90'ını kullanıldığında sistem Azure yığın Yönetici portalı'nda uyarıları başlatır. Bulut operatörleri kullanılabilir depolama kapasitesinin gözden geçirin ve içeriği yeniden dengelemeniz planlamanız gerekir. Depolama Birimi hizmeti, % 100 kullanılan bir diski olduğundan ve hiçbir ek uyarı çalışmayı durdurur.

### <a name="disks"></a>Diskler
VM diskleri kapsayıcılar için kiracılar tarafından eklenir ve bir işletim sistemi diski içerir. VM'ler, bir veya daha fazla veri diski olarak da sağlayabilirsiniz. Her iki tür diskleri sayfa blobları depolanır. Her disk VM performansını artırmak için ayrı bir kapsayıcıya yerleştirmek için kiracılar için yönergeler verilmiştir.
- VM bir diskten (sayfa blob) tutan her kapsayıcı diskini VM'ye ekli bir kapsayıcı olarak kabul edilir.
- Herhangi bir VM diskten tutmaz bir kapsayıcı boş bir kapsayıcı olarak kabul edilir.

Ekli kapsayıcısı üzerinde alan boşaltmak için seçenekleri [sınırlıdır](#move-vm-disks).
> [!TIP]  
> Bulut operatörleri kiracılar için bir kapsayıcı ekleme olasılığınız sanal makineler (VM'ler) bağlı olan diskler doğrudan yönetmez. Ancak, depolama paylaşımlarındaki alanını yönetmek planlama yaparken, diskleri kapsayıcıları ve paylaşımları nasıl ilişkili olduğunu anlamak için kullanım olabilir.

## <a name="monitor-shares"></a>İzleyici paylaşımları
Boş alan sınırlı olduğunda anlamaları için paylaşımları izlemek için PowerShell veya Yönetici portalı'nı kullanın. Portal kullanırken, alan düşük paylaşımları hakkında uyarı almak.    

### <a name="use-powershell"></a>PowerShell kullanma
Bulut operatörü PowerShell kullanarak bir paylaşım depolama kapasitesini izleyebilirsiniz **Get-AzsStorageShare** cmdlet'i. Get-AzsStorageShare cmdlet toplam, ayrılmış ve boş alan bayt cinsinden her paylaşımları döndürür.   
![Örnek: paylaşımları için boş alan Döndür](media/azure-stack-manage-storage-shares/free-space.png)

- **Toplam Kapasite** paylaşımında kullanılabilir bayt cinsinden toplam alanı. Bu alan, veri ve depolama hizmetleri tarafından korunur meta veriler için kullanılır.
- **Kapasite kullanılan** tüm kapsamları Kiracı veri ve ilişkili meta verileri depolayan dosyalarından tarafından kullanılan veri bayt miktarı.

### <a name="use-the-administrator-portal"></a>Yönetici portalı'nı kullanın
Bulut operatörü olarak, tüm paylaşımlar depolama kapasitesini görüntülemek için Yönetim Portalı'nı kullanabilirsiniz. **Depolama gidin** > **dosya paylaşımları** kullanım bilgileri görüntüleyebileceğiniz dosya paylaşımı listesini açın.
![Örnek: Depolama dosyası paylaşımları](media/azure-stack-manage-storage-shares/storage-file-shares.png)
- **Toplam** paylaşımında kullanılabilir bayt cinsinden toplam alanı. Bu alan, veri ve depolama hizmetleri tarafından korunur meta veriler için kullanılır.
- **KULLANILAN** tüm kapsamları Kiracı veri ve ilişkili meta verileri depolayan dosyalarından tarafından kullanılan veri bayt miktarı.

### <a name="storage-space-alerts"></a>Depolama alanı uyarıları
Yönetim Portalı'nı kullandığınızda, alan düşük paylaşımları hakkında uyarı almak.

> [!IMPORTANT]
> Bulut operatörü, tam kullanım ulaşmasını paylaşımları tutun. Bir paylaşım kullanılan, % 100 depolama olduğunda, artık bu paylaşım için işlevleri hizmet. Boş alan kurtarmak ve kullanılan % 100 olan bir paylaşımı üzerinde işlemler geri yüklemek için Microsoft Destek'e başvurmanız gerekir.

**Uyarı**: bir dosya paylaşımı üzerinde kullanılan % 80 olduğunda, aldığınız bir *uyarı* yönetim portalında Uyarı: ![örnek: uyarı](media/azure-stack-manage-storage-shares/alert-warning.png)


**Kritik**: bir dosya paylaşımı üzerinde kullanılan % 90 olduğunda, aldığınız bir *kritik* yönetim portalında Uyarı: ![örnek: Kritik Uyarı](media/azure-stack-manage-storage-shares/alert-critical.png)

**Ayrıntıları Görüntüle**: Yönetim Portalı'nda azaltma seçenekleri görüntülemek için bir uyarı ayrıntılarını açabilirsiniz: ![örnek: Uyarı ayrıntılarını görüntüleyin](media/azure-stack-manage-storage-shares/alert-details.png)


## <a name="manage-available-space"></a>Kullanılabilir alan yönetme
Boş alan bir paylaşım için gerekli olduğunda az bozucu yöntemleri ilk kullanın. Örneğin, bir kapsayıcı geçirmek seçmeden önce alanı geri almasına olanak deneyin.  

### <a name="reclaim-capacity"></a>Kapasiteyi geri kazanmak
*Bu seçenek, hem çok düğümlü dağıtımlar hem de Azure yığın Geliştirme Seti için geçerlidir.*

Silinmiş Kiracı hesapları tarafından kullanılan kapasite geri kazanabilirsiniz. Bu kapasite otomatik olarak ne zaman iadesi veri [saklama dönemi](azure-stack-manage-storage-accounts.md#set-the-retention-period) ulaşıldığında veya hemen geri kazanmak için davranamaz.

Daha fazla bilgi için bkz: [geri kapasite](azure-stack-manage-storage-accounts.md#reclaim) Yönet depolama kaynakları.

### <a name="migrate-a-container-between-volumes"></a>Bir kapsayıcı birimler arasında geçiş
*Bu seçenek yalnızca çok düğümlü dağıtımlar için geçerlidir.*

Kiracı kullanım desenlerini nedeniyle bazı Kiracı paylaşımları diğerlerinden daha fazla alan kullanın. Sonuç alanı görece kullanılmayan diğer paylaşımları önce düşük çalışır bir paylaşımı olabilir.

Bazı blob kapsayıcıları için farklı bir paylaşım el ile geçiş yaparak bir aşırı kullanılmasına paylaşımında alan boşaltmak deneyebilirsiniz. Çok küçük kapsayıcı tümünü tutmak için kapasiteye sahip tek bir paylaşım geçirebilirsiniz. Taşımak için geçiş kullanabilirsiniz *ücretsiz* kapsayıcıları. Boş kapsayıcıları bir disk için bir VM içermeyen kapsayıcılardır.   

Geçiş yeni paylaşımındaki tüm kapsayıcıları blob birleştirir.

- Bir kapsayıcıyı taşma moduna girdi ve BLOB'lar ek birimlerde yerleştirdiğini, yeni paylaşım tüm geçiş kapsayıcısı için BLOB tutmak için yeterli kapasitesi olması gerekir. Bu ek paylaşımlarında bulunan BLOB'ları içerir.

- PowerShell cmdlet *Get-AzsStorageContainer* yalnızca kullanımda olan ilk birim kapsayıcı için alan tanımlar. Cmdlet ek birimlerde put BLOB'ları tarafından kullanılan alanı tanımlamaz. Bu nedenle, bir kapsayıcı tam boyutunun güvenli olmayabilir. Bir kapsayıcıda yeni bir paylaşım birleştirilmesi yeni paylaşan nerede ek paylaşımlar üzerine veri yerleştirir bir taşma koşuluna gönderebilirsiniz mümkündür. Sonuç olarak, yeniden paylaşımlarına yeniden dengelemeniz gerekebilir.

- Bir kaynak grubu için izinler yetersiz ve PowerShell taşma veriler için ek birimler sorgulamak için kullanamaz, bu kaynak gruplarını ve kapsayıcıları verilerin toplam boyutu anlamak için bu verileri geçirmeden önce geçirmek için sahibi ile çalışır.  

> [!IMPORTANT]
> Geçiş BLOB kapsayıcı için PowerShell kullanılmasını gerektiren çevrimdışı bir işlemdir. Geçiş tamamlanana kadar geçirdiğiniz kapsayıcısı için tüm BLOB'lar çevrimdışı kalır ve kullanılamaz. Ayrıca, tüm devam eden Geçiş tamamlanana kadar Azure yığını yükseltmeden kaçınmalısınız.

#### <a name="to-migrate-containers-using-powershell"></a>PowerShell kullanarak kapsayıcıları geçirmek için
1. Sahip olduğunuzu onaylamak [Azure PowerShell yüklenmiş ve yapılandırılmış](http://azure.microsoft.com/documentation/articles/powershell-install-configure/). Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](http://go.microsoft.com/fwlink/?LinkId=394767).
2.  Hangi verilerin, geçirmeyi planladığınız paylaşımında olduğunu anlamak için kapsayıcı inceleyin. Bir birim içinde geçiş için en iyi adayı kapsayıcıları tanımlamak için kullanmak **Get-AzsStorageContainer** cmdlet:

    ````PowerShell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    $shares = Get-AzsStorageShare -FarmName $farm_name
    $containers = Get-AzsStorageContainer -ShareName $shares[0].ShareName -FarmName $farm_name
    ````
    Ardından $containers inceleyin:

    ````PowerShell
    $containers
    ````

    ![Örnek: $Containers](media/azure-stack-manage-storage-shares/containers.png)

3.  Geçiş kapsayıcı tutmak için en iyi hedef paylaşımları tanımlayın:

    ````PowerShell
    $destinationshares = Get-AzsStorageShare -SourceShareName
    $shares[0].ShareName -Intent ContainerMigration
    ````

    Ardından $destinationshares inceleyin:

    '''PowerShell $destinationshares
    ````

    ![Example: $destination shares](media/azure-stack-manage-storage-shares/examine-destinationshares.png)

4. Start migration for a container. Migration is asynchronous. If you start migration of additional containers before the first migration completes, use the job id to track the status of each.

  ````PowerShell
  $job_id = Start-AzsStorageContainerMigration -StorageAccountName $containers[0].Accountname -ContainerName $containers[0].Containername -ShareName $containers[0].Sharename -DestinationShareUncPath $destinationshares[0].UncPath -FarmName $farm_name
  ````

  Ardından $jobId inceleyin. Aşağıdaki örnekte *d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0* incelemek istediğiniz iş kimliği:

  ````PowerShell
  $jobId
  d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0
  ````

5. İş kimliği, geçiş işinin durumunu denetlemek için kullanın. Kapsayıcı geçiş tamamlandığında, **MigrationStatus** ayarlanır **tam**.

  ````PowerShell 
  Get-AzsStorageContainerMigrationStatus -JobId $job_id -FarmName $farm_name
  ````

  ![Örnek: Geçiş durumu](media/azure-stack-manage-storage-shares/migration-status1.png)

6.  Devam eden geçiş işi iptal edebilirsiniz. Geçiş işleri zaman uyumsuz olarak işlenir iptal edildi. İptal $jobid kullanarak izleyebilirsiniz:

  ````PowerShell
  Stop-AzsStorageContainerMigration -JobId $job_id -FarmName $farm_name
  ````

  ![Örnek: Geri alma durumu](media/azure-stack-manage-storage-shares/rollback.png)

7. Geçiş işi durumu onaylayıncaya kadar komutu 6. adımda yeniden çalıştırabilirsiniz **iptal edildi**:  

    ![Örnek: İptal edildi durumu](media/azure-stack-manage-storage-shares/cancelled.png)

### <a name="move-vm-disks"></a>VM diskleri taşıma
*Bu seçenek yalnızca çok düğümlü dağıtımlar için geçerlidir.*

Alanını yönetmek üzere en aşırı yöntemi, sanal makine disklerini taşıma içerir. Ekli bir kapsayıcı (bir VM disk içerir) taşıma karmaşık olduğu için bu eylemi gerçekleştirmek için Microsoft Support başvurun.

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi edinmek [kullanıcılara sanal makinelere sunumu](azure-stack-tutorial-tenant-vm.md).
