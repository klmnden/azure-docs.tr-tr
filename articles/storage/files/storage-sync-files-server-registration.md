---
title: Azure dosya eşitleme ile kayıtlı sunucuları yönetme | Microsoft Docs
description: Kaydolun ve bir Azure dosya eşitleme depolama eşitleme hizmeti ile bir Windows Server kaydını öğrenin.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 07/19/2018
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 9f1195927acee143a34ec6c74f3ad301194fbc84
ms.sourcegitcommit: 37343b814fe3c95f8c10defac7b876759d6752c3
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63759480"
---
# <a name="manage-registered-servers-with-azure-file-sync"></a>Azure dosya eşitleme ile kayıtlı sunucuları yönetme
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Bunu Windows sunucularınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürerek yapar. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilir ve dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Aşağıdaki makalede kaydetmek ve depolama eşitleme hizmeti ile bir sunucuyu yönetmek nasıl gösterir. Bkz: [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md) Azure dosya eşitleme uçtan uca dağıtma hakkında daha fazla bilgi için.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="registerunregister-a-server-with-storage-sync-service"></a>Depolama eşitleme hizmeti ile bir sunucu kaydı/unregister
Azure dosya eşitleme ile bir sunucu kaydetme, Windows Server ve Azure arasında bir güven ilişkisi oluşturur. Bu ilişki oluşturmak için daha sonra kullanılabilir *sunucu uç noktaları* sunucuda, bir Azure dosya paylaşımı ile eşitlenmesi gereken belirli klasörlere temsil (olarak da bilinen bir *bulut uç noktası*). 

### <a name="prerequisites"></a>Önkoşullar
Depolama eşitleme hizmeti ile bir sunucuyu kaydetmek için önce gerekli önkoşulları sunucunuzla hazırlamanız gerekir:

* Sunucunuz Windows Server'ın desteklenen bir sürümünü çalıştırmalıdır. Daha fazla bilgi için [Azure dosya eşitleme sistem gereksinimleri ve birlikte çalışabilirlik](storage-sync-files-planning.md#azure-file-sync-system-requirements-and-interoperability).
* Depolama eşitleme hizmeti dağıtıldığını emin olun. Depolama eşitleme hizmeti dağıtma hakkında daha fazla bilgi için bkz. [Azure dosya eşitleme dağıtmayı](storage-sync-files-deployment-guide.md).
* Sunucu internet'e bağlı olduğundan ve Azure erişilebilir olduğundan emin olun.
* Sunucu Yöneticisi kullanıcı Arabirimi ile Yöneticiler için IE Artırılmış Güvenlik Yapılandırması devre dışı bırakın.
    
    ![Sunucu Yöneticisi kullanıcı Arabirimi ile vurgulanmış IE Artırılmış Güvenlik Yapılandırması](media/storage-sync-files-server-registration/server-manager-ie-config.png)

* Azure PowerShell Modülü'nın sunucuda yüklü olduğundan emin olun. Sunucunuz bir yük devretme kümesinin bir üyesi ise, kümedeki her düğümün Az modül gerektirir. Az Modül yükleme hakkında daha fazla ayrıntı bulunabilir [Azure PowerShell'i yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-Az-ps).

    > [!Note]  
    > Bir sunucu kaydı/kaydını kaldırma Az PowerShell modülünün en yeni sürümü kullanmanızı öneririz. Az paket bu sunucuda daha önce yüklenmişse (ve bu sunucu üzerindeki PowerShell sürümü 5.* veya üzeri), kullanabilirsiniz `Update-Module` bu paketi güncelleştirmeye yönelik cmdlet'i. 
* Ortamınızda ağ proxy sunucusu kullanmak, sunucunuzdaki yararlanmak eşitleme aracısı için proxy ayarlarını yapılandırın.
    1. Ara sunucu IP adresi ve bağlantı noktası numarasını belirleyin
    2. Bu iki dosyayı düzenleyin:
        * C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config
        * C:\Windows\Microsoft.NET\Framework\v4.0.30319\Config\machine.config
    3. Yukarıdaki iki dosyalarda 127.0.0.1:8888 doğru IP adresi (127.0.0.1 değiştirin) için ve doğru bağlantı noktası numarasını (Değiştir 8888) değiştirme /System.ServiceModel altında Şekil 1 (altında bu bölümde) satırları ekleyin:
    4. Komut satırı aracılığıyla WinHTTP Ara sunucu ayarlarını ayarlayın:
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

### <a name="register-a-server-with-storage-sync-service"></a>Depolama eşitleme hizmeti ile bir sunucuyu kaydetmek
Bir sunucu olarak kullanılabilmesi için önce bir *sunucu uç noktası* bir Azure dosya eşitleme'deki *eşitleme grubu*, ile kaydedilmelidir bir *depolama eşitleme hizmeti*. Bir sunucu aynı anda yalnızca bir depolama eşitleme hizmeti ile kaydedilebilir.

#### <a name="install-the-azure-file-sync-agent"></a>Azure Dosya Eşitleme aracısını yükleme
1. [Azure dosya eşitleme Aracısı'nı indirme](https://go.microsoft.com/fwlink/?linkid=858257).
2. Azure dosya eşitleme Aracısı Yükleyicisi'ni başlatın.
    
    ![Azure dosya eşitleme Aracısı Yükleyicisi'nin ilk bölmesi](media/storage-sync-files-server-registration/install-afs-agent-1.png)

3. Microsoft Update kullanarak Azure dosya eşitleme Aracısı güncelleştirmeleri etkinleştirmek emin olun. Kritik güvenlik düzeltmeleri ve özellik geliştirmeleri sunucu paketini için Microsoft Update sağlanan olduğundan önemlidir.

    ![Microsoft Update Microsoft Update bölmesinde Azure dosya eşitleme Aracısı Yükleyicisi'nin etkinleştirildiğinden emin olun](media/storage-sync-files-server-registration/install-afs-agent-2.png)

4. Sunucu önceden kayıtlı değil, sunucu kaydı UI yüklemesi tamamlandıktan hemen sonra açılır.

> [!Important]  
> Sunucu bir yük devretme kümesinin bir üyesi ise, Azure dosya eşitleme aracısını kümedeki tüm düğümlerde yüklü olması gerekir.

#### <a name="register-the-server-using-the-server-registration-ui"></a>Sunucu kaydı UI kullanarak sunucuyu kaydetme
> [!Important]  
> Bulut çözümü sağlayıcısı (CSP) Abonelikleri, sunucu kaydı UI kullanamazsınız. Bunun yerine PowerShell (Bu bölümü) kullanın.

1. Azure dosya eşitleme Aracısı yüklemesi tamamlandıktan hemen sonra sunucu kaydı UI başlatmadıysanız, bunu el ile çalıştırarak başlatılabilir `C:\Program Files\Azure\StorageSyncAgent\ServerRegistration.exe`.
2. Tıklayın *oturum* Azure aboneliğinize erişmek için. 

    ![Sunucu kaydı kullanıcı Arabirimi iletişim kutusu açma](media/storage-sync-files-server-registration/server-registration-ui-1.png)

3. Doğru abonelik, kaynak grubu ve depolama eşitleme hizmeti iletişim kutusundan seçin.

    ![Depolama eşitleme hizmeti bilgileri](media/storage-sync-files-server-registration/server-registration-ui-2.png)

4. Önizleme aşamasında olan bir daha fazla oturum açma işlemini tamamlamak için gereklidir. 

    ![İletişim kutusunda oturum](media/storage-sync-files-server-registration/server-registration-ui-3.png)

> [!Important]  
> Sunucu bir yük devretme kümesinin bir üyesi ise, her sunucu sunucu kaydını çalıştırmak gerekir. Kayıtlı sunucular Azure Portalı'nda görüntülediğinizde, Azure dosya eşitleme otomatik olarak her düğüm aynı yük devretme kümesinin bir üyesi olarak tanır ve bunları uygun şekilde gruplandıran.

#### <a name="register-the-server-with-powershell"></a>PowerShell ile sunucu kaydetme
Ayrıca, PowerShell aracılığıyla sunucu kaydı gerçekleştirebilirsiniz. Sunucu kaydı bulut çözümü sağlayıcısı (CSP) abonelikleri için desteklenen tek yol budur:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"
Login-AzStorageSync -SubscriptionID "<your-subscription-id>" -TenantID "<your-tenant-id>"
Register-AzStorageSyncServer -SubscriptionId "<your-subscription-id>" - ResourceGroupName "<your-resource-group-name>" - StorageSyncService "<your-storage-sync-service-name>"
```

### <a name="unregister-the-server-with-storage-sync-service"></a>Depolama eşitleme hizmeti ile sunucunun kaydını Kaldır
Depolama eşitleme hizmeti ile bir sunucu kaydını silmek için gereken birkaç adım vardır. Düzgün bir sunucunun kaydını nasıl bir göz atalım.

> [!Warning]  
> Kaydını ve bir sunucu kaydetme veya kaldırarak ve sunucu uç noktaları için bir Microsoft mühendisi tarafından açıkça belirtilmediği sürece yeniden eşitleme, bulut katmanlandırma veya herhangi bir Azure dosya eşitleme yönüyle sorunlarını giderme çalışmayın. Bir sunucu kaydını ve sunucu uç noktalarını kaldırarak zararlı bir işlemdir ve "kayıtlı sunucu ve sunucu uç noktaları sonra sunucu uç noktaları ile birimlerde katmanlı dosyalar için Azure dosya paylaşımında konumlarına bağlanır değil" yeniden, hangi eşitlenmiş hatalara neden olabilecek. Ayrıca unutmayın, bir sunucu uç noktası ad dışında var olan katmanlı dosyalar kalıcı olarak kaybolur. Katmanlı dosyaların bulunabilir Sunucusu'nda uç noktaları olsa bile bulut katmanlaması hiçbir zaman etkinleştirilmemiş.

#### <a name="optional-recall-all-tiered-data"></a>(İsteğe bağlı) Tüm katmanlı verileri geri çağırma
Azure dosya eşitleme (yani bu, bir üretim olmayan bir test, ortam) kaldırdıktan sonra kullanılabilir olması için şu anda katmanlı dosyalar isterseniz, sunucu uç noktaları içeren her bir birimdeki tüm dosyaları geri çağırma. Bulut katmanlaması tüm sunucu uç noktaları için devre dışı bırakın ve sonra aşağıdaki PowerShell cmdlet'ini çalıştırın:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <a-volume-with-server-endpoints-on-it>
```

> [!Warning]  
> Sunucu uç noktasını barındıran yerel birim katmanlı veriler tüm çağırmak için yeterli boş alana sahip değilse `Invoke-StorageSyncFileRecall` cmdlet başarısız olur.  

#### <a name="remove-the-server-from-all-sync-groups"></a>Sunucuyu tüm eşitleme gruplarından Kaldır
Depolama eşitleme hizmeti sunucuda kaydını önce bu sunucudaki tüm sunucu uç noktalarını kaldırılması gerekir. Bu, Azure portalı üzerinden yapılabilir:

1. Burada sunucunuzun kayıtlı depolama eşitleme hizmetine gidin.
2. Her bir eşitleme grubunda depolama eşitleme hizmetinde bu sunucu için tüm sunucu uç noktalarını kaldırın. Bu eşitleme grubunu bölmesinde ilgili sunucu uç noktasını sağ tıklayarak gerçekleştirilebilir.

    ![Sunucu uç noktası bir eşitleme grubundan kaldırılıyor](media/storage-sync-files-server-registration/sync-group-server-endpoint-remove-1.png)

Bu, basit bir PowerShell Betiği ile de gerçekleştirilebilir:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll"

$accountInfo = Connect-AzAccount
Login-AzStorageSync -SubscriptionId $accountInfo.Context.Subscription.Id -TenantId $accountInfo.Context.Tenant.Id -ResourceGroupName "<your-resource-group>"

$StorageSyncService = "<your-storage-sync-service>"

Get-AzStorageSyncGroup -StorageSyncServiceName $StorageSyncService | ForEach-Object { 
    $SyncGroup = $_; 
    Get-AzStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name | Where-Object { $_.DisplayName -eq $env:ComputerName } | ForEach-Object { 
        Remove-AzStorageSyncServerEndpoint -StorageSyncServiceName $StorageSyncService -SyncGroupName $SyncGroup.Name -ServerEndpointName $_.Name 
    } 
}
```

#### <a name="unregister-the-server"></a>Sunucunun kaydını Kaldır
Tüm veriler geri ve sunucuyu tüm eşitleme gruplarından kaldırılmıştır göre sunucu kaydı olabilir. 

1. Azure portalında gidin *kayıtlı sunucuları* depolama eşitleme hizmeti bölümü.
2. "Sunucu kaydını" kaydını silin ve istediğiniz sunucuda sağ tıklayın.

    ![Sunucunun kaydını kaldır](media/storage-sync-files-server-registration/unregister-server-1.png)

## <a name="ensuring-azure-file-sync-is-a-good-neighbor-in-your-datacenter"></a>Veri merkezinizde iyi bir komşu kalmasını sağlama Azure dosya eşitleme olduğundan 
Azure dosya eşitleme, veri merkezinde çalışan tek hizmet nadiren olacağından, Azure dosya eşitleme, ağ ve depolama kullanımını sınırlamak isteyebilirsiniz.

> [!Important]  
> Sınırları çok düşük ayarlanması, Azure dosya eşitleme eşitleme ve geri çağırma performansını etkiler.

### <a name="set-azure-file-sync-network-limits"></a>Azure dosya eşitleme ağ sınırlarını ayarlama
Kullanarak Azure dosya eşitleme ağ kullanımını daraltabilir `StorageSyncNetworkLimit` cmdlet'leri.

> [!Note]  
> Ağ sınırları katmanlanmış bir dosyanın erişilebilir veya Invoke-StorageSyncFileRecall cmdlet'i kullanıldığında geçerli değildir.

Örneğin, Azure dosya eşitleme 09: 00 ve 17: 00 (17:00 h) çalışma haftası boyunca arasında fazla 10 MB/sn kullanmadığından emin olmak için yeni bir kısıtlama sınırı oluşturabilirsiniz: 

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
New-StorageSyncNetworkLimit -Day Monday, Tuesday, Wednesday, Thursday, Friday -StartHour 9 -EndHour 17 -LimitKbps 10000
```

Aşağıdaki cmdlet'i kullanarak, sınırınızı görebilirsiniz:

```powershell
Get-StorageSyncNetworkLimit # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

Ağ sınırları kaldırmak için `Remove-StorageSyncNetworkLimit`. Örneğin, aşağıdaki komut, tüm ağ sınırları kaldırır:

```powershell
Get-StorageSyncNetworkLimit | ForEach-Object { Remove-StorageSyncNetworkLimit -Id $_.Id } # assumes StorageSync.Management.ServerCmdlets.dll is imported
```

### <a name="use-windows-server-storage-qos"></a>Windows Server depolama QoS kullanma 
Azure dosya eşitleme, bir sanal makinede çalışan bir Windows Server sanallaştırma konağında barındırıldığında, depolama g/ç tüketim düzenlemek için depolama hizmet kalitesi (depolama hizmet kalitesi) kullanabilirsiniz. Depolama QoS ilkesi, en fazla (veya sınırı StorageSyncNetwork sınırı üstünde nasıl zorlanır gibi) olarak ya da en az (veya ayırma) olarak ayarlanabilir. En fazla yerine en düşük ayarlanması, diğer iş yükleri kullanmıyorsanız, kullanılabilir depolama bant genişliği kullanılacak veri bloğu Azure dosya eşitleme sağlar. Daha fazla bilgi için [depolama hizmet kalitesi](https://docs.microsoft.com/windows-server/storage/storage-qos/storage-qos-overview).

## <a name="see-also"></a>Ayrıca bkz.
- [Bir Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md)
- [Azure dosya eşitleme işlemi dağıtma](storage-sync-files-deployment-guide.md)
- [Azure dosya eşitleme İzleyicisi](storage-sync-files-monitoring.md)
- [Azure dosya eşitleme sorunlarını giderme](storage-sync-files-troubleshoot.md)
