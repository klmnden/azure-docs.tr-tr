---
title: Şirket içi Windows Server 2008 sunucuları Azure Site Recovery ile Azure'a geçiş | Microsoft Docs
description: Bu makalede, Azure Site Recovery kullanılarak Azure'da, şirket içi Windows Server 2008 makineleri geçirmeyi açıklar.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/23/2018
ms.author: bsiva
ms.openlocfilehash: 6e5946f3f9dcf1c7d941054c844adcf683b485ab
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308652"
---
# <a name="migrate-servers-running-windows-server-2008-to-azure"></a>Azure'da Windows Server 2008 çalıştıran sunucularının geçirme

Bu öğreticide Azure Site Recovery kullanılarak Azure'da Windows Server 2008 veya 2008 R2 çalıştıran şirket içi sunucuları geçirmek nasıl gösterir. Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Şirket içi ortamınızı geçiş için hazırlama
> * Hedef ortamı ayarlama
> * Çoğaltma ilkesi ayarlama
> * Çoğaltmayı etkinleştirme
> * Her şeyin beklendiği gibi çalıştığından emin olmak için bir geçiş testi çalıştırma
> * Azure'a yük devretme ve geçiş işlemi

Azure'a geçirme Windows Server 2008 makineler ederken, sınırlamalar ve bilinen sorunlar bölümünde bazı sınırlamalar ve geçici çözümler için bilinen sorunlar, listeler karşılaşabilirsiniz. 


## <a name="supported-operating-systems-and-environments"></a>Desteklenen işletim sistemleri ve ortamlar


|İşletim Sistemi  | Şirket içi ortamı  |
|---------|---------|
|Windows Server 2008 SP2 - 32 bit ve 64 bit (IA-32 ve 64 x86)</br>-Standart</br>-Kurumsal</br>-Datacenter   |     VMware Vm'leri, Hyper-V Vm'lerini ve fiziksel sunucuları    |
|Windows Server 2008 R2 SP1 - 64 bit</br>-Standart</br>-Kurumsal</br>-Datacenter     |     VMware Vm'leri, Hyper-V Vm'lerini ve fiziksel sunucuları|

> [!WARNING]
> - Sunucu Çekirdeği çalıştıran sunucuların geçişi desteklenmiyor.
> - En son hizmet paketini ve Windows güncelleştirmelerini geçirmeden önce yüklü olduğundan emin olun.


## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce Azure Site Recovery mimarisi gözden geçirmeniz yararlıdır [VMware ve fiziksel sunucu geçişi](vmware-azure-architecture.md) veya [Hyper-V sanal makine geçişi](hyper-v-azure-architecture.md) 

Windows Server 2008 veya Windows Server 2008 R2 çalıştıran Hyper-V sanal makinelerini geçirmek için adımları izleyin. [şirket içi makineleri Azure'a geçirme](migrate-tutorial-on-premises-azure.md) öğretici.

Bu öğreticinin geri kalanını nasıl şirket içi VMware sanal makinelerini ve fiziksel sunucuları Windows Server 2008 veya 2008 R2 çalıştıran geçirebilirsiniz gösterir.


## <a name="limitations-and-known-issues"></a>Sınırlamalar ve bilinen sorunlar

- Yapılandırma sunucusu, ek işlem sunucularının ve mobility hizmetini Windows Server 2008 SP2 sunucularını geçirmek için kullanılan Azure Site kurtarma yazılımının 9.18.0.1 sürümünü çalıştırmalıdır. Sürüm 9.18.0.1 yapılandırma sunucusu ve işlem sunucusu için birleşik Kurulumu indirilebileceğini [ https://aka.ms/asr-w2k8-migration-setup ](https://aka.ms/asr-w2k8-migration-setup).

- Bir mevcut yapılandırma sunucusu veya işlem sunucusu, Windows Server 2008 SP2 çalıştıran sunucuları geçirmek için kullanılamaz. Yeni bir yapılandırma sunucusu, Azure Site kurtarma yazılımının 9.18.0.1 sürümü ile sağlanmalıdır. Bu yapılandırma sunucusu, yalnızca Windows sunucularının azure'a geçiş için kullanılmalıdır.

- Windows Server 2008 SP2 çalıştıran sunucuların çoğaltma için uygulama tutarlı kurtarma noktalarına ve çoklu VM tutarlılığı özelliği desteklenmez. Bir kilitlenme tutarlı bir kurtarma noktası için Windows Server 2008 SP2 sunucuları geçirilmelidir. Kilitlenme tutarlı kurtarma noktalarına her 5 dakikada bir, varsayılan olarak oluşturulur. Yapılandırılmış uygulama tutarlı anlık görüntü sıklığı ile bir çoğaltma İlkesi'ni kullanarak uygulama tutarlı kurtarma noktalarına eksikliği nedeniyle kritik etkinleştirmek çoğaltma durumu neden olur. Hatalı pozitif sonuçlardan kaçınmak için uygulamayla tutarlı anlık görüntü sıklığı "Kapalı" çoğaltma ilkesi ayarlayın.

- Geçirilmekte olan sunucuların, .NET Framework 3.5 Service Pack çalışmak için 1 mobility hizmeti için sahip olması gerekir.


- Dinamik disk sunucunuz varsa, bu diskleri bazı yapılandırmalarda, karşılaşabilirsiniz başarısız sunucusu çevrimdışı ya da gösterilmesini yabancı diskler olarak işaretlenmiş. Ayrıca dinamik diskler arasında yansıtılmış birimler için yansıtılmış kümesi durum, "yedekleme başarısız oldu" olarak işaretlenmiş fark edebilirsiniz. El ile bu diskleri alma ve bunları yeniden etkinleştirme diskmgmt.msc bu sorunu düzeltebilirsiniz.

- Geçirilmekte olan sunucuları vmstorfl.sys sürücü olması gerekir. Sürücü Geçirilmekte olan sunucuda mevcut değilse, yük devretme başarısız olabilir. 
  > [!TIP]
  >Sürücü "C:\Windows\system32\drivers\vmstorfl.sys sırasında" mevcut olup olmadığını denetleyin. Sürücü bulunamadı, yerinde işlevsiz bir dosya oluşturarak sorunun geçici çözümü kullanabilirsiniz. 
  >
  > Komut istemini Aç (çalıştırın > cmd) ve aşağıdaki komutu çalıştırın: "nul c:\Windows\system32\drivers\vmstorfl.sys Kopyala"

- Sizin için RDP üzerinden hemen başarısız olduktan sonra 32-bit işletim sistemi ya da Azure'a yük devretme testi çalıştıran Windows Server 2008 SP2 sunucu olabilir. Azure portalında sanal makine üzerinde başarısız yeniden başlatın ve yeniden bağlanmayı deneyin. Bağlanmak hala bulamıyorsanız, Uzak Masaüstü bağlantılarına izin ver ve hiçbir güvenlik duvarı kuralları veya ağ güvenlik grupları bağlantıya engel olmasını sağlamak için sunucu yapılandırılıp yapılandırılmadığını denetleyin. 
  > [!TIP]
  > Yük devretme testi geçirmeden önce önemle tavsiye edilir sunucuları. Geçirdiğiniz her sunucuda en az bir başarılı test yük devretmesi gerçekleştirdiğiniz emin olun. Test yük devretmenin bir parçası olarak işlemi yapılan makine üzerinde test bağlanabilir ve beklendiği gibi şeyler iş emin olun.
  >
  >Test yük devretme işlemi kesintiye neden olmayan ve tercih ettiğiniz yalıtılmış ağ sanal makineler oluşturarak geçişleri test yardımcı olur. Test yük devretme işlemi sırasında bir yük devretme işlemi aksine veri çoğaltma için progres devam eder. Geçişe hazır olduğunuzda önce dilediğiniz kadar test yük devretme gerçekleştirebilirsiniz. 
  >
  >


## <a name="getting-started"></a>Başlarken

Azure aboneliği ve şirket içi VMware/fiziksel ortamı hazırlamak üzere aşağıdaki görevleri gerçekleştirin:

1. [Azure’u hazırlama](tutorial-prepare-azure.md)
2. Şirket içi hazırlama [VMware](vmware-azure-tutorial-prepare-on-premises.md)


## <a name="create-a-recovery-services-vault"></a>Kurtarma Hizmetleri kasası oluşturma

1. [Azure Portal](https://portal.azure.com) > **Kurtarma Hizmetleri**’nde oturum açın.
2. **Kaynak oluştur** > **İzleme ve Yönetim** > **Backup ve Site Recovery** öğesine tıklayın.
3. İçinde **adı**, kolay ad belirtin **W2K8 geçiş**. Birden fazla aboneliğiniz varsa uygun olanı seçin.
4. Bir kaynak grubu oluşturma **w2k8migrate**.
5. Bir Azure bölgesi belirtin. Desteklenen bölgeleri kontrol etmek için [Azure Site Recovery Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/site-recovery/) bölümündeki coğrafi kullanılabilirlik kısmına bakın.
6. Panodan kasaya hızlıca erişmek için önce **Panoya sabitle** seçeneğine ve sonra **Oluştur**’a tıklayın.

   ![Yeni kasa](media/migrate-tutorial-windows-server-2008/migrate-windows-server-2008-vault.png)

Yeni kasa, **Pano**’da **Tüm kaynaklar** bölümüne ve ana **Kurtarma Hizmetleri kasaları** sayfasına eklenir.


## <a name="prepare-your-on-premises-environment-for-migration"></a>Şirket içi ortamınızı geçiş için hazırlama

- Yapılandırma sunucusunu (Birleşik kurulum) Yükleyici indirin [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup)
- Önceki adımda yüklenen Installer dosyası kullanarak kaynak ortamı ayarlamak için aşağıda açıklanan adımları izleyin.

> [!IMPORTANT]
> - Yükleme ve yapılandırma sunucusunu kaydetmek için yukarıdaki ilk adımında indirilen kurulum dosyasına kullandığınızdan emin olun. Azure portalından kurulum dosyasını karşıdan yüklemeyin. Kullanılabilir kurulum dosyasını [ https://aka.ms/asr-w2k8-migration-setup ](https://aka.ms/asr-w2k8-migration-setup) Windows Server 2008 geçişi destekler yalnızca sürümüdür.
>
> - Var olan bir yapılandırma sunucusu, Windows Server 2008 çalıştıran makineleri geçirmek için kullanamazsınız. Yukarıda sağlanan bağlantıyı kullanarak yeni bir yapılandırma sunucusu gerekir.
>
> - Yapılandırma sunucusunu yüklemek için aşağıda verilen adımları izleyin. Birleşik Kurulumu çalıştırarak doğrudan GUI tabanlı yükleme yordamı kullanmaya çalışmayın. Bunun yapılması bir yanlış internet bağlantısı olmayan olduğunu belirten hata ile başarısız yükleme denemesi neden olur.

 
1) Portaldan kasa kimlik bilgileri dosyası indirin: Azure portalında, önceki adımda oluşturduğunuz kurtarma Hizmetleri kasasını seçin. Kasası sayfasında menüsünden seçin **Site Recovery altyapısı** > **Configuration Servers**. Ardından **+ sunucusu**. Seçin *fiziksel için yapılandırma sunucusu* açılır sayfasındaki formu açılır. Adım 4 kasa kimlik bilgileri dosyası indirmek için indir düğmesine tıklayın.

 ![Kasa kayıt anahtarını indir](media/migrate-tutorial-windows-server-2008/download-vault-credentials.png) 

2) Kasa kimlik bilgileri dosyası, önceki adımda ve birleşik kurulum dosyasını karşıdan kopyalama daha önce indirdiğiniz masaüstüne yapılandırması sunucu makinesinin (Windows Server 2012 R2 veya Windows Server 2016 makine üzerinde seçeceğiz yüklemek için yapılandırma sunucusu yazılım.)

3) Yapılandırma sunucusunu internet bağlantısı olduğundan ve bu makinedeki saat dilimi ayarları ve sistem saati doğru yapılandırıldığından emin olun. İndirme [MySQL 5.7](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi) yükleyici ve üzerine yerleştirin *C:\Temp\ASRSetup* (dizin yoksa oluşturun.) 

4) Aşağıdaki satırları içeren bir MySQL kimlik bilgileri dosyası oluşturun ve masaüstünde Yerleştir **C:\Users\Administrator\MySQLCreds.txt** . Değiştir "parola ~ 1" altında uygun ve güçlü bir parola ile:

```
[MySQLCredentials]
MySQLRootPassword = "Password~1"
MySQLUserPassword = "Password~1"
```

5) Aşağıdaki komutu çalıştırarak masaüstüne indirilen birleşik Kurulum dosyasının içeriğini ayıklayın:

```
cd C:\Users\Administrator\Desktop

MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Users\Administrator\Desktop\9.18
```
  
6) Ayıklanan içerikleri aşağıdaki komutları yürüterek kullanarak yapılandırma sunucusu yazılımı yükleyin:

```
cd C:\Users\Administrator\Desktop\9.18.1

UnifiedSetup.exe /AcceptThirdpartyEULA /ServerMode CS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /MySQLCredsFilePath "C:\Users\Administrator\Desktop\MySQLCreds.txt" /VaultCredsFilePath <vault credentials file path> /EnvType VMWare /SkipSpaceCheck
```

## <a name="set-up-the-target-environment"></a>Hedef ortamı ayarlama

Hedef kaynaklarını seçin ve doğrulayın.

1. **Altyapıyı hazırlama** > **Hedef** seçeneklerine tıklayıp kullanmak istediğiniz Azure aboneliğini seçin.
2. Kaynak Yöneticisi dağıtım modelini belirtin.
3. Site Recovery, bir veya birden çok uyumlu Azure depolama hesabınızın ve ağınızın olup olmadığını denetler.


## <a name="set-up-a-replication-policy"></a>Çoğaltma ilkesi ayarlama

1. Yeni bir çoğaltma ilkesi oluşturmak için tıklayın **Site Recovery altyapısı** > **çoğaltma ilkeleri** > **+ Çoğaltma İlkesi**.
2. İçinde **çoğaltma ilkesi oluşturma**, bir ilke adı belirtin.
3. İçinde **RPO eşiği**, kurtarma noktası hedefi (RPO) sınırı belirtin. RPO çoğaltma Bu limiti aşarsa bir uyarı üretilir.
4. İçinde **kurtarma noktası bekletme**, (saat) cinsinden ne kadar saklama aralığı için her bir kurtarma noktası belirtin. Çoğaltılan VM’ler bir aralıktaki herhangi bir noktaya kurtarılabilir. 24 saat bekletme için premium depolama ve standart depolama için 72 saat çoğaltılan makineler için desteklenir.
5. İçinde **uygulamayla tutarlı anlık görüntü sıklığı**, belirtin **kapalı**. İlkeyi oluşturmak için **Tamam**’a tıklayın.

İlke, yapılandırma sunucusu ile otomatik olarak ilişkilendirilir.

> [!WARNING]
> Belirttiğinizden emin olun **OFF** frekansına çoğaltma ilkesinin uygulamayla tutarlı anlık görüntüsünü alın. Yalnızca kilitlenme ile tutarlı kurtarma noktaları, Windows Server 2008 çalıştıran sunucularının çoğaltılırken desteklenir. Belirtmeyi sunucusunun çoğaltma durumu kritik uygulamayla tutarlı kurtarma noktalarını eksikliği nedeniyle kapatarak, uygulamayla tutarlı anlık görüntü sıklığı için başka bir değer false uyarıları neden olur.

   ![Çoğaltma ilkesi oluşturma](media/migrate-tutorial-windows-server-2008/create-policy.png)

## <a name="enable-replication"></a>Çoğaltmayı etkinleştirme

[Çoğaltmayı etkinleştirme](physical-azure-disaster-recovery.md#enable-replication) Windows Server 2008 SP2 / Windows Server 2008 R2 SP1 server geçirilecek.
   
   ![Fiziksel sunucu ekleme](media/migrate-tutorial-windows-server-2008/Add-physical-server.png)

   ![Çoğaltmayı etkinleştirme](media/migrate-tutorial-windows-server-2008/Enable-replication.png)

## <a name="run-a-test-migration"></a>Geçiş testi çalıştırma

Yük devretme testi sunucu durumunu etkinleştirir ve ilk çoğaltma tamamlandıktan sonra sunucuları çoğaltmak gerçekleştirebileceğiniz **korumalı**.

Her şeyin beklendiği gibi çalıştığından emin olmak için bir Azure’a [yük devretme testi](tutorial-dr-drill-azure.md) çalıştırın.

   ![Yük devretme testi](media/migrate-tutorial-windows-server-2008/testfailover.png)


## <a name="migrate-to-azure"></a>Azure’a geçiş

Geçirmek istediğiniz makineler için yük devretmeyi çalıştırın.

1. **Ayarlar** > **Çoğaltılan öğeler** bölümünde makine > **Yük devretme**’ye tıklayın.
2. **Yük devretme**’de yük devretmenin yapılacağı bir **Kurtarma Noktası** seçin. En son kurtarma noktasını seçin.
3. **Yük devretmeyi başlatmadan önce makineyi kapatın** seçeneğini belirleyin. Site Recovery, yük devretmeyi tetiklemeden önce sunucuyu kapatın dener. Kapatma işlemi başarısız olsa bile yük devretme devam eder. Yük devretme işleminin ilerleme durumunu **İşler** sayfasında takip edebilirsiniz.
4. Azure VM’nin Azure’da beklendiği gibi görüntülenip görüntülenmediğini kontrol edin.
5. **Çoğaltılan öğeler** bölümünde VM’ye sağ tıklayıp **Geçişi Tamamla**’ya tıklayın. Böylece geçiş işlemi tamamlanır, VM için çoğaltma durdurulur ve sanal makine için Site Recovery faturalaması durdurulur.

   ![Geçişi tamamlama](media/migrate-tutorial-windows-server-2008/complete-migration.png)


> [!WARNING]
> **Devam eden yük devretme işlemini iptal etmeyin**: Yük devretme başlatılmadan önce VM çoğaltma durdurulur. Devam eden bir yük devretme işlemini iptal ederseniz yük devretme durdurulur, ancak VM yeniden çoğaltılmaz.
