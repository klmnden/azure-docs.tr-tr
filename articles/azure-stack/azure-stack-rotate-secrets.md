---
title: Gizli anahtarları Azure yığınında döndürme | Microsoft Docs
description: Gizli anahtarlarınız Azure yığınında döndürme öğrenin.
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
ms.date: 05/15/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.openlocfilehash: a3dfce6ce1b136e39047cfd47b336b2fb2a35af9
ms.sourcegitcommit: 96089449d17548263691d40e4f1e8f9557561197
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/17/2018
---
# <a name="rotate-secrets-in-azure-stack"></a>Gizli anahtarları Azure yığınında Döndür

*Bu yönergeler yalnızca Azure yığın tümleşik sistemleri sürüm 1803 ve sonrası için geçerlidir. Gizli dönüş öncesi 1802 Azure yığın sürümlerinde çalışmayın*

Azure yığın çeşitli gizli anahtarları Azure yığın altyapı kaynaklarını ve hizmetleri arasında güvenli iletişim sağlamak için kullanır.

- **İç gizli**  
Tüm sertifikalar, parolalar, güvenli dizeler ve Azure yığın işlecinin müdahalesi olmadan Azure yığın altyapısı tarafından kullanılan anahtarları. 

- **Dış gizli**  
Azure yığın operatör tarafından sağlanan dışa dönük Hizmetleri için altyapı hizmet sertifikaları. Bu, aşağıdaki hizmetler için sertifikaları içerir: 
    - Yönetici portalı 
    - Ortak portalı 
    - Yönetici Azure Kaynak Yöneticisi 
    - Genel Azure Kaynak Yöneticisi 
    - Yönetici Keyvault 
    - Key Vault 
    - ACS (blob, tablo ve kuyruk depolama dahil) 
    - ADFS<sup>*</sup>
    - Grafik<sup>*</sup>

    > <sup>*</sup> Yalnızca ortam kimlik sağlayıcısı Active Directory Federasyon Hizmetleri (AD FS) ise geçerlidir.

> [!NOTE]
> Diğer tüm anahtarları ve BMC dahil olmak üzere dizeleri, güvenli ve parolaları geçiş, kullanıcı ve yönetici hesabı parolaları yine de el ile yönetici tarafından güncelleştirilir. 

Azure yığın altyapı bütünlüğü korumak için düzenli aralıklarla kendi altyapının gizli kuruluşun güvenlik gereksinimleriyle tutarlı sıklıklarını adresindeki döndürme olanağı işleçleri gerekir.

### <a name="rotating-secrets-with-external-certificates-from-a-new-certificate-authority"></a>Yeni bir sertifika yetkilisinden dış sertifikalar ile gizli döndürme

Azure yığını gizli döndürme dış sertifikalar bir yeni sertifika yetkilisi (CA) ile aşağıdaki bağlamlarda destekler:

|Yüklü sertifika CA|CA için döndürmek için|Desteklenen|Azure yığın sürümleri desteklenir|
|-----|-----|-----|-----|-----|
|Otomatik olarak imzalanan gelen|Kuruluş|Desteklenmiyor||
|Otomatik olarak imzalanan gelen|Otomatik olarak imzalanan|Desteklenmiyor||
|Otomatik olarak imzalanan gelen|Ortak olarak<sup>*</sup>|Desteklenen|1803 & sonraki|
|Kuruluş|Kuruluş|AYNI kuruluş CA'sı olarak dağıtım sırasında kullanılan müşterilerin kullandığı sürece desteklenen|1803 & sonraki|
|Kuruluş|Otomatik olarak imzalanan|Desteklenmiyor||
|Kuruluş|Ortak olarak<sup>*</sup>|Desteklenen|1803 & sonraki|
|Ortak<sup>*</sup>|Kuruluş|Desteklenmiyor|1803 & sonraki|
|Ortak<sup>*</sup>|Otomatik olarak imzalanan|Desteklenmiyor||
|Ortak<sup>*</sup>|Ortak olarak<sup>*</sup>|Desteklenen|1803 & sonraki|

<sup>*</sup> Ortak sertifika yetkililerini Windows Güvenilen Kök Programı'nın parçası olan şunlardır. Tam listesini bulabilirsiniz [Microsoft güvenilen kök sertifika programı: katılımcıları (itibariyle, 27 Haziran 2017)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).

## <a name="alert-remediation"></a>Uyarı düzeltme

Gizli 30 gün içinde sona erme olduğunda, aşağıdaki uyarıları Yönetici portalı'nda üretilir: 

- Bekleyen hizmet hesabı parolasının süresinin 
- Bekleyen iç sertifika süre sonu 
- Bekleyen dış sertifika süre sonu 

Aşağıdaki yönergeleri kullanarak gizli döndürme çalıştıran Bu uyarılar düzeltir.

## <a name="pre-steps-for-secret-rotation"></a>Gizli dönüş öncesi adımları

1.  Kullanıcılarınıza bakım işlemleri bildirin. Normal bakım pencereleri, iş saatleri sırasında mümkün olduğunca, zamanlayın. Bakım işlemleri, kullanıcı iş yükleri ve portal işlemlerini etkileyebilir.

    > [!note]  
    > Sonraki adımlar, yalnızca Azure yığın dış gizli anahtarları döndürme olduğunda geçerlidir.

2.  Yeni bir değişiklik kümesi dış sertifikalar hazırlayın. Yeni küme özetlenen sertifika belirtimleri eşleşen [Azure yığın PKI sertifikası gereksinimleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs).
3.  Bir geri dönüş güvenli yedekleme konumu için kullanılan sertifikaları kadar saklar. Döndürme çalıştırır ve ardından başarısız olursa, dosya paylaşımı sertifikaları döndürme yeniden çalıştırmadan önce yedek kopyaları değiştirin. Unutmayın, yedek kopyaları güvenli yedekleme konumda saklayın.
3.  ERCS Vm'lerden erişmek için bir dosya paylaşımı oluşturun. Dosya Paylaşımı okunabilir ve yazılabilir için olmalıdır **CloudAdmin** kimliği.
4.  Dosya paylaşımı erişimi sahip olduğu bir bilgisayardan bir PowerShell ISE konsolunu açın. Dosya Paylaşımı gidin. 
5.  Çalıştırma **[CertDirectoryMaker.ps1](http://www.aka.ms/azssecretrotationhelper)** dış sertifikalar için gerekli dizinler oluşturulamadı.

## <a name="rotating-external-and-internal-secrets"></a>İç ve dış gizli anahtarları döndürme

Her iki harici bir iç gizli döndürmek için:

1. Yeni oluşturulan içinde **/Sertifikalar** öncesi adımlarda oluşturulan dizin zorunlu sertifikaları bölümünde özetlenen biçimde göre dizin yapısına değiştirme dış sertifikalar yeni kümesi yerleştirin [Azure yığın PKI sertifikası gereksinimleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).
2. Bir PowerShell oturumu oluşturmak [ayrıcalıklı uç nokta](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) kullanarak **CloudAdmin** oturumları değişken olarak depolamak ve hesap. Bu değişken, bir sonraki adımda parametre olarak kullanır.

    > [!IMPORTANT]  
    > Oturum girin değil, oturumu bir değişkene depolamak.
    
3. Çalıştırma  **[Invoke-command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-5.1)**. Ayrıcalıklı uç nokta PowerShell oturumu değişken olarak geçirmek **oturum** parametresi. 
4. Çalıştırma **başlangıç SecretRotation** aşağıdaki parametrelerle:
    - **PfxFilesPath**  
    Daha önce oluşturduğunuz sertifikalar dizininize ağ yolunu belirtin.  
    - **PathAccessCredential**  
    Paylaşım için kimlik bilgileri için bir PSCredential nesnesi. 
    - **CertificatePassword**  
    Tüm oluşturulan pfx sertifika dosyaları için kullanılan parola, güvenli bir dize.
4. Gizli anahtarlarınız döndürme bekleyin.  
Gizli döndürme başarıyla tamamlandığında, konsolu görüntüler **genel eylem durumu: başarılı**. 
5. Başarılı tamamlama gizli döndürme sonra ön adımda oluşturduğunuz paylaşımından sertifikalarınızı ve güvenli yedekleme konumlarında depolayabilirsiniz. 

## <a name="walkthrough-of-secret-rotation"></a>İzlenecek yol gizli döndürme

Aşağıdaki PowerShell örneğinde cmdlet'leri ve gizli anahtarlarınız döndürmek için çalıştırmak için parametreleri gösterilmektedir.

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
## <a name="rotating-only-internal-secrets"></a>Yalnızca dahili gizli anahtarları döndürme

Yalnızca dahili gizli anahtarları Azure yığın döndürmek için:

1. Bir PowerShell oturumu oluşturmak [ayrıcalıklı uç nokta](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Ayrıcalıklı Endpoint oturumda çalıştırılan **başlangıç SecretRotation** bağımsız değişken içermeyen.

## <a name="start-secretrotation-reference"></a>Başlangıç SecretRotation başvurusu

Bir Azure yığın sistemi Sırları döndürür. Yalnızca Azure yığın ayrıcalıklı uç noktası karşı yürütülebilir.

### <a name="syntax"></a>Sözdizimi

Yol (varsayılan)

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential] <PSCredential> [-CertificatePassword <SecureString>]  
```

### <a name="description"></a>Açıklama

Start-SecretRotation cmdlet'i bir Azure yığın sistemi altyapı Sırları döndürür. İç altyapı ağına kullanıma sunulan tüm gizlilikleri döndüğü varsayılan olarak, kullanıcı girişi ile aynı zamanda tüm dış ağ altyapısı uç noktaları sertifikaları döndürür. Dış ağ altyapısı uç noktaları döndürme, başlangıç SecretRotation oturum parametre olarak geçirilen Azure yığın ortamı ayrıcalıklı endpoint oturum ile bir Invoke-Command betik bloğu yürütülmelidir.
 
### <a name="parameters"></a>Parametreler

| Parametre | Tür | Gerekli | Konum | Varsayılan | Açıklama |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | Dize  | False  | Adlı  | None  | Fileshare yolu **\Certificates** tüm dış içeren dizin ağ uç noktası sertifikaları. Yalnızca dahili ve harici gizli anahtarları döndürme durumlarda gereklidir. Son dizin olmalıdır **\Certificates**. |
| CertificatePassword | SecureString | False  | Adlı  | None  | -PfXFilesPath sağlanan tüm sertifikalar için parola. İç ve dış gizli Döndürülmüş zaman PfxFilesPath sağladıysanız değeri gerekiyor. |
|

### <a name="examples"></a>Örnekler
 
**Yalnızca iç altyapı gizli anahtarları döndürme**

```powershell  
PS C:\> Start-SecretRotation  
```

Bu komut tüm Azure yığın iç ağa sunulan altyapı gizli döndürür. Tüm yığın oluşturulan gizlilikleri başlangıç SecretRotation döndürür, ancak sağlanan sertifika olduğundan, dış uç noktası sertifikaları değil döndürülür.  

**İç ve dış altyapı gizli Döndür**
  
```powershell
PS C:\> Invoke-Command -session $PEPSession -ScriptBlock { 
Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCred -CertificatePassword $using:CertPassword } 
Remove-PSSession -Session $PEPSession
```

Bu komut tüm Azure yığın dış ağ altyapısı uç noktaları için kullanılan TLS sertifikaları yanı sıra Azure yığın iç ağa sunulan altyapı gizli döndürür. Başlangıç SecretRotation tüm yığın oluşturulan gizlilikleri döndürür ve orada sertifikaların sağlandığından dış uç noktası sertifikaları da döndürülür.  


## <a name="update-the-baseboard-management-controller-bmc-password"></a>Temel kart yönetim denetleyicisi (BMC) parolasını güncelleştirin

Temel kart yönetim denetleyicisi (BMC) sunucularınız fiziksel durumunu izler. BMC parola güncelleştirme hakkında yönergeler ve belirtimleri özgün donanım üreticisi (OEM) donanım satıcınıza göre farklılık gösterir. Normal bir tempoyla Azure yığın bileşenler için parolalar güncelleştirmeniz gerekir.

1. BMC Azure yığın fiziksel sunucularda, OEM yönergelerini izleyerek güncelleştirin. Ortamınızdaki her BMC için parola aynı olmalıdır.
2. Ayrıcalıklı bir uç nokta Azure yığın oturumu açın. Yönergeler için bkz: [Azure yığınında kullanan ayrıcalıklı uç noktasını](azure-stack-privileged-endpoint.md).
3. Sonra PowerShell istemi olarak değiştirildi **[IP adresi veya ERCS VM name]: PS >** veya **[azs-ercs01]: PS >** çalıştırabileceğiniz bir ortam, bağlı olarak `Set-BmcPassword` çalıştırarak `invoke-command`. Ayrıcalıklı endpoint oturum değişkeni bir parametre olarak geçirin. Örneğin:

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
    
    Statik PowerShell sürümü parolalarıyla kod satırı da kullanabilirsiniz:
    
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

[Azure yığın güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md)
