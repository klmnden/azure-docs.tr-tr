---
title: Hazırlama ve ana bir VHD görüntüsü - Azure'ı özelleştirme
description: Nasıl hazırlama, özelleştirme ve Azure'a bir Windows sanal masaüstü Önizleme ana görüntü yükleyin.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/03/2019
ms.author: helohr
ms.openlocfilehash: 9df4be5534a1cbe6aa4ffb9c60bb180fd4587d32
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65551038"
---
# <a name="prepare-and-customize-a-master-vhd-image"></a>Ana VHD görüntüsünü hazırlama ve özelleştirme

Bu makalede yüklemek için Azure sanal makineler (VM) oluşturma ve yazılım yükler dahil olmak üzere, bir ana sanal sabit disk (VHD) görüntüsü hazırlama anlatır. Kuruluşunuzun mevcut işlemleri ile kullanılabilecek bir Windows sanal masaüstü Önizleme özgü yapılandırma için bu yönergeleri yöneliktir.

## <a name="create-a-vm"></a>VM oluşturma

Windows 10 Enterprise çok oturumu, Azure görüntü galerisinde kullanılabilir. Bu görüntü özelleştirmeye yönelik iki seçenek vardır.

İlk sanal makine (VM) sağlamak için Azure'da yönergelerini takip ederek seçenektir [yönetilen bir görüntüden VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)ve ardından atlayın [yazılım hazırlama ve yükleme](set-up-customize-master-image.md#software-preparation-and-installation).

İkinci seçenek görüntünün yerel görüntü yükleniyor, Hyper-V VM sağlama ve aşağıdaki bölümde ele gereksinimlerinize uyacak şekilde özelleştirme oluşturmaktır.

### <a name="local-image-creation"></a>Yerel görüntü oluşturma

Görüntüyü yerel bir konuma indirdikten sonra açmak **Hyper-V Yöneticisi'ni** kopyaladığınız VHD ile VM oluşturmak için. Aşağıdaki yönergeler, basit bir sürümü yöneliktir, ancak daha ayrıntılı yönergeleri bulabilirsiniz [Hyper-V sanal makine oluşturma](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v).

Kopyalanan VHD ile VM oluşturmak için:

1. Açık **yeni sanal makine Sihirbazı**.

2. Nesli belirtin sayfasında, seçin **1. nesil**.

    ![Sayfasının ekran görüntüsü nesli belirtin. "1. nesil" seçeneği seçilidir.](media/a41174fd41302a181e46385e1e701975.png)

3. Denetim noktası türü'nün altında kontrol noktaları onay kutusunun işaretini kaldırarak devre dışı bırakın.

    ![Kontrol Noktaları sayfasını kontrol noktası türü bölümünün ekran görüntüsü.](media/20c6dda51d7cafef33251188ae1c0c6a.png)

Ayrıca, kontrol noktaları devre dışı bırakmak için PowerShell'de aşağıdaki cmdlet'i çalıştırabilirsiniz.

```powershell
Set-VM -Name <VMNAME> -CheckpointType Disabled
```

### <a name="fixed-disk"></a>Sabit disk

Mevcut bir VHD'den VM oluşturma, varsayılan olarak dinamik bir disk oluşturur. Sabit bir diske onu değiştirilebilir seçerek **Disk Düzenle...**  aşağıdaki görüntüde gösterildiği gibi. Daha ayrıntılı yönergeler için bkz. [Windows VHD veya VHDX yüklemek için hazırlama](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

![Diski seçeneğinin ekran görüntüsü.](media/35772414b5a0f81f06f54065561d1414.png)

Diski sabit bir disk olarak değiştirmek için aşağıdaki PowerShell cmdlet'ini de çalıştırabilirsiniz.

```powershell
Convert-VHD –Path c:\\test\\MY-VM.vhdx –DestinationPath c:\\test\\MY-NEW-VM.vhd -VHDType Fixed
```

## <a name="software-preparation-and-installation"></a>Yazılım hazırlama ve yükleme

Bu bölümde, hazırlama ve FSLogix, Windows Defender ve diğer genel uygulamalara yükleme ele alınmaktadır. 

Sanal makinenizde Office 365 ProPlus'ı ve OneDrive yüklüyorsanız, bkz. [ana bir VHD görüntüsü Office yükleme](install-office-on-wvd-master-image.md). Bu makaleye dönün ve ana VHD işlemi tamamlamak için bu makalenin sonraki adımlarda bağlantıyı izleyin.

Kullanıcılarınızın belirli LOB uygulamalarına erişmeniz gerekiyorsa, bunları bu bölümün yönergeleri tamamladıktan sonra yüklemeniz önerilir.

### <a name="disable-automatic-updates"></a>Otomatik Güncelleştirmeler devre dışı bırak

Yerel Grup İlkesi aracılığıyla otomatik güncelleştirmeler devre dışı bırakmak için:

1. Açık **yerel Grup İlkesi Düzenleyicisi\\Yönetim Şablonları\\Windows bileşenleri\\Windows Update**.
2. Sağ **otomatik güncelleştirme yapılandırma** ve **devre dışı bırakılmış**.

Ayrıca, bir komut istemi otomatik güncelleştirmeler devre dışı bırakmak için aşağıdaki komutu çalıştırabilirsiniz.

```batch
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v NoAutoUpdate /t REG_DWORD /d 1 /f
```

### <a name="specify-start-layout-for-windows-10-pcs-optional"></a>Windows 10 bilgisayarları için (isteğe bağlı) başlangıç düzenini belirtin

Windows 10 bilgisayarları için bir başlangıç düzenini belirtmek için bu komutu çalıştırın.

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" /v SpecialRoamingOverrideAllowed /t REG_DWORD /d 1 /f
```

### <a name="set-up-user-profile-container-fslogix"></a>Kullanıcı profili kapsayıcısı ayarlamak (FSLogix)

FSLogix kapsayıcı görüntüsünün bir parçası dahil etmek için yönergeleri izleyin. [ana makine havuzu için bir kullanıcı profili paylaşımını ayarlama](create-host-pools-user-profile.md#configure-the-fslogix-profile-container). FSLogix kapsayıcıyla işlevselliğini test edebilirsiniz [Bu hızlı başlangıçta](https://docs.fslogix.com/display/20170529/Profile+Containers+-+Quick+Start).

### <a name="configure-windows-defender"></a>Windows Defender'ı yapılandırma

Windows Defender VM yapılandırıldıysa, bu VHD ve VHDX dosyaları tüm içeriğini değil için tarama sırasında ek yapılandırılmış emin olun.

Bu yapılandırma yalnızca VHD ve VHDX dosyaları tarama sırasında ek kaldırır, ancak gerçek zamanlı tarama etkilemez.

Daha ayrıntılı yönergeler için Windows Server üzerinde Windows Defender'ın nasıl yapılandıracağınızı öğrenmek için bkz. [Windows Server'da yapılandırma Windows Defender virüsten koruma dışlamaları](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus).

Windows Defender'ın belirli dosyaları taramanın dışında tutulacak yapılandırma hakkında daha fazla bilgi için bkz. [yapılandırma ve dosya uzantısı ve klasör konumuna dayalı özel durumlar doğrulama](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-extension-file-exclusions-windows-defender-antivirus).

### <a name="configure-session-timeout-policies"></a>Oturum zaman aşımı ilkelerini yapılandırma

Bir ana makine havuzundaki tüm VM'lerin aynı güvenlik grubunun bir parçası olduğundan, uzak oturum ilkeleri Grup İlkesi düzeyinde uygulanabilir.

Uzak oturum ilkeleri yapılandırmak için:

1. Gidin **Yönetim Şablonları** > **Windows bileşenleri** > **Uzak Masaüstü Hizmetleri**  >  **Uzak Masaüstü oturumu konağı** > **olanağı**.
2. Sağ taraftaki bölmede seçin **etkin ancak boşta Uzak Masaüstü Hizmetleri oturumları için süre sınırı** ilkesi.
3. Kalıcı pencere göründükten sonra ilke seçeneği değiştirmek **yapılandırılmadı** için **etkin** ilkelerini etkinleştirmek için.
4. İlke seçeneği altındaki aşağı açılan menüde için süreyi ayarlama **4 saat**.

Aşağıdaki komutları çalıştırarak uzak oturum ilkeleri el ile de yapılandırabilirsiniz:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fResetBroken /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxConnectionTime /t REG_DWORD /d 10800000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxDisconnectionTime /t REG_DWORD /d 5000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxIdleTime /t REG_DWORD /d 7200000 /f
```

### <a name="set-up-time-zone-redirection"></a>Saat dilimi yeniden yönlendirme ayarlayın

Bir ana makine havuzundaki tüm VM'lerin aynı güvenlik grubunun bir parçası olduğundan, saat dilimi yeniden yönlendirme Grup İlkesi düzeyinde uygulanabilir.

Saat dilimlerini yönlendirmek için:

1. Active Directory sunucusunda açın **Grup İlkesi Yönetim Konsolu**.
2. Etki alanı ve Grup İlkesi Nesneleri'ni genişletin.
3. Sağ **Grup İlkesi nesnesini** seçin ve Grup İlkesi ayarları için oluşturulan **Düzenle**.
4. İçinde **Grup İlkesi Yönetimi Düzenleyicisi**, gitmek **Bilgisayar Yapılandırması** > **ilkeleri** > **Yönetim Şablonları** > **Windows bileşenleri** > **Uzak Masaüstü Hizmetleri** > **Uzak Masaüstü oturumu konağı**   >  **Cihaz ve kaynak yeniden yönlendirme**.
5. Etkinleştirme **saat dilimi yeniden yönlendirmesine izin** ayarı.

Ayrıca, saat dilimlerini yönlendirmek için ana görüntü bu komutu çalıştırabilirsiniz:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fEnableTimeZoneRedirection /t REG_DWORD /d 1 /f
```

### <a name="disable-storage-sense"></a>Depolama algılama devre dışı bırak

Windows 10 Enterprise veya Windows 10 Enterprise çok oturumu kullanan Windows Sanal Masaüstü oturumu ana bilgisayarı için depolama algılama devre dışı bırakılması önerilir. Depolama algılama Ayarlar menüsünden devre dışı bırakabilirsiniz **depolama**aşağıdaki ekran görüntüsünde gösterildiği gibi:

![Ayarlar altındaki depolama menüsünün ekran görüntüsü. "Depolama algılama" seçeneği devre dışıdır.](media/storagesense.png)

Aşağıdaki komutu çalıştırarak da ile kayıt defteri ayarını değiştirebilirsiniz:

```batch
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy /v 01 /t REG_DWORD /d 0 /f
```

### <a name="include-additional-language-support"></a>Ek dil desteği içerir

Bu makalede, dili ve bölgesel destek nasıl yapılandırılacağı ele alınmamıştır. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Windows görüntüleri dil Ekle](https://docs.microsoft.com/windows-hardware/manufacture/desktop/add-language-packs-to-windows)
- [İsteğe bağlı özellikler](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)
- [İsteğe bağlı (FOD) dil ve bölge Özellikler](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-language-fod)

### <a name="other-applications-and-registry-configuration"></a>Diğer uygulama ve kayıt defteri yapılandırma

Bu bölüm, uygulama ve işletim sistemi yapılandırması kapsar. Bu bölümdeki tüm yapılandırma komut satırı tarafından yürütülebilen kayıt defteri girdileri ile gerçekleştirilir ve regedit araçları.

>[!NOTE]
>Grup İlkesi nesneleri (GPO'lar) ya da kayıt defteri içeri aktarmalar yapılandırmasında en iyi yöntemleri uygulayabilirsiniz. Yönetici kullanıcıların, kuruluş gereksinimlerine göre iki seçenekten birini seçebilirsiniz.

Geri bildirim hub'ı koleksiyonu telemetri verilerinin için Windows 10 Enterprise çok oturumunda şu komutu çalıştırın:

```batch
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection "AllowTelemetry"=dword:00000003
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection /v AllowTelemetry /d 3
```

Watson çökme düzeltmek için aşağıdaki komutu çalıştırın:

```batch
remove CorporateWerServer* from Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting
```

Kayıt Defteri Düzenleyicisi 5 k çözünürlük desteği düzeltmek için aşağıdaki komutları girin. Yan yana yığın etkinleştirmeden önce komutları çalıştırmanız gerekir.

```batch
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp]
"MaxMonitors"=dword:00000004
"MaxXResolution"=dword:00001400
"MaxYResolution"=dword:00000b40

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs]
"MaxMonitors"=dword:00000004
"MaxXResolution"=dword:00001400
"MaxYResolution"=dword:00000b40
```

## <a name="prepare-the-image-for-upload-to-azure"></a>Yüklemek için Azure görüntüsü hazırlama

Yapılandırma tamamlandı ve tüm uygulamaların yüklenmesini sonra yönergeleri izleyin [Windows VHD veya VHDX yüklemek için hazırlama](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) görüntüsünü hazırlamak için.

Görüntüyü karşıya yükleme için hazırlandıktan sonra sanal makine kapalı veya serbest bırakıldı durumunda kalır emin olun.

## <a name="upload-master-image-to-a-storage-account-in-azure"></a>Ana görüntü, Azure depolama hesabında karşıya yükleme

Bu bölüm, yalnızca yerel ana görüntü oluşturulduğunda geçerlidir.

Azure depolama hesabınız, ana görüntü karşıya yükleme aşağıdaki yönergeler size bildirir. Azure depolama hesabınız yoksa, yönergeleri izleyin [bu makalede](https://code.visualstudio.com/tutorials/static-website/create-storage) oluşturmak için.

1. Henüz yapmadıysanız sabit VM görüntüsü (VHD) dönüştürmek. Sabit görüntü yoksa dönüştürürseniz, görüntünün başarıyla oluşturulamıyor.

2. VHD, depolama hesabınızdaki blob kapsayıcısına yükleyin. İle hızlı bir şekilde karşıya yükleyebilirsiniz [Depolama Gezgini aracını](https://azure.microsoft.com/features/storage-explorer/). Depolama Gezgini aracı hakkında daha fazla bilgi için bkz. [bu makalede](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

    ![Microsoft Azure Depolama Gezgini Aracı'nın arama penceresinin ekran görüntüsü. ".Vhd ya da vhdx dosyalarını karşıya yükle (önerilen), sayfa blobları" onay kutusu seçilidir.](media/897aa9a9b6acc0aa775c31e7fd82df02.png)

3. Ardından, tarayıcı ve "Görüntüler" için arama Azure portalına gidin Arama için sağlama **görüntüsü oluştur** sayfasında, aşağıdaki ekran görüntüsünde gösterildiği gibi:

    ![Azure portal'ın Oluştur görüntüsü sayfasının ekran görüntüsü görüntüsü için örnek değerleri ile doldurulur.](media/d3c840fe3e2430c8b9b1f44b27d2bf4f.png)

4. Görüntüyü oluşturduktan sonra aşağıdaki ekran görüntüsüne benzeyen bir bildirim görmeniz gerekir:

    !["Görüntüsü başarıyla oluşturuldu" bildiriminin ekran görüntüsü.](media/1f41b7192824a2950718a2b7bb9e9d69.png)

## <a name="next-steps"></a>Sonraki adımlar

Bir görüntünüz olduğuna göre oluşturun veya ana bilgisayar havuzları güncelleştirin. Ana bilgisayar havuzları oluşturmak nasıl hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Bir Azure Resource Manager şablonu ile bir ana makine havuzu oluşturma](create-host-pools-arm-template.md)
- [Öğretici: Azure Marketi ile konak havuz oluşturma](create-host-pools-azure-marketplace.md)
- [PowerShell ile bir ana makine havuzu oluşturma](create-host-pools-powershell.md)
- [Bir ana makine havuzu için bir kullanıcı profili paylaşımını ayarlama](create-host-pools-user-profile.md)
- [Windows sanal masaüstü yük dengeleme yöntemini yapılandırma](configure-host-pool-load-balancing.md)
