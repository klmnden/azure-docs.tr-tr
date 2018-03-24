---
title: Azure Backup sunucusu v2 yükleme | Microsoft Docs
description: Azure yedekleme sunucusu v2, VM'ler, dosyalar ve klasörler, iş yükleri ve daha fazla korunması için geliştirilmiş yedekleme özellikleri sunar. Yüklemek veya yükseltmek için Azure yedekleme sunucusu v2 öğrenin.
services: backup
documentationcenter: ''
author: markgalioto
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: dd7b76d9e06bc82ffd75f12131c2c247da05cc91
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="install-azure-backup-server-v2"></a>Azure Backup sunucusu v2 yükleyin

Azure yedekleme sunucusu sanal makineleri (VM'ler), iş yükleri, dosyaları ve klasörleri ve daha fazla korunmasına yardımcı olur. Azure yedekleme sunucusu v2 üzerinde Azure yedekleme sunucusu v1 oluşturur ve v1 kullanılabilir değil yeni özellikleri sağlar. Özellikler v1 ve v2 arasındaki karşılaştırması için bkz: [Azure yedekleme sunucusu koruma matris](backup-mabs-protection-matrix.md). 

Ek yedekleme sunucusu v2 olarak yedekleme sunucusu v1'den yükseltme özellikleridir. Ancak, yedekleme sunucusu v1 yedekleme sunucusu v2 yüklemek için bir önkoşul değil. Yedekleme sunucusu v1 ile yedekleme sunucusu v2'ye yükseltmek istiyorsanız, yedekleme sunucusu v2 yedekleme sunucusu koruma sunucuya yükleyin. Var olan yedekleme sunucusu ayarlarınızı değişmeden kalır.

Windows Server 2012 R2 veya Windows Server 2016 yedekleme sunucusu v2 yükleyebilirsiniz. System Center 2016 veri koruma Yöneticisi Modern yedekleme depolama alanı gibi yeni özelliklerden yararlanmak için Windows Server 2016 yedekleme sunucusu v2 yüklemeniz gerekir. Yükseltme veya yedekleme sunucusu v2 yüklemeden önce okuyun [yükleme önkoşulları](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> Azure yedekleme sunucusu olarak System Center Data Protection Manager temel aynı koduna sahip. Yedek sunucu v1 Data Protection Manager 2012 R2'ye eşdeğerdir ve yedekleme sunucusu v2 Data Protection Manager 2016 eşdeğerdir. Bu makalede, Data Protection Manager belge bazen başvurur.
>
>

## <a name="upgrade-backup-server-to-v2"></a>Yedek sunucu v2'ye yükseltme
Yedekleme sunucusu v1 ile yedekleme sunucusu v2'ye yükseltmek için gerekli güncelleştirmeleri yüklemenizi olduğundan emin olun:

- [Koruma aracılarını güncelleştirme](backup-mabs-upgrade-to-v2.md#update-the-data-protection-manager-protection-agent) korumalı sunuculardaki.
- Windows Server 2012 R2, Windows Server 2016 yükseltme.
- Azure yedekleme sunucusu uzak yönetici tüm üretim sunucularında yükseltin.
- Yedeklemeleri üretim sunucunuzu yeniden başlatmanıza gerek kalmadan devam etmek için ayarlandığından emin olun.


### <a name="upgrade-steps-for-backup-server-v2"></a>Yedekleme sunucusu v2 yükseltme adımları

1. İndirme Merkezi'nde [yükseltme yükleyici indirmek](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Kurulum Sihirbazı'nı ayıkladıktan sonra olduğundan emin olun **setup.exe yürütme** seçili ve ardından **son**.

  ![Kurulum Yükleyici - Çalıştır kurulum](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. Microsoft Azure Yedekleme Sunucusu Sihirbazı'nda altında **yükleme**seçin **Microsoft Azure yedekleme sunucusu**.

  ![Kurulum Yükleyici - Select yükleme](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. Üzerinde **Hoş Geldiniz** sayfasında, Uyarıları gözden geçirin ve ardından **sonraki**.

  ![Kurulum Yükleyici - Karşılama sayfası](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. Kurulum Sihirbazı'nı ortamınızı yükseltebilirsiniz emin olmak için önkoşul denetimleri de gerçekleştirir. Üzerinde **önkoşul denetler** sayfasında, **denetleyin**.

  ![Kurulum Yükleyici - önkoşul denetler sayfası](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Ortamınız önkoşul denetimleri geçmesi gerekir. Ortamınızı denetimleri geçmiyor sorunlara dikkat edin ve düzeltin. Ardından, seçin **yeniden denetle**. Önkoşul denetimleri geçirdiğiniz sonra seçin **sonraki**.

  ![Kurulum Yükleyici - yeniden denetle düğmesi](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. Üzerinde **SQL ayarlarını** sayfasında, SQL yüklemeniz için ilgili seçeneği seçin ve ardından **denetle ve Yükle**.

  ![Kurulum Yükleyici - SQL Ayarları sayfası](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  Denetimleri birkaç dakika sürebilir. Denetimleri bittiğinde seçin **sonraki**.

  ![Kurulum Yükleyici - SQL ayarlarını denetleyin ve Yükle düğmesini](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. Üzerinde **yükleme ayarları** sayfasında, yedekleme sunucusu yüklü olduğu bir konuma ya da sıfırdan konuma değişiklikleri yapın. **İleri**’yi seçin.

  ![Kurulum Yükleyici - yükleme ayarları sayfası](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. Kurulum Sihirbazı'nı bitirmek için seçin **son**.

  ![Kurulum Yükleyici - bitiş](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a>Modern yedekleme depolama için depolama ekleme

Yedekleme depolama verimliliği artırmak için yedekleme sunucusuna v2 birimler için destek ekler. Yedekleme sunucusu v1 gibi yedekleme sunucusu v2 diskleri destekler.

### <a name="add-volumes-and-disks"></a>Birimler ve diskleri ekleme
Windows Server 2016 yedekleme sunucusu v2 çalıştırırsanız, yedek verileri depolamak için birimler kullanabilirsiniz. Birimleri depolama alanından tasarruf etmenizi ve daha hızlı yedekleme sunar. Birimleri yedekleme sunucusuna yeni olduğundan, bunları eklemeniz gerekir. 

Bir birim için yedekleme sunucusu eklediğinizde, birim kolay bir ad verin. Tıklatın **kolay ad** sütun adı istediğiniz birimin. Gerekirse, ad daha sonra değiştirebilirsiniz. Eklemek veya birimler için kolay adlar değiştirmek için PowerShell de kullanabilirsiniz.

Yönetici konsolunda bir birim eklemek için:

1. Azure yedekleme sunucusu Yönetici konsolunda seçin **Yönetim** > **Disk Depolama** > **Ekle**.

    ![Disk Depolama Alanı Ekle Sihirbazı'nı açın](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    Bu, Disk Depolama Alanı Ekle Sihirbazı'nı açar.

2. Üzerinde **Disk Depolama Alanı Ekle** sayfasında **kullanılabilir birimleri** kutusunda, bir birim seçin ve ardından **Ekle**.
3. İçinde **seçili birimleri** kutusuna, birim için bir kolay ad girin ve ardından **Tamam**.

      ![Disk depolama alanı Ekle Sihirbazı - birim ekleme](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Disk, bir disk eklemek istiyorsanız, eski depolama alanına sahip bir koruma grubuna ait olmalıdır. Bu diskleri yalnızca bu koruma grupları için kullanılabilir. Yedekleme sunucusu eski koruma kaynakları yoksa, diskin listede yok.

  Diskleri ekleme hakkında daha fazla bilgi için bkz: [eski depolama alanını büyütmek için diskleri ekleme](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). Kolay bir ad bir disk veremez.


### <a name="assign-workloads-to-volumes"></a>İş yükleri için birimler atamak

Yedekleme Server'da hangi iş yükleri için hangi birimlerin atanan belirtin. Örneğin, sık kullanılan, yüksek hacimli yedeklemeler gerektiren iş yüklerini depolamak için çok sayıda giriş/çıkış işlemi (IOPS) saniyede destekleyen pahalı birimler ayarlayabilirsiniz. SQL Server ile işlem günlükleri örneğidir.

#### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Yedekleme sunucusu depolama havuzu biriminde özelliklerini güncelleştirmek için güncelleştirme DPMDiskStorage PowerShell cmdlet'ini kullanın.

Sözdizimi:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

PowerShell kullanarak yaptığınız tüm değişiklikler kullanıcı Arabiriminde yansıtılır.


## <a name="protect-data-sources"></a>Veri kaynaklarını korumak
Veri kaynaklarını korumaya başlamak için bir koruma grubu oluşturun. Aşağıdaki adımları değişiklik veya yeni koruma grubu Sihirbazı'nı eklemeleri vurgulayın.

Bir koruma grubu oluşturmak için:

1. Yedek sunucu Yönetici konsolunda seçin **koruma**.

2. Araç şeridinde seçin **yeni**.

    Bu, yeni koruma grubu oluşturma Sihirbazı'nı açar.

  ![Yeni koruma grubu oluşturma Sihirbazı](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. **Hoş Geldiniz** sayfasında, **İleri**’yi seçin.
4. Üzerinde **koruma grubu türünü seçin** sayfasında, oluşturmak ve ardından istediğiniz koruma grubu türünü seçin **sonraki**.

  ![Koruma grubu seç türü sayfası](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. Üzerinde **grup üyelerini seçin** sayfasında **kullanılabilir üyeler** bölmesinde, koruma aracılarını listelenen üyeleriyle. Bu örnekte, D:\ birimi seçin ve E:\ ve bunları Ekle **seçili üyeleri** bölmesi. **İleri**’yi seçin.

  ![Grup Seç üyeleri sayfası](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. Üzerinde **veri koruma yöntemini seçin** want bir **koruma grubu adı**koruma yöntemini seçin ve ardından **sonraki**. Kısa vadeli koruma istiyorsanız seçmelisiniz **Disk** yedekleme yöntemi.

  ![Veri koruma yöntemini sayfa seçin](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. Üzerinde **kısa vadeli hedefleri belirtin** sayfasında, ayrıntıları **bekletme aralığı** ve **eşitleme sıklığı**. Ardından, seçin **sonraki**. Kurtarma noktalarının ne zaman gerçekleştirilecek zamanlamasını değiştirmek için isteğe bağlı olarak seçin **Değiştir**.

  ![Kısa vadeli hedefleri sayfasında belirtin](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. Üzerinde **Disk Depolaması ayırmasını gözden** , gözden geçirme Ayrıntıları sayfası veri kaynakları hakkında seçtiğiniz, boyutu ve sağlanacak alan ve hedef depolama birimi değerleri.

  ![Gözden geçirme Disk Depolaması ayırması sayfası](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Depolama birimleri (PowerShell kullanarak ayarlayın) iş yükü birim ayırma ve kullanılabilir depolama alanına bağlıdır. Depolama birimleri, aşağı açılan menüsünde diğer birimleri'i seçerek değiştirebilirsiniz. Değeri değiştirirseniz **hedef depolama**, değeri **kullanılabilir disk depolama** altında değerleri yansıtacak şekilde değişir dinamik olarak **boş alan** ve  **Underprovisioned alanı**.

  Veri kaynakları için değer planlı olarak büyümesine **Underprovisioned alanı** sütununda **kullanılabilir disk depolama** gerekli olan ek depolama alanı miktarını gösterir. Kesintisiz yedeklemeler için depolama gereksinimlerini planlamaya yardımcı olması için bu değeri kullanın. Değerin sıfır ise, öngörülebilir gelecekte depolama alanı hiçbir olası sorunları vardır. Değerin sıfır dışındaki bir sayı ise, ayrılmış yeterli depolama alanı yok (koruma ilkeniz ve korumalı üyeleri veri boyutuna göre).

  ![Eksik yüklenmiş disk depolama](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   Koruma grubu oluşturmayı tamamlamak için sihirbazı tamamlayın.

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Eski depolama Modern bir yedekleme depolama alanına geçiş
Veya yedekleme sunucusu v2 yüklemek ve Windows Server 2016 için işletim sistemini yükseltmek için yükseltme yaptıktan sonra koruma gruplarınızı Modern yedekleme depolama kullanacak şekilde güncelleştirin. Varsayılan olarak, koruma grupları değiştirilmez. Bunlar başlangıçta kurulmuş gibi çalışmaya devam eder. 

Modern yedekleme depolama kullanmak için koruma grupları güncelleştirme isteğe bağlıdır. Koruma grubunu güncelleştirmek için verileri tutma seçeneğini kullanarak tüm veri kaynaklarının korumasını durdurun. Ardından, veri kaynaklarını yeni bir koruma grubuna ekleyin.

1. Yönetici Konsolu'nda seçin **koruma** özelliği. İçinde **koruma grubu üyesi** listesi, üye sağ tıklayın ve ardından **üyenin korumasını Durdur**.

  ![Üyenin korumasını Durdur](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. İçinde **grubundan** iletişim kutusunda, kullanılan disk alanı ve depolama havuzu için kullanılabilir boş alanı gözden geçirin. Disk üzerinde kurtarma noktaları bırakın ve bunları kendi ilişkili bekletme ilkesi süresi dolacak şekilde izin vermek için varsayılandır. **Tamam**’a tıklayın.

  Kullanılan disk alanı boş depolama havuzuna derhal döndürülmesini istiyorsanız seçin **diskteki çoğaltmayı Sil** yedekleme verilerini (ve kurtarma noktaları) silmek için onay kutusunu bu üye ile ilişkili.

  ![Grup iletişim kutusundan Kaldır](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Modern yedekleme depolama kullanan bir koruma grubu oluşturun. Korunmayan veri kaynaklarını içerir.


## <a name="add-disks-to-increase-legacy-storage"></a>Eski depolama alanını büyütmek için disk ekleme

Eski depolama yedekleme sunucusu ile kullanmak istiyorsanız, eski depolama alanını büyütmek için disk eklemeniz gerekebilir. 

Disk depolama alanı eklemek için:

1. Yönetici Konsolu'nda seçin **Yönetim** > **Disk Depolama** > **Ekle**.

    ![Disk depolama iletişim ekleyin](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. İçinde **Disk Depolama Alanı Ekle** iletişim kutusunda **eklemek diskleri**.

5. Kullanılabilir diskler listesinde, eklemek, seçmek istediğiniz diskleri seçin **Ekle**ve ardından **Tamam**.

## <a name="update-the-data-protection-manager-protection-agent"></a>Data Protection Manager koruma aracısını güncelleştirin

Yedekleme sunucusu System Center Data Protection Manager koruma Aracısı güncelleştirmeleri için kullanır. Ağa bağlı olmayan bir koruma aracısını yükseltiyorsanız, bağlı aracı yükseltme işlemini tamamlamak için veri koruma Yöneticisi konsolunu kullanamazsınız. Etkin olmayan etki alanı ortamında koruma aracısını yükseltmeniz gerekir. İstemci bilgisayar ağa bağlanana kadar veri koruma Yöneticisi konsolunu koruma Aracısı güncelleştirmesinin Beklemede olduğunu gösterir.

Aşağıdaki bölümler, bağlı olan istemci bilgisayarları ve bağlı olmayan istemci bilgisayarlar için koruma aracılarını güncelleştirme açıklar.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Bağlantılı bir istemci bilgisayarda koruma aracısını güncelleştir

1. Yedek sunucu Yönetici konsolunda seçin **Yönetim** > **aracıları**.

2. Görüntü bölmesinde, koruma aracısını güncelleştirmek istediğiniz bilgisayarları seçin.

  > [!NOTE]
  > **Aracı güncelleştirmeleri** sütun gösteren bir koruma Aracısı güncelleştirmesi her korumalı bilgisayar için kullanılabilir olduğunda. İçinde **Eylemler** bölmesinde **güncelleştirme** eylem, yalnızca korumalı bir bilgisayarın seçili olduğundan ve kullanılabilir güncelleştirmeler olduğunda kullanılabilir.
  >
  >

3. Seçili bilgisayarlara güncelleştirilmiş koruma aracıları yüklemek için **Eylemler** bölmesinde, **güncelleştirme**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Bağlı olmayan bir istemci bilgisayarda koruma aracısını güncelleştirin

1. Yedek sunucu Yönetici konsolunda seçin **Yönetim** > **aracıları**.

2. Görüntü bölmesinde, koruma aracısını güncelleştirmek istediğiniz bilgisayarları seçin.

  > [!NOTE]
   > **Aracı güncelleştirmeleri** sütun gösteren bir koruma Aracısı güncelleştirmesi her korumalı bilgisayar için kullanılabilir olduğunda. İçinde **Eylemler** bölmesinde **güncelleştirme** eylemi kullanılabilir değil korumalı bir bilgisayar seçildiğinde kullanılabilir güncelleştirme yoksa.
  >
  >

3. Seçili bilgisayarlara güncelleştirilmiş koruma aracıları yüklemek için seçin **güncelleştirme**.

4. Bilgisayar ağa bağlanana kadar ağa bağlı olmayan bir istemci bilgisayar için **aracı durumu** sütunda görüntülenir durumunu **güncelleştirme Beklemede**.

  Bir istemci bilgisayar ağa bağlandıktan sonra **aracı güncelleştirmeleri** sütun istemci bilgisayar için bir durumu gösterir **güncelleştirme**.
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-the-new-version-with-azure"></a>Eski koruma grupları eski sürümden taşıyın ve yeni sürümü Azure ile eşitleme

Azure yedekleme sunucusu ve işletim sistemi hem de güncelleştirilmiştir sonra Modern yedekleme depolama kullanarak yeni veri kaynaklarını korumak hazırsınız. Ancak zaten korumalı veri kaynakları Azure yedekleme sunucusu ancak tüm yeni koruma oldukları gibi eski biçimde korunmaya devam eder, Modern yedekleme depolama kullanır.

Aşağıdaki adımları olan veri kaynaklarını koruma eski modundan Modern yedekleme depolama alanına geçirmek için.

• Yeni birimlerin DPM depolama havuzuna ekleyin ve isterseniz kolay adlarını ve veri kaynağı etiketleri atayın.
• Eski modunda, "Korumalı verileri Koru" ve veri kaynaklarını korumayı durdurun her veri kaynağı için.  Bu kurtarma eski kurtarma noktaları geçişten sonra olanak tanır.

• Yeni PG oluşturun ve yeni biçimi kullanarak depolanacağı yeri veri kaynaklarını seçin.
• DPM yerel olarak Modern yedekleme depolama birimine yerleştirilebilmesi eski yedekleme depolama biriminden çoğaltma kopyasını yapın.
Not: Bu görülür tüm yeni eşitleme ve kurtarma noktalarını sonra Modern yedekleme depolama alanında depolanacak kurtarma sonrası işlemi iş • olarak.
Süresi dolacak ve sonunda disk alanını boşaltmanız • eski kurtarma noktalarını kullanıma ayıklanır.
Tüm eski birimleri eski depolama alanından disk silindikten sonra • Azure yedekleme ve sistem kaldırılabilir.
• Azure DPMDB yedeklemesini alın.

2. Kısım:-Önemli öğeleri > Yeni sunucunun özgün Azure yedekleme sunucusu olarak aynı adlı gerekir. Eski depolama havuzu ve DPMDB korumak için kullanmak istiyorsanız, yeni Azure yedekleme sunucusu adını değiştiremezsiniz geri gerekeceğinden, Kurtarma noktaları - DPMDB yedeklemesini olması gerekir

1) Kapatma özgün Azure sunucuyu yedekleme veya devre dışı kablo alır.
2) Makine hesabı active Directory'de sıfırlayın.
3) Server 2016 yeni makineye yüklemek ve onu aynı makine özgün Azure yedekleme sunucusu adı.
4) Etki alanına katılma
5) Azure yedekleme sunucusu V2 yükleyin (taşıma DPM depolama havuzu diskleri eski sunucu ve içeri aktarma)
6) 2. Bölüm sonundan itibaren geçen DPMDB geri yükleme
7) Depolama özgün yedekleme sunucudan yeni bir sunucuya ekleyin.
8) DPMDB'yi SQL geri yükleme
9) Yönetici olarak komut satırı Microsoft Azure yedekleme için yeni sunucu CD'sindeki yükleme konumu ve bin klasörü

Yol örnek: C:\windows\system32 > cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\
Azure için yedekleme Çalıştır DPMSYNC-SYNC

10) Dpmsync aracını çalıştırın-eşitleme Not eskilerle taşıma yerine DPM depolama havuzuna yeni diskler ekledikten sonra çalıştırırsanız DPMSYNC - reallocatereplica öğesini

## <a name="new-powershell-cmdlets-in-v2"></a>V2 yeni PowerShell cmdlet'leri

Azure yedekleme sunucusu v2 yüklediğinizde, iki yeni cmdlet'leri kullanılabilir: 
* [Mount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Sonraki adımlar

Sunucunuzu hazırlama ya da bir iş yükü korumaya başlamak öğrenin:
- [Yedekleme sunucusu iş yükleri hazırlama](backup-azure-microsoft-azure-backup.md)
- [VMware sunucusunu yedeklemek için yedekleme sunucusu kullanın](backup-azure-backup-server-vmware.md)
- [SQL Server için yedekleme sunucusu kullanın](backup-azure-sql-mabs.md)
- [Yedekleme sunucusu ile modern yedekleme depolama alanı kullanın](backup-mabs-add-storage.md)

