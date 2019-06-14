---
title: Uzak Masaüstü Hizmetleri değil bir Azure sanal makinesinde başlangıç | Microsoft Docs
description: Bir sanal makineye bağlandığınızda, Uzak Masaüstü Hizmetleri ile ilgili sorunları gidermeyi öğrenin | Microsoft Docs
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
ms.openlocfilehash: 5458a02c09a3600875c7300b27c5a87a735b2f1b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60318910"
---
# <a name="remote-desktop-services-isnt-starting-on-an-azure-vm"></a>Azure sanal makinesine Uzak Masaüstü Hizmetleri başlatma değil

Bu makalede, bir Azure sanal makinesine (VM) bağlanın ve Uzak Masaüstü Hizmetleri veya TermService, başlangıç değil ya da başlatılamıyor sorunlarını açıklar.

> [!NOTE]  
> Azure, oluşturmak ve bu kaynaklarla çalışmak için iki farklı dağıtım modeli vardır: [Azure Resource Manager ve klasik](../../azure-resource-manager/resource-manager-deployment-model.md). Bu makalede Resource Manager dağıtım modeli kullanılarak açıklanır. Bu modeli Klasik dağıtım modeli yerine yeni dağıtımlar için kullanmanızı öneririz.

## <a name="symptoms"></a>Belirtiler

Bir VM'ye bağlanmaya çalıştığınızda, aşağıdaki senaryolarda karşılaşırsınız:

- VM ekran görüntüsü, işletim sistemini tam olarak yüklenir ve kimlik bilgileri bekleniyor olduğunu gösterir.

    ![VM durumunun ekran görüntüsü](./media/troubleshoot-remote-desktop-services-issues/login-page.png)

- Uzaktan olay günlüklerini VM ile Olay Görüntüleyicisi'ni kullanarak görüntüleyin. Uzak Masaüstü Hizmetleri, TermService, başlangıç değil veya başlatılamıyor görürsünüz. Aşağıdaki günlük kaydı, bir örnek verilmiştir:

    **Oturum adı**:      Sistem </br>
    **Kaynak**:        Hizmet Denetimi Yöneticisi </br>
    **Tarih**:          16/12/2017 11:19:36: 00</br>
    **Olay Kimliği**:      7022</br>
    **Görev kategorisi**: None</br>
    **Düzey**:         Hata</br>
    **Anahtar sözcükler**:      Klasik</br>
    **Kullanıcı**:          Yok</br>
    **Bilgisayar**: vm.contoso.com</br>
    **Açıklama**: Uzak Masaüstü Hizmetleri hizmeti başlatılırken askıya alındı. 

    Seri erişim Konsolu özelliği, aşağıdaki sorguyu çalıştırarak bu hataları aramak için de kullanabilirsiniz: 

        wevtutil qe system /c:1 /f:text /q:"Event[System[Provider[@Name='Service Control Manager'] and EventID=7022 and TimeCreated[timediff(@SystemTime) <= 86400000]]]" | more 

## <a name="cause"></a>Nedeni
 
Uzak Masaüstü Hizmetleri sanal makinede çalışmadığından bu sorun oluşur. Nedeni aşağıdaki senaryolara bağlı olabilir: 

- TermService'nin kümesine **devre dışı bırakılmış**. 
- TermService'nin kilitlenme veya yanıt vermiyor. 
- TermService hatalı bir yapılandırma için başlangıç nedeniyle değil.

## <a name="solution"></a>Çözüm

Bu sorunu gidermek için seri Konsolu kullanın. Veya başka [çevrimdışı VM'yi onarın](#repair-the-vm-offline) sanal Makinenin işletim sistemi diskini bir kurtarma sanal Makinesine ekleyerek.

### <a name="use-serial-console"></a>Seri Konsolu

1. Erişim [seri konsol](serial-console-windows.md) seçerek **destek ve sorun giderme** > **seri konsol**. VM'de özelliği etkinleştirilmişse, sanal makine başarıyla bağlanabilirsiniz.

2. CMD örneği için yeni bir kanal oluşturun. Girin **CMD** kanalı başlatmak ve kanal adını almak için.

3. CMD örneğini çalıştıran kanala geçin. Bu durumda, kanal 1 olmalıdır:

   ```
   ch -si 1
   ```

4. Seçin **Enter** yeniden ve sanal makine için geçerli bir kullanıcı adı ve parola, yerel veya etki alanı kimliği girin.

5. Sorgu TermService'nin durumu:

   ```
   sc query TermService
   ```

6. Hizmet durumunu gösteriyorsa **durduruldu**, hizmeti başlatmayı deneyin:

    ```
    sc start TermService
     ``` 

7. Hizmet yeniden hizmeti başarıyla başlatıldığını emin olmak için sorgu:

   ```
   sc query TermService
   ```
8. Aldığınız hatanın alan çözüm, başlatmak hizmet başarısız olursa izleyin:

    |  Hata |  Öneri |
    |---|---|
    |5 - ERİŞİM REDDEDİLDİ |Bkz: [TermService'nin erişim reddedildi hatası nedeniyle durdurulduğunu](#termservice-service-is-stopped-because-of-an-access-denied-problem). |
    |1053 - ERROR_SERVICE_REQUEST_TIMEOUT  |Bkz: [TermService'nin devre dışı](#termservice-service-is-disabled).  |  
    |1058 - ERROR_SERVICE_DISABLED  |Bkz: [TermService'nin kilitlenmesine veya yanıt vermemeye başlıyor](#termservice-service-crashes-or-hangs).  |
    |1059 - ERROR_CIRCULAR_DEPENDENCY |[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.|
    |1067 - ERROR_PROCESS_ABORTED  |Bkz: [TermService'nin kilitlenmesine veya yanıt vermemeye başlıyor](#termservice-service-crashes-or-hangs).  |
    |1068 - ERROR_SERVICE_DEPENDENCY_FAIL|[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.|
    |1069 - ERROR_SERVICE_LOGON_FAILED  |Bkz: [TermService'nin oturum açma hatası nedeniyle başarısız oluyor](#termservice-service-fails-because-of-logon-failure) |
    |1070 - ERROR_SERVICE_START_HANG   | Bkz: [TermService'nin kilitlenmesine veya yanıt vermemeye başlıyor](#termservice-service-crashes-or-hangs). |
    |1077 - ERROR_SERVICE_NEVER_STARTED   | Bkz: [TermService'nin devre dışı](#termservice-service-is-disabled).  |
    |1079 - ERROR_DIFERENCE_SERVICE_ACCOUNT   |[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için. |
    |1753   |[Destek ekibiyle iletişime geçin](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) sorununuzun hızlıca çözülebilmesi için.   |
    
#### <a name="termservice-service-is-stopped-because-of-an-access-denied-problem"></a>Erişim reddedildi bir sorun nedeniyle TermService'nin durduruldu

1. Bağlanma [seri konsol](serial-console-windows.md) ve PowerShell örneği açın.
2. İşlem izleme aracı, aşağıdaki komutu çalıştırarak yükleyin:

   ```
   remove-module psreadline  
   $source = "https://download.sysinternals.com/files/ProcessMonitor.zip" 
   $destination = "c:\temp\ProcessMonitor.zip" 
   $wc = New-Object System.Net.WebClient 
   $wc.DownloadFile($source,$destination) 
   ```

3. Şimdi bir **procmon** izleme:

   ```
   procmon /Quiet /Minimized /BackingFile c:\temp\ProcMonTrace.PML 
   ```

4. Veren hizmete problemi **erişim reddedildi**: 

   ```
   sc start TermService 
   ```

   Başarısız olduğunda, işlem İzleyici Sonlandır:

   ```   
   procmon /Terminate 
   ```

5. Dosya toplama **c:\temp\ProcMonTrace.PML**:

    1. [VM'ye veri diski](../windows/attach-managed-disk-portal.md
).
    2. Seri konsol dosyayı yeni bir sürücüye kopyalayın kullanın. Örneğin, `copy C:\temp\ProcMonTrace.PML F:\`. Bu komutta, F bağlanan veri diskinin sürücü harfidir.
    3. Veri sürücüsü ayırma ve bir çalışan işlem İzleyicisi ubstakke yüklü olan bir VM ekleyebilirsiniz.

6. Açık **ProcMonTrace.PML** işlem İzleyicisi'ni kullanarak VM çalışma. Ardından filtre **sonucudur erişim reddedildi**aşağıdaki ekran görüntüsünde gösterildiği gibi:

    ![İşlem İzleyicisi sonuca göre filtreleme](./media/troubleshoot-remote-desktop-services-issues/process-monitor-access-denined.png)

 
6. Kayıt defteri anahtarlarını, klasörlere veya çıktı dosyaları düzeltin. Genellikle, hizmette kullanılan oturum açma hesabı ACL bu nesnelere erişme izni olmadığından, bu soruna neden olur. Oturum açma hesabı için doğru ACL izni bilmek sağlıklı bir VM'de kontrol edebilirsiniz. 

#### <a name="termservice-service-is-disabled"></a>TermService'nin devre dışı bırakıldı

1. Hizmet, varsayılan başlangıç değerine geri yükleme:

   ```
   sc config TermService start= demand 
   ```

2. Hizmetini başlatın:

   ```
   sc start TermService
   ```

3. Durumunu yeniden hizmetinin çalıştığından emin olmak için sorgu:

   ```
   sc query TermService 
   ```

4. Uzak Masaüstü kullanarak sanal Makineye bağlanmayı deneyin.

#### <a name="termservice-service-fails-because-of-logon-failure"></a>TermService'nin oturum açma hatası nedeniyle başarısız olur.

1. Bu hizmet başlangıç hesabını değiştirdiyseniz, bu sorun oluşur. Bu varsayılan değiştirildi: 

        sc config TermService obj= 'NT Authority\NetworkService'
2. Hizmetini başlatın:

        sc start TermService
3. Uzak Masaüstü kullanarak sanal Makineye bağlanmayı deneyin.

#### <a name="termservice-service-crashes-or-hangs"></a>TermService'nin kilitlenmesine veya yanıt vermemeye başlıyor
1. Hizmet durumunu takılıyorsa **başlangıç** veya **durdurma**, sonra hizmeti durdurmayı deneyin: 

        sc stop TermService
2. Kendi 'svchost' kapsayıcı hizmeti ayırma:

        sc config TermService type= own
3. Hizmetini başlatın:

        sc start TermService
4. Hizmet yine de başlatmak, başarısız olduysa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).

### <a name="repair-the-vm-offline"></a>VM'yi çevrimdışı onarın

#### <a name="attach-the-os-disk-to-a-recovery-vm"></a>İşletim sistemi diskini bir kurtarma VM'si ekleme

1. [İşletim sistemi diskini bir kurtarma VM'si ekleme](../windows/troubleshoot-recovery-disks-portal.md).
2. Kurtarma VM'sini bir Uzak Masaüstü Bağlantısı'nı başlatın. Bağlı disk olarak işaretlenmiş olduğundan emin olun **çevrimiçi** Disk Yönetimi Konsolu'nda. Ekli işletim sistemi diski için atanan sürücü harfini unutmayın.
3. Yükseltilmiş bir komut istemi örneği açın (**yönetici olarak çalıştır**). Ardından aşağıdaki betiği çalıştırın. Ekli işletim sistemi diski için atanan sürücü harfini olduğunu varsayıyoruz **F**. Vm'nizde uygun değerle değiştirin. 

   ```
   reg load HKLM\BROKENSYSTEM F:\windows\system32\config\SYSTEM.hiv
        
   REM Set default values back on the broken service 
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v start /t REG_DWORD /d 3 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService“ /f
   reg add "HKLM\BROKENSYSTEM\ControlSet001\services\TermService" /v type /t REG_DWORD /d 16 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v start /t REG_DWORD /d 3 /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v ObjectName /t REG_SZ /d "NT Authority\NetworkService" /f
   reg add "HKLM\BROKENSYSTEM\ControlSet002\services\TermService" /v type /t REG_DWORD /d 16 /f
   ```

4. [İşletim sistemi diskini ve VM yeniden](../windows/troubleshoot-recovery-disks-portal.md). Daha sonra sorun çözülmüş olup olmadığını denetleyin.

## <a name="need-help-contact-support"></a>Yardım mı gerekiyor? Desteğe başvurun

Hala yardıma ihtiyacınız varsa [desteğe](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) çözümlenen sorununuzun için.
