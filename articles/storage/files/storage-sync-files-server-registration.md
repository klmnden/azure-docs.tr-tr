---
title: Azure dosya eşitleme (Önizleme) ile kayıtlı sunucuları yönetme | Microsoft Docs
description: Kaydet ve bir Azure dosya eşitleme depolama eşitleme hizmeti ile bir Windows sunucusu kaydı hakkında bilgi edinin.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: 297f3a14-6b3a-48b0-9da4-db5907827fb5
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2018
ms.author: wgries
ms.openlocfilehash: 7385e8b84668facf8bf44f569a611e7dcdba9a1e
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738301"
---
# <a name="manage-registered-servers-with-azure-file-sync-preview"></a>Azure dosya eşitleme (Önizleme) ile kayıtlı sunucuları yönetme
Azure Dosya Eşitleme (önizleme) aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunu Windows sunucularınızı hızlı Azure dosya paylaşımınıza önbelleğine dönüştürerek yapar. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Aşağıdaki makalede kaydetmek ve depolama eşitleme hizmeti ile bir sunucuyu yönetmek nasıl gösterilmektedir. Bkz: [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md) Azure dosya eşitleme uçtan uca dağıtma hakkında bilgi için.

## <a name="registerunregister-a-server-with-storage-sync-service"></a>Sunucu depolama eşitleme hizmeti ile kaydetme/kaydını Kaldır
Azure dosya eşitleme ile bir sunucu kaydetme, Windows Server ve Azure arasında bir güven ilişkisi oluşturur. Bu ilişki oluşturmak için daha sonra kullanılabilir *sunucu uç noktaları* sunucuda temsil eden bir Azure dosya paylaşımı ile eşitlenen belirli klasörleri (olarak da bilinen bir *endpoint bulut*). 

### <a name="prerequisites"></a>Önkoşullar
Bir depolama eşitleme hizmeti ile bir sunucuyu kaydetmek için önce gerekli önkoşulları sunucunuzla hazırlamanız gerekir:

* Sunucunuz Windows Server'ın desteklenen bir sürümü çalıştırması gerekir. Daha fazla bilgi için bkz: [desteklenen Windows Server sürümleri](storage-sync-files-planning.md#supported-versions-of-windows-server).
* Bir depolama eşitleme hizmeti dağıtıldıktan emin olun. Bir depolama eşitleme hizmetinin nasıl dağıtılacağı hakkında daha fazla bilgi için bkz: [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md).
* Sunucu internet'e bağlı olduğunu ve Azure erişilebilir olduğundan emin olun.
* Sunucu Yöneticisi kullanıcı Arabirimi ile Yöneticiler için IE Artırılmış Güvenlik Yapılandırması devre dışı bırakın.
    
    ![Sunucu Yöneticisi kullanıcı Arabirimi ile vurgulanan IE Artırılmış Güvenlik Yapılandırması](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* AzureRM PowerShell modülü sunucunuz üzerinde yüklü olduğundan emin olun. Sunucunuz bir yük devretme kümesinin bir üyesi ise, kümedeki her düğümün AzureRM modülü gerektirir. AzureRM modülü yükleme hakkında daha fazla ayrıntı bulunabilir [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-azurerm-ps).

    > [!Note]  
    > Bir sunucu kaydı/kaydını silmek için AzureRM PowerShell modülü en yeni sürümünü kullanmanızı öneririz. AzureRM paket bu sunucuda önceden yüklü olmadığını (ve bu sunucuda PowerShell sürümü 5.* veya daha büyük), kullanabileceğiniz `Update-Module` bu paketi güncellemek için cmdlet. 
* Ortamınızda bir ağ proxy sunucusu kullanmak, sunucunuzdaki faydalanmak eşitleme aracısı için proxy ayarlarını yapılandırın.
    1. Proxy IP adresi ve bağlantı noktası numarasını belirleyin
    2. Bu iki dosyayı düzenleyin:
        * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
        * C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
    3. Yukarıdaki iki dosyalarında 127.0.0.1:8888 doğru IP adresine (Değiştir 127.0.0.1) ve doğru bağlantı noktası numarasını (Değiştir 8888) değiştirme /System.ServiceModel altında Şekil 1'e (altında bu bölümde) satırları ekleyin:
    4. Komut satırı aracılığıyla WinHTTP proxy ayarları ayarlayın:
        * Proxy göster: netsh winhttp proxy Göster
        * Proxy ayarı: netsh winhttp proxy 127.0.0.1:8888 ayarlayın
        * Proxy sıfırlama: netsh winhttp proxy Sıfırla
        * Aracı yüklendikten sonra bu kurulumu ise, bizim eşitleme aracıyı yeniden başlatın: net stop filesyncsvc
    
```XML
    Figure 1:
    <system.net>
        <defaultProxy enabled="true" useDefaultCredentials="true">
            <proxy autoDetect="false" bypassonlocal="false" proxyaddress="http://127.0.0.1:8888" usesystemdefault="false" />
        </defaultProxy>
    </system.net>
```    

### <a name="register-a-server-with-storage-sync-service"></a>Bir sunucu depolama eşitleme hizmetine kaydetme
Bir sunucu olarak kullanılabilmesi için önce bir *sunucusu uç noktası* bir Azure dosya eşitleme *eşitleme grubu*, ile kaydedilmelidir bir *depolama eşitleme hizmeti*. Bir sunucu aynı anda yalnızca bir depolama eşitleme hizmeti ile kaydedilebilir.

#### <a name="install-the-azure-file-sync-agent"></a>Azure dosya eşitleme Aracısı'nı yükleme
1. [Azure dosya eşitleme Aracısı'nı indirme](https://go.microsoft.com/fwlink/?linkid=858257).
2. Azure dosya eşitleme Aracısı Yükleyicisi'ni başlatın.
    
    ![Azure dosya eşitleme Aracısı Yükleyicisi'nin ilk bölmesi](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Microsoft Update kullanarak Azure dosya eşitleme aracının güncelleştirmelerinin etkinleştirmeyi unutmayın. Kritik güvenlik düzeltmelerini ve özellik geliştirmeleri server paketi için Microsoft Update gönderilen olduğundan önemlidir.

    ![Microsoft Update Azure dosya eşitleme aracı yükleyici Microsoft Update bölmesinde etkinleştirildiğinden emin olun](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Sunucu önceden kayıtlı değil, sunucu kaydı UI yüklemesi tamamlandıktan hemen sonra açılır.

> [!Important]  
> Sunucusu bir yük devretme kümesinin bir üyesi ise, Azure dosya eşitleme Aracısı kümedeki her düğümde yüklü olması gerekir.

#### <a name="register-the-server-using-the-server-registration-ui"></a>Sunucu kaydı kullanıcı arabirimini kullanarak sunucu kaydetme
> [!Important]  
> Bulut çözümü sağlayıcısı (CSP) abonelikleri sunucu kaydı UI kullanamazsınız. Bunun yerine, PowerShell (Bu bölümü) kullanın.

1. Sunucu kaydı UI Azure dosya eşitleme Aracısı yüklemesi tamamlandıktan hemen sonra başlamazsa, el ile çalıştırarak başlatılabilir `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Tıklatın *oturum açma* , Azure aboneliğinizin erişmek için. 

    ![Sunucu kaydı UI iletişim açma](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Doğru abonelik, kaynak grubu ve depolama eşitleme hizmeti iletişim kutusundan seçin.

    ![Depolama eşitleme hizmet bilgileri](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. Önizleme'de, bir daha fazla oturum açma işlemini tamamlamak için gereklidir. 

    ![İletişim kutusunda oturum](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Sunucusu bir yük devretme kümesinin bir üyesi ise, sunucu kaydı'nı çalıştırmak için her sunucu gerekir. Azure Portalı'nda kayıtlı sunucuları görüntülediğinizde, Azure dosya eşitleme otomatik olarak her düğümün aynı yük devretme kümesinin bir üyesi algılar ve bunları uygun şekilde gruplandıran.

#### <a name="register-the-server-with-powershell"></a>PowerShell ile sunucu kaydetme
Sunucu kaydı PowerShell aracılığıyla de gerçekleştirebilirsiniz. Bu sunucu kayıt bulut çözümü sağlayıcısı (CSP) abonelikler için desteklenen tek yol.

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
Login-AzureRmStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzureRmStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

### <a name="unregister-the-server-with-storage-sync-service"></a>Depolama eşitleme hizmeti ile sunucu kaydını Kaldır
Bir depolama eşitleme hizmeti ile bir sunucu kaydını kaldırmak için gereken birkaç adım vardır. Düzgün bir sunucu kaydını silmek nasıl bir göz atalım.

> [!Warning]  
> Kaydı ve bir sunucu kaydetme veya kaldırarak ve sunucu uç noktalar açıkça için bir Microsoft mühendisi tarafından belirtilmedikçe yeniden eşitleme, bulut katmanlandırma veya herhangi bir değişiklik, Azure dosya eşitleme sorunlarını giderme çalışmayın. Bir sunucu kaydını ve sunucu uç noktalarını kaldırma yıkıcı bir işlemdir ve "kayıtlı sunucu ve sunucu uç noktaları sonra sunucu uç noktaları birimlerdeki katmanlı dosyalar konumlarına Azure Dosya paylaşımındaki için bağlanır değil" yeniden, hangi Eşitleme hataları neden olur. Ayrıca unutmayın, bir sunucu uç nokta ad alanı dışında mevcut katmanlı dosyalar kalıcı olarak kaybolabilir. Katmanlı dosyaları bulunabilir sunucu içinde bile bulut katmanlandırma uç noktaları hiçbir zaman etkindir.

#### <a name="optional-recall-all-tiered-data"></a>(İsteğe bağlı) Tüm katmanlı veri geri çağırma
Azure dosya (örn. Bu, bir üretim olmayan bir test, ortam) eşitleme kaldırdıktan sonra kullanılabilir olması için şu anda katmanlı dosyaları isterseniz, sunucu uç noktaları içeren her bir birim üzerinde tüm dosyalar geri çağırma. Tüm sunucu uç noktaları için katmanlama Bulut devre dışı bırakın ve ardından aşağıdaki PowerShell cmdlet'ini çalıştırın:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <a-volume-with-server-endpoints-on-it>
```

> [!Warning]  
> Sunucu uç noktayı barındıran yerel birim tüm katmanlı verileri geri çekmek için yeterli boş alan yoksa `Invoke-StorageSyncFileRecall` cmdlet'i hata verir.  

#### <a name="remove-the-server-from-all-sync-groups"></a>Sunucu tüm eşitleme gruplardan kaldırma
Depolama eşitleme hizmeti sunucuda kaydını önce o sunucudaki tüm sunucu uç noktaları kaldırılması gerekir. Bu, Azure portal yapılabilir:

1. Sunucunuz, kayıtlı depolama eşitleme hizmetine gidin.
2. Bu sunucu için tüm sunucu uç noktaları her eşitleme grubunda depolama eşitleme hizmeti kaldırın. Bu, eşitleme grubu bölmesinde ilgili sunucusu uç noktası sağ tıklayarak gerçekleştirilebilir.

    ![Sunucusu uç noktası eşitleme grubundan kaldırma](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Bu basit bir PowerShell Betiği ile de gerçekleştirilebilir:

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Connect-AzureRmAccount
Login-AzureRmStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzureRmStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzureRmStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

#### <a name="unregister-the-server"></a>Sunucunun kaydı silinemedi
Tüm verileri geri ve sunucunun tüm eşitleme gruplardan kaldırıldı göre sunucu kaydı olabilir. 

1. Azure portalında gidin *kayıtlı sunucuları* depolama eşitleme hizmeti bölümü.
2. Kaydı kaldırmak ve "Server kaydı" tıklatın istediğiniz sunucuda sağ tıklayın.

    ![Sunucunun kaydını kaldır](media/storage-sync-files-server-registration/unregister-server-1.png)

## <a name="ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter"></a>Azure dosya eşitleme sağlayarak, veri merkezinizdeki iyi komşu olduğu 
Azure dosya eşitleme, veri merkezinizde çalışan tek hizmet nadiren olacağından, Azure dosya eşitleme ağ ve depolama kullanımını sınırlamak isteyebilirsiniz.

> [!Important]  
> Sınırları fazla düşük ayarlanması, Azure dosya eşitleme eşitleme ve geri çağırma performansını etkiler.

### <a name="set-azure-file-sync-network-limits"></a>Azure dosya eşitleme ağ sınırlarını ayarlama
Kullanarak Azure dosya eşitleme ağ kullanımını daraltabilir `StorageSyncNetworkLimit` cmdlet'leri. 

Örneğin, Azure dosya eşitleme 09: 00 ve 18: 00 (17:00 h) iş hafta arasında birden fazla 10 MB/sn kullanmadığından emin olmak için yeni bir kısıtlama sınırı oluşturabilirsiniz: 

```PowerShell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
New-StorageSyncNetworkLimit -Day Monday, Tuesday, Wednesday, Thursday, Friday -StartHour 9 -EndHour 17 -LimitKbps 10000
```

Aşağıdaki cmdlet'i kullanarak sınırınızı görebilirsiniz:

```PowerShell
Get-StorageSyncNetworkLimit # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

Ağ sınırları kaldırmak için kullanın `Remove-StorageSyncNetworkLimit`. Örneğin, aşağıdaki komut, tüm ağ sınırları kaldırır:

```PowerShell
Get-StorageSyncNetworkLimit | ForEach-Object { Remove-StorageSyncNetworkLimit -Id $_.Id } # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

### <a name="use-windows-server-storage-qos"></a>Windows Server depolama QoS kullanın 
Windows Server sanallaştırma ana bilgisayar üzerinde çalışan bir sanal makinede Azure dosya eşitleme barındırıldığında, depolama g/ç tüketim düzenlemek için depolama hizmet kalitesi (depolama hizmet kalitesi) kullanabilirsiniz. Depolama QoS ilkesi, en fazla (veya yukarıda StorageSyncNetwork sınırı nasıl zorlanır gibi sınırı) olarak ya da en az (veya rezervasyon) olarak ayarlanabilir. En az bir maksimum yerine ayarlamayı diğer iş yüklerini kullanmıyorsanız kullanılabilir depolama alanı bant genişliği kullanmak üzere veri bloğu Azure dosya eşitleme sağlar. Daha fazla bilgi için bkz: [depolama hizmet kalitesi](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview).

## <a name="see-also"></a>Ayrıca bkz.
- [Bir Azure dosya eşitleme (Önizleme) dağıtımı için planlama](storage-sync-files-planning.md)
- [Azure dosya eşitleme (Önizleme) dağıtma](storage-sync-files-deployment-guide.md) 
- [Azure dosya eşitleme (Önizleme) sorunlarını giderme](storage-sync-files-troubleshoot.md)
