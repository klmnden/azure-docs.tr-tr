---
title: Yedekleme ve şifrelenmiş VM'ler Azure Yedekleme'yi kullanarak geri yükleme
description: Bu makalede, Azure Disk şifrelemesi kullanılarak şifrelenmiş VM'ler için yedekleme ve geri yükleme deneyimi hakkında alınmaktadır.
services: backup
author: JPallavi
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 10/13/2017
ms.author: pajosh
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 18a4b0f369ea7ed65945fd43a60cbf44589ca8f2
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34607295"
---
# <a name="back-up-and-restore-encrypted-virtual-machines-with-azure-backup"></a>Ve Azure yedekleme ile şifrelenmiş sanal makineleri geri yükleme
Bu makalede, yedekleme ve Azure Yedekleme'yi kullanarak sanal makineleri (VM'ler) geri yükleme adımlarını hakkında alınmaktadır. Ayrıca hata durumları için desteklenen senaryolar, önkoşulları ve sorun giderme adımları hakkında ayrıntılar sağlar.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

 * Yedekleme ve geri yükleme şifrelenmiş VM'lerin Azure Resource Manager dağıtım modelini kullanan VM'ler için desteklenir. Klasik dağıtım modelini kullanan sanal makineleri için desteklenmez. <br>
 * Hem Windows hem de Azure Disk Şifrelemesi'ni kullanmak Linux VM'ler için yedekleme ve geri yükleme şifrelenmiş VM'ler desteklenir. Disk şifrelemesi disk şifreleme sağlamak için endüstri standart BitLocker özelliği, Windows ve Linux dm-crypt özelliği kullanır. <br>
 
 Aşağıdaki tabloda, BitLocker şifreleme anahtarı (BEK) - yalnızca ve anahtar şifreleme anahtarı (KEK) - için desteklenen senaryolar şifrelenmiş VM'ler gösterilmektedir:
 
 
   |  | BEK + KEK VM'ler | Yalnızca BEK VM'ler |
   | --- | --- | --- |
   | **Yönetilmeyen VM'ler**  | Evet | Evet  |
   | **Yönetilen sanal makineleri**  | Evet | Evet  |

## <a name="prerequisites"></a>Önkoşullar
* VM kullanarak şifrelenmiş [Azure Disk şifrelemesi](../security/azure-security-disk-encryption.md).

* Kurtarma Hizmetleri kasası oluşturuldu ve depolama çoğaltma içindeki adımları izleyerek ayarlandığı [yedekleme için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md).

* Yedekleme verilen [bir anahtar kasası erişim izinleri](#provide-permissions-to-backup) anahtarları ve gizli anahtarları için içeren VM'ler şifrelenmiş.

## <a name="backup-encrypted-vm"></a>Yedekleme şifrelenmiş VM
Yedekleme hedefi ayarlamak, ilke tanımlamak, öğeleri yapılandırın ve bir yedeklemeyi tetikleyin için aşağıdaki adımları kullanın.

### <a name="configure-backup"></a>Yedeklemeyi yapılandırma
1. Açık kurtarma Hizmetleri kasası zaten varsa, sonraki adıma geçebilirsiniz. Açık kurtarma Hizmetleri kasası varsa yoktur, ancak Azure portalında, select olduğunuz **tüm hizmetleri**.

   a. Kaynak listesinde **Kurtarma Hizmetleri** yazın.

   b. Yazmaya başladığınızda liste, girişinize göre filtrelenir. Gördüğünüzde **kurtarma Hizmetleri kasaları**, onu seçin.

      ![Kurtarma Hizmetleri kasası](./media/backup-azure-vms-encryption/browse-to-rs-vaults.png) <br/>

    c. Kurtarma Hizmetleri kasalarının listesi görünür. Bir kasa listeden seçin.

     Seçilen kasa panosu açılır.
2. Kasa altında görüntülenen öğelerin listesinden **yedekleme** şifrelenmiş VM yedeklemesi başlatmak için.

      ![Yedekleme dikey penceresi](./media/backup-azure-vms-encryption/select-backup.png)
3. Üzerinde **yedekleme** kutucuğu, select **yedekleme hedefi**.

      ![Senaryo dikey penceresi](./media/backup-azure-vms-encryption/select-backup-goal-one.png)
4. Altında **, iş yükünü çalıştırdığı?** seçin **Azure**. Altında **neleri yedeklemek istiyorsunuz?** seçin **sanal makine**. Sonra **Tamam**’ı seçin.

   ![Senaryo dikey penceresini açma](./media/backup-azure-vms-encryption/select-backup-goal-two.png)
5. Altında **yedekleme ilkesi seçin**, kasaya uygulamak istediğiniz yedekleme ilkesini seçin. Sonra **Tamam**’ı seçin.

      ![Yedekleme ilkesini seçme](./media/backup-azure-vms-encryption/setting-rs-backup-policy-new.png)

    Varsayılan ilke ayrıntılarını listelenir. Bir ilke oluşturmak isteyip istemediğinizi seçin **Yeni Oluştur** aşağı açılan listeden. Siz seçtikten sonra **Tamam**, yedekleme İlkesi kasayla ilişkilendirilir.

6. Belirtilen ilke ile ilişkilendirmek ve seçmek için şifrelenmiş Vm'leri seçin **Tamam**.

      ![Şifrelenmiş sanal makineleri seçin](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)
7. Bu sayfada seçtiğiniz şifrelenmiş VM'ler ilişkili anahtar kasalarını hakkında bir ileti görüntülenir. Yedekleme, anahtarlar ve gizli anahtar kasasında salt okunur erişimi gerektirir. Bu izinler, anahtarları ve gizli anahtarları, ilişkili sanal makineleri birlikte yedeklemek için kullanır.<br>
Kullanıyorsanız bir **üye kullanıcı**, yedeklemeyi etkinleştir işlem sorunsuz bir şekilde anahtar Kasası'na erişim elde yedekleme, kullanıcı müdahalesi gerektirmeden VM'ler şifrelenmiş.

   ![Şifrelenmiş VM'ler ileti](./media/backup-azure-vms-encryption/member-user-encrypted-vm-warning-message.png)

   İçin bir **Konuk kullanıcı**, anahtar kasası yedeklemelerinin çalışmak erişim izni yedekleme hizmetine sağlamanız gerekir. Bu izinleri izleyerek sağlayabilir [aşağıdaki bölümde belirtilen adımlar](#provide-permissions-to-backup)

   ![Şifrelenmiş VM'ler ileti](./media/backup-azure-vms-encryption/guest-user-encrypted-vm-warning-message.png)
 
    Kasa için tüm ayarları tanımladığınız, seçin **yedeklemeyi etkinleştir** sayfanın sonundaki. **Yedeklemeyi etkinleştirme** ilkeyi kasaya ve Vm'lere dağıtır.
  
8. Hazırlık sonraki aşamasında VM Aracısı yükleme veya VM Aracısı emin yüklü. Aynı yapmak için adımları [yedekleme için ortamınızı hazırlama](backup-azure-arm-vms-prepare.md).

### <a name="trigger-a-backup-job"></a>Bir yedekleme işi tetikleyeceğinden
Adımları [bir kurtarma Hizmetleri kasasına yedekleme Azure VM'ler](backup-azure-arm-vms.md) yedekleme işini tetikleyecek.

### <a name="continue-backups-of-already-backed-up-vms-with-encryption-enabled"></a>Yedeklenen zaten VM'ler yedeklerini etkin şifrelemesi ile devam et  
Daha sonra şifreleme için etkinleştirilmiş bir kurtarma Hizmetleri kasasına zaten yedeklenen Vm'leriniz varsa, yedekleme devam etmek yedeklemeler için anahtar kasasını erişmek için izinleri vermeniz gerekir. Bu izinleri izleyerek sağlayabilir [aşağıdaki bölümde adımları](#provide-permissions-to-azure-backup). Veya "Yedeklemeyi etkinleştir" bölümündeki PowerShell adımları izleyebilirsiniz [PowerShell belgelerine](backup-azure-vms-automation.md). 

## <a name="provide-permissions-to-backup"></a>Yedekleme için izinler sağlar
Anahtar kasası erişmek ve şifrelenmiş Vm'leri Yedekleme gerçekleştirmek için yedekleme için ilgili izinleri sağlamak için aşağıdaki adımları kullanın.
1. Seçin **tüm hizmetleri**, arayın ve **anahtar kasalarını**.

    ![Anahtar kasaları](./media/backup-azure-vms-encryption/search-key-vault.png)
    
2. Yedeklenmesi gereken şifrelenmiş VM ile ilişkili anahtar kasası anahtar kasalarının listesinden seçin.

     ![Anahtar kasası seçimi](./media/backup-azure-vms-encryption/select-key-vault.png)
     
3. Seçin **erişim ilkeleri**ve ardından **yeni Ekle**.

    ![Yeni ekle](./media/backup-azure-vms-encryption/select-key-vault-access-policy.png)
    
4. Seçin **Select asıl**ve ardından **yedekleme yönetim hizmeti** arama kutusuna. 

    ![Yedekleme Hizmeti arama](./media/backup-azure-vms-encryption/search-backup-service.png)
    
5. Seçin **yedekleme yönetim hizmeti**ve ardından **seçin**.

    ![Yedekleme hizmeti seçimi](./media/backup-azure-vms-encryption/select-backup-service.png)
    
6. Altında **yapılandırma (isteğe bağlı) şablondan**seçin **Azure Backup**. Gerekli izinler için doldurulmuş **anahtar izinleri** ve **gizli izinleri**. VM kullanarak şifrelenmişse **BEK yalnızca**, parolaları yalnızca izinlerini gereklidir, seçimini kaldırmanız gerekir böylece **anahtar izinleri**.

    ![Azure yedekleme seçimi](./media/backup-azure-vms-encryption/select-backup-template.png)
    
7. **Tamam**’ı seçin. Dikkat **yedekleme yönetim hizmeti** eklenen **erişim ilkeleri**. 

    ![Erişim ilkeleri](./media/backup-azure-vms-encryption/backup-service-access-policy.png)
    
8. Seçin **kaydetmek** yedekleme için gerekli izinleri vermek için.

    ![Yedekleme erişim ilkesi](./media/backup-azure-vms-encryption/save-access-policy.png)

İzinleri başarıyla girildikten sonra yedekleme şifrelenmiş VM'ler için etkinleştirme işlemiyle devam edebilirsiniz.

## <a name="restore-an-encrypted-vm"></a>Şifrelenmiş bir VM geri yükleme
Şifrelenmiş bir VM geri yüklemek için önce disk "yedeklenen diskleri geri yükle" bölümünde açıklanan adımları izleyerek geri [VM geri yükleme yapılandırmasını seçin](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Bundan sonra aşağıdaki seçeneklerden birini kullanabilirsiniz:

* PowerShell adımları izleyin [geri yüklenen disklerden bir VM oluşturmak](backup-azure-vms-automation.md#create-a-vm-from-restored-disks) geri yüklenen disklerden tam bir VM oluşturmak için.
* Veya, [geri yüklenen VM özelleştirmek için şablonlar kullanın](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm) geri yüklenen disklerden VM'ler oluşturmak için. Şablonları yalnızca 26 Nisan 2017 sonra oluşturulan kurtarma noktaları için kullanılabilir.

## <a name="troubleshooting-errors"></a>Hatalarda sorun giderme
| İşlem | Hata ayrıntıları | Çözüm |
| --- | --- | --- |
|Backup | Yedekleme şifrelenmiş VM'nin anahtar kasasına yedekleme için yeterli izinlere sahip değil. | Yedekleme sağlanmalıdır: Bu izinleri izleyerek [önceki bölümdeki adımları](#provide-permissions-to-azure-backup). Veya PowerShell belgelerine "korumayı etkinleştir" bölümünde PowerShell adımları izleyebilirsiniz [sanal makineleri yedeklemek için kullanım AzureRM.RecoveryServices.Backup cmdlet'leri](backup-azure-vms-automation.md#back-up-azure-vms). |  
| Geri Yükleme |Bu VM ile ilişkili anahtar kasası var olmadığı için bu şifrelenmiş VM geri yükleyemezsiniz. |Kullanarak bir anahtar kasası oluşturma [Azure anahtar kasası ile çalışmaya başlama](../key-vault/key-vault-get-started.md). Bkz: [Azure Yedekleme'yi kullanarak bir anahtar kasası anahtarı ve gizli geri](backup-azure-restore-key-secret.md) mevcut değilse bir anahtarı ve gizli geri yüklemek için. |
| Geri Yükleme |Anahtar ve bu VM ile ilişkili gizli mevcut değil çünkü bu şifrelenmiş VM geri yükleyemezsiniz. |Bkz: [Azure Yedekleme'yi kullanarak bir anahtar kasası anahtarı ve gizli geri](backup-azure-restore-key-secret.md) mevcut değilse bir anahtarı ve gizli geri yüklemek için. |
| Geri Yükleme |Yedekleme aboneliğinizi kaynaklara erişim yetkisi yok. |Daha önce belirtildiği gibi "yedeklenen diskleri geri yükle" bölümünde açıklanan adımları izleyerek diskleri ilk geri [VM geri yükleme yapılandırmasını seçin](backup-azure-arm-restore-vms.md#choose-a-vm-restore-configuration). Bundan sonra powershell'den [geri yüklenen disklerden bir VM oluşturmak](backup-azure-vms-automation.md#create-a-vm-from-restored-disks). |
