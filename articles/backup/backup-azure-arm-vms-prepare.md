---
title: Azure Backup'ı kullanarak Azure Vm'lerini bir kurtarma Hizmetleri kasasına yedekleme
description: Azure Backup'ı kullanarak Azure Vm'lerini bir kurtarma Hizmetleri kasasında yedekleme açıklar
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: raynew
ms.openlocfilehash: 3342b15511305ab337d9b5032080e205e36150d3
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/05/2019
ms.locfileid: "59049822"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Azure Vm'lerini bir kurtarma Hizmetleri kasasına yedekleme

Bu makalede kullanarak Azure sanal makinelerini bir kurtarma Hizmetleri kasasına yedeklemek nasıl [Azure Backup](backup-overview.md) hizmeti. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Destek ve bir yedekleme önkoşulları doğrulayın.
> * Azure sanal makineleri hazırlayın. Gerekirse Azure VM aracısını yükleyin ve VM'ler için giden erişim doğrulayın.
> * Bir kasa oluşturun.
> * Vm'leri bulmak ve yedekleme ilkesini yapılandırın.
> * Azure Vm'leri için yedeklemeyi etkinleştirin.


> [!NOTE]
> Bu makalede bir kasası ayarlama ve sanal makineleri yedeklemek için seçin. Birden çok sanal makinelerini yedekleme istiyorsanız kullanışlıdır. Alternatif olarak, [tek bir Azure VM yedekleme](backup-azure-vms-first-look-arm.md) VM ayarlarını doğrudan.

## <a name="before-you-start"></a>Başlamadan önce


- [Gözden geçirme](backup-architecture.md#architecture-direct-backup-of-azure-vms) Azure VM yedekleme mimarisi.
- [Hakkında bilgi edinin](backup-azure-vms-introduction.md) Azure VM yedeklemesi ve yedek Dahili hat.
- [Destek matrisi gözden](backup-support-matrix-iaas.md) Azure VM yedeklemesi için.


## <a name="prepare-azure-vms"></a>Azure sanal makineleri hazırlama

Bazı durumlarda, Azure vm'lerinde Azure VM aracısı ayarlayın veya açıkça bir VM üzerinde giden erişimi izin vermeniz gerekebilir.

### <a name="install-the-vm-agent"></a>VM aracısını yükleyin 

Azure yedekleme makine üzerinde çalışan Azure VM aracısı için bir uzantı yükleyerek Azure sanal makinelerini yedekler. Sanal makinenize bir Azure Market görüntüsünden oluşturulduysa, aracı yüklü ve çalışır durumdadır. Özel bir VM oluşturmak veya bir şirket içi makineyi geçirmek, aracıyı tabloda özetlendiği gibi el ile yüklemeniz gerekebilir.

**VM** | **Ayrıntılar**
--- | ---
**Windows** | 1. [İndirme ve yükleme](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) Aracısı MSI dosyası.<br/><br/> 2. Makine üzerinde yönetici izinlerine sahip yükleyin.<br/><br/> 3. Yüklemeyi doğrulayın. İçinde *C:\WindowsAzure\Packages* VM'de sağ **WaAppAgent.exe** > **özellikleri**. Üzerinde **ayrıntıları** sekmesinde **ürün sürümü** 2.6.1198.718 olmalıdır veya üzeri.<br/><br/> Aracıyı güncelleştiriyorsanız, hiçbir yedekleme işlemleri çalışır durumda olduğundan emin olun ve [aracıyı yeniden](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Dağıtımınıza ait bir paket deposundaki bir RPM veya DEB paketini kullanarak yükleyin. Bu, yükleme ve Azure Linux Aracısı yükseltme için tercih edilen yöntemdir. Tüm [dağıtım sağlayıcıları onaylı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure Linux Aracısı paketi, görüntüleri ve depoları tümleştirme. Aracı kullanılabilir [GitHub](https://github.com/Azure/WALinuxAgent), ancak buradan yükleme önermemekteyiz.<br/><br/> Aracıyı güncelleştiriyorsanız, hiçbir yedekleme işlemleri çalıştırıyorsanız ve ikili dosyalarını güncelleştir emin olun.


### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

VM'de çalışan yedekleme uzantısı, Azure genel IP adreslerine giden erişime izin gerekir.

Genellikle, Azure Backup ile iletişim kurabilmesi için bir Azure VM açıkça giden ağ erişim izni gerekmez.
Sanal makinelerinizin bağlanamıyorsa ve hatasını görürseniz **ExtensionSnapshotFailedNoNetwork**, erişim açıkça izin vermeniz. Backup uzantısı, ardından yedekleme trafiği için Azure genel IP adresleri ile iletişim kurabilir.


#### <a name="explicitly-allow-outbound-access"></a>Açıkça giden erişime izin ver

VM yedekleme hizmetine bağlanamıyor, tabloda özetlenen yöntemlerden birini kullanarak açıkça giden erişime izin.

**Seçenek** | **Eylem** | **Ayrıntılar** 
--- | --- | --- 
**NSG kurallarını ayarlayın** | İzin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). | İzin verme ve her bir adres aralığı yönetmek yerine kullanarak Azure Backup hizmetine erişimi veren bir ağ güvenlik grubu (NSG) kuralı ekleyebileceğiniz bir [hizmet etiketi](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure). [Daha fazla bilgi edinin](../virtual-network/security-overview.md#service-tags).<br/><br/> Ek maliyet olmadan vardır.<br/><br/> Hizmet etiketleri ile yönetmek basit kurallardır.
**Bir proxy dağıtma** | Bir HTTP proxy sunucusu için trafiği yönlendirme dağıtın. | Bu yöntem, yalnızca depolama ve Azure'nın tam erişim sağlar.<br/><br/> Depolama URL'leri üzerinde ayrıntılı denetim izin verilir.<br/><br/> VM'ler için bir tek noktası internet erişimi yoktur.<br/><br/> Bir ara sunucu için ek bir maliyeti yoktur.
**Azure güvenlik duvarını ayarlama** | Azure güvenlik duvarı üzerinden geçen trafik, Azure Backup hizmeti için bir FQDN etiketi kullanarak sanal makinede izin. |  Azure sanal ağ alt ağında ayarlanmış güvenlik duvarı varsa, kullanmak bu yöntem basittir.<br/><br/> Kendi FQDN etiketleri oluşturun veya FQDN'ler bir etiketi değiştirme.<br/><br/> Azure yönetilen diskler kullanıyorsanız, güvenlik duvarları hakkında ek bağlantı noktası açma (bağlantı noktası 8443) gerekebilir.

##### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Azure'da giden erişime izin vermek için bir NSG kuralı ayarlama

Bir NSG, VM erişimi yönetir ise gerekli aralıkları ve bağlantı noktaları için Yedekleme depolaması için giden erişime izin verir.

1. Sanal makine özellikleri > **ağ**seçin **giden bağlantı noktası kuralı Ekle**.
2. İçinde **Giden Güvenlik Kuralı Ekle**seçin **Gelişmiş**.
3. İçinde **kaynak**seçin **VirtualNetwork**.
4. İçinde **kaynak bağlantı noktası aralıkları**, herhangi bir bağlantı noktasından giden erişime izin vermek için yıldız işareti (*) girin.
5. İçinde **hedef**seçin **hizmet etiketi**. Listesinden **Storage.region**. Kasa ve yedeklemek istediğiniz VM'lerin bulunduğu bölgedir.
6. İçinde **hedef bağlantı noktası aralıkları**, bağlantı noktasını seçin.
    - Yönetilmeyen VM şifrelenmemiş bir depolama hesabı ile: 80
    - Şifrelenmiş depolama hesabında yönetilmeyen VM: 443 (varsayılan ayar)
    - Yönetilen sanal makine: 8443.
7. İçinde **Protokolü**seçin **TCP**.
8. İçinde **öncelik**, bir öncelik değeri belirtin değerinden daha da kuralları'nı engelle.
   
   Kural yüksek erişimini engellediği bir kuralı varsa, yeni izin verin. Örneğin, bir **Deny_All** kural öncelik, 1000, yeni bir kural kümesi için 1000'den daha az ayarlanmalıdır.
9. Bir ad ve kural için bir açıklama girin ve seçin **Tamam**.

Birden fazla VM'ye giden erişime izin vermek için NSG kuralı uygulayabilirsiniz. Bu videoda sürecinde yardımcı olur.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


##### <a name="route-backup-traffic-through-a-proxy"></a>Bir ara sunucu aracılığıyla yedekleme trafiği yönlendirme

Bir ara sunucu aracılığıyla yedekleme trafiği yönlendirmek ve ardından proxy erişimi için gerekli Azure aralıkları sağlar. VM aşağıdaki izin vermek için proxy yapılandırın:

- Azure VM için genel internet Ara sunucusu üzerinden bağlı tüm HTTP trafiğini yönlendirme.
- Proxy, geçerli bir sanal ağda Vm'lerden gelen trafiğe izin vermelidir.
- NSG **NSF kilitleme** Ara sunucuya VM giden internet trafiğine izin verecek bir kural gerekir.

###### <a name="set-up-the-proxy"></a>Proxy'sini ayarlama

Bir sistem hesabı proxy sahip değilseniz, birini aşağıdaki ayarlamaları yapın:

1. İndirme [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Çalıştırma **PsExec.exe -i -s cmd.exe** komut istemini bir sistem hesabı altında çalıştırılacak.
3. Tarayıcı sistem bağlamında çalışır. Örneğin, **%PROGRAMFILES%\Internet Explorer\iexplore.exe** Internet Explorer için.  
4. Proxy ayarlarını tanımlayın.
   - Linux makineleri üzerinde:
     - Bu satırı **/etc/ortam** dosyası:
       - **http_proxy = http:\//proxy IP adresi: proxy bağlantı noktası**
     - Bu satırları ekleyin **/etc/waagent.conf** dosyası:
         - **HttpProxy.Host=proxy IP adresi**
         - **HttpProxy.Port=proxy bağlantı noktası**
   - Windows makinelerinde, tarayıcı ayarlarınızı bir ara sunucu kullanılması gerektiğini belirtin. Bir kullanıcı hesabı şu anda bir ara sunucu kullanıyorsanız, sistem düzeyinde ayarları uygulamak için bu betiği kullanabilirsiniz.
       ```powershell
      $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
      $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
      Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

       ```

###### <a name="allow-incoming-connections-on-the-proxy"></a>Proxy gelen bağlantılara izin verin

Proxy ayarları gelen bağlantılara izin.

1. Windows Güvenlik Duvarı'nda açın **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
2. Sağ **gelen kuralları** > **yeni kural**.
3. İçinde **kural türü**seçin **özel** > **sonraki**.
4. İçinde **Program**seçin **tüm programlar** > **sonraki**.
5. İçinde **protokoller ve bağlantı noktaları**:
   - Tür kümesine **TCP**.
   - Ayarlama **yerel bağlantı noktaları** için **belirli bağlantı noktaları**.
   - Ayarlama **uzak bağlantı noktası** için **tüm bağlantı noktaları**.
  
6. Sihirbazı tamamlayın ve kural için bir ad belirtin.

###### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Bir özel durum kuralı için NSG için proxy ekleyin.

NSG üzerinde **NSF kilitleme**, 10.0.0.5 üzerinde herhangi bir bağlantı noktasından herhangi bir Internet adresi 80 (HTTP) veya 443 (HTTPS) bağlantı noktası üzerinde trafiğe izin vermek.

Aşağıdaki PowerShell betiğini trafiğe izin vermek için bir örnek sağlar.
Tüm ortak Internet adreslerine giden izin vermek yerine, bir IP adresi aralığı belirtebilirsiniz (`-DestinationPortRange`), veya storage.region hizmet etiketini kullanabilirsiniz.   

```powershell
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

### <a name="allow-firewall-access-by-using-an-fqdn-tag"></a>Bir FQDN etiketi kullanarak güvenlik duvarı erişime izin ver

Azure Güvenlik Duvarı'nı, ağ trafiği için Azure Backup giden erişime izin verecek şekilde ayarlayabilirsiniz.

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) Azure güvenlik duvarı dağıtılıyor.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN etiketler.

## <a name="modify-storage-replication-settings"></a>Depolama çoğaltma ayarlarını değiştirme

Varsayılan olarak, kasanız sahip [coğrafi olarak yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). GRS için birincil yedeklemenizi öneririz. Kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ucuz seçeneği.

Depolama çoğaltma türü şu şekilde değiştirin:

1. Portalda, yeni kasayı seçin. Altında **ayarları**seçin **özellikleri**.
2. İçinde **özellikleri**altında **yedekleme yapılandırması**seçin **güncelleştirme**.
3. Depolama çoğaltma türü seçip **Kaydet**.

![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/full-blade.png)


## <a name="configure-a-backup-policy"></a>Bir yedekleme ilkesi yapılandırma

Abonelikte Vm'leri bulmak ve yedekleme yapılandırın.

1. Kasadaki > **genel bakış**seçin **+ yedekleme**.

   ![Yedekleme düğmesi](./media/backup-azure-arm-vms-prepare/backup-button.png)

   **Yedekleme** ve **yedekleme hedefi** bölmeleri açın.

2. İçinde **yedekleme hedefi** > **iş yükünüz çalıştığı?** seçin **Azure**. İçinde **neleri yedeklemek istiyorsunuz?** seçin **sanal makine** >  **Tamam**.

   ![Yedekleme ve yedekleme hedefi bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Bu adım, VM uzantısını kasa ile kaydeder. **Yedekleme hedefi** bölmeyi kapatır ve **yedekleme İlkesi** bölmesi açılır.

3. İçinde **yedekleme İlkesi**, kasa ile ilişkilendirmek istediğiniz ilkeyi seçin. Sonra **Tamam**’ı seçin.
    - Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir.
    - Seçin **Yeni Oluştur** bir ilke oluşturmak için. [Daha fazla bilgi edinin](backup-azure-arm-vms-prepare.md#configure-a-backup-policy) ilke tanımlama.

    !["Yedekleme" ve "Yedekleme İlkesi" bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. İçinde **sanal makineleri** bölmesinde, belirtilen yedekleme İlkesi kullanan VM'ler seçin > **Tamam**.

   Seçili VM doğrulanır. Kasayla aynı bölgede VM'ler yalnızca seçebilirsiniz. VM'ler yalnızca tek bir kasada yedeklenebilecek.

   !["Sanal makineleri seçin" bölmesi](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. İçinde **yedekleme**seçin **yedeklemeyi etkinleştir**.

   Bu adım, ilkeyi kasaya ve vm'lere dağıtır. Ayrıca Azure VM'de çalışan VM aracısı üzerinde yedekleme uzantısını yükler.
   
   Bu adım, sanal makine için ilk kurtarma noktasını oluşturmaz.

   !["Yedeklemeyi etkinleştir" düğmesi](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Sonra yedeklemeyi etkinleştir:

- İlk yedekleme, yedekleme planınızın çalıştırır.
- VM'nin çalışır durumda olup olmadığını Backup hizmeti yedekleme uzantısını yükler.

Çalışan bir VM, uygulamayla tutarlı bir kurtarma noktası alınma olasılığını en yükseğe çıkarır. Ancak, sanal makine kapalı ve uzantı yüklenemiyor olsa bile yedeklenir. Bu bir çevrimdışı VM adı verilir. Bu durumda, kurtarma noktası kilitlenme tutarlı olur.
    
> [!NOTE]
> Azure Backup, Azure VM yedeklemeleri için ışığından değişikliklerin otomatik saat ayarlama desteklememektedir. Yedekleme ilkelerini el ile gerektiği gibi değiştirin.

## <a name="run-the-initial-backup"></a>İlk yedeklemeyi çalıştırın

El ile bunu hemen çalıştırmanız sürece zamanlamaya uygun olarak ilk yedekleme çalışır. Elle çalıştırın gibidir:

1. Kasa menüde **yedekleme öğeleri**.
2. İçinde **yedekleme öğeleri**seçin **Azure sanal makine**.
3. İçinde **yedekleme öğeleri** listesinde, üç noktayı seçin (**...** ).
4. Seçin **Şimdi Yedekle**.
5. İçinde **Şimdi Yedekle**, kurtarma noktası korunması gereken son günü seçmek için takvim denetimini kullanın > **Tamam**.
6. Portal bildirimlerini izleyin. Kasa panosunda iş ilerleme durumunu izleyebilirsiniz > **yedekleme işleri** > **sürüyor**. Sanal makinenizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.



## <a name="next-steps"></a>Sonraki adımlar

- Herhangi bir sorunu gidermeye [Azure VM aracıları](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) veya [Azure VM yedeklemesi](backup-azure-vms-troubleshoot.md).
- [Geri yükleme](backup-azure-arm-restore-vms.md) Azure Vm'leri.

