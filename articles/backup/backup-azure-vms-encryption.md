---
title: Azure Backup kullanarak şifreli VM'ler geri
description: Bu makalede, Azure Disk şifrelemesi kullanılarak şifrelenmiş VM'ler için yedekleme ve geri yükleme deneyimi hakkında konuşuyor.
services: backup
author: sogup
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: sogup
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49361aef774e9eb5a0995bc106e73b236a71b0bb
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2018
ms.locfileid: "37441140"
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Yedekleme ve Azure Backup ile şifrelenmiş sanal makineleri geri yükleme
Bu makalede Azure Backup'ı kullanarak sanal makineleri (VM'ler) geri adım hakkında konuşuyor. Ayrıca hata durumları için desteklenen senaryolar, önkoşulları ve sorun giderme adımları hakkında ayrıntılar sağlar.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

 * Azure Resource Manager dağıtım modelini kullanan VM'ler için yedekleme ve geri yükleme şifrelenmiş VM'lerin desteklenir. Klasik dağıtım modelini kullanan VM'ler için desteklenmiyor. <br>
 * Hem Windows hem de Linux Vm'leri, Azure Disk Şifrelemesi'ni kullanmak için yedekleme ve geri yükleme şifrelenmiş VM'lerin desteklenir. Disk şifrelemesi, disk şifreleme sağlamak için sektörde standart BitLocker özelliğini Windows ve Linux'ın dm-crypt özelliğini kullanır. <br>
 
 Aşağıdaki tabloda, BitLocker şifreleme anahtarı (BEK) - yalnızca ve anahtar şifreleme anahtarı (KEK) - için desteklenen senaryolar şifrelenmiş VM'ler gösterilmektedir:
 
 
   |  | BEK ve KEK VM'ler | Yalnızca BEK VM'ler |
   | --- | --- | --- |
   | **Yönetilmeyen VM'ler**  | Evet | Evet  |
   | **Yönetilen VM'ler**  | Evet | Evet  |

## <a name="prerequisites"></a>Önkoşullar
* VM kullanılarak şifrelenen [Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md).

* Kurtarma Hizmetleri kasası oluşturuldu ve depolama çoğaltması içindeki adımları izleyerek ayarlandığı [yedekleme için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md).

* Yedekleme verilen [bir anahtar kasasına erişim izni](#provide-permissions-to-backup) anahtarları ve gizli anahtarları için içeren şifrelenmiş VM'ler.

## <a name="backup-encrypted-vm"></a>VM yedekleme şifreli
Yedekleme hedefi ayarlayın, bir ilkesi tanımlama, yapılandırma öğeleri ve bir yedeklemeyi tetikleyin için aşağıdaki adımları kullanın.

### <a name="configure-backup"></a>Yedeklemeyi yapılandırma
1. Açık kurtarma Hizmetleri kasası zaten varsa sonraki adıma geçin. Açık kurtarma Hizmetleri kasası yoksa ancak Azure portalında, select **tüm hizmetleri**.

   a. Kaynak listesinde **Kurtarma Hizmetleri** yazın.

   b. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

      ![Kurtarma Hizmetleri kasası](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. Kurtarma Hizmetleri kasalarının listesi görünür. Listeden bir kasa seçin.

     Seçilen kasa panosu açılır.
2. Kasa altında görüntülenen öğelerin listesinden **yedekleme** şifreli VM'yi yedekleme başlatmak için.

      ![Yedekleme dikey penceresi](./media/backup-azure-vms-encryption/select-backup.png)
3. Üzerinde **yedekleme** kutucuk seçin **yedekleme hedefi**.

      ![Senaryo dikey penceresi](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. Altında **iş yükünüz çalıştığı?** seçin **Azure**. Altında **neleri yedeklemek istiyorsunuz?** seçin **sanal makine**. Sonra **Tamam**’ı seçin.

   ![Senaryo dikey penceresini açma](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. Altında **yedekleme ilkesi seçmek**, kasaya uygulamak istediğiniz yedekleme ilkesini seçin. Sonra **Tamam**’ı seçin.

      ![Yedekleme ilkesini seçme](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Varsayılan ilkenin ayrıntıları listelenir. Bir ilke oluşturmak istiyorsanız seçin **Yeni Oluştur** aşağı açılan listeden. Seçtikten sonra **Tamam**, yedekleme İlkesi kasayla ilişkilendirilir.

6. Belirtilen ilke ile ilişkilendirmek ve şifrelenmiş Vm'leri seçin **Tamam**.

      ![Şifrelenmiş sanal makineleri seçin](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Bu sayfa, seçtiğiniz şifrelenmiş VM'ler ile ilişkili anahtar kasalarını hakkında bir ileti gösterir. Yedekleme, anahtarları ve gizli anahtarları key vault'ta yalnızca okuma erişimi gerektirir. Bu izinler, ilişkili sanal makinelerin yanı sıra gizli dizileri ve anahtarları yedeklemek için kullanır.<br>
Eğer bir **üye kullanıcı**, etkinleştirme Yedekleme işleminin sorunsuz bir şekilde için anahtar kasasına erişim elde yedekleme, kullanıcı müdahalesi gerektirmeden VM'ler şifreli.

   ![Şifrelenmiş Vm'leri message](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   İçin bir **Konuk kullanıcı**, yedekleme hizmetine çalışmak yedeklemeler için anahtar kasasına erişmek için gerekli izinleri sağlamanız gerekir. Takip ederek, bu izinleri sağlayabilirsiniz [aşağıdaki bölümde belirtilen adımlar](#provide-permissions-to-backup)

   ![Şifrelenmiş Vm'leri message](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)
 
    Kasa için tüm ayarları tanımladığınıza göre seçin **yedeklemeyi etkinleştir** sayfanın alt kısmındaki. **Yedeklemeyi etkinleştir** ilkeyi kasaya ve Vm'lere dağıtır.
  
8. VM Aracısı sonraki hazırlama aşamasında yüklüyor veya VM Aracısı emin olarak yüklenir. Aynı işlemi gerçekleştirmek için adımları [yedekleme için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md).

### <a name="trigger-a-backup-job"></a>Bir yedekleme işi tetikleme
Bağlantısındaki [Azure Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-arm-vms.md) bir yedekleme işini tetiklemek için.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Yedeklenen zaten VM yedekleri etkin şifreleme ile devam edin  
Daha sonra şifreleme için etkinleştirilen, bir kurtarma Hizmetleri kasasında zaten yedeklenen Vm'leriniz varsa, yedekleme devam etmek yedeklemeler için anahtar kasasına erişim izinleri vermeniz gerekir. Takip ederek, bu izinleri sağlayabilirsiniz [aşağıdaki bölümde adımları](#provide-permissions-to-azure-backup). Veya "Yedeklemeyi etkinleştir" bölümündeki PowerShell adımları izleyebilirsiniz [PowerShell belgeleri](backup-azure-vms-automation.md). 

## <a name="provide-permissions-to-backup"></a>Yedekleme için izinler sağlayın
Yedekleme için anahtar kasasına erişim ve şifrelenmiş vm'leri Yedekleme gerçekleştirmek için uygun izinlere sağlamak için aşağıdaki adımları kullanın.
1. Seçin **tüm hizmetleri**, araması **anahtar kasalarını**.

    ![Anahtar kasaları](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Yedeklenmesi gereken şifrelenmiş VM ile ilişkilendirilmiş key vault anahtar kasalarının listesinden seçin.

     ![Anahtar kasası seçimi](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Seçin **erişim ilkeleri**ve ardından **yeni Ekle**.

    ![Yeni ekle](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Seçin **Select sorumlusu**, Anahtar'a tıklayın ve **yedekleme yönetim hizmeti** arama kutusuna. 

    ![Yedekleme Hizmeti arama](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Seçin **yedekleme yönetim hizmeti**ve ardından **seçin**.

    ![Yedekleme hizmeti seçimi](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Altında **yapılandırma (isteğe bağlı) şablonundan**seçin **Azure Backup**. Gerekli izinler için doldurulmuş **anahtar izinleri** ve **gizli dizi izinleri**. Sanal makinenizin kullanılarak şifrelendiyse **yalnızca BEK**, seçimini kaldırmanız gerekir, böylece gizli izinleri yalnızca gerekli **anahtar izinleri**.

    ![Azure yedekleme seçimi](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. **Tamam**’ı seçin. Dikkat **yedekleme yönetim hizmeti** eklenen **erişim ilkeleri**. 

    ![Erişim ilkeleri](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Seçin **Kaydet** yedekleme için gerekli izinleri vermek için.

    ![Yedekleme erişim ilkesi](./media/backup-azure-vms-encryption/save-access-policy.png)

İzinler başarıyla sağlandıktan sonra şifreli VM'ler için yedekleme etkinleştirme işlemiyle devam edebilirsiniz.

## <a name="restore-an-encrypted-vm"></a>Şifrelenmiş bir sanal Makineyi geri yükleme
Şifrelenmiş bir sanal Makineyi geri yüklemek için önce diskleri "yedeklenen diskleri geri yükleme" kısmında bulunan adımları izleyerek geri [bir VM geri yükleme yapılandırması seçin](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Bundan sonra aşağıdaki seçeneklerden birini kullanabilirsiniz:

* PowerShell adımları izleyin [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) geri yüklenen disklerden tam bir VM oluşturmak için.
* Veya, [geri yüklenen VM özelleştirmek için şablonlar kullanın](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm) geri yüklenen disklerden sanal makineler oluşturmak için. Şablonlar, 26 Nisan 2017'den sonra oluşturulan kurtarma noktaları için kullanılabilir.

## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme
| İşlem | Hata ayrıntıları | Çözüm |
| --- | --- | --- |
|Backup | Yedekleme, şifrelenmiş VM'lerin anahtar kasasına yedekleme için yeterli izinlere sahip değil. | Yedekleme sağlanması bu izinleri izleyerek [önceki bölümdeki adımları](#provide-permissions-to-azure-backup). Ya da PowerShell belgeleri "korumayı etkinleştirme" bölümündeki PowerShell adımları izleyebilirsiniz [sanal makinelerini yedeklemek için AzureRM.RecoveryServices.Backup cmdlet'lerini](backup-azure-vms-automation.md#back-up-azure-vms). |  
| Geri Yükleme |Bu VM ile ilişkili anahtar kasası olmadığından bu şifreli VM'yi geri yükleyemezsiniz. |Kullanarak anahtar kasası oluşturma [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md). Bkz: [Azure Backup'ı kullanarak bir anahtar kasası anahtar ve gizli dizi geri](backup-azure-restore-key-secret.md) mevcut değilse, bir anahtar ve gizli dizi geri yüklemek için. |
| Geri Yükleme |Anahtar ve bu VM ile ilişkili gizli dizi mevcut olmadığından bu şifreli VM'yi geri yükleyemezsiniz. |Bkz: [Azure Backup'ı kullanarak bir anahtar kasası anahtar ve gizli dizi geri](backup-azure-restore-key-secret.md) mevcut değilse, bir anahtar ve gizli dizi geri yüklemek için. |
| Geri Yükleme |Yedekleme, aboneliğinizdeki kaynaklara erişme yetkisi yok. |Daha önce belirtildiği gibi diskleri önce "yedeklenen diskleri geri yükleme" kısmında bulunan adımları izleyerek geri [bir VM geri yükleme yapılandırması seçin](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Bundan sonra PowerShell'i kullanarak [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
