---
title: Azure Vm'lerini bir kurtarma Hizmetleri kasasına Azure Backup ile yedekleme
description: Azure Vm'lerini bir kurtarma Hizmetleri kasasına Azure Backup ile yedekleme açıklar
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: c6d6e380cded18a089f624f90d998477a89293be
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259050"
---
# <a name="back-up-azure-vms-in-a-recovery-services-vault"></a>Azure Vm'lerini bir kurtarma Hizmetleri kasasına yedekleme

Bu makalede Azure VM kullanarak yedeklemek nasıl bir [Azure Backup](backup-overview.md) dağıtarak ve bir kurtarma Hizmetleri kasasında yedeklemeyi etkinleştirme.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Desteklenen senaryolar ve önkoşulları doğrulayın.
> * Azure sanal makineleri hazırlayın. Gerekirse Azure VM aracısını yükleyin ve VM'ler için giden erişim doğrulayın.
> * Bir kasa oluşturun.
> * Kasa için depolama ayarlama
> * Vm'leri bulmak, yedekleme ayarları ve ilkesi yapılandırın.
> * Azure Vm'leri için yedeklemeyi etkinleştirme


> [!NOTE]
   > Bu makalede, bir kasası ayarlama ve yedeklenecek VM'ler seçerek Azure Vm'lerini yedekleme açıklar. Birden çok sanal makinelerini yedekleme istiyorsanız kullanışlıdır. Ayrıca [Azure VM yedekleme](backup-azure-vms-first-look-arm.md) VM ayarlarını doğrudan.

## <a name="before-you-start"></a>Başlamadan önce

Azure yedekleme makine üzerinde çalışan Azure VM aracısı için bir uzantı yükleyerek Azure sanal makinelerini yedekler.

1. [Gözden geçirme](backup-architecture.md#architecture-direct-backup-of-azure-vms) Azure VM yedekleme mimarisi.
[Hakkında bilgi edinin](backup-azure-vms-introduction.md) Azure VM yedeklemesi ve yedek Dahili hat.
2. [Destek matrisi gözden](backup-support-matrix-iaas.md) Azure VM yedeklemesi için.
3. Azure sanal makineleri hazırlayın. VM aracısı yüklü değilse yükleyin ve yedeklemek istediğiniz VM'ler için giden erişimini doğrulama.


## <a name="prepare-azure-vms"></a>Azure sanal makineleri hazırlama

Gerekirse VM aracısını yükleyin ve vm'lerden giden erişimini doğrulayın.

### <a name="install-the-vm-agent"></a>VM aracısını yükleyin
Gerekli olursa, aracıyı şu şekilde yükleyin.

**VM** | **Ayrıntılar**
--- | ---
**Windows VM’leri** | [İndirme ve yükleme](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) Aracısı MSI dosyası. Makine üzerinde yönetici izinlerine sahip yükleyin.<br/><br/> Yüklemeyi doğrulamak için *C:\WindowsAzure\Packages* WaAppAgent.exe VM'ye sağ tıklayın > **özellikleri**, > **ayrıntıları** sekmesi. **Ürün sürümü** 2.6.1198.718 olmalıdır veya üzeri.<br/><br/> Aracıyı güncelleştiriyorsanız, hiçbir yedekleme işlemleri çalıştırdığınızdan emin olun ve [aracıyı yeniden](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409).
**Linux VM'leri** | Dağıtımınıza ait bir paket deposundaki bir RPM veya DEB paketini kullanarak yükleme, Azure Linux Aracısı yükseltme ve yükleme için tercih edilen yöntemdir. Tüm [dağıtım sağlayıcıları onaylı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure Linux Aracısı paketi, görüntüleri ve depoları tümleştirme. Aracı kullanılabilir [GitHub](https://github.com/Azure/WALinuxAgent), ancak buradan yükleme önermemekteyiz.<br/><br/> Aracıyı güncelleştiriyorsanız, herhangi bir yedekleme işleminin çalıştırır ve güncelleştirme ikili dosyaları emin olun.


### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

VM'de çalışan yedekleme uzantısı, Azure genel IP adreslerine giden erişime izin olmalıdır.

- Azure Backup hizmeti ile iletişim kurmak için Azure sanal makinesi için hiçbir açık giden ağ erişimi gereklidir.
- Bazı eski sanal makineler ancak ve karşılaştığınız sorunlar şu hatayla başarısız **ExtensionSnapshotFailedNoNetwork** bağlanmaya çalışırken. Bu durumda, Azure genel IP adreslerine trafiği yedekleme için yedekleme uzantısı iletişim kurabilmesi için aşağıdaki seçeneklerden birini kullanın.

   **Seçenek** | **Eylem** | **Avantajları** | **Dezavantajları**
   --- | --- | --- | ---
   **NSG kurallarını ayarlayın** | İzin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653).<br/><br/>  Azure Backup hizmetini kullanarak erişimine izin verecek bir kural ekleyebileceğiniz bir [hizmet etiketi](backup-azure-arm-vms-prepare.md#set-up-an-nsg-rule-to-allow-outbound-access-to-azure), tek tek izin verme ve her bir adres aralığı yönetmek yerine. [Daha fazla bilgi edinin](../virtual-network/security-overview.md#service-tags) hizmet etiketleri hakkında. | Ek maliyet olmadan. Hizmet etiketleri ile yönetmek basit
   **Bir proxy dağıtma** | Bir HTTP proxy sunucusu için trafiği yönlendirme dağıtın. | Yalnızca depolama ve Azure'nın tam erişim sağlar. Depolama URL'leri üzerinde ayrıntılı denetim izin verilir.<br/><br/> VM'ler için tek noktası internet erişimi.<br/><br/> Ek maliyet proxy.<br/><br/>
   **Azure güvenlik duvarını ayarlama** | Azure Backup hizmeti için bir FQDN etiketi kullanarak VM, Azure güvenlik duvarı üzerinden trafiğine izin verin.|  Azure sanal ağ içindeki alt ağ ayarlama güvenlik duvarı varsa kullanımı kolaydır | Kendi FQDN etiketleri oluşturun veya FQDN'ler bir etiketi değiştirme.<br/><br/> Azure yönetilen diskler kullanıyorsanız, güvenlik duvarları hakkında ek bağlantı noktası açma (bağlantı noktası 8443) gerekebilir.

#### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Azure'da giden erişime izin vermek için bir NSG kuralı ayarlama

Azure VM bir NSG tarafından yönetilen erişimi varsa, gerekli aralıkları ve bağlantı noktaları için Yedekleme depolaması için giden erişim verin.

1. VM > **ağ**, tıklayın **giden bağlantı noktası kuralı Ekle**.
2. İçinde **Giden Güvenlik Kuralı Ekle**, tıklayın **Gelişmiş**.
3. İçinde **kaynak**seçin **VirtualNetwork**.
4. İçinde **kaynak bağlantı noktası aralıkları**, herhangi bir bağlantı noktasından giden erişime izin vermek için yıldız işareti (*) yazın.
5. İçinde **hedef**seçin **hizmet etiketi**. Listesinden **Storage.region**. Bölge, kasa ve yedeklemek istediğiniz VM'lerin bulunduğu bölgedir.
6. İçinde **hedef bağlantı noktası aralıkları**, bağlantı noktasını seçin.
    - Yönetilmeyen VM şifrelenmemiş bir depolama hesabı ile: 80
    - Şifrelenmiş depolama hesabında yönetilmeyen VM: 443 (varsayılan ayar)
    - Yönetilen sanal makine: 8443.
7. İçinde **Protokolü**seçin **TCP**.
8. İçinde **öncelik**, bir öncelik değeri belirtin değerinden daha da kuralları'nı engelle. Kuralın daha yüksek olması, erişimi reddettikten bir kuralı varsa, yeni izin verin. Örneğin, bir **Deny_All** kural öncelik, 1000, yeni bir kural kümesi için 1000'den daha az ayarlanmalıdır.
9. Bir ad ve kural için bir açıklama girin ve tıklatın **Tamam**.

Birden fazla VM'ye giden erişime izin vermek için NSG kuralı uygulayabilirsiniz.

Bu videoda sürecinde yardımcı olur.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]


#### <a name="route-backup-traffic-through-a-proxy"></a>Bir ara sunucu aracılığıyla yedekleme trafiği yönlendirme

Bir ara sunucu aracılığıyla yedekleme trafiği yönlendirmek ve ardından proxy erişimi için gerekli Azure aralıkları sağlar.
Aşağıdaki izin vermek için VM proxy yapılandırmanız gerekir:

- Azure VM için genel internet Ara sunucusu üzerinden bağlı tüm HTTP trafiğini yönlendirme.
- Proxy, geçerli sanal ağ (VNet) içinde Vm'lerden gelen trafiğe izin vermelidir.
- NSG **NSF kilitleme** Ara sunucuya VM giden internet trafiğine izin verecek bir kural gerekir.

##### <a name="set-up-the-proxy"></a>Proxy'sini ayarlama
Bir sistem hesabı proxy sahip değilseniz, birini aşağıdaki ayarlamaları yapın:

1. İndirme [PsExec](https://technet.microsoft.com/sysinternals/bb897553).

2. Çalıştırma **PsExec.exe -i -s cmd.exe** komut istemini bir sistem hesabı altında çalıştırılacak.
3. Tarayıcı sistem bağlamında çalışır. Örneğin: **%PROGRAMFILES%\Internet Explorer\iexplore.exe** Internet Explorer için.  
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

##### <a name="allow-incoming-connections-on-the-proxy"></a>Proxy gelen bağlantılara izin verin

Proxy ayarları gelen bağlantılara izin.

- Örneğin, açmak **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
    - Sağ **gelen kuralları** > **yeni kural**.
    - İçinde **kural türü** seçin **özel** > **sonraki**.
    - İçinde **Program**seçin **tüm programlar** > **sonraki**.
    - İçinde **protokoller ve bağlantı noktaları** türünü ayarlayın **TCP**, **yerel bağlantı noktaları** için **belirli bağlantı noktaları**, ve **uzak bağlantı noktası**için **tüm bağlantı noktaları**.
    - Sihirbazı tamamlayın ve kural için bir ad belirtin.

##### <a name="add-an-exception-rule-to-the-nsg-for-the-proxy"></a>Bir özel durum kuralı için NSG için proxy ekleyin.

NSG üzerinde **NSF kilitleme**, 10.0.0.5 üzerinde herhangi bir bağlantı noktasından herhangi bir Internet adresi 80 (HTTP) veya 443 (HTTPS) bağlantı noktası üzerinde trafiğe izin vermek.

- Aşağıdaki PowerShell betiğini trafiğe izin vermek için bir örnek sağlar.
- Tüm ortak Internet adreslerine giden izin vermek yerine, bir IP adresi aralığı belirtebilirsiniz (-DestinationPortRange), veya storage.region hizmet etiketini kullanabilirsiniz.   

    ```powershell
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```

### <a name="allow-firewall-access-with-fqdn-tag"></a>FQDN etiketi ile güvenlik duvarı erişime izin ver

Ağ trafiği için Azure Backup giden erişime izin vermek için Azure güvenlik duvarını ayarlayabilirsiniz.

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) Azure güvenlik duvarı dağıtılıyor.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN etiketler.

## <a name="set-up-storage-replication"></a>Depolama çoğaltmayı ayarlama

Varsayılan olarak, kasanız sahip [coğrafi olarak yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). GRS için birincil yedeklemenizi öneririz, ancak kullanabileceğiniz[yerel olarak yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ucuz seçeneği.

Azure yedekleme kasası için depolama otomatik olarak işler. Bu depolamanın nasıl çoğaltılacağını belirtmenize gerek.
Depolama çoğaltma şu şekilde değiştirin:

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın. Altında **ayarları** bölümünde **özellikleri**.
2. İçinde **özellikleri**altında **yedekleme yapılandırması**, tıklayın **güncelleştirme**.

3. Depolama çoğaltma türü seçin ve tıklayın **Kaydet**.

      ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/full-blade.png)


## <a name="configure-a-backup-policy"></a>Bir yedekleme ilkesi yapılandırma

Abonelikte Vm'leri bulmak ve yedekleme yapılandırın.

1. Kasadaki > **genel bakış**, tıklayın **+ yedekleme**

   ![Yedekleme düğmesi](./media/backup-azure-arm-vms-prepare/backup-button.png)

   **Yedekleme** ve **yedekleme hedefi** bölmeleri açın.

2. İçinde **yedekleme hedefi**> **iş yükünüz çalıştığı?** seçin **Azure**. İçinde **neleri yedeklemek istiyorsunuz?** seçin **sanal makine** >  **Tamam**. Bu kasada VM uzantısı kaydeder.

   ![Yedekleme ve yedekleme hedefi bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Bu adım, VM uzantısını kasa ile kaydeder. **Yedekleme hedefi** bölmeyi kapatır ve **yedekleme İlkesi** bölmesi açılır.

3. İçinde **yedekleme İlkesi**, kasa ile ilişkilendirmek istediğiniz ilkeyi seçin. Daha sonra, **Tamam**'a tıklayın.
    - Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir.
    - Tıklayın **Yeni Oluştur** bir ilke oluşturmak için. [Daha fazla bilgi edinin](backup-azure-arm-vms-prepare.md#configure-a-backup-policy) ilke tanımlama.

      !["Yedekleme" ve "Yedekleme İlkesi" bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)

4. İçinde **sanal makineleri** bölmesinde, belirtilen yedekleme İlkesi kullanan VM'ler seçin > **Tamam**.

   - Seçili VM doğrulanır.
   - Kasayla aynı bölgede VM'ler yalnızca seçebilirsiniz. VM'ler yalnızca tek bir kasada yedeklenebilecek.

     !["Sanal makineleri seçin" bölmesi](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

5. İçinde **yedekleme**seçin **yedeklemeyi etkinleştir**.

   - Bu, ilkeyi kasaya ve vm'lere dağıtır ve Azure sanal makinesinde çalışan VM Aracısı yedekleme uzantısını yükler.
   - Bu adım, sanal makine için ilk kurtarma noktasını oluşturmaz.

     !["Yedeklemeyi etkinleştir" düğmesi](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Backup'ı etkinleştirdikten sonra:

- İlk yedekleme, yedekleme planınızın çalıştırır.
- VM'nin çalışır durumda olup olmadığını Backup hizmeti yedekleme uzantısını yükler.
    - Çalışan bir VM, uygulamayla tutarlı bir kurtarma noktası alınma olasılığını en yükseğe çıkarır.
    -  Ancak, sanal makine kapalı ve uzantı yüklenemiyor olsa bile yedeklenir. Bu olarak bilinir *çevrimdışı VM*. Bu durumda, kurtarma noktası olur *kilitlenmeyle tutarlı* olacaktır.
    Azure yedekleme Azure VM yedeklemeleri için ışığından değişikliklerin otomatik saat ayarlama desteklemediğini unutmayın. Yedekleme ilkelerini el ile gerektiği gibi değiştirin.

## <a name="run-the-initial-backup"></a>İlk yedeklemeyi çalıştırın

El ile bunu hemen çalıştırmanız sürece zamanlamaya uygun olarak ilk yedekleme çalışır. Elle çalıştırın gibidir:

1. Kasa menüden **yedekleme öğeleri**.
2. İçinde **yedekleme öğeleri** tıklayın **Azure sanal makine**.
3. İçinde **yedekleme öğeleri** listesinde, üç nokta simgesini tıklayın **...** .
4. Tıklayın **Şimdi Yedekle**.
5. İçinde **Şimdi Yedekle**, kurtarma noktası korunması gereken son günü seçmek için takvim denetimini kullanın > **Tamam**.
6. Portal bildirimlerini izleyin. Kasa panosunda iş ilerleme durumunu izleyebilirsiniz > **yedekleme işleri** > **sürüyor**. VM’nizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.



## <a name="next-steps"></a>Sonraki adımlar

- Ortaya herhangi bir sorunu gidermeye [Azure VM aracıları](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) veya [Azure VM yedeklemesi](backup-azure-vms-troubleshoot.md).
- [Azure VM'lerini yedekleme](backup-azure-vms-first-look-arm.md)
