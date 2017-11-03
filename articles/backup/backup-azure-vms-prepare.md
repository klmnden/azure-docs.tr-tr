---
title: "Azure sanal makineleri yedeklemek için ortamınızı hazırlama | Microsoft Docs"
description: "Azure sanal makineleri yedeklemek için ortamınızı hazır olduğundan emin olun"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonn
editor: 
keywords: yedeklemeleri; Yedekleme;
ms.assetid: 238ab93b-8acc-4262-81b7-ce930f76a662
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 9/10/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 35b40f80c5a9ccc67830429a5a75d2974d0d138c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="prepare-your-environment-to-back-up-azure-virtual-machines"></a>Azure sanal makinelerini yedeklemek için ortamınızı hazırlama
> [!div class="op_single_selector"]
> * [Resource Manager modeli](backup-azure-arm-vms-prepare.md)
> * [Klasik modeli](backup-azure-vms-prepare.md)
>
>

Bir Azure sanal makinesini (VM) yedekleyebilirsiniz önce bulunmalıdır üç koşullar vardır.

* Bir yedekleme kasası oluşturun veya mevcut bir yedekleme kasası tanımlamak gereken *VM ile aynı bölgede*.
* Azure ortak Internet adresleri ve Azure depolama uç noktaları arasında ağ bağlantısı kurun.
* VM Aracısı'nı VM'e yükleyin.

Bu koşullar zaten ortamınızda mevcut sonra devam biliyorsanız [VM'ler Makalenizi yedekleme](backup-azure-vms.md). Aksi takdirde, okuyun, bu makalede, Azure VM'yi yedeklemek için ortamınızı hazırlama adımlarını kılavuzluk eder.

##<a name="supported-operating-system-for-backup"></a>Yedekleme için desteklenen işletim sistemi
 * **Linux**: Azure Backup, [Azure tarafından onaylanan bir dağıtım listesini](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (CoreOS Linux hariç) destekler. _Diğer Getir-bilgisayarınızı-kendi-Linux dağıtımları da VM aracısının sanal makinede kullanılabilir olduğu sürece çalışır ve Python var. desteği. Ancak, biz yedekleme için bu dağıtımları desteklemiyorsanız._
 * **Windows Server**:  Windows Server 2008 R2’den eski sürümler desteklenmez.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Yedekleme ve geri yükleme VM sınırlamaları
> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki dağıtım modeline sahiptir: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md). Aşağıdaki liste, Klasik modelde dağıtırken sınırlamaları sağlar.
>
>

* 16'dan fazla veri diskleri içeren sanal makineleri yedekleme desteklenmiyor.
* Ayrılmış bir IP adresi ve tanımlanmış hiçbir uç nokta ile sanal makineleri yedekleme desteklenmiyor.
* Yedekleme verilerini bağlı ağ sürücülerine VM'ye ekli içermez.
* Geri yükleme sırasında mevcut bir sanal makinenin değiştirilmesi desteklenmez. Varolan bir sanal makine ve ilişkili tüm diskleri silmeniz ve verileri yedekten geri yükleyin.
* Çapraz bölge yedekleme ve geri yükleme desteklenmez.
* Azure Yedekleme hizmetini kullanarak sanal makineleri yedekleme tüm ortak Azure bölgelerde desteklenir (bkz [denetim listesi](https://azure.microsoft.com/regions/#services) desteklenen bölgeler). Aradığınız bölge bugün desteklenmiyorsa, kasa oluşturma sırasında açılan listede görünmez.
* Azure Yedekleme hizmetini kullanarak sanal makineleri yedekleme yalnızca select işletim sistemi sürümleri için desteklenir:
* Bir etki alanı denetleyicisini geri multi-DC yapılandırmasının bir parçası olan (DC) VM yalnızca PowerShell aracılığıyla desteklenir. Daha fazla bilgi edinin [multi-DC etki alanı denetleyicisini geri](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Aşağıdaki özel ağ yapılandırmalarının sanal makineleri geri yüklenmesi yalnızca PowerShell aracılığıyla desteklenir. Geri yükleme işlemi tamamlandıktan sonra kullanıcı Arabiriminde geri yükleme iş akışı kullanarak oluşturduğunuz sanal makineleri bu ağ yapılandırmaları sahip olmaz. Daha fazla bilgi için bkz: [geri VM'ler özel ağ yapılandırmaları ile](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Sanal makineler yük dengeleyici yapılandırması (dahili ve harici)
  * Birden çok ayrılmış IP adreslerine sahip sanal makineler
  * Birden çok ağ bağdaştırıcısı ile sanal makineler

## <a name="create-a-backup-vault-for-a-vm"></a>Bir VM için bir yedekleme kasası oluşturun
Yedekleme kasası, zaman içinde oluşturulan tüm yedeklemeleri ve kurtarma noktalarını depolayan bir varlıktır. Yedekleme kasası, yedeklenmekte olan sanal makinelere uygulanan yedekleme ilkelerini de içerir.

> [!IMPORTANT]
> Mart 2017’den itibaren Backup kasaları oluşturmak için klasik portalı kullanamayacaksınız. Mevcut Backup kasaları desteklenmeye devam eder ve [Backup kasaları oluşturmak için Azure PowerShell kullanabilirsiniz](./backup-client-automation-classic.md#create-a-backup-vault). Ancak, gelecekteki iyileştirmeler yalnızca Kurtarma Hizmetleri kasaları için geçerli olacağından, Microsoft tüm dağıtımlar için Kurtarma Hizmetleri kasalarının kullanılmasını önerir.


Bu görüntü çeşitli Azure Backup varlıklar arasındaki ilişkiler gösterilmektedir: ![Azure Backup varlıkları ve ilişkileri](./media/backup-azure-vms-prepare/vault-policy-vm.png)



## <a name="network-connectivity"></a>Ağ bağlantısı
VM anlık görüntülerini yönetmek için Azure ortak IP adreslerine bağlantısı yedekleme uzantısını gerekir. Sağ Internet bağlantısı olmadan zaman aşımı sanal makinenin HTTP istekleri ve yedekleme işlemi başarısız olur. Dağıtımınızı (örneğin ağ güvenlik grubu (NSG)) aracılığıyla yerinde erişim sınırlamaları varsa, yedekleme trafiği için açık bir yol sağlamak için bu seçeneklerden birini seçin:

* [Beyaz liste Azure veri merkezi IP aralıkları](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -beyaz liste ile IP adreslerinin nasıl hakkında yönergeler için makalesine bakın.
* Yönlendirme trafiği için bir HTTP proxy sunucusu dağıtın.

Hangi seçeneği kullanacağınıza karar verirken dengelemeler yönetilebilirlik, ayrıntılı bir denetim ve maliyet arasında ' dir.

| Seçenek | Avantajları | Olumsuz yönleri |
| --- | --- | --- |
| Beyaz liste IP aralıkları |Hiçbir ek maliyet.<br><br>Bir NSG'yi erişim açmak için kullanmak <i>kümesi AzureNetworkSecurityRule</i> cmdlet'i. |Etkilenen yönetmek için karmaşık IP aralıkları zamanla değiştirin.<br><br>Azure ve yalnızca depolama tam erişim sağlar. |
| HTTP proxy |Depolama üzerinde ayrıntılı denetim proxy'de URL'lere izin. Kurulum ayrıntılı denetime proxy https://\*.blob.core.windows.net/\* URL deseni Güvenilenler listesine olması gerekir. Güvenilir listeye yalnızca VM tarafından kullanılan depolama hesabı https://\<storageAccount\>.blob.core.windows.net/\* URL deseni Güvenilenler listesine olması gerekir. <br>Sanal makineleri tek noktası Internet erişimi.<br>Azure IP adresi değişiklikleri tabi değildir. |Bir VM ile Ara yazılım çalıştırmak için ek ücrete. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Beyaz liste Azure veri merkezi IP aralıkları
Lütfen Azure veri merkezi IP aralıkları, beyaz liste için bkz: [Azure Web sitesi](http://www.microsoft.com/en-us/download/details.aspx?id=41653) IP aralıkları ve yönergeleri hakkında ayrıntılı bilgi için.

### <a name="using-an-http-proxy-for-vm-backups"></a>VM yedeklemeler için bir HTTP proxy kullanma
VM yedeklemesi, yedekleme uzantısını VM üzerinde bir HTTPS API kullanarak Azure Storage anlık görüntü yönetimi komutları gönderir. Genel internet erişimi için yapılandırılmış tek bileşen olduğundan HTTP proxy üzerinden backup uzantısı trafiği yönlendirmek.

> [!NOTE]
> Kullanılması gereken ara yazılımı için hiçbir öneri yok. Giden sürekliliği olan proxy çekme emin olun ve aşağıdaki yapılandırma adımları ile uyumludur. Üçüncü taraf program proxy ayarlarını değiştirmeyin emin olun
>
>

Aşağıdaki örnek görüntü üç yapılandırma adımlarının bir HTTP Ara sunucusunu kullanmak için gerekli gösterir:

* Uygulama VM ortak Internet Proxy VM üzerinden bağlanan tüm HTTP trafiğini yönlendirir.
* Proxy VM sanal ağ Vm'lerden gelen trafiğine izin verir.
* NSF kilitleme adlı ağ güvenlik grubu (NSG) bir güvenlik kuralı izin giden Internet trafiği Proxy VM gerekir.

![NSG ile HTTP proxy dağıtım diyagramı](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

Genel Internet'e iletişim için bir HTTP proxy kullanmak için aşağıdaki adımları izleyin:

#### <a name="step-1-configure-outgoing-network-connections"></a>1. Adım Giden ağ bağlantılarını yapılandırma
###### <a name="for-windows-machines"></a>Windows makineler için
Bu, yerel sistem hesabı için proxy sunucusu yapılandırmasını Kurulum.

1. Karşıdan [PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Yükseltilmiş isteminden şu komutu çalıştırın,

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Internet explorer penceresi açar.
3. Araçlar -> Internet Seçenekleri -> bağlantıları LAN Ayarları ->.
4. Sistem hesabı için proxy ayarlarını doğrulayın. Proxy IP ve bağlantı noktası ayarlayın.
5. Internet Explorer'ı kapatın.

Bu bir makine genelinde proxy yapılandırmasını ayarlanır ve tüm giden HTTP/HTTPS trafiği için kullanılır.

Geçerli kullanıcı hesabında (yerel sistem hesabı değildir) bir proxy sunucu kurulum varsa, bunları için SYSTEMACCOUNT uygulamak için aşağıdaki komut dosyasını kullanın:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Proxy sunucu oturum açma "(407) Proxy kimlik doğrulaması gerekli" gözlemlerseniz, kimlik doğrulama Kurulumu doğru ayarlandığından emin olun.
>
>

###### <a name="for-linux-machines"></a>Linux makineler için
Aşağıdaki satırı ekleyin ```/etc/environment``` dosyası:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Aşağıdaki satırları ekleyin ```/etc/waagent.conf``` dosyası:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>2. Adım Gelen bağlantılar proxy sunucusuna izin ver:
1. Proxy sunucu üzerinde Windows Güvenlik Duvarı'nı açın. Güvenlik Duvarı erişmek için kolay Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı aramak için yoludur.

    ![Güvenlik Duvarı'nı açın](./media/backup-azure-vms-prepare/firewall-01.png)
2. Windows Güvenlik Duvarı iletişim kutusunda sağ **gelen kuralları** tıklatıp **yeni kural...** .

    ![Yeni bir kural oluşturun](./media/backup-azure-vms-prepare/firewall-02.png)
3. İçinde **yeni gelen kuralı Sihirbazı**, seçin **özel** seçenek için **kural türü** tıklatıp **sonraki**.
4. Seçilecek sayfasında **Program**, seçin **tüm programlar** tıklatıp **sonraki**.
5. Üzerinde **protokol ve bağlantı noktaları** sayfasında, aşağıdaki bilgileri girin ve tıklayın **sonraki**:

    ![Yeni bir kural oluşturun](./media/backup-azure-vms-prepare/firewall-03.png)

   * için *protokol türü* seçin *TCP*
   * için *yerel bağlantı noktası* seçin *belirli bağlantı noktaları*, aşağıdaki alana belirtin ```<Proxy Port>``` yapılandırılmış.
   * için *uzak bağlantı noktası* seçin *tüm bağlantı noktaları*

     Sihirbazın geri kalanı için tüm sonuna kadar tıklayın ve bu kural bir ad verin.

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>3. Adım NSG'yi bir özel durum kuralı ekleyin:
Bir Azure PowerShell komut isteminde aşağıdaki komutu girin:

Aşağıdaki komut, bir özel durum NSG'yi ekler. Bu özel bağlantı noktası 80 (HTTP) veya 443 (HTTPS) herhangi bir Internet adresi 10.0.0.5 üzerinde herhangi bir bağlantı gelen TCP trafiğine izin verir. Genel Internet belirli bir bağlantı noktası gerekiyorsa, bu bağlantı noktasına eklediğinizden emin olun ```-DestinationPortRange``` de.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

*Dağıtımınız için uygun ayrıntılarla örnek adları yerine emin olun.*

## <a name="vm-agent"></a>VM Aracısı
Azure sanal makineyi yedekleyebilirsiniz önce Azure VM Aracısı sanal makineye doğru yüklendiğinden emin olmalısınız. VM Aracısı sanal makinenin oluşturulduğu sırada isteğe bağlı bir bileşen olduğundan, sanal makine sağlanmadan önce VM Aracısı onay kutusunu seçili olduğundan emin olun.

### <a name="manual-installation-and-update"></a>El ile yükleme ve güncelleştirme
VM Aracısı, Azure galerisinden oluşturulan VM'ler içinde zaten mevcuttur. Ancak, şirket içi veri merkezlerinden Geçişi sanal makineleri VM aracısının yüklü sahip olmaz. Bu tür VM'ler için VM Aracısı açıkça yüklü olması gerekir. 

| **İşlem** | **Windows** | **Linux** |
| --- | --- | --- |
| VM Aracısı'nı yükleme |[Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir.<li>Aracının yüklü olduğunu belirtmek üzere [VM özelliğini güncelleştirin](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx). |<li> En son yükleme [Linux Aracısı](../virtual-machines/linux/agent-user-guide.md). Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. Aracı dağıtım depodan yüklemenizi öneririz. Biz **değil önerilir** doğrudan github'dan yükleme Linux VM Aracısı.  |
| VM Aracısı'nı güncelleştirme |VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. <br>VM aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |[Linux VM Aracısı'nı güncelleştirme](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile ilgili yönergeleri uygulayın. Aracı dağıtım havuzunuzdan güncelleştirme öneririz. Biz **değil önerilir** doğrudan github'dan güncelleştirme Linux VM Aracısı.<br>VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |
| VM Aracısı yüklemesini doğrulama |<li>Azure VM'de *C:\WindowsAzure\Packages* klasörüne gidin. <li>Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.<li> Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün Sürümü alanı 2.6.1198.718 veya üzeri olmalıdır. |Yok |

Hakkında bilgi edinin [VM Aracısı](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) ve [nasıl yükleneceği](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/).

### <a name="backup-extension"></a>Backup uzantısı
Sanal makineyi yedeklemek için Azure Backup hizmeti uzantı VM Aracısı'na yükler. Azure Backup hizmeti, ek kullanıcı müdahalesi olmadan sorunsuz bir şekilde yedekleme uzantısını yükseltir ve uzantıya düzeltme eki uygular.

VM çalışıyorsa, yedekleme uzantısı yüklenir. Çalışan bir VM Ayrıca, bir uygulama tutarlı bir kurtarma noktası alınma olasılığını en yükseğe. Ancak, hizmet VM'yi yedeklemek--kapalı ve uzantısı yüklenemedi olsa bile için devam edecek Azure Backup (diğer adıyla çevrimdışı VM) yüklü. Bu durumda, kurtarma noktası olur *kilitlenmeyle tutarlı* yukarıda açıklandığı gibi.

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Sonraki adımlar
VM yedeklemesi için ortamınızı hazırlandığınıza göre sonraki mantıksal bir yedekleme oluşturmak için adımdır. Planlama makale Vm'leri yedekleme konusunda daha ayrıntılı bilgi sağlar.

* [Sanal makineleri yedekleme](backup-azure-vms.md)
* [VM yedekleme altyapınızı planlama](backup-azure-vms-introduction.md)
* [Sanal makine yedeklerini yönetme](backup-azure-manage-vms.md)
