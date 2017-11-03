---
title: "StorSimple cihazınıza uzaktan bağlanma | Microsoft Docs"
description: "Cihazınızı uzaktan yönetim için yapılandırma ve HTTP veya HTTPS aracılığıyla StorSimple için Windows PowerShell için bağlanma açıklanmaktadır."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/07/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ff76884f020a0fb8a1b48bd371c419bd65e85fd3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="connect-remotely-to-your-storsimple-8000-series-device"></a>StorSimple 8000 serisi cihazınıza uzaktan bağlanma

## <a name="overview"></a>Genel Bakış

Windows PowerShell aracılığıyla Cihazınızı uzaktan bağlanabilir. Bu şekilde bağlandığınızda, menü görmezsiniz. (Yalnızca seri konsol cihazda bağlamak için kullanırsanız, bir menü bakın.) Windows PowerShell uzaktan iletişimini ile belirli bir çalışma bağlayın. Görüntüleme dili de belirtebilirsiniz.

Cihazınızı yönetmek için Windows PowerShell uzaktan iletişimini kullanma hakkında daha fazla bilgi için Git [StorSimple Cihazınızı yönetmek StorSimple için Windows PowerShell'i kullanın](storsimple-8000-windows-powershell-administration.md).

Bu öğretici, Cihazınızı uzaktan yönetim için yapılandırma ve StorSimple için Windows PowerShell için bağlanma açıklanmaktadır. Uzaktan Windows PowerShell ile bağlanmak için HTTP veya HTTPS kullanın. Ancak, StorSimple için Windows PowerShell'e bağlanmak nasıl karar verirken aşağıdaki bilgileri göz önünde bulundurun:

* Doğrudan cihaz seri konsoluna bağlanmak güvenlidir, ancak seri konsol ağ anahtarları bağlanma değil. Cihaz seri konsoluna ağ anahtarları bağlanırken güvenlik riski dikkatli olun.
* Bir HTTP oturumu aracılığıyla bağlanan ağ üzerinden seri konsol üzerinden bağlanma daha fazla güvenlik sunar. Bu en güvenli yöntemi olmamasına karşın, güvenilen ağlarda kabul edilebilir.
* Kendinden imzalı bir sertifika ile HTTPS oturumu aracılığıyla bağlanan en güvenli ve önerilen seçenek olur.

Windows PowerShell arabirimine uzaktan bağlanabilirsiniz. Ancak, Windows PowerShell arabirimi üzerinden StorSimple cihazınız için uzaktan erişim varsayılan olarak etkin değildir. Cihazda uzaktan yönetimi ilk etkinleştirin ve sonra istemci üzerinde Cihazınızı erişmek için kullanılır.

Bu makalede açıklanan adımları Windows Server 2012 R2 çalıştıran bir konak sisteminde gerçekleştirilmiştir.

## <a name="connect-through-http"></a>HTTP bağlanma

Windows PowerShell için StorSimple için bir HTTP oturumu aracılığıyla bağlanma StorSimple Cihazınızı seri konsol üzerinden bağlanma daha fazla güvenlik sunar. Bu en güvenli yöntemi olmamasına karşın, güvenilen ağlarda kabul edilebilir.

Uzaktan yönetimini yapılandırmak için Azure portal'ı veya seri konsolunu kullanabilirsiniz. Aşağıdaki yordamlardan seçin:

* [HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanma](#use-the-azure-classic-portal-to-enable-remote-management-over-http)
* [Seri konsol HTTP üzerinden uzaktan yönetimi etkinleştirmek için kullanın](#use-the-serial-console-to-enable-remote-management-over-http)

Uzaktan Yönetimi etkinleştirdikten sonra istemci uzak bir bağlantı için hazırlamak için aşağıdaki yordamı kullanın.

* [Uzak bağlantı için istemci hazırlayın](#prepare-the-client-for-remote-connection)

### <a name="use-the-azure-portal-to-enable-remote-management-over-http"></a>HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanma

HTTP üzerinden uzaktan yönetimi etkinleştirmek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-azure-portal"></a>Azure portalı üzerinden uzaktan yönetimi etkinleştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Seçin **aygıtları** seçin ve Uzaktan Yönetim için yapılandırmak istediğiniz cihaz seçeneğine tıklayın. Git **Aygıt Ayarları > Güvenlik**.
2. İçinde **güvenlik ayarları** dikey penceresinde tıklatın **Uzaktan Yönetim**.
3. İçinde **uzaktan yönetimi** dikey penceresinde ayarlamak **uzaktan yönetimini etkinleştirme** için **Evet**.
4. Artık HTTP kullanarak bağlanmayı seçebilirsiniz. (HTTPS üzerinden bağlanmak için varsayılandır.) HTTP seçili olduğundan emin olun.
   
   > [!NOTE]
   > HTTP üzerinden bağlanma yalnızca güvenilen ağlarda kabul edilebilir.
   
5. Tıklatın **kaydetmek** ve onaylamanız istendiğinde seçin **Evet**.

### <a name="use-the-serial-console-to-enable-remote-management-over-http"></a>Seri konsol HTTP üzerinden uzaktan yönetimi etkinleştirmek için kullanın
Uzaktan Yönetimi etkinleştirmek için cihaz seri konsoluna aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Cihaz seri Konsolu aracılığıyla uzaktan yönetimini etkinleştirmek için
1. Seri konsol menüsünde 1 seçeneğini belirleyin. Cihaza seri Konsolu kullanma hakkında daha fazla bilgi için Git [cihaz seri Konsolu aracılığıyla StorSimple için Windows PowerShell Bağlan](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. İstemine yazın:`Enable-HcsRemoteManagement –AllowHttp`
3. Cihaza bağlanmak için HTTP kullanmanın güvenlik açıkları hakkında bilgilendirilirsiniz. İstendiğinde, yazarak onaylayın **Y**.
4. HTTP yazarak etkin olduğunu doğrulayın:`Get-HcsSystem`
5. Doğrulayın **RemoteManagementMode** alan gösterir **HttpsAndHttpEnabled**. Aşağıdaki çizimde, bu ayarları içinde PuTTY gösterir.
   
     ![Seri HTTPS ve HTTP Etkin](./media/storsimple-remote-connect/HCS_SerialHttpsAndHttpEnabled.png)

### <a name="prepare-the-client-for-remote-connection"></a>Uzak bağlantı için istemci hazırlayın
Uzaktan Yönetimi etkinleştirmek için istemcide aşağıdaki adımları gerçekleştirin.

#### <a name="to-prepare-the-client-for-remote-connection"></a>Uzak bağlantı için istemci hazırlamak için
1. Bir Windows PowerShell oturumu yönetici olarak başlatın.
2. StorSimple cihazı IP adresi istemcinin güvenilir ana bilgisayarlar listesine eklemek için aşağıdaki komutu yazın:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
     Değiştir <*device_ip*> Cihazınızı; IP adresi ile örneğin: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Cihaz kimlik bilgileri bir değişkene kaydetmek için aşağıdaki komutu yazın: 
   
    ```
    $cred = Get-Credential
    ```
    
4. Görüntülenen iletişim kutusunda:
   
   1. Kullanıcı adı şu biçimde yazın: *device_ip\SSAdmin*.
   2. Cihaz Kurulum Sihirbazı ile yapılandırıldığında ayarlandı aygıt yönetici parolasını yazın. Varsayılan parola *Parola1*.
5. Bir Windows PowerShell oturumunda aşağıdaki komutu yazarak cihazda başlatın:
   
     `Enter-PSSession -Credential $cred -ConfigurationName SSAdminConsole -ComputerName <device_ip>`
   
   > [!NOTE]
   > StorSimple sanal cihazı ile kullanmak için bir Windows PowerShell oturumu oluşturmak için Ekle `–Port` parametre ve Remoting StorSimple sanal Gereci için yapılandırılmış ortak bağlantı noktasını belirtin.
   
   
Bu aşamada, aygıta bir etkin uzaktan Windows PowerShell oturumu olması gerekir.
   
![HTTP kullanarak PowerShell uzaktan iletişim](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTP.png)

## <a name="connect-through-https"></a>HTTPS bağlanır

Windows PowerShell için StorSimple için bir HTTPS oturumu aracılığıyla bağlanma Microsoft Azure StorSimple cihazınıza uzaktan bağlanma en güvenli ve önerilen yöntemdir. Aşağıdaki yordamlar, seri konsol ve istemci bilgisayarları StorSimple için Windows PowerShell'e bağlanmak için HTTPS kullanacak şekilde ayarlamak açıklanmaktadır.

Uzaktan yönetimini yapılandırmak için Azure portal'ı veya seri konsolunu kullanabilirsiniz. Aşağıdaki yordamlardan seçin:

* [HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanma](#use-the-azure-classic-portal-to-enable-remote-management-over-https)
* [Seri konsol HTTPS üzerinden uzaktan yönetimi etkinleştirmek için kullanın](#use-the-serial-console-to-enable-remote-management-over-https)

Uzaktan Yönetimi etkinleştirdikten sonra ana bilgisayar bir uzaktan yönetime hazırlamak ve aygıta uzak ana bilgisayara bağlanmak için aşağıdaki yordamları kullanın.

* [Konağın uzaktan yönetimi için hazırlama](#prepare-the-host-for-remote-management)
* [Uzak ana bilgisayardan cihaza bağlanın](#connect-to-the-device-from-the-remote-host)

### <a name="use-the-azure-portal-to-enable-remote-management-over-https"></a>HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalını kullanma

HTTPS üzerinden uzaktan yönetimi etkinleştirmek için Azure portalında aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-over-https-from-the-azure-portal"></a>Azure portalından HTTPS üzerinden uzaktan yönetimi etkinleştirmek için

1. StorSimple Cihaz Yöneticisi hizmetinize gidin. Seçin **aygıtları** seçin ve Uzaktan Yönetim için yapılandırmak istediğiniz cihaz seçeneğine tıklayın. Git **Aygıt Ayarları > Güvenlik**.
2. İçinde **güvenlik ayarları** dikey penceresinde tıklatın **Uzaktan Yönetim**.
3. **Uzaktan yönetimi Etkinleştir**’i **Evet** olarak ayarlayın.
4. Artık HTTPS kullanarak bağlanmayı seçebilirsiniz. (HTTPS üzerinden bağlanmak için varsayılandır.) HTTPS seçili olduğundan emin olun.
5. Tıklatın ve ardından **uzaktan yönetim sertifikası indir**. Bu dosyayı kaydetmek için bir konum belirtin. Bu sertifikayı cihaza bağlanmak için kullanacağınız istemci veya ana bilgisayarda yüklemeniz gerekir.
6. Tıklatın **kaydetmek** ve ardından **Evet** onaylamanız istendiğinde.

### <a name="use-the-serial-console-to-enable-remote-management-over-https"></a>Seri konsol HTTPS üzerinden uzaktan yönetimi etkinleştirmek için kullanın

Uzaktan Yönetimi etkinleştirmek için cihaz seri konsoluna aşağıdaki adımları gerçekleştirin.

#### <a name="to-enable-remote-management-through-the-device-serial-console"></a>Cihaz seri Konsolu aracılığıyla uzaktan yönetimini etkinleştirmek için
1. Seri konsol menüsünde 1 seçeneğini belirleyin. Cihaza seri Konsolu kullanma hakkında daha fazla bilgi için Git [cihaz seri Konsolu aracılığıyla StorSimple için Windows PowerShell Bağlan](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console).
2. İstemine yazın:
   
     `Enable-HcsRemoteManagement`
   
    Bu, Cihazınızda HTTPS etkinleştirmeniz gerekir.
3. Yazarak HTTPS etkin olduğunu doğrulayın: 
   
     `Get-HcsSystem`
   
    Olduğundan emin olun **RemoteManagementMode** alan gösterir **HttpsEnabled**. Aşağıdaki çizimde, bu ayarları içinde PuTTY gösterir.
   
     ![Seri HTTPS etkin](./media/storsimple-remote-connect/HCS_SerialHttpsEnabled.png)
4. Çıktısından `Get-HcsSystem`, cihazın seri numarası kopyalayın ve daha sonra kullanmak üzere kaydedin.
   
   > [!NOTE]
   > Seri numarası sertifikanın CN adı eşler.
   
5. Uzak Yönetim sertifikası yazarak alın: 
   
     `Get-HcsRemoteManagementCert`
   
    Bir sertifika aşağıdakine benzer görünecektir.
   
    ![Uzaktan Yönetim sertifikası alma](./media/storsimple-remote-connect/HCS_GetRemoteManagementCertificate.png)
6. Sertifika bilgileri kopyalayın **---başlangıç sertifika---** için **---son SERTİFİKAYI---** .cer dosyası olarak bir metin düzenleyicisine Not Defteri gibi ve kaydedin. (Konak hazırlarken bu dosyayı uzak ana bilgisayara kopyalayacak.)
   
   > [!NOTE]
   > Yeni bir sertifika oluşturmak için kullanın `Set-HcsRemoteManagementCert` cmdlet'i.
   
### <a name="prepare-the-host-for-remote-management"></a>Konağın uzaktan yönetimi için hazırlama

Bir HTTPS oturumu kullanan uzak bir bağlantı için ana bilgisayarı hazırlamak için aşağıdaki yordamları gerçekleştirin:

* [İstemci veya uzak ana bilgisayarın kök deposuna .cer dosyasını içeri](#to-import-the-certificate-on-the-remote-host).
* [Uzak ana bilgisayarda hosts dosyasını cihaz seri numaraları eklemek](#to-add-device-serial-numbers-to-the-remote-host).

Önceki yordamların her biri aşağıda tanımlanmıştır.

#### <a name="to-import-the-certificate-on-the-remote-host"></a>Uzak ana bilgisayarda sertifikasını içeri aktarmak için
1. .Cer dosyasını sağ tıklatın ve seçin **yükleme sertifika**. Bu Sertifika Alma Sihirbazı'nı başlatır.
   
    ![Sertifika Alma Sihirbazı 1](./media/storsimple-remote-connect/HCS_CertificateImportWizard1.png)
2. İçin **depo konumuna**seçin **yerel makine**ve ardından **sonraki**.
3. Seçin **tüm sertifikaları aşağıdaki depolama alanına yerleştir**ve ardından **Gözat**. Uzak ana bilgisayarınız kök deposuna gidin ve ardından **sonraki**.
   
    ![Sertifika Alma Sihirbazı'nı 2](./media/storsimple-remote-connect/HCS_CertificateImportWizard2.png)
4. **Son**'a tıklayın. Alma işleminin başarılı olduğunu bildiren bir ileti görüntülenir.
   
    ![Sertifika Alma Sihirbazı'nı 3](./media/storsimple-remote-connect/HCS_CertificateImportWizard3.png)

#### <a name="to-add-device-serial-numbers-to-the-remote-host"></a>Cihaz seri numaraları uzak ana bilgisayara eklemek için
1. Not Defteri'ni yönetici olarak başlatın ve sonra \Windows\System32\Drivers\etc bulunan hosts dosyasını açın.
2. Aşağıdaki üç girdileri hosts dosyanıza ekleyin: **veri 0 IP adresi**, **denetleyici 0 sabit IP adresi**, ve **Denetleyici 1 sabit IP adresi**.
3. Daha önce kaydettiğiniz cihaz seri numarasını girin. Aşağıdaki görüntüde gösterildiği gibi bu IP adresine eşleyen. Denetleyici 0 ve denetleyici 1 için sona **Controller0** ve **Controller1** seri numarasını (CN adı), sonunda.
   
    ![Konaklar dosyasına CN adı ekleme](./media/storsimple-remote-connect/HCS_AddingCNNameToHostsFile.png)
4. Hosts dosyasını kaydedin.

### <a name="connect-to-the-device-from-the-remote-host"></a>Uzak ana bilgisayardan cihaza bağlanın

Bir uzak ana bilgisayara veya istemci, Cihazınızda bir SSAdmin oturumu girmek için Windows PowerShell ve SSL kullanın. 1 seçeneğinde SSAdmin oturum eşlendiği [seri konsol](storsimple-8000-windows-powershell-administration.md#connect-to-windows-powershell-for-storsimple-via-the-device-serial-console) Cihazınızı menüsü.

Aşağıdaki yordam, uzak Windows PowerShell bağlantı kurmak istediğiniz bilgisayarda gerçekleştirin.

#### <a name="to-enter-an-ssadmin-session-on-the-device-by-using-windows-powershell-and-ssl"></a>Windows PowerShell ve SSL kullanarak aygıtta bir SSAdmin oturumu girmek için
1. Bir Windows PowerShell oturumu yönetici olarak başlatın.
2. Aygıt IP adresi yazarak istemcinin güvenilir konaklar ekleyin:
   
     `Set-Item wsman:\localhost\Client\TrustedHosts <device_ip> -Concatenate -Force`
   
    Burada <*device_ip*> cihazınızın IP adresidir; örneğin: 
   
     `Set-Item wsman:\localhost\Client\TrustedHosts 10.126.173.90 -Concatenate -Force`
3. Yeni bir kimlik bilgisi oluşturmak için şunu yazın:
   
     `$cred = New-Object pscredential @("<IP of target device>\SSAdmin", (ConvertTo-SecureString -Force -AsPlainText "<Device Administrator Password>"))`
   
    Burada <*hedef aygıtın IP*> cihazınız için; veri 0 IP adresidir Örneğin, **10.126.173.90** hosts dosyasını önceki görüntüde gösterildiği gibi. Ayrıca, cihazınız için yönetici parolasını sağlayın.
4. Bir oturum yazarak oluşturun:
   
     `$session = New-PSSession -UseSSL -ComputerName <Serial number of target device> -Credential $cred -ConfigurationName "SSAdminConsole"`
   
    Cmdlet - ComputerName parametresinde için sağlayın <*hedef cihaz seri numarasını*>. Bu seri numarasına veri uzak ana bilgisayarda hosts dosyasında 0 IP adresine eşlendi; Örneğin, **SHX0991003G44MT** aşağıdaki görüntüde gösterildiği gibi.
5. Şunu yazın:
   
     `Enter-PSSession $session`
6. Birkaç dakika beklemeniz gerekir ve ardından, HTTPS üzerinden aygıtınıza SSL üzerinden bağlanır. Cihazınıza bağlı belirten bir ileti görürsünüz.
   
    ![PowerShell uzaktan iletişimini HTTPS ve SSL kullanma](./media/storsimple-remote-connect/HCS_PSRemotingUsingHTTPSAndSSL.png)

## <a name="next-steps"></a>Sonraki adımlar

* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için Windows PowerShell kullanarak](storsimple-8000-windows-powershell-administration.md).
* Daha fazla bilgi edinmek [StorSimple Cihazınızı yönetmek için StorSimple cihaz Yöneticisi hizmetini kullanarak](storsimple-8000-manager-service-administration.md).

