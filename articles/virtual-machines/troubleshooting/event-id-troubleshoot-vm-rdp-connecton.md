---
title: Olay Kimliğine göre Azure VM RDP bağlantı sorunlarını giderme | Microsoft Docs
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
ms.openlocfilehash: 4c783c70217a84bbe5ccf15accc4a2bec0b7cca8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61485501"
---
# <a name="troubleshoot-azure-vm-rdp-connection-issues-by-event-id"></a>Azure VM RDP bağlantı sorunlarını olay kimliğine göre giderme 

Bu makale, Uzak Masaüstü Protokolü (RDP) bağlantısını için Azure sanal makinesi (VM) engelleyen sorunları gidermek için olay kimlikleri kullanmayı açıklar.

## <a name="symptoms"></a>Belirtiler

Bir Azure VM'ye Uzak Masaüstü Protokolü (RDP) oturumu kullanmayı deneyin. Kimlik bilgilerinizi girdikten sonra bağlantı başarısız olur ve aşağıdaki hata iletisini alıyorsunuz:

**Bu bilgisayar, uzak bilgisayara bağlanamıyor. Sorun devam ederse, yeniden bağlanmayı deneyin, sahibi, uzak bilgisayar ya da ağ yöneticinize başvurun.**

Bu sorunu gidermek için VM olay günlüklerini gözden geçirin ve ardından aşağıdaki senaryolara bakın.

## <a name="before-you-troubleshoot"></a>Sorun giderme önce

### <a name="create-a-backup-snapshot"></a>Bir yedek anlık görüntüsü oluşturma

Bir yedek anlık görüntüsü oluşturmak için adımları [bir diskin anlık görüntüsünü alma](../windows/snapshot-copy-managed-disk.md).

### <a name="connect-to-the-vm-remotely"></a>VM'ye uzaktan bağlanın

VM'ye uzaktan bağlanmak için metotlarından birini kullanın [Azure VM sorunlarını gidermek için Uzak Araçlar'ı kullanma](remote-tools-troubleshoot-azure-vm-issues.md).

## <a name="scenario-1"></a>Senaryo 1

### <a name="event-logs"></a>Olay günlükleri

Bir CMD örneğinde 1058 veya olayın 1057 sistem günlüğünde son 24 saat içinde günlüğe kaydedilip kaydedilmediğini kontrol etmek için aşağıdaki komutları çalıştırın:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-TerminalServices-RemoteConnectionManager'] and EventID=1058 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Microsoft-Windows-TerminalServices-RemoteConnectionManager'] and EventID=1057 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Günlük adı:**      Sistem <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Tarih:***zaman* <br />
**Olay Kimliği:**      1058 <br />
**Görev kategorisi:** None <br />
**Düzeyi:**         Hata <br />
**Anahtar sözcükler:**      Klasik <br />
**Kullanıcı:**          Yok <br />
**Bilgisayar:***bilgisayar* <br />
**Açıklama:** RD Oturumu Ana bilgisayarı, kendinden imzalı SSL bağlantıları RD Oturumu Ana Bilgisayar sunucusu kimlik doğrulaması için kullanılan sertifikanın süresi dolmuş değiştirmek başarısız oldu. İlgili durum kodu: erişim engellendi.

**Günlük adı:**      Sistem <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Tarih:***zaman* <br />
**Olay Kimliği:**      1058 <br />
**Görev kategorisi:** None <br />
**Düzeyi:**         Hata <br />
**Anahtar sözcükler:**      Klasik <br />
**Kullanıcı:**          Yok <br />
**Bilgisayar:***bilgisayar* <br />
**Açıklama:** RD Oturumu Ana Bilgisayar sunucusu SSL bağlantıları üzerinde RD Oturumu Ana bilgisayarı sunucu kimlik doğrulaması için kullanılacak yeni bir otomatik olarak imzalanan sertifika oluşturmak başarısız oldu, ilgili durum kodu: nesne zaten mevcut.

**Günlük adı:**      Sistem <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-RemoteConnectionManager <br />
**Tarih:***zaman* <br />
**Olay Kimliği:**      1057 <br />
**Görev kategorisi:** None <br />
**Düzeyi:**         Hata <br />
**Anahtar sözcükler:**      Klasik <br />
**Kullanıcı:**          Yok <br />
**Bilgisayar:***bilgisayar* <br />
**Açıklama:** RD Oturumu Ana bilgisayarı, yeni imzalanan SSL bağlantıları üzerinde RD Oturumu Ana Bilgisayar sunucusu kimlik doğrulaması için kullanılacak bir sertifika oluşturmak başarısız oldu. İlgili durum kodu: anahtar kümesi yok.

Aşağıdaki komutları çalıştırarak 36872 ve 36870 SCHANNEL hata olayları için de göz atabilirsiniz:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36870 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36872 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Günlük adı:**      Sistem <br />
**Kaynak:**        Schannel <br />
**Tarih:** — <br />
**Olay Kimliği:**      36870 <br />
**Görev kategorisi:** None <br />
**Düzeyi:**         Hata <br />
**Anahtar sözcükler:**       <br />
**Kullanıcı:**          SİSTEM <br />
**Bilgisayar:***bilgisayar* <br />
**Açıklama:** SSL sunucu kimlik bilgisi özel anahtarına erişme girişimi sırasında önemli bir hata oluştu. Şifreleme modülünden döndürülen hata kodu 0x8009030D ' dir.  <br />
10001 iç bir hata durumudur.

### <a name="cause"></a>Nedeni
VM üzerindeki MachineKeys klasörü yerel RSA şifreleme anahtarları erişilemediğinden bu sorun oluşur. Bu sorun aşağıdaki nedenlerden birinden dolayı ortaya çıkabilir:

1. Yanlış izin yapılandırmayı Machinekeys klasörü veya RSA dosyaları.

2. RSA anahtarı bozuk veya eksik.

### <a name="resolution"></a>Çözüm

Bu sorunu gidermek için aşağıdaki adımları kullanarak RDP sertifikası doğru izinleri ayarlamanız gerekir.

#### <a name="grant-permission-to-the-machinekeys-folder"></a>MachineKeys klasörüne izin ver

1. Aşağıdaki içeriğe kullanarak bir komut dosyası oluşturun:

   ```powershell
   remove-module psreadline 
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\BeforeScript_permissions.txt
   takeown /f "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys" /a /r
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\System:(F)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "NT AUTHORITY\NETWORK SERVICE:(R)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c /grant "BUILTIN\Administrators:(F)"
   icacls C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys /t /c > c:\temp\AfterScript_permissions.txt
   Restart-Service TermService -Force
   ```

2.  MachineKey klasörün izinlerini sıfırlanır ve RSA dosyaları varsayılan değerlere sıfırlamak için bu betiği çalıştırın.

3.  VM yeniden erişmeyi deneyin.

Betiği çalıştırdıktan sonra izin sorunları yaşıyorsanız aşağıdaki dosyaları denetleyebilirsiniz:

* c:\temp\BeforeScript_permissions.txt
* c:\temp\AfterScript_permissions.txt

#### <a name="renew-rdp-self-signed-certificate"></a>RDP otomatik olarak imzalanan sertifikasını yenileme

Sorun devam ederse RDP otomatik imzalı sertifikanın yenilendiğini emin olmak için aşağıdaki betiği çalıştırın:

```powershell
Import-Module PKI
Set-Location Cert:\LocalMachine
$RdpCertThumbprint = 'Cert:\LocalMachine\Remote Desktop\'+((Get-ChildItem -Path 'Cert:\LocalMachine\Remote Desktop\').thumbprint)
Remove-Item -Path $RdpCertThumbprint
Stop-Service -Name "SessionEnv"
Start-Service -Name "SessionEnv"
```

Sertifika yenileyemezsiniz, sertifikayı silmek için aşağıdaki adımları izleyin:

1. Aynı vnet'teki başka bir VM üzerinde açın **çalıştırma** kutusuna **mmc**ve tuşuna **Tamam**. 

2. Üzerinde **dosya** menüsünde **Ekle/Kaldır ek bileşenini**.

3. İçinde **kullanılabilir ek bileşenler** listesinden **sertifikaları**ve ardından **Ekle**.

4. Seçin **bilgisayar hesabı**ve ardından **sonraki**.

5. Seçin **başka bir bilgisayara**ve ardından sorunları olan sanal makinenin IP adresini ekleyin.
   >[!Note]
   >Sanal bir IP adresi kullanmaktan kaçınmak için iç ağ kullanmayı deneyin.

6. Seçin **son**ve ardından **Tamam**.

   ![Bilgisayar seçin](./media/event-id-troubleshoot-vm-rdp-connecton/select-computer.png)

7. Sertifikalar'ı genişletin, uzak Desktop\Certificates klasöre gidin, sertifikaya sağ tıklayın ve ardından **Sil**.

8. Uzak Masaüstü yapılandırması hizmetini yeniden başlatın:

   ```cmd
   net stop SessionEnv
   net start SessionEnv
   ```

   >[!Note]
   >Bu noktada, mağaza'dan mmc yenilerseniz, sertifikayı yeniden görüntülenir. 

VM RDP kullanarak yeniden erişmeyi deneyin.

#### <a name="update-secure-sockets-layer-ssl-certificate"></a>Güvenli Yuva Katmanı (SSL) sertifikasını güncelleştir

VM'yi bir SSL sertifikası kullanmak üzere ayarlarsanız, parmak izini edinmek için aşağıdaki komutu çalıştırın. Sonra sertifikanın parmak izi ile aynı olup olmadığını denetleyin:

```cmd
reg query "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash
```

Yoksa, parmak izini değiştirin:

```cmd
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash /t REG_BINARY /d <CERTIFICATE THUMBPRINT>
```

RDP için RDP otomatik olarak imzalanan sertifika kullanır. böylece anahtarı silmek de deneyebilirsiniz:

```cmd
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SSLCertificateSHA1Hash
```

## <a name="scenario-2"></a>Senaryo 2

### <a name="event-log"></a>Olay günlüğü

CMD örneğinde, SCHANNEL hata olayı 36871 son 24 saat içinde sistem günlüğüne kaydedilir olup olmadığını denetlemek için aşağıdaki komutları çalıştırın:

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Schannel'] and EventID=36871 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Günlük adı:**      Sistem <br />
**Kaynak:**        Schannel <br />
**Tarih:** — <br />
**Olay Kimliği:**      36871 <br />
**Görev kategorisi:** None <br />
**Düzeyi:**         Hata <br />
**Anahtar sözcükler:**       <br />
**Kullanıcı:**          SİSTEM <br />
**Bilgisayar:***bilgisayar* <br />
**Açıklama:** TLS sunucusu kimlik bilgisi oluşturulurken önemli bir hata oluştu. 10013 iç bir hata durumudur.
 
### <a name="cause"></a>Nedeni

Bu sorunu, güvenlik ilkeleri tarafından neden olur. Eski TLS sürümlerini (örneğin, 1.0) ne zaman devre dışı bırakılır, RDP erişim başarısız olur.

### <a name="resolution"></a>Çözüm

RDP varsayılan protokol TLS 1.0 kullanır. Bununla birlikte, yeni bir standart olduğu protokolü, TLS 1.1 olarak'e değiştirilebilir.

Bu sorunu gidermek için bkz: [Azure VM'ye bağlanmak için RDP kullandığınızda, kimlik doğrulama hatalarını giderme](troubleshoot-authentication-error-rdp-vm.md#tls-version).

## <a name="scenario-3"></a>Senaryo 3

Yüklediyseniz **Uzak Masaüstü Bağlantı Aracısı** rol VM'de bulunup bulunmadığını 2056 veya olayın 1296 son 24 saat içinde denetleyin. CMD örnekte, aşağıdaki komutları çalıştırın: 

```cmd
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name=' Microsoft-Windows-TerminalServices-SessionBroker '] and EventID=2056 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name=' Microsoft-Windows-TerminalServices-SessionBroker-Client '] and EventID=1296 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more
```

**Günlük adı:**      Microsoft-Windows-TerminalServices-SessionBroker/işlem <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-SessionBroker <br />
**Tarih:***zaman* <br />
**Olay Kimliği:**      2056 <br />
**Görev kategorisi:** (109) <br />
**Düzeyi:**         Hata <br />
**Anahtar sözcükler:**       <br />
**Kullanıcı:**          AĞ HİZMETİ <br />
**Bilgisayar:***bilgisayar fqdn* <br />
**Açıklama:** Olay Kimliği 2056 kaynağından açıklaması Microsoft-Windows-TerminalServices-SessionBroker nebyla nalezena. Bu olayı oluşturan bileşen, yerel bilgisayarınızda yüklü değil veya yüklemenin bozuk. Yüklediğinizde veya yerel bilgisayarda bileşen onarın. <br />
Olay başka bir bilgisayarda bulunuyorsa, görüntü bilgilerini olay ile kayıtlı gerekiyordu. <br />
Aşağıdaki bilgiler, olay ile eklendi: <br />
NULL <br />
NULL <br />
Veritabanına oturum açma başarısız oldu.

**Günlük adı:**      Microsoft-Windows-TerminalServices-SessionBroker-istemci/işlem <br />
**Kaynak:**        Microsoft-Windows-TerminalServices-SessionBroker-Client <br />
**Tarih:***zaman* <br />
**Olay Kimliği:**      1296 <br />
**Görev kategorisi:** (104) <br />
**Düzeyi:**         Hata <br />
**Anahtar sözcükler:**       <br />
**Kullanıcı:**          AĞ HİZMETİ <br />
**Bilgisayar:***bilgisayar fqdn* <br />
**Açıklama:** Olay Kimliği 1296 kaynağından açıklaması Microsoft-Windows-TerminalServices-SessionBroker-Client nebyla nalezena. Bu olayı oluşturan bileşen, yerel bilgisayarınızda yüklü değil veya yüklemenin bozuk. Yüklediğinizde veya yerel bilgisayarda bileşen onarın.
Olay başka bir bilgisayarda bulunuyorsa, görüntü bilgilerini olay ile kayıtlı gerekiyordu.
Aşağıdaki bilgiler, olay ile eklendi:  <br />
*Metin* <br />
*Metin* <br />
Uzak Masaüstü Bağlantı Aracısı RPC iletişimi için hazır değil.

### <a name="cause"></a>Nedeni

Uzak Masaüstü Bağlantı Aracısı sunucusu konak adı, desteklenen bir değişiklik değil değiştiğinden, bu sorun oluşur. 

Ana bilgisayar adı, Windows İç Uzak Masaüstü hizmetini grubu tarafından çalışabilmek için gerekli olan veritabanında girişleri ve bağımlılıkları vardır. Ana bilgisayar grubu zaten oluşturulduktan sonra değiştirilmesi, birçok hatalarına neden olur ve Aracısı sunucunun çalışmayı durdurmasına neden olabilir.

### <a name="resolution"></a>Çözüm 

Bu sorunu gidermek için Uzak Masaüstü Bağlantı Aracısı rol ve Windows İç Veritabanı yüklenmesi gerekir.

## <a name="next-steps"></a>Sonraki Adımlar

[Schannel olayları](https://technet.microsoft.com/library/dn786445(v=ws.11).aspx)

[Schannel SSP teknik genel bakış](https://technet.microsoft.com/library/dn786429(v=ws.11).aspx)

[Olay Kimliği 1058 & olay 36870 Uzak Masaüstü Oturumu Ana bilgisayar sertifika & SSL iletişimi ile RDP başarısız oluyor](https://blogs.technet.microsoft.com/askperf/2014/10/22/rdp-fails-with-event-id-1058-event-36870-with-remote-desktop-session-host-certificate-ssl-communication/)

[Schannel 36872 veya bir etki alanı denetleyicisinde 36870 Schannel](https://blogs.technet.microsoft.com/instan/2009/01/05/schannel-36872-or-schannel-36870-on-a-domain-controller/)

[Olay Kimliği 1058 — Uzak Masaüstü Hizmetleri kimlik doğrulaması ve şifreleme](https://technet.microsoft.com/library/ee890862(v=ws.10).aspx)

