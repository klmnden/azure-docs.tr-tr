---
title: "Windows Azure'a yüklemeniz VHD hazırlama | Microsoft Docs"
description: "Azure için karşıya yüklemeden önce Windows VHD veya VHDX hazırlama"
services: virtual-machines-windows
documentationcenter: 
author: glimoli
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7802489d-33ec-4302-82a4-91463d03887a
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: troubleshooting
ms.date: 08/01/2017
ms.author: genli
ms.openlocfilehash: 60da4d7f418e2aac6ed6d41092486d2ccdaf940c
ms.sourcegitcommit: 9c3150e91cc3075141dc2955a01f47040d76048a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/26/2017
---
# <a name="prepare-a-windows-vhd-or-vhdx-to-upload-to-azure"></a>Bir Windows VHD veya VHDX Azure'a karşıya yüklemek için hazırlama
Bir Windows sanal makineler (VM) şirket içi Microsoft Azure karşıya yüklemeden önce sanal sabit disk (VHD veya VHDX) hazırlamanız gerekir. Azure, sabit boyutlu disk VHD dosyası biçiminde olan ve 1. nesil VM destekler. VHD için izin verilen maksimum boyutu 1,023 GB'dir. 1 sanal makineden VHDX dosya sistemi VHD ve sabit boyutlu için dinamik olarak genişleyen bir diskten nesil dönüştürebilirsiniz. Ancak, bir sanal makinenin oluşturma değiştiremezsiniz. Daha fazla bilgi için bkz: [kuşak 1 veya 2 oluşturmanız gerekir Hyper-V'de VM](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/plan/should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v).

Azure VM için destek ilkesi hakkında daha fazla bilgi için bkz: [Microsoft Azure sanal makineleri için Microsoft sunucu yazılımı desteği](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).

> [!Note]
> Bu makaledeki yönergeleri Windows Server 2008 R2 ve sonraki Windows server işletim sistemi 64-bit sürümü için geçerlidir. Azure işletim sisteminde çalışan 32 bit sürümü hakkında daha fazla bilgi için bkz: [Azure sanal makinelerde 32-bit işletim sistemleri için destek](https://support.microsoft.com/help/4021388/support-for-32-bit-operating-systems-in-azure-virtual-machines).

## <a name="convert-the-virtual-disk-to-vhd-and-fixed-size-disk"></a>Sanal disk VHD ve sabit boyutlu disk dönüştürün 
Bu bölümde, sanal diskinizin Azure için gerekli biçime dönüştürmek gerekiyorsa, yöntemlerden birini kullanın. Sanal disk dönüştürme işlemi çalıştırın ve emin olmak için önce VM yedeklemesi Windows VHD yerel sunucuda düzgün çalışır. Dönüştür veya Azure'a yüklemek üzere çalışmadan önce VM dahilinde hataları çözümleyin.

Disk dönüştürdükten sonra dönüştürülmüş disk kullanan bir VM oluşturun. Başlangıç ve karşıya yükleme için VM hazırlama tamamlamak için VM oturum açın.

### <a name="convert-disk-using-hyper-v-manager"></a>Hyper-V Yöneticisi'ni kullanarak diski Dönüştür
1. Hyper-V Yöneticisi'ni açın ve sol yerel bilgisayarınızda seçin. Bilgisayar listesi yukarıda menüye tıklayın **eylem** > **Düzenle Disk**.
2. Üzerinde **sanal sabit diski bulma** ekranında, bulun ve sanal disk seçin.
3. Üzerinde **eylemine** ekran ve ardından **Dönüştür** ve **sonraki**.
4. VHDX'ten dönüştürmek istiyorsanız seçin **VHD** ve ardından **sonraki**
5. Dinamik olarak genişleyen bir diskten dönüştürmek istiyorsanız seçin **boyutu sabit** ve ardından **sonraki**
6. Bulun ve yeni VHD dosyasına kaydetmek için bir yol seçin.
7. **Son**'a tıklayın.

>[!NOTE]
>Bu makalede komutları yükseltilmiş bir PowerShell oturumunda çalıştırmanız gerekir.

### <a name="convert-disk-by-using-powershell"></a>PowerShell kullanarak diski Dönüştür
Kullanarak bir sanal disk dönüştürebilirsiniz [Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx) Windows PowerShell komutu. Seçin **yönetici olarak çalıştır** PowerShell başlattığınızda. 

Aşağıdaki örnek komut, VHD VHDX ve sabit boyutlu dinamik olarak genişleyen bir diske dönüştürür:

```Powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```
Bu değeri komutta, "-yolu" dönüştürmek istediğiniz sanal sabit disk ve değeri yoluyla "-DestinationPath" dönüştürülen disk adını ve yeni yol ile.

### <a name="convert-from-vmware-vmdk-disk-format"></a>VMware VMDK disk biçiminden Dönüştür
Bir Windows VM görüntüsü varsa [VMDK dosya biçimi](https://en.wikipedia.org/wiki/VMDK), kullanarak bir VHD'ye Dönüştür [Microsoft VM dönüştürücüsü](https://www.microsoft.com/download/details.aspx?id=42497). Daha fazla bilgi için blog makalesine bakın [VMware VMDK Hyper-V VHD'ye dönüştürecek nasıl](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx).

## <a name="set-windows-configurations-for-azure"></a>Azure için Windows yapılandırmalarını ayarlama

Azure için karşıya yüklemeyi planladığınız VM tüm komutları alan aşağıdaki adımları çalıştırmanız bir [yükseltilmiş bir komut istemi penceresi](https://technet.microsoft.com/library/cc947813.aspx):

1. Yönlendirme tablosundaki statik herhangi kalıcı yol kaldırın:
   
   * Yönlendirme tablosunu görüntülemek için Çalıştır `route print` komut isteminde.
   * Denetleme **Kalıcılık yollar** bölümler. Kalıcı bir yol ise kullanın [rota delete](https://technet.microsoft.com/library/cc739598.apx) kaldırmak için.
2. WinHTTP proxy kaldırın:
   
    ```PowerShell
    netsh winhttp reset proxy
    ```
3. Disk SAN ilkesini ayarlamak [Onlineall](https://technet.microsoft.com/library/gg252636.aspx). 
   
    ```PowerShell
    diskpart 
    ```
    Açık bir komut istemi penceresinde aşağıdaki komutları yazın:

     ```DISKPART
    san policy=onlineall
    exit   
    ```

4. Windows ve Windows Saati (w32time) hizmeti başlangıç türü için Eşgüdümlü Evrensel Saat (UTC) saati ayarlayın **otomatik olarak**:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\TimeZoneInformation' -name "RealTimeIsUniversal" 1 -Type DWord

    Set-Service -Name w32time -StartupType Auto
    ```
5. Güç profili kümesine **yüksek performanslı**:

    ```PowerShell
    powercfg /setactive SCHEME_MIN
    ```

## <a name="check-the-windows-services"></a>Windows hizmetlerini denetle
Aşağıdaki Windows hizmetlerinin her biri olarak ayarlandığından emin olun **Windows varsayılan değerlerin**. VM'nin bağlantısı olduğundan emin olmak için ayarlanması gereken hizmetleri en az sayıda bunlar. Başlangıç ayarları sıfırlamak için aşağıdaki komutları çalıştırın:
   
```PowerShell
Set-Service -Name bfe -StartupType Auto
Set-Service -Name dhcp -StartupType Auto
Set-Service -Name dnscache -StartupType Auto
Set-Service -Name IKEEXT -StartupType Auto
Set-Service -Name iphlpsvc -StartupType Auto
Set-Service -Name netlogon -StartupType Manual
Set-Service -Name netman -StartupType Manual
Set-Service -Name nsi -StartupType Auto
Set-Service -Name termService -StartupType Manual
Set-Service -Name MpsSvc -StartupType Auto
Set-Service -Name RemoteRegistry -StartupType Auto
```

## <a name="update-remote-desktop-registry-settings"></a>Uzak Masaüstü kayıt defteri ayarlarını güncelleştirme
Aşağıdaki ayarlar için Uzak Masaüstü Bağlantısı doğru şekilde yapılandırıldığından emin olun:

>[!Note] 
>Programını çalıştırdığınızda, bir hata iletisi alabilirsiniz **kümesi ItemProperty-yolu ' HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Hizmetleri - adı &lt;nesne adı&gt; &lt;değeri&gt;**  aşağıdaki adımları. Hata iletisi güvenle yoksayılabilir. Bu, yalnızca etki alanı Grup İlkesi nesnesi aracılığıyla bu yapılandırma Ftp'den değil anlamına gelir.
>
>

1. Uzak Masaüstü Protokolü (RDP) etkinleştirilir:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server' -name "fDenyTSConnections" -Value 0 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDenyTSConnections" -Value 0 -Type DWord
    ```
   
2. RDP bağlantı noktası doğru ayarlandığından (varsayılan bağlantı noktası 3389):
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "PortNumber" 3389 -Type DWord
    ```
    Bir VM dağıttığınızda, varsayılan kuralları 3389 numaralı bağlantı noktasına göre oluşturulur. Bağlantı noktası numarasını değiştirmek istiyorsanız, Azure'da VM dağıtıldıktan sonra yapın.

3. Her ağ arabiriminde dinleyicisinin dinleme yaptığı:
   
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "LanAdapter" 0 -Type DWord
   ```
4. RDP bağlantıları için ağ düzeyinde kimlik doğrulama modunu yapılandırın:
   
    ```PowerShell
   Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SecurityLayer" 1 -Type DWord

    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "fAllowSecProtocolNegotiation" 1 -Type DWord
     ```

5. Tutma değerini ayarlayın:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveEnable" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "KeepAliveInterval" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "KeepAliveTimeout" 1 -Type DWord
    ```
6. Yeniden bağlanma:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services' -name "fDisableAutoReconnect" 0 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fInheritReconnectSame" 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "fReconnectSame" 0 -Type DWord
    ```
7. Eşzamanlı bağlantı sayısını sınırla:
    
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp' -name "MaxInstanceCount" 4294967295 -Type DWord
    ```
8. RDP dinleyicisini bağlı herhangi bir otomatik olarak imzalanan sertifika varsa, bunları kaldırın:
    
    ```PowerShell
    Remove-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "SSLCertificateSHA1Hash"
    ```
    Bu VM dağıttığınızda başında bağlanabildiğinden emin olmaktır. VM Azure'da gerekirse dağıtıldıktan sonra bu üzerinde bir sonraki aşama gözden geçirebilirsiniz.

9. VM bir etki alanının parçası olacaksa, önceki ayarlar değil döndürülür emin olmak için aşağıdaki tüm ayarları denetleyin. Denetlenmelidir ilkeler şunlardır:
    
    - RDP etkinleştirilir:

         Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Templates\ Bileşenleri\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayarı\Bağlantılar:
         
         **Kullanıcıların Uzak Masaüstü'nü kullanarak uzaktan bağlanmasına izin ver**

    - NLA Grup İlkesi:

        Settings\Administrative Templates\Components\Remote Masaüstü Hizmetleri\Uzak Masaüstü Oturumu Ana Bilgisayarı\Güvenlik: 
        
        **Ağ düzeyinde kimlik doğrulama kullanarak uzak bağlantılar için kullanıcı kimlik doğrulaması gerektirir**
    
    - Canlı ayarları tutun:

        Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Şablonları\Windows Bileşenleri\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayarı\Bağlantılar: 
        
        **Tutma bağlantı aralığını Yapılandır**

    - Ayarları yeniden bağlanın:

        Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Şablonları\Windows Bileşenleri\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayarı\Bağlantılar: 
        
        **Otomatik yeniden bağlanma**

    - Bağlantı ayarlarını sayısı sınırı:

        Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Şablonları\Windows Bileşenleri\Uzak Masaüstü Hizmetleri\Uzak Masaüstü Oturum Ana Bilgisayarı\Bağlantılar: 
        
        **Bağlantı sayısını sınırla**

## <a name="configure-windows-firewall-rules"></a>Windows Güvenlik duvarı kurallarını yapılandırma
1. Windows Güvenlik Duvarı'nı (etki alanı, standart ve genel) üç profilleri açın:

   ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\DomainProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\PublicProfile' -name "EnableFirewall" -Value 1 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\services\SharedAccess\Parameters\FirewallPolicy\Standardprofile' -name "EnableFirewall" -Value 1 -Type DWord
   ```

2. WinRM üç güvenlik duvarı profilleri (etki alanı, özel ve genel) izin vermek ve PowerShell uzaktan hizmetini etkinleştirmek için PowerShell içinde aşağıdaki komutu çalıştırın:
   
   ```PowerShell
    Enable-PSRemoting -force
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
   ```
3. RDP trafiğine izin vermek aşağıdaki güvenlik duvarı kurallarını etkinleştirin 

   ```PowerShell
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes
   ```   
4. VM sanal ağ içindeki bir ping komutuna yanıt vermesini sağlayabilirsiniz dosya ve yazıcı paylaşımı kuralını etkinleştirin:

   ```PowerShell
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes
   ``` 
5. VM bir etki alanının parçası olacaksa, önceki ayarlar değil döndürülür emin olmak için aşağıdaki ayarları denetleyin. Denetlenmelidir AD ilkeler şunlardır:

    - Windows Güvenlik duvarı profillerini etkinleştirme

        Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Güvenlik Duvarı: **tüm ağ bağlantılarını korumaya**

       Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Güvenlik Duvarı: **tüm ağ bağlantılarını korumaya**

    - RDP etkinleştir 

        Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Güvenlik Duvarı: **gelen Uzak Masaüstü özel durumlara izin ver**

        Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Güvenlik Duvarı: **gelen Uzak Masaüstü özel durumlara izin ver**

    - ICMP V4 etkinleştir

        Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Domain Profile\Windows Güvenlik Duvarı: **ICMP özel durumlarına izin ver**

        Bilgisayar Yapılandırması\İlkeler\Windows Settings\Administrative Templates\Network\Network Connection\Windows Firewall\Standard Profile\Windows Güvenlik Duvarı: **ICMP özel durumlarına izin ver**

## <a name="verify-vm-is-healthy-secure-and-accessible-with-rdp"></a>VM'yi sağlıklı, güvenli ve RDP ile erişilebilir olduğunu doğrulayın 
1. Disk sağlıklı ve tutarlı olduğundan emin olmak için sonraki VM yeniden başlatma sırasında bir onay disk işlemini çalıştırın:

    ```PowerShell
    Chkdsk /f
    ```
    Raporun disk temiz ve Sağlıklı gösterdiğinden emin olun.

2. Önyükleme yapılandırma verileri (BCD) ayarlar. 

    > [!Note]
    > Yükseltilmiş bir komut penceresinde bu komutları çalıştırdığınızdan emin olun ve **değil** PowerShell:
   
   ```CMD
   bcdedit /set {bootmgr} integrityservices enable
   
   bcdedit /set {default} device partition=C:
   
   bcdedit /set {default} integrityservices enable
   
   bcdedit /set {default} recoveryenabled Off
   
   bcdedit /set {default} osdevice partition=C:
   
   bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
   ```
3. Windows Yönetim Alt Yapısı'nın deposunun tutarlı olduğunu doğrulayın. Bunu gerçekleştirmek için aşağıdaki komutu çalıştırın:

    ```PowerShell
    winmgmt /verifyrepository
    ```
    Deposu bozuk olup [WMI: deposu bozulma veya](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

4. Başka bir uygulama 3389 numaralı bağlantı noktası kullanmadığından emin olun. Bu bağlantı noktası Azure RDP hizmetinde için kullanılır. Çalıştırabilirsiniz **netstat - anob** VM hangi bağlantı noktalarının kullanılacağını görmek için:

    ```PowerShell
    netstat -anob
    ```

5. Karşıya yüklemek istediğiniz Windows VHD bir etki alanı denetleyicisi ise, şu adımları izleyin:

    A. İzleyin [bu ek adımları](https://support.microsoft.com/kb/2904015) disk hazırlamak için.

    B. VM, belirli bir noktada DSRM modunda başlatmak gerekirse DSRM parolasını bildiğinizden emin olun. Bu bağlantıyı ayarlamak için başvurmak isteyebilirsiniz [DSRM parolasını](https://technet.microsoft.com/library/cc754363(v=ws.11).aspx).

6. Yerleşik yönetici hesabı ve parolası, bilinir emin olun. Geçerli yerel yönetici parolasını sıfırlama ve RDP bağlantısı üzerinden Windows'da oturum açmak için bu hesabı kullanabilirsiniz emin olmak isteyebilirsiniz. Bu erişim izni "oturum Uzak Masaüstü Hizmetleri üzerinden izin ver" Grup İlkesi nesnesi tarafından denetlenir. Bu nesne yerel Grup İlkesi Düzenleyicisi'nde altında görüntüleyebilirsiniz:

    Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Ataması

7. RDP aracılığıyla veya ağ üzerinden RDP erişiminizi engellemesi değil emin olmak için aşağıdaki AD ilkeleri denetleyin:

    - Bu bilgisayara ağdan Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Assignment\Deny erişimi

    - Uzak Masaüstü Hizmetleri üzerinden Bilgisayar Yapılandırması\Windows Ayarları\Güvenlik Ayarları\Yerel İlkeler\Kullanıcı Hakları Assignment\Deny oturum açma


8. Yeniden başlatma Windows hala sağlıklı olduğundan emin olmak için VM RDP bağlantı kullanılarak erişilebilir. Bu noktada, yerel Hyper-VM tamamen başlangıç emin olun ve sonra RDP erişilebilir olup olmadığını test etmek için V'de VM oluşturmak isteyebilirsiniz.

9. TCP çözümler yazılım gibi ek bir aktarım sürücüsü arabirimi filtreleri, kaldırma paketler ya da ek güvenlik duvarları. VM Azure'da gerekirse dağıtıldıktan sonra bu üzerinde bir sonraki aşama gözden geçirebilirsiniz.

10. Herhangi bir üçüncü taraf yazılım ve fiziksel bileşenler veya başka bir sanallaştırma teknolojisi ilgili sürücü kaldırın.

### <a name="install-windows-updates"></a>Windows güncelleştirmelerini yükleme
İdeal yapılandırma **makineyi en son düzeltme eki düzeyine sahip**. Bu mümkün değilse, aşağıdaki güncelleştirmelerin yüklü olduğundan emin olun:

|                       |                   |           |                                       En düşük dosya sürümü x64       |                                      |                                      |                            |
|-------------------------|-------------------|------------------------------------|---------------------------------------------|--------------------------------------|--------------------------------------|----------------------------|
| Bileşen               | İkili            | Windows 7 ve Windows Server 2008 R2 | Windows 8 ve Windows Server 2012             | Windows 8.1 ve Windows Server 2012 R2 | Windows 10 ve Windows Server 2016 RS1 | Windows 10 RS2             |
| Depolama                 | disk.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17638 / 6.2.9200.21757 - KB3137061 | 6.3.9600.18203 - KB3137061           | -                                    | -                          |
|                         | Storport.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17188 / 6.2.9200.21306 - KB3018489 | 6.3.9600.18573 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.332             |
|                         | NTFS.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17623 / 6.2.9200.21743 - KB3121255 | 6.3.9600.18654 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.447             |
|                         | Iologmsg.dll      | 6.1.7601.23403 - KB3125574         | 6.2.9200.16384 - KB2995387                  | -                                    | -                                    | -                          |
|                         | Classpnp.sys      | 6.1.7601.23403 - KB3125574         | 6.2.9200.17061 / 6.2.9200.21180 - KB2995387 | 6.3.9600.18334 - KB3172614           | 10.0.14393.953 - KB4022715           | -                          |
|                         | Volsnap.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17047 / 6.2.9200.21165 - KB2975331 | 6.3.9600.18265 - KB3145384           | -                                    | 10.0.15063.0               |
|                         | Partmgr.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.16681 - KB2877114                  | 6.3.9600.17401 - KB3000850           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | volmgr.sys        |                                    |                                             |                                      |                                      | 10.0.15063.0               |
|                         | Volmgrx.sys       | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | 10.0.15063.0               |
|                         | Msiscsi.sys       | 6.1.7601.23403 - KB3125574         | 6.2.9200.21006 - KB2955163                  | 6.3.9600.18624 - KB4022726           | 10.0.14393.1066 - KB4022715          | 10.0.15063.447             |
|                         | MSDSM.sys         | 6.1.7601.23403 - KB3125574         | 6.2.9200.21474 - KB3046101                  | 6.3.9600.18592 - KB4022726           | -                                    | -                          |
|                         | Mpio.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.21190 - KB3046101                  | 6.3.9600.18616 - KB4022726           | 10.0.14393.1198 - KB4022715          | -                          |
|                         | Fveapi.dll        | 6.1.7601.23311 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.18294 - KB3172614           | 10.0.14393.576 - KB4022715           | -                          |
|                         | Fveapibase.dll    | 6.1.7601.23403 - KB3125574         | 6.2.9200.20930 - KB2930244                  | 6.3.9600.17415 - KB3172614           | 10.0.14393.206 - KB4022715           | -                          |
| Ağ                 | netvsc.sys        | -                                  | -                                           | -                                    | 10.0.14393.1198 - KB4022715          | 10.0.15063.250 - KB4020001 |
|                         | mrxsmb10.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.22108 - KB4022724                  | 6.3.9600.18603 - KB4022726           | 10.0.14393.479 - KB4022715           | 10.0.15063.483             |
|                         | mrxsmb20.sys      | 6.1.7601.23816 - KB4022722         | 6.2.9200.21548 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.483             |
|                         | Mrxsmb.sys        | 6.1.7601.23816 - KB4022722         | 6.2.9200.22074 - KB4022724                  | 6.3.9600.18586 - KB4022726           | 10.0.14393.953 - KB4022715           | 10.0.15063.0               |
|                         | Tcpip.sys         | 6.1.7601.23761 - KB4022722         | 6.2.9200.22070 - KB4022724                  | 6.3.9600.18478 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.447             |
|                         | HTTP.sys          | 6.1.7601.23403 - KB3125574         | 6.2.9200.17285 - KB3042553                  | 6.3.9600.18574 - KB4022726           | 10.0.14393.251 - KB4022715           | 10.0.15063.483             |
|                         | vmswitch.sys      | 6.1.7601.23727 - KB4022719         | 6.2.9200.22117 - KB4022724                  | 6.3.9600.18654 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.138             |
| Çekirdek                    | Ntoskrnl.exe      | 6.1.7601.23807 - KB4022719         | 6.2.9200.22170 - KB4022718                  | 6.3.9600.18696 - KB4022726           | 10.0.14393.1358 - KB4022715          | 10.0.15063.483             |
| Uzak Masaüstü Hizmetleri | rdpcorets.dll     | 6.2.9200.21506 - KB4022719         | 6.2.9200.22104 - KB4022724                  | 6.3.9600.18619 - KB4022726           | 10.0.14393.1198 - KB4022715          | 10.0.15063.0               |
|                         | Termsrv.dll       | 6.1.7601.23403 - KB3125574         | 6.2.9200.17048 - KB2973501                  | 6.3.9600.17415 - KB3000850           | 10.0.14393.0 - KB4022715             | 10.0.15063.0               |
|                         | termdd.sys        | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Win32k.sys        | 6.1.7601.23807 - KB4022719         | 6.2.9200.22168 - KB4022718                  | 6.3.9600.18698 - KB4022726           | 10.0.14393.594 - KB4022715           | -                          |
|                         | Rdpdd.dll         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
|                         | Rdpwd.sys         | 6.1.7601.23403 - KB3125574         | -                                           | -                                    | -                                    | -                          |
| Güvenlik                | WannaCrypt nedeniyle | KB4012212                          | KB4012213                                   | KB4012213                            | KB4012606                            | KB4012606                  |
|                         |                   |                                    | KB4012216                                   |                                      | KB4013198                            | KB4013198                  |
|                         |                   | KB4012215                          | KB4012214                                   | KB4012216                            | KB4013429                            | KB4013429                  |
|                         |                   |                                    | KB4012217                                   |                                      | KB4013429                            | KB4013429                  |
       
### Sysprep kullanma zamanı<a id="step23"></a>    

Sysprep, sistem yüklemesini sıfırlar ve bir "kutu dışı deneyimi" tüm kişisel verilerini kaldırma ve çeşitli bileşenleri sıfırlama sağlayacak bir windows yüklemesinden içine çalışabilecek bir işlemdir. Belirli bir yapılandırmaya sahip birden fazla diğer sanal makineleri dağıtmak bir şablon oluşturmak istiyorsanız, genellikle bunu. Bu adlı bir **görüntü genelleştirilmiş**.

Bunun yerine, yalnızca bir disk, bir VM oluşturmak istiyorsanız sysprep kullanmak zorunda değilsiniz. Bu durumda, yalnızca VM olarak Bilineni gelen oluşturabileceğiniz bir **özel görüntü**.

Özelleştirilmiş bir diskten VM oluşturma hakkında daha fazla bilgi için bkz:

- [Özelleştirilmiş bir diskten VM oluşturma](create-vm-specialized.md)
- [Özel bir VHD diskten bir VM oluşturma](https://azure.microsoft.com/resources/templates/201-vm-specialized-vhd/)

Genelleştirilmiş bir yansıma oluşturmak istiyorsanız, sysprep çalıştırmanız gerekir. Sysprep hakkında daha fazla bilgi için bkz: [kullanım Sysprep nasıl: Giriş](http://technet.microsoft.com/library/bb457073.aspx). 

Her rol ya da Windows tabanlı bir bilgisayarda yüklü uygulamayı bu Genelleştirme destekler. Bu yordamı çalıştırmadan önce bu nedenle, bu bilgisayarın rolü sysprep tarafından desteklendiğinden emin olmak için aşağıdaki makaleye bakın. Daha fazla bilgi için [sunucu rolleri için Sysprep desteği](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

### <a name="steps-to-generalize-a-vhd"></a>Bir VHD genelleştirmek için adımları

>[!NOTE]
> Aşağıdaki adımlarda belirtilen sysprep.exe çalıştırdıktan sonra VM'yi kapatmayın ve görüntünün ondan Azure içinde oluşturduğunuz kadar yeniden açmak değil.

1. Windows VM oturum açın.
2. Çalıştırma **komut istemi** yönetici olarak. 
3. Dizine geçin: **%windir%\system32\sysprep**ve ardından çalıştırın **sysprep.exe**.
3. İçinde **Sistem Hazırlama aracı** iletişim kutusunda **girin sistem Out-of-Box deneyimi (OOBE)**, emin olun **Generalize** onay kutusu seçilidir.

    ![Sistem Hazırlama Aracı](media/prepare-for-upload-vhd-image/syspre.png)
4. İçinde **kapatma seçenekleri**seçin **kapatma**.
5. **Tamam** düğmesine tıklayın.
6. Sysprep tamamlandığında VM kapatma. Kullanmayın **yeniden** VM kapatma için.
7. Artık VHD yüklenebilmesi hazırdır. Genelleştirilmiş bir diskten VM oluşturma hakkında daha fazla bilgi için bkz: [genelleştirilmiş bir VHD yüklemek ve yeni sanal makineleri oluşturmak için kullanın](sa-upload-generalized.md).


## <a name="complete-recommended-configurations"></a>Önerilen yapılandırmaları tamamlayın
Aşağıdaki ayarlar, VHD karşıya etkilemez. Ancak, bunları yapılandırılmış öneririz.

* Yükleme [Azure VM Aracısı](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Ardından, VM uzantıları etkinleştirebilirsiniz. VM uzantıları, RDP yapılandırma, parola sıfırlama gibi VM ile birlikte kullanmak istiyorsanız ve benzeri kritik işlevselliğinin büyük kısmını uygulayın. Daha fazla bilgi için bkz.

    - [VM aracısı ve uzantılar – bölüm 1](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-1/)
    - [VM aracısı ve uzantılar – bölüm 2](https://azure.microsoft.com/blog/vm-agent-and-extensions-part-2/)
* Döküm günlük Windows kilitlenme sorunları gidermenize yardımcı olabilir. Döküm günlük toplama etkinleştirin:
  
    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "AutoReboot" -Value 0 -Type DWord
    New-Item -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps'
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    New-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
    Bu makalede yordam adımları sırasında herhangi bir hata alırsanız, bu kayıt defteri anahtarlarını zaten anlamına gelir. Bu durumda, bunun yerine aşağıdaki komutları kullanın:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "CrashDumpEnable" -Value "2" -Type DWord
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\CrashControl' -name "DumpFile" -Value "%SystemRoot%\MEMORY.DMP"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpFolder" -Value "c:\CrashDumps"
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpCount" -Value 10 -Type DWord
    Set-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps' -name "DumpType" -Value 2 -Type DWord
    Set-Service -Name WerSvc -StartupType Manual
    ```
*  Azure'da VM oluşturulduktan sonra performansı artırmak için "Zamana bağlı sürücüsü" birim üzerinde disk belleği dosyasını put öneririz. Bu aşağıdaki gibi ayarlayabilirsiniz:

    ```PowerShell
    Set-ItemProperty -Path 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management' -name "PagingFiles" -Value "D:\pagefile"
    ```
Zamana bağlı sürücü birimin sürücü harfini VM'ye bağlı herhangi bir veri disk ise, genellikle "D". Bu atama kullanılabilir sürücü sayısını ve yaptığınız ayarları bağlı olarak farklı olabilir.

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Windows VM görüntüsü için Azure Resource Manager dağıtımları için karşıya yükleme](upload-generalized-managed.md)
* [Azure Windows sanal makine etkinleştirme sorunlarını giderme](troubleshoot-activation-problems.md)

