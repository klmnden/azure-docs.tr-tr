---
title: "Azure yığınında kullanan ayrıcalıklı uç noktasını | Microsoft Docs"
description: "Ayrıcalıklı endpoint Azure yığınında (Azure yığın işleci için) kullanmayı gösterir."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: e94775d5-d473-4c03-9f4e-ae2eada67c6c
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 80c3f248edb40b66e3177c512f3caf77295c6c5d
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/12/2018
---
# <a name="using-the-privileged-endpoint-in-azure-stack"></a>Azure yığınında ayrıcalıklı uç noktası kullanma

*Uygulandığı öğe: Azure yığın tümleşik sistemleri ve Azure yığın Geliştirme Seti*

Bir Azure yığın operatör olarak, Yönetici portalı'nı, PowerShell veya Azure Resource Manager API'leri en günlük yönetim görevleri için kullanmanız gerekir. Ancak, bazı ortak işlemleri daha az kullanmanız gerekir *ayrıcalıklı endpoint* (CESARETLENDİRİCİ). Bu uç noktaya gerekli bir görevi gerçekleştirmenize yardımcı olmak için yeterli yetenekleri sağlayan bir önceden yapılandırılmış Uzaktan PowerShell konsoludur. Uç nokta PowerShell cmdlet'leri yalnızca sınırlı sayıda kullanıma sunmak için JEA (yalnızca yeterince yönetim) yararlanır. Ayrıcalıklı uç noktasına erişmek ve kısıtlı bir cmdlet kümesi çağırmak için bir düşük ayrıcalıklı hesap kullanılır. Hiç yönetici hesabı gereklidir. Ek güvenlik için komut dosyası izin verilmiyor.

Ayrıcalıklı uç noktası aşağıdaki gibi görevleri gerçekleştirmek için kullanabilirsiniz:

- Alt düzey görevler gerçekleştirmesini [tanılama günlüklerinin toplanması](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostics#log-collection-tool).
- Dağıtım ayarı grafik tümleştirme, Active Directory Federasyon Hizmetleri (AD FS) tümleştirmesi, sertifika sonra etki alanı adı sistemi (DNS) ileticiler ekleme gibi tümleşik sistemler için birçok dağıtım sonrası datacenter tümleştirme görevleri gerçekleştirmek için döndürme, vb.
- Tümleşik bir sistemde geniş kapsamlı sorun giderme için geçici, üst düzey erişim elde etmek için destek çalışmak için. 

Ayrıcalıklı uç nokta her eylem (ve karşılık gelen çıktısını) PowerShell oturumunda gerçekleştiren kaydeder. Bu, tam saydamlığı ve işlemlerini tam denetim sağlar. Gelecekte yapılacak denetim için bu günlük dosyaları koruyabilirsiniz.

> [!NOTE]
> Azure yığın Geliştirme Seti (ASDK), bazı ayrıcalıklı uç kullanılabilir komutlar doğrudan PowerShell oturumundan Geliştirme Seti konakta çalıştırabilirsiniz. Ancak, bu tümleşik sistemleri ortamında belirli işlemlerin gerçekleştirilmesi kullanabileceğiniz tek yöntem olduğu için gerçekleştirdiğiniz günlük toplama gibi ayrıcalıklı uç nokta kullanarak bazı işlemleri test etmek isteyebilirsiniz.

## <a name="access-the-privileged-endpoint"></a>Ayrıcalıklı uç noktasına erişmek

Uzak PowerShell oturumuna ayrıcalıklı uç noktasını barındıran sanal makinede üzerinden ayrıcalıklı uç noktasına erişmek. İçinde ASDK, bu sanal makine AzS ERCS01 olarak adlandırılır. Tümleşik bir sistem kullanıyorsanız, üç örneği vardır ayrıcalıklı uç noktası, her bir sanal makinede çalışan (*önek*-ERCS01, *önek*-ERCS02, veya *önek*-ERCS03) dayanıklılık için farklı konaklarda. 

Tümleşik bir sistem için bu yordama başlamadan önce IP adresi veya DNS üzerinden ayrıcalıklı uç noktasına erişebildiğinden emin olun. Azure yığın ilk dağıtımdan sonra DNS tümleştirme henüz ayarlanmamış olduğundan yalnızca IP adresine göre ayrıcalıklı uç noktasına erişebilirsiniz. OEM donanım satıcınıza "ayrıcalıklı uç noktası IP adreslerini içeren AzureStackStampDeploymentInfo" adlı bir JSON dosyası ile sağlayacaktır.

Ayrıcalıklı uç noktasına yalnızca donanım yaşam döngüsü konak veya ayrılmış, kilitli bir bilgisayardan gibi bağlanmasını öneririz bir [ayrıcalıklı erişim iş istasyonu](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations).

1. Ortamınıza bağlı olarak aşağıdakilerden birini yapın:

    - Tümleşik bir sistemde ayrıcalıklı endpoint donanım yaşam döngüsü konak ya da ayrıcalıklı erişim iş istasyonu güvenilen bir konak olarak eklemek için aşağıdaki komutu çalıştırın.

      ````PowerShell
        winrm s winrm/config/client '@{TrustedHosts="<IP Address of Privileged Endpoint>"}'
      ````
    - ADSK çalıştırıyorsanız, Geliştirme Seti ana bilgisayara oturum açın.

2. Donanım yaşam döngüsü konak veya ayrıcalıklı erişim iş istasyonu üzerinde yükseltilmiş bir Windows PowerShell oturumu açın. Ayrıcalıklı uç noktasını barındıran sanal makinede uzak oturum oluşturmak için aşağıdaki komutları çalıştırın:
 
    - Tümleşik bir sistemde:
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName <IP_address_of_ERCS>`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ````
      `ComputerName` Parametresi, IP adresini veya DNS adını ayrıcalıklı bir uç noktasını barındıran sanal makinelerden biri olabilir. 
    - ADSK çalıştırıyorsanız:
     
      ````PowerShell
        $cred = Get-Credential

        Enter-PSSession -ComputerName azs-ercs01`
          -ConfigurationName PrivilegedEndpoint -Credential $cred
      ```` 
   İstendiğinde, aşağıdaki kimlik bilgilerini kullanın:

      - **Kullanıcı adı**: CloudAdmin hesabı biçiminde belirtin  **&lt;* Azure yığın etki alanı*&gt;\cloudadmin**. (ASDK için kullanıcı adı. **azurestack\cloudadmin**.)
      - **Parola**: AzureStackAdmin etki alanı yönetici hesabı için yükleme sırasında sağlanan parolanın aynısını girin.
    
3.  Bağlandıktan sonra istemi değiştirir  **[*IP adresi veya ERCS VM adı*]: PS > ** veya **[azs-ercs01]: PS >**ortamına bağlı olarak. Buradan, çalıştırmak `Get-Command` kullanılabilir cmdlet'lerinin listesini görüntülemek için.

    ![Get-Command cmdlet'i çıkış gösteren kullanılabilecek komutların listesi](media/azure-stack-privileged-endpoint/getcommandoutput.png)

    Bu cmdlet'lerin çoğu yalnızca tümleşik sistemi ortamları (örneğin, veri merkezi tümleştirmesiyle ilgili cmdlet'leri) yöneliktir. Aşağıdaki cmdlet ASDK içinde doğrulandı:

    - Clear-ana bilgisayar
    - Kapat PrivilegedEndpoint
    - Çıkış-PSSession
    - Get-AzureStackLog
    - Get-AzureStackStampInformation
    - Get-Command
    - Get-FormatData
    - Get-Help
    - Get-ThirdPartyNotices
    - Ölçü nesnesi
    - CloudAdminUser yeni
    - Dışarı varsayılan
    - Remove-CloudAdminUser
    - Select-Object
    - Set-CloudAdminUserPassword
    - Test-AzureStack
    - Stop-AzureStack
    - Get-ClusterLog

4.  Komut dosyası izin verilmediğinden, sekme tamamlama parametre değerleri için kullanamazsınız. Belirli bir cmdlet için parametre listesini almak için aşağıdaki komutu çalıştırın:

    ````PowerShell
    Get-Command <cmdlet_name> -Syntax
    ```` 
    Komut dosyası işlemi herhangi bir türde çalışırsanız, işlemi şu hata ile başarısız oluyor **ScriptsNotAllowed**. Bu beklenen bir davranıştır.

## <a name="close-the-privileged-endpoint-session"></a>Ayrıcalıklı endpoint oturumunu kapatma

 Daha önce belirtildiği gibi ayrıcalıklı uç nokta her eylem (ve karşılık gelen çıktısını) PowerShell oturumunda gerçekleştiren günlüğe kaydeder. Kullanarak oturumunu kapatmalısınız `Close-PrivilegedEndpoint` cmdlet'i. Bu cmdlet doğru uç nokta kapatır ve bir dış dosya paylaşımı bekletme için günlük dosyaları aktarır.

Uç nokta oturumu kapatmak için:

1. Ayrıcalıklı bitiş noktası tarafından erişilebilen bir harici dosya paylaşımı oluşturun. Bir geliştirme seti ortamında, yalnızca bir dosya paylaşımı Geliştirme Seti konakta oluşturabilirsiniz.
2. Çalıştırma `Close-PrivilegedEndpoint` cmdlet'i. 
3. Dökümü günlük dosyasının depolanacağı yol için istenir. Biçim &#92; &#92;daha önce oluşturduğunuz dosya paylaşımı belirtin; *servername*&#92; *ShareName*. Bir yol belirtmezseniz, cmdlet başarısız olur ve oturumu açık kalır. 

    ![Dökümü hedef yolu belirlediğiniz gösterir Kapat PrivilegedEndpoint cmdlet çıktısında](media/azure-stack-privileged-endpoint/closeendpoint.png)

Dökümü günlük dosyaları dosya paylaşımına başarıyla aktarıldıktan sonra otomatik olarak ayrıcalıklı uç noktasından silinmiş. Cmdlet'lerini kullanarak ayrıcalıklı endpoint oturumu kapatmak, `Exit-PSSession` veya `Exit`, veya PowerShell Konsolu kapatmanız yeterlidir, dökümü günlükleri için bir dosya paylaşımı transfer yok. Ayrıcalıklı uç kalırlar. Sonraki çalıştırmanızda `Close-PrivilegedEndpoint` ve bir dosya paylaşımı içerir, önceki dökümü günlüklerinden oturumlara ayrıca aktarır.

## <a name="next-steps"></a>Sonraki adımlar
[Azure yığın tanılama araçları](azure-stack-diagnostics.md)







