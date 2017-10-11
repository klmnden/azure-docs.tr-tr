---
title: "Azure için Windows sistem durumu yedekleme | Microsoft Docs"
description: "Sistem durumunu Windows Server ve/veya Windows bilgisayarları için Azure yedekleme öğrenin."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "yedekleme nasıl yapılır; yedekleme; dosya ve klasör yedekleme"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: 6fbd96935f444d8b0c6d068ebd0d28e612f19816
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a>Resource Manager dağıtım Windows sistem durumu yedekleme
Bu makalede, Azure için Windows Server sistem durumunu yedekleme açıklanmaktadır. Bu, size temel işlemler boyunca yol göstermeye yönelik bir öğreticidir.

Azure Backup hakkında daha fazla bilgi edinmek istiyorsanız bu [genel bakışı](backup-introduction-to-azure-backup.md) okuyun.

Azure aboneliğiniz yoksa istediğiniz Azure hizmetine erişmenizi sağlayan [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.

## <a name="create-a-recovery-services-vault"></a>Kurtarma hizmetleri kasası oluşturma
Dosya ve klasörlerinizi yedeklemek için, verileri depolamak istediğiniz bölgede bir Kurtarma Hizmetleri kasası oluşturmanız gerekir. Ayrıca, depolama alanınızın nasıl çoğaltılmasını istediğinizi belirlemeniz gerekir.

### <a name="to-create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturmak için
1. Önceden yapmadıysanız Azure aboneliğinizi kullanarak [Azure Portal](https://portal.azure.com/)'da oturum açın.
2. Hub menüsünde **Diğer hizmetler**'e tıklayın ve kaynak listesinde **Kurtarma Hizmetleri** yazıp **Kurtarma Hizmetleri kasaları** seçeneğine tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 1. adım](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    Abonelikte kurtarma hizmetleri kasaları varsa kasalar listelenir.
3. **Kurtarma Hizmetleri kasaları** menüsünde **Ekle**'ye tıklayın.

    ![Kurtarma Hizmetleri Kasası oluşturma 2. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    Kurtarma Hizmetleri kasası dikey penceresi açılır ve sizden bir **Ad**, **Abonelik**, **Kaynak Grubu** ve **Konum** sağlamanızı ister.

    ![Kurtarma Hizmetleri Kasası Oluşturma 3. adım](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. **Ad** alanına, kasayı tanımlayacak kolay bir ad girin. Adın Azure aboneliği için benzersiz olması gerekir. 2 ila 50 karakterden oluşan bir ad yazın. Ad bir harf ile başlamalıdır ve yalnızca harf, rakam ve kısa çizgi içerebilir.

5. **Abonelik** bölümündeki açılır menüyü kullanarak Azure aboneliğini seçin. Yalnızca bir abonelik kullanıyorsanız bu abonelik görüntülenir ve sonraki adıma atlayabilirsiniz. Hangi aboneliğin kullanılacağından emin değilseniz varsayılan (veya önerilen) aboneliği kullanın. Yalnızca kuruluş hesabınızın birden çok Azure aboneliği ile ilişkili olması durumunda birden çok seçenek olur.

6. **Kaynak grubu** bölümünde:

    * bir Kaynak grubu oluşturmak istiyorsanız, **Yeni oluştur**’u seçin.
    Veya
    * **Var olanı kullan**’ı seçin ve açılır menüyü kullanarak mevcut Kaynak gruplarının listesine bakın.

  Kaynak grupları hakkında eksiksiz bilgiler için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md).

7. Kasa için coğrafi bölgeyi seçmek üzere **Konum**'a tıklayın. Bu seçim, yedekleme verilerinizin gönderildiği coğrafi bölgeyi belirler.

8. Kurtarma Hizmetleri kasası dikey penceresinin alt kısmındaki **Oluştur**’a tıklayın.

    Kurtarma Hizmetleri kasasının oluşturulması birkaç dakika sürebilir. Portalın sağ üst kısmından durum bildirimlerini izleyin. Kasanız oluşturulduktan sonra Kurtarma Hizmetleri kasaları listesinde görünür. Birkaç dakika sonra kasayı görmezseniz **Yenile**’ye tıklayın.

    ![Yenile düğmesine tıklayın](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    Kasanızı Kurtarma Hizmetleri kasaları listesinde gördükten sonra, depolama yedekliliğini ayarlamaya hazır olursunuz.

### <a name="set-storage-redundancy-for-the-vault"></a>Kasa için depolama artıklığı ayarlama
Kurtarma Hizmetleri kasası oluşturduğunuzda, depolama yedekliliğinin istediğiniz şekilde yapılandırıldığından emin olun.

1. **Kurtarma Hizmetleri kasaları** dikey penceresinden yeni kasaya tıklayın.

    ![Kurtarma Hizmetleri kasası listesinden yeni kasayı seçin](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    Kasayı seçtiğinizde **Kurtarma Hizmetleri kasası** dikey penceresi daralır ve Ayarlar dikey penceresi (*en üstünde kasanın adı bulunur*) ve ile kasa ayrıntıları dikey penceresi açılır.

    ![Yeni kasa için depolama yapılandırmasını görüntüleme](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. Yeni kasanın Ayarlar dikey penceresinde dikey kaydırma çubuğunu kullanarak Yönet bölümüne inin ve **Yedekleme Altyapısı**’na tıklayın.
    Yedekleme Altyapısı dikey penceresi açılır.
3. Yedekleme Altyapısı dikey penceresinde, **Yedekleme Yapılandırması**’na tıklayarak **Yedekleme Yapılandırması** dikey penceresini açın.

    ![Yeni kasa için depolama yapılandırması ayarlama](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. Kasanız için uygun depolama çoğaltma seçeneğini belirleyin.

    ![depolama yapılandırması seçenekleri](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    Varsayılan olarak, kasanız coğrafi olarak yedekli depolamaya sahiptir. Azure'ı birincil yedek depolama uç noktası olarak kullanıyorsanız, **Coğrafi olarak yedekli** seçeneğini kullanmaya devam edin. Azure’u birincil yedek depolama uç noktası olarak kullanmıyorsanız, Azure depolama maliyetlerini azaltan **Yerel olarak yedekli** seçeneğini belirleyin. [Coğrafi olarak yedekli](../storage/common/storage-redundancy.md#geo-redundant-storage) ve [yerel olarak yedekli](../storage/common/storage-redundancy.md#locally-redundant-storage) depolama seçenekleri hakkında daha fazla bilgiyi [Depolama yedekliliğine genel bakış](../storage/common/storage-redundancy.md) bölümünden edinebilirsiniz.

Bir kasa oluşturduğunuza göre Windows sistem durumunu yedekleme için yapılandırın.

## <a name="configure-the-vault"></a>Kasa yapılandırma
1. Kurtarma Hizmetleri kasası dikey penceresinin (yeni oluşturduğunuz kasa için) Başlarken bölümünde **Yedekle**’ye tıklayın, ardından **Yedeklemeye Başlama** dikey penceresinde **Yedekleme hedefi**’ne tıklayın.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    **Yedekleme Hedefi** dikey penceresi açılır.

    ![Yedekleme hedefi dikey penceresini açma](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. **İş yükünüz nerede çalışıyor?** açılır menüsünden **Şirket içi**’ni seçin.

    Windows Server veya Windows bilgisayarınız Azure üzerinde olmayan fiziksel bir makine olduğu için **Şirket içi** seçeneğini belirlersiniz.

3. Gelen **neleri yedeklemek istiyorsunuz?** menüsünde seçin **sistem durumu**, tıklatıp **Tamam**.

    ![Dosya ve klasörleri yedekleme](./media/backup-azure-system-state/backup-goal-system-state.png)

    Tamam'a tıkladıktan sonra **Yedekleme hedefi**’nin yanında bir onay işareti görünür ve **Altyapıyı hazırlama** dikey penceresi açılır.

    ![Yedekleme hedefi yapılandırılmıştır, bundan sonra altyapıyı hazırlayın](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. **Altyapıyı hazırlama** dikey penceresinde **Windows Server veya Windows İstemcisi için Aracı'yı indir** seçeneğine tıklayın.

    ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    Windows Server Essential kullanıyorsanız Windows Server Essential aracısını indirmeyi seçin. Açılır menü, MARSAgentInstaller.exe dosyasını çalıştırma veya kaydetme seçeneğini sunar.

    ![MARSAgentInstaller iletişim kutusu](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. İndirme açılır penceresinde **Kaydet**'e tıklayın.

    Varsayılan olarak, **MARSagentinstaller.exe** dosyası İndirilenler klasörünüze kaydedilir. Yükleyici tamamlandığında yükleyiciyi çalıştırmak veya klasörü açmak isteyip istemediğinizi soran bir açılır pencere görüntülenir.

    ![altyapıyı hazırlama](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    Aracıyı yüklemeniz henüz gerekli değildir. Kasa kimlik bilgilerini indirdikten sonra aracıyı yükleyebilirsiniz.

6. **Altyapıyı hazırlama** dikey penceresinde **İndir**'e tıklayın.

    ![kasa kimlik bilgilerini indirme](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    Kasa kimlik bilgileri, İndirmeler klasörünüze indirilir. Kasa kimlik bilgilerini indirme tamamlandıktan sonra kimlik bilgilerini açmak veya kaydetmek isteyip istemediğinizi soran bir açılır pencere görüntülenir. **Kaydet**’e tıklayın. Yanlışlıkla **Aç**’a tıklarsanız, kasa kimlik bilgilerini açmaya çalışan iletişim kutusu başarısız olur. Kasa kimlik bilgilerini açamazsınız. Sonraki adıma geçin. Kasa kimlik bilgileri İndirmeler klasöründedir.   

    ![kasa kimlik bilgilerini indirme tamamlandı](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a>Aracıyı yükleme ve kaydetme

> [!NOTE]
> Azure portal üzerinden yedeklemeyi etkinleştirme olanağı henüz mevcut değildir. Microsoft Azure kurtarma Hizmetleri Aracısı, Windows Server sistem durumunu yedeklemek için kullanın.
>

1. İndirilenler klasöründen (veya diğer kayıtlı konumdan) **MARSagentinstaller.exe** dosyasını bulun ve dosyaya çift tıklayın.

    Yükleyici, Kurtarma Hizmetleri aracısı ayıklama, yükleme ve kaydetme sırasında bir dizi ileti sunar.

    ![Kurtarma Hizmetleri aracısı yükleyici kimlik bilgilerini çalıştırma](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. Microsoft Azure Kurtarma Hizmetleri Aracısı Kurulum Sihirbazı'nı tamamlayın. Sihirbazı tamamlamak için şunları yapmanız gerekir:

   * Yükleme ve önbellek klasörü için bir konum seçin.
   * İnternet'e bağlanmak için bir ara sunucu kullanıyorsanız ara sunucu bilgilerinizi sağlayın.
   * Kimliği doğrulanmış bir ara sunucu kullanıyorsanız kullanıcı adı ve parola bilgilerinizi sağlayın.
   * İndirilen kasa kimlik bilgilerini sağlayın
   * Şifreleme parolasını güvenli bir konuma kaydedin.

     > [!NOTE]
     > Parolayı kaybeder veya unutursanız Microsoft, yedekleme verilerini kurtarmanıza yardımcı olamaz. Dosyayı güvenli bir konuma kaydedin. Bu dosya, bir yedeklemeyi geri yüklemek için gereklidir.
     >
     >

Aracı artık yüklenmiş ve makineniz kasaya kaydedilmiştir. Yedeklemenizi yapılandırıp zamanlamak için hazırsınız.

## <a name="back-up-windows-server-system-state-preview"></a>Windows Server sistem durumunu (Önizleme) yedekleme
İlk yedekleme üç görevleri içerir:

* Azure Backup aracısını kullanarak sistem durumu yedeklemesi etkinleştir
* Yedeklemeyi zamanlama
* Dosya ve klasörleri ilk kez yedekleme

İlk yedeklemeyi tamamlamak için Microsoft Azure Kurtarma Hizmetleri aracısını kullanın.

### <a name="to-enable-system-state-backup-using-the-azure-backup-agent"></a>Azure Backup Aracısı kullanılarak sistem durumu yedeklemesi etkinleştirmek için

1. Bir PowerShell oturumunda Azure Backup altyapısını durdurmak için aşağıdaki komutu çalıştırın.

  ```
  PS C:\> Net stop obengine
  ```

2. Windows kayıt defterini açın.

  ```
  PS C:\> regedit.exe
  ```

3. Aşağıdaki kayıt defteri anahtarı ile belirtilen DWord değerini ekleyin.

  | Kayıt defteri yolu | Kayıt defteri anahtarı | DWord değeri |
  |---------------|--------------|-------------|
  | HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | TurnOffSSBFeature | 2 |

4. Yükseltilmiş bir komut isteminde aşağıdaki komutu çalıştırarak Backup altyapısını yeniden başlatın.

  ```
  PS C:\> Net start obengine
  ```

### <a name="to-schedule-the-backup-job"></a>Yedekleme işini zamanlamak için

1. Microsoft Azure Kurtarma Hizmetleri aracısını açın. Bunu, makinenizde **Microsoft Azure Backup** aramasını yaparak bulabilirsiniz.

    ![Azure Kurtarma Hizmetleri aracısını başlatma](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. Kurtarma Hizmetleri aracısında, **Yedeklemeyi Zamanla**'ya tıklayın.

    ![Windows Server yedeklemesini zamanlama](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. Yedeklemeyi Zamanlama Sihirbazı'nın Başlarken sayfasında **İleri**'ye tıklayın.

4. Yedeklenecek Öğeleri Seçin sayfasında **Öğe Ekle**'ye tıklayın.

5. Seçin **sistem durumu** ve ardından **Tamam**.

6. **İleri**’ye tıklayın.

7. Sistem Durumu yedekleme ve bekletme zamanlaması her Pazar 9:00 PM yerel zaman yedeklemek için otomatik olarak ayarlanır ve bekletme süresini 60 gün olarak ayarlanır.

   > [!NOTE]
   > Sistem Durumu yedekleme ve bekletme ilkesi otomatik olarak yapılandırılır. Dosya ve klasörleri ek olarak Windows Server sistem durumu yedekleme yapıyorsanız, yalnızca yedekleme ve Bekletme İlkesi Sihirbazı'ndan dosyası yedeklerini belirtin. 
   >

8. Onay sayfasında bilgileri gözden geçirin ve ardından **Son**'a tıklayın.

9. Sihirbaz yedekleme zamanlamasını oluşturduktan sonra **Kapat**'a tıklayın.

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a>İlk kez Windows Server sistem durumunu yedekleme

1. Windows Server için bir yeniden başlatma gerektiren bekleyen güncelleştirme bulunmadığından emin olun.

2. Kurtarma Hizmetleri aracısında, ağ üzerinden ilk doldurma işlemini tamamlamak için **Şimdi Yedekle**'ye tıklayın.

    ![Windows Server şimdi yedekle](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. Onay sayfasında, Şimdi Yedekle Sihirbazı'nın makineyi yedeklemek için kullanacağı ayarları gözden geçirin. Ardından **Yedekle**'ye tıklayın.

4. Sihirbazı kapatmak için **Kapat**'a tıklayın. Yedekleme işlemi tamamlanmadan önce sihirbazı kapatırsanız, sihirbaz arka planda çalışmaya devam eder.

5. Sunucunuzda, Windows Server sistem durumu yanı sıra dosya ve klasörlerinizi yedekleme yapıyorsanız Şimdi Yedekle Sihirbazı yalnızca dosyaların yedeğini alın. Geçici bir sistem durumu Yedekleme gerçekleştirmek için aşağıdaki PowerShell komutunu kullanın:

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  İlk yedekleme tamamlandıktan sonra, Yedekleme konsolunda **İş tamamlandı** durumu görünür.

  ![IR tamamlandı](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

Aşağıdaki sorular ve yanıtlar tamamlayıcı bilgiler sağlar.

### <a name="what-is-the-staging-volume"></a>Hazırlama toplu nedir?

Hazırlama toplu sistem durumu yedeklemesi yerel olarak kullanılabilir, Windows Server Yedekleme'burada aşamaları Ara konumunu temsil eder. Azure Backup Aracısı sonra sıkıştırır ve bu Ara yedekleme şifreler ve güvenli HTTPS protokolü aracılığıyla yapılandırılmış kurtarma Hizmetleri Kasası'na gönderir. **Bir Windows işletim birim hazırlama toplu oluşturmak önerilir. Sistem durumu yedeklemeleri sorunları gözlemlerseniz, hazırlama biriminiz konumunu denetimi ilk sorun giderme adımdır.** 

### <a name="how-can-i-change-the-staging-volume-path-specified-in-the-azure-backup-agent"></a>Hazırlama birimi Azure Backup aracısını belirtilen yolu nasıl değiştirebilir miyim?

Hazırlama toplu varsayılan önbellek klasöründe bulunur. 

1. Bu konumu değiştirmek için (bir yükseltilmiş komut istemi'nde) aşağıdaki komutu kullanın:
  ```
  PS C:\> Net stop obengine
  ```

2. Ardından aşağıdaki kayıt defteri girdilerini yeni hazırlama toplu klasörünün yolu ile güncelleştirin.

  |Kayıt defteri yolu|Kayıt defteri anahtarı|Değer|
  |-------------|------------|-----|
  |HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider | SSBStagingPath | Yeni hazırlama toplu konumu |

Hazırlama yolu büyük küçük harfe duyarlıdır ve hangi sunucuda mevcut olarak tam aynı büyük küçük harf olmalıdır. 

3. Hazırlama birimi yolu değiştirdiğinizde, Backup altyapısını yeniden başlatın:
  ```
  PS C:\> Net start obengine
  ```
4. Değiştirilen yolun almak için Microsoft Azure kurtarma Hizmetleri Aracısı'nı açın ve sistem durumunun geçici bir yedeklemeyi tetikleyin.

### <a name="why-is-the-system-state-default-retention-set-to-60-days"></a>Neden sistem durumu varsayılan bekletme 60 gün olarak ayarlanır?

Bir sistem durumu yedeklemesi kullanım ömrünü "kullanım ömrü" ayarı Windows Server Active Directory rolü için aynıdır. Silinmiş Öğe işareti yaşam süresi girişi için varsayılan değer 60 gündür. Bu değer dizin hizmeti (NTDS) config nesnede ayarlanabilir.

### <a name="how-do-i-change-the-default-backup-and-retention-policy-for-system-state"></a>Varsayılan yedekleme ve bekletme ilkesi için sistem durumu nasıl değiştiririm?

Varsayılan yedekleme ve sistem durumu için bekletme ilkesini değiştirmek için:
1. Backup altyapısını durdurun. Yükseltilmiş bir komut isteminden aşağıdaki komutu çalıştırın.

  ```
  PS C:\> Net stop obengine
  ```

2. Ekleyin veya HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider aşağıdaki kayıt defteri anahtar girişlerinde güncelleştirin.

  |Kayıt defteri adı|Açıklama|Değer|
  |-------------|-----------|-----|
  |SSBScheduleTime|Yedekleme yapılandırmak için kullanılır. 9 yerel saati varsayılandır.|DWord: Biçimi 9:30 yerel saati 2130 örneğin SSDD (ondalık)|
  |SSBScheduleDays|Belirtilen zamanda bir sistem durumu yedeklemesi zaman gerçekleştirilmelidir gün yapılandırmak için kullanılır. Tek tek basamak haftanın günleri belirtin. 0 Pazar gösteren, 1. Pazartesi, vb.. Varsayılan yedekleme için Pazar günüdür.|DWord: Yedekleme (örneğin 1230 Pazartesi, Salı, Çarşamba ve Pazar yedeklemeler zamanlar ondalık) çalıştırmak için haftanın günlerini.|
  |SSBRetentionDays|Yedekleme tutulacağı gün yapılandırmak için kullanılır. Varsayılan değer 60'tır. Değer izin verilen en fazla 180'dir.|DWord: Yedekleme (ondalık) tutulacağı gün sayısı.|

3. Yedekleme Altyapısı yeniden başlatmak için aşağıdaki komutu kullanın.
    ```
    PS C:\> Net start obengine
    ```

4. Microsoft Kurtarma Hizmetleri Aracısı'nı açın.

5. Tıklatın **yedekleme zamanlaması** ve ardından **sonraki** kadar yansıtılan değişiklikleri görebilirsiniz.

6. Tıklatın **son** değişiklikleri uygulamak için.


## <a name="questions"></a>Sorularınız mı var?
Sorularınız varsa veya dahil edilmesini istediğiniz herhangi bir özellik varsa [bize geri bildirim gönderin](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Sonraki adımlar
* [Windows makinelerini yedekleme](backup-configure-vault.md) konusunda daha fazla bilgi edinin.
* Dosya ve klasörlerinizi yedeklediğinize göre artık [kasa ve sunucularınızı yönetebilirsiniz](backup-azure-manage-windows-server.md).
* Bir yedeklemeyi geri yüklemeniz gerekirse [dosyaları bir Windows makinesine geri yüklemek](backup-azure-restore-windows-server.md) için bu makaleyi kullanın.
