---
title: CLI ile Azure stack'e bağlanma | Microsoft Docs
description: Platformlar arası komut satırı arabirimi (CLI) yönetmek ve Azure Stack'te kaynakları dağıtmak için kullanmayı öğrenin
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2019
ms.author: mabrigg
ms.reviewer: sijuman
ms.lastreviewed: 02/28/2019
ms.openlocfilehash: fa1731c9361be83949aa794ed8842681bd81d995
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59257578"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>Azure Stack'te Azure CLI ile API Sürüm profillerini kullanma

*Şunlara uygulanır Azure Stack tümleşik sistemleri ve Azure Stack Geliştirme Seti*

Linux, Mac ve Windows istemci platformlarını Azure Stack geliştirme Seti'ni (ASDK) kaynaklarını yönetmek için Azure komut satırı arabirimi (CLI) yedekleme ayarlamak için bu makaledeki adımları izleyebilirsiniz.

## <a name="prepare-for-azure-cli"></a>Azure CLI için hazırlama

Geliştirme makinenizde Azure CLI kullanmak Azure Stack için CA kök sertifikası gerekir. CLI aracılığıyla kaynaklarını yönetmek için bir sertifika kullanın.

 - **Azure Stack CA kök sertifikasını** ASDK dışında bir iş istasyonundan CLI kullanıyorsanız gereklidir.  

 - **Sanal makine diğer uç nokta** "UbuntuLTS" veya "bir görüntü yayımcısı, teklif, SKU ve sürüm Vm'leri dağıtırken tek bir parametre başvuran Win2012Datacenter," gibi bir diğer ad sağlar.  

Aşağıdaki bölümlerde, bu değerleri almak nasıl açıklanmaktadır.

### <a name="export-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını dışarı aktarma

Tümleşik bir sistem kullanıyorsanız, CA kök sertifikasını dışarı aktarmanız gerekmez. Bir ASDK üzerinde CA kök sertifikasını dışarı aktarmanız gerekir.

PEM biçiminde ASDK kök sertifikasını dışarı aktarmak için:

1. [Azure Stack üzerinde bir Windows VM oluşturma](azure-stack-quick-windows-portal.md).

2. Makinede oturum açın, yükseltilmiş bir PowerShell istemi açın ve ardından aşağıdaki betiği çalıştırın:

    ```powershell  
      $label = "AzureStackSelfSignedRootCert"
      Write-Host "Getting certificate from the current user trusted store with subject CN=$label"
      $root = Get-ChildItem Cert:\CurrentUser\Root | Where-Object Subject -eq "CN=$label" | select -First 1
      if (-not $root)
      {
          Write-Error "Certificate with subject CN=$label not found"
          return
      }

    Write-Host "Exporting certificate"
    Export-Certificate -Type CERT -FilePath root.cer -Cert $root

    Write-Host "Converting certificate to PEM format"
    certutil -encode root.cer root.pem
    ```

3. Sertifikayı yerel makinenize kopyalayın.


### <a name="set-up-the-virtual-machine-aliases-endpoint"></a>Sanal makine diğer uç nokta ayarlamayı

Sanal makine diğer dosyasını barındıran bir genel olarak erişilebilir uç ayarlayabilirsiniz. Sanal makine diğer dosyasının bir görüntü için ortak bir ad sağlar bir JSON dosyasıdır. Azure CLI parametresi olarak bir VM dağıtırken adı kullanacaksınız.

1. Özel bir görüntü yayımlarsanız, yayımlama sırasında belirtilen yayımcı, teklif, SKU ve sürüm bilgileri not edin. Marketten bir görüntü varsa kullanarak bilgileri görüntüleyebilir ```Get-AzureVMImage``` cmdlet'i.  

2. İndirme [örnek dosyası](https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json) github'dan.

3. Azure Stack'te bir depolama hesabı oluşturun. Tamamlandığında, bir blob kapsayıcı oluşturun. "Genel" erişim ilkesi ayarlayın  

4. JSON dosyasını yeni kapsayıcısına yükleyin. Bu yapıldığında blobun URL'sini görüntüleyebilirsiniz. Blob adı belirledikten sonra URL'yi blob özelliklerini seçin.

### <a name="install-or-upgrade-cli"></a>Yüklemeniz veya yükseltmeniz CLI

Geliştirme iş istasyonunda oturum açın ve CLI'yı yükleyin. Azure Stack 2.0 veya Azure CLI'ın sonraki bir sürümünü gerektirir. CLI'ın geçerli bir sürüm API profillerini en son sürümünü gerektirir.  İçinde açıklanan adımları kullanarak CLI'yı yükleyebilirsiniz [Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) makalesi. Yüklemenin başarılı olup olmadığını doğrulamak için bir terminal veya komut istemi penceresi açın ve aşağıdaki komutu çalıştırın:

```shell
az --version
```

Azure CLI ve bilgisayarınızda yüklü diğer bağımlı kitaplıkların sürüm görmeniz gerekir.

### <a name="install-python-on-windows"></a>Windows üzerinde Python'ı yükleyin

1. Yükleme [Python 3 sisteminize](https://www.python.org/downloads/).

2. PIP yükseltin. PIP, Python için bir paket yöneticisidir. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```powershell  
    python -m pip install --upgrade pip
    ```

3. Yükleme **certifi** modülü. [Certifi](https://pypi.org/project/certifi/) TLS konakları kimliğini doğrularken, SSL sertifikaları güvenilirliğini doğrulamak için bir modül ve kök sertifika koleksiyonunu olduğu. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```powershell
    pip install certifi
    ```

### <a name="install-python-on-linux"></a>Linux'ta Python yükleme

1. Ubuntu 16.04 görüntü, Python 2.7 ve Python 3. 5'in varsayılan olarak yüklenen ile birlikte gelir. Python 3 sürümünüz, aşağıdaki komutu çalıştırarak doğrulayabilirsiniz:

    ```bash  
    python3 --version
    ```

2. PIP yükseltin. PIP, Python için bir paket yöneticisidir. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```bash  
    sudo -H pip3 install --upgrade pip
    ```

3. Yükleme **certifi** modülü. [Certifi](https://pypi.org/project/certifi/) kök sertifikaları SSL sertifikaları güvenilirliğini doğrulamak için TLS konakları kimliğini doğrularken koleksiyonudur. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```bash
    pip3 install certifi
    ```

### <a name="install-python-on-macos"></a>Macos'ta Python yükleme

1. Yükleme [Python 3 sisteminize](https://www.python.org/downloads/). Python 3.7 sürümleri için Python.org indirmek için iki ikili yükleyici seçenekleri sağlar. Varsayılan değişken, 64-bit-yalnızca ve 10.9 (Mavericks) ve üzeri sistemleri macOS üzerinde çalışır. Terminal açıp aşağıdaki komutu yazarak, python sürümünüzdür denetleyin:

    ```bash  
    python3 --version
    ```

2. PIP yükseltin. PIP, Python için bir paket yöneticisidir. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```bash  
    sudo -H pip3 install --upgrade pip
    ```

3. Yükleme **certifi** modülü. [Certifi](https://pypi.org/project/certifi/) TLS konakları kimliğini doğrularken, SSL sertifikaları güvenilirliğini doğrulamak için bir modül ve kök sertifika koleksiyonunu olduğu. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```bash
    pip3 install certifi
    ```

## <a name="windows-azure-ad"></a>Windows (Azure AD)

Azure AD, kimlik yönetimi hizmeti olarak kullanıyorsanız ve bir Windows makinede CLI kullanıyorsanız bu bölümde, CLI ayarlama yol gösterir.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını güven

ASDK kullanıyorsanız, CA kök sertifikasını uzak makinenizdeki güven gerekir. Tümleşik sistemlerle bunu gerekmez.

Azure Stack CA kök sertifikasına güvenmek için mevcut Python sertifikayı ekleyin.

1. Makinenizde sertifika konumu bulun. Konum, Python yüklediğiniz bağlı olarak değişiklik gösterebilir. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```powershell  
      python -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu not edin. Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Belirli yolunuzu işletim sisteminiz ve yüklemiş olduğunuz Python sürümünü bağlıdır.

2. Azure Stack CA kök sertifikasını ekleyerek mevcut Python sertifikaya güvenir.

    ```powershell
    $pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

    $root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $root.Import($pemFile)

    Write-Host "Extracting required information from the cert file"
    $md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
    $sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
    $sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

    $issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
    $subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
    $labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
    $serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
    $md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
    $sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
    $sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
    $certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

    $rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
    $serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

    Write-Host "Adding the certificate content to Python Cert store"
    Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

    Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"
    ```

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

1. Azure Stack ortamınıza çalıştırarak kayıt `az cloud register` komutu.

    Bazı senaryolarda, doğrudan giden internet bağlantısı, bir proxy veya SSL durdurma zorlar güvenlik duvarı yönlendirilir. Bu gibi durumlarda, `az cloud register` komut "uç noktalar buluttan alınamıyor." şeklinde bir hata ile başarısız olabilir Bu hatayı çözmek için aşağıdaki ortam değişkenlerini ayarlayabilirsiniz:

    ```shell  
    set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
    set ADAL_PYTHON_SSL_NO_VERIFY=1
    ```

2. Ortamınıza kaydedin. Çalıştırırken aşağıdaki parametreleri kullanın `az cloud register`.

    | Değer | Örnek | Açıklama |
    | --- | --- | --- |
    | Ortam adı | AzureStackUser | Kullanım `AzureStackUser` kullanıcı ortam için. İşleci, belirtin `AzureStackAdmin`. |
    | Resource Manager uç noktası | https://management.local.azurestack.external | **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): `https://management.local.azurestack.external/` **ResourceManagerUrl** tümleşik sisteminde: `https://management.<region>.<fqdn>/` Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0` Tümleşik sistem uç nokta hakkında bir sorunuz varsa, bulut operatörünüze başvurun. |
    | Depolama uç noktası | Local.azurestack.external | `local.azurestack.external` için ASDK olur. Tümleşik bir sistem için bir uç nokta sisteminiz için kullanmak istediğiniz.  |
    | Keyvalut soneki | .vault.local.azurestack.external | `.vault.local.azurestack.external` için ASDK olur. Tümleşik bir sistem için bir uç nokta sisteminiz için kullanmak istediğiniz.  |
    | VM görüntüsü diğer belge uç nokta- | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | Sanal makine görüntüsü diğer adları içeren belge URI'si. Daha fazla bilgi için [### sanal makine diğer uç nokta ayarlamayı](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
    ```

1. Aşağıdaki komutları kullanarak etkin ortam ayarlayın.

      ```azurecli
      az cloud set -n <environmentname>
      ```

1. Azure Stack belirli API Sürüm profili kullanmak için ortamınızdaki yapılandırmayı güncelleştirin. Yapılandırmasını güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    az cloud update --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Azure Stack sürümü 1808 yapıdan çalıştırıyorsanız, API Sürüm profili kullanmalısınız **2017-03-09-profile** API Sürüm profili yerine **2018-03-01-karma**. Azure CLI'ın yeni bir sürümü kullanıyor gerekir.
 
1. Azure Stack ortamınıza kullanarak oturum açın `az login` komutu. Azure Stack ortamına veya bir kullanıcı olarak oturum bir [hizmet sorumlusu](../../active-directory/develop/app-objects-and-service-principals.md). 

   - Olarak oturum bir *kullanıcı*: 

     Kullanıcı adı ve parola doğrudan içinde ya da belirtebilirsiniz `az login` komutunu ya da bir tarayıcı kullanarak kimlik doğrulaması. Çok faktörlü kimlik doğrulaması etkin hesabınız varsa, ikincisi yapmanız gerekir:

     ```azurecli
     az login -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
     ```

     > [!NOTE]
     > Çok faktörlü kimlik doğrulaması etkin kullanıcı hesabınız varsa kullanabileceğiniz `az login` sağlamadan komutunu `-u` parametresi. Bu komut çalıştıran bir URL ve kimlik doğrulaması yapmak için kullanmanız gereken kodu sağlar.

   - Olarak oturum bir *hizmet sorumlusu*: 
    
     Oturum açarken, önce [Azure Portalı aracılığıyla hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md) veya CLI ve bir rol atayın. Aşağıdaki komutu kullanarak şimdi oturum açın:

     ```azurecli  
     az login --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> --service-principal -u <Application Id of the Service Principal> -p <Key generated for the Service Principal>
     ```

### <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyi ayarlamak, CLI Azure Stack içinde kaynaklar oluşturabiliriz. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az group create -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komutta aşağıdaki özellikleri yeni oluşturulan kaynağın verir:

![Çıkış kaynak grubu oluşturun](media/azure-stack-connect-cli/image1.png)

## <a name="windows-ad-fs"></a>Windows (AD FS)

Active Directory Federasyon Hizmetleri'nde (AD FS), kimlik yönetimi hizmeti olarak kullanıyorsanız ve bir Windows makinede CLI kullanıyorsanız bu bölümde, CLI ayarlama yol gösterir.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını güven

ASDK kullanıyorsanız, CA kök sertifikasını uzak makinenizdeki güven gerekir. Tümleşik sistemlerle bunu gerekmez.

1. Makinenizde sertifika konumu bulun. Konum, Python yüklediğiniz bağlı olarak değişiklik gösterebilir. Bir komut istemi veya yükseltilmiş bir PowerShell istemi açın ve aşağıdaki komutu yazın:

    ```powershell  
      python -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu not edin. Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Belirli yolunuzu işletim sisteminiz ve yüklemiş olduğunuz Python sürümünü bağlıdır.

2. Azure Stack CA kök sertifikasını ekleyerek mevcut Python sertifikaya güvenir.

    ```powershell
    $pemFile = "<Fully qualified path to the PEM certificate Ex: C:\Users\user1\Downloads\root.pem>"

    $root = New-Object System.Security.Cryptography.X509Certificates.X509Certificate2
    $root.Import($pemFile)

    Write-Host "Extracting required information from the cert file"
    $md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
    $sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
    $sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

    $issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
    $subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
    $labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
    $serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
    $md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
    $sha1Entry    = [string]::Format("# SHA1 Fingerprint: {0}", $sha1Hash)
    $sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
    $certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

    $rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
    $serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

    Write-Host "Adding the certificate content to Python Cert store"
    Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

    Write-Host "Python Cert store was updated to allow the Azure Stack CA root certificate"
    ```

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

1. Azure Stack ortamınıza çalıştırarak kayıt `az cloud register` komutu.

    Bazı senaryolarda, doğrudan giden internet bağlantısı, bir proxy veya SSL durdurma zorlar güvenlik duvarı yönlendirilir. Bu gibi durumlarda, `az cloud register` komut "uç noktalar buluttan alınamıyor." şeklinde bir hata ile başarısız olabilir Bu hatayı çözmek için aşağıdaki ortam değişkenlerini ayarlayabilirsiniz:

    ```shell  
    set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
    set ADAL_PYTHON_SSL_NO_VERIFY=1
    ```

2. Ortamınıza kaydedin. Çalıştırırken aşağıdaki parametreleri kullanın `az cloud register`.

    | Değer | Örnek | Açıklama |
    | --- | --- | --- |
    | Ortam adı | AzureStackUser | Kullanım `AzureStackUser` kullanıcı ortam için. İşleci, belirtin `AzureStackAdmin`. |
    | Resource Manager uç noktası | https://management.local.azurestack.external | **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): `https://management.local.azurestack.external/` **ResourceManagerUrl** tümleşik sisteminde: `https://management.<region>.<fqdn>/` Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0` Tümleşik sistem uç nokta hakkında bir sorunuz varsa, bulut operatörünüze başvurun. |
    | Depolama uç noktası | Local.azurestack.external | `local.azurestack.external` için ASDK olur. Tümleşik bir sistem için bir uç nokta sisteminiz için kullanmak istediğiniz.  |
    | Keyvalut soneki | .vault.local.azurestack.external | `.vault.local.azurestack.external` için ASDK olur. Tümleşik bir sistem için bir uç nokta sisteminiz için kullanmak istediğiniz.  |
    | VM görüntüsü diğer belge uç nokta- | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | Sanal makine görüntüsü diğer adları içeren belge URI'si. Daha fazla bilgi için [### sanal makine diğer uç nokta ayarlamayı](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
    ```

1. Aşağıdaki komutları kullanarak etkin ortam ayarlayın.

      ```azurecli
      az cloud set -n <environmentname>
      ```

1. Azure Stack belirli API Sürüm profili kullanmak için ortamınızdaki yapılandırmayı güncelleştirin. Yapılandırmasını güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    az cloud update --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Azure Stack sürümü 1808 yapıdan çalıştırıyorsanız, API Sürüm profili kullanmalısınız **2017-03-09-profile** API Sürüm profili yerine **2018-03-01-karma**. Azure CLI'ın yeni bir sürümü kullanıyor gerekir.

1. Azure Stack ortamınıza kullanarak oturum açın `az login` komutu. Azure Stack ortamına veya bir kullanıcı olarak oturum bir [hizmet sorumlusu](../../active-directory/develop/app-objects-and-service-principals.md). 

   - Olarak oturum bir *kullanıcı*:

     Kullanıcı adı ve parola doğrudan içinde ya da belirtebilirsiniz `az login` komutunu ya da bir tarayıcı kullanarak kimlik doğrulaması. Çok faktörlü kimlik doğrulaması etkin hesabınız varsa, ikincisi yapmanız gerekir:

     ```azurecli
     az cloud register  -n <environmentname>   --endpoint-resource-manager "https://management.local.azurestack.external"  --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-active-directory-resource-id "https://management.adfs.azurestack.local/<tenantID>" --endpoint-active-directory-graph-resource-id "https://graph.local.azurestack.external/" --endpoint-active-directory "https://adfs.local.azurestack.external/adfs/" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>   --profile "2018-03-01-hybrid"
     ```

     > [!NOTE]
     > Çok faktörlü kimlik doğrulaması etkin kullanıcı hesabınız varsa kullanabileceğiniz `az login` sağlamadan komutunu `-u` parametresi. Bu komut çalıştıran bir URL ve kimlik doğrulaması yapmak için kullanmanız gereken kodu sağlar.

   - Olarak oturum bir *hizmet sorumlusu*: 
    
     Hizmet sorumlusu oturum açma için kullanılacak .pem dosyasını hazırlayın.

     Bir pfx özel anahtarla konumundaki gibi asıl oluşturulduğu istemci makinede hizmet sorumlusu sertifikasını dışarı aktarma `cert:\CurrentUser\My`; sertifika adı, sorumlu aynı ada sahip.

     Pfx için pem (OpenSSL yardımcı programını kullanın) dönüştürün.

     CLI için oturum açın:
  
     ```azurecli  
     az login --service-principal \
      -u <Client ID from the Service Principal details> \
      -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
      --tenant <Tenant ID> \
      --debug 
     ```

### <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyi ayarlamak, CLI Azure Stack içinde kaynaklar oluşturabiliriz. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az group create -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komutta aşağıdaki özellikleri yeni oluşturulan kaynağın verir:

![Çıkış kaynak grubu oluşturun](media/azure-stack-connect-cli/image1.png)


## <a name="linux-azure-ad"></a>Linux (Azure AD)

Azure AD, kimlik yönetimi hizmeti olarak kullanıyorsanız ve bir Linux makinesinde CLI kullanıyorsanız bu bölümde, CLI ayarlama yol gösterir.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını güven

ASDK kullanıyorsanız, CA kök sertifikasını uzak makinenizdeki güven gerekir. Tümleşik sistemlerle bunu gerekmez.

Azure Stack CA kök sertifikasını ekleyerek mevcut Python sertifikaya güvenir.

1. Makinenizde sertifika konumu bulun. Konum, Python yüklediğiniz bağlı olarak değişiklik gösterebilir. Pip ve certifi sahip olması gerekir [Modülü yüklü](#install-python-on-linux). Aşağıdaki bash isteminde Python komutunu kullanabilirsiniz:

    ```bash  
    python3 -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu not edin; Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. İşletim sisteminiz ve yüklemiş olduğunuz Python sürümünü belirli yolunuzu bağlıdır.

2. Aşağıdaki bash komut sertifikanızı yol ile çalıştırın.

   - Uzak Linux makinesi için:

     ```bash  
     sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
     ```

   - Azure Stack ortamında bir Linux makine için:

     ```bash  
     sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
     ```

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Azure Stack'e bağlanmak için aşağıdaki adımları kullanın:

1. Azure Stack ortamınıza çalıştırarak kayıt `az cloud register` komutu. Bazı senaryolarda, doğrudan giden internet bağlantısı, bir proxy veya SSL durdurma zorlar güvenlik duvarı yönlendirilir. Bu gibi durumlarda, `az cloud register` komut "uç noktalar buluttan alınamıyor." şeklinde bir hata ile başarısız olabilir Bu hatayı çözmek için aşağıdaki ortam değişkenlerini ayarlayabilirsiniz:

   ```shell
   set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
   set ADAL_PYTHON_SSL_NO_VERIFY=1
   ```

2. Ortamınıza kaydedin. Çalıştırırken aşağıdaki parametreleri kullanın `az cloud register`.

    | Değer | Örnek | Açıklama |
    | --- | --- | --- |
    | Ortam adı | AzureStackUser | Kullanım `AzureStackUser` kullanıcı ortam için. İşleci, belirtin `AzureStackAdmin`. |
    | Resource Manager uç noktası | https://management.local.azurestack.external | **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): `https://management.local.azurestack.external/` **ResourceManagerUrl** tümleşik sisteminde: `https://management.<region>.<fqdn>/` Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0` Tümleşik sistem uç nokta hakkında bir sorunuz varsa, bulut operatörünüze başvurun. |
    | Depolama uç noktası | Local.azurestack.external | `local.azurestack.external` için ASDK olur. Tümleşik bir sistem için bir uç nokta sisteminiz için kullanmak istediğiniz.  |
    | Keyvalut soneki | .vault.local.azurestack.external | `.vault.local.azurestack.external` için ASDK olur. Tümleşik bir sistem için bir uç nokta sisteminiz için kullanmak istediğiniz.  |
    | VM görüntüsü diğer belge uç nokta- | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | Sanal makine görüntüsü diğer adları içeren belge URI'si. Daha fazla bilgi için [### sanal makine diğer uç nokta ayarlamayı](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
    ```

3. Etkin ortam ayarlayın. 

      ```azurecli
        az cloud set -n <environmentname>
      ```

4. Azure Stack belirli API Sürüm profili kullanmak için ortamınızdaki yapılandırmayı güncelleştirin. Yapılandırmasını güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
      az cloud update --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Azure Stack sürümü 1808 yapıdan çalıştırıyorsanız, API Sürüm profili kullanmalısınız **2017-03-09-profile** API Sürüm profili yerine **2018-03-01-karma**. Azure CLI'ın yeni bir sürümü kullanıyor gerekir.

5. Azure Stack ortamınıza kullanarak oturum açın `az login` komutu. Azure Stack ortamına veya bir kullanıcı olarak oturum bir [hizmet sorumlusu](../../active-directory/develop/app-objects-and-service-principals.md). 

   * Olarak oturum bir *kullanıcı*:

     Kullanıcı adı ve parola doğrudan içinde ya da belirtebilirsiniz `az login` komutunu ya da bir tarayıcı kullanarak kimlik doğrulaması. Çok faktörlü kimlik doğrulaması etkin hesabınız varsa, ikincisi yapmanız gerekir:

     ```azurecli
     az login \
       -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
       --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
     ```

     > [!NOTE]
     > Çok faktörlü kimlik doğrulaması etkin kullanıcı hesabınız varsa kullanabileceğiniz `az login` sağlamadan komutunu `-u` parametresi. Bu komut çalıştıran bir URL ve kimlik doğrulaması yapmak için kullanmanız gereken kodu sağlar.
   
   * Olarak oturum bir *hizmet sorumlusu*
    
     Oturum açarken, önce [Azure Portalı aracılığıyla hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md) veya CLI ve bir rol atayın. Aşağıdaki komutu kullanarak şimdi oturum açın:

     ```azurecli  
     az login \
       --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
       --service-principal \
       -u <Application Id of the Service Principal> \
       -p <Key generated for the Service Principal>
     ```

### <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyi ayarlamak, CLI Azure Stack içinde kaynaklar oluşturabiliriz. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
    az group create -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komutta aşağıdaki özellikleri yeni oluşturulan kaynağın verir:

![Çıkış kaynak grubu oluşturun](media/azure-stack-connect-cli/image1.png)

## <a name="linux-ad-fs"></a>Linux (AD FS)

Active Directory Federasyon Hizmetleri'nde (AD FS), yönetim hizmeti olarak kullanıyorsanız ve bir Linux makinesinde CLI kullanıyorsanız bu bölümde, CLI ayarlama yol gösterir.

### <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını güven

ASDK kullanıyorsanız, CA kök sertifikasını uzak makinenizdeki güven gerekir. Tümleşik sistemlerle bunu gerekmez.

Azure Stack CA kök sertifikasını ekleyerek mevcut Python sertifikaya güvenir.

1. Makinenizde sertifika konumu bulun. Konum, Python yüklediğiniz bağlı olarak değişiklik gösterebilir. Pip ve certifi sahip olması gerekir [Modülü yüklü](#install-python-on-linux). Aşağıdaki bash isteminde Python komutunu kullanabilirsiniz:

    ```bash  
    python3 -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu not edin; Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. İşletim sisteminiz ve yüklemiş olduğunuz Python sürümünü belirli yolunuzu bağlıdır.

2. Aşağıdaki bash komut sertifikanızı yol ile çalıştırın.

   - Uzak Linux makinesi için:

     ```bash  
     sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
     ```

   - Azure Stack ortamında bir Linux makine için:

     ```bash  
     sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
     ```

### <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Azure Stack'e bağlanmak için aşağıdaki adımları kullanın:

1. Azure Stack ortamınıza çalıştırarak kayıt `az cloud register` komutu. Bazı senaryolarda, doğrudan giden internet bağlantısı, bir proxy veya SSL durdurma zorlar güvenlik duvarı yönlendirilir. Bu gibi durumlarda, `az cloud register` komut "uç noktalar buluttan alınamıyor." şeklinde bir hata ile başarısız olabilir Bu hatayı çözmek için aşağıdaki ortam değişkenlerini ayarlayabilirsiniz:

   ```shell
   set AZURE_CLI_DISABLE_CONNECTION_VERIFICATION=1 
   set ADAL_PYTHON_SSL_NO_VERIFY=1
   ```

2. Ortamınıza kaydedin. Çalıştırırken aşağıdaki parametreleri kullanın `az cloud register`.

    | Değer | Örnek | Açıklama |
    | --- | --- | --- |
    | Ortam adı | AzureStackUser | Kullanım `AzureStackUser` kullanıcı ortam için. İşleci, belirtin `AzureStackAdmin`. |
    | Resource Manager uç noktası | https://management.local.azurestack.external | **ResourceManagerUrl** olan Azure Stack geliştirme Seti'ni (ASDK): `https://management.local.azurestack.external/` **ResourceManagerUrl** tümleşik sisteminde: `https://management.<region>.<fqdn>/` Gerekli meta verilerini almak için: `<ResourceManagerUrl>/metadata/endpoints?api-version=1.0` Tümleşik sistem uç nokta hakkında bir sorunuz varsa, bulut operatörünüze başvurun. |
    | Depolama uç noktası | Local.azurestack.external | `local.azurestack.external` için ASDK olur. Tümleşik bir sistem için bir uç nokta sisteminiz için kullanmak istediğiniz.  |
    | Keyvalut soneki | .vault.local.azurestack.external | `.vault.local.azurestack.external` için ASDK olur. Tümleşik bir sistem için bir uç nokta sisteminiz için kullanmak istediğiniz.  |
    | VM görüntüsü diğer belge uç nokta- | https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json | Sanal makine görüntüsü diğer adları içeren belge URI'si. Daha fazla bilgi için [### sanal makine diğer uç nokta ayarlamayı](#set-up-the-virtual-machine-aliases-endpoint). |

    ```azurecli  
    az cloud register -n <environmentname> --endpoint-resource-manager "https://management.local.azurestack.external" --suffix-storage-endpoint "local.azurestack.external" --suffix-keyvault-dns ".vault.local.azurestack.external" --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
    ```

3. Etkin ortam ayarlayın. 

      ```azurecli
        az cloud set -n <environmentname>
      ```

4. Azure Stack belirli API Sürüm profili kullanmak için ortamınızdaki yapılandırmayı güncelleştirin. Yapılandırmasını güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
      az cloud update --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Azure Stack sürümü 1808 yapıdan çalıştırıyorsanız, API Sürüm profili kullanmalısınız **2017-03-09-profile** API Sürüm profili yerine **2018-03-01-karma**. Azure CLI'ın yeni bir sürümü kullanıyor gerekir.

5. Azure Stack ortamınıza kullanarak oturum açın `az login` komutu. Azure Stack ortamına veya bir kullanıcı olarak oturum bir [hizmet sorumlusu](../../active-directory/develop/app-objects-and-service-principals.md). 

6. Oturum Aç: 

   *  Olarak bir **kullanıcı** cihaz kodu ile bir web tarayıcısı kullanarak:  

   ```azurecli  
    az login --use-device-code
   ```

   > [!NOTE]  
   >Komut çalıştıran bir URL ve kimlik doğrulaması yapmak için kullanmanız gereken kodu sağlar.

   * Hizmet sorumlusu olarak:
        
     Hizmet sorumlusu oturum açma için kullanılacak .pem dosyasını hazırlayın.

      * Bir pfx özel anahtarla konumundaki gibi asıl oluşturulduğu istemci makinede hizmet sorumlusu sertifikasını dışarı aktarma `cert:\CurrentUser\My`; sertifika adı, sorumlu aynı ada sahip.
  
      * Pfx için pem (OpenSSL yardımcı programını kullanın) dönüştürün.

     CLI için oturum açın:

      ```azurecli  
      az login --service-principal \
        -u <Client ID from the Service Principal details> \
        -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
        --tenant <Tenant ID> \
        --debug 
      ```

### <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyi ayarlamak, CLI Azure Stack içinde kaynaklar oluşturabiliriz. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
  az group create -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komutta aşağıdaki özellikleri yeni oluşturulan kaynağın verir:

![Çıkış kaynak grubu oluşturun](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bilinen sorunlar

Burada Azure Stack CLI kullanarak karşılaşılan bilinen sorunlar verilmiştir:

 - CLI etkileşimli mod; Örneğin, `az interactive` komutu, henüz Azure Stack'te desteklenmiyor.
 - Azure Stack'te kullanılabilir sanal makine görüntüleri listesini almak için kullanın `az vm image list --all` komutu yerine `az vm image list` komutu. Belirtme `--all` seçeneği sağlar yanıt yalnızca Azure Stack ortamınıza kullanılabilir görüntüleri döndürür.
 - Azure'da kullanılabilen sanal makine görüntüsü diğer adlar, Azure Stack için geçerli olmayabilir. Sanal makine görüntüleri kullanarak, tüm URN parametresini kullanmanız gerekir (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) yerine görüntü diğer adı. Bu URN görüntü belirtimleri ten türetildiği haliyle eşleşmelidir `az vm images list` komutu.

## <a name="next-steps"></a>Sonraki adımlar

- [Şablonları Azure CLI ile dağıtma](azure-stack-deploy-template-command-line.md)
- [Azure Stack kullanıcıları (işleç) için Azure CLI'yi etkinleştirme](../azure-stack-cli-admin.md)
- [Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md) 
