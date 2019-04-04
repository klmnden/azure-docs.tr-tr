---
title: Azure Stack depolama hesaplarını yönetme | Microsoft Docs
description: Bulma, yönetme, kurtarma ve Azure Stack depolama hesaplarının geri kazanmak hakkında bilgi edinin
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
ms.date: 03/19/2019
ms.author: mabrigg
ms.reviewer: xiaofmao
ms.lastreviewed: 03/19/2019
ms.openlocfilehash: ceb092192924b79f1584d72b307e3bc3512c0025
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484178"
---
# <a name="manage-azure-stack-storage-accounts"></a>Azure Stack depolama hesaplarını yönetme

Azure Stack depolama hesaplarını yönetmeyi öğrenin. Bulma, kurtarma ve iş ihtiyaçlarına göre depolama kapasiteyi geri kazanmak.

## <a name="find-a-storage-account"></a>Bir depolama hesabı bulunamadı

Bölgede depolama hesaplarının listesi Azure yığını tarafından görüntülenebilir:

1. Oturum [Yönetici portalı](https://adminportal.local.azurestack.external).

2. Seçin **tüm hizmetleri** > **depolama** > **depolama hesapları**.

   ![Azure Stack depolama hesapları](media/azure-stack-manage-storage-accounts/image4.png)

Varsayılan olarak, ilk 10 hesapları görüntülenir. Daha fazla bilgi almak seçebileceğiniz tıklayarak **daha fazla Yükle** listenin altındaki bağlantıyı.

OR

Belirli bir depolama hesabında – ilgileniyorsanız yapabilecekleriniz **filtrelemek ve ilgili hesapları fetch** yalnızca.


**Hesaplar için filtre uygulamak için:**

1. Seçin **filtre** bölmenin üstünde.
2. Filtre bölmesini üzerinde bu belirtmenizi sağlar **hesap adı**, **abonelik kimliği**, veya **durumu** görüntülenecek depolama hesaplarının listesi ince ayar yapmak için. Bunları uygun şekilde kullanın.
3. Siz yazarken liste otomatik olarak filtre uygulanır.  .
   
    ![Azure Stack depolama hesaplarının Filtrele](media/azure-stack-manage-storage-accounts/image5.png)

4. Filtreyi sıfırlamak için: seçin **filtre**, seçimleri Temizle ve güncelleştirin.

Arama metin kutusuna (üst kısmındaki depolama hesaplarının listesi bölmesinde) hesapları listesinde seçili metni vurgulayın olanak tanır. Tam adı veya kimliği kolayca kullanılabilir olmadığı durumlarda kullanabilirsiniz.

İlgilendiğiniz hesap bulmak için serbest metin kullanabilirsiniz.

![Azure Stack depolama hesaplarını bulma](media/azure-stack-manage-storage-accounts/image6.png)

## <a name="look-at-account-details"></a>Hesap ayrıntılarını inceleyin
Görüntüleme ilgilendiğiniz hesaplarını bulduktan sonra belirli ayrıntılarını görüntülemek için belirli bir hesabı seçebilirsiniz. Yeni bir bölme hesabı ayrıntıları gibi açar: hesap, oluşturma zamanı, konum, vb. türü.

![](media/azure-stack-manage-storage-accounts/image7.png)

## <a name="recover-a-deleted-account"></a>Silinen hesabı kurtarma
Silinen hesabı kurtarma için gerek duyduğunuz bir durumda olabilir.

Azure Stack'te Bunu yapmak için basit bir yolu yoktur:

1. Depolama hesapları listesine göz atın. Daha fazla bilgi için bu makaledeki bir depolama hesabı Bul bakın.
2. Bu belirli hesabını listede bulun. Filtre gerekebilir.
3. Denetleme *durumu* hesabının. Söyleyin **silinmiş**.
4. Hesap ayrıntıları bölmesi açılır hesabı seçin.
5. Bu bölmede üzerinde bulun **kurtarmak** düğmesini tıklatın ve seçin.
6. Onaylamak için **Evet**’i seçin.
   
   ![](media/azure-stack-manage-storage-accounts/image8.png)
7. Yer artık kurtarma *bekleyin... işlem* için başarılı olduğunu göstergesidir.
   İlerleme göstergeleri görüntülemek için portalın üst kısmındaki "zil" simgesini de seçebilirsiniz.
   
   ![](media/azure-stack-manage-storage-accounts/image9.png)
   
   Kurtarılan hesabı başarıyla eşitlendi sonra yeniden kullanılabilir.

### <a name="some-gotchas"></a>Bazı tuzakları
* Hesabınız silindi durumu olarak gösterir **bekletme dışında**.
  
  Silinen hesabı saklama süresini aştı ve olmayabilir kurtarılabilir dışında bekletme anlamına gelir.
* Silinen hesabınızı hesaplar listesinde göstermez.
  
  Silinen hesabı zaten atık bırakıldığında hesabınız hesap listesinde gösterilmeyebilir. Bu durumda, kurtarılamaz. Bkz: [kapasiteyi geri kazanmak](#reclaim) bu makaledeki.

## <a name="set-the-retention-period"></a>Bekletme süresini ayarlama
Saklama dönemi ayarı aşamasında olası tüm silinen hesabı kurtarılabilir gün içinde (0 ve 9999 gün arasında) bir zaman aralığı belirtmek bir bulut işlecini verir. Varsayılan saklama süresi 0 gün olarak ayarlanır. Tüm silinen hesabı hemen bekletme dışında olan ve düzenli çöp toplama için işaretlenmiş, "0" anlamına gelir değeri ayarlanamadı.

**Bekletme süresini değiştirmek için:**

1. Oturum [Yönetici portalı](https://adminportal.local.azurestack.external).
2. Seçin **tüm hizmetleri** > **bölge Yönetimi** altında **Yönetim**.
3. Seçin **kaynak sağlayıcıları** > **depolama** > **ayarları**. Giriş yoludur > *bölge* -kaynak sağlayıcıları > depolama.
4. Seçin **yapılandırma** Bekletme dönemi değerini düzenleyin.

   Gün sayısını ayarlayın ve kaydedin.
   
   Bu değer, hemen etkili olur ve bölgeniz için ayarlanır.

   ![](media/azure-stack-manage-storage-accounts/image10.png)

## <a name="reclaim"></a>Kapasiteyi geri kazanmak
Bir bekletme dönemi yan etkileri silinen hesabı dışında bir bekletme dönemi gelene kadar kapasite kullanma devam ettiğinden biridir. Bir bulut işleci olarak saklama süresi henüz süresi olsa bile silinen hesabı alan kazanmak için bir yol gerekebilir.

Portal veya PowerShell kullanarak kapasite kazanabilirsiniz.

**Portalı kullanarak kapasiteyi geri kazanmak için:**
1. Depolama hesapları bölmesine gidin. Bulma bir depolama hesabına bakın.
2. Seçin **alanı geri kazan** bölmenin üstünde.
3. İleti okumak ve ardından **Tamam**.

    ![](media/azure-stack-manage-storage-accounts/image11.png)
4. Başarı bildirimini bakın portalında zil simgesine bekleyin.

    ![](media/azure-stack-manage-storage-accounts/image12.png)
5. Depolama hesapları sayfayı yenileyin. Bunlar temizlenmiş olduğundan silinen hesaplar artık listesinde gösterilir.

Ayrıca Bekletme dönemi açıkça geçersiz kılmak için PowerShell kullanın ve hemen kapasiteyi geri kazanmak.

**Kapasiteyi geri kazanmak için PowerShell'i kullanma:**   

1. Azure PowerShell sürümünün yüklü ve yapılandırılmış olduğunu doğrulayın. Aksi durumda, aşağıdaki yönergeleri kullanın: 
   * En son Azure PowerShell sürümünü yükleyin ve Azure aboneliğinizle ilişkilendirmek için bkz: [Azure PowerShell'i yükleme ve yapılandırma işlemini](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
   Azure Resource Manager cmdlet'leri hakkında daha fazla bilgi için bkz: [Azure PowerShell'i Azure Resource Manager ile kullanma](https://go.microsoft.com/fwlink/?LinkId=394767)
2. Aşağıdaki cmdlet'leri çalıştırın:

> [!NOTE]  
> Bu cmdlet'leri çalıştırmak, hesabını ve içeriğini kalıcı olarak sil. Kurtarılabilir değil. Bu, dikkatli kullanın.

```powershell  
    $farm_name = (Get-AzsStorageFarm)[0].name
    Start-AzsReclaimStorageCapacity -FarmName $farm_name
```

Daha fazla bilgi için [Azure Stack PowerShell belgeleri](https://docs.microsoft.com/powershell/azure/azure-stack/overview).
 

## <a name="next-steps"></a>Sonraki adımlar

 - İzinleri yönetme hakkında bilgi için bkz. [Manage Role-Based erişim denetimi](azure-stack-manage-permissions.md).
 - Azure Stack için depolama kapasitesi yönetme hakkında daha fazla bilgi için bkz: [Azure Stack için depolama kapasitesi yönetme](azure-stack-manage-storage-shares.md).
