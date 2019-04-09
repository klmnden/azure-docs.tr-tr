---
title: Azure sanal makinelerini Azure Backup'ı kullanarak bir kurtarma Hizmetleri kasasına yedekleme
description: Azure sanal makinelerini Azure Backup'ı kullanarak bir kurtarma Hizmetleri kasasında yedekleme açıklar
service: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 04/03/2019
ms.author: raynew
ms.openlocfilehash: 142ffdadf4adb1ee07f3592624cbdddfb310b580
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59264565"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Azure Vm'lerini bir kurtarma Hizmetleri kasasına yedekleme

Bu makalede Azure Vm'lerini bir kurtarma Hizmetleri kasasında yedekleme işlemini kullanarak [Azure Backup](backup-overview.md) hizmeti. 

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure sanal makineleri hazırlayın.
> * Bir kasa oluşturun.
> * Vm'leri bulmak ve yedekleme ilkesini yapılandırın.
> * Azure Vm'leri için yedeklemeyi etkinleştirin.
> * İlk yedeklemeyi çalıştırın.


> [!NOTE]
> Bu makalede bir kasası ayarlama ve sanal makineleri yedeklemek için seçin. Birden çok sanal makinelerini yedekleme istiyorsanız kullanışlıdır. Alternatif olarak, [tek bir Azure VM yedekleme](backup-azure-vms-first-look-arm.md) VM ayarlarını doğrudan.

## <a name="before-you-start"></a>Başlamadan önce


- [Gözden geçirme](backup-architecture.md#architecture-direct-backup-of-azure-vms) Azure VM yedekleme mimarisi.
- [Hakkında bilgi edinin](backup-azure-vms-introduction.md) Azure VM yedeklemesi ve yedek Dahili hat.
- [Destek matrisi gözden](backup-support-matrix-iaas.md) yedekleme yapılandırmadan önce.

Ayrıca, birkaç bazı durumlarda yapmanız gerekebilecek şey vardır:

- **VM Aracısı VM üzerinde yükleme**: Azure yedekleme makine üzerinde çalışan Azure VM aracısı için bir uzantı yükleyerek Azure sanal makinelerini yedekler. Sanal makinenize bir Azure Market görüntüsünden oluşturulduysa, aracı yüklü ve çalışır durumdadır. Özel bir VM oluşturmak veya bir şirket içi makineyi geçirmek gerekebilir [aracıyı el ile yükleme](#install-the-vm-agent).
- **Açıkça giden erişime izin**: Genel olarak, giden ağ erişimi için Azure Backup ile iletişim kurmak bir Azure VM sırayla açıkça izin vermeniz gerekmez. Gösteren bazı VM'ler bağlantı sorunları, ancak karşılaşabilirsiniz **ExtensionSnapshotFailedNoNetwork** bağlanmaya çalışılırken bir hata oluştu. Böyle bir durumda, gereken [açıkça giden erişime izin](#explicitly-allow-outbound-access), Azure Backup uzantısı yedekleme trafiği için Azure genel IP adresleri ile iletişim kurabilirsiniz.


## <a name="create-a-vault"></a>Kasa oluşturma

 Kasa yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan ve yedeklenen makine ile ilişkili yedekleme ilkeleri depolar. Şu şekilde bir kasa oluşturun:    

1. [Azure Portal](https://portal.azure.com/) oturum açın.    
2. Arama alanına yazın **kurtarma Hizmetleri**. Altında **Hizmetleri**, tıklayın **kurtarma Hizmetleri kasaları**.   

     ![Kurtarma Hizmetleri kasaları arayın](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/> 

3. İçinde **kurtarma Hizmetleri kasaları** menüsünü tıklatın **+ Ekle**.    

     ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)   

4. İçinde **kurtarma Hizmetleri kasası**, kasayı tanımlamak için bir kolay ad yazın.   
    - Adın Azure aboneliği için benzersiz olması gerekir.   
    - Bu, 2 ile 50 karakter içerebilir.    
    - Bir harf ile başlamalı ve yalnızca harf, rakam ve kısa çizgi içerebilir.   
5. Azure aboneliği, kaynak grubu ve kasa oluşturulması gereken coğrafi bölgeyi seçin. Sonra **Oluştur**’a tıklayın.    
    - Bu, kasasının oluşturulması biraz zaman alabilir.  
    - Portalın sağ üst alandaki durum bildirimlerini izleyin.   


 Kasayı oluşturduktan sonra kurtarma Hizmetleri kasaları listesinde görünür. Kasanız görmüyorsanız seçin **Yenile**.
 
![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)    

### <a name="modify-storage-replication"></a>Depolama çoğaltma değiştirme

Varsayılan olarak, kullanım kasaları [coğrafi olarak yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs).

- Kasa birincil yedekleme yönteminiz varsa, GRS kullanmanızı öneririz.
- Kullanabileceğiniz [yerel olarak yedekli depolama (LRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ucuz seçeneği.

Depolama çoğaltma türü şu şekilde değiştirin:

1. Yeni kasaya tıklayın **özellikleri** içinde **ayarları** bölümü.
2. İçinde **özellikleri**altında **yedekleme yapılandırması**, tıklayın **güncelleştirme**.
3. Depolama çoğaltma türü seçin ve tıklayın **Kaydet**.

      ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/full-blade.png)
> [!NOTE]
   > Kasa ayarlanır ve yedekleme öğeleri içeren sonra depolama çoğaltma türü değiştirilemiyor. Bunu yapmak istiyorsanız kasaya yeniden oluşturmanız gerekir. 

## <a name="apply-a-backup-policy"></a>Yedekleme ilkesini uygulama

Kasa için bir yedekleme ilkesi yapılandırın.

1. Kasaya tıklayın **+ yedekleme** içinde **genel bakış** bölümü.

   ![Yedekleme düğmesi](./media/backup-azure-arm-vms-prepare/backup-button.png)


2. İçinde **yedekleme hedefi** > **iş yükünüz çalıştığı?** seçin **Azure**. İçinde **neleri yedeklemek istiyorsunuz?** seçin **sanal makine** >  **Tamam**. Bu kasada VM uzantısı kaydeder.

   ![Yedekleme ve yedekleme hedefi bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. İçinde **yedekleme İlkesi**, kasa ile ilişkilendirmek istediğiniz ilkeyi seçin. 
    - Varsayılan ilke günde bir kez VM'yi yedekler. Günlük yedekleri 30 gün boyunca saklanır. Anında kurtarma anlık görüntüleri iki gün boyunca saklanır.
    - Varsayılan ilkeyi kullanmak istemiyorsanız seçin **Yeni Oluştur**ve bir sonraki yordamda açıklandığı gibi özel bir ilke oluşturun.

      ![Varsayılan yedekleme İlkesi](./media/backup-azure-arm-vms-prepare/default-policy.png)

4. İçinde **sanal makineleri**, ilkeyi kullanan yedeklemek istediğiniz Vm'leri seçin. Daha sonra, **Tamam**'a tıklayın.

   - Seçili Vm'lerden doğrulanır.
   - Kasayla aynı bölgede VM'ler yalnızca seçebilirsiniz.
   - VM'ler yalnızca tek bir kasada yedeklenebilecek.

     !["Sanal makineleri seçin" bölmesi](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. İçinde **yedekleme**, tıklayın **yedeklemeyi etkinleştir**. Bu, ilkeyi kasaya ve vm'lere dağıtır ve Azure sanal makinesinde çalışan VM Aracısı yedekleme uzantısını yükler.
     
     !["Yedeklemeyi etkinleştir" düğmesi](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Backup'ı etkinleştirdikten sonra:

- VM'nin çalışır durumda olup olmadığını Backup hizmeti yedekleme uzantısını yükler.
- İlk yedekleme, yedekleme planınızın çalıştırılır.
- Yedeklemeleri çalıştırdığınızda, dikkat edin:
    - Çalıştıran bir VM sahip bir uygulamayla tutarlı kurtarma noktası yakalamak için olasılığını en yükseğe çıkarır.
    - VM kapalı olsa bile ancak bunu yedeklenir. Böyle bir VM, bir çevrimdışı VM adı verilir. Bu durumda, kurtarma noktası kilitlenme tutarlı olur.
    

### <a name="create-a-custom-policy"></a>Özel ilke oluşturma

Yeni bir yedekleme ilkesi oluşturmak için seçtiyseniz, ilke ayarlarını doldurun.

1. İçinde **ilke adı**, anlamlı bir ad belirtin.
2. İçinde **yedekleme zamanlaması** yedeklemeleri ne zaman alınması gerektiğini belirtin. Azure Vm'leri için günlük veya haftalık yedeklemeler alabilir.
2. İçinde **anında geri yükleme**, anlık geri yükleme için yerel anlık görüntüleri tutmak istediğiniz süreyi belirtin.
    - Geri yüklendiğinde, yedeklenen VM'yi yedekleme, Kurtarma depolama konumuna ağda, depolama alanından disk kopyalanır. Anında geri yükleme ile bir yedekleme işi sırasında yedekleme verileri kasaya aktarılacak beklemeden alınan yerel olarak depolanan anlık görüntülere yararlanabilirsiniz.
    - Bir ile beş gün arasında anında geri yükleme için anlık görüntüleri tutabilirsiniz. Varsayılan ayar iki gündür.
3. İçinde **bekletme aralığı**, günlük veya haftalık yedekleme noktaları saklamak istediğiniz süreyi belirtin.
4. İçinde **aylık yedekleme noktası bekletme**, aylık, günlük veya haftalık yedekleri yedekleme tutmak isteyip istemediğinizi belirtin. 
5. Tıklayın **Tamam** ilkeyi kaydedin.

    ![Yeni bir yedekleme İlkesi](./media/backup-azure-arm-vms-prepare/new-policy.png)

> [!NOTE]
   > Azure Backup, Azure VM yedeklemeleri için ışığından değişikliklerin otomatik saat ayarlama desteklememektedir. Zaman değişiklikler oldukça, yedekleme ilkeleri el ile gerektiği gibi değiştirin.

## <a name="trigger-the-initial-backup"></a>İlk yedeklemeyi tetikleme

İlk yedeklemeyi zamanlamaya uygun olarak çalışır, ancak bunu hemen aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüden **yedekleme öğeleri**.
2. İçinde **yedekleme öğeleri** tıklayın **Azure sanal makine**.
3. İçinde **yedekleme öğeleri** listesinde, üç nokta (...) tıklayın.
4. Tıklayın **Şimdi Yedekle**.
5. İçinde **Şimdi Yedekle**, kurtarma noktası korunması gereken son günü seçmek için takvim denetimini kullanın. Daha sonra, **Tamam**'a tıklayın.
6. Portal bildirimlerini izleyin. Kasa panosunda iş ilerleme durumunu izleyebilirsiniz > **yedekleme işleri** > **sürüyor**. VM’nizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.

## <a name="optional-steps-install-agentallow-outbound"></a>İsteğe bağlı adımlar (Yükleme aracı/izin Giden)
### <a name="install-the-vm-agent"></a>VM aracısını yükleyin

Azure yedekleme makine üzerinde çalışan Azure VM aracısı için bir uzantı yükleyerek Azure sanal makinelerini yedekler. Sanal makinenize bir Azure Market görüntüsünden oluşturulduysa, aracı yüklü ve çalışır durumdadır. Özel bir VM oluşturmak veya bir şirket içi makineyi geçirmek, aracıyı tabloda özetlendiği gibi el ile yüklemeniz gerekebilir.

**VM** | **Ayrıntılar**
--- | ---
**Windows** | 1. [İndirme ve yükleme](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) Aracısı MSI dosyası.<br/><br/> 2. Makine üzerinde yönetici izinlerine sahip yükleyin.<br/><br/> 3. Yüklemeyi doğrulayın. İçinde *C:\WindowsAzure\Packages* VM'de sağ **WaAppAgent.exe** > **özellikleri**. Üzerinde **ayrıntıları** sekmesinde **ürün sürümü** 2.6.1198.718 olmalıdır veya üzeri.<br/><br/> Aracıyı güncelleştiriyorsanız, hiçbir yedekleme işlemleri çalışır durumda olduğundan emin olun ve [aracıyı yeniden](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux** | Dağıtımınıza ait bir paket deposundaki bir RPM veya DEB paketini kullanarak yükleyin. Bu, yükleme ve Azure Linux Aracısı yükseltme için tercih edilen yöntemdir. Tüm [dağıtım sağlayıcıları onaylı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure Linux Aracısı paketi, görüntüleri ve depoları tümleştirme. Aracı kullanılabilir [GitHub](https://github.com/Azure/WALinuxAgent), ancak buradan yükleme önermemekteyiz.<br/><br/> Aracıyı güncelleştiriyorsanız, hiçbir yedekleme işlemleri çalıştırıyorsanız ve ikili dosyalarını güncelleştir emin olun.

### <a name="explicitly-allow-outbound-access"></a>Açıkça giden erişime izin ver

VM'de çalışan yedekleme uzantısı, Azure genel IP adreslerine giden erişime izin gerekir.

- Genellikle giden ağ erişimi için Azure Backup ile iletişim kurmak bir Azure VM sırayla açıkça izin vermeniz gerekmez.
- Sorunlar bağlanma ile Vm'leri çalıştırma veya hata görürseniz **ExtensionSnapshotFailedNoNetwork** yedekleme uzantısını Azure genel IP için iletişim kurabilmesi bağlanmaya çalışılırken açıkça erişim izin vermeniz Yedekleme trafiği adresleridir. Erişim yöntemleri aşağıdaki tabloda özetlenmiştir.


**Seçenek** | **Eylem** | **Ayrıntılar** 
--- | --- | --- 
**NSG kurallarını ayarlayın** | İzin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).<br/><br/> İzin verme ve her bir adres aralığı yönetmek yerine Azure Backup hizmetini kullanarak erişimine izin verecek bir kural ekleyebileceğiniz bir [hizmet etiketi](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure). | [Daha fazla bilgi edinin](../virtual-network/security-overview.md#service-tags) hizmet etiketleri hakkında.<br/><br/> Hizmet etiketleri erişim yönetimini basitleştirin ve ek ücret uygulanmaz.
**Bir proxy dağıtma** | Bir HTTP proxy sunucusu için trafiği yönlendirme dağıtın. | Yalnızca depolama ve Azure'nın tam erişim sağlar.<br/><br/> Depolama URL'leri üzerinde ayrıntılı denetim izin verilir.<br/><br/> VM'ler için tek noktası internet erişimi.<br/><br/> Ek maliyet proxy.
**Azure güvenlik duvarını ayarlama** | Azure Backup hizmeti için bir FQDN etiketi kullanarak VM, Azure güvenlik duvarı üzerinden trafiğine izin verin | Azure sanal ağ içindeki alt ağ ayarlama güvenlik duvarı varsa kullanımı kolaydır.<br/><br/> Kendi FQDN etiketleri oluşturun veya FQDN'ler bir etiketi değiştirme.<br/><br/> Azure sanal makinelerinize yönetilen disklere sahipseniz, ek bir açmanız gerekebilir (8443) üzerinde güvenlik duvarları bağlantı noktası.

#### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

Proxy veya güvenlik duvarı üzerinden ile NSG, bağlantı kurun

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

##### <a name="allow-firewall-access-with-an-fqdn-tag"></a>Bir FQDN etiketi ile güvenlik duvarı erişime izin ver

Azure Güvenlik Duvarı'nı, ağ trafiği için Azure Backup giden erişime izin verecek şekilde ayarlayabilirsiniz.

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) Azure güvenlik duvarı dağıtılıyor.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN etiketler.



## <a name="next-steps"></a>Sonraki adımlar

- Herhangi bir sorunu gidermeye [Azure VM aracıları](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) veya [Azure VM yedeklemesi](backup-azure-vms-troubleshoot.md).
- [Geri yükleme](backup-azure-arm-restore-vms.md) Azure Vm'leri.

