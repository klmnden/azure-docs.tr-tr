---
title: StorSimple dosya paylaşımını DR Azure Site Recovery ile otomatik hale getirin | Microsoft Docs
description: Adımları ve Microsoft Azure StorSimple depolama alanında barındırılan dosya paylaşımları için bir olağanüstü durum kurtarma çözümü oluşturmaya yönelik en iyi uygulamaları açıklar.
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: ''
ms.assetid: 23049a2c-055e-4d0e-b8f5-af2a87ecf53f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/13/2017
ms.author: vidarmsft
ms.openlocfilehash: 8c82170cf9cff1870739bb13db9ac0e348a46c07
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67443057"
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>StorSimple üzerinde barındırılan dosya paylaşımları için Azure Site Recovery kullanarak otomatik olağanüstü durum kurtarma çözümü

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple, yaygın olarak ilişkilendirilen dosya paylaşımları ile yapılandırılmamış verileri karmaşıklığını ele alan bir hibrit bulut depolaması çözümü ' dir. StorSimple şirket içi depolama ve bulut depolama alanı üzerinden şirket içi çözüm ve otomatik olarak katmanları verileri bir uzantısı olarak bulut depolama kullanır. Yerel ile tümleşik veri koruması ve bulut anlık görüntüleri, sprawling bir depolama altyapısı ihtiyacını ortadan kaldırır.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) çoğaltma, yük devretme ve sanal makineleri kurtarma işlemlerini düzenleyerek olağanüstü durum kurtarma (DR) özellikleri sağlayan bir Azure tabanlı bir hizmettir. Azure Site Recovery çoğaltma sürekli olarak çoğaltmak, korumayı ve sorunsuz bir şekilde sanal makineler ve private/public veya barındırılan bulut uygulamaları yük devretme teknolojilerini destekler.

Azure Site Recovery, sanal makine çoğaltma ve StorSimple bulut anlık görüntü özelliklerini kullanarak tüm dosya sunucusu ortamı koruyabilirsiniz. Bir kesinti olması durumunda dosya paylaşımlarınızı çevrimiçi yalnızca birkaç dakika içinde Azure'da getirmek için tek tıklamayla kullanabilirsiniz.

Bu belge, nasıl StorSimple depolama üzerinde barındırılan, dosya paylaşımları için bir olağanüstü durum kurtarma çözümü oluşturma ve gerçekleştirme planlanmış, Planlanmayan ve test yük devretmeleri tek tıklamayla kurtarma planı kullanarak ayrıntılı olarak açıklanmaktadır. Esas olarak, nasıl, kurtarma planı StorSimple yük devretme işlemleri sırasında olağanüstü durum senaryoları etkinleştirmek için Azure Site Recovery kasanızın değiştirebilirsiniz gösterir. Ayrıca, desteklenen yapılandırmalar ve önkoşulları açıklar. Bu belge, Azure Site Recovery ve StorSimple mimarileri temelleri hakkında bilgi sahibi olduğunuzu varsayar.

## <a name="supported-azure-site-recovery-deployment-options"></a>Desteklenen Azure Site Recovery dağıtım seçenekleri
Müşteriler, fiziksel sunucu veya Hyper-V veya VMware üzerinde çalışan sanal makineler (VM'ler) olarak dosya sunucuları dağıtmak ve sonra gerekmez dışında StorSimple depolama birimleri dosya paylaşımları oluşturabilirsiniz. Azure Site Recovery, ikincil bir siteye veya azure'a fiziksel ve sanal dağıtımları koruyabilirsiniz. Bu belge, bir DR çözümü VM Hyper-V üzerinde barındırılan bir dosya sunucusu için kurtarma sitesi olarak Azure ve StorSimple depolama dosya paylaşımı ile ayrıntılarını içermektedir. Dosya sunucusu VM'sine bir VMware VM veya fiziksel makine üzerinde olduğu diğer senaryolar benzer şekilde uygulanabilir.

## <a name="prerequisites"></a>Önkoşullar
StorSimple depolama alanında barındırılan dosya paylaşımları için Azure Site Recovery kullanan bir tek tıklamayla olağanüstü durum kurtarma çözümü uygulamak için aşağıdaki önkoşulları vardır:

   - Şirket içi VM, Hyper-V veya VMware veya fiziksel bir makinede barındırılan Windows Server 2012 R2 dosya sunucusu
   - StorSimple depolama cihazı şirket içi Azure StorSimple Manager'a kayıtlı
   - StorSimple Cloud Appliance Azure StorSimple Yöneticisi'nde oluşturulur. Gereç kapalı durumda tutulabilir.
   - StorSimple depolama cihazında birimlerde barındırılan dosya paylaşımları
   - [Azure Site kurtarma Hizmetleri kasası](../site-recovery/site-recovery-vmm-to-vmm.md) bir Microsoft Azure aboneliği için oluşturulan

Ayrıca, Azure kurtarma siteniz varsa, çalıştırmak [Azure sanal makine hazır olma durumu değerlendirmesi aracı](https://azure.microsoft.com/downloads/vm-readiness-assessment/) Hizmetleri sanal makinesi, Azure Vm'leri ve Azure Site Recovery ile uyumlu olduklarından emin olun.

Gecikme süresi, StorSimple Cloud Appliance, Otomasyon hesabı ve depolama oluşturma emin olun (Bu, maliyetin neden olabilir) sorunları önlemek için aynı bölgede hesabı.

## <a name="enable-dr-for-storsimple-file-shares"></a>StorSimple dosya paylaşımları için DR etkinleştir
Şirket içi Ortamı'nın her bileşeninin tam çoğaltma ve kurtarma sağlamak için korunması gerekir. Bu bölümde açıklanmaktadır nasıl yapılır:
    
   - Active Directory ve DNS çoğaltma işlemini (isteğe bağlı) ayarlama
   - Dosya sunucusu VM korumasını etkinleştirmek için Azure Site RECOVERY'yi kullanın.
   - StorSimple birimlerini korumasını etkinleştirin
   - Ağı yapılandırma

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Active Directory ve DNS çoğaltma işlemini (isteğe bağlı) ayarlama
DR sitede kullanılabilir, böylece Active Directory ve DNS'yi çalıştıran makineleri korumak istiyorsanız, açıkça bunları korumak (dosya sunucuları kimlik doğrulaması ile yük devretme sonrasında erişilebilir olacak şekilde) gerekir. Bir müşterinin şirket içi ortamda karmaşıklığı iki önerilen seçenek vardır.

#### <a name="option-1"></a>Seçenek 1
Az sayıda uygulamayı müşterinin, tamamı tek bir etki alanı denetleyicisi şirket içinde site ve ikincil etki alanı denetleyicisi makinesine çoğaltmak için Azure Site Recovery çoğaltma kullanmanızı öneririz sonra tüm site için başarısız site (siteden siteye ve Azure için site için geçerli olur).

#### <a name="option-2"></a>Seçenek 2
Müşteri sahip çok sayıda uygulama, bir Active Directory ormanı çalıştırmayı ve bazı uygulamalar üzerinde aynı anda başarısız olabilir ve DR sitedeki bir ek etki alanı denetleyicisi ayarlama öneririz, (ikincil bir siteye veya Azure).

Başvurmak [Active Directory ve Azure Site Recovery ile Azure DNS için otomatik DR çözümü](../site-recovery/site-recovery-active-directory.md) bir etki alanı denetleyicisi DR sitesi üzerindeki yaparken, yönergeler için. Bu belgenin geri kalanı için bir etki alanı denetleyicisi DR sitesi üzerindeki kullanılabilir varsayıyoruz.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Dosya sunucusu VM korumasını etkinleştirmek için Azure Site RECOVERY'yi kullanın.
Bu adım, şirket içi dosya sunucusu ortamı hazırlama gerektirir, oluşturma ve Azure Site Recovery kasası hazırlamak ve dosya VM korumasını etkinleştirin.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Şirket içi dosya sunucusu ortamı hazırlamak için
1. Ayarlama **kullanıcı hesabı denetimi** için **hiçbir zaman uyarma**. Bu gereklidir, böylece Azure Site Recovery tarafından yük devretme sonrasında iSCSI hedefleri bağlanmak için Azure Otomasyon betikleri kullanabilirsiniz.
   
   1. Windows tuşu + Q tuşlarına basın ve arama **UAC**.  
   1. Seçin **değişiklik kullanıcı hesabı denetimi ayarlarını**.  
   1. Yukarıdan aşağıya doğru çubuğunu sürükleyin **hiçbir zaman uyarma**.  
   1. Tıklayın **Tamam** seçip **Evet** istendiğinde.  
   
      ![Kullanıcı Hesabı Denetimi Ayarları](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png) 

1. VM Aracısı her dosya sunucusu sanal makineleri yükleyin. Bu, Azure Otomasyon betikleri başarısız VM'lerin yükünü çalıştırabilmeniz için gereklidir.
   
   1. [Aracıyı indirin](https://aka.ms/vmagentwin) için `C:\\Users\\<username>\\Downloads`.
   1. Windows PowerShell'i Yönetici modunda (yönetici olarak çalıştır) açın ve ardından indirme konumuna gitmek için aşağıdaki komutu girin:  
         `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`
         
         > [!NOTE]
         > Dosya adı, sürüme bağlı olarak değişebilir.
      
1. **İleri**’ye tıklayın.
1. Kabul **anlaşma koşulları** ve ardından **sonraki**.
1. **Son**'a tıklayın.
1. StorSimple depolama alanınız gerekmez birimleri kullanarak dosya paylaşımları oluşturun. Daha fazla bilgi için [birimleri yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manage-volumes.md).
   
   1. Şirket içi Vm'lerinizi Windows tuşu + Q tuşlarına basın ve arama **iSCSI**.
   1. Seçin **iSCSI başlatıcısı**.
   1. Seçin **yapılandırma** sekmesini ve Başlatıcı adını kopyalayın.
   1. [Azure Portal](https://portal.azure.com/)’da oturum açın.
   1. Seçin **StorSimple** sekmesini ve ardından içeren fiziksel cihaz StorSimple Yöneticisi hizmeti seçin.
   1. Birim kapsayıcılarının ve ardından birim oluşturun. (Bu birimler Vm'leri dosya sunucusu dosya paylaşımlarında içindir). Başlatıcı adı kopyalayın ve birimleri oluşturduğunuzda, erişim denetimi kayıtları için uygun bir ad verin.
   1. Seçin **yapılandırma** sekmesi ve not edin cihazın IP adresi.
   1. Şirket içi Vm'lerinizi Git **iSCSI başlatıcısı** yeniden ve Hızlı Bağlan bölümünde IP girin. Tıklayın **Hızlı Bağlan** (cihaz artık bağlı olması).
   1. Azure portal ve select açın **birimleri ve cihazları** sekmesi. Tıklayın **otomatik yapılandırma**. Oluşturduğunuz birimin görüntülenmesi gerekir.
   1. Portalında **cihazları** sekmesini seçip **yeni bir sanal cihaz oluşturma.** (Bir yük devretme oluşması durumunda bu sanal cihazı kullanılır). Bu yeni sanal Cihazınızda ek maliyetlerden kaçınmak için çevrimdışı bir durumda tutulabilir. Sanal cihaz çevrimdışına almak için Git **sanal makineler** bölümünde portalda ve bilgisayarı kapat.
   1. Şirket içi Vm'leri için geri dönün ve Disk Yönetimi'ni açın (Windows tuşu + X tuşlarına basın ve seçin **Disk Yönetimi**).
   1. (Oluşturduğunuz birim sayısı) bağlı olarak bazı ek diskler fark edeceksiniz. İlk select sağ **diski başlatma**seçip **Tamam**. Sağ **Unallocated** bölümünden **yeni basit birim**, bir sürücü harfi atamak ve Sihirbazı tamamlayın.
   1. M tüm diskler için yineleyin. Artık tüm diskler üzerinde gördüğünüz **bu PC** Windows Gezgini'nde.
   1. Bu birimlerde dosya paylaşımları oluşturmak için dosya ve Depolama Hizmetleri rolünü kullanın.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Oluşturma ve Azure Site Recovery kasası hazırlamak için
Başvurmak [Azure Site Recovery belgeleri](../site-recovery/site-recovery-hyper-v-site-to-azure.md) dosya sunucusu VM korumadan önce Azure Site Recovery ile başlamak için.

#### <a name="to-enable-protection"></a>Korumayı etkinleştirmek için
1. İSCSI hedef Azure Site Recovery ile korumak istediğiniz şirket içi Vm'leri bağlantıyı kesin:
   
   1. Windows tuşu + Q tuşlarına basın ve arama **iSCSI**.
   1. Seçin **iSCSI başlatıcısı ayarlayın**.
   1. Daha önce bağlı StorSimple cihaz bağlantısını kesin. Alternatif olarak, devre dışı dosya sunucusu için birkaç dakika geçebilirsiniz koruma etkinleştirilirken.
      
   > [!NOTE]
   > Bu, dosya paylaşımları, geçici olarak kullanılamamasına neden olur.
   
1. [Sanal makine korumasını etkinleştirmek](../site-recovery/site-recovery-hyper-v-site-to-azure.md) Azure Site Recovery portalından dosya sunucusu VM.
1. İlk eşitleme başladığında, hedef yeniden bağlanabilirsiniz. İSCSI başlatıcısı için StorSimple cihazı seçip tıklayın Git **Connect**.
1. Zaman eşitleme tamamlandıktan ve VM durumu **korumalı**, VM'yi seçin, **yapılandırma** sekmesini tıklatın ve VM ağının güncellemenize (ağı budur, başarısız üzerinden VM bir parçası olacaktır). Ağ gösterilmesini, eşitleme hala gittiğini anlamına gelir.

### <a name="enable-protection-of-storsimple-volumes"></a>StorSimple birimlerini korumasını etkinleştirin
Değil seçtiyseniz **bu birim için varsayılan yedeklemeyi etkinleştir** seçeneği için StorSimple birimlerini, Git **yedekleme ilkelerini** StorSimple Yöneticisi'nde hizmet ve için uygun bir yedekleme ilkesi oluşturma tüm birimler için. Yedekleme sıklığını, uygulama için görmek istediğiniz kurtarma noktası hedefi (RPO) ayarlamanızı öneririz.

### <a name="configure-the-network"></a>Ağı yapılandırma
Böylece yük devretme sonrasında VM ağlarının doğru DR ağa bağlı dosya sunucusu VM için Azure Site Recovery, ağ ayarlarını yapılandırın.

VM'yi seçebileceğiniz **çoğaltılan öğeler** aşağıdaki çizimde gösterildiği gibi ağ ayarlarını yapılandırmak için sekmesinde.

![İşlem ve ağ](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma
Dosya paylaşımlarının yük devretme işlemini otomatikleştirmek ASR'deki bir kurtarma planı oluşturabilirsiniz. Bir kesinti oluşursa, dosya paylaşımları yalnızca tek bir tıklama ile birkaç dakika içinde kutusunu görüntüleyebilirsiniz. Bu Otomasyon etkinleştirmek için bir Azure Otomasyonu hesabı gerekir.

#### <a name="to-create-an-automation-account"></a>Bir Otomasyon hesabı oluşturmak için
1. Azure portalı &gt; **Otomasyon** bölümü.
1. Tıklayın **+ Ekle** düğmesi, aşağıda dikey penceresi açılır.
   
   ![Automation Hesabı ekleme](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)
   
   - Ad - yeni bir Otomasyon hesabı girin
   - Abonelik - abonelik seçin
   - Kaynak grubu - yeni seçin/var olan kaynak grubu oluşturma
   - Konumu - konum seçmek, aynı coğrafi /, depolama hesapları ve StorSimple bulut Gereci oluşturulduğu bölgede tutun.
   - Azure farklı çalıştır hesabı seç oluşturma - **Evet** seçeneği.
   
1. Otomasyon hesabınıza gidin, tıklayın **runbook'ları** &gt; **Galeriye Gözat** için gerekli tüm runbook Otomasyon hesabına aktarın.
1. Aşağıdaki runbook'lar bularak ekleme **olağanüstü durum kurtarma** galerideki etiketi:
   
   - Test yük devretme (TFO) sonra StorSimple birimlerini temizleme
   - Yük devretme StorSimple birim kapsayıcıları
   - Yük devretmeden sonra StorSimple cihazında bağlama birimleri
   - Azure VM'deki özel betik uzantısı'nı kaldırma
   - StorSimple sanal Gereci Başlat
   
      ![Galeriye Gözat](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)
   
1. Tüm betikler Otomasyon hesabında runbook seçerek yayımlama ve tıklayın **Düzenle** &gt; **Yayımla** ardından **Evet** doğrulama iletisi. Bu adımdan sonra **runbook'ları** sekmesi şu şekilde görünür:
   
   ![Runbook'lar](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)
   
1. Otomasyon hesabı'nda tıklatın **değişkenleri** &gt; **değişken Ekle** ve aşağıdaki değişkenleri ekleyin. Bu varlıklar şifrelemeyi seçebilirsiniz. Bu kurtarma planı belirli değişkenlerdir. Kurtarma planı, değişkenlerinizi TestPlan StorSimRegKey, TestPlan-AzureSubscriptionName olması ve benzeri sonraki adımda oluşturacağınız TestPlan, adıdır.

   - **BaseUrl**: Azure bulut Kaynak Yöneticisi URL'si. Kullanarak başlayın **Get-AzEnvironment | Select-Object Name, ResourceManagerUrl** cmdlet'i.
   - _RecoveryPlanName_ **- ResourceGroupName**: Resource Manager Grup StorSimple kaynak vardır.
   - _RecoveryPlanName_ **- ManagerName**: StorSimple cihazı olan StorSimple kaynağıdır.
   - _RecoveryPlanName_ **- DeviceName**: Devredilecek olan StorSimple cihaz.
   - _RecoveryPlanName_ **- DeviceIpAddress**: Cihazın IP adresi (Bu bulunabilir **cihazları** sekmesini StorSimple cihaz Yöneticisi bölümünde &gt; **ayarları** &gt; **ağ** &gt; **DNS ayarlarını** grup).
   - _RecoveryPlanName_ **- VolumeContainers**: Virgülle ayrılmış bir dize birim kapsayıcılarının yük devretmesi için gereken cihaz üzerinde mevcut; Örneğin: volcon1, volcon2, volcon3.
   - _RecoveryPlanName_ **- TargetDeviceName**: Üzerinde başarısız olan kapsayıcılar StorSimple Cloud Appliance.
   - _RecoveryPlanName_ **-TargetDeviceIpAddress**: Hedef cihazın IP adresi (Bu bulunabilir **sanal makine** bölümü &gt; **ayarları** grubu &gt; **ağ** sekmesi).
   - _RecoveryPlanName_ **- StorageAccountName**: (Bu VM üzerinde başarısız üzerinde çalıştırılacak olan) betik depolanacağı depolama hesabı adı. Bu betik geçici olarak depolamak için bazı alana sahip herhangi bir depolama hesabı olabilir.
   - _RecoveryPlanName_ **- StorageAccountKey**: Yukarıdaki depolama hesabı için erişim anahtarı.
   - _RecoveryPlanName_ **- VMGUIDS**: Bir VM koruma sırasında Azure Site Recovery VM üzerinde başarısız ayrıntılarını sağlayan benzersiz bir kimliği her VM atar. VMGUID elde etmek için seçin **kurtarma Hizmetleri** sekmesine **korumalı öğesi** &gt; **koruma grupları** &gt;  **Makineleri** &gt; **özellikleri**. Birden fazla VM varsa, GUID'leri virgülle ayrılmış bir dize olarak ekleyin.

     Kurtarma planı fileServerpredayRP, adıdır, örneğin, ardından, **değişkenleri**, **bağlantıları** ve **sertifikaları** sekmesi, ekledikten sonra aşağıdaki gibi görünmelidir Tüm varlıklar için.

      ![Varlıklar](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

1. StorSimple 8000 serisi Runbook modülü Otomasyon hesabınızdaki karşıya yükleyin. Kullanım bir modül eklemek için aşağıdaki adımları:
   
   1. PowerShell'i açın, yeni bir klasör oluşturun ve dizini klasörü olarak değiştirin.
      
      ```
            mkdir C:\scripts\StorSimpleSDKTools
            cd C:\scripts\StorSimpleSDKTools
      ```
   1. Adım1 aynı klasörde altında nuget CLI indirin.
      Nuget.exe çeşitli sürümleri mevcuttur [nuget indirir](https://www.nuget.org/downloads). Her indirme bağlantısı doğrudan bir .exe dosyasına işaret eder. Bu nedenle sağ tıklayın ve dosyayı bilgisayarınıza kaydedin emin olun yerine tarayıcıda çalışan.
      
      ```
            wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
      ```
      
   1. Bağımlı SDK'sını indirin
      
      ```
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
            C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
      ```
      
   1. StorSimple 8000 serisi cihaz yönetimi için bir Azure Otomasyonu Runbook modülünü oluşturun. Kullanım aşağıdaki komutları bir Otomasyon modül zip dosyası oluşturmak için.
         
      ```powershell
            # set path variables
            $downloadDir = "C:\scripts\StorSimpleSDKTools"
            $moduleDir = "$downloadDir\AutomationModule\Microsoft.Azure.Management.StorSimple8000Series"

            #don't change the folder name "Microsoft.Azure.Management.StorSimple8000Series"
            mkdir "$moduleDir"

            copy "$downloadDir\Microsoft.IdentityModel.Clients.ActiveDirectory.2.28.3\lib\net45\Microsoft.IdentityModel.Clients.ActiveDirectory*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.3.3.7\lib\net452\Microsoft.Rest.ClientRuntime.Azure*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.2.3.8\lib\net452\Microsoft.Rest.ClientRuntime*.dll" $moduleDir
            copy "$downloadDir\Newtonsoft.Json.6.0.8\lib\net45\Newtonsoft.Json*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Rest.ClientRuntime.Azure.Authentication.2.2.9-preview\lib\net45\Microsoft.Rest.ClientRuntime.Azure.Authentication*.dll" $moduleDir
            copy "$downloadDir\Microsoft.Azure.Management.Storsimple8000series.1.0.0\lib\net452\Microsoft.Azure.Management.Storsimple8000series*.dll" $moduleDir

            #Don't change the name of the Archive
            compress-Archive -Path "$moduleDir" -DestinationPath Microsoft.Azure.Management.StorSimple8000Series.zip
      ```
         
   1. Yukarıda oluşturulan Azure Otomasyonu modül zip dosyası (Microsoft.Azure.Management.StorSimple8000Series.zip) içeri aktarın. Bu Otomasyon hesabı seçerek yapılabilir, tıklayın **modülleri** paylaşılan kaynaklar ve ardından altındaki **bir modül ekleyecek**.
   
   StorSimple 8000 serisi modülü içeri aktardıktan sonra **modülleri** sekmesinde aşağıdaki gibi görünmelidir:
   
      ![Modüller](./media/storsimple-disaster-recovery-using-azure-site-recovery/image12.png)

1. Git **kurtarma Hizmetleri** bölümünde ve daha önce oluşturduğunuz Azure Site Recovery kasasını seçin.
1. Seçin **kurtarma planları (Site Recovery)** seçeneğini **Yönet** gruplandırın ve yeni bir kurtarma planı gibi oluşturun:
   
   - Tıklayın **+ kurtarma planı** düğmesi, aşağıda dikey penceresi açılır.
      
      ![Kurtarma planı oluşturma](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)
      
   - Kurtarma planı adı girin, kaynak, hedef & dağıtım modeli değerlerini seçin.
   
   - Koruma grubundan tıklayın ve kurtarma planına dahil istediğiniz Vm'leri seçin **Tamam** düğmesi.
   
   - Daha önce oluşturduğunuz kurtarma planı seçin, **Özelleştir** kurtarma planı özelleştirme görünümü açmak için düğmeyi.
   
   - Sağ tıklayın **tüm gruplar kapalı** tıklatıp **ön Eylem Ekle**.
   
   - Açılır Ekle eylemini dikey penceresinde, bir ad girin, seçin **birincil taraf** çalıştırdığınız, Otomasyon hesabı (hangi eklediğiniz runbook'ları) seçin ve ardından nerede seçeneğinde **yük devretme StorSimple birim kapsayıcıları**  runbook.
   
   - Sağ tıklayın **Grup 1: Başlangıç** tıklatıp **Ekle korumalı öğeler** seçeneği ardından'a tıklayın ve kurtarma planı korunacak olan Vm'leri seçin **Tamam** düğmesi. VM'ler zaten ise isteğe bağlı, seçili.
   
   - Sağ tıklayın **Grup 1: Başlangıç** tıklatıp **sonrası eylem** seçeneğini sonra aşağıdaki betikler ekleyin:  
      
      - Start-StorSimple-sanal-Gereci runbook  
      - StorSimple birim kapsayıcıları üzerinde runbook başarısız oldu  
      - Bağlama birimleri sonra devretme runbook  
      - Kaldırma-özel-betik-uzantısı runbook  
        
   - Aynı yukarıdaki 4 betikleri sonra el ile gerçekleştirilen bir eylem ekleme **Grup 1: Sonraki adımlar** bölümü. Bu eylem, her şeyin düzgün çalıştığını doğrulayabilirsiniz noktasıdır. Bu eylem yalnızca yük devretme testi bir parçası olarak eklenmesi gerekir (Bu nedenle yalnızca seçilen **yük devretme testi** onay kutusu).
    
   - El ile gerçekleştirilen eylem sonra Ekle **Temizleme** diğer runbook'lar için kullandığınız yordamın aynısını kullanarak betik. **Kaydet** kurtarma planı.
    
   > [!NOTE]
   > El ile gerçekleştirilen eylem tamamlandıktan sonra hedef cihazda kopyalanan StorSimple birimlerini temizleme kapsamında silinecek olduğundan bir yük devretme testi çalıştırırken, her şeye el ile gerçekleştirilen eylem adım doğrulamanız gerekir.
       
      ![Kurtarma planı](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Yük devretme testi gerçekleştirme
Başvurmak [Active Directory DR çözümü](../site-recovery/site-recovery-active-directory.md) konular için belirli Active Directory için yük devretme testi sırasında yardımcı kılavuz. Şirket içi Kurulumu test yük devretme gerçekleştiğinde tüm dağıtılmış bir dosya değil. Azure'da StorSimple Cloud Appliance için şirket içi sanal Makineye bağlı StorSimple birimlerini klonlanır. Azure'da bir VM test amacıyla sağlanmıştır ve kopyalanan birimler sanal Makineye eklenir.

#### <a name="to-perform-the-test-failover"></a>Sınama yük devretmesi gerçekleştirme
1. Azure portalında Site Recovery kasanızı seçin.
1. Dosya sunucusu VM için oluşturulan kurtarma planına tıklayın.
1. Tıklayın **yük devretme testi**.
1. Yük devretme gerçekleştikten sonra Azure Vm'lerinin bağlanması Azure sanal ağı seçin.
   
   ![Yük devretmeyi Başlat](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
   
1. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Özelliklerini açmak için VM üzerinde veya tıklayarak ilerlemeyi izleyebilirsiniz **Test yük devretme işi** kasa adı &gt; **işleri** &gt; **Site Recovery işleri**.
1. Yük devretme işlemi tamamlandıktan sonra da çoğaltılan görüyor olmalısınız makine, Azure portalında görünen &gt; **sanal makineler**. Doğrulamaları gerçekleştirebilirsiniz.
1. Doğrulamaları bitirdikten sonra tıklayın **doğrulamaları tam**. Bu, StorSimple birimlerini kaldırmak ve StorSimple Cloud Appliance'ı kapatın.
1. İşiniz bitince **yük devretme testini Temizle** kurtarma planında. Notları yük devretme testiyle ilişkili gözlemlerinizi kaydetmek ve. Bu, yük devretme testi sırasında oluşturulan sanal makineyi siler.

## <a name="perform-a-planned-failover"></a>Planlı bir yük devretme gerçekleştirme
   Planlı yük devretme sırasında şirket içi dosya sunucusu VM düzgün bir şekilde kapatılır ve Bulutu StorSimple cihazında birim yedekleme anlık görüntüsü alınır. StorSimple birimlerini sanal cihaza yük devredildi, Azure'da çoğaltma VM'si getirildikten ve birimler sanal Makineye eklenir.

#### <a name="to-perform-a-planned-failover"></a>Planlı bir yük devretme gerçekleştirme
1. Azure portalında **kurtarma Hizmetleri** kasası &gt; **kurtarma planları (Site Recovery)** &gt; **recoveryplan_name** için oluşturuldu dosya sunucusu VM.
1. Kurtarma planı dikey penceresinde tıklayın **daha fazla** &gt; **planlı yük devretme**.

   ![Kurtarma planı](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
1. Üzerinde **planlı yük devretmeyi Onayla** dikey penceresinde, kaynak ve hedef konumları ve select hedef ağ seçin ve yük devretme işlemini başlatmak için ✓ onay simgesine tıklayın.
1. Çoğaltma sanal makinelerini oluşturulduktan sonra yürütme bekleme durumuna demektir. Tıklayın **işleme** için yük devretmeyi yürütürsünüz.
1. Çoğaltma tamamlandıktan sonra sanal makineleri ikincil konumda Başlat.

## <a name="perform-a-failover"></a>Bir yük devretme gerçekleştirme
Planlanmamış bir yük devretme sırasında StorSimple birimlerini sanal cihaza yük devredildi, çoğaltma VM'si Azure'da kapsama dahil edilecektir ve birimler sanal Makineye eklenir.

#### <a name="to-perform-a-failover"></a>Bir yük devretme gerçekleştirmek için
1. Azure portalında **kurtarma Hizmetleri** kasası &gt; **kurtarma planları (Site Recovery)** &gt; **recoveryplan_name** için oluşturuldu dosya sunucusu VM.
1. Kurtarma planı dikey penceresinde tıklayın **daha fazla** &gt; **yük devretme**.
1. Üzerinde **yük devretmeyi Onayla** dikey penceresinde kaynağını seçin ve hedef konumları.
1. Seçin **sanal makineleri kapatmak ve en son verileri eşitleyin** Site Recovery korumalı sanal makineyi kapatır ve verileri eşitleyerek veri en son sürümünü yük devretme denemesi gerektiğini belirtmek için.
1. Yük devretmeden sonra yürütme bekleme durumuna sanal makineler yer. Tıklayın **işleme** için yük devretmeyi yürütürsünüz.


## <a name="perform-a-failback"></a>Yeniden çalışma gerçekleştirme
Bir yedekleme yapıldıktan sonra bir yeniden çalışma sırasında StorSimple birim kapsayıcıları fiziksel cihaza yük devredildi.

#### <a name="to-perform-a-failback"></a>Bir yeniden çalışma gerçekleştirme
1. Azure portalında **kurtarma Hizmetleri** kasası &gt; **kurtarma planları (Site Recovery)** &gt; **recoveryplan_name** için oluşturuldu dosya sunucusu VM.
1. Kurtarma planı dikey penceresinde tıklayın **daha fazla** &gt; **planlı yük devretme**.
1. Kaynak ve hedef konumların seçin, VM oluşturma seçenekleri ve uygun veri eşitlemeyi seçin.
1. Tıklayın **Tamam** yeniden çalışma işlemini başlatmak için düğme.
   
   ![Yeniden Başlat](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>En İyi Uygulamalar
### <a name="capacity-planning-and-readiness-assessment"></a>Kapasite planlama ve hazır olma durumu değerlendirmesi
#### <a name="hyper-v-site"></a>Hyper-V sitesi
Kullanım [kullanıcı kapasite Planlayıcısı aracını](https://www.microsoft.com/download/details.aspx?id=39057) sunucu, depolama ve Hyper-V çoğaltma ortamınız için ağ altyapısını tasarlama.

#### <a name="azure"></a>Azure
Çalıştırabileceğiniz [Azure sanal makine hazır olma durumu değerlendirmesi aracı](https://azure.microsoft.com/downloads/vm-readiness-assessment/) vm'lerinde Azure Vm'leri ve Azure Site kurtarma Hizmetleri ile uyumlu olduklarından emin olun. Hazır olma durumu değerlendirmesi aracı, VM yapılandırmaları denetler ve yapılandırmaları Azure ile uyumsuz olduğunda uyarır. Örneğin, C: sürücüsü 127 GB'den büyük olması durumunda bir uyarı verir.

Kapasite planlama en az iki önemli işlemden oluşur:

   - Eşleme Hyper-V Vm'lerini Azure VM boyutlarına (örneğin, A6 ve A7, A8 ve A9) şirket içi.
   - Gerekli Internet bant genişliği belirleniyor.

## <a name="limitations"></a>Sınırlamalar
- Şu anda yalnızca 1 StorSimple cihazı yerine (tek StorSimple Cloud Appliance için) çalışılabilir. Senaryo birden çok StorSimple cihazını yayılan bir dosya sunucusu henüz desteklenmiyor.
- Bir sanal makine için koruma etkinleştirilirken bir hata alırsanız, iSCSI hedefleri kesildikten emin olun.
- Birim kapsayıcıları yayılan yedekleme ilkeleri nedeniyle birlikte gruplandırılmış tüm birim kapsayıcıları üzerinden araya getirilir.
- Seçtiğiniz birim kapsayıcılarından tüm birimler yük devretme.
- Tek bir StorSimple Cloud Appliance en fazla kapasitesi 64 TB'a kadar çünkü 64 TB'a kadar fazla birimleri üzerinden devredilemez.
- Planlanan/planlanmamış yük devretme başarısız olur ve Azure Vm'leri oluşturulur, ardından VM'ler temizlemeyin. Bunun yerine, bir yeniden çalışma yapın. Vm'leri silerseniz daha sonra şirket içi Vm'leri yeniden açılamaz.
- Bir yük devretme sonrasında birimleri görmek mümkün değilse, Vm'lere gidin, Disk Yönetimi'ni açın, diskleri yeniden tarayın ve ardından bunları çevrimiçi duruma getirmeden.
- Bazı durumlarda, DR sitesi sürücü harflerini harf şirket içi farklı olabilir. Bu meydana gelirse, yük devretme işlemi tamamlandıktan sonra sorun el ile düzeltmeniz gerekir.
- Yük devretme işi zaman aşımı: StorSimple betik zaman aşımı birim kapsayıcıları için yük devretme betiği (şu anda 120 dakika) başına Azure Site Recovery sınırından daha uzun sürerse olur.
- Yedekleme işi zaman aşımı: Birimlerin yedeğini betik (şu anda 120 dakika) başına Azure Site Recovery sınırından daha uzun sürerse StorSimple betik zaman aşımına uğradı.
   
  > [!IMPORTANT]
  > Yedekleme Azure portalından el ile çalıştırın ve ardından kurtarma planını yeniden çalıştırın.
   
- Kopyalama işi zaman aşımı: Birimleri kopyalama komut dosyası (şu anda 120 dakika) başına Azure Site Recovery sınırından daha uzun sürerse StorSimple betik zaman aşımına uğradı.
- Zaman eşitleme hatası: StorSimple yedekleme portalda başarılı olsa bile, yedeklemeler başarısız belirten kullanıma hataları komutlar. Bunun olası bir nedeni, StorSimple gereç saat saat dilimindeki geçerli saati ile eşitlenmemiş olabilir olabilir.
   
  > [!IMPORTANT]
  > Gereç saat saat dilimindeki geçerli saati ile eşitleyin.
   
- Gereç yük devretme hatası: Varsa bir gereç yük devretme kurtarma planı çalışırken StorSimple betiği başarısız olabilir.
   
  > [!IMPORTANT]
  > Kurtarma planı, gereç yük devretme işlemi tamamlandıktan sonra yeniden çalıştırın.


## <a name="summary"></a>Özet
Azure Site RECOVERY'yi kullanarak, bir dosya sunucusu VM için bir tam otomatik bir olağanüstü durum kurtarma planı oluşturabilirsiniz StorSimple depolama alanında barındırılan dosya paylaşımlarına sahip. Her yerden saniyeler içinde yük devretmeyi başlatın kesintisi ve get uygulamayı birkaç dakika içinde çalışır durumda olması durumunda.
