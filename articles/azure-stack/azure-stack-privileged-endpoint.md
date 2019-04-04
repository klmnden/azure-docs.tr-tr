---
title: Azure Stack'te ayrıcalıklı uç noktayı kullanarak | Microsoft Docs
description: Ayrıcalıklı uç noktası (CESARETLENDİRİCİ) kullanmak nasıl Azure Stack'te (Azure Stack operatörü için) gösterir.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/25/2019
ms.author: mabrigg
ms.reviewer: fiseraci
ms.lastreviewed: 01/25/2019
ms.openlocfilehash: ef75b161bcdb9e1b9658612b783dff46d1fa2502
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484347"
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Azure Stack'te ayrıcalıklı uç noktası kullanma

*Uygulama hedefi: Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Azure Stack operatörü olarak, gündelik yönetim görevlerinin çoğunda yönetici portalını, PowerShell'i veya Azure Resource Manager API'lerini kullanmanız gerekir. Ancak, bazı yaygın işlemlerin daha az kullanmanız gerekir *ayrıcalıklı uç nokta* (CESARETLENDİRİCİ). Gerekli bir görevi gerçekleştirmenize yardımcı olması için yeterli özellikleriyle sağlayan önceden yapılandırılmış bir uzak PowerShell konsolunu CESARETLENDİRİCİ var. Uç nokta kullanan [PowerShell JEA (yeterli yönetim)](https://docs.microsoft.com/powershell/jea/overview) yalnızca sınırlı bir dizi cmdlet kullanıma sunmak için. Düşük ayrıcalıklı hesap CESARETLENDİRİCİ erişmek ve kısıtlı bir cmdlet kümesi çağırmak için kullanılır. Hiç yönetici hesabı gereklidir. Ek güvenlik için komut dosyası izin verilmez.

CESARETLENDİRİCİ aşağıdaki gibi görevleri gerçekleştirmek için kullanabilirsiniz:

- Gibi alt düzey görevleri gerçekleştirmek için [tanılama günlüklerinin toplanması](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Microsoft Graph tümleştirmenin, Active Directory Federasyon Hizmetleri (AD FS) Tümleştirmesi ayarlanması dağıtımdan sonra etki alanı adı sistemi (DNS) ileticiler eklemek gibi tümleşik sistemler için birçok dağıtım sonrası datacenter tümleştirme görevleri gerçekleştirmek için Sertifika döndürme, vb.
- Geniş kapsamlı tümleşik bir sistem sorun giderme için geçici, üst düzey erişim elde etmek için destek ile çalışmak için.

Her eylem (ve karşılık gelen çıktısını) PowerShell oturumunda gerçekleştiren CESARETLENDİRİCİ kaydeder. Bu, tam şeffaflık sağlamak ve tüm işlemlerin denetimini sağlar. Gelecekteki denetimleri için bu günlük dosyalarını tutabilirsiniz.

> [!NOTE]
> Azure Stack geliştirme Seti'ni (ASDK içinde), içinde CESARETLENDİRİCİ kullanılabilir komutlardan bazıları doğrudan bir PowerShell oturumundan Geliştirme Seti ana bilgisayarda çalıştırabilirsiniz. Ancak, bu tek yöntem tümleşik sistemleri ortamında belirli işlemlerin gerçekleştirilmesi kullanılabilir olduğundan kullanarak günlük toplama gibi CESARETLENDİRİCİ bazı işlemleri test etmek isteyebilirsiniz.

## <a name="access-the-privileged-endpoint"></a>Ayrıcalıklı uç noktasına erişmesi

CESARETLENDİRİCİ CESARETLENDİRİCİ sanal makinede uzak PowerShell oturumu aracılığıyla erişin. Bu sanal makine ASDK adlandırılır **AzS-ERCS01**. Tümleşik bir sistem kullanıyorsanız, her çalışan CESARETLENDİRİCİ üç örneği vardır bir sanal makine içinde (*önek*-ERCS01, *önek*-ERCS02, veya *önek*- ERCS03) dayanıklılık için farklı konaklarda. 

Tümleşik bir sistem için bu yordama başlamadan önce IP adresi veya DNS aracılığıyla CESARETLENDİRİCİ erişebildiğinden emin olun. Azure Stack ilk dağıtımdan sonra DNS tümleştirme henüz ayarlanmamış olduğundan yalnızca IP adresine göre CESARETLENDİRİCİ erişebilir. OEM donanım satıcınıza adlı bir JSON dosyası sağlayacak **AzureStackStampDeploymentInfo** , CESARETLENDİRİCİ IP adreslerini içerir.


> [!NOTE]
> Güvenlik nedenleriyle, biz size CESARETLENDİRİCİ için yalnızca bir sağlamlaştırılmış sanal makine çalıştırma donanım yaşam döngüsü konak üzerinde veya ayrılmış, güvenli bir bilgisayar gibi bağlanmasını gerektirecek bir [ayrıcalıklı erişim iş istasyonu](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations). Ya da bunu için CESARETLENDİRİCİ bağlanmak için kullanılması gereken özgün donanım yaşam döngüsü konağın yapılandırmasını yeni bir yazılım yükleme dahil olmak üzere kendi özgün yapılandırmasından değiştirilmemelidir.

1. Güven oluşturun.

    - Tümleşik bir sistemde CESARETLENDİRİCİ donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonu çalışan sağlamlaştırılmış sanal makinede güvenilir bir konak olarak eklemek için yükseltilmiş bir Windows PowerShell oturumunda aşağıdaki komutu çalıştırın.

      ```powershell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ```
    - ASDK çalıştırıyorsanız, Geliştirme Seti ana bilgisayara oturum açın.

2. Sağlamlaştırılmış donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonu üzerinde çalışan sanal makinenin üzerinde bir Windows PowerShell oturumu açın. CESARETLENDİRİCİ sanal makinede uzaktan oturum oluşturmak için aşağıdaki komutları çalıştırın:
 
   - Tümleşik bir sistem üzerinde:
     ```powershell
       $cred = Get-Credential

       Enter-PSSession -ComputerName <IP_address_of_ERCS> `
         -ConfigurationName PrivilegedEndpoint -Credential $cred
     ```
     `ComputerName` Parametresi, IP adresi veya DNS adını CESARETLENDİRİCİ barındıran sanal makinelerden birinde olabilir. 
   - ASDK çalıştırıyorsanız:
     
     ```powershell
       $cred = Get-Credential

       Enter-PSSession -ComputerName azs-ercs01 `
         -ConfigurationName PrivilegedEndpoint -Credential $cred
     ``` 
     İstendiğinde aşağıdaki kimlik bilgilerini kullanın:

     - **Kullanıcı adı**: CloudAdmin hesabı, belirttiğiniz biçimde  **&lt; *Azure Stack etki*&gt;\cloudadmin**. (ASDK için kullanıcı adı. **azurestack\cloudadmin**.)
     - **Parola**: AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

     > [!NOTE]
     > ERCS uç noktasına bağlantı kuramazsa, bir ve ikinci adımlar için zaten bağlanmak denemediniz ERCS VM'deki IP adresini kullanarak yeniden deneyin.

3. Bağlandıktan sonra istemde değişir **[*IP adresi veya ERCS VM adı*]: PS >** veya **[azs-ercs01]: PS >** ortamı bağlı olarak. Buradan, çalıştırma `Get-Command` kullanılabilir cmdlet'lerin listesini görüntülemek için.

   Bu cmdlet'lerin çoğu yalnızca tümleşik sistem ortamları (örneğin, veri merkezi tümleştirmesiyle ilgili cmdlet'ler) için tasarlanmıştır. Aşağıdaki cmdlet ASDK içinde doğrulandı:

   - Konak Temizle
   - PrivilegedEndpoint Kapat
   - Çıkış-PSSession
   - Get-AzureStackLog
   - Get-AzureStackStampInformation
   - Get-Command
   - Get-FormatData
   - Get-Help
   - Get-ThirdPartyNotices
   - Ölçü nesnesi
   - Yeni CloudAdminUser
   - Varsayılan dışarı
   - Remove-CloudAdminUser
   - Select-Object
   - Set-CloudAdminUserPassword
   - Test-AzureStack
   - Stop-AzureStack
   - Get-ClusterLog

## <a name="tips-for-using-the-privileged-endpoint"></a>Ayrıcalıklı uç noktası kullanımıyla ilgili ipuçları 

Yukarıda belirtildiği gibi CESARETLENDİRİCİ olduğu bir [PowerShell JEA](https://docs.microsoft.com/powershell/jea/overview) uç noktası. Bir JEA uç noktası bir güçlü güvenlik katman sağlamasının yanı sıra, bazı komut dosyası veya sekme tamamlama gibi temel PowerShell özelliklerini azaltır. Komut dosyası işlemi herhangi bir türde çalışırsanız, işlem hatasıyla başarısız **ScriptsNotAllowed**. Bu beklenen bir davranıştır.

Bu nedenle, örneğin, belirli bir cmdlet için parametrelerin listesini almak için aşağıdaki komutu çalıştırın:

```powershell
    Get-Command <cmdlet_name> -Syntax
```

Alternatif olarak, [Import-PSSession](https://docs.microsoft.com/powershell/module/Microsoft.PowerShell.Utility/Import-PSSession?view=powershell-5.1) yerel makinenizdeki geçerli oturuma tüm CESARETLENDİRİCİ cmdlet'lerini içeri aktarmak için cmdlet'ini. Bunu yaptığınızda, tüm cmdlet'ler ve İşlevler CESARETLENDİRİCİ, artık genel olarak, komut yerel makinenizde, sekme tamamlama ve daha fazlası ile birlikte kullanılabilir. 

Yerel makinenizde CESARETLENDİRİCİ oturumun içeri aktarmak için aşağıdaki adımları uygulayın:

1. Güven oluşturun.

    -Tümleşik bir sistem üzerinde CESARETLENDİRİCİ donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonu çalışan sağlamlaştırılmış sanal makinede güvenilir bir konak olarak eklemek için yükseltilmiş bir Windows PowerShell oturumunda aşağıdaki komutu çalıştırın.

      ```powershell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ```
    - ASDK çalıştırıyorsanız, Geliştirme Seti ana bilgisayara oturum açın.

2. Sağlamlaştırılmış donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonu üzerinde çalışan sanal makinenin üzerinde bir Windows PowerShell oturumu açın. CESARETLENDİRİCİ sanal makinede uzaktan oturum oluşturmak için aşağıdaki komutları çalıştırın:
 
   - Tümleşik bir sistem üzerinde:
     ```powershell
       $cred = Get-Credential

       $session = New-PSSession -ComputerName <IP_address_of_ERCS> `
         -ConfigurationName PrivilegedEndpoint -Credential $cred
     ```
     `ComputerName` Parametresi, IP adresi veya DNS adını CESARETLENDİRİCİ barındıran sanal makinelerden birinde olabilir. 
   - ASDK çalıştırıyorsanız:
     
     ```powershell
      $cred = Get-Credential

      $session = New-PSSession -ComputerName azs-ercs01 `
         -ConfigurationName PrivilegedEndpoint -Credential $cred
     ``` 
     İstendiğinde aşağıdaki kimlik bilgilerini kullanın:

     - **Kullanıcı adı**: CloudAdmin hesabı, belirttiğiniz biçimde  **&lt; *Azure Stack etki*&gt;\cloudadmin**. (ASDK için kullanıcı adı. **azurestack\cloudadmin**.)
     - **Parola**: AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.

3. Yerel makinenize CESARETLENDİRİCİ oturumun içeri aktarma
    ```powershell 
        Import-PSSession $session
    ```
4. Şimdi, sekme tamamlamayı kullanma ve her zaman olduğu gibi tüm işlevleri ve CESARETLENDİRİCİ cmdlet'lerin yerel PowerShell oturumunuzda üzerinde Azure Stack güvenlik duruşunu azaltmayı olmadan komut gerçekleştirebilirsiniz. Keyfini çıkarın!


## <a name="close-the-privileged-endpoint-session"></a>Ayrıcalıklı uç nokta Oturumu Kapat

 Daha önce belirtildiği gibi her eylem (ve karşılık gelen çıktısını) PowerShell oturumunda gerçekleştiren CESARETLENDİRİCİ günlüğe kaydeder. Kullanarak oturumu kapatmanız gerekir `Close-PrivilegedEndpoint` cmdlet'i. Bu cmdlet doğru uç noktaya kapatır ve günlük dosyaları bekletme için bir dış dosya paylaşımına aktarır.

Uç nokta oturumu kapatmak için:

1. CESARETLENDİRİCİ tarafından erişilebilen bir dış dosya paylaşımı oluşturun. Bir geliştirme seti ortamında geliştirme seti konakta yalnızca bir dosya paylaşımı oluşturabilirsiniz.
2. Çalıştırma `Close-PrivilegedEndpoint` cmdlet'i. 
3. Transkript günlük dosyasının depolanacağı bir yolu için istenir. Biçiminde daha önce oluşturduğunuz bir dosya paylaşımı belirtin &#92; &#92; *servername*&#92;*sharename*. Bir yol belirtmezseniz, cmdlet başarısız olur ve oturumu açık kalır. 

    ![Transkript hedef yolu belirttiğiniz gösteren Kapat PrivilegedEndpoint cmdlet çıkışı](media/azure-stack-privileged-endpoint/closeendpoint.png)

Transkript günlük dosyaları, dosya paylaşımına başarıyla aktarıldıktan sonra otomatik olarak CESARETLENDİRİCİ silindi. 

> [!NOTE]
> Cmdlet'lerini kullanarak CESARETLENDİRİCİ oturumu kapatmak, `Exit-PSSession` veya `Exit`, veya PowerShell konsolunu kapatmanız yeterlidir, döküm günlükleri bir dosya paylaşımına aktarılmıyor. İçinde CESARETLENDİRİCİ kalırlar. Bir sonraki çalıştırmanızda `Close-PrivilegedEndpoint` ve bir dosya paylaşımı eklemek, önceki döküm günlüklerinden de oturumlara aktarır. Kullanmayın `Exit-PSSession` veya `Exit` ; CESARETLENDİRİCİ oturumu kapatmak için kullanmak `Close-PrivilegedEndpoint` yerine.


## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack tanılama araçları](azure-stack-diagnostics.md)
