---
title: Azure Backup Sunucusu'nu v2 yükleyin
description: Azure Backup Sunucusu'nu v2, VM'ler, dosyaları ve klasörleri, iş yükleri ve daha fazla korumak için yedekleme gelişmiş özellikler sunar. Yükleme veya Azure Backup Sunucusu'nu v2'ye yükseltme konusunda bilgi edinin.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: adigan
ms.openlocfilehash: fdf69003566f704354a17335b1f46fc3077aedbc
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38598399"
---
# <a name="install-azure-backup-server-v2"></a>Azure Backup Sunucusu'nu v2 yükleyin

Azure Backup sunucusu, sanal makinelerinizi (VM), iş yükleri, dosyaları ve klasörleri ve diğer korunmasına yardımcı olur. Azure Backup Sunucusu'nu v2 Azure Backup sunucusu v1 oluşturur ve v1'de kullanılabilir olan yeni özellikleri sunar. V1 ve v2 arasında özellik karşılaştırması için bkz: [Azure Backup sunucusu koruma matrisi](backup-mabs-protection-matrix.md). 

Ek özellikler Backup Sunucusu'nu v2'de, Backup sunucusu v1'den yükseltme olabilir. Ancak, Backup sunucusu v1 Backup Sunucusu'nu v2 yüklemek için bir önkoşul olmamasına. Backup sunucusu v1'den Backup Sunucusu'nu v2'ye yükseltmek istiyorsanız, Backup Sunucusu'nu v2 Backup sunucusu koruma sunucusuna yükleyin. Mevcut Backup sunucusu ayarlarınızı değişmeden kalır.

Backup Sunucusu'nu v2 Windows Server 2012 R2 veya Windows Server 2016'yı yükleyebilirsiniz. System Center 2016 veri koruma Yöneticisi'ni Modern yedekleme depolama alanı gibi yeni özelliklerden yararlanmak için Backup Sunucusu'nu v2 Windows Server 2016'da yüklemeniz gerekir. Yükseltme veya Backup Sunucusu'nu v2'ı yüklemeden önce okumanız [yükleme önkoşulları](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> Azure Backup sunucusu, aynı kod tabanını System Center Data Protection Manager olarak sahiptir. Yedek sunucu v1 Data Protection Manager 2012 R2'ye eşdeğerdir ve Backup sunucusu v2 Data Protection Manager 2016'ya eşdeğerdir. Bu makale, Data Protection Manager belgeleri bazen başvuruyor.
>
>

## <a name="upgrade-backup-server-to-v2"></a>Backup Sunucusu'nu v2'ye yükseltme
Backup sunucusu v1'den Backup Sunucusu'nu v2'ye yükseltmek için yüklemenizi gerekli güncelleştirmeleri içerdiğinden emin olun:

- [Koruma aracılarını güncelleştirme](backup-mabs-upgrade-to-v2.md#update-the-data-protection-manager-protection-agent) korumalı sunuculardaki.
- Windows Server 2012 R2, Windows Server 2016'ya yükseltin.
- Azure Backup sunucusu uzak yönetici, tüm üretim sunucularında yükseltin.
- Yedeklemeler, üretim sunucunuz yeniden başlatılmadan devam etmek için ayarlandığından emin olun.


### <a name="upgrade-steps-for-backup-server-v2"></a>Backup Sunucusu'nu v2 için yükseltme adımları

1. İndirme Merkezi'nde [yükseltme yükleyiciyi indirin](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Kurulum Sihirbazı'nı ayıkladıktan sonra emin **setup.exe yürütme** seçili ve ardından **son**.

  ![Kurulum Yükleyici - Kurulumu çalıştırın](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. Microsoft Azure Backup Sunucusu Sihirbazı'nda altında **yükleme**seçin **Microsoft Azure Backup sunucusu**.

   ![Kurulum Yükleyici - Select yükleme](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. Üzerinde **Hoş Geldiniz** sayfasında, Uyarıları gözden geçirin ve ardından **sonraki**.

   ![Kurulum Yükleyici - Karşılama sayfası](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. Kurulum Sihirbazı, ortamınızı yükseltebilirsiniz emin olmak için önkoşul denetimlerini yapar. Üzerinde **önkoşul denetimleri** sayfasında **denetleyin**.

   ![Kurulum Yükleyici - sayfasında önkoşul denetimleri](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Ortamınız önkoşul denetimleri geçmesi gerekir. Ortamınızı denetimler başarısız olursa, sorunlara dikkat edin ve bunları da onarabilir. Ardından, **tekrar kontrol edin**. Önkoşul denetimleri geçirdiğiniz sonra seçin **sonraki**.

  ![Kurulum Yükleyici - düğmesini tekrar kontrol edin](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. Üzerinde **SQL ayarları** sayfasında, SQL yüklemesi için ilgili seçeneği seçin ve ardından **denetle ve Yükle**.

   ![Kurulum Yükleyici - SQL Ayarları sayfası](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  Denetimler, birkaç dakika sürebilir. Denetimleri bittiğinde seçin **sonraki**.

   ![Kurulum Yükleyici - SQL ayarları denetle ve yükle düğmesine](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and-fix-settings.png)

8. Üzerinde **yükleme ayarları** sayfasında, yedekleme sunucusu yüklü olduğu konuma veya karalama konumu için herhangi bir değişiklik yapın. **İleri**’yi seçin.

  ![Kurulum Yükleyici - yükleme ayarları sayfası](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. Kurulum Sihirbazı'nı bitirmek için seçin **son**.

  ![Kurulum Yükleyici - bitiş](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a>Modern yedekleme depolama alanı için depolama ekleme

Yedekleme depolama verimliliğini artırmak için Backup Sunucusu'nu v2 birimler için destek ekler. Backup sunucusu v1 gibi Backup Sunucusu'nu v2 diskleri destekler.

### <a name="add-volumes-and-disks"></a>Birimleri ve diskler ekleme
Windows Server 2016'da Backup Sunucusu'nu v2 çalıştırırsanız, yedek verileri depolamak için birimler kullanabilirsiniz. Birimler depolama tasarrufu ve daha hızlı yedeklemeler sağlar. Birimler için yedekleme sunucusu yeni olduğundan, bunları eklemeniz gerekir. 

Bir birim için Backup sunucusu eklediğinizde, birimin bir kolay ad verebilirsiniz. Tıklayın **kolay ad** biriminin adını istediğiniz sütun. Gerekirse daha sonra adı değiştirebilirsiniz. Eklemek veya birimlere kolay ad değiştirmek için PowerShell de kullanabilirsiniz.

Yönetici konsolunda bir birim eklemek için:

1. Azure Backup Sunucu Yöneticisi Konsolu'ndaki seçin **Yönetim** > **Disk Depolama** > **Ekle**.

    ![Disk Depolama Ekle Sihirbazı'nı açın](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    Bu, Disk Depolama Ekle Sihirbazı'nı açar.

2. Üzerinde **Disk Depolama Ekle** sayfasında **kullanılabilir birimler** kutusuna bir birim seçin ve ardından **Ekle**.
3. İçinde **seçilen birimler** kutusuna birim için bir kolay ad girin ve ardından **Tamam**.

      ![Disk depolama alanı Ekle Sihirbazı - birim ekleme](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Disk, bir disk eklemek istiyorsanız, eski depolama alanına sahip bir koruma grubuna ait olmalıdır. Bu diskler yalnızca bu koruma grupları için kullanılabilir. Disk yedekleme sunucusu eski koruması olan kaynaklar yoksa, listede yok.

  Disk ekleme hakkında daha fazla bilgi için bkz. [eski depolama alanını artırmak için disk ekleme](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). Bir diski bir kolay ad veremezsiniz.


### <a name="assign-workloads-to-volumes"></a>Birimlere iş yükleri atama

Backup sunucusu iş yüklerini hangi birimlere atanan belirtin. Örneğin, sık sık, yüksek hacimli yedeklemeler gerektiren iş yükleri depolamak için çok sayıda giriş/çıkış işlemi (IOPS) saniyede destekleyen pahalı birimler ayarlayabilirsiniz. SQL Server ile işlem günlüklerini örneğidir.

#### <a name="update-dpmdiskstorage"></a>Update = DPMDiskStorage

Backup sunucusu, depolama havuzundaki bir birimin özelliklerini güncelleştirmek için Update = DPMDiskStorage PowerShell cmdlet'ini kullanın.

Sözdizimi:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

PowerShell kullanarak yaptığınız tüm değişiklikler UI'de yansıtılır.


## <a name="protect-data-sources"></a>Veri kaynaklarını korumak
Veri kaynaklarını korumaya başlamak için bir koruma grubu oluşturun. Aşağıdaki adımlar değişiklikleri veya eklemeler yeni koruma grubu Sihirbazı'nı vurgulayın.

Bir koruma grubu oluşturmak için:

1. Yedek sunucu yöneticisi Konsolu'ndaki seçin **koruma**.

2. Araç şeridinde seçin **yeni**.

    Bu, yeni koruma grubu oluşturma Sihirbazı'nı açar.

  ![Yeni koruma grubu oluşturma Sihirbazı](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. **Hoş Geldiniz** sayfasında, **İleri**’yi seçin.
4. Üzerinde **koruma grubu türünü seçin** sayfasında, oluşturun ve ardından istediğiniz koruma grubu türünü seçin **sonraki**.

  ![Koruma grubu seç türü sayfası](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. Üzerinde **grup üyelerini seçin** sayfasında **kullanılabilir üyeler** bölmesinde, koruma aracıları listelenen üyeleriyle. Bu örnekte D:\ birimi seçin ve E:\ ve bunları Ekle **seçili üyeleri** bölmesi. **İleri**’yi seçin.

  ![Grup üyeleri sayfasında](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. Üzerinde **veri koruma yöntemini seçin** want bir **koruma grubu adı**koruma yöntemini seçin ve ardından **sonraki**. Kısa vadeli koruma istiyorsanız seçmelisiniz **Disk** yedekleme yöntemi.

  ![Veri koruma yöntemini seçin sayfası](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. Üzerinde **kısa vadeli hedefleri belirtin** sayfasında, ayrıntıları **bekletme aralığı** ve **eşitleme sıklığı**. Sonra **İleri**’yi seçin. Kurtarma noktalarının ne zaman alınacağına zamanlamasını değiştirmek için isteğe bağlı olarak seçin **Değiştir**.

  ![Sayfa kısa vadeli hedefleri belirtin](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. Üzerinde **Disk Depolama ayırmayı gözden** , gözden geçirme Ayrıntıları sayfası veri kaynakları hakkında seçtiğiniz, boyutu ve sağlanacak alan ve hedef depolama birimi için değerler.

  ![Disk Depolama ayırmayı İncele sayfası](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Depolama birimleri iş yükü birimi ayırma işlemi (PowerShell kullanarak ayarlama) ve kullanılabilir depolama alanına bağlıdır. Depolama birimlerini, açılır menüden diğer birimleri seçerek değiştirebilirsiniz. Değerini değiştirirseniz **hedef depolama**, değeri **kullanılabilir disk depolama** altında değerleri yansıtacak şekilde dinamik olarak değişir **boş alan** ve  **Eksik sağlanmış alan**.

  Veri kaynakları için değer planlandığı gibi büyümesi durumunda **eksik sağlanmış alan** sütununda **kullanılabilir disk depolama** gerekli olan ek depolama alanı miktarını gösterir. Kesintisiz yedeklemeleri için depolama ihtiyaçlarınızı planlamanıza yardımcı olacak bu değeri kullanın. Değer sıfır ise, öngörülebilir gelecekte depolama ile herhangi bir olası sorun vardır. Değer sıfır dışındaki bir sayı ise, ayrılmış yeterli depolama alanınız yok (koruma ilkenize ve korumalı üyelerinizin veri boyutuna göre).

  ![Eksik ayrılmış disk depolama](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   Koruma grubunuzu oluşturmayı tamamlamak için sihirbazı tamamlayın.

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Eski depolamayı Modern yedekleme depolama alanına geçirme
Backup Sunucusu'nu v2 veya işletim sistemini Windows Server 2016'ya yükseltme sürümüne yükselttikten sonra koruma gruplarınızı Modern yedekleme depolama kullanacak şekilde güncelleştirin. Varsayılan olarak, koruma grupları değiştirilmez. Bunların başlangıçta ayarlanan gibi çalışmaya devam eder. 

Modern yedekleme depolama alanı kullanmak için koruma gruplarını güncelleştirmek isteğe bağlıdır. Koruma grubunu güncelleştirmek için verileri tut seçeneğini kullanarak tüm veri kaynaklarının korumasını durdurun. Ardından, veri kaynaklarını yeni bir koruma grubuna ekleyin.

1. Yönetici Konsolu'nda seçin **koruma** özelliği. İçinde **koruma grubu üyesi** listesinde üyeye sağ tıklayın ve ardından **üyenin korumasını Durdur**.

  ![Üyenin korumasını Durdur](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. İçinde **grubundan** iletişim kutusunda, kullanılan disk alanı ve depolama havuzu için kullanılabilir boş alanı gözden geçirin. Diskte kurtarma noktalarını bırakmak ve kendi ilişkilendirildikleri bekletme ilkesine göre sürelerinin dolmasına izin vermek için varsayılandır. **Tamam**’a tıklayın.

  Kullanılan disk alanını hemen boş depolama havuzuna döndürülecek istiyorsanız belirleyin **diskteki çoğaltmayı Sil** yedekleme verileri (ve kurtarma noktalarını) silmek için onay kutusunu, bu üye ile ilişkili.

  ![Grup iletişim kutusundan kaldırma](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Modern yedekleme depolama alanı kullanan bir koruma grubu oluşturun. Korumasız veri kaynaklarını içerir.


## <a name="add-disks-to-increase-legacy-storage"></a>Eski depolama alanını artırmak için disk ekleme

Backup sunucusu ile eski depolama kullanmak istiyorsanız, eski depolama alanını artırmak için disk eklemek gerekebilir. 

Disk depolama eklemek için:

1. Yönetici Konsolu'nda seçin **Yönetim** > **Disk Depolama** > **Ekle**.

    ![Disk depolama alanı iletişim kutusu Ekle](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. İçinde **Disk Depolama Ekle** iletişim kutusunda **disk Ekle**.

5. Kullanılabilir diskler listesi, seçin, eklemek istediğiniz diskleri seçin **Ekle**ve ardından **Tamam**.

## <a name="update-the-data-protection-manager-protection-agent"></a>Data Protection Manager koruma Aracısı güncelleştirmesi

Backup sunucusu, güncelleştirmeleri System Center Data Protection Manager koruma Aracısı kullanır. Ağa bağlı olmayan bir koruma aracısını yükseltiyorsanız, bağlı aracı yükseltme işlemini tamamlamak için veri koruma Yöneticisi Yönetici Konsolu'nu kullanamazsınız. Etkin olmayan bir etki alanı ortamında koruma aracısını yükseltmeniz gerekir. Data Protection Manager Yönetici Konsolu, istemci bilgisayar ağa bağlanana kadar koruma Aracısı güncelleştirmesinin Beklemede olduğunu gösterir.

Aşağıdaki bölümlerde, bağlı olan istemci bilgisayarlar ve bağlı olmayan istemci bilgisayarlar için koruma aracılarını güncelleştirme konusunda açıklanmaktadır.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Bağlantılı bir istemci bilgisayarda koruma aracısını güncelleştirme

1. Yedek sunucu yöneticisi Konsolu'ndaki seçin **Yönetim** > **aracıları**.

2. Görüntü bölmesinde, koruma aracısını güncelleştirmek istediğiniz bilgisayarları seçin.

  > [!NOTE]
  > **Aracı güncelleştirmeleri** sütunu her korumalı bilgisayar için bir koruma Aracısı güncelleştirmesi çıktığında gösterir. İçinde **eylemleri** bölmesinde **güncelleştirme** eylemi, yalnızca korumalı bir bilgisayarın seçili olduğundan ve güncelleştirmeleri kullanılabilir.
  >
  >

3. Seçili bilgisayarlara güncelleştirilmiş koruma aracıları yüklemek için **eylemleri** bölmesinde **güncelleştirme**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Bağlı olmayan istemci bilgisayarda koruma aracısını güncelleştirme

1. Yedek sunucu yöneticisi Konsolu'ndaki seçin **Yönetim** > **aracıları**.

2. Görüntü bölmesinde, koruma aracısını güncelleştirmek istediğiniz bilgisayarları seçin.

  > [!NOTE]
   > **Aracı güncelleştirmeleri** sütunu her korumalı bilgisayar için bir koruma Aracısı güncelleştirmesi çıktığında gösterir. İçinde **eylemleri** bölmesinde **güncelleştirme** eylemi kullanılabilir değil korumalı bir bilgisayar seçildiğinde kullanılabilir güncelleştirme yoksa.
  >
  >

3. Seçili bilgisayarlara güncelleştirilmiş koruma aracıları yüklemek için seçin **güncelleştirme**.

4. Bilgisayar ağa bağlanana kadar ağa bağlı olmayan bir istemci bilgisayar için **aracı durumu** sütun bir durumu gösterir **güncelleştirme Beklemede**.

  Bir istemci bilgisayar ağa bağlandıktan sonra **aracı güncelleştirmeleri** sütununda istemci bilgisayar durumunu gösteren **güncelleştirme**.
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-the-new-version-with-azure"></a>Eski bir sürümünden eski koruma grupları Taşı ve yeni sürümü Azure ile eşitleme

Azure Backup sunucusu ve işletim sistemi hem de güncelleştirildikten sonra Modern yedekleme depolama alanı'nı kullanarak yeni veri kaynaklarını korumak hazır olursunuz. Ancak zaten korunan veri kaynakları, Azure Backup sunucusu ancak tüm yeni koruma oldukları gibi eski biçimde korunacak devam eder, Modern yedekleme depolama alanı kullanacaktır.

Adımlarla Modern yedekleme depolama alanı koruma eski modundan veri kaynakları geçirilemedi.

• Yeni birimleri DPM depolama havuzuna ekleyin ve isterseniz kolay adları ve veri kaynağı etiketleri atayın.
• Eski modda korumasını durdurun ve veri kaynakları "Korumalı verileri Koru" olan her veri kaynağı için.  Bu kurtarma eski kurtarma noktalarını geçişten sonra olanak tanır.

•, Yeni bir sayfa oluşturun ve yeni format kullanılarak depolanması için olan veri kaynaklarını seçin.
• DPM Modern yedekleme depolama birimine yerel olarak eski yedekleme depolama alanından çoğaltma kopyasını yaparsınız.
Not: Bu görülür olarak bir kurtarma sonrası işlem işi • tüm yeni eşitleme ve kurtarma noktalarını ardından Modern yedekleme depolama alanı depolanır.
Süresi dolacak ve sonunda disk alanı boşaltın • eski kurtarma noktalarını kullanıma ayıklanır.
Eski olan tüm birimler, eski depolama alanından disk silindikten sonra • Azure backup ve sisteminden kaldırabilirsiniz.
• Azure DPMDB yedeklemesini alır.

2. Bölüm:-Önemli öğeleri > Yeni sunucunun aynı özgün Azure Backup sunucusu olarak adlandırılması gerekir. Eski depolama havuzu ve DPMDB korumak için kullanmak istiyorsanız, yeni Azure backup sunucusu adını değiştiremezsiniz geri eklemesi gerekeceğinden, Kurtarma noktaları - DPMDB yedeklemesini olması gerekir

1) Kapatma özgün Azure backup sunucusu veya kablo devre dışı duruma.
2) Makine hesabı active Directory'de sıfırlayın.
3) Server 2016 yeni makineye yüklemek ve aynı makine adı özgün Azure Backup sunucusu olarak adlandırın.
4) Etki alanına katılma
5) Azure Backup Sunucusu'nu V2 yüklemeye (taşıma DPM depolama havuzu diskleri eski sunucu ve içeri aktarma)
6) Geçen bölüm 2 sonundan DPMDB geri yükleme
7) Depolama, yedekleme özgün sunucudan yeni sunucuya ekleyin.
8) DPMDB'yi SQL geri yükleme
9) Microsoft Azure Backup'a yeni server CD'sinde yönetici komut satırından konumu ve bin klasörüne yükleyin

Örnek yol: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\
Azure'a yedekleme çalıştırmak DPMSYNC-SYNC

10) DPMSYNC çalıştırın-eşitleme Not eskileri taşıma yerine DPM depolama havuzuna yeni diskler ekledikten sonra çalıştırırsanız DPMSYNC - Reallocatereplica

## <a name="new-powershell-cmdlets-in-v2"></a>V2'de yeni PowerShell cmdlet'leri

Azure Backup Sunucusu'nu v2 yüklediğinizde, iki yeni cmdlet kullanılabilir: 
* [Mount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Sonraki adımlar

Sunucunuzu hazırlama veya bir iş yükü korumaya başlamak öğrenin:
- [Backup sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
- [Bir VMware sunucusunu yedeklemek için Backup sunucusu kullanma](backup-azure-backup-server-vmware.md)
- [SQL sunucusunu yedeklemek için Backup sunucusu kullanma](backup-azure-sql-mabs.md)
- [Modern yedekleme depolama alanı ile Backup sunucusu kullanma](backup-mabs-add-storage.md)

