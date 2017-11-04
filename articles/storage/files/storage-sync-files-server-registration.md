---
title: "Azure dosya eşitleme (Önizleme) sahip bir sunucu kaydı/kaydı | Microsoft Docs"
description: "Kaydet ve bir Azure dosya eşitleme depolama eşitleme hizmeti ile bir Windows sunucusu kaydı hakkında bilgi edinin."
services: storage
documentationcenter: 
author: wmgries
manager: klaasl
editor: jgerend
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/08/2017
ms.author: wgries
ms.openlocfilehash: 13a75d5cafd94435346660614721399f2d77919b
ms.sourcegitcommit: b723436807176e17e54f226fe00e7e977aba36d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2017
---
# <a name="registerunregister-a-server-with-azure-file-sync-preview"></a>Azure dosya eşitleme (Önizleme) ile bir sunucu kaydı/kaydını Kaldır
Azure Dosya Eşitleme (önizleme) aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunun için Windows sunucularınızı hızlı bir Azure Dosyaları paylaşım önbelleğine dönüştürür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Aşağıdaki makalede, kaydetme ve depolama eşitleme hizmeti ile bir sunucu kaydını göstermektedir. Bu, bir sunucu kullanımdan alındığında veya yeni bir sunucu uç noktası bir eşitleme grubundaki isterseniz istenen. Bkz: [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md) Azure dosya eşitleme uçtan uca dağıtma hakkında bilgi için.

## <a name="prerequisites"></a>Ön koşullar
Windows Server depolama eşitleme hizmeti ile kaydetmek için önce gerekli önkoşulları bir Windows Server hazırlamanız gerekir:

* Bir depolama eşitleme hizmeti dağıtıldıktan emin olun. Bir depolama eşitleme hizmetinin nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md).
* Sunucu internet'e bağlı olduğunu ve Azure erişilebilir olduğundan emin olun.
* Sunucu Yöneticisi kullanıcı Arabirimi ile Yöneticiler için IE Artırılmış Güvenlik Yapılandırması devre dışı bırakın.
    
    ![Sunucu Yöneticisi kullanıcı Arabirimi ile vurgulanan IE Artırılmış Güvenlik Yapılandırması](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* AzureRM PowerShell modülü sunucunuz üzerinde yüklü olduğundan emin olun. Sunucunuz bir yük devretme kümesinin bir üyesi ise, kümedeki her düğümün AzureRM modülü gerektirir. AzureRM modülü yükleme hakkında daha fazla ayrıntı bulunabilir [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > Bir sunucu kaydı/kaydını silmek için AzureRM PowerShell modülü en yeni sürümünü kullanmanızı öneririz. AzureRM paket bu sunucuda önceden yüklü olmadığını (ve bu sunucuda PowerShell sürümü 5.* veya daha büyük), kullanabileceğiniz `Update-Module` bu paketi güncellemek için cmdlet. 

## <a name="register-a-server-with-storage-sync-service"></a>Bir sunucu depolama eşitleme hizmetine kaydetme
Bir Windows sunucusu olarak kullanılabilmesi için önce bir *sunucusu uç noktası* bir Azure dosya eşitleme *eşitleme grubu*, ile kaydedilmelidir bir *depolama eşitleme hizmeti*. Bir sunucu yalnızca bir kaydedilebilir *depolama eşitleme hizmeti* birer birer.

### <a name="install-the-azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı'nı yükleme
1. [Azure dosya eşitleme Aracısı'nı indirme](https://go.microsoft.com/fwlink/?linkid=858257).
2. Azure dosya eşitleme Aracısı Yükleyicisi'ni başlatın.
    
    ![Azure dosya eşitleme Aracısı Yükleyicisi'nin ilk bölmesi](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Microsoft Update kullanarak Azure dosya eşitleme aracının güncelleştirmelerinin etkinleştirmeyi unutmayın. Kritik güvenlik düzeltmelerini ve özellik geliştirmeleri server paketi için Microsoft Update gönderilen olduğundan önemlidir.

    ![Microsoft Update Azure dosya eşitleme aracı yükleyici Microsoft Update bölmesinde etkinleştirildiğinden emin olun](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Sunucu önceden kayıtlı değil, sunucu kaydı UI yüklemesi tamamlandıktan hemen sonra açılır.

> [!Important]  
> Sunucusu bir yük devretme kümesinin bir üyesi ise, Azure dosya eşitleme Aracısı kümedeki her düğümde yüklü olması gerekir.

### <a name="register-the-server-using-the-server-registration-ui"></a>Sunucu kaydı kullanıcı arabirimini kullanarak sunucu kaydetme

> [!Important]  
> Bulut çözümü sağlayıcısı abonelikleri sunucu kaydı UI kullanamazsınız. Bunun yerine, PowerShell (Bu bölümü) kullanın.

1. Sunucu kaydı UI Azure dosya eşitleme Aracısı yüklemesi tamamlandıktan hemen sonra başlamazsa, el ile çalıştırarak başlatılabilir `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Tıklatın *oturum açma* , Azure aboneliğinizin erişmek için. 

    ![Sunucu kayıt UI iletişim açma](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Doğru abonelik, kaynak grubu ve depolama eşitleme hizmeti iletişim kutusundan seçin.

    ![Depolama eşitleme hizmet bilgileri](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. Önizleme'de, bir daha fazla oturum açma işlemini tamamlamak için gereklidir. 

    ![İletişim kutusunda oturum](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Sunucusu bir yük devretme kümesinin bir üyesi ise, sunucu kaydı'nı çalıştırmak için her sunucu gerekir. Azure Portalı'nda kayıtlı sunucuları görüntülediğinizde, Azure dosya eşitleme otomatik olarak her düğümün aynı yük devretme kümesinin bir üyesi algılar ve bunları uygun şekilde gruplandıran.

### <a name="register-the-server-with-powershell"></a>PowerShell ile sunucu kaydetme
Sunucu kaydı PowerShell aracılığıyla de gerçekleştirebilirsiniz. Bu sunucu kayıt bulut çözümü sağlayıcısı abonelikler için desteklenen tek yol.

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

## <a name="unregister-the-server-with-storage-sync-service"></a>Depolama eşitleme hizmeti ile sunucu kaydını Kaldır
Bir depolama eşitleme hizmeti ile bir sunucu kaydını kaldırmak için gereken birkaç adım vardır. Düzgün bir sunucu kaydını silmek nasıl bir göz atalım.

### <a name="optional-recall-all-tiered-data"></a>(İsteğe bağlı) Tüm katmanlı veri geri çağırma
Katmanlama sunucusu uç noktası için etkinleştirildiğinde, bulut *katmanı* , Azure dosya paylaşımları için dosya. Bu dosya sunucusu üzerindeki alanı verimli kullanılmasını sağlamak için veri kümesi, tam bir kopyasını yerine bir önbellek olarak davranmak üzere şirket içi dosya paylaşımları sağlar. Ancak, bir sunucu uç sunucuda yerel olarak hala katmanlı dosyalarla kaldırılırsa, bu dosyaları erişilemiyor olur. Bu nedenle, dosya erişimini istenen etseydi, Azure dosyaları tüm katmanlı dosyaları silme ile devam etmeden önce geri çağırma gerekir. 

Bu PowerShell cmdlet'ini aşağıda gösterildiği gibi yapılabilir:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```

> [!Warning]  
> Barındırma sunucusu yerel birim tüm katmanlı verileri geri çekmek için yeterli boş alan yoksa `Invoke-StorageSyncFileRecall` cmdlet'i hata verir.  

### <a name="remove-the-server-from-all-sync-groups"></a>Sunucu tüm eşitleme gruplardan kaldırma
Depolama eşitleme hizmeti sunucuda kaydını önce bu sunucu için tüm sunucu uç noktaları kaldırılması gerekir. Bu, Portal yapılabilir:

1. Sunucunuz, kayıtlı depolama eşitleme hizmetine gidin.
2. Bu sunucu için tüm sunucu uç noktaları her eşitleme grubunda depolama eşitleme hizmeti kaldırın. Bu, ilgili sunucusu uç noktası eşitleme grubu bölmesinde sağ tıklayarak gerçekleştirilebilir.

    ![Sunucusu uç noktası eşitleme grubundan kaldırma](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Bu basit bir PowerShell Betiği ile de gerçekleştirilebilir:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Login-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

### <a name="unregister-the-server"></a>Sunucunun kaydı silinemedi
Tüm verileri geri ve sunucunun tüm eşitleme gruplardan kaldırıldı göre sunucu kaydı olabilir. 

1. Azure Portal'da depolama eşitleme hizmeti "Kayıtlı sunucuları" bölümüne gidin.
2. Kaydı kaldırmak ve "Server kaydı" tıklatın istediğiniz sunucuda sağ tıklayın.

    ![Sunucu kaydını Kaldır](media/storage-sync-files-server-registration/unregister-server-1.png)