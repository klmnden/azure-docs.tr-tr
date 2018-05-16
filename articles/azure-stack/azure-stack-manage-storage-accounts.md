---
title: Azure yığın depolama hesaplarını yönetme | Microsoft Docs
description: Bulma, yönetme, kurtarmak ve Azure yığın depolama hesapları geri hakkında bilgi edinin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 627d355b-4812-45cb-bc1e-ce62476dab34
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: PowerShell
ms.topic: get-started-article
ms.date: 05/10/2018
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.openlocfilehash: 2ae2b628b2e61893a5289151c3b405e7412e7d13
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/12/2018
---
# <a name="manage-storage-accounts-in-azure-stack"></a>Depolama hesaplarını Azure yığınında yönetme
Azure bulmak, kurtarmak ve iş ihtiyaçlarına göre depolama kapasiteyi geri kazanmak için yığın depolama hesaplarında yönetmeyi öğrenin.

## <a name="find"></a>Bir depolama hesabı bulunamadı
Bölgede depolama hesaplarının listesini Azure yığını tarafından görüntülenebilir:

1. Bir Internet tarayıcısında gidin https://adminportal.local.azurestack.external.
2. Bir bulut işleci (dağıtım sırasında sağladığınız kimlik bilgilerini kullanarak) olarak Azure yığın yönetim portalında oturum açın
3. Varsayılan Panoda – Bul **bölge Yönetimi** listesinde ve örneğin keşfetmek istediğiniz bölgeyi seçin **(yerel**).
   
   ![](media/azure-stack-manage-storage-accounts/image1.png)
4. Seçin **depolama** gelen **kaynak sağlayıcıları** listesi.
   
   ![](media/azure-stack-manage-storage-accounts/image2.png)
5. Aşağı kaydırın depolama kaynak sağlayıcısı yönetici bölmesinde – şimdi **depolama hesapları** sekmesinde ve seçin.
   
   ![](media/azure-stack-manage-storage-accounts/image3.png)
   
   Bu bölgede depolama hesaplarının listesi sonuçta elde edilen sayfasıdır.
   
   ![](media/azure-stack-manage-storage-accounts/image4.png)

Varsayılan olarak, ilk 10 hesapları görüntülenir. Daha fazla bilgi almak seçebileceğiniz tıklayarak **daha fazla yük** listesinin altındaki bağlantıyı.

OR

Bir depolama hesabında – ilgileniyorsanız yapabilecekleriniz **filtre ve ilgili hesapları fetch** yalnızca.


**Hesapları için filtre uygulamak için:**

1. Seçin **filtre** bölmesinin üstünde.
2. Filtre bölmesinde belirtmenizi sağlar **hesap adı**, ** abonelik kimliği veya **durum** görüntülenecek depolama hesaplarının listesini ince ayar yapmak için. Bunları uygun şekilde kullanın.
3. Seçin **güncelleştirme**. Listeden uygun şekilde yenilemeniz gerekir.
   
    ![](media/azure-stack-manage-storage-accounts/image5.png)
4. Filtreyi sıfırlamak için: seçin **filtre**, seçimlerini temizlemek ve güncelleştirin.

Arama metin kutusuna (üst kısmında depolama hesapları liste bölmesinde) hesaplar listesinde seçili metni vurgulama olanak sağlar. Tam adı veya kimliği kolayca kullanılabilir olmadığında bunu kullanabilirsiniz.

Burada serbest metin ilgilendiğiniz hesabını bulmak amacıyla kullanabilirsiniz.

![](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Hesap ayrıntılarını inceleyin
Görüntüleme ilgilendiğiniz hesapları bulduktan sonra bazı ayrıntıları görüntülemek için belirli hesap seçebilirsiniz. Yeni bir bölme hesabı ayrıntıları gibi açılır: hesap, oluşturulma zamanı, konum vb. türü.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Silinmiş bir hesabı Kurtar
Burada, silinmiş bir hesabı kurtarmanız gereken bir durumda olabilir.

Azure yığınında Bunu yapmak için basit bir yol vardır:

1. Depolama hesapları listesine göz atın. Bkz: [bir depolama hesabını bulmak](#find) daha fazla bilgi için bu konudaki.
2. Belirli bir hesabındaki listesinde bulun. Filtre gerekebilir.
3. Denetleme *durumu* hesabı. Yazması gerekir **silinmiş**.
4. Hesap ayrıntıları bölmesi açılır hesabı seçin.
5. Bu bölme üstünde bulun **kurtarmak** düğmesini tıklatın ve seçin.
6. Seçin **Evet** onaylamak için.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. Kurtarma olduğunu *bekleyin... işlem* için başarılı olduğunu göstergesidir.
   İlerleme göstergeleri görüntülemek için portalı üstündeki "zil" simgesine de seçebilirsiniz.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Kurtarılan hesabı başarıyla eşitlendi sonra yeniden kullanılabilir.

### <a name="some-gotchas"></a>Bazı FRİKİKLERİNDEN
* Silinen hesabınız olarak durumunu gösterir **bekletme dışında**.
  
  Dışında tutma anlamına gelir, silinen hesabın bekletme süresini aştı ve olmayabilir kurtarılabilir.
* Silinen hesabınızı hesaplar listesinde göstermez.
  
  Çöp toplama silinen hesabın zaten olduğunda, hesap hesap listesinden göstermeyebilir. Bu durumda, kurtarılamaz. Bkz: [geri kapasite](#reclaim) bu konuda.

## <a name="set-the-retention-period"></a>Bekletme süresini ayarlama
Saklama dönemi ayarı sırasında olası tüm silinen hesabın kurtarılabilir gün içinde (0 ve 9999 gün arasında) bir süre belirtmek bir bulut operatörü sağlar. Varsayılan saklama dönemi 0 gün olarak ayarlanır. Değer, silinen tüm hesap hemen dışında tutma olduğu ve düzenli atık toplama için işaretlenmiş "0" anlamına gelir için ayarlama.

**Saklama dönemi değiştirmek için:**

1. Bir internet tarayıcısında gidin https://adminportal.local.azurestack.external.
2. Bir bulut işleci (dağıtım sırasında sağladığınız kimlik bilgilerini kullanarak) olarak Azure yığın yönetim portalında oturum açın
3. Varsayılan Panoda – Bul **bölge Yönetimi** listesinde ve – örneğin keşfetmek istediğiniz bölgeyi seçin **(yerel**).
4. Seçin **depolama** gelen **kaynak sağlayıcıları** listesi.
5. Seçin **ayarları** ayarı bölmesini açmak için üstünde.
6. Seçin **yapılandırma** saklama dönemi değerini düzenleyin.

   Gün sayısını ayarlayın ve sonra kaydedin.
   
   Bu değer hemen etkili olur ve tüm bölgeniz için ayarlanır.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Kapasiteyi geri kazanmak
Bir bekletme dönemi yan etkileri silinmiş bir hesabı dışında tutma süresi gelene kadar kapasite tüketmeye devam biridir. Bulut operatör olarak Bekletme dönemi henüz süresi olsa da silinen hesabın alanı geri kazanmak için bir yol gerekebilir.

Kapasite portal veya PowerShell kullanarak geri kazanabilirsiniz.

**Portalı kullanarak kapasiteyi geri kazanmak için:**
1. Depolama hesapları bölmesine gidin. Bkz: [bir depolama hesabını bulmak](#find).
2. Seçin **geri alanı** bölmesinin üstünde.
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
2. Aşağıdaki cmdlet'leri çalıştırın:

> [!NOTE]
> Bu cmdlet'ler çalıştırırsanız, hesap ve içeriği kalıcı olarak sil. Kurtarılabilir değil. Dikkatli kullanın.

```PowerShell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    Start-AzsReclaimStorageCapacity -FarmName $farm_name
````

Daha fazla bilgi için bkz: [Azure yığın PowerShell belgeleri.](https://msdn.microsoft.com/library/mt637964.aspx)
 

## <a name="next-steps"></a>Sonraki adımlar

 - İzinleri yönetme hakkında bilgi için bkz: [Manage Role-Based erişim denetimi](azure-stack-manage-permissions.md).
 - Azure yığın depolama kapasitesi yönetme hakkında daha fazla bilgi için bkz: [Azure yığın depolama kapasitesi yönetmek](azure-stack-manage-storage-shares.md).