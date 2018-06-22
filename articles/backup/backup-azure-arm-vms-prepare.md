---
title: 'Azure yedekleme: sanal makineleri yedeklemek hazırlama'
description: Azure sanal makineleri yedeklemek için ortamınızı hazır olduğundan emin olun.
services: backup
author: markgalioto
manager: carmonm
keywords: yedeklemeleri; Yedekleme;
ms.service: backup
ms.topic: conceptual
ms.date: 6/21/2018
ms.author: markgal
ms.openlocfilehash: 5aa07d7861413fa0ddc0d5af7aefe828df412b4d
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309155"
---
# <a name="prepare-your-environment-to-back-up-resource-manager-deployed-virtual-machines"></a>Resource Manager ile dağıtılan sanal makineleri yedeklemek için ortamınızı hazırlama

Bu makalede, bir Azure Resource Manager tarafından dağıtılan sanal makinesini (VM) yedeklemek için ortamınızı hazırlama için adımları sağlar. Yordamda gösterildiği adımları Azure Portalı'nı kullanın. Bir sanal makineyi yedeklediğinizde, yedekleme verilerini veya kurtarma noktaları, Kurtarma Hizmetleri kasasına depolanır. Kurtarma Hizmetleri kasaları Klasik ve Resource Manager tarafından dağıtılan sanal makineler için yedekleme verileri depolar.

> [!NOTE]
> Azure oluşturmak ve kaynaklarla çalışmak için iki dağıtım modeline sahiptir: [Resource Manager ve klasik](../azure-resource-manager/resource-manager-deployment-model.md).

Koruma (veya yedekleme önce) bir Resource Manager tarafından dağıtılan sanal makine bu Önkoşullar var olduğundan emin olun:

* Oluşturma veya bir kurtarma Hizmetleri kasası tanımlama *sanal makineniz ile aynı bölgede*.
* Bir senaryo seçmek, yedekleme ilkesi tanımlama ve korunacak öğeleri tanımlama.
* Sanal makine üzerinde VM Aracısı (uzantısı) yüklemesini inceleyin.
* Ağ bağlantısını denetleyin.
* Linux VM'ler için uygulamayla tutarlı yedeklemeler için yedekleme ortamınızı özelleştirmek istiyorsanız, izleyin [anlık görüntü öncesi ve anlık görüntü sonrası betiklerini yapılandırma adımları](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).

Bu koşullar, ortamınızda zaten mevcutsa devam [, Vm'leri yedekleme](backup-azure-arm-vms.md) makalesi. Ayarlayın veya bu Önkoşullar hiçbirini denetleyin ihtiyacınız varsa, bu makalede adımlarında size yol gösterir.

## <a name="supported-operating-systems-for-backup"></a>Yedekleme için desteklenen işletim sistemleri

 * **Linux**: Azure Backup destekleyen [Azure Symantec'in dağıtımları listesini](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hariç CoreOS Linux. Dosyaları geri yükleme desteği Linux işletim sistemlerinin listesi için bkz [dosyaları sanal makine yedeklemeden kurtarmak](backup-azure-restore-files-from-vm.md#for-linux).

    > [!NOTE] 
    > Diğer Getir-bilgisayarınızı-kendi-Linux dağıtımları VM aracısının sanal makinede kullanılabilir olduğu sürece, iş ve Python var. desteği. Ancak, bu dağıtımları desteklenmez.
    >
 * **Windows Server**, **Windows İstemcisi**: Windows Server 2008 R2 veya Windows 7 ' daha eski sürümleri desteklenmez.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Yedekleme ve geri yükleme VM sınırlamaları
Ortamınızı hazırlama önce sınırlamalara anladığınızdan emin olun:

* 16'dan fazla veri diskleri içeren sanal makineleri yedekleme desteklenmiyor.
* Ayrılmış bir IP adresi ve tanımlanmış hiçbir uç nokta ile sanal makineleri yedekleme desteklenmiyor.
* Linux VM'ler üzerinden Linux birleşik anahtar Kurulum (LUKS) şifreleme şifreli yedekleme desteklenmiyor.
* Küme Paylaşılan birimleri (CSV) veya genişleme dosya sunucusu yapılandırması içeren Vm'leri yedekleme öneririz yok. Bu yapıldığında, CSV yazıcılarının başarısızlığını beklenir. Bunlar, bir anlık görüntü görev sırasında küme yapılandırmasında bulunan tüm VM'ler içeren gerektirir. Azure yedekleme çoklu VM tutarlılığını desteklemiyor. 
* Yedekleme verilerini bir VM'ye bağlı takılı ağ sürücülerini içermez.
* Geri yükleme sırasında mevcut bir sanal makinenin değiştirilmesi desteklenmez. VM VM mevcut olduğunda geri yüklemeye geri yükleme işlemi başarısız olur.
* Çapraz bölge yedeklemek ve geri yükleme desteklenmez.
* Yukarı geri yapılandırırken, emin **güvenlik duvarları ve sanal ağlar** depolama hesabı ayarlarını tüm ağlardan erişim izni.
* Seçili ağlar için depolama hesabınız için güvenlik duvarı ve sanal ağ ayarlarını yapılandırdıktan sonra Seç **izin güvenilen bu depolama hesabına erişmek için Microsoft Hizmetleri** Azure Backup hizmeti etkinleştirmek için bir özel durum kısıtlı ağ depolama hesabına erişim. Öğe düzeyinde kurtarma kısıtlı ağ depolama hesapları için desteklenmiyor.
* Tüm ortak bölgelerde Azure sanal makineleri yedekleyebilirsiniz. (Bkz [denetim listesi](https://azure.microsoft.com/regions/#services) desteklenen bölgeler.) Aradığınız bölge bugün desteklenmiyorsa, kasa oluşturma sırasında aşağı açılan listede görünmez.
* Bir etki alanı denetleyicisini geri multi-DC yapılandırmasının bir parçası olan (DC) VM yalnızca PowerShell aracılığıyla desteklenir. Daha fazla bilgi için bkz: [multi-DC etki alanı denetleyicisini geri](backup-azure-arm-restore-vms.md#restore-domain-controller-vms).
* Anlık görüntü yazma etkinleştirilmiş Hızlandırıcı disk üzerinde desteklenmiyor. Bu kısıtlama sanal makinenin tüm disklerinin bir uygulamayla tutarlı anlık görüntü gerçekleştirmek için Azure Backup hizmetinin özelliğini engeller.
* Aşağıdaki özel ağ yapılandırmalarının sanal makineleri geri yüklenmesi yalnızca PowerShell aracılığıyla desteklenir. Geri yükleme işlemi tamamlandıktan sonra geri yükleme iş akışı içinde UI aracılığıyla oluşturulan sanal makineleri bu ağ yapılandırmaları sahip olmaz. Daha fazla bilgi için bkz: [geri VM'ler özel ağ yapılandırmaları ile](backup-azure-arm-restore-vms.md#restore-vms-with-special-network-configurations).
  * Sanal makineler yük dengeleyici yapılandırması (dahili ve harici)
  * Birden çok ayrılmış IP adreslerine sahip sanal makineler
  * Birden çok ağ bağdaştırıcısı ile sanal makineler

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Bir VM için bir kurtarma Hizmetleri kasası oluşturma
Kurtarma Hizmetleri kasası yedeklemeleri ve zaman içinde oluşturulan kurtarma noktalarını depolayan bir varlıktır. Kurtarma Hizmetleri kasası, korunan sanal makinelerle ilişkili yedekleme ilkelerini de içerir.

Kurtarma Hizmetleri kasası oluşturmak için:

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. Üzerinde **Hub** menüsünde, select **Gözat**ve ardından **kurtarma Hizmetleri**. Yazmaya başladığınızda, girişinizi kaynakların listesini filtreler. Seçin **kurtarma Hizmetleri kasaları**.

    ![Kutusuna yazarak ve "Kurtarma Hizmetleri kasalarının" sonuçlarında seçme](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    Kurtarma Hizmetleri kasalarının listesi görünür.
3. Üzerinde **kurtarma Hizmetleri kasaları** menüsünde, select **Ekle**.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    **Kurtarma Hizmetleri kasaları** bölmesini açar. Bilgilerini sağlamak için ister **adı**, **abonelik**, **kaynak grubu**, ve **konumu**.

    !["Kurtarma Hizmetleri kasaları" bölmesi](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ile 50 karakter içeren bir ad yazın. Bir harfle başlamalı ve yalnızca harf, rakam ve kısa çizgi içerebilir.
5. Seçin **abonelik** kullanılabilir abonelik listesini görmek için. Hangi aboneliğin emin değilseniz varsayılan kullanın (veya önerilen) aboneliği. Çalışmanızı birden çok seçenek eksikse veya Okul hesabı birden çok Azure aboneliği ile ilişkili olduğunda.
6. Seçin **kaynak grubu** kullanılabilir kaynak gruplarının listesini görmek veya seçmek için **yeni** yeni bir kaynak grubu oluşturmak için. Kaynak grupları hakkında tam bilgi için bkz: [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md).
7. Seçin **konumu** kasa için coğrafi bölgeyi seçin. Kasa korumak istediğiniz sanal makinelerle aynı bölgede *olmalıdır*.

   > [!IMPORTANT]
   > VM bulunduğu konumu emin değilseniz kasa oluşturma iletişim kutusunu kapatın ve portaldaki sanal makinelerin listesini gidin. Birden çok bölgede sanal makineniz varsa her bölgede bir kurtarma Hizmetleri kasası oluşturmanız gerekir. Sonraki konuma geçmeden önce, kasayı ilk konumda oluşturun. Yedek verileri depolamak için depolama hesapları belirtmek için gerek yoktur. Kurtarma Hizmetleri kasası ve Azure Backup hizmeti, otomatik olarak işler.
   >
   >

8. **Oluştur**’u seçin. Kurtarma Hizmetleri kasasının oluşturulması biraz zaman alabilir. Portalın sağ üst alandaki durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra kurtarma Hizmetleri kasaları listesinde görünür. Kasanızı görmüyorsanız seçin **yenileme**.

    ![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

Kasanızı oluşturduğunuza göre, artık depolama çoğaltmayı nasıl ayarlayacağınızı öğrenebilirsiniz.

## <a name="set-storage-replication"></a>Depolama çoğaltma
Depolama çoğaltma seçeneği, coğrafi olarak yedekli depolama ve yerel olarak yedekli depolama arasında seçim yapmanızı sağlar. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Birincil yedeklemenizin coğrafi olarak yedekli depolama seçeneği ayarını bırakın. Olarak ucuz bir seçenek istiyorsanız yerel olarak yedekli depolamayı seçin.

Depolama çoğaltma ayarını düzenlemek için:

1. Üzerinde **kurtarma Hizmetleri kasaları** bölmesinde kasanızı seçin.
    Kasa adınız seçtiğinizde **ayarları** (olduğu kasanın adını en üstte) bölmesinde ve kasa ayrıntılar bölmesini açın.

   ![Yedekleme kasalarının listesinden kasanızı seçin](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. Üzerinde **ayarları** bölmesinde, aşağı kaydırarak için dikey kaydırıcıyı kullanın **Yönet** bölümünde ve seçin **Yedekleme Altyapısı**. İçinde **genel** bölümünde, select **yedekleme yapılandırması**. Üzerinde **yedekleme yapılandırması** bölmesinde, kasanız için depolama çoğaltma seçeneğini belirleyin. Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir.

   ![Yedekleme kasalarının listesi](./media/backup-azure-arm-vms-prepare/full-blade.png)

   Azure birincil yedekleme alanı uç noktası kullanıyorsanız coğrafi olarak yedekli depolamayı kullanmaya devam edin. Azure birincil olmayan yedekleme alanı uç noktası olarak kullanıyorsanız, yerel olarak yedekli depolamayı seçin. Depolama seçenekleri hakkında daha fazla bilgiyi [Azure Storage Çoğaltmaya genel bakış](../storage/common/storage-redundancy.md).

3. Depolama çoğaltma türü değişirse seçin **kaydetmek**.
    
Kasanız için depolama seçeneğini belirledikten sonra VM'yi kasa ile ilişkilendirmek hazırsınız. İlişkilendirmeyi başlatmak için Azure sanal makinelerini bulmanız ve kaydetmeniz gerekir.

## <a name="select-a-backup-goal-set-policy-and-define-items-to-protect"></a>Yedekleme hedefi seçme, ilke ayarlamak ve korunacak öğeleri tanımlama
Bir sanal makine bir kurtarma Hizmetleri kasasına kaydetmeniz önce aboneliğe eklenmiş herhangi bir yeni sanal makine tanımlamak için bulma işlemini çalıştırın. Bulma işlemi Azure Abonelikteki sanal makinelerin listesini sorgular. Yeni sanal makineler bulunursa, portal ilişkili bölge ve bulut hizmeti adını görüntüler. Azure portalında *senaryo* kurtarma Hizmetleri kasasına girin. *İlke* ne sıklıkta ve ne zaman kurtarma noktaları alınır için bir zamanlama. İlke aynı zamanda kurtarma noktaları için bekletme aralığını içerir.

1. Açık bir Kurtarma Hizmetleri kasanız zaten varsa 2. adıma geçin. Açık kurtarma Hizmetleri kasası yoksa açmak [Azure portal](https://portal.azure.com/). Üzerinde **Hub** menüsünde, select **daha fazla hizmet**.

   a. Kaynak listesinde **Kurtarma Hizmetleri** yazın. Yazmaya başladığınızda, giriş listesini filtreler. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

      ![Kutusuna yazarak ve "Kurtarma Hizmetleri kasalarının" sonuçlarında seçme](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

      Kurtarma Hizmetleri kasalarının listesi görünür. Aboneliğinizde hiçbir kasalarını varsa, bu boş bir listedir.

      ![Kurtarma Hizmetleri kasaları listesinin görünümü](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   b. Kurtarma Hizmetleri kasalarının listesinden bir kasa seçin.

      **Ayarları** bölmesinde ve seçilen kasa Panosu kasa açın.

      ![Ayarlar bölmesini ve kasa Panosu](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. Kasa Panosu menüsünden seçin **yedekleme**.

   ![Yedekleme düğmesi](./media/backup-azure-arm-vms-prepare/backup-button.png)

   **Yedekleme** ve **yedekleme hedefi** bölmeleri açın.

3. Üzerinde **yedekleme hedefi** kümesi bölmesi, **, iş yükünü çalıştırdığı?** olarak **Azure** ve **neleri yedeklemek istiyorsunuz?** olarak  **Sanal makine**. Sonra **Tamam**’ı seçin.

   ![Yedekleme ve yedekleme hedefi bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

   Bu adım, kasa ile VM uzantısı kaydeder. **Yedekleme hedefi** bölmesi kapanır ve **yedekleme İlkesi** bölmesini açar.

   !["Yedekleme" ve "ilke yedekleme" bölmeleri](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. Üzerinde **yedekleme İlkesi** bölmesinde, kasaya uygulamak istediğiniz yedekleme ilkesini seçin.

   ![Yedekleme ilkesini seçme](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

   Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir. Yeni bir ilke oluşturmak istiyorsanız açılan menüden **Yeni Oluştur**'u seçin. Bir yedekleme ilkesi tanımlamaya yönelik yönergeler için bkz. [Yedekleme ilkesi tanımlama](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Seçin **Tamam** yedekleme İlkesi kasa ile ilişkilendirilecek.

   **Yedekleme İlkesi** bölmesi kapanır ve **sanal makine Seç** bölmesini açar.
5. Üzerinde **sanal makine Seç** bölmesinde, belirtilen ilke ile ilişkilendirmek ve seçmek için sanal makineleri seçin **Tamam**.

   !["Sanal makineleri seçin" bölmesi](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

   Seçilen sanal makine doğrulanır. Beklenen sanal makinelerin görmüyorsanız, sanal makineleri kurtarma Hizmetleri kasasıyla aynı Azure bölgesinde olup olmadığını denetleyin. Sanal makineler hala göremiyorsanız, bunlar zaten başka bir kasaya korunmayan olduğunu denetleyin. Kasa Panosu, Kurtarma Hizmetleri kasası bulunduğu bölgeyi gösterir.

6. Kasa için tüm ayarların tanımladığınız göre **yedekleme** bölmesinde, **yedeklemeyi etkinleştir**. Bu adım ilkeyi kasaya ve Vm'lere dağıtır. Bu adım, sanal makine için ilk kurtarma noktası oluşturmaz.

   !["Yedekleme etkinleştir" düğmesi](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

Yedekleme başarıyla etkinleştirdikten sonra yedekleme ilkenizi zamanlamaya göre çalışır. Şimdi, sanal makineleri geri görmek için bir talep üzerine yedekleme işi oluşturmak istiyorsanız [yedekleme işini tetiklemeden](./backup-azure-vms-first-look-arm.md#initial-backup).

Sanal makine kaydetme sorunları varsa, aşağıdaki bilgileri VM Aracısı'nı yükleme ve ağ bağlantısına bakın. Azure üzerinde oluşturulan sanal makineleri koruyorsanız aşağıdaki bilgileri muhtemelen gerekmez. Ancak, sanal makineleriniz için Azure geçirdiyseniz, VM Aracısı düzgün yüklenmiş ve sanal makinenizi sanal ağ ile iletişim kurabildiğinden emin olun.

## <a name="install-the-vm-agent-on-the-virtual-machine"></a>VM Aracısı sanal makineye yükleme
Çalışmak Azure Backup uzantısı [VM Aracısı](../virtual-machines/extensions/agent-windows.md) Azure sanal makineye yüklenmesi gerekir. VM Azure Marketi'nden oluşturulmuşsa VM Aracısı sanal makineye zaten. 

Aşağıdaki bilgiler olduğunuz durumlarda sağlanan *değil* VM kullanılarak oluşturulan Azure Marketi'nde. **Örneğin, bir VM bir şirket içi veri merkezinden geçişi. Böyle bir durumda, VM aracısının sanal makineyi korumak için yüklü olması gerekir.**

**Not**: VM Aracısı'nı yükledikten sonra ayrıca Azure PowerShell'i Azure VM aracısı yüklü olan bilmesi için ProvisionGuestAgent özelliğini güncelleştirmek için kullanmanız gerekir. 

Azure VM'yi yedekleme konusunda sorun yaşarsanız Azure VM Aracısı sanal makineye doğru şekilde yüklendiğini denetlemek için aşağıdaki tabloyu kullanın. Tablo, Windows ve Linux VM'ler için VM Aracısı hakkında ek bilgi sağlar.

| **İşlem** | **Windows** | **Linux** |
| --- | --- | --- |
| VM Aracısı'nı yükleme |[Aracı MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) dosyasını indirip yükleyin. Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. |<li> En son yükleme [Linux Aracısı](../virtual-machines/extensions/agent-linux.md). Yüklemeyi tamamlamak için Yönetici ayrıcalıklarına sahip olmanız gerekir. Aracı dağıtım depodan yüklemenizi öneririz. Biz **değil önerilir** doğrudan github'dan yükleme Linux VM Aracısı.  |
| VM Aracısı'nı güncelleştirme |VM Aracısı'nı güncelleştirmek için [VM Aracısı ikili dosyalarının](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409) yeniden yüklenmesi yeterlidir. <br>VM aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |[Linux VM Aracısı'nı güncelleştirme](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ile ilgili yönergeleri uygulayın. Aracı dağıtım havuzunuzdan güncelleştirme öneririz. Biz **değil önerilir** doğrudan github'dan güncelleştirme Linux VM Aracısı.<br>VM Aracısı güncelleştirilirken herhangi bir yedekleme işleminin çalıştırılmadığından emin olun. |
| VM Aracısı yüklemesini doğrulama |<li>Azure VM'de *C:\WindowsAzure\Packages* klasörüne gidin. <li>Mevcut WaAppAgent.exe dosyasını bulmanız gerekir.<li> Dosyaya sağ tıklayın, **Özellikler**'e gidin ve ardından **Ayrıntılar** sekmesini seçin. Ürün Sürümü alanı 2.6.1198.718 veya üzeri olmalıdır. |Yok |

### <a name="backup-extension"></a>Backup uzantısı
VM Aracısı sanal makineye yüklendikten sonra Azure Backup hizmeti yedekleme uzantısını VM Aracısı'na yükler. Yedekleme hizmeti sorunsuz bir şekilde yükseltir ve yedekleme uzantısını düzeltme ekleri.

VM çalıştıran olup olmadığına bakılmaksızın Backup hizmeti yedekleme uzantısını yükler. Çalışan bir VM, uygulamayla tutarlı bir kurtarma noktası alınma olasılığını en yükseğe çıkarır. Ancak, kapalı ve uzantı yüklenmemiş olsa bile VM'yi yedeklemek Backup hizmetini sürdürür. Bu olarak bilinir *çevrimdışı VM*. Bu durumda, kurtarma noktası olur *kilitlenmeyle tutarlı* olacaktır.

## <a name="establish-network-connectivity"></a>Ağ bağlantısı kurma
VM anlık görüntülerini yönetmek için Azure ortak IP adreslerine bağlantısı yedekleme uzantısını gerekir. Sağ internet bağlantısı olmadan zaman aşımı sanal makinenin HTTP istekleri ve yedekleme işlemi başarısız olur. Örneğin bir ağ güvenlik grubu (NSG) aracılığıyla--yerinde erişim kısıtlamalarını dağıtımınız varsa, yedekleme trafiği için açık bir yol sağlamak için bu seçeneklerden birini seçin:

* [Beyaz liste Azure veri merkezi IP aralıkları](http://www.microsoft.com/en-us/download/details.aspx?id=41653).
* Yönlendirme trafiği için bir HTTP proxy sunucusu dağıtın.

Hangi seçeneğin kullanılacağını saptarken dengelemeler yönetilebilirlik, ayrıntılı bir denetim ve maliyet arasında ' dir.

| Seçenek | Avantajları | Olumsuz yönleri |
| --- | --- | --- |
| Beyaz liste IP aralıkları |Hiçbir ek maliyet.<br><br>Bir NSG'yi erişim açmak için kullanmak **kümesi AzureNetworkSecurityRule** cmdlet'i. |Etkilenen yönetmek için karmaşık IP aralıkları zamanla değiştirin.<br><br>Azure ve yalnızca depolama tam erişim sağlar. |
| Bir HTTP proxy sunucu kullan |Depolama üzerinde ayrıntılı denetim proxy'de URL'leri izin verilir.<br><br>Sanal makineleri tek noktası internet erişimi.<br><br>Azure IP adresi değişiklikleri tabi değildir. |Bir VM ile Ara yazılım çalıştırmak için ek ücrete. |

### <a name="whitelist-the-azure-datacenter-ip-ranges"></a>Beyaz liste Azure veri merkezi IP aralıkları
Azure veri merkezi IP aralıkları, beyaz liste için bkz: [Azure Web sitesi](http://www.microsoft.com/en-us/download/details.aspx?id=41653) IP aralıkları ve yönergeleri hakkında ayrıntılı bilgi için.

Kullanarak belirli bir bölge depolama bağlantılara izin vermek [hizmet etiketleri](../virtual-network/security-overview.md#service-tags). Depolama hesabına erişim izni veren Kuralı Internet erişimi engelleyen kural daha yüksek önceliğe sahip olduğundan emin olun. 

![Bir bölge için depolama etiketlerle NSG](./media/backup-azure-arm-vms-prepare/storage-tags-with-nsg.png)

Aşağıdaki videoda, servis etiketlerini yapılandırmak için adım adım yordam boyunca size yardımcı olur: 

>[!VIDEO https://www.youtube.com/embed/1EjLQtbKm1M]

> [!WARNING]
> Depolama hizmet etiketleri yalnızca belirli bölgelerde kullanılabilir ve önizlemede. Bölgelerin bir listesi için bkz: [etiketler için depolama hizmet](../virtual-network/security-overview.md#service-tags).

### <a name="use-an-http-proxy-for-vm-backups"></a>VM yedeklemeler için bir HTTP Proxy'si kullanın
Bir VM'yi yedekleme yapıyorsanız, yedekleme uzantısını VM üzerinde bir HTTPS API kullanarak Azure Storage anlık görüntü yönetimi komutları gönderir. Genel internet erişimi için yapılandırılmış tek bileşen olduğundan HTTP proxy üzerinden backup uzantısı trafiği yönlendirmek.

> [!NOTE]
> Kullanmanız gereken belirli proxy yazılım öneririz yok. Bu izleme ile yapılandırma adımları uyumlu bir proxy çekme emin olun.
>
>

Aşağıdaki örnek resmi üç yapılandırma adımlarının bir HTTP Ara sunucusunu kullanmak için gerekli gösterir:

* Uygulama VM yollar tüm HTTP trafiğini VM proxy'si aracılığıyla ortak Internet bağlı.
* Proxy VM gelen trafiği VM'lerin sanal ağ sağlar.
* NSF kilitleme adlı ağ güvenlik grubu proxy VM giden Internet trafiği izin veren bir güvenlik kuralı gerekiyor.

Ortak internet ile iletişim kurmak için bir HTTP Ara sunucusunu kullanmak için aşağıdaki adımları tamamlayın.

> [!NOTE]
> Bu adımları, bu örnek için belirli adları ve değerleri kullanın. Ne zaman, girme (veya yapıştırma), koda ayrıntıları, dağıtımınız için adlarını ve değerlerini kullanın.

#### <a name="step-1-configure-outgoing-network-connections"></a>1. adım: giden ağ bağlantılarını yapılandırma
###### <a name="for-windows-machines"></a>Windows makineler için
Bu yordamı yerel sistem hesabı için proxy sunucusu yapılandırmasını ayarlar.

1. Karşıdan [PsExec](https://technet.microsoft.com/sysinternals/bb897553).
2. Yükseltilmiş isteminden aşağıdaki komutu çalıştırarak Internet Explorer'ı açın:

    ```
    psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
    ```

3. Internet Explorer'da Git **Araçları** > **Internet Seçenekleri** > **bağlantıları** > **LAN Ayarları**.
4. Sistem hesabı için proxy ayarlarını doğrulayın. Proxy IP ve bağlantı noktası ayarlayın.
5. Internet Explorer'ı kapatın.

Aşağıdaki komut dosyasını bir makine genelinde proxy yapılandırmasını ayarlar ve tüm giden HTTP veya HTTPS trafiği için kullanır. Geçerli bir kullanıcı hesabı (yerel sistem hesabı değil) proxy sunucusunda ayarladıysanız için SYSTEMACCOUNT uygulamak için bu komut dosyasını kullanın.

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Proxy sunucusu günlüğünde "(407) Proxy kimlik doğrulaması gerekli" gözlemlerseniz, kimlik doğrulaması doğru şekilde kurulduğundan emin denetleyin.
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

#### <a name="step-2-allow-incoming-connections-on-the-proxy-server"></a>2. adım: proxy sunucuda gelen bağlantılara izin ver
1. Proxy sunucu üzerinde Windows Güvenlik Duvarı'nı açın. Aramak için güvenlik duvarı erişmek için en kolay yolu olan **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı**.
2. İçinde **Gelişmiş Güvenlik Özellikli Windows Güvenlik Duvarı** iletişim kutusu, sağ **gelen kuralları** seçip **yeni kural**.
3. Yeni gelen kuralı sihirbazında, üzerinde **kural türü** sayfasında, **özel** seçeneğini ve **sonraki**.
4. Üzerinde **Program** sayfasında, **tüm programlar** seçip **sonraki**.
5. Üzerinde **protokol ve bağlantı noktaları** sayfasında, aşağıdaki bilgileri girin ve seçin **sonraki**:
   * İçin **protokol türü**seçin **TCP**.
   * İçin **yerel bağlantı noktası**seçin **belirli bağlantı noktaları**. Aşağıdaki kutuya yapılandırıldı proxy bağlantı noktası sayısını belirtin.
   * İçin **uzak bağlantı noktası**seçin **tüm bağlantı noktaları**.

Sonuna ulaşana kadar Sihirbazı'nı geri kalanı için varsayılan ayarları kabul edin. Ardından bu kural, bir ad verin. 

#### <a name="step-3-add-an-exception-rule-to-the-nsg"></a>3. adım: NSG'yi bir özel durum kuralı ekleyin
Aşağıdaki komut, bir özel durum NSG'yi ekler. Bu özel bağlantı noktası 80 (HTTP) veya 443 (HTTPS) herhangi bir internet adresi 10.0.0.5 üzerinde herhangi bir bağlantı gelen TCP trafiğine izin verir. Ortak internet üzerindeki belirli bir bağlantı noktası gerekiyorsa, bu bağlantı noktasına eklediğinizden emin olun ```-DestinationPortRange```.

Bir Azure PowerShell komut isteminde aşağıdaki komutu girin:

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```

## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [Geri bildirimlerinizi bize gönderin](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Sonraki adımlar
VM yedeklemesi için ortamınızı hazırlandığınıza göre sonraki mantıksal bir yedekleme oluşturmak için adımdır. Planlama makale Vm'leri yedekleme konusunda daha ayrıntılı bilgi sağlar.

* [Sanal makineleri yedekleme](backup-azure-arm-vms.md)
* [VM yedekleme altyapınızı planlama](backup-azure-vms-introduction.md)
* [Sanal makine yedeklerini yönetme](backup-azure-manage-vms.md)
