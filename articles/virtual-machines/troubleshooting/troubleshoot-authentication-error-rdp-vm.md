---
title: Azure VM'ye bağlanmak için RDP kullandığınızda, kimlik doğrulaması sorunlarını giderme | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 11/01/2018
ms.author: delhan
ms.openlocfilehash: 47d3b827099d3a4a7520ac66765d2928795b6e49
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60594917"
---
# <a name="troubleshoot-authentication-errors-when-you-use-rdp-to-connect-to-azure-vm"></a>Azure VM'ye bağlanmak için RDP kullandığınızda, kimlik doğrulama hatalarını giderme

Bu makalede, bir Azure sanal makinesine (VM) bağlamak için Uzak Masaüstü Protokolü (RDP) bağlantı kullanırken oluşan kimlik doğrulama hatalarını giderme yardımcı olabilir.

## <a name="symptoms"></a>Belirtiler

Karşılama ekranı gösterilir ve işletim sisteminin çalıştığını gösteren bir Azure VM görüntüsü yakalayın. VM'ye Uzak Masaüstü bağlantısı kullanarak bağlanmaya çalıştığınızda, ancak aşağıdaki hata iletilerinden birini alırsınız.

### <a name="error-message-1"></a>1\. hata iletisi

**Bir kimlik doğrulama hatası oluştu. Yerel Güvenlik Yetkilisi iletişim kurulamıyor.**

### <a name="error-message-2"></a>Hata iletisi 2

**Gerektirir ağ düzeyi kimlik doğrulama (NLA) bağlanmaya çalışıyorsunuz, ancak NLA gerçekleştirmek için Windows etki alanı denetleyicinizi temas kurulamıyor uzak bilgisayar. Uzak bilgisayardaki bir yöneticiyseniz Uzak sekmesinde Sistem Özellikleri iletişim kutusu seçenekleri kullanarak NLA devre dışı bırakabilirsiniz.**

### <a name="error-message-3-generic-connection-error"></a>Hata iletisi 3 (genel bağlantı hatası)

**Bu bilgisayar, uzak bilgisayara bağlanamıyor. Sorun devam ederse, yeniden bağlanmayı deneyin, sahibi, uzak bilgisayar ya da ağ yöneticinize başvurun.**

## <a name="cause"></a>Nedeni

Neden NLA VM'ye RDP erişimini engelleyebilecek birden çok neden vardır.

### <a name="cause-1"></a>Neden 1

Sanal etki alanı denetleyicisi (DC) ile iletişim kuramıyor. Bu sorun, bir RDP oturumu etki alanı kimlik bilgilerini kullanarak bir VM erişmesini engelleyebilir. Ancak, yine de yerel yönetici kimlik bilgilerini kullanarak oturum açmak hazırdır. Bu sorun aşağıdaki durumlarda oluşabilir:

1. Bu VM ve DC arasında Active Directory güvenlik kanalı bozulur.

2. Sanal makine hesabı parola eski bir kopyasını ve daha yeni bir kopya DC sahiptir.

3. Bu VM'nin bağlandığı etki alanı denetleyicisi, sağlam değil.

### <a name="cause-2"></a>Neden 2

VM şifreleme düzeyini, istemci bilgisayar tarafından kullanılan daha yüksektir.

### <a name="cause-3"></a>Neden 3

TLS 1.0, 1.1 ve 1.2 (sunucu) protokolleri, sanal makinede devre dışı bırakıldı.

### <a name="cause-4"></a>Neden 4

Etki alanı kimlik bilgilerini kullanarak oturum açma devre dışı bırakmak için VM ayarlanmıştır ve yerel güvenlik yetkilisi (LSA) yanlış ayarlanır.

### <a name="cause-5"></a>Neden 5

VM yalnızca Federal Bilgi İşleme Standardı (FIPS) kabul etmek üzere ayarlandığı-uyumlu algoritma bağlantıları. Bu durum genellikle Active Directory ilkesi kullanılarak gerçekleştirilir. Bu nadir bir yapılandırmadır ancak FIPS yalnızca Uzak Masaüstü bağlantıları için zorunlu tutulabilir.

## <a name="before-you-troubleshoot"></a>Sorun giderme önce

### <a name="create-a-backup-snapshot"></a>Bir yedek anlık görüntüsü oluşturma

Bir yedek anlık görüntüsü oluşturmak için adımları [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

### <a name="connect-to-the-vm-remotely"></a>VM'ye uzaktan bağlanın

VM'ye uzaktan bağlanmak için metotlarından birini kullanın [Azure VM sorunlarını gidermek için Uzak Araçlar'ı kullanma](remote-tools-troubleshoot-azure-vm-issues.md).

### <a name="group-policy-client-service"></a>Grup İlkesi istemci hizmeti

Bu etki alanına katılmış bir VM ise, önce tüm Active Directory ilkesi değişikliklerin üzerine yazmasını engellemek için Grup İlkesi İstemcisi hizmeti durdurun. Bunu yapmak için aşağıdaki komutu çalıştırın:

```cmd
REM Disable the member server to retrieve the latest GPO from the domain upon start
REG add "HKLM\SYSTEM\CurrentControlSet\Services\gpsvc" /v Start /t REG_DWORD /d 4 /f
```

Sorun düzeltildikten sonra bu VM'yi en son GPO etki alanından almak için etki alanıyla bağlantı olanağı geri yükleyin. Bunu yapmak için aşağıdaki komutları çalıştırın:

```cmd
sc config gpsvc start= auto
sc start gpsvc

gpupdate /force
```

Değişiklik geri alınır, bir Active Directory ilkesini soruna neden olan anlamına gelir. 

### <a name="workaround"></a>Geçici Çözüm

Bu sorunu çözmek için NLA devre dışı bırakmak için komut penceresinde aşağıdaki komutları çalıştırın:

```cmd
REM Disable the Network Level Authentication
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 0
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 0
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 0
```

Ardından, VM'yi yeniden başlatın.

NLA yeniden etkinleştirmek için aşağıdaki komutu çalıştırın ve ardından VM'yi yeniden başlatın:

```cmd
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v disabledomaincreds /t REG_DWORD /d 0 /f

REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD /d 1 /f
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD /d 1 /f
REG add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD /d 1 /f
```

## <a name="troubleshooting"></a>Sorun giderme

### <a name="for-domain-joined-vms"></a>Etki alanına katılmış sanal makineleri için

Bu sorunu gidermek için ilk VM için bir DC bağlanabilir ve DC durumu "iyi" olur ve işleyebilir VM'den istekleri olup olmadığını denetleyin.

>[!Note] 
>DC sistem test etmek için başka bir VM aynı sanal ağ ve Paylaşım aynı oturum açma sunucusu alt ağı kullanabilirsiniz.

Seri konsol veya uzak CMD "VM'ye uzaktan bağlanın" bölümündeki adımlara göre uzak PowerShell kullanarak sorunlu VM bağlanın.

VM'nin bağlandığı hangi DC belirlemek için konsolda aşağıdaki komutu çalıştırın: 

```cmd
set | find /i "LOGONSERVER"
```

Ardından, sanal makine ve etki alanı denetleyicisi arasında güvenli bir kanal durumunu denetleyin. Bunu yapmak için yükseltilmiş bir PowerShell örneğinde aşağıdaki komutu çalıştırın. Bu komut, güvenli kanal Canlı olup olmadığını belirten bir Boole bayrağı döndürür:

```powershell
Test-ComputerSecureChannel -verbose
```

Kanal bozuk, onarmak için aşağıdaki komutu çalıştırın:

```powershell
Test-ComputerSecureChannel -repair
```

VM ve etki alanı denetleyicisi bilgisayar hesabı parolasını Active Directory'de güncelleştirildiğinden emin olun:

```powershell
Reset-ComputerMachinePassword -Server "<COMPUTERNAME>" -Credential <DOMAIN CREDENTIAL WITH DOMAIN ADMIN LEVEL>
```

DC ve VM arasındaki iletişimi iyidir ancak DC bir RDP oturumu açmak için yeterince sağlıklı değil, etki alanı denetleyicisi yeniden başlatmayı deneyebilirsiniz.

Yukarıdaki komutların etki iletişim sorunu gidermediyse bu VM'nin etki alanına katılabilir. Bunu yapmak için şu adımları uygulayın:

1. Aşağıdaki içeriğe kullanarak Unjoin.ps1 adlı bir komut dosyası oluşturun ve ardından Azure portalında bir özel betik uzantısı olarak betik dağıtın:

    ```cmd
    cmd /c "netdom remove <<MachineName>> /domain:<<DomainName>> /userD:<<DomainAdminhere>> /passwordD:<<PasswordHere>> /reboot:10 /Force"
    ```
    
    Bu betik, sanal Makinenin etki alanı dışına zorla sürer ve 10 saniye sonra yeniden başlatır. Ardından, etki alanı tarafında bilgisayar nesnesi temizlemek gerekir.

2.  Temizleme işlemini gerçekleştirdikten sonra bu VM'nin etki alanına katın. Bunu yapmak için aşağıdaki içeriğe kullanarak JoinDomain.ps1 adlı bir komut dosyası oluşturabilir ve ardından Azure portalında bir özel betik uzantısı olarak betik dağıtın: 

    ```cmd
    cmd /c "netdom join <<MachineName>> /domain:<<DomainName>> /userD:<<DomainAdminhere>> /passwordD:<<PasswordHere>> /reboot:10"
    ```

    >[!Note] 
    >Belirtilen kimlik bilgilerini kullanarak bu VM bir etki alanı üzerinde birleştirir.

Active Directory kanal kötü durumda, bilgisayar parola güncelleştirilir ve etki alanı denetleyicisi beklendiği gibi çalıştığından, aşağıdaki adımları deneyin.

Sorun devam ederse, etki alanı kimlik bilgileri devre dışı bırakılıp bırakılmadığını kontrol edin. Bunu yapmak için yükseltilmiş bir komut istemi penceresi açın ve ardından VM VM oturum açmak için etki alanı hesapları devre dışı bırakmak için kurulup kurulmadığını belirlemek için aşağıdaki komutu çalıştırın:

```cmd
REG query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v disabledomaincreds
```

Anahtar ayarlanırsa **1**, bu sunucunun ayarlandı anlamına gelir. etki alanı kimlik bilgilerini izin vermeyecek şekilde. Bu anahtar için değiştirme **0**.

### <a name="for-standalone-vms"></a>Tek başına VM'ler için

#### <a name="check-minencryptionlevel"></a>MinEncryptionLevel denetleyin

CMD örneği sorgulamak için aşağıdaki komutu çalıştırarak **MinEncryptionLevel** kayıt defteri değeri:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel
```

Kayıt defteri değerine göre aşağıdaki adımları izleyin:

* 4 (FIPS): Git [denetleyin FIPS uyumlu algoritmaları bağlantıları](#fips-compliant).

* 3 (128 bit şifreleme): Önem derecesi kümesine **2** aşağıdaki komutu çalıştırarak:

    ```cmd
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 2 /f
    ```

* 2 (istemci tarafından belirlenen en yüksek şifrelemeyi mümkün): Şifreleme için en küçük değerini ayarlamak deneyebilirsiniz **1** aşağıdaki komutu çalıştırarak:

    ```cmd
    reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 1 /f
    ```
    
Kayıt defterindeki değişiklikleri etkili olacak şekilde sanal Makineyi yeniden başlatın.

#### <a name="tls-version"></a>TLS sürümü

Sistemine bağlı olarak, RDP TLS 1.0, 1.1 ve 1.2 (sunucu) protokolünü kullanır. Bu protokollerin VM üzerinde nasıl ayarlanır sorgulamak için CMD örneği açın ve ardından aşağıdaki komutları çalıştırın:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled
```

Döndürülen değerler tüm değilse **1**, bu protokolü devre dışı olduğu anlamına gelir. Bu protokollerin etkinleştirmek için aşağıdaki komutları çalıştırın:

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.0\Server" /v Enabled /t REG_DWORD /d 1 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.1\Server" /v Enabled /t REG_DWORD /d 1 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS 1.2\Server" /v Enabled /t REG_DWORD /d 1 /f
```

Diğer protokol sürümleri için aşağıdaki komutları çalıştırabilirsiniz:

<pre lang="bat">
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS <i>x.x</i>\Server" /v Enabled
reg query "HKLM\SYSTEM\CurrentControlSet\Control\SecurityProviders\SCHANNEL\Protocols\TLS <i>x.x</i>\Server" /v Enabled
</pre>

> [!Note]
> Konuk işletim sistemi günlüklerinde SCHANNEL hataları SSH/TLS sürümü x.x alın.

#### <a name="fips-compliant"></a> FIPS uyumlu algoritmaları bağlantılarını denetleyin

Uzak Masaüstü FIPS uyumlu algoritma bağlantılarını kullanmasını zorunlu tutulabilir. Bu, bir kayıt defteri anahtarı kullanılarak ayarlanabilir. Bunu yapmak için yükseltilmiş bir komut istemi penceresi açın ve sonra aşağıdaki anahtarları sorgu:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy" /v Enabled
```

Komut döndürürse **1**, kayıt defteri değerini değiştirin **0**.

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Lsa\FIPSAlgorithmPolicy" /v Enabled /t REG_DWORD /d 0
```

Sanal makinedeki geçerli MinEncryptionLevel olan denetimi:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel
```

Komut döndürürse **4**, kayıt defteri değerini değiştirin **2**

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MinEncryptionLevel /t REG_DWORD /d 2
```

Kayıt defterindeki değişiklikleri etkili olacak şekilde sanal Makineyi yeniden başlatın.

## <a name="next-steps"></a>Sonraki adımlar

[Win32_TSGeneralSetting sınıfının SetEncryptionLevel yöntemi](https://docs.microsoft.com/windows/desktop/TermServ/win32-tsgeneralsetting-setencryptionlevel)

[Sunucu kimlik doğrulaması ve şifreleme düzeyleri yapılandırın](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770833(v=ws.11))

[Win32_TSGeneralSetting sınıfı](https://docs.microsoft.com/windows/desktop/TermServ/win32-tsgeneralsetting)
