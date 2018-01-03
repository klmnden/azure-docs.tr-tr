---
title: "X.509 sertifikaları oluşturmak için PowerShell kullanma | Microsoft Docs"
description: "X.509 sertifikaları yerel olarak oluşturup X.509 etkinleştirmek için PowerShell kullanma sanal bir ortamda Azure IOT hub'ınızdaki güvenlik temel."
services: iot-hub
documentationcenter: 
author: dsk-2015
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/10/2017
ms.author: dkshir
ms.openlocfilehash: b2f78e8debd367f86ee9bb06bf7de50590c61ad7
ms.sourcegitcommit: b7adce69c06b6e70493d13bc02bd31e06f291a91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/19/2017
---
# <a name="powershell-scripts-to-manage-ca-signed-x509-certificates"></a>CA tarafından imzalanmış X.509 sertifikalarını yönetmek için PowerShell komut dosyaları

İle başlamak X.509 Sertifika tabanlı güvenlik IOT Hub'ında gerektirir bir [X.509 Sertifika zinciri](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification), tüm ara sertifikaların Yaprak sertifikası kadar yanı sıra kök sertifikası içerir. Bu *nasıl* kılavuzda anlatılmaktadır, örnek PowerShell komut dosyalarıyla kullanan [OpenSSL](https://www.openssl.org/) oluşturmak ve X.509 sertifikaları imzalamak için. Bu adımların gerçek dünya işleminde üretim sırasında gerçekleşecek beri bu kılavuzu yalnızca deneme kullanmanızı öneririz. Azure IOT hub'ı kullanarak güvenlik benzetimini yapmak için bu sertifikaları kullanacak *X.509 sertifikası kimlik doğrulaması*. Bu kılavuzdaki adımları sertifikaları Windows makinenizde yerel olarak oluşturun. 

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, OpenSSL ikili dosyaları edindiğiniz varsayar. Ya da olabilir
    - OpenSSL kaynak kodunu indirebilir ve ikili dosyaları, makinenizde yapı veya 
    - herhangi bir yükleyip [üçüncü taraf OpenSSL ikili](https://wiki.openssl.org/index.php/Binaries), örneğin, gelen [SourceForge bu projede](https://sourceforge.net/projects/openssl/).

<a id="createcerts"></a>

## <a name="create-x509-certificates"></a>X.509 sertifikaları oluşturma
Aşağıdaki adımlar, yerel olarak X.509 kök sertifikalarını oluşturmak nasıl bir örneği gösterir. 

1. Farklı bir PowerShell penceresi açın bir *yönetici*. 
2. Çalışma dizinine gidin. Genel değişkenler ayarlamak için aşağıdaki betiği çalıştırın. 
    ```PowerShell
    $openSSLBinSource = "<full_path_to_the_binaries>\OpenSSL\bin"
    $errorActionPreference    = "stop"

    # Note that these values are for test purpose only
    $_rootCertSubject         = "CN=Azure IoT Root CA"
    $_intermediateCertSubject = "CN=Azure IoT Intermediate {0} CA"
    $_privateKeyPassword      = "123"

    $rootCACerFileName          = "./RootCA.cer"
    $rootCAPemFileName          = "./RootCA.pem"
    $intermediate1CAPemFileName = "./Intermediate1.pem"
    $intermediate2CAPemFileName = "./Intermediate2.pem"
    $intermediate3CAPemFileName = "./Intermediate3.pem"

    $openSSLBinDir              = Join-Path $ENV:TEMP "openssl-bin"

    # Whether to use ECC or RSA.
    $useEcc                     = $true
    ```
3. Çalışma dizininizi OpenSSL ikili dosyaları kopyalar ve ortam değişkenlerini ayarlar aşağıdaki betiği çalıştırın:

    ```PowerShell
    function Initialize-CAOpenSSL()
    {
        Write-Host ("Beginning copy of openssl binaries to {0} (and setting up env variables...)" -f $openSSLBinDir)
        if (-not (Test-Path $openSSLBinDir))
        {
            mkdir $openSSLBinDir | Out-Null
        }

        robocopy $openSSLBinSource $openSSLBinDir * /s 
        robocopy $openSSLBinSource . * /s 

        Write-Host "Setting up PATH and other environment variables."
        $ENV:PATH += "; $openSSLBinDir"
        $ENV:OPENSSL_CONF = Join-Path $openSSLBinDir "openssl.cnf"

        Write-Host "Success"
    }
    Initialize-CAOpenSSL
    ```
4. Ardından, bir sertifika olup olmadığını tarafından belirtilen arar aşağıdaki betiği çalıştırın *konu adı* zaten yüklendi ve OpenSSL makinenizde doğru olup olmadığını yapılandırılmış:
    ```PowerShell
    function Get-CACertBySubjectName([string]$subjectName)
    {
        $certificates = gci -Recurse Cert:\LocalMachine\ |? { $_.gettype().name -eq "X509Certificate2" }
        $cert = $certificates |? { $_.subject -eq $subjectName -and $_.PSParentPath -eq "Microsoft.PowerShell.Security\Certificate::LocalMachine\My" }
        if ($NULL -eq $cert)
        {
            throw ("Unable to find certificate with subjectName {0}" -f $subjectName)
        }
    
        write $cert
    }
    function Test-CAPrerequisites()
    {
        $certInstalled = $null
        try
        {
            $certInstalled = Get-CACertBySubjectName $_rootCertSubject
        }
        catch {}

        if ($NULL -ne $certInstalled)
        {
            throw ("Certificate {0} already installed.  Cleanup CA certs 1st" -f $_rootCertSubject)
        }

        if ($NULL -eq $ENV:OPENSSL_CONF)
        {
            throw ("OpenSSL not configured on this system.  Run 'Initialize-CAOpenSSL' (even if you've already done so) to set everything up.")
        }
        Write-Host "Success"
    }
    Test-CAPrerequisites
    ```
    Her şeyin doğru şekilde yapılandırıldıysa, "Başarılı" görmelisiniz ileti. 

<a id="createcertchain"></a>

## <a name="create-x509-certificate-chain"></a>X.509 sertifikası zinciri oluşturmak
Örneğin, bir kök CA sertifikası zinciri oluşturmak "CN = Azure IOT kök CA'ın" Bu örnek, aşağıdaki PowerShell betiğini çalıştırarak kullandığını. Bu komut dosyası ayrıca Windows işletim sistemi sertifika deponuza güncelleştirmeleri, sertifika dosyaları çalışma dizininizi de oluşturur. 
    1. Aşağıdaki komut dosyası için otomatik olarak imzalanan sertifika, oluşturmak için bir PowerShell işlev oluşturur bir verilen *konu adı* ve yetkilisi imzalama. 
    ```PowerShell
    function New-CASelfsignedCertificate([string]$subjectName, [object]$signingCert, [bool]$isASigner=$true)
    {
        # Build up argument list
        $selfSignedArgs =@{"-DnsName"=$subjectName; 
                           "-CertStoreLocation"="cert:\LocalMachine\My";
                           "-NotAfter"=(get-date).AddDays(30); 
                          }

        if ($isASigner -eq $true)
        {
            $selfSignedArgs += @{"-KeyUsage"="CertSign"; }
            $selfSignedArgs += @{"-TextExtension"= @(("2.5.29.19={text}ca=TRUE&pathlength=12")); }
        }
        else
        {
            $selfSignedArgs += @{"-TextExtension"= @("2.5.29.37={text}1.3.6.1.5.5.7.3.2,1.3.6.1.5.5.7.3.1", "2.5.29.19={text}ca=FALSE&pathlength=0")  }
        }

        if ($signingCert -ne $null)
        {
            $selfSignedArgs += @{"-Signer"=$signingCert }
        }

        if ($useEcc -eq $true)
        {
            $selfSignedArgs += @{"-KeyAlgorithm"="ECDSA_nistP256";
                             "-CurveExport"="CurveName" }
        }

        # Now use splatting to process this
        Write-Host ("Generating certificate {0} which is for prototyping, NOT PRODUCTION.  It will expire in 30 days." -f $subjectName)
        write (New-SelfSignedCertificate @selfSignedArgs)
    }
    ``` 
    2. Aşağıdaki PowerShell işlevi OpenSSL ikili dosyalarının yanı sıra önceki işlevi kullanarak ara X.509 sertifikaları oluşturur. 
    ```PowerShell
    function New-CAIntermediateCert([string]$subjectName, [Microsoft.CertificateServices.Commands.Certificate]$signingCert, [string]$pemFileName)
    {
        $certFileName = ($subjectName + ".cer")
        $newCert = New-CASelfsignedCertificate $subjectName $signingCert
        Export-Certificate -Cert $newCert -FilePath $certFileName -Type CERT | Out-Null
        Import-Certificate -CertStoreLocation "cert:\LocalMachine\CA" -FilePath $certFileName | Out-Null

        # Store public PEM for later chaining
        openssl x509 -inform deer -in $certFileName -out $pemFileName

        del $certFileName
   
        write $newCert
    }  
    ```
    3. Aşağıdaki PowerShell işlevi X.509 Sertifika zinciri oluşturur. Okuma [sertifika zincirleri](https://en.wikipedia.org/wiki/X.509#Certificate_chains_and_cross-certification) daha fazla bilgi için.
    ```PowerShell
    function New-CACertChain()
    {
        Write-Host "Beginning to install certificate chain to your LocalMachine\My store"
        $rootCACert =  New-CASelfsignedCertificate $_rootCertSubject $null
    
        Export-Certificate -Cert $rootCACert -FilePath $rootCACerFileName  -Type CERT
        Import-Certificate -CertStoreLocation "cert:\LocalMachine\Root" -FilePath $rootCACerFileName

        openssl x509 -inform der -in $rootCACerFileName -out $rootCAPemFileName

        $intermediateCert1 = New-CAIntermediateCert ($_intermediateCertSubject -f "1") $rootCACert $intermediate1CAPemFileName
        $intermediateCert2 = New-CAIntermediateCert ($_intermediateCertSubject -f "2") $intermediateCert1 $intermediate2CAPemFileName
        $intermediateCert3 = New-CAIntermediateCert ($_intermediateCertSubject -f "3") $intermediateCert2 $intermediate3CAPemFileName
        Write-Host "Success"
    }    
    ```
    Bu komut dosyası adlı bir dosya oluşturur *RootCA.cer* çalışma dizini içinde. 
    4. Son olarak, komutunu çalıştırarak X.509 sertifikası zinciri oluşturmak için yukarıdaki PowerShell işlevlerini kullanan `New-CACertChain` PowerShell penceresinde. 


<a id="signverificationcode"></a>

## <a name="proof-of-possession-of-your-x509-ca-certificate"></a>X.509 CA sertifikanızın kanıtını

Bu komut dosyası gerçekleştirir *kanıtı olarak elinde* X.509 sertifikası için akışı. 

Masaüstünüzde PowerShell penceresinde aşağıdaki kodu çalıştırın:
   
   ```PowerShell
   function New-CAVerificationCert([string]$requestedSubjectName)
   {
       $cnRequestedSubjectName = ("CN={0}" -f $requestedSubjectName)
       $verifyRequestedFileName = ".\verifyCert4.cer"
       $rootCACert = Get-CACertBySubjectName $_rootCertSubject
       Write-Host "Using Signing Cert:::" 
       Write-Host $rootCACert
   
       $verifyCert = New-CASelfsignedCertificate $cnRequestedSubjectName $rootCACert $false

       Export-Certificate -cert $verifyCert -filePath $verifyRequestedFileName -Type Cert
       if (-not (Test-Path $verifyRequestedFileName))
       {
           throw ("Error: CERT file {0} doesn't exist" -f $verifyRequestedFileName)
       }
   
       Write-Host ("Certificate with subject {0} has been output to {1}" -f $cnRequestedSubjectName, (Join-Path (get-location).path $verifyRequestedFileName)) 
   }
   New-CAVerificationCert "<your verification code>"
   ```

Bu kod, belirtilen konu adıyla adlı bir dosya CA tarafından imzalanmış bir sertifika oluşturur. *VerifyCert4.cer* çalışma dizini içinde. Bu sertifika dosyasını, bu CA'ın (diğer bir deyişle, özel anahtarı) imzalama iznine sahip IOT hub'ınıza doğrulamak yardımcı olur.


<a id="createx509device"></a>

## <a name="create-leaf-x509-certificate-for-your-device"></a>Cihazınız için yaprak X.509 sertifikası oluşturma

Bu bölümde, bir yaprak aygıt sertifikası ve karşılık gelen sertifika zinciri oluşturur bir PowerShell betiğini kullanabilirsiniz gösterir. 

Yerel makinenizde PowerShell penceresinde, bu cihaz için CA imzalı X.509 sertifikası oluşturmak için aşağıdaki betiği çalıştırın:

   ```PowerShell
   function New-CADevice([string]$deviceName, [string]$signingCertSubject=$_rootCertSubject)
   {
       $cnNewDeviceSubjectName = ("CN={0}" -f $deviceName)
       $newDevicePfxFileName = ("./{0}.pfx" -f $deviceName)
       $newDevicePemAllFileName      = ("./{0}-all.pem" -f $deviceName)
       $newDevicePemPrivateFileName  = ("./{0}-private.pem" -f $deviceName)
       $newDevicePemPublicFileName   = ("./{0}-public.pem" -f $deviceName)
   
       $signingCert = Get-CACertBySubjectName $signingCertSubject ## "CN=Azure IoT CA Intermediate 1 CA"

       $newDeviceCertPfx = New-CASelfSignedCertificate $cnNewDeviceSubjectName $signingCert $false
   
       $certSecureStringPwd = ConvertTo-SecureString -String $_privateKeyPassword -Force -AsPlainText

       # Export the PFX of the cert we've just created.  The PFX is a format that contains both public and private keys.
       Export-PFXCertificate -cert $newDeviceCertPfx -filePath $newDevicePfxFileName -password $certSecureStringPwd
       if (-not (Test-Path $newDevicePfxFileName))
       {
           throw ("Error: CERT file {0} doesn't exist" -f $newDevicePfxFileName)
       }

       # Begin the massaging.  First, turn the PFX into a PEM file which contains public key, private key, and other attributes.
       Write-Host ("When prompted for password by openssl, enter the password as {0}" -f $_privateKeyPassword)
       openssl pkcs12 -in $newDevicePfxFileName -out $newDevicePemAllFileName -nodes

       # Convert the PEM to get formats we can process
       if ($useEcc -eq $true)
       {
           openssl ec -in $newDevicePemAllFileName -out $newDevicePemPrivateFileName
       }
       else
       {
           openssl rsa -in $newDevicePemAllFileName -out $newDevicePemPrivateFileName
       }
       openssl x509 -in $newDevicePemAllFileName -out $newDevicePemPublicFileName
 
       Write-Host ("Certificate with subject {0} has been output to {1}" -f $cnNewDeviceSubjectName, (Join-Path (get-location).path $newDevicePemPublicFileName)) 
   }
   ```

Ardından çalıştırın `New-CADevice "<yourTestDevice>"` Cihazınızı oluşturmak için kullanılan kolay ad, bir PowerShell penceresinde kullanarak. CA'ın özel anahtar parolası istendiğinde, "123" girin. Bu oluşturur bir  _<yourTestDevice>.pfx_ çalışma dizininizi dosyasında.

## <a name="clean-up-certificates"></a>Sertifikaları temizle

Başlangıç çubuğunda veya **ayarları** uygulaması, arayın ve seçin **bilgisayar sertifikalarını yönetmek**. Tarafından verilen tüm sertifikaları kaldırın **Azure IOT CA TestOnly***. Bu sertifikalar, aşağıdaki üç konumda olması gerekir: 

* Sertifikalar - Yerel bilgisayar > kişisel > Sertifikalar
* Sertifikalar - Yerel bilgisayar > güvenilen kök sertifika yetkilileri > Sertifikalar
* Sertifikalar - Yerel bilgisayar > ara sertifika yetkilileri > Sertifikalar

   ![Azure IOT CA TestOnly sertifikaları kaldırın](./media/iot-hub-security-x509-create-certificates/cleanup.png)