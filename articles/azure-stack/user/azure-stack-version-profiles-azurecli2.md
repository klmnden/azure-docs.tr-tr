---
title: CLI ile Azure stack'e bağlanma | Microsoft Docs
description: Platformlar arası komut satırı arabirimi (CLI) yönetmek ve Azure Stack'te kaynakları dağıtmak için kullanmayı öğrenin
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/24/2019
ms.author: sethm
ms.reviewer: sijuman
ms.openlocfilehash: 15354cd7472e7cffb7a40ca431bc23eb65b9a9a9
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2019
ms.locfileid: "54845903"
---
# <a name="use-api-version-profiles-with-azure-cli-in-azure-stack"></a>Azure Stack'te Azure CLI ile API Sürüm profillerini kullanma

Linux, Mac ve Windows istemci platformlarını Azure Stack geliştirme Seti'ni kaynakları yönetmek için Azure komut satırı arabirimi (CLI) yedekleme ayarlamak için bu makaledeki adımları izleyebilirsiniz.

## <a name="install-cli"></a>CLI yükleme

Geliştirme iş istasyonunda oturum açın ve CLI'yı yükleyin. Azure Stack 2.0 veya Azure CLI'ın sonraki bir sürümünü gerektirir. İçinde açıklanan adımları kullanarak bu sürümü yükleyebilirsiniz [Azure CLI'yı yükleme](/cli/azure/install-azure-cli) makalesi. Yüklemenin başarılı olup olmadığını doğrulamak için bir terminal veya komut istemi penceresi açın ve aşağıdaki komutu çalıştırın:

```azurecli
az --version
```

Azure CLI ve bilgisayarınızda yüklü diğer bağımlı kitaplıkların sürüm görmeniz gerekir.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını güven

1. Azure Stack CA kök sertifikası alın [, Azure Stack operatörü](../azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) ve buna güvenmesi. Azure Stack CA kök sertifikasına güvenmek için mevcut Python sertifikayı ekleyin.

1. Makinenizde sertifika konumu bulun. Konum, Python yüklediğiniz bağlı olarak değişiklik gösterebilir. Sahip olması gerekir [pip](https://pip.pypa.io) ve [certifi](https://pypi.org/project/certifi/) Modülü yüklü. Aşağıdaki bash isteminde Python komutunu kullanabilirsiniz:

    ```bash  
    python -c "import certifi; print(certifi.where())"
    ```

    Sertifika konumunu not edin; Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. İşletim sisteminiz ve yüklemiş olduğunuz Python sürümünü belirli yolunuzu bağlıdır.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Bir geliştirme makineniz için yol içinde Ayarla

CLI'yi Azure Stack ortamda oluşturulan bir Linux makinesinde çalıştırıyorsanız, aşağıdaki bash komut sertifikanızı yol ile çalıştırın.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Bir geliştirme makineniz yolunu dışında Ayarla

CLI Azure Stack ortam dışındaki bir makineden çalıştırıyorsanız:  

1. Ayarlanan [Azure Stack için VPN bağlantısı](azure-stack-connect-azure-stack.md).
1. Azure Stack operatörü aldığınız PEM sertifikası kopyalayın ve not edin (PATH_TO_PEM_FILE) dosyasının konumu.
1. Aşağıdaki bölümlerde, geliştirme iş istasyonunuzu işletim sistemine bağlı olarak komutları çalıştırın.

#### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/<yourpath>/cacert.pem
```

#### <a name="windows"></a>Windows

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

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Sanal makine diğer adlar uç noktası alma

CLI kullanarak sanal makineleri oluşturmadan önce Azure Stack operatörü başvurun ve sanal makine diğer adlar uç noktası URI'si alın. Örneğin, aşağıdaki URI Azure kullanır: `https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json`. Bulut yöneticisine benzer bir uç nokta ayarlama, Azure Stack Market'te kullanıma sunulan görüntüleri ile Azure Stack için ayarlamanız gerekir. Uç noktası URI'si geçmesi gereken, `endpoint-vm-image-alias-doc` parametresi `az cloud register` komutu, sonraki bölümde gösterildiği gibi. 
  
## <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Azure Stack'e bağlanmak için aşağıdaki adımları kullanın:

1. Azure Stack ortamınıza çalıştırarak kayıt `az cloud register` komutu.
   
    a. Kaydedilecek *bulut Yönetim* ortamı kullanın:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```
    b. Kaydedilecek *kullanıcı* ortamı kullanın:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```
    c. Kaydedilecek *kullanıcı* çoklu müşteri mimarisi bir ortam üzerinde kullanır:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases> \
        --endpoint-active-directory-resource-id=<URI of the ActiveDirectoryServiceEndpointResourceID> \
        --profile 2018-03-01-hybrid
      ```
    d. AD FS ortamında kullanıcı kaydetmek için kullanın:

      ```azurecli
      az cloud register \
        -n AzureStack  \
        --endpoint-resource-manager "https://management.local.azurestack.external" \
        --suffix-storage-endpoint "local.azurestack.external" \
        --suffix-keyvault-dns ".vault.local.azurestack.external"\
        --endpoint-active-directory-resource-id "https://management.adfs.azurestack.local/<tenantID>" \
        --endpoint-active-directory-graph-resource-id "https://graph.local.azurestack.external/"\
        --endpoint-active-directory "https://adfs.local.azurestack.external/adfs/"\
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases> \
        --profile "2018-03-01-hybrid"
      ```
1. Aşağıdaki komutları kullanarak etkin ortam ayarlayın.
   
    a. İçin *bulut Yönetim* ortamı kullanın:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

    b. İçin *kullanıcı* ortamı kullanın:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

1. Azure Stack belirli API Sürüm profili kullanmak için ortamınızdaki yapılandırmayı güncelleştirin. Yapılandırmasını güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```azurecli
    az cloud update \
      --profile 2018-03-01-hybrid
   ```

    >[!NOTE]  
    >Azure Stack sürümü 1808 yapıdan çalıştırıyorsanız, API Sürüm profili kullanmalısınız **2017-03-09-profile** API Sürüm profili yerine **2018-03-01-karma**.

1. Azure Stack ortamınıza kullanarak oturum açın `az login` komutu. Azure Stack ortamına veya bir kullanıcı olarak oturum bir [hizmet sorumlusu](../../active-directory/develop/app-objects-and-service-principals.md). 

    * Azure AD ortamları
      * Olarak oturum bir *kullanıcı*: Kullanıcı adı ve parola doğrudan içinde ya da belirtebilirsiniz `az login` komutunu ya da bir tarayıcı kullanarak kimlik doğrulaması. Çok faktörlü kimlik doğrulaması etkin hesabınız varsa, ikincisi yapmanız gerekir:

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Çok faktörlü kimlik doğrulaması etkin kullanıcı hesabınız varsa kullanabileceğiniz `az login command` sağlamadan `-u` parametresi. Bu komut çalıştıran bir URL ve kimlik doğrulaması yapmak için kullanmanız gereken kodu sağlar.
   
      * Olarak oturum bir *hizmet sorumlusu*: Oturum açarken, önce [Azure Portalı aracılığıyla hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md) veya CLI ve bir rol atayın. Aşağıdaki komutu kullanarak şimdi oturum açın:

      ```azurecli  
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```
    * AD FS ortamları

        * Bir cihaz kodu ile bir web tarayıcısı kullanarak bir kullanıcı olarak oturum açın:  
           ```azurecli  
           az login --use-device-code
           ```

           > [!NOTE]  
           >Komut çalıştıran bir URL ve kimlik doğrulaması yapmak için kullanmanız gereken kodu sağlar.

        * Bir hizmet sorumlusu olarak oturum açın:
        
          1. Hizmet sorumlusu oturum açma için kullanılacak .pem dosyasını hazırlayın.

            * Bir pfx özel anahtarla konumundaki gibi asıl oluşturulduğu istemci makinede hizmet sorumlusu sertifikasını dışarı aktarma `cert:\CurrentUser\My`; sertifika adı, sorumlu aynı ada sahip.
        
            * Pfx için pem (OpenSSL yardımcı programını kullanın) dönüştürün.

          2.  CLI için oturum açın:
            ```azurecli  
            az login --service-principal \
              -u <Client ID from the Service Principal details> \
              -p <Certificate's fully qualified name, such as, C:\certs\spn.pem>
              --tenant <Tenant ID> \
              --debug 
            ```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyi ayarlamak, CLI Azure Stack içinde kaynaklar oluşturabiliriz. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az group create \
  -n MyResourceGroup -l local
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