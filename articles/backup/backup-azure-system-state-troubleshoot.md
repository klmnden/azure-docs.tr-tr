---
title: Azure Backup ile sistem durumu yedekleme sorunlarını giderme
description: Sistem durumu yedeklemesine sorunları giderin.
services: backup
author: srinathvasireddy
manager: sivan
keywords: Yedekleme nasıl; Yedekleme sistem durumu
ms.service: backup
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: srinathv
ms.openlocfilehash: 87b5fff58ecf9e89bc94f31a0bc3a591c146c88f
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705009"
---
# <a name="troubleshoot-system-state-backup"></a>Sistem Durumu yedekleme sorunlarını giderme

Bu makalede, sistem durumu yedeklemesi kullanırken karşılaşabileceğiniz sorunların çözümleri açıklanmaktadır.

## <a name="basic-troubleshooting"></a>Temel sorun giderme
Gerçekleştirmenizi öneririz doğrulama başlamadan önce sistem durumu yedekleme sorunlarını giderme:

- [Microsoft Azure kurtarma Hizmetleri (MARS) Aracısı'nın güncel olduğundan emin olun.](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [MARS aracısı ile Azure arasında ağ bağlantısı sağlandığından emin olun](https://aka.ms/AB-A4dp50)
- Microsoft Azure Kurtarma Hizmetleri'nin çalıştığından emin olun (Hizmet konsolunda). Gerekirse yeniden başlatın ve işlemi yeniden deneyin
- [Boş klasör konumunda %5-10 oranında kullanılabilir alan olduğundan emin olun](https://aka.ms/AB-AA4dwtt)
- [Azure Backup ile çakışan başka bir işlem veya virüsten koruma yazılımı olup olmadığını kontrol edin](https://aka.ms/AB-AA4dwtk)
- [Zamanlanmış yedekleme başarısız oluyor ancak el ile yedekleme çalışıyor](https://aka.ms/ScheduledBackupFailManualWorks)
- İşletim sisteminizde en son güncelleştirmelerin yüklü olduğundan emin olun
- [Desteklenmeyen sürücüleri ve desteklenmeyen öznitelikleri olan dosyaları yedeklemeden hariç tutulur emin olun.](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Korumalı sistemdeki **Sistem Saatinin** doğru saat dilimine göre yapılandırıldığından emin olun <br>
- [Sunucuda en az .Net Framework 4.5.2 ve üzeri bir sürümün yüklü olduğundan emin olun](https://www.microsoft.com/download/details.aspx?id=30653)<br>
- **Sunucunuzu bir kasaya yeniden kaydetmeye** çalışıyorsanız: <br>
  - Aracının sunucudan kaldırıldığından ve portaldan silindiğinden emin olun <br>
  - Sunucu kaydedilirken kullanılan parolayı kullanın <br>
- Çevrimdışı Yedekleme durumunda çevrimdışı yedekleme işlemi başlamadan önce Azure PowerShell sürüm 3.7.0 hem kaynak hem de kopya bilgisayarda yüklü olduğundan emin olun.
- [Backup aracısını bir Azure sanal makinesi üzerinde çalışırken önemli noktalar](https://aka.ms/AB-AA4dwtr)

### <a name="limitation"></a>Sınırlama
- Sistem Durumu kurtarmayı kullanarak farklı donanımda kurtarma işlemi yapılması Microsoft tarafından önerilmez
- Sistem Durumu yedekleme şu an "şirket içi" Windows sunucuları destekler, bu işlev, Azure Vm'leri için kullanılamıyor.

## <a name="pre-requisite"></a>Önkoşul

Biz Azure Backup ile sistem durumu yedeklemesi gidermeye başlamadan önce yeniden sağlamak ön koşulları denetleyin.  

### <a name="verify-windows-server-backup-is-installed"></a>Windows Server Yedekleme yüklendiğini doğrulayın

Windows Server Yedekleme yüklenir ve etkinleştirilir sunucu emin olun. Yükleme durumunu denetlemek için çalıştırın aşağıdaki PowerShell komutu:

 ```
 PS C:\> Get-WindowsFeature Windows-Server-Backup
 ```
Çıktıyı görüntüler varsa **yükleme durumu** olarak **kullanılabilir**, Windows Server Yedekleme özelliğini yükleme için kullanılabilir, ancak sunucuda yüklü değil demektir. Windows Server Yedekleme yüklü değilse, ancak ardından birini aşağıdaki yöntemlerden yüklemek için.

**1. yöntem: PowerShell kullanarak Windows Server Yedekleme'yi yükleyin**

PowerShell kullanarak Windows Server Yedekleme yüklemek için aşağıdaki komutu:

  ```
  PS C:\> Install-WindowsFeature -Name Windows-Server-Backup
  ```

**2. yöntem: Sunucu Yöneticisi'ni kullanarak Windows Server Yedekleme'yi yükleyin**

Windows Server Yedekleme, Sunucu Yöneticisi'ni yüklemek için gerçekleştirmek aşağıda:

1. İçinde **Sunucu Yöneticisi'ni** tıklatıp **rol ve Özellik Ekle**. **Rol ve Özellik Ekleme Sihirbazı** görünür.

    ![Pano](./media/backup-azure-system-state-troubleshoot/server_management.jpg)

2. Seçin **yükleme türünü** tıklatıp **sonraki**.

    ![Yükleme türü](./media/backup-azure-system-state-troubleshoot/install_type.jpg)

3. Sunucu havuzundan bir sunucu seçin ve tıklayın **sonraki**. Sunucu rolünde varsayılan seçimi bırakın ve tıklayın **sonraki**.
4. Seçin **Windows Server Yedekleme** içinde **özellikleri** sekmesine **sonraki**.

    ![SaaS Uygulamaları Geliştirme](./media/backup-azure-system-state-troubleshoot/features.png)

5. İçinde **onay** sekmesinde **yükleme** yükleme işlemini başlatmak için.
6. İçinde **sonuçları** sekmesinde, bu görüntüler Windows Server Yedekleme özelliği Windows Server'da başarıyla yüklendi.

    ![Sonuç](./media/backup-azure-system-state-troubleshoot/results.jpg)


### <a name="system-volume-information-permission"></a>Sistem birimi bilgileri izni

Yerel sistem tam denetim olduğundan emin **Sistem Birim bilgisi** klasöründe, Windows'un yüklü olduğu birimin. Genellikle budur **C:\System Volume Information**. Yukarıdaki izinleri doğru ayarlanmamışsa, Windows Server yedekleme başarısız olabilir

### <a name="dependent-services"></a>Bağımlı hizmetleri

Olun hizmetlerdir çalışır durumda:

**Hizmet adı** | **Başlangıç türü**
--- | ---
Uzak yordam çağrısı (RPC) | Otomatik
COM + olay System(EventSystem) | Otomatik
Sistem olay bildirim Service(SENS) | Otomatik
Birim Gölge kopyası (VSS) | Manual
Microsoft yazılım gölge kopya Provider(SWPRV) | Manual

### <a name="validate-windows-server-backup-status"></a>Windows Server Yedekleme durumunu doğrula

Windows Server Yedekleme durumunu doğrulamak için gerçekleştirmek aşağıda:

  * WSB PowerShell çalıştığından emin olun

    -   Çalıştırma `Get-WBJob` yükseltilmiş bir PowerShell ve emin aşağıdaki hata döndürmez:

    > [!WARNING]
    > Get-WBJob: ' % S'terim 'Get-WBJob' cmdlet'i, işlev, komut dosyası veya çalıştırılabilir program adı olarak tanınmıyor. Adının yazımını denetleyin veya bir yol varsa, yolun doğru olduğundan emin olun ve yeniden deneyin.

    -   Bu hata ile başarısız olursa 1. adım önkoşullarda belirtildiği gibi server makinesinde Windows Server Yedekleme özelliği yeniden yükleyin.

  * WSB yedekleme düzgün çalıştığından, çalıştırılarak sağlamak aşağıdaki komutu yükseltilmiş komut isteminden:

      `wbadmin start systemstatebackup -backuptarget:X: -quiet`

      > [!NOTE]
      >Sistem durumunu depolamak için istediğiniz birimin sürücü harfiyle değiştirin X görüntüyü yedekleyin.

    - Düzenli aralıklarla işinin durumunu denetleyin `Get-WBJob` yükseltilmiş bir PowerShell komutu        
    - Yedekleme işi tamamlandıktan sonra son iş durumunu denetleyin `Get-WBJob -Previous 1` komutu

İş başarısız olursa, MARS Aracısı sistem durumu yedeklemeleri hatası sonuçlanır bir WSB sorunu gösterir.

## <a name="common-errors"></a>Sık karşılaşılan hatalar

### <a name="vss-writer-timeout-error"></a>VSS Yazıcı zaman aşımı hatası

| Belirti | Nedeni | Çözüm
| -- | -- | --
| -MARS Aracısı hata iletisiyle başarısız olur: "WSB işi VSS hatalarla başarısız oldu. Hatayı gidermek için VSS Olay günlüklerini denetleyin"<br/><br/> -Hata aşağıdaki günlük VSS uygulama olay günlüklerinde bulunmaktadır: "Arasındaki dondurma/çözme olayları yazıcı'nın süresi doldu, VSS yazıcısı hata 0x800423f2 olan bir olayı reddetti."| VSS Yazıcı nedeniyle süresinde makine üzerindeki CPU ve bellek kaynaklarının yetersizliği nedeniyle tamamlanamadı <br/><br/> Başka bir yedekleme yazılımları, VSS yazıcısını zaten kullanıyor, sonuç olarak bu yedekleme için anlık görüntü işlemi tamamlanamadı | CPU/sistemde önbellekten veya çok fazla bellek/CPU alma işlemleri iptal etmek ve işlemi yeniden deneyin. bellek için bekleyin <br/><br/>  Devam eden yedekleme işlemi daha sonraki bir noktada yedekleme, makinede çalışırken deneyin bekleyin


### <a name="insufficient-disk-space-to-grow-shadow-copies"></a>Gölge kopyaları büyüme için yeterli disk alanı

| Belirti | Çözüm
| -- | --
| -MARS Aracısı hata iletisiyle başarısız olur: Gölge kopya birimi yetersiz disk alanı nedeniyle sistem dosyalarını içeren birimlerde büyüyebilir değil olarak yedekleme başarısız oldu. <br/><br/> -Hata/uyarı günlüğü aşağıdaki volsnap sistem olay günlüklerine bulunmaktadır: "Oluştu. yeterli disk alanı biriminde ve gölge kopya depolama C: gölge kopyalarını bu hata nedeniyle büyümesine C: C: silinmesiyle sonuçlanacak bir risk altında olan birimin tüm gölge kopyaları" | -Alanını boşaltın vurgulanan toplu olay günlüğünde yedekleme devam ederken büyütmek gölge kopyaları için yeterli alanı olmasını sağlayın <br/><br/> -Gölge kopya alanı biz gölge kopya için kullanılan alanı miktarını kısıtlayabilir yapılandırırken, daha fazla bilgi için bu bkz [makale](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788050(v=ws.11)#syntax)


### <a name="efi-partition-locked"></a>Kilitlenmiş EFI bölümü

| Belirti | Çözüm
| -- | --
| MARS Aracısı hata iletisiyle başarısız olur: "EFI sistem bölümü kilitli olduğundan sistem durumu başarısız oldu. Bu sistem bölümüne erişimi tarafından üçüncü taraf güvenlik nedeniyle olabilir veya yazılım geri" | -Bir üçüncü taraf güvenlik yazılımları nedeniyle sorundur, MARS Aracısı izin verebilir böylece yazılımdan koruma virüs satıcınızla iletişime geçin gerekir <br/><br/> -Üçüncü taraf yedekleme yazılımı çalıştırıyorsa, ardından ve yukarı arka daha sonra yeniden deneyin bekleyin


## <a name="next-steps"></a>Sonraki adımlar

- Resource Manager dağıtımında Windows sistem durumu hakkında daha fazla bilgi için bkz: [Windows Server sistem durumunu yedekleme](backup-azure-system-state.md)
