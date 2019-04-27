---
title: StorSimple cihazınıza uzaktan bağlanma | Microsoft Docs
description: Cihazınızı uzaktan yönetimi yapılandırma ve HTTP veya HTTPS aracılığıyla StorSimple için Windows PowerShell için nasıl açıklar.
services: storsimple
documentationcenter: ''
author: alkohli
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/02/2018
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 05bec60f4c56c98e9b910b50e858656a2e5554b2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60631874"
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi Cihazınızı uzaktan bağlayın

## <a name="overview"></a>Genel Bakış

Cihazınızın Windows PowerShell aracılığıyla uzaktan bağlanabilirsiniz. Bu şekilde bağlandığınızda menü görmez. (Yalnızca seri konsol cihaza bağlanmak için kullandığınız bir menü görürsünüz.) Windows PowerShell uzaktan iletişimiyle belirli bir çalışma alanı için bağlanın. Ayrıca, görüntüleme dili de belirtebilirsiniz.

Cihazınızı yönetmek için Windows PowerShell uzaktan iletişimini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek StorSimple için Windows PowerShell kullanarak](storsimple-8000-windows-powershell-administration.md).

Bu öğreticide, Cihazınızı uzaktan yönetimi yapılandırma ve StorSimple için Windows PowerShell'e bağlanma açıklanır. Windows PowerShell uzaktan bağlanmak için HTTP veya HTTPS kullanın. Ancak, StorSimple için Windows PowerShell'e bağlanma verirken aşağıdakileri göz önünde bulundurun:

* Doğrudan cihaz seri konsoluna bağlanmak güvenlidir, ancak ağ anahtarları seri konsoluna bağlanmak değil. Cihaz seri konsoluna ağ anahtarları bağlanırken güvenlik riskini dikkatli olun.
* Bir HTTP oturumu aracılığıyla bağlanan ağ üzerinden seri konsol üzerinden bağlanma daha fazla güvenlik sunar. Bu en güvenli yöntem olmamasına karşın, güvenilen ağlarda kabul edilebilir.
* Bir otomatik olarak imzalanan bir sertifika ile HTTPS oturumu aracılığıyla bağlamak, en güvenli ve önerilen seçenek aynıdır.

Windows PowerShell arabirimi için uzaktan bağlanabilirsiniz. Ancak, Windows PowerShell arabirimi üzerinden StorSimple cihazınıza uzaktan erişim varsayılan olarak etkin değil. Cihazda uzaktan yönetimi önce etkinleştirmeniz gerekir ve ardından istemci, cihazınıza erişmek için kullanılır.

Bu makalede açıklanan adımlar, Windows Server 2012 R2 çalıştıran bir konak sistemi üzerinde gerçekleştirildi.

## <a name="connect-through-http"></a>HTTP bağlanma

Windows PowerShell için StorSimple için bir HTTP oturumu aracılığıyla bağlanan StorSimple cihazınızın seri konsol üzerinden bağlanma daha fazla güvenlik sunar. Bu en güvenli yöntem olmamasına karşın, güvenilen ağlarda kabul edilebilir.

Azure portalında veya seri konsol, uzaktan yönetimi yapılandırmak üzere kullanabilirsiniz. Aşağıdaki yordamlardan seçin:

* HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanma
* [HTTP üzerinden uzaktan yönetimi etkinleştirmek için seri Konsolu](#use-the-serial-console-to-enable-remote-management-over-http)

Uzaktan Yönetimi etkinleştirdikten sonra istemci uzak bağlantı için hazırlamak için aşağıdaki yordamı kullanın.

* [Uzak bağlantı için istemci hazırlayın](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a>HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanma

HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-azure-portal"></a>Azure Portalı aracılığıyla uzaktan yönetimi etkinleştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Seçin **cihazları** ve sonra seçin ve Uzaktan Yönetim için yapılandırmak istediğiniz cihaza tıklayın. Git **cihaz Ayarları > Güvenlik**.
2. İçinde **güvenlik ayarları** dikey penceresinde tıklayın **uzaktan yönetimi**.
3. İçinde **uzaktan yönetimi** dikey penceresinde ayarlayın **uzaktan yönetimi etkinleştirme** için **Evet**.
4. Artık HTTP kullanarak bağlanmayı seçebilirsiniz. (Varsayılan HTTPS üzerinden bağlanmaktır.) HTTP'ın seçili olduğundan emin olun.
   
   > [!NOTE]
   > HTTP üzerinden bağlanma yalnızca güvenilen ağlarda kabul edilebilir.
   
5. Tıklayın **Kaydet** ve onaylamanız istendiğinde **Evet**.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>HTTP üzerinden uzaktan yönetimi etkinleştirmek için seri Konsolu
Uzaktan Yönetimi etkinleştirmek için cihaz seri konsoluna aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Cihaz seri konsolu üzerinden uzaktan yönetimi etkinleştirmek için
1. Seri konsol menüsünde 1 seçeneğini belirleyin. Cihaza seri Konsolu kullanma hakkında daha fazla bilgi için Git [cihaz seri Konsolu aracılığıyla StorSimple için Windows PowerShell Bağlan](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. İsteminde aşağıdakini yazın: `Enable-HcsRemoteManagement –AllowHttp`
3. Cihaza bağlanmak için HTTP kullanarak güvenlik açıkları hakkında bilgilendirilirsiniz. Sorulduğunda yazarak onaylayın **Y**.
4. HTTP yazarak etkin olduğunu doğrulayın: `Get-HcsSystem`
5. Doğrulayın **RemoteManagementMode** alan gösterir **HttpsAndHttpEnabled**. Aşağıdaki çizimde, bu ayarları içinde PuTTY gösterir.
   
     ![Seri HTTPS ve HTTP Etkin](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Uzak bağlantı için istemci hazırlayın
Uzaktan Yönetimi etkinleştirmek için istemcide aşağıdaki adımları gerçekleştirin.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Uzak bağlantı için istemci hazırlamak için
1. Bir Windows PowerShell oturumu yönetici olarak başlatın. Varsayılan olarak bir Windows 10 istemcisi kullanarak Windows Uzaktan Yönetimi hizmetini el ile olarak ayarlanır. Yazarak hizmeti başlatmak gerekebilir:

    `Start-Service WinRM`
    
2. StorSimple cihazı IP adresi istemcinin güvenilir konaklar listesine eklemek için aşağıdaki komutu yazın:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     Değiştir <*device_ip*> IP adresiyle cihazınızın; örneğin: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Cihaz kimlik bilgilerini bir değişkene kaydetmek için aşağıdaki komutu yazın: 
   
    ```
    $cred = Get-Credential
    ```
    
4. Görüntülenen iletişim kutusunda:
   
   1. Kullanıcı adı şu biçimde yazın: *device_ip\SSAdmin*.
   2. Cihaz Kurulum Sihirbazı ile yapılandırıldığında ayarlanan cihaz Yöneticisi parolasını yazın. Varsayılan parola *Password1*.
5. Bir Windows PowerShell oturumunda aşağıdaki komutu yazarak cihazda başlatın:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > StorSimple sanal cihazı ile kullanmak için bir Windows PowerShell oturumu oluşturmak için URL'ye `–Port` parametresi ve StorSimple sanal Gereci için uzaktan iletişim'de yapılandırılmış ortak bağlantı noktasını belirtin.
   
   
Bu noktada, cihaz bir etkin uzak Windows PowerShell oturumu olması gerekir.
   
![PowerShell uzaktan iletişimini HTTP kullanma](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>HTTPS bağlanma

Windows PowerShell için StorSimple için bir HTTPS oturum üzerinden bağlanma, Microsoft Azure StorSimple cihazınıza uzaktan bağlanma en güvenli ve önerilen yöntemdir. Aşağıdaki yordamlarda, seri konsolu ve istemci bilgisayarları, StorSimple için Windows PowerShell'e bağlanmak için HTTPS kullanacak şekilde ayarlamak açıklanmaktadır.

Azure portalında veya seri konsol, uzaktan yönetimi yapılandırmak üzere kullanabilirsiniz. Aşağıdaki yordamlardan seçin:

* HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanma
* [HTTPS üzerinden uzaktan yönetimi etkinleştirmek için seri Konsolu](#use-the-serial-console-to-enable-remote-management-over-https)

Uzaktan Yönetimi etkinleştirdikten sonra ana bilgisayar bir uzaktan yönetim için hazırlama ve uzak ana bilgisayardan cihaza bağlanmak için aşağıdaki yordamları kullanın.

* [Konağın uzaktan yönetimi için hazırlama](#prepare-the-host-for-remote-management)
* [Uzak ana bilgisayardan cihaza bağlanma](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a>HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanma

HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a>Azure portalından HTTPS üzerinden uzaktan yönetimi etkinleştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Seçin **cihazları** ve sonra seçin ve Uzaktan Yönetim için yapılandırmak istediğiniz cihaza tıklayın. Git **cihaz Ayarları > Güvenlik**.
2. İçinde **güvenlik ayarları** dikey penceresinde tıklayın **uzaktan yönetimi**.
3. **Uzaktan yönetimi Etkinleştir**’i **Evet** olarak ayarlayın.
4. Artık HTTPS kullanarak bağlanmayı seçebilirsiniz. (Varsayılan HTTPS üzerinden bağlanmaktır.) HTTPS'ın seçili olduğundan emin olun.
5. Tıklatın ve ardından **uzaktan yönetim sertifikası indir**. Bu dosyanın kaydedileceği bir konum belirtin. Bu sertifikayı cihaza bağlanmak için kullanacağınız istemci veya konak bilgisayara yüklemeniz gerekir.
6. Tıklayın **Kaydet** ve ardından **Evet** onaylamanız istendiğinde.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>HTTPS üzerinden uzaktan yönetimi etkinleştirmek için seri Konsolu

Uzaktan Yönetimi etkinleştirmek için cihaz seri konsoluna aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Cihaz seri konsolu üzerinden uzaktan yönetimi etkinleştirmek için
1. Seri konsol menüsünde 1 seçeneğini belirleyin. Cihaza seri Konsolu kullanma hakkında daha fazla bilgi için Git [cihaz seri Konsolu aracılığıyla StorSimple için Windows PowerShell Bağlan](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. İsteminde aşağıdakini yazın:
   
     `Enable-HcsRemoteManagement`
   
    Bu, Cihazınızda HTTPS etkinleştirmeniz gerekir.
3. Yazarak HTTPS etkin olduğunu doğrulayın: 
   
     `Get-HcsSystem`
   
    Emin olun **RemoteManagementMode** alan gösterir **HttpsEnabled**. Aşağıdaki çizimde, bu ayarları içinde PuTTY gösterir.
   
     ![Seri HTTPS etkin](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Çıktısından `Get-HcsSystem`, cihazın seri numarasını kopyalayın ve daha sonra kullanmak üzere kaydedin.
   
   > [!NOTE]
   > Sertifikanın CN adı için seri numarasını eşler.
   
5. Yazarak bir uzaktan yönetim sertifikası alın: 
   
     `Get-HcsRemoteManagementCert`
   
    Bir sertifika aşağıdakine benzer görünecektir.
   
    ![Uzaktan Yönetim sertifikası alma](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Sertifikada bulunan bilgileri kopyalayın **---BEGIN CERTIFICATE---** için **---END CERTIFICATE---** .cer dosyası olarak bir metin düzenleyicisine Not Defteri gibi ve kaydedin. (Konak hazırlarken bu dosya, uzak konağa kopyalayacak.)
   
   > [!NOTE]
   > Yeni bir sertifika oluşturmak için `Set-HcsRemoteManagementCert` cmdlet'i.
   
### <a name="prepare-the-host-for-remote-management"></a>Konağın uzaktan yönetimi için hazırlama

Bir HTTPS oturum kullanan bir uzak bağlantı için ana bilgisayarı hazırlamak için aşağıdaki yordamları gerçekleştirin:

* [İstemci veya uzak ana bilgisayarın kök deposuna .cer dosyasını alma](#to-import-the-certificate-on-the-remote-host).
* [Hosts dosyasını uzak ana bilgisayarınızda cihaz seri numaraları ekleme](#to-add-device-serial-numbers-to-the-remote-host).

Önceki yordamların her biri aşağıda açıklanmıştır.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Uzak ana bilgisayarda sertifikayı içeri aktarmak için
1. .Cer dosyasını sağ tıklayıp **yükleme sertifika**. Bu Sertifika Alma Sihirbazı'nı başlatır.
   
    ![Sertifika Alma Sihirbazı 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. İçin **Store konumu**seçin **yerel makine**ve ardından **sonraki**.
3. Seçin **tüm sertifikaları aşağıdaki depolama alanına yerleştir**ve ardından **Gözat**. Uzak ana kök deposuna gidin ve ardından **sonraki**.
   
    ![Sertifika Alma Sihirbazı'nı 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. **Son**'a tıklayın. İçeri aktarmanın başarılı olduğunu bildiren bir ileti görüntülenir.
   
    ![Sertifika Alma Sihirbazı'nı 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Uzak konağa cihaz seri numaralarını eklemek için
1. Not Defteri'ni yönetici olarak başlatın ve ardından \Windows\System32\Drivers\etc bulunan hosts dosyasını açın.
2. Aşağıdaki üç girdi, hosts dosyasına ekleyin: **Veri 0 IP adresi**, **denetleyici 0 sabit IP adresi**, ve **Denetleyici 1 sabit IP adresi**.
3. Daha önce kaydettiğiniz cihaz seri numarasını girin. Aşağıdaki görüntüde gösterildiği gibi bu IP adresine eşlenir. Denetleyici 0 ve denetleyici 1 için ekleme **Controller0** ve **Controller1** sonunda, seri numarası (CN adı).
   
    ![Konaklar dosyasına CN adı ekleme](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Hosts dosyasını kaydedin.

### <a name="connect-to-the-device-from-the-remote-host"></a>Uzak ana bilgisayardan cihaza bağlanma

Windows PowerShell ve SSL SSAdmin oturumu, uzak konak ya da istemci Cihazınızda girmek için kullanın. Seçeneğinde 1 SSAdmin oturumu eşlendiği [seri konsol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) cihazınızın menüsü.

Uzak Windows PowerShell bağlantı kurmak istediğiniz bilgisayarda aşağıdaki yordamı gerçekleştirin.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Windows PowerShell ve SSL kullanarak cihaz üzerinde bir SSAdmin oturumu girmek için
1. Bir Windows PowerShell oturumu yönetici olarak başlatın. Varsayılan olarak bir Windows 10 istemcisi kullanarak Windows Uzaktan Yönetimi hizmetini el ile olarak ayarlanır. Yazarak hizmeti başlatmak gerekebilir:

    `Start-Service WinRM`

2. Cihazı IP adresi, istemcinin güvenilir ana yazarak ekleyin:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    Burada <*device_ip*> cihazınızın IP adresidir; örneğin: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Yeni bir kimlik bilgisi oluşturmak için şunu yazın:
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Burada <*hedef cihaz IP'SİNDEN*> veri 0; cihazınız için IP adresi gibi **10.126.173.90** hosts dosyasına bir önceki resimde gösterildiği gibi. Ayrıca, cihazınız için yönetici parolasını sağlayın.
4. Yazarak bir oturum oluşturun:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    -ComputerName parametresi için cmdlet sağlar <*hedef cihazın seri numarasını*>. Bu seri numarasına DATA 0, uzak ana bilgisayarda hosts dosyasında IP adresi eşlendiği; Örneğin, **SHX0991003G44MT** aşağıdaki görüntüde gösterildiği gibi.
5. Şunu yazın:
   
     `Enter-PSSession $session`
6. Birkaç dakika beklemeniz gerekecektir ve daha sonra Cihazınızı HTTPS üzerinden SSL üzerinden bağlanır. Cihazınıza bağlı belirten bir ileti görürsünüz.
   
    ![PowerShell uzaktan iletişimini HTTPS ve SSL kullanma](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için Windows PowerShell kullanarak](storsimple-8000-windows-powershell-administration.md).
* Daha fazla bilgi edinin [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

