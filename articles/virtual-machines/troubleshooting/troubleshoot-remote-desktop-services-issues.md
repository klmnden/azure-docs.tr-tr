---
title: Azure sanal makinesine Uzak Masaüstü Hizmetleri Başlatılıyor yok | Microsoft Docs
description: Bir sanal makineye bağlanırken, Uzak Masaüstü Hizmetleri ile ilgili sorunları gidermeyi öğrenin | Microsoft Docs
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: cshepard
editor: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/23/2018
ms.author: genli
ms.openlocfilehash: a9967aec61aaab5bc6b4517407f36e2a6c7342c8
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2018
ms.locfileid: "50238871"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Azure sanal makinesine Uzak Masaüstü Hizmetleri başlatma değil

Bu makale, Uzak Masaüstü Hizmetleri (TermService) başlangıç değil veya başlatmak başarısız olduğunda, bir Azure sanal makine (VM) için bağlanma sorunlarını açıklar.

>[!NOTE]
>Azure, kaynak oluşturmak ve bu kaynaklarla çalışmak için iki dağıtım modeli kullanır: [Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Resource Manager dağıtım modeli kullanılarak açıklanır. Bu model Klasik dağıtım modeli kullanılarak yerine yeni dağıtımlar için kullanmanızı öneririz.

## <a name="symptoms"></a>Belirtiler

Bir VM'ye bağlanmaya çalıştığınızda, aşağıdaki senaryolarda karşılaşırsınız:

- VM ekran görüntüsü, işletim sistemini tam olarak yüklenir ve kimlik bilgileri bekleniyor olduğunu gösterir.

    ![VM durumunun ekran görüntüsü](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- Olay günlüklerini görüntüleyin VM'yi Olay Görüntüleyicisi'ni kullanarak, uzaktan, Uzak Masaüstü Hizmetleri (TermServ) olmayan başlatma veya başlatılamamasına göreceksiniz. Örnek günlük verilmiştir:

    **Oturum adı**: Sistem </br>
    **Kaynak**: Hizmet Denetimi Yöneticisi </br>
    **Tarih**: 16/12/2017 11:19:36: 00</br>
    **Olay Kimliği**: 7022</br>
    **Görev kategorisi**: yok</br>
    **Düzey**: hata</br>
    **Anahtar sözcükler**: Klasik</br>
    **Kullanıcı**: yok</br>
    **Bilgisayar**: vm.contoso.com</br>
    **Açıklama**: Uzak Masaüstü Hizmetleri hizmeti başlatılırken askıya alındı. 

    Seri erişim Konsolu özelliği, aşağıdaki sorguyu kullanarak bu hataları aramak için de kullanabilirsiniz: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Nedeni
 
Uzak Masaüstü Hizmetleri sanal makinede çalışmadığından bu sorun oluşur. Nedeni aşağıdaki senaryolara bağlı olabilir: 

- TermService'nin kümesine **devre dışı bırakılmış**. 
- TermService'nin kilitlenme veya askıda. 

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için seri konsolu veya [çevrimdışı VM'yi onarın](#repair-the-vm-offline) sanal Makinenin işletim sistemi diskini bir kurtarma sanal Makinesine ekleyerek.

### <a name="use-serial-console"></a>Seri Konsolu

1. Erişim [seri konsol](serial-console-windows.md) seçerek **destek ve sorun giderme** > **seri konsol**. VM'de özelliği etkinleştirilmişse, sanal makine başarıyla bağlanabilirsiniz.

2. CMD örneği için yeni bir kanal oluşturun. Tür **CMD** kanal adını almak için kanal başlatmak için.

3. Çalışan CMD örneği kanala geçin. Bu durumda, kanal 1 olmalıdır.

   ```
   ch -si 1
   ```

4. Tuşuna **Enter** yeniden ve sanal makine için geçerli bir kullanıcı adı ve parola (yerel veya etki alanı kimliği) girin.

5. TermService hizmetin durumunu sorgulayın.

   ```
   sc query TermService
   ```

6. Hizmet durumunu gösteriyorsa **durduruldu**, hizmeti başlatmayı deneyin.

    ```
    sc start TermService
     ``` 

7. Hizmet yeniden hizmeti başarıyla başlatıldığını emin olmak için sorgu.

   ```
   sc query TermService
   ```
    Aldığınız hatanın alan çözüm, başlatmak hizmet başarısız olursa izleyin:

    |  Hata |  Öneri |
    |---|---|
    |5 - ERİŞİM REDDEDİLDİ |Bkz: [TermService'nin erişim reddedildi hatası nedeniyle durduruldu](#termService-service-is-stopped-because-of-access-denied-error) |
    |1058 - ERROR_SERVICE_DISABLED  |Bkz: [TermService'nin devre dışı bırakıldı.](#termService-service-is-disabled)  |
    |1059 - ERROR_CIRCULAR_DEPENDENCY |[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için|
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL|[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için|
    |1069 - ERROR_SERVICE_LOGON_FAILED  |[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için    |
    |1070 - ERROR_SERVICE_START_HANG   | [Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için  |
    |1077 - ERROR_SERVICE_NEVER_STARTED   | Bkz: [TermService'nin devre dışı bırakıldı](#termService-service-is-disabled)  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   |[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için |
    |1753   |[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için   |

#### <a name="termservice-service-is-stopped-because-of-access-denied-error"></a>Erişim reddedildi hatası nedeniyle TermService'nin durduruldu

1. Bağlanma [seri konsol](serial-console-windows.md#) ve PowerShell örneği açın.
2. İşlem izleme aracı, aşağıdaki komutu çalıştırarak yükleyin:

        remove-module psreadline  
        $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
        $destination = "c:\temp\ProcessMonitor.zip" 
        $wc = New-Object System.Net.WebClient 
        $wc.DownloadFile($source,$destination) 
3. Artık bir procmon İzlemeyi Başlat:

        procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
4. Yeniden oluşturma erişimi verme işlemi hizmet başlatarak sorun Reddet: 

        sc start TermService 
        
    Başarısız olduğunda devam edin ve işlem İzleyici sonlandırma:

        procmon /Terminate 
5. Dosya toplama **c:\temp\ProcMonTrace.PML**procmon kullanarak açın ve ardından filtre **sonucudur erişim reddedildi** aşağıdaki ekran görüntüsünde gösterilmektedir:

    ![İşlem İzleyicisi sonuca göre filtreleme](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Kayıt defteri anahtarlarını, klasör veya çıktı dosyaları düzeltin. Genellikle, bu sorun günlüğünü hizmette kullanılan hesapta kaynaklanır ACL bu nesnelere erişme izni yok. Oturum açma hesabının doğru ACL iznini bilmek sağlıklı bir VM'de kontrol edebilirsiniz. 

#### <a name="termservice-service-is-disabled"></a>TermService'nin devre dışı bırakıldı

1.  Hizmet, varsayılan başlangıç değerine geri yükleme:

        sc config TermService start= demand 
        
2.  Hizmetini başlatın:

        sc start TermService 
3.  Bir kez daha hizmetinin çalıştığından emin olmak için durumu sorgulama: sc sorgu TermService 
4.  Uzak Masaüstü kullanarak sanal makineye conntet deneyin.


### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onarın

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM'si ekleme

1. [İşletim sistemi diskini bir kurtarma VM'si ekleme](../windows/troubleshoot-recovery-disks-portal.md).
2. Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın. Bağlı disk olarak işaretlenmiş olduğundan emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Ekli işletim sistemi diski için atanan sürücü harfini unutmayın.
3.  Yükseltilmiş bir komut istemi örneği açın (**yönetici olarak çalıştır**), ve ardından aşağıdaki betiği çalıştırın. Ekli işletim sistemi diski için atanan sürücü harfini f Değiştir olduğunu varsayıyoruz, sanal makinenizin uygun değere sahip. 

        reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
        REM Set default values back on the broken service 
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
        reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
        reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
4. [İşletim sistemi diskini ve VM yeniden](../windows/troubleshoot-recovery-disks-portal.md)ve sorunun çözülüp çözülmediğini denetleyin.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.
