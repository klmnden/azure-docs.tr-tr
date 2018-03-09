---
title: "Azure yığın depolama hesaplarını yönetme | Microsoft Docs"
description: "Bulma, yönetme, kurtarmak ve Azure yığın depolama hesapları geri hakkında bilgi edinin"
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 627d355b-4812-45cb-bc1e-ce62476dab34
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/22/2018
ms.author: mabrigg
ms.reviewer: anirudha
ms.openlocfilehash: 395cd113e21bf747c796ff28026f552f30656b47
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="manage-storage-accounts-in-azure-stack"></a>Depolama hesaplarını Azure yığınında yönetme
Azure bulmak, kurtarmak ve iş ihtiyaçlarına göre depolama kapasiteyi geri kazanmak için yığın depolama hesaplarında yönetmeyi öğrenin.

## <a name="find"></a>Bir depolama hesabı bulunamadı
Bölgede depolama hesaplarının listesini Azure yığını tarafından görüntülenebilir:

1. Bir Internet tarayıcısında için https://adminportal.local.azurestack.external gidin.
2. Bir bulut işleci (dağıtım sırasında sağladığınız kimlik bilgilerini kullanarak) olarak Azure yığın yönetim portalında oturum açın
3. Varsayılan Panoda – Bul **bölge Yönetimi** listesinde ve örneğin keşfetmek istediğiniz bölgeyi tıklatın **(yerel**).
   
   ![](media/azure-stack-manage-storage-accounts/image1.png)
4. Seçin **depolama** gelen **kaynak sağlayıcıları** listesi.
   
   ![](media/azure-stack-manage-storage-accounts/image2.png)
5. Aşağı kaydırın depolama kaynak sağlayıcısı yönetici bölmesinde – şimdi **depolama hesapları** sekmesinde ve tıklatın.
   
   ![](media/azure-stack-manage-storage-accounts/image3.png)
   
   Bu bölgede depolama hesaplarının listesi sonuçta elde edilen sayfasıdır.
   
   ![](media/azure-stack-manage-storage-accounts/image4.png)

Varsayılan olarak, ilk 10 hesapları görüntülenir. Daha fazla bilgi almak seçebileceğiniz tıklayarak **daha fazla yük** listesinin altındaki bağlantıyı.

OR

Bir depolama hesabında – ilgileniyorsanız yapabilecekleriniz **filtre ve ilgili hesapları fetch** yalnızca.


**Hesapları için filtre uygulamak için:**

1. Tıklatın **filtre** bölmesinin üstünde.
2. Filtre bölmesinde belirtmenizi sağlar **hesap adı**, ** abonelik kimliği veya **durum** görüntülenecek depolama hesaplarının listesini ince ayar yapmak için. Bunları uygun şekilde kullanın.
3. Tıklatın **güncelleştirme**. Listeden uygun şekilde yenilemeniz gerekir.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Filtreyi sıfırlamak için: tıklatın **filtre**, seçimlerini temizlemek ve güncelleştirme.

Arama metin kutusuna (üst kısmında depolama hesapları liste bölmesinde) hesaplar listesinde seçili metni vurgulama olanak sağlar. Tam adı veya kimliği kolayca kullanılabilir olmadığında bunu kullanabilirsiniz.

Burada serbest metin ilgilendiğiniz hesabını bulmak amacıyla kullanabilirsiniz.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Hesap ayrıntılarını inceleyin
Görüntüleme ilgilendiğiniz hesapları bulduktan sonra bazı ayrıntıları görüntülemek için belirli hesap tıklatabilirsiniz. Yeni bir bölme hesabı ayrıntıları gibi açılır: hesap, oluşturulma zamanı, konum vb. türü.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Silinmiş bir hesabı Kurtar
Burada, silinmiş bir hesabı kurtarmanız gereken bir durumda olabilir.

Azure yığınında Bunu yapmak için basit bir yol vardır:

1. Depolama hesapları listesine göz atın. Bkz: [bir depolama hesabını bulmak](#find) daha fazla bilgi için bu konudaki.
2. Belirli bir hesabındaki listesinde bulun. Filtre gerekebilir.
3. Denetleme *durumu* hesabı. Yazması gerekir **silinmiş**.
4. Hesap ayrıntıları bölmesi açılır hesabı'nı tıklatın.
5. Bu bölme üstünde bulun **kurtarmak** düğmesine tıklayın ve tıklatın.
6. Onaylamak için **Evet**’e tıklayın.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. Kurtarma olduğunu *bekleyin... işlem* için başarılı olduğunu göstergesidir.
   İlerleme göstergeleri görüntülemek için portalı üstündeki "zil" simgeye tıklayın.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Kurtarılan hesabı başarıyla eşitlendi sonra yeniden kullanılabilir.

### <a name="some-gotchas"></a>Bazı FRİKİKLERİNDEN
* Silinen hesabınız olarak durumunu gösterir **bekletme dışında**.
  
  Dışında tutma anlamına gelir, silinen hesabın bekletme süresini aştı ve olmayabilir kurtarılabilir.
* Silinen hesabınızı hesaplar listesinde göstermez.
  
  Çöp toplama silinen hesabın zaten olduğunda, hesap hesap listesinden göstermeyebilir. Bu durumda, kurtarılamaz. Bkz: [geri kapasite](#reclaim) bu konuda.

## <a name="set-the-retention-period"></a>Bekletme süresini ayarlama
Saklama dönemi ayarı sırasında olası tüm silinen hesabın kurtarılabilir gün içinde (0 ve 9999 gün arasında) bir süre belirtmek bir bulut operatörü sağlar. Varsayılan saklama dönemi 15 gün olarak ayarlanır. Değer, silinen tüm hesap hemen dışında tutma olduğu ve düzenli atık toplama için işaretlenmiş "0" anlamına gelir için ayarlama.

**Saklama dönemi değiştirmek için:**

1. Bir internet tarayıcısında için https://adminportal.local.azurestack.external gidin.
2. Bir bulut işleci (dağıtım sırasında sağladığınız kimlik bilgilerini kullanarak) olarak Azure yığın yönetim portalında oturum açın
3. Varsayılan Panoda – Bul **bölge Yönetimi** listesinde ve – örneğin keşfetmek istediğiniz bölgeyi tıklatın **(yerel**).
4. Seçin **depolama** gelen **kaynak sağlayıcıları** listesi.
5. Tıklatın **ayarları** ayarı bölmesini açmak için üstünde.
6. Tıklatın **yapılandırma** saklama dönemi değerini düzenleyin.

   Gün sayısını ayarlayın ve sonra kaydedin.
   
   Bu değer hemen etkili olur ve tüm bölgeniz için ayarlanır.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Kapasiteyi geri kazanmak
Bir bekletme dönemi yan etkileri silinmiş bir hesabı dışında tutma süresi gelene kadar kapasite tüketmeye devam biridir. Bulut operatör olarak Bekletme dönemi henüz süresi olsa da silinen hesabın alanı geri kazanmak için bir yol gerekebilir.

Kapasite portal veya PowerShell kullanarak geri kazanabilirsiniz.

**Portalı kullanarak kapasiteyi geri kazanmak için:**
1. Depolama hesapları bölmesine gidin. Bkz: [bir depolama hesabını bulmak](#find).
2. Tıklatın **geri alanı** bölmesinin üstünde.
3. İletisini okuyun ve ardından **Tamam**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Başarı bildirimi bakın portalındaki zil simgesine bekleyin.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Depolama hesapları sayfayı yenileyin. Bunlar temizlenmiş olduğundan silinen hesap artık listesinde gösterilir.

Hemen kapasiteyi geri kazanmak ve aynı zamanda açıkça bekletme süresini geçersiz kılmak için PowerShell kullanın.

**Kapasiteyi geri kazanmak için PowerShell kullanarak:**   

1. Azure PowerShell yüklenmiş ve yapılandırılmış olduğunu doğrulayın. Aksi durumda, aşağıdaki yönergeleri kullanın: 
   * En son Azure PowerShell sürümü yükleyin ve Azure aboneliğinizle ilişkilendirmek için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](http://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Azure Resource Manager cmdlet'leri hakkında daha fazla bilgi için bkz: [Azure PowerShell'i Azure Resource Manager ile kullanma](http://go.microsoft.com/fwlink/?LinkId=394767)
2. Aşağıdaki cmdlet'i çalıştırın:

> [!NOTE]
> Bu cmdlet'i çalıştırmak, hesap ve içeriği kalıcı olarak sil. Kurtarılabilir değil. Dikkatli kullanın.


        Clear-ACSStorageAccount -ResourceGroupName system.local -FarmName <farm ID>


Daha fazla bilgi için bkz: [Azure yığın powershell belgeleri.](https://msdn.microsoft.com/library/mt637964.aspx)
 

## <a name="migrate-a-container"></a>Bir kapsayıcı geçirme
Düzensiz depolama kullanımı kiracılar tarafından nedeniyle bir bulut işleci bir fark edebilirsiniz veya diğerlerinden daha fazla alan kullanarak daha temel Kiracı paylaşır. Bu gerçekleşirse, bulut operatörü bazı blob kapsayıcıları için başka bir paylaşım el ile geçiş yaparak vurgulu paylaşımında biraz alan boşaltın deneyebilirsiniz. 

Kapsayıcıları geçirmek için PowerShell kullanmanız gerekir.
> [!NOTE]
>BLOB kapsayıcı geçiş dinamik geçişi desteklemez ve şu anda çevrimdışı bir işlemdir. Geçiş sırasında ve arka plandaki BLOB'ları bu kapsayıcıda işlemi tamamlanana kadar kullanılamaz ve "Çevrimdışı". 

**PowerShell kullanarak kapsayıcıları geçirmek için:**

1. Azure PowerShell yüklenmiş ve yapılandırılmış olduğunu doğrulayın. Aksi durumda, aşağıdaki yönergeleri kullanın:
    * En son Azure PowerShell sürümü yükleyin ve Azure aboneliğinizle ilişkilendirmek için bkz: [Azure PowerShell'i yükleme ve yapılandırma nasıl](http://azure.microsoft.com/documentation/articles/powershell-install-configure/). Azure Resource Manager cmdlet'leri hakkında daha fazla bilgi için bkz: [Azure PowerShell'i Azure Resource Manager ile kullanma](http://go.microsoft.com/fwlink/?LinkId=394767)
2. Grup adı alın: 
      
      `$farm = Get-ACSFarm -ResourceGroupName system.local`
3. Paylaşımlar alın: 

   `$shares = Get-ACSShare -ResourceGroupName system.local -FarmName $farm.FarmName`

4. Kapsayıcıları için belirli bir paylaşım alın. Sayı ve amacı isteğe bağlı parametreler olduğuna dikkat edin:
            
   `$containers = Get-ACSContainer -ResourceGroupName system.local -FarmName $farm.FarmName -ShareName $shares[0].ShareName -Count 4 -Intent Migration`  

   Ardından $containers inceleyin:

   `$containers`

    ![](media/azure-stack-manage-storage-accounts/image13.png)
5. Kapsayıcı geçiş için en iyi hedef paylaşımları alın:

    `$destinationshares= Get-ACSSharesForMigration  -ResourceGroupName system.local -FarmName $farm.farmname -SourceShareName $shares[0].ShareName`

    Ardından $destinationshares inceleyin:

    `$destinationshares`

    ![](media/azure-stack-manage-storage-accounts/image14.png)
6. Geçiş için bir kapsayıcı kapalı kazandırın, biri bir paylaşımda tüm kapsayıcıları döngü ve döndürülen iş kimliğini kullanarak durumunu izlemek için bu zaman uyumsuz uygulama olduğuna dikkat edin

    `$jobId = Start-ACSContainerMigration -ResourceGroupName system.local -FarmName $farm.farmname -ContainerToMigrate $containers[1] -DestinationShareUncPath $destinationshares.UncPath`

    Ardından $jobId inceleyin:

   ```
   $jobId
   d1d5277f-6b8d-4923-9db3-8bb00fa61b65
   ```
7. İş kimliğini tarafından geçiş işi durumunu denetleme Kapsayıcı geçiş tamamlandığında MigrationStatus "Tamamlandı" olarak ayarlanır

    `Get-ACSContainerMigrationStatus -ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId`

    ![](media/azure-stack-manage-storage-accounts/image15.png)

8. Devam eden geçiş işi iptal edebilirsiniz. Bu yeniden zaman uyumsuz bir işlem ve $jobid kullanarak izlenebilir:

    `Stop-ACSContainerMigration-ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId-Verbose`

    ![](media/azure-stack-manage-storage-accounts/image16.png)

    Geçiş durumları yeniden iptal denetleyebilirsiniz:

    `Get-ACSContainerMigrationStatus-ResourceGroupName system.local -FarmName $farm.farmname -JobId $jobId`

    ![](media/azure-stack-manage-storage-accounts/image17.png)




  
  