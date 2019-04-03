---
title: Yedekleme ve Azure Backup Hizmetleri kullanılarak şifrelenmiş Azure Vm'lerini geri yükleme
description: Bu makalede, Azure Disk şifrelemesi kullanılarak şifrelenmiş VM'ler için yedekleme ve geri yükleme deneyimi hakkında konuşuyor.
services: backup
author: geetha
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 7/10/2018
ms.author: geetha
ms.openlocfilehash: 28126df0dfd9a03e93a76fa5071331603c4819a4
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2019
ms.locfileid: "58851027"
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Yedekleme ve Azure Backup ile şifrelenmiş sanal makineleri geri yükleme
Bu makalede Azure Backup'ı kullanarak sanal makineleri (VM'ler) geri adım hakkında konuşuyor. Ayrıca hata durumları için desteklenen senaryolar, önkoşulları ve sorun giderme adımları hakkında ayrıntılar sağlar.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

 Azure Resource Manager dağıtım modelini kullanan VM'ler için yedekleme ve geri yükleme şifrelenmiş VM'lerin desteklenir. Klasik dağıtım modelini kullanan VM'ler için desteklenmiyor. Windows ve Linux Vm'leri, Azure Disk Şifrelemesi'ni kullanmak için yedekleme ve geri yükleme şifrelenmiş VM'lerin desteklenir. Disk şifrelemesi, disk şifreleme sağlamak için sektörde standart BitLocker özelliğini Windows ve Linux'ın dm-crypt özelliğini kullanır. Aşağıdaki tabloda şifreleme türü ve VM'ler için destek gösterilmektedir.

   |  | BEK ve KEK VM'ler | Yalnızca BEK VM'ler |
   | --- | --- | --- |
   | **Yönetilmeyen VM'ler**  | Evet | Evet  |
   | **Yönetilen VM'ler**  | Evet | Evet  |

   > [!NOTE]
   > Azure Backup, tek başına anahtarlar kullanılarak şifrelendi Vm'leri destekler. Bir VM şifrelemek için kullanılan bir sertifika bir parçası olan herhangi bir tuşa bugün desteklenmiyor.
   >

## <a name="prerequisites"></a>Önkoşullar
* VM kullanılarak şifrelenen [Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md).

* Kurtarma Hizmetleri kasası oluşturuldu ve depolama çoğaltması içindeki adımları izleyerek ayarlandığı [yedekleme için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md).

* Yedekleme şifreli VM'ler için anahtarları ve gizli anahtarları içeren bir anahtar kasasına erişim izni verildi.

## <a name="back-up-an-encrypted-vm"></a>Şifrelenmiş bir VM'yi yedekleme
Yedekleme hedefi ayarlayın, bir ilkesi tanımlama, yapılandırma öğeleri ve bir yedeklemeyi tetikleyin için aşağıdaki adımları kullanın.

### <a name="configure-backup"></a>Yedeklemeyi yapılandırma
1. Açık kurtarma Hizmetleri kasası zaten varsa sonraki adıma geçin. Açık kurtarma Hizmetleri kasası yoksa ancak Azure portalında, select **tüm hizmetleri**.

   a. Kaynak listesinde **Kurtarma Hizmetleri** yazın.

   b. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

      ![Kurtarma Hizmetleri kasası](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. Kurtarma Hizmetleri kasalarının listesi görünür. Listeden bir kasa seçin.

     Seçilen kasa panosu açılır.
1. Kasa altında görüntülenen öğelerin listesinden **yedekleme** şifreli VM'yi yedekleme başlatmak için.

      ![Yedekleme dikey penceresi](./media/backup-azure-vms-encryption/select-backup.png)
1. Üzerinde **yedekleme** kutucuk seçin **yedekleme hedefi**.

      ![Senaryo dikey penceresi](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
1. Altında **iş yükünüz çalıştığı?** seçin **Azure**. Altında **neleri yedeklemek istiyorsunuz?** seçin **sanal makine**. Sonra **Tamam**’ı seçin.

   ![Senaryo dikey penceresini açma](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
1. Altında **yedekleme ilkesi seçmek**, kasaya uygulamak istediğiniz yedekleme ilkesini seçin. Sonra **Tamam**’ı seçin.

      ![Yedekleme ilkesini seçme](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Varsayılan ilkenin ayrıntıları listelenir. Bir ilke oluşturmak istiyorsanız seçin **Yeni Oluştur** aşağı açılan listeden. Seçtikten sonra **Tamam**, yedekleme İlkesi kasayla ilişkilendirilir.

1. Belirtilen ilke ile ilişkilendirmek ve şifrelenmiş Vm'leri seçin **Tamam**.

      ![Şifrelenmiş sanal makineleri seçin](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
1. Bu sayfa, seçtiğiniz şifrelenmiş VM'ler ile ilişkili anahtar kasalarını hakkında bir ileti gösterir. Yedekleme, anahtarları ve gizli anahtarları key vault'ta yalnızca okuma erişimi gerektirir. Bu izinler, ilişkili sanal makinelerin yanı sıra gizli dizileri ve anahtarları yedeklemek için kullanır.<br>
Eğer bir **üye kullanıcı**, etkinleştirme Yedekleme işleminin sorunsuz bir şekilde herhangi bir kullanıcı müdahalesi gerektirmeden şifrelenmiş Vm'leri yedekleme için anahtar kasasına erişim elde.

   ![Şifrelenmiş Vm'leri message](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   İçin bir **Konuk kullanıcı**, yedekleme hizmetine çalışmak yedeklemeler için anahtar kasasına erişmek için gerekli izinleri sağlamanız gerekir. Aşağıdaki bölümde anlatılan adımları izleyerek bu izinleri sağlayabilir.

   ![Şifrelenmiş Vm'leri message](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)

    Kasa için tüm ayarları tanımladığınıza göre seçin **yedeklemeyi etkinleştir** sayfanın alt kısmındaki. **Yedeklemeyi etkinleştir** ilkeyi kasaya ve Vm'lere dağıtır.

1. VM Aracısı sonraki hazırlama aşamasında yüklüyor veya VM Aracısı emin olarak yüklenir. Aynı işlemi gerçekleştirmek için adımları [yedekleme için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md).

### <a name="trigger-a-backup-job"></a>Bir yedekleme işi tetikleme
Bağlantısındaki [Azure Vm'leri bir kurtarma Hizmetleri kasasına yedekleme](backup-azure-arm-vms.md) bir yedekleme işini tetiklemek için.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Yedeklenen zaten VM yedekleri etkin şifreleme ile devam edin  
Daha sonra şifreleme için etkinleştirilen, bir kurtarma Hizmetleri kasasında zaten yedeklenen Vm'leriniz varsa, devam etmek yedeklemeler için anahtar kasası erişim kadar geri izinleri vermelisiniz. Takip ederek, bu izinleri sağlayabilirsiniz [aşağıdaki bölümde adımları](#provide-permissions). Veya "Yedeklemeyi etkinleştir" bölümündeki PowerShell adımları izleyebilirsiniz [PowerShell belgeleri](backup-azure-vms-automation.md).

## <a name="provide-permissions"></a>İzinler sağlayın
Anahtar kasasına erişim ve şifrelenmiş vm'leri Yedekleme gerçekleştirmek Azure Backup için ilgili izinleri sağlamak için aşağıdaki adımları kullanın.
1. Seçin **tüm hizmetleri**, araması **anahtar kasalarını**.

    ![Anahtar kasaları](./media/backup-azure-vms-encryption/search-key-vault.png)

1. Yedeklenmesi gereken şifrelenmiş VM ile ilişkilendirilmiş key vault anahtar kasalarının listesinden seçin.

     ![Anahtar kasası seçimi](./media/backup-azure-vms-encryption/select-key-vault.png)

1. Seçin **erişim ilkeleri**ve ardından **yeni Ekle**.

    ![Yeni ekle](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)

1. Seçin **Select sorumlusu**, Anahtar'a tıklayın ve **yedekleme yönetim hizmeti** arama kutusuna.

    ![Yedekleme Hizmeti arama](./media/backup-azure-vms-encryption/search-backup-service.png)

1. Seçin **yedekleme yönetim hizmeti**ve ardından **seçin**.

    ![Yedekleme hizmeti seçimi](./media/backup-azure-vms-encryption/select-backup-service.png)

1. Altında **yapılandırma (isteğe bağlı) şablonundan**seçin **Azure Backup**. Gerekli izinler için doldurulmuş **anahtar izinleri** ve **gizli dizi izinleri**. Sanal makinenizin kullanılarak şifrelendiyse **yalnızca BEK**, seçimini kaldırmanız gerekir, böylece gizli izinleri yalnızca gerekli **anahtar izinleri**.

    ![Azure yedekleme seçimi](./media/backup-azure-vms-encryption/select-backup-template.png)

1. **Tamam**’ı seçin. Dikkat **yedekleme yönetim hizmeti** eklenen **erişim ilkeleri**.

    ![Erişim ilkeleri](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

1. Seçin **Kaydet** yedekleme için gerekli izinleri vermek için.

    ![Yedekleme erişim ilkesi](./media/backup-azure-vms-encryption/save-access-policy.png)

İzinler başarıyla sağlandıktan sonra şifreli VM'ler için yedekleme etkinleştirme işlemiyle devam edebilirsiniz.

## <a name="restore-an-encrypted-vm"></a>Şifrelenmiş bir sanal Makineyi geri yükleme
Azure Backup artık destekliyor geri yüklemesi [Azure, Azure AD olmayan VM şifreli](https://docs.microsoft.com/azure/security/azure-security-disk-encryption-prerequisites-aad) yanı sıra önceki teklifi geri yükleme desteği Azure için Azure AD ile VM şifreli.<br>

Şifrelenmiş bir sanal Makineyi geri yüklemek için önce diskleri "yedeklenen diskleri geri yükleme" kısmında bulunan adımları izleyerek geri [bir VM geri yükleme yapılandırması seçin](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Bundan sonra aşağıdaki seçeneklerden birini kullanabilirsiniz:

* PowerShell adımları izleyin [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) geri yüklenen disklerden tam bir VM oluşturmak için.
* Veya, [geri yüklenen VM özelleştirmek için şablonlar kullanın](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm) geri yüklenen disklerden sanal makineler oluşturmak için. Şablonlar, 26 Nisan 2017'den sonra oluşturulan kurtarma noktaları için kullanılabilir.

## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme
| İşlem | Hata Ayrıntıları | Çözüm |
| --- | --- | --- |
|Backup | Hata kodu: UserErrorKeyVaultPermissionsNotConfigured<br><br>Hata iletisi: Azure Backup hizmeti yedekleme, şifrelenmiş sanal makineleri için yeterli izinlere için Key Vault yok. | Yedekleme sağlanması bu izinleri izleyerek [önceki bölümdeki adımları](#provide-permissions). Veya makalenin "korumayı etkinleştirme" bölümündeki PowerShell adımları izleyebilirsiniz [yedeklemek ve sanal makineleri geri yükleme için PowerShell kullanma](backup-azure-vms-automation.md#enable-protection). |  
| Geri Yükleme | Bu VM ile ilişkili anahtar kasası olmadığından bu şifreli VM'yi geri yükleyemezsiniz. |Kullanarak anahtar kasası oluşturma [Azure anahtar kasası nedir?](../key-vault/key-vault-overview.md). Bkz: [Azure Backup'ı kullanarak bir anahtar kasası anahtar ve gizli dizi geri](backup-azure-restore-key-secret.md) mevcut değilse, bir anahtar ve gizli dizi geri yüklemek için. |
| Geri Yükleme | Hata kodu: UserErrorKeyVaultKeyDoesNotExist<br><br> Hata iletisi: Bu VM ile ilişkili anahtar mevcut olmadığından bu şifreli VM'yi geri yükleyemezsiniz. |Bkz: [Azure Backup'ı kullanarak bir anahtar kasası anahtar ve gizli dizi geri](backup-azure-restore-key-secret.md) mevcut değilse, bir anahtar ve gizli dizi geri yüklemek için. |
| Geri Yükleme | Hata kodu: ProviderAuthorizationFailed/UserErrorProviderAuthorizationFailed<br><br>Hata iletisi: Yedekleme Hizmeti'nin aboneliğinizdeki kaynaklara erişme yetkisi yok. |Daha önce belirtildiği gibi diskleri önce "yedeklenen diskleri geri yükleme" kısmında bulunan adımları izleyerek geri [bir VM geri yükleme yapılandırması seçin](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Bundan sonra PowerShell'i kullanarak [geri yüklenen diskten VM oluşturma](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
