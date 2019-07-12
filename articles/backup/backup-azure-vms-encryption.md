---
title: Yedekleme ve Azure Backup ile şifrelenmiş Azure Vm'lerini geri yükleme
description: Azure Backup hizmeti ile şifrelenmiş Azure Vm'lerini geri açıklar.
services: backup
author: geetha
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 4/3/2019
ms.author: geg
ms.openlocfilehash: 24ae6ddae30110f6d125158d6f2744bf4eae5006
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67704986"
---
# <a name="back-up-and-restore-encrypted-azure-vm"></a>Yedekleme ve şifrelenmiş Azure VM geri yükleme

Bu makalede ile şifrelenmiş diskler kullanarak yedekleme ve geri yükleme Windows veya Linux Azure sanal makineleri (VM'ler) nasıl [Azure Backup](backup-overview.md) hizmeti.

Başlamadan önce nasıl Azure Backup ile Azure Vm'leri etkileşim hakkında daha fazla bilgi istiyorsanız, bu kaynakları gözden geçirin:

- [Gözden geçirme](backup-architecture.md#architecture-direct-backup-of-azure-vms) Azure VM yedekleme mimarisi.
- [Hakkında bilgi edinin](backup-azure-vms-introduction.md) Azure VM yedeklemesi ve Azure Backup uzantısı.

## <a name="encryption-support"></a>Şifreleme desteği

Azure Backup, şifrelenmiş işletim sistemi/veri disklerini ile Azure Disk şifrelemesi (ADE) olan Azure Vm'lerinin yedeklenmesini destekler. ADE Linux Vm'leri için Windows Vm'leri dm-crypt özelliğini ve şifreleme için BitLocker kullanır. ADE disk şifreleme anahtarlarını ve gizli anahtarları yönetmek için Azure anahtar kasası ile tümleştirilir. Key Vault anahtar şifreleme anahtarları (Dünyaları) ek bir güvenlik katmanı eklemek için anahtar Kasası'na yazmadan önce şifreleme parolaları şifreleme kullanılabilir.

Azure Backup, yedekleme ve aşağıdaki tabloda özetlendiği gibi ADE ve Azure AD uygulaması olmadan kullanarak Azure Vm'leri geri yükleyebilirsiniz.

**VM disk türü** | **ADE (BEK/dm-crypt)** | **ADE ve KEK**
--- | --- | ---
**Yönetilmeyen** | Evet | Evet
**Yönetilen**  | Evet | Evet

- Daha fazla bilgi edinin [ADE](../security/azure-security-disk-encryption-overview.md), [Key Vault](../key-vault/key-vault-overview.md), ve [Dünyaları](https://blogs.msdn.microsoft.com/cclayton/2017/01/03/creating-a-key-encrypting-key-kek/).
- Okuma [SSS](../security/azure-security-disk-encryption-faq.md) Azure VM disk şifrelemesi için.



### <a name="limitations"></a>Sınırlamalar

- Yedekleme ve aynı abonelik ve bölge içinde şifrelenmiş Vm'leri geri yükleme.
- Azure Backup, tek başına anahtarlar kullanılarak şifrelendi Vm'leri destekler. Bir VM şifrelemek için kullanılan bir sertifika bir parçası olan herhangi bir tuşa şu anda desteklenmemektedir.
- Yedekleme ve aynı abonelik ve kurtarma Hizmetleri yedekleme kasasıyla bölge içinde şifrelenmiş Vm'leri geri yükleme.
- Dosya/klasör düzeyinde şifrelenmiş Vm'leri geri alınamaz. Dosya ve klasörleri geri yüklemek için tüm VM kurtarmanız gerekecektir.
- Bir VM geri yüklenirken kullanamazsınız [var olan VM değiştirin](backup-azure-arm-restore-vms.md#restore-options) şifreli VM'ler için seçenek. Bu seçenek yalnızca şifrelenmemiş yönetilen diskler için desteklenir.




## <a name="before-you-start"></a>Başlamadan önce

Başlamadan önce aşağıdakileri yapın:

1. Bir veya daha fazla olduğundan emin olun [Windows](../security/azure-security-disk-encryption-windows.md) veya [Linux](../security/azure-security-disk-encryption-linux.md) ADE Vm'lerle etkin.
2. [Destek matrisi gözden](backup-support-matrix-iaas.md) Azure VM yedeklemesi
3. [Oluşturma](backup-azure-arm-vms-prepare.md#create-a-vault) , yoksa, bir kurtarma Hizmetleri Backup kasası.
4. Yedekleme için zaten etkinleştirilmiş olan VM'ler için şifrelemeyi etkinleştirirseniz, yalnızca yedekleme kesintisiz devam edebilmesi için Key Vault erişim izinleri olan yedekleme sağlamanız gerekir. [Daha fazla bilgi edinin](#provide-permissions) bu izinleri atama hakkında.

Ayrıca, birkaç bazı durumlarda yapmanız gerekebilecek şey vardır:

- **VM Aracısı VM üzerinde yükleme**: Azure yedekleme makine üzerinde çalışan Azure VM aracısı için bir uzantı yükleyerek Azure sanal makinelerini yedekler. Sanal makinenize bir Azure Market görüntüsünden oluşturulduysa, aracı yüklü ve çalışır durumdadır. Özel bir VM oluşturmak veya bir şirket içi makineyi geçirmek gerekebilir [aracıyı el ile yükleme](backup-azure-arm-vms-prepare.md#install-the-vm-agent).
- **Açıkça giden erişime izin**: Genel olarak, giden ağ erişimi için Azure Backup ile iletişim kurmak bir Azure VM sırayla açıkça izin vermeniz gerekmez. Gösteren bazı VM'ler bağlantı sorunları, ancak karşılaşabilirsiniz **ExtensionSnapshotFailedNoNetwork** bağlanmaya çalışılırken bir hata oluştu. Böyle bir durumda, gereken [açıkça giden erişime izin](backup-azure-arm-vms-prepare.md#explicitly-allow-outbound-access), Azure Backup uzantısı yedekleme trafiği için Azure genel IP adresleri ile iletişim kurabilirsiniz.



## <a name="configure-a-backup-policy"></a>Bir yedekleme ilkesi yapılandırma

1. Bir kurtarma Hizmetleri kasasına yedekleme henüz oluşturmadıysanız, izleyin [bu yönergeleri](backup-azure-arm-vms-prepare.md#create-a-vault)
2. Kasa portalda açın ve seçin **yedekleme** içinde **Başlarken** bölümü.

    ![Yedekleme dikey penceresi](./media/backup-azure-vms-encryption/select-backup.png)

3. İçinde **yedekleme hedefi** > **iş yükünüz çalıştığı?** seçin **Azure**.
4. İçinde **neleri yedeklemek istiyorsunuz?** seçin **sanal makine** > **Tamam**.

      ![Senaryo dikey penceresi](./media/backup-azure-vms-encryption/select-backup-goal-one.png)

5. İçinde **yedekleme İlkesi** > **yedekleme ilkesi seçmek**, kasa ile ilişkilendirmek istediğiniz ilkeyi seçin. Daha sonra, **Tamam**'a tıklayın.
    - Bir yedekleme İlkesi yedeklemeleri ne zaman alınacağının ve ne kadar süreyle saklanır belirtir.
    - Varsayılan ilkenin ayrıntıları, açılan menü altında listelenir.

    ![Senaryo dikey penceresini açma](./media/backup-azure-vms-encryption/select-backup-goal-two.png)

6. Varsayılan ilkeyi kullanmak istemiyorsanız seçin **Yeni Oluştur**, ve [özel bir ilke oluşturmak](backup-azure-arm-vms-prepare.md#create-a-custom-policy).


7. Select İlkesi'ni kullanarak yedeklemek istediğiniz şifrelenmiş Vm'leri seçip **Tamam**.

      ![Şifrelenmiş sanal makineleri seçin](./media/backup-azure-vms-encryption/selected-encrypted-vms.png)

8. Azure Key Vault, kasa sayfada kullanıyorsanız, Azure Backup'ın anahtarlar ve gizli anahtarları Key vault'ta salt okunur erişmesi gereken bir ileti görürsünüz.

    - Bu iletiyi alırsanız, hiçbir eylem gerekmiyor.

        ![Erişim Tamam](./media/backup-azure-vms-encryption/access-ok.png)

    - Bu iletiyi alırsanız açıklandığı izinleri ayarlamak gereken [aşağıdaki yordamı](#provide-permissions).

        ![Erişim Uyarısı](./media/backup-azure-vms-encryption/access-warning.png)

9. Tıklayın **yedeklemeyi etkinleştir** kasadaki yedekleme ilkesini dağıtma ve seçili sanal makineler için yedeklemeyi etkinleştirin.


## <a name="trigger-a-backup-job"></a>Bir yedekleme işi tetikleme

İlk yedeklemeyi zamanlamaya uygun olarak çalışır, ancak bunu hemen aşağıdaki gibi çalıştırabilirsiniz:

1. Kasa menüden **yedekleme öğeleri**.
2. İçinde **yedekleme öğeleri** tıklayın **Azure sanal makine**.
3. İçinde **yedekleme öğeleri** listesinde, üç nokta (...) tıklayın.
4. Tıklayın **Şimdi Yedekle**.
5. İçinde **Şimdi Yedekle**, kurtarma noktası korunması gereken son günü seçmek için takvim denetimini kullanın. Daha sonra, **Tamam**'a tıklayın.
6. Portal bildirimlerini izleyin. Kasa panosunda iş ilerleme durumunu izleyebilirsiniz > **yedekleme işleri** > **sürüyor**. VM’nizin boyutuna bağlı olarak, ilk yedeklemenin oluşturulması biraz zaman alabilir.


## <a name="provide-permissions"></a>İzinler sağlayın

Azure VM ilişkili sanal makinelerin yanı sıra gizli dizileri ve anahtarları yedeklemek için salt okunur erişim verilmesi gerekir.

- Key Vault, Azure aboneliğinin Azure AD kiracınız ile ilişkilidir. Size bir **üye kullanıcı**, Azure Backup, başka bir eylem olmadan Key vault'a erişim alır.
- Size bir **Konuk kullanıcı**, Azure Backup'ın anahtar kasasına erişmek için gerekli izinleri sağlamanız gerekir.

İzinleri ayarlamak için:

1. Azure portalında **tüm hizmetleri**, araması **anahtar kasalarını**.
2. Yedekleme yapıyorsanız şifrelenmiş VM ile ilişkili anahtar Kasası'nı seçin.
3. Seçin **erişim ilkeleri** > **yeni Ekle**.
4. Seçin **Select sorumlusu**, Anahtar'a tıklayın ve **yedekleme Yönetim**.
5. Seçin **Backup yönetim hizmeti** > **seçin**.

    ![Yedekleme hizmeti seçimi](./media/backup-azure-vms-encryption/select-backup-service.png)

6. İçinde **Erişim İlkesi Ekle** > **yapılandırma (isteğe bağlı) şablonundan**seçin **Azure Backup**.
    - Gerekli izinler için doldurulmuş **anahtar izinleri** ve **gizli dizi izinleri**.
    - Sanal makinenizin kullanılarak şifrelendiyse **yalnızca BEK**, seçimini kaldırın **anahtar izinleri** gizli dizileri için izinleri yalnızca gerekli olduğundan.

    ![Azure yedekleme seçimi](./media/backup-azure-vms-encryption/select-backup-template.png)

6. **Tamam**'ı tıklatın. **Backup Yönetimi Hizmeti** eklenir **erişim ilkeleri**.

    ![Erişim ilkeleri](./media/backup-azure-vms-encryption/backup-service-access-policy.png)

7. Tıklayın **Kaydet** izinlerle Azure yedekleme sağlamak için.

## <a name="restore-an-encrypted-vm"></a>Şifrelenmiş bir sanal Makineyi geri yükleme

Şifrelenmiş Vm'leri gibi geri yükleyin:

1. [Sanal makine diskini geri yükleme](backup-azure-arm-restore-vms.md#restore-disks).
2. Ardından aşağıdakilerden birini yapın:
    - VM ayarlarını özelleştirmek ve VM dağıtımı tetiklemek için geri yükleme işlemi sırasında oluşturulan şablonu kullanın. [Daha fazla bilgi edinin](backup-azure-arm-restore-vms.md#use-templates-to-customize-a-restored-vm).
    - Powershell kullanarak geri yüklenen disklerden yeni bir VM oluşturun. [Daha fazla bilgi edinin](backup-azure-vms-automation.md#create-a-vm-from-restored-disks).

## <a name="next-steps"></a>Sonraki adımlar

Herhangi bir sorunla karşılaşırsanız, gözden geçirin

- [Sık karşılaşılan](backup-azure-vms-troubleshoot.md) , yedekleme ve geri yükleme şifrelenmiş Azure Vm'leri.
- [Azure VM Aracısı/yedekleme uzantısı](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md) sorunları.
