---
title: Azure Vm'leri Azure Backup ile Yedeklemeye hazırlanma
description: Azure sanal makinelerini Azure Backup hizmeti ile yedekleme için hazırlanma işlemi açıklanmaktadır
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 12/17/2018
ms.author: raynew
ms.openlocfilehash: ee7a9c407a26f9334a854c98793db8fc01244e2a
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2019
ms.locfileid: "53994683"
---
# <a name="prepare-to-back-up-azure-vms"></a>Azure sanal makinelerini yedeklemek hazırlama

Bu makalede bir Azure VM yedekleme için hazırlamak nasıl bir [Azure Backup](backup-introduction-to-azure-backup.md) kurtarma Hizmetleri kasası. Yedekleme için hazırlama içerir:


> [!div class="checklist"]
> * Başlamadan önce desteklenen senaryolar ve sınırlamaları gözden geçirin.
> * Azure VM gereksinimleri ve ağ bağlantısı gibi önkoşulları doğrulayın.
> * Bir kasa oluşturun.
> * Depolama nasıl çoğaltır seçin.
> * Vm'leri bulmak, yedekleme ayarları ve ilkesi yapılandırın.
> * Seçili sanal makineler için yedeklemeyi etkinleştirme


> [!NOTE]
   > Bu makalede, bir kasası ayarlama ve yedeklenecek VM'ler seçerek Azure Vm'lerini yedekleme açıklar. Birden çok sanal makinelerini yedekleme istiyorsanız kullanışlıdır. Ayrıca, bir Azure VM'yi doğrudan kendi VM ayarlarından yedekleyebilirsiniz. [Daha fazla bilgi](backup-azure-vms-first-look-arm.md)

## <a name="before-you-start"></a>Başlamadan önce

1. [Genel bakışın](backup-azure-vms-introduction.md) Azure Vm'leri için Azure Backup'ın.
2. Destek ayrıntıları ve sınırlamalar aşağıda gözden geçirin.

   **Destek/sınırlama** | **Ayrıntılar**
   --- | ---
   **Windows işletim sistemi** | Windows Server 2008 R2 64 bit veya üstü.<br/><br/> Windows istemci 7 64-bit veya üstü.
   **Linux işletim sistemi** | 64-bit Linux dağıtımları yedekleyebilirsiniz [Azure tarafından desteklenen](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), CoreOS Linux hariç.<br/><br/> Linux işletim sistemlerini gözden geçirin, [dosya geri yükleme desteği](backup-azure-restore-files-from-vm.md#for-linux-os).<br/><br/> Diğer Linux dağıtımlarına VM Aracısı VM üzerinde kullanılabilir olduğu sürece, çalışma ve Python desteği bulunduğu. Ancak, bu dağıtımları desteklenmez.
   **Bölge** | Tüm Azure sanal makinelerini yedekleyebilirsiniz [desteklenen bölgeler](https://azure.microsoft.com/regions/#services). Bir bölge desteklenmiyorsa, kasayı oluşturduğunuzda seçmek mümkün olmayacaktır.<br/><br/> Yedekleme ve geri yükleme Azure bölgeleri arasında olamaz. Yalnızca tek bir bölgede.
   **Veri disk sınırı** | 16'dan fazla veri diskleri ile Vm'leri yedekleyemezsiniz.
   **Paylaşılan depolama** | VM'lerin yedeklenmesi CSV veya genişleme dosya sunucusu kullanımı önerilmemektedir. CSV yazarları, büyük olasılıkla başarısız olur.
   **Linux şifreleme** | Linux birleşik anahtar Kurulum (LUKS ile) şifrelenmiş sanal makineleri yedekleme desteklenmez.
   **VM tutarlılığı** | Azure Backup, çoklu VM tutarlılığı desteklememektedir.
   **Ağ** | Yedeklenen verileri, bir VM'ye bağlı ağ sürücülerini içermez.<br/><br/>
   **Anlık görüntüleri** | Bir yazma Hızlandırıcısı etkinleştirilmiş disk üzerinde anlık görüntü alma desteklenmiyor. Azure Backup, tüm VM disklerini uygulamayla tutarlı bir anlık görüntüsünü almasını engeller.
   **PowerShell** | Bir dizi yalnızca PowerShell ile kullanılabilen eylem vardır:<br/><br/> -Vm'leri geri yükleme iç/dış yük Dengeleyiciler veya birden çok ayrılmış IP adresleri veya bağdaştırıcıları ile yönetilir. [Daha fazla bilgi](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations)<br/><br/> -Birden çok etki alanı denetleyicisi yapılandırmasında bir etki alanı denetleyicisi VM'SİNİN geri yükleniyor. [Daha fazla bilgi edinin](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
   **Sistem saati** | Azure Backup, Azure VM yedeklemeleri için ışığından değişikliklerin otomatik saat ayarlama desteklememektedir. Yedekleme ilkelerini el ile gerektiği gibi değiştirin.
   **Depolama hesapları** | Bir ağla kısıtlı depolama hesabı kullanıyorsanız, etkinleştirdiğinizden emin olun **güvenilen Microsoft hizmetlerinin bu depolama hesabına erişmesine izin ver** böylece Azure Backup hizmeti hesabına erişebilir. Öğe düzeyinde kurtarma ağla kısıtlı depolama hesapları için desteklenmez.<br/><br/> Bir depolama hesabında emin **güvenlik duvarları ve sanal ağlar** ayarları izin erişimden **tüm ağlar**.


## <a name="prerequisites"></a>Önkoşullar

- Aynı bölgede Azure Vm'lerini yedeklemek istediğiniz kasaya oluşturmanız gerekir.
- Başlamadan önce Azure VM bölgeleri kontrol edin.
    - Birden çok bölgede Vm'leriniz varsa, her bölgede bir kasa oluşturun.
    - Yedekleme verilerini depolamak için depolama hesaplarının belirtilmesi gerekmez. Kasa ve Azure Backup hizmeti, otomatik olarak işler.
- VM Aracısı, yedeklemek istediğiniz bir Azure sanal makinelerinde yüklü olduğunu doğrulayın.



### <a name="install-the-vm-agent"></a>VM aracısını yükleyin

Yedeklemeyi etkinleştirmek için Azure Backup (VM anlık görüntüsü veya VM anlık görüntüsü Linux) bir yedekleme uzantısı Azure VM üzerinde çalışan VM aracısı yükler.
    -  Azure VM Aracısı, bir Azure Market görüntü üzerinden dağıtılan herhangi bir Windows VM üzerinde varsayılan olarak yüklenir. Azure VM Aracısı, bir Azure Market görüntüsünden portal, PowerShell, CLI veya Azure Resource Manager şablonu dağıtırken de yüklenir.
    - Bir sanal makine şirket içinden geçiş yaptıysanız, aracı yüklü değil ve VM için yedeklemeyi etkinleştirmeden önce yüklemeniz gerekir.

Gerekli olursa, aracıyı şu şekilde yükleyin.

**VM** | **Ayrıntılar**
--- | ---
**Windows VM’leri** | [İndirme ve yükleme](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) aracı makine üzerinde yönetici izinlerine sahip.<br/><br/> Yüklemeyi doğrulamak için *C:\WindowsAzure\Packages* WaAppAgent.exe VM'ye sağ tıklayın > **özellikleri**, > **ayrıntıları** sekmesi. **Ürün sürümü** 2.6.1198.718 olmalıdır veya üzeri.
**Linux VM'leri** | Dağıtımınıza ait bir paket deposundaki bir RPM veya DEB paketini kullanarak yükleme, Azure Linux Aracısı yükseltme ve yükleme için tercih edilen yöntemdir. Tüm [dağıtım sağlayıcıları onaylı](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) Azure Linux Aracısı paketi, görüntüleri ve depoları tümleştirme. Aracı kullanılabilir [GitHub](https://github.com/Azure/WALinuxAgent), ancak buradan yükleme önermemekteyiz.
Azure VM'yi yedekleme konusunda sorun varsa, Azure VM Aracısı sanal makinede düzgün yüklendiğini kontrol etmek için aşağıdaki tabloyu kullanın. Tablo, Windows ve Linux Vm'leri için VM Aracısı hakkında ek bilgi sağlar.

### <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma

VM'de çalışan yedekleme uzantısı, Azure genel IP adreslerine giden erişime izin olmalıdır. Erişime izin vermek için şunları yapabilirsiniz:


- **NSG kuralları**: İzin [Azure veri merkezi IP aralıkları](https://www.microsoft.com/download/details.aspx?id=41653). Azure Backup hizmetini kullanarak erişimine izin verecek bir kural ekleyebileceğiniz bir [hizmet etiketi](../virtual-network/security-overview.md#service-tags), tek tek her adres aralığını sağlayan ve zaman içinde bunları yönetmek yerine.
- **Proxy**: Bir HTTP proxy sunucusu için trafiği yönlendirme dağıtın.
- **Azure Güvenlik Duvarı**: Azure Backup hizmeti için bir FQDN etiketi kullanarak VM, Azure güvenlik duvarı üzerinden trafiğine izin verin.

Seçenekler arasında karar verirken, ödün göz önünde bulundurun.

**Seçenek** | **Avantajları** | **Dezavantajları**
--- | --- | ---
**NSG** | Ek maliyet olmadan. Hizmet etiketleri ile yönetmek basit | Yalnızca depolama ve Azure'nın tam erişim sağlar. |
**HTTP Ara sunucusu** | Depolama URL'leri üzerinde ayrıntılı denetim izin verilir.<br/><br/> VM'ler için tek noktası internet erişimi.<br/><br/> Ek maliyet proxy.
**FQDN etiketleri** | Azure sanal ağ içindeki alt ağ ayarlama güvenlik duvarı varsa kullanımı kolaydır | Kendi FQDN etiketleri oluşturun veya FQDN'ler bir etiketi değiştirme.



Azure yönetilen diskler kullanıyorsanız, güvenlik duvarları hakkında ek bağlantı noktası açma (bağlantı noktası 8443) gerekebilir.



### <a name="set-up-an-nsg-rule-to-allow-outbound-access-to-azure"></a>Azure'da giden erişime izin vermek için bir NSG kuralı ayarlama

Azure VM bir NSG tarafından yönetilen erişimi varsa, gerekli aralıkları ve bağlantı noktaları için Yedekleme depolaması için giden erişim verin.



1. VM > **ağ**, tıklayın **giden bağlantı noktası kuralı Ekle**.
- Kuralın daha yüksek olması, erişimi reddettikten bir kuralı varsa, yeni izin verin. Örneğin, bir **Deny_All** kural öncelik, 1000, yeni bir kural kümesi için 1000'den daha az ayarlanmalıdır.
2. İçinde **Giden Güvenlik Kuralı Ekle**, tıklayın **Gelişmiş**.
3. Kaynağı seçin **VirtualNetwork**.
4. İçinde **kaynak bağlantı noktası aralıkları**, herhangi bir bağlantı noktasından giden erişime izin vermek için yıldız işareti (*) yazın.
5. İçinde **hedef**seçin **hizmet etiketi**. Listeden depolama alanını seçin. <region>. Bölge, kasa ve yedeklemek istediğiniz VM'lerin bulunduğu bölgedir.
6. İçinde **hedef bağlantı noktası aralıkları**, bağlantı noktasını seçin.

    - VM ile yönetilmeyen diskler ve şifrelenmemiş bir depolama hesabı: 80
    - VM ile yönetilmeyen diskler ve şifrelenmiş depolama hesabı: 443 (varsayılan ayar)
    - Yönetilen sanal makine: 8443.
1. İçinde **Protokolü**seçin **TCP**.
2. İçinde **öncelik**, bir öncelik değeri vermek değerinden daha da kuralları'nı engelle.
3. Bir ad ve kural için bir açıklama girin ve tıklatın **Tamam**.

Azure Backup için Azure giden erişime izin vermek için birden çok VM için bir NSG kuralı uygulayabilirsiniz.

Bu videoda sürecinde yardımcı olur.

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]



### <a name="route-backup-traffic-through-a-proxy"></a>Bir ara sunucu aracılığıyla yedekleme trafiği yönlendirme

Bir ara sunucu aracılığıyla yedekleme trafiği yönlendirmek ve ardından proxy erişimi için gerekli Azure aralıkları sağlar.

Aşağıdaki izin vermek için VM proxy yapılandırmanız gerekir:

- Azure VM için genel internet Ara sunucusu üzerinden bağlı tüm HTTP trafiğini yönlendirme.
- Proxy, geçerli sanal ağ (VNet) içinde Vm'lerden gelen trafiğe izin vermelidir.
- NSG **NSF kilitleme** Ara sunucuya VM giden internet trafiğine izin verecek bir kural gerekir.

İşte nasıl proxy ayarlamanız gerekir. Örnek değerler kullanıyoruz. Bunları kendi değerlerinizle değiştirmeniz gerekir.

#### <a name="set-up-a-system-account-proxy"></a>Bir sistem hesabı proxy'sini ayarlama
Bir sistem hesabı proxy sahip değilseniz, birini aşağıdaki ayarlamaları yapın:

1. İndirme [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Çalıştırma **PsExec.exe -i -s cmd.exe** komut istemini bir sistem hesabı altında çalıştırılacak.
3. Tarayıcı sistem bağlamında çalışır. Örneğin: **PROGRAMFILES%\Internet Explorer\iexplore.exe** Internet Explorer için.  
4. Proxy ayarlarını tanımlayın.
    - Linux makineleri üzerinde:
        - Bu satırı **/etc/ortam** dosyası:
            - **http_proxy =http://proxy IP adresi: proxy bağlantı noktası**
        - Bu satırları ekleyin **/etc/waagent.conf** dosyası:
            - **HttpProxy.Host=proxy IP adresi**
            - **HttpProxy.Port=proxy bağlantı noktası**
    - Windows makinelerinde, tarayıcı ayarlarınızı bir ara sunucu kullanılması gerektiğini belirtin. Bir kullanıcı hesabı şu anda bir ara sunucu kullanıyorsanız, system hesabına düzeyinde ayarları uygulamak için bu betiği kullanabilirsiniz.
        ```
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
       $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
       Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver

        ```

#### <a name="allow-incoming-connections-on-the-proxy"></a>Proxy gelen bağlantılara izin verin

1. Proxy ayarları gelen bağlantılara izin.
2. Örneğin, açmak **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
    - Sağ **gelen kuralları** > **yeni kural**.
    - İçinde **kural türü** seçin **özel** > **sonraki**.
    - İçinde **Program**seçin **tüm programlar** > **sonraki**.
    - İçinde **protokoller ve bağlantı noktaları** türünü ayarlayın **TCP**, **yerel bağlantı noktaları** için **belirli bağlantı noktaları**, ve **uzak bağlantı noktası**için **tüm bağlantı noktaları**.
    - Sihirbazı tamamlayın ve kural için bir ad belirtin.

#### <a name="add-an-exception-rule-to-the-nsg"></a>NSG ile bir özel durum kuralı ekleyin

NSG üzerinde **NSF kilitleme**, 10.0.0.5 üzerinde herhangi bir bağlantı noktasından herhangi bir Internet adresi 80 (HTTP) veya 443 (HTTPS) bağlantı noktası üzerinde trafiğe izin vermek.

- Aşağıdaki PowerShell betiğini trafiğe izin vermek için bir örnek sağlar.
- Tüm ortak Internet adreslerine giden izin vermek yerine, bir IP adresi aralığı belirtebilirsiniz (-DestinationPortRange), veya storage.region hizmet etiketini kullanabilirsiniz.   

    ```
    Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
    Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
    ```
### <a name="allow-firewall-access-with-fqdn-tag"></a>FQDN etiketi ile güvenlik duvarı erişime izin ver

Ağ trafiği için Azure Backup giden erişime izin vermek için Azure güvenlik duvarını ayarlayabilirsiniz.

- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) Azure güvenlik duvarı dağıtılıyor.
- [Hakkında bilgi edinin](https://docs.microsoft.com/azure/firewall/fqdn-tags) FQDN etiketler.


## <a name="create-a-vault"></a>Kasa oluşturma

Yedekleme için kurtarma Hizmetleri kasası, yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan ve yedeklenen makine ile ilişkili yedekleme ilkeleri depolar. Şu şekilde bir kasa oluşturun:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Üzerinde **Hub** menüsünde **Gözat**ve türü **kurtarma Hizmetleri**. Yazmaya başladığınızda, giriş kaynakların listesini filtrelenir. Seçin **kurtarma Hizmetleri kasaları**.

    ![Kutuya yazarak ve sonuçlarda "Kurtarma Hizmetleri kasaları" seçme](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Kurtarma Hizmetleri kasalarının listesi görünür.
3. Üzerinde **kurtarma Hizmetleri kasaları** menüsünde **Ekle**.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    **Kurtarma Hizmetleri kasaları** bölmesi açılır. Bilgilerini sağlamak için ister **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    !["Kurtarma Hizmetleri kasaları" bölmesi](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. **Ad** alanına kasayı tanımlamak için kolay bir ad girin.
    - Adın Azure aboneliği için benzersiz olması gerekir.
    - Bu, 2 ile 50 karakter içerebilir.
    - Bir harf ile başlamalı ve yalnızca harf, rakam ve kısa çizgi içerebilir.
5. Seçin **abonelik** kullanılabilir abonelik listesini görmek için. Hangi aboneliğin emin değilseniz varsayılan (veya önerilen) aboneliği. Çalışmanızı birden çok seçenek eksikse veya Okul hesabı birden çok Azure aboneliği ile ilişkili olduğunda.
6. Seçin **kaynak grubu** kullanılabilir kaynak grubu listesini görmek veya **yeni** yeni bir kaynak grubu oluşturmak için. [Daha fazla bilgi edinin](../azure-resource-manager/resource-group-overview.md) kaynak grupları hakkında.
7. Seçin **konumu** kasa için coğrafi bölgeyi seçin. Kasa *gerekir* yedeklemek istediğiniz VM'lerin aynı bölgede olmalıdır.
8. **Oluştur**’u seçin.
    - Bu, kasasının oluşturulması biraz zaman alabilir.
    - Portalın sağ üst alandaki durum bildirimlerini izleyin.
    ![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Kasanız oluşturulduktan sonra kurtarma Hizmetleri kasaları listesinde görünür. Kasanız görmüyorsanız seçin **Yenile**.

## <a name="set-up-storage-replication"></a>Depolama çoğaltmayı ayarlama

Varsayılan olarak, kasanız sahip [coğrafi olarak yedekli depolama (GRS)](https://docs.microsoft.com/azure/storage/common/storage-redundancy-grs). GRS için birincil yedeklemenizi öneririz, ancak kullanabileceğiniz[yerel olarak yedekli depolama](https://docs.microsoft.com/azure/storage/common/storage-redundancy-lrs?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) ucuz seçeneği. 

Depolama çoğaltma şu şekilde değiştirin:

1. Kasadaki > **Yedekleme Altyapısı**, tıklayın **yedekleme yapılandırması**

   ![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/full-blade.png)

2. İçinde **yedekleme yapılandırması**, gerekli ve select depolama yedekliliği yöntemi Değiştir **Kaydet**.


## <a name="configure-backup"></a>Yedeklemeyi yapılandırma

Abonelikte Vm'leri bulmak ve yedekleme yapılandırın.

1. Kasadaki > **genel bakış**, tıklayın **+ yedekleme**

   ![Yedekleme düğmesi](./media/backup-azure-arm-vms-prepare/backup-button.png)

   **Yedekleme** ve **yedekleme hedefi** bölmeleri açın.

2. İçinde **yedekleme hedefi**> **iş yükünüz çalıştığı?** seçin **Azure**. İçinde **neleri yedeklemek istiyorsunuz?** seçin **sanal makine** >  **Tamam**. Bu kasada VM uzantısı kaydeder.

   ![Yedekleme ve yedekleme hedefi bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Bu adım, VM uzantısını kasa ile kaydeder. **Yedekleme hedefi** bölmeyi kapatır ve **yedekleme İlkesi** bölmesi açılır.

3. İçinde **yedekleme İlkesi**, kasa ile ilişkilendirmek istediğiniz ilkeyi seçin. Daha sonra, **Tamam**'a tıklayın.
    - Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir.
    - Tıklayın **Yeni Oluştur** bir ilke oluşturmak için. [Daha fazla bilgi edinin](backup-azure-vms-first-look-arm.md#defining-a-backup-policy) ilke tanımlama.

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

- Yedekleme İlkesi, yedekleme planınızın çalıştırır.
- VM'nin çalışır durumda olup olmadığını Backup hizmeti yedekleme uzantısını yükler.
    - Çalışan bir VM, uygulamayla tutarlı bir kurtarma noktası alınma olasılığını en yükseğe çıkarır.
    -  Ancak, sanal makine kapalı ve uzantı yüklenemiyor olsa bile yedeklenir. Bu olarak bilinir *çevrimdışı VM*. Bu durumda, kurtarma noktası olur *kilitlenmeyle tutarlı* olacaktır.
- İsteğe bağlı yedekleme VM için hemen oluşturmak istiyorsanız, **yedekleme öğeleri**, VM yanındaki üç noktaya (…) tıklayın > **Şimdi Yedekle**.


## <a name="next-steps"></a>Sonraki adımlar

- Ortaya herhangi bir sorunu gidermeye [Azure VM aracıları](/backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) veya [Azure VM yedeklemesi](backup-azure-vms-troubleshoot.md).
- [Azure VM'lerini yedekleme](backup-azure-vms-first-look-arm.md)
