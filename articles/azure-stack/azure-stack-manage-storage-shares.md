---
title: Azure stack'teki depolama kapasitesi yönetme | Microsoft Docs
description: İzleme ve Azure Stack için Azure Stack depolama kapasitesi ve kullanılabilir depolama alanı yönetin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 03/29/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 03/19/2019
ms.openlocfilehash: e5188a7f7a1ce889c8f4340f100cfe767ff2dff8
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2019
ms.locfileid: "58629390"
---
# <a name="manage-storage-capacity-for-azure-stack"></a>Azure Stack için depolama kapasitesi yönetme 

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Bu makaledeki bilgiler, Azure Stack bulut işleci İzleyici yardımcı olur ve Azure Stack dağıtımı depolama kapasitesini yönetin. Azure Stack depolama altyapısının bir alt kümesi için kullanılacak Azure Stack dağıtımı, toplam depolama kapasitesinin ayırır **depolama hizmetleri**. Depolama Hizmetleri dağıtımı düğümlerine karşılık gelen birimlere paylaşımları bir kiracının verileri depolayın.

Bulut operatörü olarak, depolama ile çalışmak için sınırlı bir süre vardır. Depolama alanı miktarının uygulamanız çözüm tarafından tanımlanır. Çözümünüzü Azure Stack geliştirme Seti'ni yüklemek donanım veya bir çok düğümlü çözümü kullandığınızda, OEM satıcısı tarafından sağlanır.

Azure Stack depolama kapasitesini genişletme desteklemediğinden önemlidir [İzleyici](#monitor-shares) işlemlerin verimli için kullanılabilir depolamayı korunur.  

Bir paylaşımı kalan boş kapasitesi sınırlı olduğunda planladığınız [alanını yönetmek](#manage-available-space) paylaşımları kapasite dışında çalışmasını önlemek için.

Kapasite yönetmek için Seçenekler şunlardır:
- Kapasiteyi geri kazanmak
- Bir kapsayıcı geçirme

Bir paylaşımı kullanılan, % 100 depolama olduğunda bu paylaşım için işlevleri artık hizmeti. Geri yükleme işlemleri paylaşım için Yardım almak için Microsoft desteğine başvurun.

## <a name="understand-volumes-and-shares-containers-and-disks"></a>Birimler ve paylaşımlar, kapsayıcılar ve diskleri anlama
### <a name="volumes-and-shares"></a>Birim ve paylaşımların
*Depolama hizmeti* Kiracı verileri tutmak için ayrılan ayrı ve eşit birimlere kullanılabilir depolama alanına bölümler. Azure Stack dağıtım eşit sayıda düğüme birim sayısı:

- Bir dört düğümlü dağıtım dört birim yok. Her birimin tek bir paylaşım yok. Bir düğümü kaldırılan veya hatalı çalışan ise bir çok düğümlü dağıtımı paylaşım sayısı azaltılır değil.
- Azure Stack geliştirme Seti'ni kullanırsanız, tek bir paylaşım ile tek bir birim yok.

Depolama Hizmetleri özel kullanım için depolama hizmeti paylaşımları olduğundan, doğrudan değiştirme, ekleme veya paylaşımlarındaki tüm dosyaları kaldırmanız gerekir. Yalnızca depolama hizmetleri, bu birimleri'nde depolanan dosyalar üzerinde çalışması gerekir.

Paylaşımları birimlerde Kiracı verileri tutar. Kiracı verilerini sayfa BLOB'ları içeren, blok blobları, ekleme blobları, tabloları, kuyrukları, veritabanları ve ilgili meta verileri depolar. Depolama nesneleri (BLOB'lar, vb.) tek tek tek bir paylaşım içinde bulunduğundan, en büyük boyutunu her nesne bir paylaşımı boyutunu aşamaz. Yeni nesne oluşturulduğunda bir paylaşımı kullanılmayan kalır ve kapasite üzerinde yeni nesneler en büyük boyutunu bağlıdır.

Bir paylaşımı olduğunda düşük boş alan ve eylemlere [geri kazan](#reclaim-capacity) alanı başarılı ya da mevcut değilse, Azure Stack bulut operatörü blob kapsayıcıları bir paylaşımından diğerine geçirebilirsiniz.

- Kiracı kullanıcılar'blob depolama alanında Azure Stack ile nasıl çalıştığı hakkında daha fazla bilgi için bkz: [Azure Stack depolama hizmetleri](/azure/azure-stack/user/azure-stack-storage-overview#azure-stack-storage-services).


### <a name="containers"></a>Kapsayıcılar
Kiracı kullanıcılar blob verilerini depolamak için kullanılan bir kapsayıcı oluşturun. Kullanıcı hangi kapsayıcısında bloblar yerleştirmek için karar, depolama hizmeti bir algoritma kapsayıcı koymak için hangi birimde belirlemek için kullanır. Algoritma, genellikle en fazla kullanılabilir alan birimi seçer.  

Bir blob bir kapsayıcıda yerleştirildikten sonra daha fazla alan kullanmak için blob büyüyebilir. Yeni BLOB'ları ve varolan ekleyip blobları büyütün, bu kapsayıcı küçültür tutan birimdeki kullanılabilir alanı.  

Kapsayıcıları tek bir paylaşım için sınırlı değildir. Birleşik blob verilerini bir kapsayıcıda %80 kullanın veya daha fazla kullanılabilir alan büyürken, kapsayıcı girer *taşma* modu. Taşma modundayken bu kapsayıcı içinde oluşturulan tüm yeni blobların yeterli alana sahip farklı bir birime ayrılır. Zaman içinde birden çok birim üzerinde dağıtılmış BLOB'ları bir kapsayıcıda overflow modu olabilir.

% 80'e ve ardından % 90'birimindeki kullanılabilir alanı kullanıldığında sistem Azure Stack Yönetici portalında uyarıları başlatır. Bulut operatörleri kullanılabilir depolama kapasitesi gözden geçirin ve planlama içeriği yeniden dengelemeniz gerekir. Depolama hizmeti, %100 kullanılan bir diski olduğundan ve başka bir uyarı üretilir çalışmayı durduruyor.

### <a name="disks"></a>Diskler
VM diskleri için kapsayıcılar kiracılar tarafından eklenir ve bir işletim sistemi diskini dahil edin. VM'ler, bir veya daha fazla veri diski olarak da olabilir. Her iki türdeki diskleri sayfa blobları depolanır. Kiracılar için her disk, sanal makine performansını artırmak için ayrı bir kapsayıcının içine yerleştirmek için kılavuzdur.
- Bir VM'den bir diski (sayfa blobu) barındıran her kapsayıcı diskine sahip VM ekli bir kapsayıcı olarak kabul edilir.
- Bir VM'den bir diski bulundurmayan bir kapsayıcı, ücretsiz bir kapsayıcı olarak kabul edilir.

Ekli bir kapsayıcısı üzerinde alan boşaltmak için seçenekleri [sınırlıdır](#move-vm-disks).
> [!TIP]  
> Bulut operatörleri, kiracılar için bir kapsayıcı eklemek isteyebileceğiniz sanal makineler (VM) bağlı diskler, doğrudan yönetmez. Ancak, paylaşımlarında depolama alanını yönetmek planlama yaparken, kapsayıcılar ve paylaşımları için disklerin nasıl ilişkili olduğunu anlamak için kullanım olabilir.

## <a name="monitor-shares"></a>İzleyici paylaşımları
Boş alan sınırlı olduğunda anlayabilmeniz paylaşımları izlemek için PowerShell veya Yönetim Portalı'nı kullanın. Portalı kullandığınızda, alan düşük paylaşımlar hakkında uyarılar alırsınız.    

### <a name="use-powershell"></a>PowerShell kullanma
Bulut operatörü olarak, PowerShell kullanarak bir paylaşım depolama kapasitesini izleyebilirsiniz **Get-AzsStorageShare** cmdlet'i. Get-AzsStorageShare cmdlet toplam, ayrılmış ve boş alan bayt cinsinden her paylaşımları döndürür.   
![Örnek: Paylaşımları için boş alan döndürür](media/azure-stack-manage-storage-shares/free-space.png)

- **Toplam Kapasite** paylaşımında kullanılabilir bayt cinsinden toplam alan. Bu alan, veri ve depolama hizmetleri tarafından korunur meta verileri için kullanılır.
- **Kullanılan kapasite** tüm kapsamları kiracısına ilişkin veriler ve ilişkili meta verileri depolayan dosyalarından tarafından kullanılan veri bayt miktarı.

### <a name="use-the-administrator-portal"></a>Yönetici portalını kullanma
Bulut operatörü olarak, tüm paylaşımlar depolama kapasitesini görüntülemek için Yönetim Portalı kullanabilirsiniz.

1. Oturum [Yönetici portalı](https://adminportal.local.azurestack.external).
2. Seçin **tüm hizmetleri** > **depolama** > **dosya paylaşımlarını** kullanım bilgileri görüntüleyebileceğiniz dosya paylaşımı listesini açın. 

    ![Örnek: Depolama dosya paylaşımları](media/azure-stack-manage-storage-shares/storage-file-shares.png)

   - **Toplam** paylaşımında kullanılabilir bayt cinsinden toplam alan. Bu alan, veri ve depolama hizmetleri tarafından korunur meta verileri için kullanılır.
   - **KULLANILAN** tüm kapsamları kiracısına ilişkin veriler ve ilişkili meta verileri depolayan dosyalarından tarafından kullanılan veri bayt miktarı.

### <a name="storage-space-alerts"></a>Depolama alanı uyarıları
Yönetim Portalı'nı kullandığınızda, alan düşük paylaşımlar hakkında uyarılar alırsınız.

> [!IMPORTANT]
> Bir bulut işleci olarak tam kullanım ulaşmasını paylaşımları tutun. Bir paylaşımı kullanılan, % 100 depolama olduğunda bu paylaşım için işlevleri artık hizmeti. Boş alan kurtarmak ve % 100 kullanılan bir paylaşımında işlemleri geri yüklemek için Microsoft Destek'e başvurmanız gerekir.

**Uyarı**: Bir dosya paylaşımı % 80'kullanılan üzerinde olduğunda, aldığınız bir *uyarı* uyarı Yönetim Portalı'nda: ![Örnek: Uyarı bildirimi](media/azure-stack-manage-storage-shares/alert-warning.png)


**Kritik**: Bir dosya paylaşımı % kullanılan 90 üzerinde olduğunda, aldığınız bir *kritik* uyarı Yönetim Portalı'nda: ![Örnek: Kritik Uyarı](media/azure-stack-manage-storage-shares/alert-critical.png)

**Ayrıntıları Görüntüle**: Yönetim Portalı'nda azaltma seçenekleri görmek bir uyarının ayrıntılarını açabilirsiniz: ![Örnek: Uyarı ayrıntılarını görüntüleme](media/azure-stack-manage-storage-shares/alert-details.png)


## <a name="manage-available-space"></a>Kullanılabilir alanı yönetme
Bir paylaşımı boş alan gerekli olduğunda, az bozucu yöntemleri ilk kullanın. Örneğin, bir kapsayıcı geçmeyi tercih etmeniz önce alanı geri kazanmalıdır deneyin.  

### <a name="reclaim-capacity"></a>Kapasiteyi geri kazanmak
*Bu seçenek, çok düğümlü dağıtımlar ve Azure Stack Geliştirme Seti için geçerlidir.*

Silinmiş Kiracı hesapları tarafından kullanılan kapasite kazanabilirsiniz. Bu kapasitesidir otomatik olarak ne zaman iadesi veri [saklama süresi](azure-stack-manage-storage-accounts.md#set-the-retention-period) ulaşıldığında veya hemen geri kazanmak için çalışabilir.

Daha fazla bilgi için [kapasiteyi geri kazanmak](azure-stack-manage-storage-accounts.md#reclaim) içinde depolama kaynaklarını yönetin.

### <a name="migrate-a-container-between-volumes"></a>Bir kapsayıcıyı birimler arasında geçirin
*Bu seçenek, yalnızca çok düğümlü dağıtımlar için geçerlidir.*

Kiracı kullanım düzenlerini nedeniyle bazı Kiracı paylaşımları diğerlerinden daha fazla alan kullanın. Göreceli olarak kullanılmayan diğer paylaşımları önce boşluk düşük çalışır bir paylaşımı sonucu olabilir.

Bazı blob kapsayıcılar için farklı bir paylaşım el ile geçiş yaparak aşırı kullanılmasına paylaşımına alan boşaltmak deneyebilirsiniz. Tümünü tutacak kapasiteye sahip tek bir paylaşım çok daha küçük kapsayıcı geçirebilirsiniz. Taşımak için geçiş kullanabilirsiniz *ücretsiz* kapsayıcıları. Ücretsiz kapsayıcıları bir disk için bir VM içermeyen kapsayıcılardır.   

Geçiş tüm kapsayıcıları blob üzerinde yeni bir paylaşım birleştirir.

- Bir kapsayıcı taşma modu geçtiğini ve blobları ek birimlere yerleştirilen yeni bir paylaşım tüm blobların kapsayıcı geçiş için tutmak için yeterli kapasite olmalıdır. Bu ek paylaşımlar üzerinde bulunan blobları içerir.

- PowerShell cmdlet *Get-AzsStorageContainer* yalnızca bir kapsayıcı için ilk birim kullanımda alanı tanımlar. Cmdlet, ek birimlerde put BLOB'ları tarafından kullanılan alanı tanımlamaz. Bu nedenle, bir kapsayıcının tam boyutlu yetkisiz değiştirmeye karşı korumalı olmayabilir. Bu, yeni paylaşımdaki bir kapsayıcı birleştirilmesi bu yeni paylaşıma ek paylaşımlar verilerin nerede yerleştirir bir taşma durumu gönderebilirsiniz mümkündür. Sonuç olarak, paylaşımları yeniden yeniden dengelemek gerekebilir.

- Bir kaynak grubu için izinler yetersiz olduğundan ve PowerShell ek birimler için taşma veri sorgulamak için kullanılamaz, bu kaynak grubu ve kapsayıcı verilerin toplam boyutuna anlamak için bu verileri geçirmeden önce geçirmek sahibi çalışın.  

> [!IMPORTANT]
> BLOB kapsayıcı için geçiş PowerShell kullanılmasını gerektiren bir çevrimdışı bir işlemdir. Geçiş işlemi tamamlanana kadar tüm bloblar için geçirdiğiniz kapsayıcı çevrimdışı kalır ve kullanılamaz. Ayrıca, tüm devam eden Geçiş tamamlanana kadar Azure Stack yükseltme kaçınmanız gerekir.

#### <a name="to-migrate-containers-using-powershell"></a>PowerShell kullanarak kapsayıcıları geçirmek için
1. Sahip olduğunuzu onaylamak [Azure PowerShell yüklenmiş ve yapılandırılmış](https://azure.microsoft.com/documentation/articles/powershell-install-configure/). Daha fazla bilgi için bkz. [Azure PowerShell'i Azure Resource Manager ile kullanma](https://go.microsoft.com/fwlink/?LinkId=394767).
2. Geçirmeyi planladığınız paylaşımında verilerin ne olduğunu anlamak için kapsayıcı inceleyin. Bir birimi geçiş için en iyi aday kapsayıcıları tanımlamak için kullanmak **Get-AzsStorageContainer** cmdlet:

   ```powershell  
   $farm_name = (Get-AzsStorageFarm)[0].name
   $shares = Get-AzsStorageShare -FarmName $farm_name
   $containers = Get-AzsStorageContainer -ShareName $shares[0].ShareName -FarmName $farm_name
   ```
   Ardından $containers inceleyin:

   ```powershell
   $containers
   ```

   ![Örnek: $Containers](media/azure-stack-manage-storage-shares/containers.png)

3. Geçiş kapsayıcı tutmak için en iyi hedef paylaşımları tanımlayın:

   ```powershell
   $destinationshares = Get-AzsStorageShare -SourceShareName
   $shares[0].ShareName -Intent ContainerMigration
   ```

   Ardından $destinationshares inceleyin:

   ```powershell 
   $destinationshares
   ```

   ![Örnek: $destination Paylaşımları](media/azure-stack-manage-storage-shares/examine-destinationshares.png)

4. Geçiş için bir kapsayıcı başlatın. Geçiş zaman uyumsuzdur. İlk geçiş tamamlanmadan önce ek kapsayıcıları geçişini başlatırsanız, iş kimliği her durumunu izlemek için kullanın.

   ```powershell
   $job_id = Start-AzsStorageContainerMigration -StorageAccountName $containers[0].Accountname -ContainerName $containers[0].Containername -ShareName $containers[0].Sharename -DestinationShareUncPath $destinationshares[0].UncPath -FarmName $farm_name
   ```

   Ardından $jobId inceleyin. Aşağıdaki örnekte, değiştirin *d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0* incelemek istediğiniz iş Kimliğine sahip:

   ```powershell
   $jobId
   d62f8f7a-8b46-4f59-a8aa-5db96db4ebb0
   ```

5. Geçiş işinin durumunu denetlemek için iş kimliği kullanın. Kapsayıcı geçiş tamamlandığında **MigrationStatus** ayarlanır **tam**.

   ```powershell 
   Get-AzsStorageContainerMigrationStatus -JobId $job_id -FarmName $farm_name
   ```

   ![Örnek: Geçiş durumu](media/azure-stack-manage-storage-shares/migration-status1.png)

6. Devam eden geçiş işi iptal edebilirsiniz. Geçiş işleri zaman uyumsuz olarak işlenmesi iptal edildi. İptal $jobid kullanarak izleyebilirsiniz:

   ```powershell
   Stop-AzsStorageContainerMigration -JobId $job_id -FarmName $farm_name
   ```

   ![Örnek: Geri alma durumu](media/azure-stack-manage-storage-shares/rollback.png)

7. Geçiş işinin durumunu doğrulayana kadar komut 6. adımdan yeniden çalıştırabileceğiniz **iptal**:  

    ![Örnek: İptal edildi durumu](media/azure-stack-manage-storage-shares/cancelled.png)

### <a name="move-vm-disks"></a>VM diskleri taşıma
*Bu seçenek, yalnızca çok düğümlü dağıtımlar için geçerlidir.*

En aşırı yöntemin alanını yönetmek üzere sanal makine diskleri taşıma içerir. Ekli bir kapsayıcı (bir sanal makine diskini içeriyor) taşıma karmaşık olduğundan, bu eylemi gerçekleştirmek için Microsoft Support başvurun.

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi edinin [kullanıcılarına sanal makineleri teklifi](azure-stack-tutorial-tenant-vm.md).
