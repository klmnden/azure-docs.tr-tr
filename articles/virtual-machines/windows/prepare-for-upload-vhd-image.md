---
title: Windows Azure'a karşıya yüklenecek VHD'yi hazırlama | Microsoft Docs
description: Bir Windows VHD veya VHDX Azure'a yüklemek üzere hazırlamayı öğrenin
services: virtual-machines-windows
documentationcenter: ''
author: glimoli
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 05/11/2019
ms.author: genli
ms.openlocfilehash: f40b3e0d2a49f6522149a977572d4f3c12e34255
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67720058"
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Bir Windows VHD veya VHDX yüklemek için hazırlama

Şirket içinden Azure'a bir Windows sanal makinesi (VM) yüklemeden önce sanal sabit disk (VHD veya VHDX) hazırlamanız gerekir. Azure, hem 1. kuşak hem de VHD dosyası biçiminde olan ve sabit boyutlu disk olan 2. kuşak Vm'leri destekler. VHD için izin verilen boyut 1,023 GB'dir. 

Nesil 1'de VM, VHD'ye bir VHDX dosya sistemi dönüştürebilirsiniz. Ayrıca, dinamik olarak genişleyen bir disk için bir sabit boyutlu disk dönüştürebilirsiniz. Ancak, bir sanal makinenin oluşturulması değiştiremezsiniz. Daha fazla bilgi için [oluşturmalıyım 1 veya 2. nesil Hyper-v VM?](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v) ve [Azure desteği için nesil 2 VM'ler (Önizleme)](generation-2.md).

Azure Vm'leri için destek ilkesi hakkında bilgi için bkz. [Azure Vm'leri için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!NOTE]
> Bu makaledeki yönergeler, Windows Server 2008 R2 ve sonraki Windows Server işletim sistemleri 64-bit sürümü için geçerlidir. Bir 32-bit işletim sistemi Azure'da çalıştırma hakkında daha fazla bilgi için bkz: [Azure sanal makinelerinde 32-bit işletim sistemleri için destek](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-the-virtual-disk-to-a-fixed-size-and-to-vhd"></a>Sanal diski sabit bir boyuta ve VHD dönüştürme 
Bu bölümde, sanal diskinizin Azure için gerekli biçime dönüştürmek gerekiyorsa yöntemlerden birini kullanın. Sanal diski dönüştürmeden önce VM'yi yedekleme. Windows VHD yerel sunucuda düzgün çalıştığından emin olun. Ardından, dönüştürme veya Azure'a yüklemek üzere çalışmadan önce VM'de hataları giderin.

Disk dönüştürdükten sonra disk kullanan bir VM oluşturun. Başlatın ve karşıya yükleme için hazırlanılıyor tamamlamak için VM'de oturum açın.

### <a name="use-hyper-v-manager-to-convert-the-disk"></a>Disk dönüştürmek için Hyper-V Yöneticisi'ni kullanın 
1. Hyper-V Yöneticisi'ni açın ve sol yerel bilgisayarınızda seçin. Bilgisayar listesinin üstündeki menüde **eylem** > **diski**.
2. Üzerinde **sanal sabit diski bulma** sayfasında, sanal diskinizin seçin.
3. Üzerinde **Eylem Seç** sayfasında **dönüştürme** > **sonraki**.
4. VHDX'te dönüştürmek istiyorsanız, seçin **VHD** > **sonraki**.
5. Dinamik olarak genişleyen bir diskten dönüştürmek istiyorsanız, seçin **boyutu sabit** > **sonraki**.
6. Bulun ve yeni VHD dosyasına kaydetmek için bir yol seçin.
7. **Son**’u seçin.

> [!NOTE]
> Bu makalede komutları çalıştırmak için yükseltilmiş bir PowerShell oturumu kullanın.

### <a name="use-powershell-to-convert-the-disk"></a>Disk dönüştürmek için PowerShell kullanma 
Bir sanal disk kullanarak da dönüştürebilirsiniz [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) Windows PowerShell komutu. Seçin **yönetici olarak çalıştır** PowerShell başlattığınızda. 

Aşağıdaki örnek komut, diskin VHDX'te VHD'ye dönüştürür. Komut ayrıca diski dinamik olarak genişleyen bir diskten bir sabit boyutlu diske dönüştürür.

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

Bu değeri komutta `-Path` yola dönüştürmek istediğiniz sanal sabit diske sahip. Değeri Değiştir `-DestinationPath` yeni yolunu ve dönüştürülen disk adı.

### <a name="convert-from-vmware-vmdk-disk-format"></a>VMware VMDK disk biçimden Dönüştür
Bir Windows VM görüntüsü varsa [VMDK dosya biçimi](https://en.wikipedia.org/wiki/VMDK), kullanın [Microsoft Virtual Machine Converter](https://www.microsoft.com/download/details.aspx?id=42497) VHD biçimine dönüştürülemedi. Daha fazla bilgi için [Hyper-V VHD için bir VMware VMDK dönüştürme](https://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Azure için Windows yapılandırmalarını ayarlama

Azure'a yüklemek planladığınız sanal makinede aşağıdaki komutları çalıştırın bir [yükseltilmiş bir komut istemi penceresi](https://technet.microsoft.com/library/cc947813.aspx):

1. Tüm statik kalıcı yolu yönlendirme tablosunda kaldırın:
   
   * Rota tablosunu görüntülemek için çalıştırın `route print` komut isteminde.
   * Denetleme `Persistence Routes` bölümler. Kalıcı bir yolu ise kullanın `route delete` kaldırmak için komutu.
2. WinHTTP proxy kaldırın:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```

    VM, belirli bir ara sunucu ile çalışmak gerekiyorsa proxy özel durum için Azure IP adresini ekleyin ([168.63.129.16](https://blogs.msdn.microsoft.com/mast/2015/05/18/what-is-the-ip-address-168-63-129-16/
)) VM'yi Azure'a bağlanabilmesi için:
    ```
    $proxyAddress="<your proxy server>"
    $proxyBypassList="<your list of bypasses>;168.63.129.16"

    netsh winhttp set proxy $proxyAddress $proxyBypassList
    ```

3. Disk SAN ilkesinin kümesine [ `Onlineall` ](https://technet.microsoft.com/library/gg252636.aspx):
   
    ```PowerShell
    diskpart 
    ```
    Açık komut istemi penceresinde aşağıdaki komutları yazın:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Windows için Eşgüdümlü Evrensel Saat (UTC) zamanı ayarlayın. Ayrıca Windows Saat hizmeti başlatma türünü ayarlayın (`w32time`) için `Automatic`:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" -Value 1 -Type DWord -force

    Set-Service -Name w32time -StartupType Automatic
    ```
5. Güç profili, yüksek performans olarak ayarlayın:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```
6. Ortam değişkenlerini emin `TEMP` ve `TMP` varsayılan değerlerine ayarlayın:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -name "TEMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Environment' -name "TMP" -Value "%SystemRoot%\TEMP" -Type ExpandString -force
    ```

## <a name="check-the-windows-services"></a>Windows Hizmetleri denetleme
Aşağıdaki Windows hizmetlerinin her biri Windows varsayılan değerlere ayarlandığından emin olun. Bu VM bağlantısı sağlamak için ayarlanması gereken en düşük hizmetleridir. Başlangıç ayarlarını sıfırlamak için aşağıdaki komutları çalıştırın:
   
```PowerShell
Set-Service -Name bfe -StartupType Automatic
Set-Service -Name dhcp -StartupType Automatic
Set-Service -Name dnscache -StartupType Automatic
Set-Service -Name IKEEXT -StartupType Automatic
Set-Service -Name iphlpsvc -StartupType Automatic
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Automatic
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Automatic
Set-Service -Name RemoteRegistry -StartupType Automatic
```

## <a name="update-remote-desktop-registry-settings"></a>Uzak Masaüstü kayıt defteri ayarlarını güncelleştirme
Aşağıdaki ayarlar, uzaktan erişim için doğru şekilde yapılandırıldığından emin olun:

>[!NOTE] 
>Programını çalıştırdığınızda, bir hata iletisi alabilirsiniz `Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services -name &lt;object name&gt; -value &lt;value&gt;`. Bu iletiyi güvenle yok sayabilirsiniz. Bu, yalnızca etki alanı Grup İlkesi nesnesi aracılığıyla söz konusu yapılandırmayı dağıtmaya değil anlamına gelir.

1. Uzak Masaüstü Protokolü (RDP) etkinleştirilir:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord -force
    ```
   
2. RDP bağlantı noktası doğru şekilde ayarlanır. Varsayılan bağlantı noktası 3389'dur:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" -Value 3389 -Type DWord -force
    ```
    Bir sanal makine dağıttığınızda, varsayılan kuralları 3389 numaralı bağlantı noktası karşı oluşturulur. Bağlantı noktası numarasını değiştirmek istiyorsanız, Azure'da VM dağıtıldıktan sonra yapın.

3. Dinleyici her ağ arabirimine dinliyor:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" -Value 0 -Type DWord -force
   ```
4. RDP bağlantıları için ağ düzeyinde kimlik doğrulama (NLA) modunu yapılandırın:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" -Value 1 -Type DWord -force

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" -Value 1 -Type DWord -force
     ```

5. Tutma değeri ayarlayın:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" -Value 1  -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" -Value 1  -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" -Value 1 -Type DWord -force
    ```
6. Yeniden bağlanma:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" -Value 0 -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" -Value 1 -Type DWord -force
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" -Value 0 -Type DWord -force
    ```
7. Eş zamanlı bağlantı sayısını sınırla:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" -Value 4294967295 -Type DWord -force
    ```
8. RDP dinleyiciye bağlı herhangi bir otomatik olarak imzalanan sertifikaları kaldırın:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash" -force
    ```
    Bu kod, VM dağıtırken başında bağlanabildiğinizi sağlar. Bu daha sonra gözden geçirmek gerekiyorsa, Azure'da VM dağıtıldıktan sonra bunu yapabilirsiniz.

9. VM bir etki alanının parçası olacaksa, önceki ayarları geri olmayan emin olmak için aşağıdaki ilkeleri denetleyin. 
    
    | Hedef                                     | İlke                                                                                                                                                       | Değer                                                                                    |
    |------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------|
    | RDP etkin                           | Computer Configuration\Policies\Windows Settings\Administrative Templates\Components\Remote Desktop Services\Remote Desktop Session Host\Connections         | Kullanıcıların Uzak Masaüstü'nü kullanarak bağlanmasına izin ver                                  |
    | NLA Grup İlkesi                         | Settings\Administrative Templates\Components\Remote Masaüstü Hizmetleri\Uzak Masaüstü Oturumu Ana Bilgisayarı\Güvenlik                                                    | NLA kullanarak uzaktan erişim için kullanıcı kimlik doğrulaması gerektir |
    | Tutma ayarları                      | Computer Configuration\Policies\Windows Settings\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections | Bağlantı tutma aralığı yapılandırın                                                 |
    | Ayarları yeniden                       | Computer Configuration\Policies\Windows Settings\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections | Otomatik olarak yeniden                                                                   |
    | Sınırlı sayıda bağlantı ayarları | Computer Configuration\Policies\Windows Settings\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Session Host\Connections | Bağlantı sayısını sınırla                                                              |

## <a name="configure-windows-firewall-rules"></a>Windows Güvenlik duvarı kuralları yapılandırma
1. Üç profil (etki alanı, standart ve genel) Windows Güvenlik duvarını açma:

   ```PowerShell
    Set-NetFirewallProfile -Profile Domain,Public,Private -Enabled True
   ```

2. WinRM (etki alanı, özel ve genel) üç güvenlik duvarı profilleri izin vermek için PowerShell'de aşağıdaki komutu çalıştırın ve PowerShell uzaktan hizmeti etkinleştirin:
   
   ```PowerShell
    Enable-PSRemoting -force

    Set-NetFirewallRule -DisplayName "Windows Remote Management (HTTP-In)" -Enabled True
   ```
3. RDP trafiğine izin vermek aşağıdaki güvenlik duvarı kuralları etkinleştirin:

   ```PowerShell
    Set-NetFirewallRule -DisplayGroup "Remote Desktop" -Enabled True
   ```   
4. Dosya ve yazıcı VM sanal ağ içindeki bir ping komutuna yanıt verebilmek paylaşımı için kuralı etkinleştir:

   ```PowerShell
   Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -Enabled True
   ``` 
5. VM bir etki alanının parçası olacaksa, önceki ayarları geri olmayan emin olmak için aşağıdaki Azure AD'ye ilkelerine göz atın. 

    | Hedef                                 | İlke                                                                                                                                                  | Value                                   |
    |--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------|
    | Windows Güvenlik duvarı profillerini etkinleştirme | Computer Configuration\Policies\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | Tüm ağ bağlantılarını korumaya         |
    | RDP'yi etkinleştirin                           | Computer Configuration\Policies\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | Gelen Uzak Masaüstü özel durumlarına izin ver |
    |                                      | Computer Configuration\Policies\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall | Gelen Uzak Masaüstü özel durumlarına izin ver |
    | ICMP V4 etkinleştir                       | Computer Configuration\Policies\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Firewall   | ICMP özel durumlarına izin ver                   |
    |                                      | Computer Configuration\Policies\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Firewall | ICMP özel durumlarına izin ver                   |

## <a name="verify-the-vm"></a>VM doğrulayın 

VM, iyi durumda, güvenli olduğundan emin olun ve RDP erişilebilir: 

1. Disk sağlıklı ve tutarlı olduğundan emin olmak için VM yeniden başlatıldığında konumundaki diskin kontrol edin:

    ```PowerShell
    Chkdsk /f
    ```
    Rapor temiz ve sorunsuz bir disk göründüğünden emin olun.

2. Önyükleme yapılandırma verileri (BCD) ayarlar. 

    > [!NOTE]
    > Bu komutları çalıştırmak için yükseltilmiş bir PowerShell penceresi kullanın.
   
   ```powershell
    cmd

    bcdedit /set {bootmgr} integrityservices enable
    bcdedit /set {default} device partition=C:
    bcdedit /set {default} integrityservices enable
    bcdedit /set {default} recoveryenabled Off
    bcdedit /set {default} osdevice partition=C:
    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures

    #Enable Serial Console Feature
    bcdedit /set {bootmgr} displaybootmenu yes
    bcdedit /set {bootmgr} timeout 5
    bcdedit /set {bootmgr} bootems yes
    bcdedit /ems {current} ON
    bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200

    exit
   ```
3. Döküm günlük Windows kilitlenme sorunları gidermeye yardımcı olabilir. Döküm günlük toplama etkinleştir:

    ```powershell
    # Set up the guest OS to collect a kernel dump on an OS crash event
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name CrashDumpEnabled -Type DWord -force -Value 2
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name DumpFile -Type ExpandString -force -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name NMICrashDump -Type DWord -force -Value 1

    # Set up the guest OS to collect user mode dumps on a service crash event
    $key = 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    if ((Test-Path -Path $key) -eq $false) {(New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting' -Name LocalDumps)}
    New-ItemProperty -Path $key -name DumpFolder -Type ExpandString -force -Value "c:\CrashDumps"
    New-ItemProperty -Path $key -name CrashCount -Type DWord -force -Value 10
    New-ItemProperty -Path $key -name DumpType -Type DWord -force -Value 2
    Set-Service -Name WerSvc -StartupType Manual
    ```
4. Windows Yönetim Araçları (WMI) deposunun tutarlı olduğunu doğrulayın:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Deposu bozuk olmadığını [WMI: Depo Bozulması veya](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

5. Başka bir uygulama bağlantı noktası 3389 kullandığından emin olun. Bu bağlantı noktası, azure'da RDP hizmeti için kullanılır. Sanal makinede hangi bağlantı noktalarının kullanıldığını görmek için çalıştırın `netstat -anob`:

    ```PowerShell
    netstat -anob
    ```

6. Bir etki alanı denetleyicisi bir Windows VHD karşıya yüklemek için:

   * İzleyin [ek adımları](https://support.microsoft.com/kb/2904015) disk hazırlamak için.

   * VM'yi belirli bir noktada DSRM modunda başlatmak gerekirse Dizin Hizmetleri Geri Yükleme Modu'nda (DSRM) parolasını bildiğinizden emin olun. Daha fazla bilgi için [DSRM parolasını ayarlayın](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

7. Yerleşik yönetici hesabı ve parolasını bildiğinizden emin olun. Geçerli yerel yönetici parolasını sıfırlayabilir ve Windows için RDP bağlantısı oturum açmak için bu hesabı kullandığınızdan emin olun isteyebilirsiniz. Bu erişim izni "oturum Uzak Masaüstü Hizmetleri üzerinden izin ver" Grup İlkesi nesnesi tarafından denetlenir. Bu nesne, yerel Grup İlkesi Düzenleyicisi'nde buraya bakın:

    Computer Configuration\Windows Settings\Security Settings\Local Policies\User Rights Assignment

8. RDP aracılığıyla veya ağdan RDP erişiminizi engellemeyen emin olmak için aşağıdaki Azure AD'ye ilkelerine göz atın:

    - Ağdan bu bilgisayara Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları ataması\yerel erişimi

    - Uzak Masaüstü Hizmetleri üzerinden Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları ataması\yerel oturum açma


9. Tüm gerekli erişim hesaplarının kaldırmakta değil emin olmak için aşağıdaki Azure AD İlkesi denetleyin:

   - Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Assignment\Access bu bilgisayara ağdan

   İlkeyi, aşağıdaki gruplar listelemesi gerekir:

   - Yöneticiler

   - Yedekleme işleçleri

   - Herkes

   - Kullanıcılar

10. Windows yine de iyi durumda olduğundan ve RDP bağlantısı üzerinden erişilebilen emin olmak için VM'yi yeniden başlatın. Bu noktada, yerel Hyper-sanal makine tamamen başlatılmadan emin olmak için V'de bir VM oluşturmak isteyebilirsiniz. Ardından VM RDP aracılığıyla size ulaşabildiğimizden emin olmak için test edin.

11. Tüm ek aktarım sürücüsü arabirimi (TDI) filtreleri kaldırın. Örneğin, TCP analiz yazılımı kaldırmak paketler ya da ek güvenlik duvarları. Bu daha sonra gözden geçirmek gerekiyorsa, Azure'da VM dağıtıldıktan sonra bunu yapabilirsiniz.

12. Herhangi bir üçüncü taraf yazılım veya ilişkili fiziksel bileşenler veya başka bir sanallaştırma teknolojisi sürücü kaldırın.

### <a name="install-windows-updates"></a>Windows güncelleştirmeleri yükle
İdeal olarak, güncelleştirme zamanı makine tutmalısınız *düzeltme eki düzeyi*. Bu mümkün değilse, aşağıdaki güncelleştirmelerin yüklü olduğundan emin olun:

| Bileşen               | Binary         | Windows 7 SP1, Windows Server 2008 R2 SP1 | Windows 8, Windows Server 2012               | Windows 8.1, Windows Server 2012 R2 | Windows 10 v1607, Windows Server 2016 v1607 | Windows 10 v1703    | Windows 10 v1709, Windows Server 2016 v1709 | Windows 10 v1803, Windows Server 2016 v1803 |
|-------------------------|----------------|-------------------------------------------|---------------------------------------------|------------------------------------|---------------------------------------------------------|----------------------------|-------------------------------------------------|-------------------------------------------------|
| Depolama                 | disk.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061         | -                                                       | -                          | -                                               | -                                               |
|                         | Storport.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.332             | -                                               | -                                               |
|                         | ntfs.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Iologmsg.dll   | 6.1.7601.23403 - KB3125574                | 6.2.9200.16384 - KB2995387                  | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Classpnp.sys   | 6.1.7601.23403 - KB3125574                | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614         | 10.0.14393.953 - KB4022715                              | -                          | -                                               | -                                               |
|                         | Volsnap.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384         | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | Partmgr.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850         | 10.0.14393.953 - KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | volmgr.sys     |                                           |                                             |                                    |                                                         | 10.0.15063.0               | -                                               | -                                               |
|                         | Volmgrx.sys    | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | 10.0.15063.0               | -                                               | -                                               |
|                         | Msiscsi.sys    | 6.1.7601.23403 - KB3125574                | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726         | 10.0.14393.1066 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | Msdsm.sys      | 6.1.7601.23403 - KB3125574                | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726         | -                                                       | -                          | -                                               | -                                               |
|                         | Mpio.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726         | 10.0.14393.1198 - KB4022715                             | -                          | -                                               | -                                               |
|                         | vmstorfl.sys   | 6.3.9600.18907 - KB4072650                | 6.3.9600.18080 - KB3063109                  | 6.3.9600.18907 - KB4072650         | 10.0.14393.2007 - KB4345418                             | 10.0.15063.850 - KB4345419 | 10.0.16299.371 - KB4345420                      | -                                               |
|                         | Fveapi.dll     | 6.1.7601.23311 - KB3125574                | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614         | 10.0.14393.576 - KB4022715                              | -                          | -                                               | -                                               |
|                         | Fveapibase.dll | 6.1.7601.23403 - KB3125574                | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614         | 10.0.14393.206 - KB4022715                              | -                          | -                                               | -                                               |
| Ağ                 | netvsc.sys     | -                                         | -                                           | -                                  | 10.0.14393.1198 - KB4022715                             | 10.0.15063.250 - KB4020001 | -                                               | -                                               |
|                         | mrxsmb10.sys   | 6.1.7601.23816 - KB4022722                | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726         | 10.0.14393.479 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | mrxsmb20.sys   | 6.1.7601.23816 - KB4022722                | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726         | 10.0.14393.953 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | Mrxsmb.sys     | 6.1.7601.23816 - KB4022722                | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726         | 10.0.14393.953 - KB4022715                              | 10.0.15063.0               | -                                               | -                                               |
|                         | Tcpip.sys      | 6.1.7601.23761 - KB4022722                | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.447             | -                                               | -                                               |
|                         | HTTP.sys       | 6.1.7601.23403 - KB3125574                | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726         | 10.0.14393.251 - KB4022715                              | 10.0.15063.483             | -                                               | -                                               |
|                         | vmswitch.sys   | 6.1.7601.23727 - KB4022719                | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.138             | -                                               | -                                               |
| Core                    | ntoskrnl.exe   | 6.1.7601.23807 - KB4022719                | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726         | 10.0.14393.1358 - KB4022715                             | 10.0.15063.483             | -                                               | -                                               |
| Uzak Masaüstü Hizmetleri | rdpcorets.dll  | 6.2.9200.21506 - KB4022719                | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726         | 10.0.14393.1198 - KB4022715                             | 10.0.15063.0               | -                                               | -                                               |
|                         | termsrv.dll    | 6.1.7601.23403 - KB3125574                | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850         | 10.0.14393.0 - KB4022715                                | 10.0.15063.0               | -                                               | -                                               |
|                         | termdd.sys     | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Win32k.sys     | 6.1.7601.23807 - KB4022719                | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726         | 10.0.14393.594 - KB4022715                              | -                          | -                                               | -                                               |
|                         | rdpdd.dll      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
|                         | Rdpwd.sys      | 6.1.7601.23403 - KB3125574                | -                                           | -                                  | -                                                       | -                          | -                                               | -                                               |
| Güvenlik                | MS17-010       | KB4012212                                 | KB4012213                                   | KB4012213                          | KB4012606                                               | KB4012606                  | -                                               | -                                               |
|                         |                |                                           | KB4012216                                   |                                    | KB4013198                                               | KB4013198                  | -                                               | -                                               |
|                         |                | KB4012215                                 | KB4012214                                   | KB4012216                          | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         |                |                                           | KB4012217                                   |                                    | KB4013429                                               | KB4013429                  | -                                               | -                                               |
|                         | CVE-2018-0886  | KB4103718               | KB4103730                | KB4103725       | KB4103723                                               | KB4103731                  | KB4103727                                       | KB4103721                                       |
|                         |                | KB4103712          | KB4103726          | KB4103715|                                                         |                            |                                                 |                                                 |
       
### Ne zaman Sysprep kullanılacağını belirleme <a id="step23"></a>    

Sistem Hazırlama Aracı'nı (Sysprep), bir Windows yükleme sıfırlamak için çalıştırabileceğiniz bir işlemdir. Sysprep tüm kişisel verilerin kaldırılması ve çeşitli bileşenleri sıfırlama "kullanıma hazır" deneyimi sağlar. 

Genellikle belirli bir yapılandırmaya sahip birden fazla diğer Vm'lere dağıtmak için bir şablon oluşturmak için Sysprep'i çalıştırın. Şablonun adlı bir *genelleştirilmiş görüntü*.

Bir diskten yalnızca bir VM oluşturmak istiyorsanız, Sysprep kullanmak zorunda değilsiniz. Bunun yerine, sanal makineden oluşturabileceğiniz bir *özelleştirilmiş görüntü*. Özelleştirilmiş diskten VM oluşturma hakkında daha fazla bilgi için bkz:

- [Özelleştirilmiş diskten VM oluşturma](create-vm-specialized.md)
- [Özel bir VHD diskten VM oluşturma](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-specialized-portal?branch=master)

Genelleştirilmiş görüntü oluşturmak istiyorsanız, Sysprep'nı çalıştırmanız gerekir. Daha fazla bilgi için [Sysprep kullanma: Giriş](https://technet.microsoft.com/library/bb457073.aspx). 

Her rol ya da Windows tabanlı bir bilgisayarda yüklü uygulama genelleştirilmiş görüntüleri destekler. Bu nedenle, bu yordamı çalıştırmadan önce Sysprep bilgisayar rolünü destekler emin olun. Daha fazla bilgi için [sunucu rolleri için Sysprep Destek](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="generalize-a-vhd"></a>Bir VHD generalize

>[!NOTE]
> Çalıştırdıktan sonra `sysprep.exe` aşağıdaki adımlarda, VM'yi açın. Görüntü buradan Azure'da oluşturduğunuz kadar yoksa tekrar etkinleştirebilirsiniz.

1. Windows VM için oturum açın.
1. Çalıştırma **komut istemi** yönetici olarak. 
1. Dizinine `%windir%\system32\sysprep`. Ardından `sysprep.exe` komutunu çalıştırın.
1. **Sistem Hazırlama Aracı** iletişim kutusunda  **Sistem İlk Çalıştırma Deneyimi (OOBE) Moduna Gir**'i seçin ve **Genelleştir** onay kutusunun seçili olduğundan emin olun.

    ![Sistem hazırlığı aracı](media/prepare-for-upload-vhd-image/syspre.png)
1. İçinde **kapatma seçenekleri**seçin **kapatma**.
1. **Tamam**’ı seçin.
1. Sysprep sona erdiğinde, VM'yi kapatın. Kullanmayın **yeniden** sanal makineyi.

Artık VHD yüklenmek hazırdır. Genelleştirilmiş bir diskten VM oluşturma hakkında daha fazla bilgi için bkz. [genelleştirilmiş VHD yükleme ve Azure'da yeni bir VM oluşturmak için bunu kullanın](sa-upload-generalized.md).


>[!NOTE]
> Özel bir *unattend.xml* dosya desteklenmiyor. Destekliyoruz ancak `additionalUnattendContent` eklemek için yalnızca sınırlı destek sağlayan bir özellik [microsoft-windows-shell-setup](https://docs.microsoft.com/windows-hardware/customize/desktop/unattend/microsoft-windows-shell-setup) içine seçenekleri *unattend.xml* dosya Azure sağlama Aracı kullanır. Örneğin, kullanabileceğiniz [additionalUnattendContent](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.compute.models.additionalunattendcontent?view=azure-dotnet) FirstLogonCommands ve LogonCommands eklemek için. Daha fazla bilgi için [additionalUnattendContent FirstLogonCommands örnek](https://github.com/Azure/azure-quickstart-templates/issues/1407).


## <a name="complete-the-recommended-configurations"></a>Önerilen yapılandırmaları tamamlayın
Aşağıdaki ayarlar, VHD'yi karşıya etkilemez. Ancak, bunları yapılandırılmış önerilir.

* Yükleme [Azure sanal makine Aracısı](https://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Ardından, VM uzantılarını etkinleştirebilirsiniz. VM uzantıları, Vm'lerle kullanmak isteyebileceğiniz kritik işlevselliğinin büyük kısmını uygulayın. Örneğin, parolalarını sıfırlama veya RDP yapılandırmak için uzantıları gerekir. Daha fazla bilgi için [Azure sanal makine Aracısı genel bakış](../extensions/agent-windows.md).
* Azure'da VM oluşturduktan sonra disk belleği dosyası yerleştirme öneririz *zamana bağlı sürücü* performansını artırmak için. Dosyası yerleştirme aşağıdaki gibi ayarlayabilirsiniz:

   ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile.sys" -Type MultiString -force
   ```
  Veri diski sanal Makineye bağlıysa, zamana bağlı sürücü birim harfi genellikle olduğu *D*. Bu gösterim ayarlarınızı ve kullanılabilir sürücüler sayısına bağlı olarak farklı olabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Resource Manager dağıtımları için azure'a bir Windows VM görüntüsü](upload-generalized-managed.md)
* [Azure Windows VM etkinleştirme sorunlarını giderme](troubleshoot-activation-problems.md)

