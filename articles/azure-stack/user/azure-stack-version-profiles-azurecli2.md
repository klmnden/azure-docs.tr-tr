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
ms.date: 06/25/2018
ms.author: mabrigg
ms.reviewer: sijuman
ms.openlocfilehash: 1b59409e43a23dd63a6697a44a20df079a751516
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/06/2018
ms.locfileid: "37866867"
---
# <a name="use-api-version-profiles-with-azure-cli-20-in-azure-stack"></a>Azure Stack'te Azure CLI 2.0 ile API Sürüm profillerini kullanma

Linux, Mac ve Windows istemci platformlarını Azure Stack geliştirme Seti'ni kaynakları yönetmek için Azure komut satırı arabirimi (CLI) yedekleme ayarlamak için bu makaledeki adımları izleyebilirsiniz.

## <a name="install-cli"></a>CLI yükleme

Geliştirme iş istasyonunda oturum açın ve CLI'yı yükleyin. Azure Stack, Azure CLI 2.0 sürümünü gerektirir. Bu bölümünde açıklanan adımları kullanarak yükleyin [Azure CLI 2.0 yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) makalesi. Yüklemenin başarılı olup olmadığını doğrulamak için bir terminal veya komut istemi penceresi açın ve aşağıdaki komutu çalıştırın:

```azurecli
az --version
```

Azure CLI ve bilgisayarınızda yüklü diğer bağımlı kitaplıkların sürüm görmeniz gerekir.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure Stack CA kök sertifikasını güven

1. Azure Stack CA kök sertifikası alın [, Azure Stack operatörü](..\azure-stack-cli-admin.md#export-the-azure-stack-ca-root-certificate) ve buna güvenmesi. Azure Stack CA kök sertifikasına güvenmek için mevcut Python sertifikayı ekleyin.

2. Makinenizde sertifika konumu bulun. Konum, Python yüklediğiniz bağlı olarak değişiklik gösterebilir. Sahip olması gerekir [pip](https://pip.pypa.io) ve [certifi](https://pypi.org/project/certifi/) Modülü yüklü. Aşağıdaki bash isteminde Python komutunu kullanabilirsiniz:

  ```bash  
    python -c "import certifi; print(certifi.where())"
  ```

  Sertifika konumunu not edin. Örneğin, `~/lib/python3.5/site-packages/certifi/cacert.pem`. Belirli yolunuzu işletim sisteminiz ve yüklemiş olduğunuz Python sürümünü bağlıdır.

### <a name="set-the-path-for-a-development-machine-inside-the-cloud"></a>Bir geliştirme makineniz için yol içinde Ayarla

CLI'yi Azure Stack ortamda oluşturulan bir Linux makinesinde çalıştırıyorsanız, aşağıdaki bash komut sertifikanızı yol ile çalıştırın.

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/<yourpath>/cacert.pem
```

### <a name="set-the-path-for-a-development-machine-outside-the-cloud"></a>Bir geliştirme makineniz yolunu dışında Ayarla

CLI bir makineden çalıştırıyorsanız **dışında** Azure Stack ortamı:  

1. Kurmanız [Azure Stack için VPN bağlantısı](azure-stack-connect-azure-stack.md).

2. Azure Stack operatörü aldığınız PEM sertifikası kopyalayın ve not edin (PATH_TO_PEM_FILE) dosyasının konumu.

3. Geliştirme iş istasyonunun işletim sisteminde bitiş bağlı olarak aşağıdaki komutları çalıştırın.

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

Write-Host "Extracting needed information from the cert file"
$md5Hash    = (Get-FileHash -Path $pemFile -Algorithm MD5).Hash.ToLower()
$sha1Hash   = (Get-FileHash -Path $pemFile -Algorithm SHA1).Hash.ToLower()
$sha256Hash = (Get-FileHash -Path $pemFile -Algorithm SHA256).Hash.ToLower()

$issuerEntry  = [string]::Format("# Issuer: {0}", $root.Issuer)
$subjectEntry = [string]::Format("# Subject: {0}", $root.Subject)
$labelEntry   = [string]::Format("# Label: {0}", $root.Subject.Split('=')[-1])
$serialEntry  = [string]::Format("# Serial: {0}", $root.GetSerialNumberString().ToLower())
$md5Entry     = [string]::Format("# MD5 Fingerprint: {0}", $md5Hash)
$sha1Entry    = [string]::Format("# SHA1 Finterprint: {0}", $sha1Hash)
$sha256Entry  = [string]::Format("# SHA256 Fingerprint: {0}", $sha256Hash)
$certText = (Get-Content -Path $pemFile -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Sanal makine diğer adlar uç noktası alma

Kullanıcılar, CLI kullanarak sanal makineleri oluşturmadan önce bunlar Azure Stack operatörü başvurun ve sanal makine diğer adlar uç noktası URI'si alın. Örneğin, aşağıdaki URI Azure kullanır: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. Bulut yöneticisine benzer bir uç nokta ayarlama, Azure Stack Market'te kullanıma sunulan görüntüleri ile Azure Stack için ayarlamanız gerekir. Kullanıcılar uç noktası URI'si geçmesi için `endpoint-vm-image-alias-doc` parametresi `az cloud register` sonraki bölümde gösterildiği gibi komutu. 
   

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

2. Aşağıdaki komutları kullanarak etkin ortam ayarlayın.

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

3. Azure Stack belirli API Sürüm profili kullanmak için ortamınızdaki yapılandırmayı güncelleştirin. Yapılandırmasını güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```azurecli
   az cloud update \
     --profile 2017-03-09-profile
   ```

4. Azure Stack ortamınıza kullanarak oturum açın `az login` komutu. Azure Stack ortamına veya bir kullanıcı olarak oturum bir [hizmet sorumlusu](https://docs.microsoft.com/azure/active-directory/develop/active-directory-application-objects). 

   * Olarak oturum bir *kullanıcı*: kullanıcı adı ve parola doğrudan içinde ya da belirtebilirsiniz `az login` komutu veya bir tarayıcı kullanarak kimlik doğrulaması. Çok faktörlü kimlik doğrulaması etkin hesabınız varsa, ikincisi yapmanız gerekir.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Çok faktörlü kimlik doğrulaması etkin kullanıcı hesabınız varsa kullanabileceğiniz `az login command` sağlamadan `-u` parametresi. Komut çalıştıran bir URL ve kimlik doğrulaması yapmak için kullanmanız gereken kodu sağlar.
   
   * Olarak oturum bir *hizmet sorumlusu*: oturum açarken, önce [Azure Portalı aracılığıyla hizmet sorumlusu oluşturma](azure-stack-create-service-principals.md) veya CLI ve bir rol atayın. Aşağıdaki komutu kullanarak şimdi oturum açın:

      ```azurecli
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Her şeyi ayarladığımıza göre artık Kurulum, Azure Stack içinde kaynaklar için CLI kullanalım. Örneğin, bir uygulama için bir kaynak grubu oluşturun ve bir sanal makine ekleyin. "MyResourceGroup" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulursa, önceki komutta aşağıdaki özellikleri yeni oluşturulan kaynağın verir:

![Çıkış kaynak grubu oluşturun](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bilinen sorunlar
CLI, Azure Stack'te kullanırken dikkat edilmesi gereken bazı bilinen sorunlar vardır:

 - CLI etkileşimli mod yani `az interactive` komutu, Azure Stack'te henüz desteklenmiyor.
 - Azure Stack'te kullanılabilir sanal makine görüntüleri listesini almak için kullanın `az vm images list --all` komutu yerine `az vm image list` komutu. Belirtme `--all` yanıt yalnızca Azure Stack ortamınıza kullanılabilir görüntüleri döndürdüğünden emin olur.
 - Azure'da kullanılabilen sanal makine görüntüsü diğer adlar, Azure Stack için geçerli olmayabilir. Sanal makine görüntüleri kullanarak, tüm URN parametresini kullanmanız gerekir (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) yerine görüntü diğer adı. Bu URN görüntü belirtimleri ten türetildiği haliyle eşleşmelidir `az vm images list` komutu.

## <a name="next-steps"></a>Sonraki adımlar

[Şablonları Azure CLI ile dağıtma](azure-stack-deploy-template-command-line.md)

[Azure Stack kullanıcıları (işleç) için Azure CLI'yi etkinleştirme](..\azure-stack-cli-admin.md)

[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)