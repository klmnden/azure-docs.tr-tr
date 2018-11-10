---
title: Azure Stack gizli döndürme | Microsoft Docs
description: Azure stack'teki dizilerinizin döndürme hakkında bilgi edinin.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/06/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: 5d2f4fc77d5849dc2be80ada9610098c9a381f92
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244109"
---
# <a name="rotate-secrets-in-azure-stack"></a>Azure Stack gizli Döndür

*Bu yönergeler yalnızca Azure Stack tümleşik sistemleri sürüm 1803 ve sonrası için geçerlidir. Gizli anahtar döndürme öncesi 1802 Azure Stack sürümlerinde kullanmayın*

Azure Stack Azure Stack altyapı kaynaklarını ve hizmetleri arasında güvenli iletişim sağlamak için çeşitli gizli diziler kullanır.

- **İç gizli dizileri**  
Tüm sertifikalar, parolalar, güvenli dizeleri ve Azure Stack operatörü müdahalesi olmadan Azure Stack altyapısı tarafından kullanılan anahtarlar. 

- **Dış gizli anahtarları**  
Azure Stack operatör tarafından sağlanan dönük hizmetleri altyapısı hizmet sertifikaları. Bu sertifikaları için aşağıdaki hizmetleri içerir: 
    - Yönetici portalı 
    - Genel kullanıma açık portala 
    - Yönetici Azure Resource Manager 
    - Küresel Azure Resource Manager 
    - Yönetici Keyvault 
    - Key Vault 
    - ACS (blob, tablo ve kuyruk depolama dahil) 
    - ADFS<sup>*</sup>
    - Graf<sup>*</sup>

   <sup>*</sup> Yalnızca ortamın kimlik sağlayıcısı Active Directory Federasyon Hizmetleri'nde (AD FS) ise geçerlidir.

> [!NOTE]
> Diğer tüm anahtarları ve BMC dahil olmak üzere dizeleri, güvenli ve parolaları geçiş, kullanıcı ve yönetici hesabı parolalarını yine de el ile yönetici tarafından güncelleştirilir. 

Azure Stack altyapısının bütünlüğünü korumak için düzenli aralıklarla altyapılarını ait gizli dizileri, kuruluşun güvenlik gereksinimleriyle uyumlu sıklık, döndürme olanağı işleçleri gerekir.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Yeni bir sertifika yetkilisinden dış sertifikalara sahip gizli anahtarları döndürme

Azure Stack ile dış sertifikalar bir yeni sertifika yetkilisi (CA) öğesinden gizli döndürme şu bağlamlarda destekler:

|Yüklü sertifika CA|CA için döndürmek için|Desteklenen|Azure Stack sürümleri desteklenir|
|-----|-----|-----|-----|
|Gelen otomatik olarak imzalanan|Kuruluş|Desteklenmiyor||
|Gelen otomatik olarak imzalanan|Kendinden imzalı|Desteklenmiyor||
|Gelen otomatik olarak imzalanan|Ortak<sup>*</sup>|Desteklenen|1803 ve sonrası|
|Kuruluştan|Kuruluş|Dağıtım sırasında kullanılan CA aynı kurumsal müşterilerin kullandığı sürece desteklenir|1803 ve sonrası|
|Kuruluştan|Kendinden imzalı|Desteklenmiyor||
|Kuruluştan|Ortak<sup>*</sup>|Desteklenen|1803 ve sonrası|
|Genel kullanıma<sup>*</sup>|Kuruluş|Desteklenmiyor|1803 ve sonrası|
|Genel kullanıma<sup>*</sup>|Kendinden imzalı|Desteklenmiyor||
|Genel kullanıma<sup>*</sup>|Ortak<sup>*</sup>|Desteklenen|1803 ve sonrası|

<sup>*</sup> Ortak sertifika yetkilileri Windows güvenilen kök programının olanlar aşağıda verilmiştir. Tam listesini bulabilirsiniz [Microsoft güvenilen kök sertifika programı: katılımcıları (27 Haziran 2017'den itibaren)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).

## <a name="alert-remediation"></a>Uyarı düzeltme

30 gün içinde sona erme gizlidir, aşağıdaki uyarıları Yönetici portalı'nda üretilir: 

- Bekleyen hizmet hesabının parola süresinin sonu 
- Bekleyen bir iç sertifika süre sonu 
- Bekleyen dış sertifika süre sonu 

Aşağıdaki yönergeleri kullanarak gizli döndürmeye çalışıyor, bu uyarıları düzeltin.

## <a name="pre-steps-for-secret-rotation"></a>Gizli anahtar döndürme öncesi adımları

   > [!IMPORTANT]  
   > Gizli döndürme ortamınızda başarılı bir şekilde yürütülen taşınmadığından emin olun. Gizli anahtar döndürme gerçekleştirilmiş, Azure Stack 1807 veya gizli döndürme yürütmeden önce daha sonraki sürüme güncelleştirin. 

1.  İşleçler, açın ve Azure Stack gizli dizilerinin dönüşü sırasında otomatik olarak Kapat uyarılar görebilirsiniz.  Bu davranış beklenir ve uyarıların yoksayılabilir.  İşleçler, Test AzureStack çalıştırarak bu uyarılar geçerliliğini doğrulayabilirsiniz.  İzlemek için SCOM kullanma işleçleri için bir sistem bakım moduna alma, Azure Stack sistemleri bu uyarılar ITSM sistemlerine erişmesini engeller ancak Azure Stack sistemi ulaşılamaz hale gelirse uyaracak şekilde devam eder. 
2. Herhangi bir bakım işlemleri, kullanıcılara bildirin. Normal bakım pencereleri, çalışma saatleri sırasında mümkün olduğunca, zamanlayın. Bakım işlemleri, kullanıcı iş yükleri hem portal işlemlerini etkileyebilir.
    > [!note]  
    > Sonraki adımlar yalnızca Azure Stack dış gizli anahtarları döndürme yaparken geçerlidir.

3. Çalıştırma **[Test AzureStack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test)** ve tüm test çıkışları gizli anahtarları döndürme önce sağlıklı olduğunu onaylayın.
4. Yeni bir değişiklik kümesi dış sertifikalar hazırlayın. Yeni küme özetlenen sertifika belirtimleri eşleşen [Azure Stack PKI sertifikası gereksinimleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs).
5.  Bir geri dönüş güvenli yedekleme konumu için kullanılan sertifikaları kadar Store. Döndürme çalıştırır ve ardından başarısız olursa, dönüş yeniden çalıştırmadan önce dosya paylaşımında sertifika yedek kopyaları değiştirin. Unutmayın, yedek kopyaları güvenli yedekleme yerde saklayın.
6.  ERCS Vm'lerden erişebileceğiniz bir dosya paylaşımı oluşturun. Dosya Paylaşımı okunabilir ve yazılabilir için **CloudAdmin** kimlik.
7.  PowerShell ISE konsolunda paylaşımına erişimine sahip olduğu bir bilgisayardan açın. Dosya paylaşımına gidin. 
8.  Çalıştırma **[CertDirectoryMaker.ps1](https://www.aka.ms/azssecretrotationhelper)** , dış sertifikalar için gerekli dizini oluşturmak için.

## <a name="rotating-external-and-internal-secrets"></a>İç ve dış gizli anahtarları döndürme

Her iki dış iç bir gizli dizi döndürmek için:

1. Yeni oluşturulan içinde **/Sertifikalar** öncesi adımlarda oluşturulan dizin, dizin yapısında zorunlu sertifikaları bölümünde özetlenen biçimde göre değiştirme dış sertifikalar yeni kümesini yerleştirin ' ın [Azure Stack PKI sertifikası gereksinimleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).
2. Bir PowerShell oturumu oluşturma [ayrıcalıklı uç nokta](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) kullanarak **CloudAdmin** hesap ve oturumları bir değişkene depolayın. Bu değişken sonraki adımda parametre olarak kullanır.

    > [!IMPORTANT]  
    > Oturum girin değil, oturumu bir değişkene depolayın.
    
3. Çalıştırma  **[Invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)**. Ayrıcalıklı uç nokta PowerShell oturumu Değişkeninizi olarak geçirmek **oturumu** parametresi. 
4. Çalıştırma **başlangıç SecretRotation** şu parametrelerle:
    - **PfxFilesPath**  
    Daha önce oluşturduğunuz sertifikalar dizininize ağ yolunu belirtin.  
    - **PathAccessCredential**  
    Paylaşım kimlik bilgilerini bir PSCredential nesnesi. 
    - **CertificatePassword**  
    Tüm oluşturulan pfx sertifika dosyaları için kullanılan parolayı güvenli bir dize.
4. Gizli anahtarlarınız döndürme bekleyin.  
Gizli anahtar döndürme işlemi başarıyla tamamlandığında, Konsolunuzda görüntülenir **genel eylem durumu: başarılı**. 
    > [!note]  
    > Gizli döndürme başarısız olursa hata iletisindeki yönergeleri izleyin ve başlangıç secretrotation ile yeniden çalıştırın **-yeniden** parametresi. Gizli anahtar döndürme hataları yaşıyorsanız desteğe başvurun yinelenir. 
5. Başarılı tamamlama gizli döndürme sonra öncesi adımda oluşturduğunuz paylaşımından sertifikalarınızı ve güvenli yedekleme konumlarında depolayabilirsiniz. 

## <a name="walkthrough-of-secret-rotation"></a>İzlenecek yol gizli döndürme

Aşağıdaki PowerShell örneği, cmdlet'leri ve parametreleri dizilerinizin döndürmek için çalıştırılacak gösterir.

```powershell
#Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IPofERCSMachine>"}'
$PEPCreds = Get-Credential 
$PEPsession = New-PSSession -computername <IPofERCSMachine> -Credential $PEPCreds -ConfigurationName PrivilegedEndpoint 

#Run Secret Rotation
$CertPassword = ConvertTo-SecureString "Certpasswordhere" -AsPlainText -Force
$CertShareCred = Get-Credential 
$CertSharePath = "<NetworkPathofCertShare>"
Invoke-Command -session $PEPsession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword }
Remove-PSSession -Session $PEPSession
```
## <a name="rotating-only-internal-secrets"></a>Yalnızca iç gizli anahtarları döndürme

Yalnızca Azure Stack'ın iç gizli döndürmek için:

1. Bir PowerShell oturumu oluşturma [ayrıcalıklı uç nokta](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Ayrıcalıklı uç nokta oturumda çalıştırılan **başlangıç SecretRotation** bağımsız değişken olmadan.
3. Gizli anahtarlarınız döndürme bekleyin.  
Gizli anahtar döndürme işlemi başarıyla tamamlandığında, Konsolunuzda görüntülenir **genel eylem durumu: başarılı**. 
    > [!note]  
    > Gizli anahtar döndürme başarısız olursa hata iletisindeki yönergeleri izleyin ve başlangıç secretrotation ile yeniden **-yeniden** parametresi. Gizli anahtar döndürme hataları yaşıyorsanız desteğe başvurun yinelenir. 

## <a name="start-secretrotation-reference"></a>Başlangıç SecretRotation başvurusu

Bir Azure Stack sisteminin gizli dizileri döndürür. Yalnızca Azure Stack ayrıcalıklı uç noktasına karşı yürütülür.

### <a name="syntax"></a>Sözdizimi

Yol (varsayılan)

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

### <a name="description"></a>Açıklama

Başlangıç SecretRotation cmdlet'i, bir Azure Stack sisteminin altyapı gizli dizileri döndürür. Varsayılan olarak, iç altyapı ağına kullanıma sunulan tüm gizli dizilerin döndürür kullanıcı girişi ile ayrıca tüm dış ağ altyapı uç noktaları'ların sertifikalarını döndürür. Dış ağ altyapı uç noktalarına döndürürken başlangıç SecretRotation oturumu parametre olarak geçirilen Azure Stack ortamının ayrıcalıklı uç nokta oturumu ile bir Invoke-Command betik bloğu yürütülmelidir.
 
### <a name="parameters"></a>Parametreler

| Parametre | Tür | Gerekli | Konumu | Varsayılan | Açıklama |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Dize  | False  | adlı  | None  | Fileshare yolu **\Certificates** tüm dış içeren dizin ağ uç noktası sertifikaları. Yalnızca dış gizli anahtarları veya tüm gizli dizilerin döndürürken gereklidir. Son dizin olmalıdır **\Certificates**. |
| CertificatePassword | SecureString | False  | adlı  | None  | -PfXFilesPath sağlanan tüm sertifikalar için parola. İç ve dış gizli anahtarları Döndürülmüş PfxFilesPath sağlanır değeri gereklidir. |
| PathAccessCredential | PSCredential | False  | adlı  | None  | PowerShell kimlik bilgilerini dosya paylaşımını **\Certificates** tüm dış içeren dizin ağ uç noktası sertifikaları. Yalnızca dış gizli anahtarları veya tüm gizli dizilerin döndürürken gereklidir.  |
| Yeniden çalıştır | SwitchParameter | False  | adlı  | None  | Başarısız bir girişimden sonra gizli döndürme yeniden denenen herhangi bir zamanda yeniden kullanılması gerekir. |

### <a name="examples"></a>Örnekler
 
**Yalnızca iç altyapı gizli dizileri Döndür**

```powershell  
PS C:\> Start-SecretRotation  
```

Bu komut tüm Azure Stack iç ağa kullanıma sunulan altyapı gizli dizileri döndürür. Yığın tarafından oluşturulan tüm gizli dizilerin başlangıç SecretRotation döndürür, ancak sağlanan sertifika olduğundan, dış uç noktası sertifikaları değil döndürülür.  

**İç ve dış altyapı gizli dizileri Döndür**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Bu komut tüm Azure Stack iç ağa kullanıma sunulan altyapı gizli dizileri ve bunun yanı sıra Azure yığını'nın dış ağ altyapı uç noktaları için kullanılan TLS sertifikalarını döndürür. Yığın tarafından oluşturulan tüm gizli dizilerin başlangıç SecretRotation döndürür ve sağlanan sertifikalar çünkü dış uç noktası sertifikaları da döndürülür.  


## <a name="update-the-baseboard-management-controller-bmc-password"></a>Temel Kart Yönetim denetleyicisine (BMC) parolasını güncelleştirin

Temel Kart Yönetim denetleyicisine (BMC) fiziksel sunucularınızı durumunu izler. BMC parolasını güncelleştirme hakkında yönergeler ve belirtimleri orijinal ekipman üreticisi (OEM) donanım satıcınıza göre değişir. Parolalarınızı Azure Stack normal temposu bileşenler için güncelleştirmeniz gerekir.

1. Azure yığını'nın fiziksel sunucularda BMC, OEM yönergelerini izleyerek güncelleştirin. Ortamınızdaki her BMC için parola aynı olmalıdır.
2. Ayrıcalıklı bir uç nokta, Azure Stack oturumunu açın. Yönergeler için bkz [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md).
3. Sonra bir PowerShell istemi için değişti **[IP adresi veya ERCS VM adı]: PS >** veya **[azs-ercs01]: PS >** çalıştırabileceğiniz bir ortam, bağlı olarak `Set-BmcPassword` çalıştırarak `invoke-command`. Ayrıcalıklı uç nokta oturum değişkeni, bir parametre olarak geçiriyoruz. Örneğin:

    ```powershell
    # Interactive Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PECred = Get-Credential "<Domain>\CloudAdmin" -Message "PE Credentials" 
    $NewBMCpwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```
    
    Kod satırları parolalarıyla statik PowerShell sürümünü de kullanabilirsiniz:
    
    ```powershell
    # Static Version
    $PEip = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEUser = "<Privileged Endpoint user for exmaple Domain\CloudAdmin>"
    $PEpwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PECred = New-Object System.Management.Automation.PSCredential ($PEUser, $PEpwd) 
    $NewBMCpwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force 

    $PEPSession = New-PSSession -ComputerName $PEip -Credential $PECred -ConfigurationName "PrivilegedEndpoint" 

    Invoke-Command -Session $PEPSession -ScriptBlock {
        Set-Bmcpassword -bmcpassword $using:NewBMCpwd
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md)
