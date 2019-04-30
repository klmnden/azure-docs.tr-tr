---
title: VMware vm'lerinin olağanüstü durum kurtarma için bir işlem sunucusu ve fiziksel sunucuları azure'a Azure Site Recovery kullanarak yönetme | Microsoft Docs
description: Bu makalede VMware Vm'lerinin ve fiziksel sunucudan azure'a Azure Site Recovery ile olağanüstü durum kurtarma için ayarlanmış bir işlem sunucusunu.
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 03/11/2019
ms.author: ramamill
ms.openlocfilehash: 0a0b6c83f800c0a479ba7a16c91b497d1a11da9e
ms.sourcegitcommit: a95dcd3363d451bfbfea7ec1de6813cad86a36bb
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "62732494"
---
# <a name="manage-process-servers"></a>İşlem sunucularını yönetme

Varsayılan olarak VMware Vm'lerini veya fiziksel sunucuları Azure'a çoğaltırken kullanılan işlem sunucusu şirket içi yapılandırma sunucusu makineye yüklenir. Birkaç ayrı işlem sunucusu kurmak gereksinim örneklerinin vardır:

- Büyük dağıtımlar için kapasite ölçeklendirme şirket içinde ek işlem sunucularının gerekebilir.
- Yeniden çalışma için ihtiyacınız geçici bir işlem sunucusu Azure'da ayarlayın. Yeniden çalışma tamamlandığında bu VM'yi silebilirsiniz. 

Bu makalede, bu ek işlem sunucularının için tipik yönetim görevleri özetler.

## <a name="upgrade-a-process-server"></a>Bir işlem sunucusunu yükseltme

Şirket içinde veya azure'da (yeniden çalışma amaçları için), şu şekilde çalışan bir işlem sunucusu yükseltme:

[!INCLUDE [site-recovery-vmware-upgrade -process-server](../../includes/site-recovery-vmware-upgrade-process-server-internal.md)]

> [!NOTE]
>   Genellikle, bir işlem sunucusu Azure'da yeniden çalışma amacıyla oluşturmak üzere Azure Galerisi görüntüsünü kullandığınızda, kullanılabilir en son sürümü çalışıyor. Site Recovery yayın düzeltmeler ve geliştirmeler düzenli olarak ekiplerinin ve işlem sunucularının güncel tutmanızı öneririz.

## <a name="balance-the-load-on-process-server"></a>İşlem sunucusu üzerindeki yük dengelemesi

İki işlem sunucusu arasında yük dengelemek için

1. Gidin **kurtarma Hizmetleri kasası** > **yönetme** > **Site Recovery altyapısı** > **için VMware ve fiziksel makineler** > **Configuration Servers**.
2. İşlem sunucusu ile kaydedilen yapılandırma sunucusuna tıklayın.
3. İşlem sunucusu için configuration servers kaydedildi Listesi sayfasında bulabilirsiniz.
4. İş yükü değiştirmek istediğiniz işlem sunucusu tıklayın.

    ![Yük Dengeleme](media/vmware-azure-manage-process-server/LoadBalance.png)

5. Kullanabilir **Yük Dengeleme** veya **anahtar** gereksinime göre aşağıda açıklandığı gibi seçenekleri.

### <a name="load-balance"></a>Yükü dengele

Bu seçeneği aracılığıyla bir veya daha fazla sanal makine seçebilirsiniz ve bunları başka bir işlem sunucusuna aktarabilirsiniz.

1. Tıklayarak **Yük Dengeleme**, aşağı açılan listeden hedef işlem sunucusunu seçin. **Tamam**’a tıklayın.

    ![LoadPS](media/vmware-azure-manage-process-server/LoadPS.PNG)

2. Tıklayarak **makineleri**, geçerli işlem sunucusundan hedef işlem sunucusuna taşımak istediğiniz sanal makineleri seçin. Ortalama veri değişikliği ayrıntılarını, her sanal makineye karşı görüntülenir.
3. **Tamam** düğmesine tıklayın. Altında işinin ilerleme durumunu izlemek **kurtarma Hizmetleri kasası** > **izleme** > **Site Recovery işleri**.
4. Bu işlem sonrası başarılı olarak tamamlanmasına yansıtacak şekilde değişir 15 dakika sürer veya [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server) etkisini hemen.

### <a name="switch"></a>Anahtar

Bu seçenek ile bir işlem sunucusu altında korunan tüm iş yükü farklı bir işlem sunucusuna taşınır.

1. Tıklayarak **anahtar**, hedef işlem sunucusunu seçin, **Tamam**.

    ![Anahtar](media/vmware-azure-manage-process-server/Switch.PNG)

2. Altında işinin ilerleme durumunu izlemek **kurtarma Hizmetleri kasası** > **izleme** > **Site Recovery işleri**.
3. Bu işlem sonrası başarılı olarak tamamlanmasına yansıtacak şekilde değişir 15 dakika sürer veya [yapılandırma sunucusunu Yenile](vmware-azure-manage-configuration-server.md#refresh-configuration-server) etkisini hemen.

## <a name="process-server-selection-guidance"></a>İşlem Sunucusu Seçimi Kılavuzu

İşlem sunucusu kullanım sınırlarını yaklaşıyorsa azure Site Recovery otomatik olarak tanımlar. Sunucu işlemleri, Ölçek genişletme ayarlamak için yönergeler sağlanır.

|Sağlık Durumu  |Açıklama  | Kaynak kullanılabilirliği  | Öneri|
|---------|---------|---------|---------|
| Sağlıklı (yeşil)    |   İşlem sunucusu bağlı ve iyi durumda      |CPU ve bellek kullanımı % 80 aşağısına dışında; Boş alan kullanılabilirlik % 30'luk kümesidir.| Bu işlem sunucusu, ek sunucular korumak için kullanılabilir. Yeni iş yükü içinde olduğundan emin olun [işlem sunucusu sınırları tanımlanmış](vmware-azure-set-up-process-server-scale.md#sizing-requirements).
|Uyarı (turuncu)    |   İşlem sunucusu bağlı, ancak belirli kaynaklar hakkında en üst düzeye ulaşmak üzeresiniz  |   CPU ve bellek kullanımı % 80-95 arasında %:; % 25-30 arasındaki % boş alan kullanılabilirlik kümesidir       | İşlem sunucusu kullanımını yakın eşik değerleri var. Yeni sunucular için aynı işlem sunucusu ekleme için eşik değerleri geçmeden olmasına neden olur ve var olan korumalı öğelerin etkileyebilir. Sürümüne güncelleştirmeleri önerilir [bir genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md#before-you-start) yeni çoğaltmalar için.
|Uyarı (turuncu)   |   İşlem sunucusu bağlı, ancak veri son 30 dakika içinde Azure'a karşıya değildi  |   Eşik sınırları içinde kaynak kullanımı:       | Sorun giderme [verileri karşıya yükleme hataları](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues) yeni iş yüklerini eklemeden önce **veya** [bir genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md#before-you-start) yeni çoğaltmalar için.
|Kritik (kırmızı)    |     İşlem sunucusu bağlantısı kesildi  |  Eşik sınırları içinde kaynak kullanımı:      | Sorun giderme [işlem sunucusu bağlantı sorunları](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues) veya [bir genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md#before-you-start) yeni çoğaltmalar için.
|Kritik (kırmızı)    |     Kaynak kullanımı sınırlarını eşiği geçtiği |  CPU ve bellek kullanımı, %95:; % 25'ten az boş alan kullanılabilirlik kümesidir.   | Yeni iş yükleri için aynı işlem sunucusu ekleme, kaynak eşik sınırları karşılandığından emin olarak devre dışı bırakıldı. Bu nedenle, [bir genişleme işlem sunucusu](vmware-azure-set-up-process-server-scale.md#before-you-start) yeni çoğaltmalar için.
Kritik (kırmızı)    |     Veriler son 45 dakika Azure'dan Azure'a karşıya değildi. |  Eşik sınırları içinde kaynak kullanımı:      | Sorun giderme [verileri karşıya yükleme hataları](vmware-azure-troubleshoot-replication.md#monitor-process-server-health-to-avoid-replication-issues) yeni iş yükleri için aynı işlem sunucusu eklemeden önce veya [bir genişleme işlem Sunucusu Kurulumu](vmware-azure-set-up-process-server-scale.md#before-you-start)

## <a name="reregister-a-process-server"></a>İşlem sunucusu yeniden kaydettirin

Şirket içinde çalışan bir işlem sunucusu yeniden kaydettirin veya Azure'da, yapılandırma sunucusu ile aşağıdakileri yapmanız gerekirse:

[!INCLUDE [site-recovery-vmware-register-process-server](../../includes/site-recovery-vmware-register-process-server.md)]

Ayarları kaydettikten sonra aşağıdakileri yapın:

1. İşlem sunucusu, bir yönetici komut istemi açın.
2. Klasöre göz atın **%PROGRAMDATA%\ASR\Agent**, ve şu komutu çalıştırın:

    ```
    cdpcli.exe --registermt
    net stop obengine
    net start obengine
    ```

## <a name="modify-proxy-settings-for-an-on-premises-process-server"></a>Bir şirket içi işlem sunucusu için proxy ayarlarını değiştirme

İşlem sunucusu, Azure Site Recovery hizmetine bağlanmak için bir ara sunucu kullanıyorsa, var olan proxy ayarları değiştirmeniz gerekirse bu yordamı kullanın.

1. İşlem sunucusu makinede oturum açın. 
2. Bir yönetici PowerShell komut penceresi açın ve aşağıdaki komutu çalıştırın:
   ```powershell
   $pwd = ConvertTo-SecureString -String MyProxyUserPassword
   Set-OBMachineSetting -ProxyServer http://myproxyserver.domain.com -ProxyPort PortNumber –ProxyUserName domain\username -ProxyPassword $pwd
   net stop obengine
   net start obengine
   ```
2. Klasöre göz atın **%PROGRAMDATA%\ASR\Agent**, ve aşağıdaki komutu çalıştırın:
   ```
   cmd
   cdpcli.exe --registermt

   net stop obengine

   net start obengine

   exit
   ```

## <a name="remove-a-process-server"></a>Bir işlem sunucusunu Kaldır

[!INCLUDE [site-recovery-vmware-unregister-process-server](../../includes/site-recovery-vmware-unregister-process-server.md)]

## <a name="manage-anti-virus-software-on-process-servers"></a>Virüsten koruma yazılımı işlem sunucularını yönetme

Virüsten koruma yazılımının bir tek başına işlem sunucusu veya ana hedef sunucu üzerinde etkin değilse, aşağıdaki klasörleri virüsten koruma işlemlerinden dışlayacak:


- C:\Program Files\Microsoft Azure Recovery Services Agent
- C:\ProgramData\ASR
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\ProgramData\Microsoft Azure Site kurtarma
- İşlem sunucusu yükleme dizininde, örneğin: C:\Program dosyaları (x86) \Microsoft Azure Site kurtarma