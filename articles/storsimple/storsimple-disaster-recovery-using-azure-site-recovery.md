---
title: StorSimple fileshare Azure Site Recovery ile DR otomatikleştirmek | Microsoft Docs
description: Adımları ve Microsoft Azure StorSimple depolama alanında barındırılan dosya paylaşımları için olağanüstü durum kurtarma çözümü oluşturmak için en iyi yöntemleri açıklar.
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
ms.openlocfilehash: e60cc83f49f9e0d0f878d7f49333f1be34ce54a6
ms.sourcegitcommit: 9ae92168678610f97ed466206063ec658261b195
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2017
ms.locfileid: "23890883"
---
# <a name="automated-disaster-recovery-solution-using-azure-site-recovery-for-file-shares-hosted-on-storsimple"></a>StorSimple üzerinde barındırılan dosya paylaşımları için Azure Site RECOVERY'yi kullanarak otomatikleştirilmiş olağanüstü durum kurtarma çözümü
## <a name="overview"></a>Genel Bakış
Microsoft Azure StorSimple yaygın olarak dosya paylaşımları ile ilgili yapılandırılmamış veriler karmaşıklığını adresleri bir karma bulut depolama çözümüdür. StorSimple bulut depolama şirket içi depolama ve bulut depolama üzerinden şirket içi çözüm ve otomatik olarak katmanları verilerinin bir uzantısı olarak kullanır. Veri koruma, yerel ile tümleşik ve bulut anlık görüntüleri, sprawling bir depolama altyapısını ortadan kaldırır.

[Azure Site Recovery](../site-recovery/site-recovery-overview.md) çoğaltma, yük devretme ve kurtarma sanal makinelerin düzenleyerek olağanüstü durum kurtarma (DR) özellikleri sağlayan bir Azure tabanlı bir hizmettir. Azure Site Recovery, tutarlı bir şekilde çoğaltmak için korumak ve sorunsuz bir şekilde sanal makineler ve özel/ortak veya barındırılan bulut uygulamaları yük devri çoğaltma teknolojilerini destekler.

Azure Site Recovery, sanal makine çoğaltma ve StorSimple bulut anlık görüntü yetenekleri kullanarak tam dosya sunucu ortamı koruyabilirsiniz. Bir kesinti durumunda, dosya paylaşımları çevrimiçi Azure'da yalnızca birkaç dakika içinde getirmek için tek bir tıklamayla kullanabilirsiniz.

Bu belge, nasıl bir olağanüstü durum kurtarma çözümü StorSimple depolama alanında barındırılan, dosya paylaşımları oluşturabilir ve planlanmış, Planlanmayan gerçekleştirmek ve test yük devretmeleri tek tıklatmayla kurtarma planı kullanarak ayrıntılı olarak açıklanmaktadır. Esas olarak, nasıl Kurtarma planlaması StorSimple yük devretme işlemleri sırasında olağanüstü durum senaryoları etkinleştirmek için Azure Site Recovery kasasına değiştirebileceğiniz gösterir. Ayrıca, desteklenen yapılandırmalar ve önkoşulları açıklanmaktadır. Bu belge, Azure Site Recovery ve StorSimple mimarilerinin temel kavramları hakkında bilgi sahibi olduğunuzu varsayar.

## <a name="supported-azure-site-recovery-deployment-options"></a>Desteklenen Azure Site Recovery dağıtım seçenekleri
Müşteriler fiziksel sunucularda veya Hyper-V veya VMware üzerinde çalışan sanal makineler (VM'ler) olarak dosya sunucuları dağıtmak ve StorSimple depolama dışında yontulmuş birimlerden dosya paylaşımları oluşturun. Azure Site Recovery hem fiziksel hem de sanal dağıtımları ikincil bir siteye veya Azure'a koruyabilirsiniz. Bu belge VM Hyper-V üzerinde barındırılan bir dosya sunucusu için kurtarma sitesi olarak Azure ile StorSimple depolama dosya paylaşımlarında DR çözüm ayrıntılarını içerir. Dosya sunucusu VM VMware VM veya fiziksel makinesi üzerinde olduğu diğer senaryolarda benzer şekilde uygulanabilir.

## <a name="prerequisites"></a>Ön koşullar
StorSimple depolama alanında barındırılan dosya paylaşımları için Azure Site Recovery kullanan bir tek tıklamayla olağanüstü durum kurtarma çözümü uygulamaya Önkoşullar şunlardır:

* Şirket içi Hyper-V veya VMware veya fiziksel bir makine üzerinde VM barındırılan Windows Server 2012 R2 dosya sunucusu
* StorSimple depolama aygıtı şirket içi Azure StorSimple Yöneticisi ile kayıtlı
* Azure StorSimple Yöneticisi'nde oluşturulan StorSimple bulut uygulaması. Bir kapatma durumda Gereci tutulabilir.
* StorSimple depolama aygıtında yapılandırılmış birimler üzerinde barındırılan dosya paylaşımları
* [Azure Site kurtarma Hizmetleri kasası](../site-recovery/site-recovery-vmm-to-vmm.md) Microsoft Azure aboneliği için oluşturulan

Ayrıca, Azure kurtarma siteniz olduğunda, çalıştırmak [Azure sanal makine hazırlık Değerlendirme Aracı](http://azure.microsoft.com/downloads/vm-readiness-assessment/) vm'lerde Azure Vm'leri ve Azure Site Recovery ile uyumlu olduklarından emin olmak için hizmetleri.

Gecikme süresi (da maliyetin neden olabilir) sorunlar StorSimple bulut uygulaması, automation hesabı ve depolama oluşturmak emin olun önlemek için aynı bölgede hesabı.

## <a name="enable-dr-for-storsimple-file-shares"></a>StorSimple dosya paylaşımları için DR etkinleştir
Şirket içi ortamının her bileşenin tam çoğaltma ve kurtarma etkinleştirmek için korunması gerekir. Bu bölümde açıklanmıştır nasıl yapılır:

* Active Directory ve DNS çoğaltmayı ayarlama (isteğe bağlı) ayarlayın
* Dosya sunucusu VM korumasını etkinleştirmek için Azure Site Recovery kullanın
* StorSimple birimleri korumayı etkinleştir
* Ağı yapılandırma

### <a name="set-up-active-directory-and-dns-replication-optional"></a>Active Directory ve DNS çoğaltmayı ayarlama (isteğe bağlı) ayarlayın
DR sitede kullanılabilir olacak şekilde, Active Directory ve DNS çalıştıran makineleri korumak istiyorsanız, açıkça onları korumak (dosya sunucuları kimlik doğrulaması ile yük devretme sonrasında erişilebilir; böylece) gerekir. Bir müşterinin şirket içi ortamına karmaşıklığı iki önerilen seçenek vardır.

#### <a name="option-1"></a>seçenek 1
Müşterinin az sayıda uygulamayı varsa, tüm tek etki alanı denetleyicisi şirket içi site ve etki alanı denetleyicisi makinesi (Bu siteden siteye ve site Azure için geçerlidir) ikincil bir siteye çoğaltmak için Azure Site Recovery çoğaltma kullanmanızı öneririz sonra tüm site başarısız.

#### <a name="option-2"></a>Seçenek 2
Müşteri çok sayıda uygulama olduğundan, bir Active Directory ormanı çalıştıran ve birkaç uygulamalar üzerinde aynı anda başarısız durumunda DR sitesi ek etki alanı denetleyicisinde ayarlama öneririz (ikincil bir site veya Azure).

Başvurmak [Active Directory ve Azure Site Recovery kullanarak DNS için otomatik DR çözüm](../site-recovery/site-recovery-active-directory.md) bir etki alanı denetleyicisi DR sitesi üzerindeki yaparken, yönergeler için. Bu belgenin geri kalanı için bir etki alanı denetleyicisi DR sitesi üzerindeki kullanılabilir varsayalım.

### <a name="use-azure-site-recovery-to-enable-protection-of-the-file-server-vm"></a>Dosya sunucusu VM korumasını etkinleştirmek için Azure Site Recovery kullanın
Bu adım, şirket içi dosya sunucusu ortamınızı hazırlayın gerektirir, oluşturma ve bir Azure Site Recovery kasası hazırlamak ve VM dosya korumayı etkinleştirin.

#### <a name="to-prepare-the-on-premises-file-server-environment"></a>Şirket içi dosya sunucusu ortamı hazırlamak için
1. Ayarlama **kullanıcı hesabı denetimi** için **hiçbir zaman uyarma**. Böylece Azure Site Recovery tarafından yük devretme sonrasında iSCSI hedefleri bağlanmak için Azure Otomasyon betikleri kullanabilirsiniz, bu gereklidir.

   1. Windows tuşu + Q tuşlarına basın ve arama **UAC**.
   2. Seçin **değişiklik kullanıcı hesabı denetimi ayarlarını**.
   3. Yukarıdan aşağıya doğru çubuğu sürükleyin **hiçbir zaman uyarma**.
   4. Tıklatın **Tamam** ve ardından **Evet** istendiğinde.

      ![Kullanıcı Hesabı Denetimi Ayarları](./media/storsimple-disaster-recovery-using-azure-site-recovery/image1.png)
2. VM Aracısı her dosya sunucusu sanal makineleri yükleyin. Bu, size Azure Otomasyon betikleri başarısız üzerinde VM'ler çalıştırabilmeniz için gereklidir.

   1. [Aracısı'nı indirme](http://aka.ms/vmagentwin) için `C:\\Users\\<username>\\Downloads`.
   2. Yönetici modunda (yönetici olarak çalıştır) Windows PowerShell'i açın ve ardından indirme konumuna gitmek için aşağıdaki komutu girin:

      `cd C:\\Users\\<username>\\Downloads\\WindowsAzureVmAgent.2.6.1198.718.rd\_art\_stable.150415-1739.fre.msi`

      > [!NOTE]
      > Dosya adı, sürüm bağlı olarak değişebilir.
      >
      >
3. **İleri**’ye tıklayın.
4. Kabul **sözleşmesi koşulları** ve ardından **sonraki**.
5. **Son**'a tıklayın.
6. StorSimple depolama dışında yontulmuş birimleri kullanarak dosya paylaşımları oluşturun. Daha fazla bilgi için bkz: [birimleri yönetmek için StorSimple Yöneticisi hizmetini kullanma](storsimple-manage-volumes.md).

   1. Şirket içi Vm'leriniz Windows tuşu + Q tuşlarına basın ve arama **iSCSI**.
   2. Seçin **iSCSI başlatıcısı**.
   3. Seçin **yapılandırma** sekmesinde ve Başlatıcı adı kopyalayın.
   4. [Azure Portal](https://portal.azure.com/)’da oturum açın.
   5. Seçin **StorSimple** sekmesini ve ardından fiziksel cihazı içeren StorSimple Yöneticisi hizmeti seçin.
   6. Birim kapsayıcısı/kapsayıcıları ve ardından birim oluşturun. (Bu birimleri VM'ler dosya sunucusundaki dosya paylaşımlar adı verilir). Başlatıcı adı kopyalayın ve birimleri oluşturduğunuzda, erişim denetimi kayıtları için uygun bir ad verin.
   7. Seçin **yapılandırma** sekmesi ve Not aşağı aygıtın IP adresi.
   8. Şirket içi Vm'leriniz Git **iSCSI başlatıcısı** yeniden ve Hızlı Bağlan bölümünde IP girin. Tıklatın **Hızlı Bağlan** (aygıt artık bağlı olması).
   9. Azure portal ve select açmak **birimleri ve aygıtları** sekmesi. Tıklatın **otomatik yapılandır**. Oluşturduğunuz toplu görüntülenmesi gerekir.
   10. Portalı'nda seçin **aygıtları** sekmesini ve ardından **yeni bir sanal cihaz oluşturma.** (Bir yük devretme gerçekleşirse bu sanal aygıt kullanılır). Bu yeni sanal aygıt ek maliyetlerden kaçınmak için çevrimdışı bir durumda tutulabilir. Sanal cihaz çevrimdışı duruma getirmek için Git **sanal makineleri** bölümünde portalda ve bilgisayarı kapat.
   11. Şirket içi sanal makineleri için geri dönün ve Disk Yönetimi'ni açın (Windows tuşu + X tuşlarına basın ve seçin **Disk Yönetimi**).
   12. Bazı ek diskleri (bağlı olarak, oluşturduğunuz birimlerin sayısı) fark edeceksiniz. Önce bir select sağ **diski başlatma**seçip **Tamam**. Sağ **Unallocated** bölümünde, select **yeni basit birim**bir sürücü harfi atamak ve Sihirbazı tamamlayın.
   13. L tüm diskler için tekrarlayın. Şimdi tüm diskler üzerinde gördüğünüz **bu PC** Windows Gezgini'nde.
   14. Bu birimlerde dosya paylaşımları oluşturmak için dosya ve Depolama Hizmetleri rolünü kullanın.

#### <a name="to-create-and-prepare-an-azure-site-recovery-vault"></a>Oluşturma ve bir Azure Site Recovery kasası hazırlamak için
Başvurmak [Azure Site Recovery belgelerine](../site-recovery/site-recovery-hyper-v-site-to-azure.md) dosya sunucusu VM korumadan önce Azure Site Recovery ile çalışmaya başlamak için.

#### <a name="to-enable-protection"></a>Korumayı etkinleştirmek için
1. İSCSI hedeflere Azure Site Recovery ile korumak istediğiniz şirket içi sanal makineleri bağlantısını kesin:

   1. Windows tuşu + Q tuşlarına basın ve arama **iSCSI**.
   2. Seçin **iSCSI başlatıcısı ayarlayın**.
   3. Daha önce bağlanmış StorSimple cihazı bağlantısını kesin. Alternatif olarak, devre dışı dosya sunucusu için bir kaç dakika geçebilirsiniz koruma etkinleştirilirken.

   > [!NOTE]
   > Bu dosya paylaşımlarını geçici olarak kullanılamaz hale gelmesine neden olur.
   >
   >
2. [Sanal makine korumayı etkinleştirin](../site-recovery/site-recovery-hyper-v-site-to-azure.md) Azure Site kurtarma portalından dosya sunucusunun VM.
3. İlk eşitleme işlemi başladığında, hedef yeniden bağlanabilirsiniz. İSCSI başlatıcısı için StorSimple cihazı seçin ve tıklayın Git **Bağlan**.
4. Zaman eşitleme tamamlandıktan ve VM durumu **korumalı**, VM seçin, **yapılandırma** sekmesini tıklatın ve VM ağının güncellemenize (devredilen sanal makine bir parçası olacak ağ budur). Ağ gösterilmesini, eşitleme hala işaretleneceğini anlamına gelir.

### <a name="enable-protection-of-storsimple-volumes"></a>StorSimple birimleri korumayı etkinleştir
Değil seçtiyseniz **bu birim için varsayılan yedeklemeyi etkinleştir** seçeneği için StorSimple birimlerini, Git **yedekleme ilkeleri** StorSimple Yöneticisi'nde hizmet ve tüm birimler için uygun bir yedekleme ilkesi oluşturun. Uygulama için görmek istediğiniz kurtarma noktası hedefi (RPO) için yedekleme sıklığını ayarlamanızı öneririz.

### <a name="configure-the-network"></a>Ağı yapılandırma
VM ağları yük devretme sonrasında doğru DR ağa bağlı böylece dosya sunucusu VM için Azure Site Recovery ağ ayarlarını yapılandırın.

VM'yi seçebileceğiniz **öğeleri çoğaltılan** sekmesinde aşağıdaki çizimde gösterildiği gibi ağ ayarlarını yapılandırmak için.

![İşlem ve ağ](./media/storsimple-disaster-recovery-using-azure-site-recovery/image2.png)

## <a name="create-a-recovery-plan"></a>Kurtarma planı oluşturma
ASR dosya paylaşımlarının yük devretme işlemini otomatikleştirmek için bir kurtarma planı oluşturabilirsiniz. Bir kesinti oluşursa, dosya paylaşımları yalnızca tek bir tıklatmayla birkaç dakika içinde getirebilir. Bu Otomasyon etkinleştirmek için Azure automation hesabı gerekir.

#### <a name="to-create-an-automation-account"></a>Bir Otomasyon hesabı oluşturmak için
1. Azure portalına gidin &gt; **Otomasyon** bölümü.
2. Tıklatın **+ Ekle** düğmesi, aşağıda dikey penceresi açılır.

   ![Automation Hesabı ekleme](./media/storsimple-disaster-recovery-using-azure-site-recovery/image11.png)

   * Name - yeni bir Otomasyon hesabı girin
   * Abonelik - abonelik seçin
   * Kaynak Grup - yeni seçin/kaynak grubu mevcut oluşturma
   * Konumu - konum seçin, aynı coğrafi/içinde depolama hesapları ve StorSimple bulut uygulaması oluşturulduğu bölgede tutun.
   * Azure farklı çalıştır hesabı - create Select **Evet** seçeneği.

3. Otomasyon hesabı gidin,'ı tıklatın **Runbook'lar** &gt; **Gözat galeri** gerekli olan tüm runbook'lardan Otomasyon dikkate alınacak.
4. Aşağıdaki runbook'lar bularak eklemek **olağanüstü durum kurtarma** galerisinde etiketi:

   * Test yük devretme (TFO sonra) StorSimple birimlerini temizleme
   * Yük devretme StorSimple birim kapsayıcıları
   * Birimler yük devretme sonrasında StorSimple cihazında bağlama
   * Azure VM'deki özel betik uzantısı kaldırma
   * StorSimple sanal gereç Başlat

     ![Gözat Galerisi](./media/storsimple-disaster-recovery-using-azure-site-recovery/image3.png)

5. Otomasyon hesabında runbook seçerek tüm betikler yayımlamak ve tıklatın **Düzenle** &gt; **Yayımla** ve ardından **Evet** doğrulama iletisi. Bu adımdan sonra **Runbook'lar** sekmesinde aşağıdaki gibi görünür:

    ![Runbook'lar](./media/storsimple-disaster-recovery-using-azure-site-recovery/image4.png)

6. Otomasyon hesabı'nda tıklatın **değişkenleri** &gt; **değişken Ekle** ve aşağıdaki değişkenleri ekleyin. Bu varlıklar şifrelemeyi seçebilirsiniz. Bu kurtarma planı belirli değişkenlerdir. Kurtarma planı, değişkenlerinizi TestPlan StorSimRegKey, TestPlan-AzureSubscriptionName olması ve benzeri sonra sonraki adımda oluşturacağınız TestPlan, adıdır.

   * **BaseUrl**: Azure bulut için Resource Manager url. Kullanarak **Get-AzureRmEnvironment | Select-Object adı, ResourceManagerUrl** cmdlet'i.
   * *RecoveryPlanName***- ResourceGroupName**: StorSimple kaynak Resource Manager grup.
   * *RecoveryPlanName***- ManagerName**: StorSimple cihazı sahip StorSimple kaynak.
   * *RecoveryPlanName***- DeviceName**: yük devretmenin sahip StorSimple cihazı.
   * *RecoveryPlanName***- DeviceIpAddress**: aygıtın IP adresi (Bu bulunabilir **aygıtları** StorSimple Aygıt Yöneticisi'ni bölüm sekmesinde &gt; **Ayarları** &gt; **ağ** &gt; **DNS ayarlarını** grup).
   * *RecoveryPlanName***- VolumeContainers**: birim kapsayıcıları üzerinde; örneğin başarısız için gereken aygıtta virgülle ayrılmış dizesi: volcon1, volcon2, volcon3.
   * *RecoveryPlanName***- TargetDeviceName**: StorSimple bulut uygulaması kapsayıcıları olduğu yükü devredilecek.
   * *RecoveryPlanName***- TargetDeviceIpAddress**: Hedef aygıtın IP adresi (Bu bulunabilir **sanal makine** bölüm &gt; **ayarları**  grup &gt; **ağ** sekmesi).
   * *RecoveryPlanName***- StorageAccountName**: (olan başarısız üzerinde VM üzerinde çalıştırmak için) komut dosyası depolanacağı depolama hesabı adı. Bu komut dosyası geçici olarak depolamak için bazı alanına sahip herhangi bir depolama hesabı olabilir.
   * *RecoveryPlanName***- StorageAccountKey**: Yukarıdaki depolama hesabının erişim anahtarı.
   * *RecoveryPlanName***- VMGUIDS**: VM koruma sonra Azure Site Recovery her VM VM başarısız ayrıntılarını verir benzersiz bir kimliği atar. VMGUID elde etmek için seçin **kurtarma Hizmetleri** sekmesinde **korumalı öğe** &gt; **koruma grupları** &gt; **makineler** &gt; **özellikleri**. Birden çok VM varsa, GUID'ler virgülle ayrılmış bir dize olarak ekleyin.

    Kurtarma planının adı fileServerpredayRP, örneğin, sonra **değişkenleri**, **bağlantıları** ve **sertifikaları** sekmesi, ekledikten sonra aşağıdaki gibi görünmelidir Tüm varlıklar için.

    ![Varlıklar](./media/storsimple-disaster-recovery-using-azure-site-recovery/image5.png)

7. StorSimple 8000 serisi Runbook modülünün Otomasyon hesabınızda karşıya yükleyin. Kullanım bir modül eklemek için aşağıdaki adımları:

   a. PowerShell'i açın, yeni bir klasör oluşturun ve klasöre dizini değiştirin.
    
    ```
         mkdir C:\scripts\StorSimpleSDKTools
         cd C:\scripts\StorSimpleSDKTools
    ```
   b. Nuget CLI Adım1 aynı klasörde altında indirin.
      Nuget.exe çeşitli sürümleri mevcuttur [nuget indirmeleri](https://www.nuget.org/downloads). Bir .exe dosyasına doğrudan her karşıdan yükleme bağlantısı işaret nedenle mutlaka sağ tıklayın ve dosyayı bilgisayarınıza kaydedin yerine tarayıcıdan çalıştıran.

    ```
         wget https://dist.nuget.org/win-x86-commandline/latest/nuget.exe -Out C:\scripts\StorSimpleSDKTools\nuget.exe
    ```

   c. Bağımlı SDK'sını indirin

    ```
         C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Azure.Management.Storsimple8000series
         C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.IdentityModel.Clients.ActiveDirectory -Version 2.28.3
         C:\scripts\StorSimpleSDKTools\nuget.exe install Microsoft.Rest.ClientRuntime.Azure.Authentication -Version 2.2.9-preview
    ```

   d. StorSimple 8000 serisi aygıt yönetimi için bir Azure Otomasyonu Runbook modülü oluşturun. Kullanım komutları bir Otomasyon modül zip dosyası oluşturmak için aşağıda.

    ```
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

     e. Yukarıdaki adım oluşturulan Azure Otomasyonu modül zip dosyası (Microsoft.Azure.Management.StorSimple8000Series.zip) içeri aktarın. Bu Otomasyon hesabı seçerek yapılabilir, tıklatın **modülleri** paylaşılan kaynakları ve ardından altında **bir modül Ekle**.

    StorSimple 8000 serisi modülü içe aktardıktan sonra **modülleri** sekmesinde aşağıdaki gibi görünmelidir:

    ![Modüller](./media/storsimple-disaster-recovery-using-azure-site-recovery/image12.png)

8. Git **kurtarma Hizmetleri** bölümünde ve daha önce oluşturduğunuz Azure Site Recovery kasası seçin.
9. Seçin **kurtarma planları (Site Recovery)** gelen seçeneği **Yönet** grup ve yeni bir kurtarma planı gibi oluşturun:

   a.  Tıklatın **+ kurtarma planı** düğmesi, aşağıda dikey penceresi açılır.

      ![Kurtarma planı oluşturma](./media/storsimple-disaster-recovery-using-azure-site-recovery/image6.png)

   b.  Kurtarma planı adını girin, kaynak, hedef & dağıtım modeli değerlerini seçin.

   c.  Sanal makineleri kurtarma planına dahil aktarmak istediğiniz koruma grubundan seçin **Tamam** düğmesi.

   d.  Daha önce oluşturduğunuz kurtarma planı seçin, **Özelleştir** kurtarma planı özelleştirme görünümü açmak için düğmeye.

   e.  Sağ tıklayın **tüm grupları kapatma** tıklatıp **öncesi eylem eklemek**.

   f.  Açılır Ekle eylem dikey penceresinde, bir ad girin, seçin **birincil tarafı** seçeneği çalıştırmak, Automation hesabı (içinde eklediğiniz runbook'ları) seçin ve ardından Where seçeneğinde **StorSimple birim kapsayıcıları yük devretme** runbook.

   g.  Sağ tıklayın **Grup 1: Başlangıç** tıklatıp **Ekle korunan öğeler** seçeneğini sonra kurtarma planı ve tıklatın korunacak olan VM'ler seçin **Tamam** düğmesi. Zaten ise isteğe bağlı, VM'ler seçili.

   h.  Sağ tıklayın **Grup 1: Başlangıç** tıklatıp **sonrası eylemi** seçeneği ardından aşağıdaki betikler ekleyin:

   * Start-StorSimple-sanal-gereç runbook
   * StorSimple birim kapsayıcıları üzerinden runbook başarısız
   * Yük sonra bağlama birimleri runbook
   * Kaldırma-özel-betik-uzantısı runbook

   ı.  Yukarıdaki 4 betikleri sonra el ile bir eylem aynı eklemek **Grup 1: sonrası adımları** bölümü. Bu eylem, her şeyi düzgün çalıştığını doğrulayabilirsiniz noktasıdır. Bu eylem yalnızca yük devretme testi bir parçası olarak eklenmesi gerekir (Bu nedenle yalnızca select **yük devretme testi** onay kutusunu).

   j.  El ile işlem sonra Ekle **Temizleme** diğer runbook'lar için kullanılan yordamın aynısını kullanarak betik. **Kaydet** kurtarma planı.

    > [!NOTE]
    > El ile işlem tamamlandıktan sonra hedef cihazda klonlanmış StorSimple birimlerini temizleme bir parçası olarak silinecek çünkü bir yük devretme testi çalıştırırken, her şeye işlemi el ile adım doğrulamanız gerekir.
    >

    ![Recoery planı](./media/storsimple-disaster-recovery-using-azure-site-recovery/image7.png)

## <a name="perform-a-test-failover"></a>Bir sınama yük devretmesi gerçekleştirme
Başvurmak [Active Directory DR çözüm](../site-recovery/site-recovery-active-directory.md) konular için belirli Active Directory için yük devretme testi sırasında yardımcı kılavuz. Şirket içi Kurulum yük devretme sınaması oluştuğunda hiç dağıtılmış bir dosya değil. Şirket içi VM'ye bağlı StorSimple birimlerini azure'da StorSimple bulut uygulamasına kopyalandığı. Azure'da VM test amaçları için hazırlanmıştır ve kopyalanan birimleri VM'ye bağlı.

#### <a name="to-perform-the-test-failover"></a>Sınama yük devretmesi gerçekleştirme
1. Azure portalında Site Recovery kasanızı seçin.
2. Dosya sunucusu VM için oluşturulan kurtarma planı'ı tıklatın.
3. Tıklatın **yük devretme testi**.
4. Yük devretme gerçekleştikten sonra Azure Vm'lerinin bağlanması Azure sanal ağı seçin.

   ![Yük devretme başlatın](./media/storsimple-disaster-recovery-using-azure-site-recovery/image8.png)
5. Yük devretmeyi başlatmak için **Tamam**'a tıklayın. Özelliklerini açmak için VM'ye veya üzerinde tıklayarak ilerleme durumunu izleyebilirsiniz **Test yük devretme işini** kasa adı &gt; **işleri** &gt; **Site Recovery işleri**.
6. Yük devretme işlemi tamamlandıktan sonra ayrıca Azure çoğaltma görüyor olmalısınız makine Azure portalında görünür &gt; **sanal makineleri**. Doğrulama gerçekleştirebilirsiniz.
7. Doğrulama tamamlandıktan sonra tıklatın **doğrulamaları tam**. StorSimple birimleri kaldırın ve StorSimple bulut uygulaması kapatın.
8. İşiniz bittiğinde tıklatın **temizleme yük devretme testi** kurtarma planı üzerinde. Notları kaydedebilir ve yük devretme testiyle ilişkili gözlemlerinizi kaydetmek. Bu yük devretme testi sırasında oluşturulan sanal makineyi siler.

## <a name="perform-a-planned-failover"></a>Planlanmış bir yük devretme gerçekleştirin
   Planlanmış bir yük devretme sırasında şirket içi dosya sunucusu VM düzgün biçimde kapatılamadı ve Bulutu StorSimple cihazı birimler yedekleme anlık görüntüsü alınır. StorSimple birimlerini sanal cihaza yük devredildi, Azure üzerinde bir çoğaltma VM getirilene ve birimler VM'ye bağlı.

#### <a name="to-perform-a-planned-failover"></a>Planlanmış bir yük devretme gerçekleştirmek için
1. Azure portalında seçin **kurtarma Hizmetleri** kasa &gt; **kurtarma planları (Site Recovery)** &gt; **recoveryplan_name** için dosya sunucusu VM oluşturuldu.
2. Kurtarma planı dikey penceresinde **daha fazla** &gt; **planlanan yük devretme**.

   ![Kurtarma planı](./media/storsimple-disaster-recovery-using-azure-site-recovery/image9.png)
3. Üzerinde **planlı yük devretme onaylayın** dikey penceresinde, kaynak ve hedef konumları ve select hedef ağ seçin ve yük devretme işlemini başlatmak için ✓ onay simgesine tıklayın.
4. Çoğaltma sanal makineleri oluşturulduktan sonra yürütme bekleme durumuna demektir. Tıklatın **tamamlama** yük devretme kaydedilemedi.
5. Çoğaltma tamamlandıktan sonra sanal makineleri ikincil konumda Başlat.

## <a name="perform-a-failover"></a>Bir yük devretme gerçekleştirin.
Planlanmamış bir yük devretme sırasında StorSimple birimlerini sanal cihaza yük devredildi, Azure üzerinde getirilebilmesi bir çoğaltma VM ve birimler VM'ye bağlı.

#### <a name="to-perform-a-failover"></a>Bir yük devretme gerçekleştirmek için
1. Azure portalında seçin **kurtarma Hizmetleri** kasa &gt; **kurtarma planları (Site Recovery)** &gt; **recoveryplan_name** için dosya sunucusu VM oluşturuldu.
2. Kurtarma planı dikey penceresinde **daha fazla** &gt; **yük devretme**.
3. Üzerinde **onaylayın yük devretme** dikey penceresinde, kaynağını seçin ve hedef konumları.
4. Seçin **sanal makineleri kapatın ve en son verileri eşitleyin** Site Recovery korumalı sanal makineyi kapatın ve böylece verilerin en son sürümünü devredilir verileri eşitlemek denemelisiniz belirtmek için.
5. Yük devretme işleminden sonra yürütme bekleme durumuna sanal makine yok. Tıklatın **tamamlama** yük devretme kaydedilemedi.


## <a name="perform-a-failback"></a>Bir yeniden çalışma gerçekleştirme
Bir yedeklemenin sonra bir yeniden çalışma sırasında StorSimple birim kapsayıcıları fiziksel cihazı yük devredildi.

#### <a name="to-perform-a-failback"></a>Bir yeniden çalışma gerçekleştirmek için
1. Azure portalında seçin **kurtarma Hizmetleri** kasa &gt; **kurtarma planları (Site Recovery)** &gt; **recoveryplan_name** için dosya sunucusu VM oluşturuldu.
2. Kurtarma planı dikey penceresinde **daha fazla** &gt; **planlı yük devretme**.
3. Kaynak ve hedef konumların seçin, ilgili veri eşitleme ve VM oluşturma seçenekleri seçin.
4. Tıklatın **Tamam** düğmesi yeniden çalışma işlemini başlatmak üzere.

   ![Yeniden Başlat](./media/storsimple-disaster-recovery-using-azure-site-recovery/image10.png)

## <a name="best-practices"></a>En İyi Uygulamalar
### <a name="capacity-planning-and-readiness-assessment"></a>Kapasite planlama ve hazırlık değerlendirme
#### <a name="hyper-v-site"></a>Hyper-V sitesi
Kullanım [kullanıcı kapasite planlayıcısı aracı](http://www.microsoft.com/download/details.aspx?id=39057) sunucu, depolama ve ağ altyapısı, Hyper-V çoğaltma ortamınız için tasarlamak için.

#### <a name="azure"></a>Azure
Çalıştırabilirsiniz [Azure sanal makine hazırlık Değerlendirme Aracı](http://azure.microsoft.com/downloads/vm-readiness-assessment/) vm'lerde Azure Vm'leri ve Azure Site kurtarma Hizmetleri ile uyumlu olduklarından emin olun. Hazırlık Değerlendirme Aracı'nı VM yapılandırmaları denetler ve yapılandırmaları Azure ile uyumsuz olduğunda sizi uyarır. Örneğin, C: sürücüsündeki 127 GB'den büyükse bir uyarı verir.

Kapasite planlama en az iki önemli işlemden oluşur:

* Eşleme Hyper-V sanal makineleri Azure VM boyutlarını (örneğin, A6, A7, A8 ve A9) şirket içi.
* Gerekli Internet bant genişliği belirleme.

## <a name="limitations"></a>Sınırlamalar
* Şu anda yalnızca 1 StorSimple cihazı (bir tek StorSimple bulut uygulaması için) yük devretmenin. Senaryo birkaç StorSimple cihaz yayılan bir dosya sunucusu henüz desteklenmiyor.
* Bir sanal makine için koruma etkinleştirilirken bir hata alırsanız, iSCSI hedefleri kesmiş emin olun.
* Birim kapsayıcıları yayılan yedekleme ilkeleri nedeniyle birlikte gruplandırılmış tüm birim kapsayıcıları üzerinden birlikte devredilir.
* Seçtiğiniz birim kapsayıcıları tüm birimlerin üzerinden başarısız.
* Tek bir StorSimple bulut uygulaması maksimum kapasitesini 64 TB olduğundan en fazla 64 TB eklemek birimler üzerinde başarısız oldu olamaz.
* Planlanan/planlanmamış yük devretme başarısız olursa ve Azure'da VM'ler oluşturulduktan sonra temiz VM'ler yukarı değil. Bunun yerine, bir yeniden çalışma yapın. Sanal makineleri silerseniz, daha sonra şirket içi sanal makineleri yeniden açılamaz.
* Bir yük devretme sonrasında birimleri görmeye değilse Vm'lere gidin, Disk Yönetimi'ni açın, diskleri yeniden tarayın ve ardından bunları çevrimiçi duruma getirmeden.
* Bazı durumlarda, DR sitesi sürücü harflerinin harf şirket içi farklı olabilir. Bu durumda, yük devretme işlemi tamamlandıktan sonra sorunu el ile düzeltmeniz gerekir.
* Yük devretme iş zaman aşımı: StorSimple komut dosyası zaman aşımına birim kapsayıcıları için yük devretme betiği (şu anda 120 dakika) Azure Site Recovery sınırı daha uzun sürerse olur.
* Yedekleme işi zaman aşımı: StorSimple komut dosyası zaman aşımına birimlerini yedekleme betiği (şu anda 120 dakika) Azure Site Recovery sınırı daha uzun sürerse.

  > [!IMPORTANT]
  > Yedekleme Azure portalından el ile çalıştırın ve sonra kurtarma planı yeniden çalıştırın.

* İş zaman aşımı kopyalama: StorSimple komut dosyası zaman aşımına birimlerin kopyalama komut dosyası (şu anda 120 dakika) Azure Site Recovery sınırı daha uzun sürerse.
* Zaman eşitleme hatası: StorSimple yedekleme Portalı'nda başarılı olsa bile, yedeklemeler başarısız belirten çıkışı hataları komut dosyaları. Olası bir nedeni, StorSimple Gereci 's zaman saat dilimindeki geçerli zaman ile eşitlenmemiş olabilir olabilir.

  > [!IMPORTANT]
  > Saat dilimindeki geçerli saati Gereci saatiyle eşitleyin.

* Gereci yük devretme hatası: StorSimple komut dosyası varsa bir gereç yük devretme kurtarma planı çalışırken başarısız olabilir.

  > [!IMPORTANT]
  > Kurtarma planı Gereci yük devretme işlemi tamamlandıktan sonra yeniden çalıştırın.


## <a name="summary"></a>Özet
Azure Site Recovery kullanarak, bir dosya sunucusu VM için bir tam otomatik olağanüstü durum kurtarma planı oluşturabilirsiniz StorSimple depolama alanında barındırılan dosya paylaşımları sahip. Her yerden saniye içinde yük devretmeyi başlatın durumunda kesintisi ve get uygulama birkaç dakika içinde çalışır.
