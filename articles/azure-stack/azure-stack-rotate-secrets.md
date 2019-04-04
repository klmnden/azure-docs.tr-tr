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
ms.date: 12/18/2018
ms.author: mabrigg
ms.reviewer: ppacent
ms.lastreviewed: 12/18/2018
ms.openlocfilehash: 54bc6bc105dab2831df6e48a64a6f766582a3fb9
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917569"
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
- Yönetici KeyVault
- KeyVault
- Yönetici uzantısı konağı
- ACS (blob, tablo ve kuyruk depolama dahil)
- ADFS *
- Graf *

\* Yalnızca ortamın kimlik sağlayıcısı Active Directory Federasyon Hizmetleri'nde (AD FS) ise geçerlidir.

> [!Note]
> Diğer tüm anahtarları ve BMC dahil olmak üzere dizeleri, güvenli ve parolaları geçiş, kullanıcı ve yönetici hesabı parolalarını yine de el ile yönetici tarafından güncelleştirilir.

> [!Important]
> Azure yığını'nın 1811 sürümünden başlayarak, gizli döndürme iç ve dış sertifikaları için ayrılmıştır.

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

<sup>*</sup>Ortak sertifika yetkilileri Windows güvenilen kök programının bir parçası olan olduğunu gösterir. Tam listeyi makalesinde bulabilirsiniz [Microsoft güvenilen kök sertifika programı: Katılımcıların (27 Haziran 2017'den itibaren)](https://gallery.technet.microsoft.com/Trusted-Root-Certificate-123665ca).

## <a name="alert-remediation"></a>Uyarı düzeltme

30 gün içinde sona erme gizlidir, aşağıdaki uyarıları Yönetici portalı'nda üretilir:

- Bekleyen hizmet hesabının parola süresinin sonu
- Bekleyen bir iç sertifika süre sonu
- Bekleyen dış sertifika süre sonu

Aşağıdaki yönergeleri kullanarak gizli döndürmeye çalışıyor, bu uyarıları düzeltin.

> [!Note]
> Azure Stack ortamlarında 1811 öncesi sürümlerinde iç sertifika veya gizli dizi süre sonu bekleyen uyarıları görebilirsiniz.
> Bu uyarılar yanlış ve iç gizli döndürme çalıştırmadan yoksayılacak.
> Yanlış iç gizli dizi süre sonu, çözümlenen 1811 – içinde iç ortam için iki yıl etkin olan sürece gizli dizileri süresi sona ermeyecek bilinen bir sorun uyarılardır.

## <a name="pre-steps-for-secret-rotation"></a>Gizli anahtar döndürme öncesi adımları

   > [!IMPORTANT]
   > Gizli anahtar döndürme, Azure Stack ortamınıza gerçekleştirilen, 1811 veya gizli döndürme yeniden yürütmeden önce sonraki bir sürümü için sistem güncelleştirmeniz gerekir.
   > Gizli anahtar döndürme gereken yürütülebilir tarafından aracılığıyla [ayrıcalıklı uç nokta](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) ve Azure Stack operatörü kimlik bilgilerini gerektirir.
   > Ortamınızı Azure Stack işleçleri gizli döndürme, ortamınıza çalıştırılıp çalıştırılmadığını bilmiyorsanız 1811 için gizli döndürme yeniden çalıştırmadan önce güncelleştirin.

1. Azure Stack örneğinizin 1811 sürümüne güncelleştirme önemle tavsiye edilir.

    > [!Note] 
    > 1811 öncesi sürümleri için uzantı ana bilgisayar sertifikaları eklemek için gizli dizileri döndürmek gerekmez. Bu makaledeki yönergeleri izlemelidir [hazırlamak için Azure Stack için uzantısı konağı](azure-stack-extension-host-prepare.md) uzantısı ana bilgisayar sertifikaları eklemek için.

2. Operatörler Azure Stack gizli dizilerinin rotasyonu sırasında uyarıların açıldığını ve otomatik olarak kapatıldığını görebilir.  Bu davranış beklenir ve uyarıların yoksayılabilir.  İşleçler, bu uyarılar geçerliliğini çalıştırarak doğrulayabilirsiniz **Test AzureStack**.  İzlemek için SCOM kullanma işleçleri için bir sistem bakım moduna alma, Azure Stack sistemleri bu uyarılar ITSM sistemlerine erişmesini engeller ancak Azure Stack sistemi ulaşılamaz hale gelirse uyaracak şekilde devam eder.

3. Herhangi bir bakım işlemleri, kullanıcılara bildirin. Normal bakım pencereleri, çalışma saatleri sırasında mümkün olduğunca, zamanlayın. Bakım işlemleri, kullanıcı iş yükleri hem portal işlemlerini etkileyebilir.

    > [!Note]
    > Sonraki adımlar yalnızca Azure Stack dış gizli anahtarları döndürme yaparken geçerlidir.

4. Çalıştırma **[Test AzureStack](https://docs.microsoft.com/azure/azure-stack/azure-stack-diagnostic-test)** ve tüm test çıkışları gizli anahtarları döndürme önce sağlıklı olduğunu onaylayın.
5. Yeni bir değişiklik kümesi dış sertifikalar hazırlayın. Yeni küme özetlenen sertifika belirtimleri eşleşen [Azure Stack PKI sertifikası gereksinimleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs). Sertifika imzalama isteği (CSR) satın almak veya özetlenen adımları kullanarak yeni sertifikalar oluşturmak için oluşturabileceğiniz [PKI sertifikalarını üretmek](https://docs.microsoft.com/azure/azure-stack/azure-stack-get-pki-certs) ve bunları adımlarıkullanarakAzureStackortamındakullanımiçinhazırlama[ Azure Stack PKI sertifikalarını hazırlamak](https://docs.microsoft.com/azure/azure-stack/azure-stack-prepare-pki-certs). Hazırlama konusunda özetlenen adımları sertifikaları doğruladığınızdan emin olun [PKI sertifikalarını doğrulamak](https://docs.microsoft.com/azure/azure-stack/azure-stack-validate-pki-certs).
6. Bir geri dönüş güvenli yedekleme konumu için kullanılan sertifikaları kadar Store. Döndürme çalıştırır ve ardından başarısız olursa, dönüş yeniden çalıştırmadan önce dosya paylaşımında sertifika yedek kopyaları değiştirin. Unutmayın, yedek kopyaları güvenli yedekleme yerde saklayın.
7. ERCS Vm'lerden erişebileceğiniz bir dosya paylaşımı oluşturun. Dosya Paylaşımı okunabilir ve yazılabilir için **CloudAdmin** kimlik.
8. PowerShell ISE konsolunda paylaşımına erişimine sahip olduğu bir bilgisayardan açın. Dosya paylaşımına gidin.
9. Çalıştırma **[CertDirectoryMaker.ps1](https://www.aka.ms/azssecretrotationhelper)** , dış sertifikalar için gerekli dizini oluşturmak için.

> [!IMPORTANT]
> CertDirectoryMaker betik tabi olacak bir klasör yapısı oluşturur:
>
> **.\Certificates\AAD** veya ***.\Certificates\ADFS*** Azure Stack için kullanılan kimlik sağlayıcınız bağlı olarak
>
> Klasör yapınız biten dayanıklılığı olduğu **AAD** veya **ADFS** klasör ve tüm alt dizinler olan bu yapı içinde; aksi takdirde, **Başlat-SecretRotation**ile gelir:
> ```powershell
> Cannot bind argument to parameter 'Path' because it is null.
> + CategoryInfo          : InvalidData: (:) [Test-Certificate], ParameterBindingValidationException
> + FullyQualifiedErrorId : ParameterArgumentValidationErrorNullNotAllowed,Test-Certificate
> + PSComputerName        : xxx.xxx.xxx.xxx
> ```
>
> Gördüğünüz gibi hata massage, dosya paylaşımını erişilirken bir sorun yoktur ancak isteğe bağlı olarak gerçekte burada zorunlu klasör yapısını olduğunu gösterir.
> Microsoft AzureStack hazırlık denetleyicisi - daha fazla bilgi bulunabilir [PublicCertHelper Modülü](https://www.powershellgallery.com/packages/Microsoft.AzureStack.ReadinessChecker/1.1811.1101.1/Content/CertificateValidation%5CPublicCertHelper.psm1)
>
> Ayrıca dosya paylaşımını klasör yapınız ile başlayan önemli olduğu **sertifikaları** ayrıca başarısız doğrulama klasör Aksi takdirde.
> Dosya paylaşımını bağlama gibi görünmelidir **\\ \\ \<IPADDRESS >\\\<ShareName >\\** klasörü içermelidir  **Certificates\AAD** veya **Certificates\ADFS** içinde.
>
> Örneğin:
> - Dosya paylaşımını =  **\\ \\ \<IPADDRESS >\\\<ShareName >\\**
> - CertFolder = **Certificates\AAD**
> - FullPath = **\\\\\<IPAddress>\\\<ShareName>\Certificates\AAD**

## <a name="rotating-external-secrets"></a>Dış gizli anahtarları döndürme

Dış gizli anahtarları döndürmek için:

1. Yeni oluşturulan içinde **\Certificates\\\<Identityprovider >** öncesi adımlarda oluşturulan dizin, dizin yapısına göre değiştirme dış sertifikalar yeni kümesini yerleştirin Zorunlu sertifikaları bölümünde belirtilen biçim [Azure Stack PKI sertifikası gereksinimleri](https://docs.microsoft.com/azure/azure-stack/azure-stack-pki-certs#mandatory-certificates).

    AAD kimlik sağlayıcısı için klasör yapısı örneği:
    ```powershell
        <ShareName>
        │   │
        │   ├───Certificates
        │   └───AAD
        │       ├───ACSBlob
        │       │       <CertName>.pfx
        │       │
        │       ├───ACSQueue
        │       │       <CertName>.pfx
        │       │
        │       ├───ACSTable
        │       │       <CertName>.pfx
        │       │
        │       ├───Admin Extension Host
        │       │       <CertName>.pfx
        │       │
        │       ├───Admin Portal
        │       │       <CertName>.pfx
        │       │
        │       ├───ARM Admin
        │       │       <CertName>.pfx
        │       │
        │       ├───ARM Public
        │       │       <CertName>.pfx
        │       │
        │       ├───KeyVault
        │       │       <CertName>.pfx
        │       │
        │       ├───KeyVaultInternal
        │       │       <CertName>.pfx
        │       │
        │       ├───Public Extension Host
        │       │       <CertName>.pfx
        │       │
        │       └───Public Portal
        │               <CertName>.pfx

    ```

2. Bir PowerShell oturumu oluşturma [ayrıcalıklı uç nokta](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint) kullanarak **CloudAdmin** hesap ve oturumları bir değişkene depolayın. Bu değişken sonraki adımda parametre olarak kullanır.

    > [!IMPORTANT]  
    > Oturum girin değil, oturumu bir değişkene depolayın.

3. Çalıştırma  **[Invoke-Command](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/Invoke-Command?view=powershell-5.1)**. Ayrıcalıklı uç nokta PowerShell oturumu Değişkeninizi olarak geçirmek **oturumu** parametresi.

4. Çalıştırma **başlangıç SecretRotation** şu parametrelerle:
    - **PfxFilesPath**  
    Daha önce oluşturduğunuz sertifikalar dizininize ağ yolunu belirtin.  
    - **PathAccessCredential**  
    Paylaşım kimlik bilgilerini bir PSCredential nesnesi.
    - **CertificatePassword**  
    Tüm oluşturulan pfx sertifika dosyaları için kullanılan parolayı güvenli bir dize.

5. Gizli anahtarlarınız döndürme bekleyin. Dış gizli dönüş genellikle yaklaşık bir saat sürer.

    Gizli anahtar döndürme işlemi başarıyla tamamlandığında, Konsolunuzda görüntülenir **genel eylem durumu: Başarı**.

    > [!Note]
    > Gizli anahtar döndürme başarısız olursa hata iletisindeki yönergeleri izleyin ve yeniden çalıştırın **başlangıç SecretRotation** ile **-yeniden** parametresi.

    ```powershell
    Start-SecretRotation -ReRun
    ```
    Gizli anahtar döndürme hataları yaşıyorsanız desteğe başvurun yinelenir.

6. Başarılı tamamlama gizli döndürme sonra öncesi adımda oluşturduğunuz paylaşımından sertifikalarınızı ve güvenli yedekleme konumlarında depolayabilirsiniz.

## <a name="walkthrough-of-secret-rotation"></a>İzlenecek yol gizli döndürme

Aşağıdaki PowerShell örneği, cmdlet'leri ve parametreleri dizilerinizin döndürmek için çalıştırılacak gösterir.

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Run Secret Rotation
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
Invoke-Command -Session $PEPSession -ScriptBlock {
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

## <a name="rotating-only-internal-secrets"></a>Yalnızca iç gizli anahtarları döndürme

> [!Note]
> İç gizli döndürme yalnızca bir iç gizli kötü amaçlı bir varlık tarafından tehlikede şüpheleniyorsanız veya iç sertifikaların süresi dolmak üzere olan belirten bir uyarı (üzerinde derleme 1811 veya üzeri) aldıysanız yapılmalıdır.
> Azure Stack ortamlarında 1811 öncesi sürümlerinde iç sertifika veya gizli dizi süre sonu bekleyen uyarıları görebilirsiniz.
> Bu uyarılar yanlış ve iç gizli döndürme çalıştırmadan yoksayılacak.
> Yanlış iç gizli dizi süre sonu, çözümlenen 1811 – içinde iç ortam için iki yıl etkin olan sürece gizli dizileri süresi sona ermeyecek bilinen bir sorun uyarılardır.

1. Bir PowerShell oturumu oluşturma [ayrıcalıklı uç nokta](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).
2. Ayrıcalıklı uç nokta oturumda çalıştırılan **başlangıç SecretRotation-iç**.

    > [!Note]
    > Azure Stack ortamlarında 1811 öncesi sürümlerinde değil gerektirecek **-iç** bayrağı. **Başlangıç SecretRotation** yalnızca iç gizli dizileri döndürüleceğini.

3. Gizli anahtarlarınız döndürme bekleyin.

Gizli anahtar döndürme işlemi başarıyla tamamlandığında, Konsolunuzda görüntülenir **genel eylem durumu: Başarı**.
    > [!Note]
    > If secret rotation fails, follow the instructions in the error message and rerun **Start-SecretRotation** with the  **–Internal** and **-ReRun** parameters.  

```powershell
Start-SecretRotation -Internal -ReRun
```

Gizli anahtar döndürme hataları yaşıyorsanız desteğe başvurun yinelenir.

## <a name="start-secretrotation-reference"></a>Başlangıç SecretRotation başvurusu

Bir Azure Stack sisteminin gizli dizileri döndürür. Yalnızca Azure Stack ayrıcalıklı uç noktasına karşı yürütülür.

### <a name="syntax"></a>Sözdizimi

#### <a name="for-external-secret-rotation"></a>Dış gizli dönüş

```powershell
Start-SecretRotation [-PfxFilesPath <string>] [-PathAccessCredential <PSCredential>] [-CertificatePassword <SecureString>]  
```

#### <a name="for-internal-secret-rotation"></a>İç gizli dönüş

```powershell
Start-SecretRotation [-Internal]  
```

#### <a name="for-external-secret-rotation-rerun"></a>Dış gizli dönüş yeniden çalıştırın

```powershell
Start-SecretRotation [-ReRun]
```

#### <a name="for-internal-secret-rotation-rerun"></a>İç gizli dönüş yeniden çalıştırın

```powershell
Start-SecretRotation [-ReRun] [-Internal]
```

### <a name="description"></a>Açıklama

**Başlangıç SecretRotation** cmdlet'i bir Azure Stack sisteminin altyapı gizli dizileri döndürür. Varsayılan olarak, tüm dış ağ altyapı uç noktaları yalnızca sertifikalarını döndürür. Altyapı gizli iç bayrağı iç kullanılırsa döndürülür. Dış ağ altyapı uç noktaları, döndürürken **başlangıç SecretRotation** ile çalıştırılması gereken bir **Invoke-Command** ayrıcalıklı uç nokta oturumu Azure Stack ortamınıza betik bloğu geçirilen olarak **oturumu** parametresi.

### <a name="parameters"></a>Parametreler

| Parametre | Type | Gerekli | Konum | Varsayılan | Açıklama |
| -- | -- | -- | -- | -- | -- |
| PfxFilesPath | String  | False  | adlı  | None  | Fileshare yolu **\Certificates** tüm dış içeren dizin ağ uç noktası sertifikaları. Yalnızca dış gizli anahtarları döndürürken gereklidir. Son dizin olmalıdır **\Certificates**. |
| CertificatePassword | SecureString | False  | adlı  | None  | -PfXFilesPath sağlanan tüm sertifikalar için parola. Dış gizli anahtarları Döndürülmüş olduğunda PfxFilesPath sağlanıyorsa değer gereklidir. |
| İç | String | False | adlı | None | İç bayrağı, iç altyapı gizli dizileri döndürmek Azure Stack operatörü istediği zaman kullanılmalıdır. |
| PathAccessCredential | PSCredential | False  | adlı  | None  | PowerShell kimlik bilgilerini dosya paylaşımını **\Certificates** tüm dış içeren dizin ağ uç noktası sertifikaları. Yalnızca dış gizli anahtarları döndürürken gereklidir.  |
| ReRun | SwitchParameter | False  | adlı  | None  | Gizli anahtar döndürme, başarısız bir girişimden sonra reattempted herhangi bir zamanda yeniden kullanılması gerekir. |

### <a name="examples"></a>Örnekler

#### <a name="rotate-only-internal-infrastructure-secrets"></a>Yalnızca iç altyapı gizli dizileri Döndür

Bu, Azure Stack ile çalıştırılmalıdır [ayrıcalıklı uç nokta ortamdaki](https://docs.microsoft.com/azure/azure-stack/azure-stack-privileged-endpoint).

```powershell
PS C:\> Start-SecretRotation -Internal
```

Bu komut tüm Azure Stack iç ağa kullanıma sunulan altyapı gizli dizileri döndürür.

#### <a name="rotate-only-external-infrastructure-secrets"></a>Gizli dizileri dış altyapı Döndür  

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Create Credentials for the fileshare
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
# Run Secret Rotation
Invoke-Command -Session $PEPSession -ScriptBlock {  
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

Bu komut, Azure yığını'nın dış ağ altyapı uç noktaları için kullanılan TLS sertifikalarını döndürür.

#### <a name="rotate-internal-and-external-infrastructure-secrets-pre-1811-only"></a>İç ve dış altyapı gizli anahtarları döndürme (**öncesi 1811** yalnızca)

> [!IMPORTANT]
> Bu komut, yalnızca Azure Stack için geçerlidir **öncesi 1811** döndürme iç ve dış sertifikaları için bölünmüş gibi.
>
> **Gelen *1811 +* hem de iç döndürülemiyor ve daha fazla harici sertifika!!!**

```powershell
# Create a PEP Session
winrm s winrm/config/client '@{TrustedHosts= "<IpOfERCSMachine>"}'
$PEPCreds = Get-Credential
$PEPSession = New-PSSession -ComputerName <IpOfERCSMachine> -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

# Create Credentials for the fileshare
$CertPassword = ConvertTo-SecureString "<CertPasswordHere>" -AsPlainText -Force
$CertShareCreds = Get-Credential
$CertSharePath = "<NetworkPathOfCertShare>"
# Run Secret Rotation
Invoke-Command -Session $PEPSession -ScriptBlock {
    Start-SecretRotation -PfxFilesPath $using:CertSharePath -PathAccessCredential $using:CertShareCreds -CertificatePassword $using:CertPassword
}
Remove-PSSession -Session $PEPSession
```

Bu komut tüm Azure Stack iç ağa kullanıma sunulan altyapı gizli dizileri ve bunun yanı sıra Azure yığını'nın dış ağ altyapı uç noktaları için kullanılan TLS sertifikalarını döndürür. Yığın tarafından oluşturulan tüm gizli dizilerin başlangıç SecretRotation döndürür ve sağlanan sertifikalar çünkü dış uç noktası sertifikaları da döndürülür.  

## <a name="update-the-baseboard-management-controller-bmc-credential"></a>Temel Kart Yönetim denetleyicisine (BMC) kimlik bilgilerini güncelleştirme

Temel Kart Yönetim denetleyicisine (BMC) fiziksel sunucularınızı durumunu izler. Özellikleri ve kullanıcı hesabı adı ve parola BMC'nin güncelleştirme yönergeler orijinal ekipman üreticisi (OEM) donanım satıcınıza göre değişir. Parolalarınızı Azure Stack bileşenlerin düzenli olarak güncelleştirmeniz gerekir.

1. Azure Stack fiziksel sunucularda BMC, OEM yönergelerini izleyerek güncelleştirin. Kullanıcı adı ve parola, ortamınızdaki her BMC için aynı olmalıdır. Not BMC kullanıcı adları 16 karakterden uzun olamaz.
2. Ayrıcalıklı bir uç nokta, Azure Stack oturumunu açın. Yönergeler için [Azure Stack'te ayrıcalıklı uç noktayı kullanarak](azure-stack-privileged-endpoint.md).
3. Sonra bir PowerShell istemi için değişti **[IP adresi veya ERCS VM adı]: PS >** veya **[azs-ercs01]: PS >** çalıştırabileceğiniz bir ortam, bağlı olarak `Set-BmcCredential` çalıştırarak `Invoke-Command`. Ayrıcalıklı uç nokta oturum değişkeni, bir parametre olarak geçiriyoruz. Örneğin:

    ```powershell
    # Interactive Version
    $PEPIp = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEPCreds = Get-Credential "<Domain>\CloudAdmin" -Message "PEP Credentials"
    $NewBmcPwd = Read-Host -Prompt "Enter New BMC password" -AsSecureString
    $NewBmcUser = Read-Host -Prompt "Enter New BMC user name"

    $PEPSession = New-PSSession -ComputerName $PEPIp -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BmcPassword is mandatory, while the BmcUser parameter is optional.
        Set-BmcCredential -BmcPassword $using:NewBmcPwd -BmcUser $using:NewBmcUser
    }
    Remove-PSSession -Session $PEPSession
    ```

    Kod satırları parolalarıyla statik PowerShell sürümünü de kullanabilirsiniz:

    ```powershell
    # Static Version
    $PEPIp = "<Privileged Endpoint IP or Name>" # You can also use the machine name instead of IP here.
    $PEPUser = "<Privileged Endpoint user for example Domain\CloudAdmin>"
    $PEPPwd = ConvertTo-SecureString "<Privileged Endpoint Password>" -AsPlainText -Force
    $PEPCreds = New-Object System.Management.Automation.PSCredential ($PEPUser, $PEPPwd)
    $NewBmcPwd = ConvertTo-SecureString "<New BMC Password>" -AsPlainText -Force
    $NewBmcUser = "<New BMC User name>"

    $PEPSession = New-PSSession -ComputerName $PEPIp -Credential $PEPCreds -ConfigurationName "PrivilegedEndpoint"

    Invoke-Command -Session $PEPSession -ScriptBlock {
        # Parameter BmcPassword is mandatory, while the BmcUser parameter is optional.
        Set-BmcCredential -BmcPassword $using:NewBmcPwd -BmcUser $using:NewBmcUser
    }
    Remove-PSSession -Session $PEPSession
    ```

## <a name="next-steps"></a>Sonraki adımlar

[Azure Stack güvenliği hakkında daha fazla bilgi edinin](azure-stack-security-foundations.md)
