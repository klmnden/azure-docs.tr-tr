---
title: "Azure CLI yığınla bağlanma | Microsoft Docs"
description: "Platformlar arası komut satırı arabirimi (CLI) yönetmek ve kaynakları Azure yığında dağıtmak için nasıl kullanılacağını öğrenin"
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: f576079c-5384-4c23-b5a4-9ae165d1e3c3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2017
ms.author: sngun
ms.openlocfilehash: 9a0ad3d8c2cdd3cd1d46e789c2b65677ac5a10b1
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/05/2017
---
# <a name="install-and-configure-cli-for-use-with-azure-stack"></a>CLI'yi yükleme ve Azure yığını ile kullanım için yapılandırma

Bu makalede, Azure komut satırı arabirimi (CLI) Linux Azure yığın Geliştirme Seti kaynakları yönetmek için ve Mac istemci platformları kullanarak işleminde size rehberlik ediyoruz. 

## <a name="install-cli"></a>CLI yükleme

Ardından, geliştirme iş istasyonunuzu oturum açın ve CLI yükleyin. Azure yığını, Azure CLI'ın 2.0 sürümünü gerektirir. Açıklanan adımları kullanarak yükleyin [yükleme Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) makalesi. Yüklemenin başarılı olup olmadığını doğrulamak için bir terminal veya bir komut istemi penceresi açın ve aşağıdaki komutu çalıştırın:

```azurecli
az --version
```

Azure CLI ve bilgisayarınızda yüklü diğer bağımlı kitaplıkları sürümü görmeniz gerekir.

## <a name="trust-the-azure-stack-ca-root-certificate"></a>Azure yığın CA kök sertifikasını güven

Azure yığın operatöründen Azure yığın CA kök sertifikası alın ve bu güven. Azure yığın CA kök sertifikasına güvenmek için varolan bir Python sertifikayı ekleyin. Azure yığın ortamında oluşturulan Linux makineden CLI çalıştırıyorsanız, aşağıdaki bash komutu çalıştırın:

```bash
sudo cat /var/lib/waagent/Certificates.pem >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

Azure Sack ortamı dışında bir makineden CLI çalıştırıyorsanız, ilk önce ayarlamanız gerekir [VPN bağlantısı Azure yığınına](azure-stack-connect-azure-stack.md). Şimdi, geliştirme iş istasyonunda daha önce dışarı PEM sertifikayı kopyalayın ve geliştirme iş istasyonunun OS bağlı olarak aşağıdaki komutları çalıştırın.

### <a name="linux"></a>Linux

```bash
sudo cat PATH_TO_PEM_FILE >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

### <a name="macos"></a>macOS

```bash
sudo cat PATH_TO_PEM_FILE >> ~/lib/azure-cli/lib/python2.7/site-packages/certifi/cacert.pem
```

### <a name="windows"></a>Windows

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
$certText = (Get-Content -Path root.pem -Raw).ToString().Replace("`r`n","`n")

$rootCertEntry = "`n" + $issuerEntry + "`n" + $subjectEntry + "`n" + $labelEntry + "`n" + `
$serialEntry + "`n" + $md5Entry + "`n" + $sha1Entry + "`n" + $sha256Entry + "`n" + $certText

Write-Host "Adding the certificate content to Python Cert store"
Add-Content "${env:ProgramFiles(x86)}\Microsoft SDKs\Azure\CLI2\Lib\site-packages\certifi\cacert.pem" $rootCertEntry

Write-Host "Python Cert store was updated for allowing the azure stack CA root certificate"
```

## <a name="get-the-virtual-machine-aliases-endpoint"></a>Sanal makine diğer adlar uç noktası alın

Kullanıcılar, CLI kullanarak sanal makineleri oluşturmadan önce bunlar Azure yığın işleci başvurun ve sanal makine diğer adlar uç noktası URI alın. Örneğin, aşağıdaki URI Azure kullanır: https://raw.githubusercontent.com/Azure/azure-rest-api-specs/master/arm-compute/quickstart-templates/aliases.json. Bulut yöneticisine benzer bir uç nokta ayarlamayı, Azure yığın marketi'ndeki görüntüleri ile Azure yığını için ayarlamanız gerekir. Kullanıcıların uç noktası URI geçmesi için `endpoint-vm-image-alias-doc` parametresi `az cloud register` sonraki bölümde gösterildiği gibi komutu. 
   

## <a name="connect-to-azure-stack"></a>Azure Stack'e Bağlanma

Azure yığınına bağlanmak için aşağıdaki adımları kullanın:

1. Azure yığın ortamınızı çalıştırarak kayıt `az cloud register` komutu.
   
   a. Kaydetmek için *bulut yönetici* ortamı, kullanın:

      ```azurecli
      az cloud register \ 
        -n AzureStackAdmin \ 
        --endpoint-resource-manager "https://adminmanagement.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".adminvault.local.azurestack.external" \ 
        --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

   b. Kaydetmek için *kullanıcı* ortamı, kullanın:

      ```azurecli
      az cloud register \ 
        -n AzureStackUser \ 
        --endpoint-resource-manager "https://management.local.azurestack.external" \ 
        --suffix-storage-endpoint "local.azurestack.external" \ 
        --suffix-keyvault-dns ".vault.local.azurestack.external" \ 
        --endpoint-active-directory-graph-resource-id "https://graph.windows.net/" \
        --endpoint-vm-image-alias-doc <URI of the document which contains virtual machine image aliases>
      ```

2. Etkin ortam aşağıdaki komutları kullanarak ayarlayın.

   a. İçin *bulut yönetici* ortamı, kullanın:

      ```azurecli
      az cloud set \
        -n AzureStackAdmin
      ```

   b. İçin *kullanıcı* ortamı, kullanın:

      ```azurecli
      az cloud set \
        -n AzureStackUser
      ```

3. Azure yığın belirli API sürümü profili kullanmak için ortam yapılandırmasını güncelleştirin. Yapılandırmasını güncelleştirmek için aşağıdaki komutu çalıştırın:

   ```azurecli
   az cloud update \
     --profile 2017-03-09-profile
   ```

4. Azure yığın ortamınızı kullanarak oturum `az login` komutu. Azure yığın ortama veya bir kullanıcı olarak oturum açabildiğinizi bir [hizmet sorumlusu](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-application-objects). 

   * Olarak oturum açın bir *kullanıcı*: kullanıcı adı ve parola doğrudan içinde ya da belirtebilirsiniz `az login` komutu veya bir tarayıcı kullanarak kimlik doğrulaması. Çok faktörlü kimlik doğrulaması etkin hesabınız varsa, ikinci yapmanız gerekir.

      ```azurecli
      az login \
        -u <Active directory global administrator or user account. For example: username@<aadtenant>.onmicrosoft.com> \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com>
      ```

      > [!NOTE]
      > Çok faktörlü kimlik doğrulaması etkin kullanıcı hesabınız varsa, kullanabileceğiniz `az login command` sağlamadan `-u` parametresi. Komutunu çalıştırarak, bir URL ve kimlik doğrulaması için kullanmanız gereken kod sağlar.
   
   * Olarak oturum açın bir *hizmet sorumlusu*: oturum açma, önce [Azure Portalı aracılığıyla hizmet sorumlusu oluşturmak](azure-stack-create-service-principals.md) veya CLI ve rol atama. Aşağıdaki komutu kullanarak şimdi oturum açın:

      ```azurecli
      az login \
        --tenant <Azure Active Directory Tenant name. For example: myazurestack.onmicrosoft.com> \
        --service-principal \
        -u <Application Id of the Service Principal> \
        -p <Key generated for the Service Principal>
      ```

## <a name="test-the-connectivity"></a>Bağlantısını test etme

Biz her şeyi olduğuna göre kurulum, CLI Azure yığın içindeki kaynaklara oluşturmak için kullanalım. Örneğin, bir uygulama için bir kaynak grubu oluşturmak ve bir sanal makine ekleyin. "Contoso.com" adlı bir kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli
az group create \
  -n MyResourceGroup -l local
```

Kaynak grubu başarıyla oluşturulduysa, önceki komutu aşağıdaki yeni oluşturulan kaynağın özelliklerini çıkarır:

![Kaynak grubu oluşturmak çıktı](media/azure-stack-connect-cli/image1.png)

## <a name="known-issues"></a>Bilinen sorunlar
CLI Azure yığınında kullanırken bilmeniz gereken bazı bilinen sorunlar vardır:

* CLI etkileşimli mod yani `az interactive` komutu Azure yığınında henüz desteklenmiyor.
* Sanal makine görüntülerini Azure yığınında kullanılabilir listesini almak için kullanmak `az vm images list --all` komutu yerine `az vm image list` komutu. Belirtme `--all` yanıtı yalnızca Azure yığın ortamınızda kullanılabilir görüntüleri döndürür emin olur. 
* Mevcut olan sanal makine görüntüsü diğer adlar Azure yığını için geçerli olmayabilir. Sanal makine görüntülerini kullanırken, tüm URN parametresini kullanmanız gerekir (Canonical: UbuntuServer:14.04.3-LTS:1.0.0) yerine görüntü diğer adı. Bu URN görüntü belirtimleri türetilmiş ile eşleşmelidir `az vm images list` komutu.
* Varsayılan olarak, CLI 2.0 "Standard_DS1_v2" varsayılan sanal makine görüntü boyutunu kullanır. Ancak, bu boyutu değil henüz Azure yığınında kullanılabilir, bu nedenle, belirtmeniz gerekir `--size` açıkça bir sanal makine oluşturulurken parametre. Kullanarak Azure yığınında kullanılabilir sanal makine boyutlarının listesi elde edebilirsiniz `az vm list-sizes --location <locationName>` komutu.


## <a name="next-steps"></a>Sonraki adımlar

[Azure CLI ile şablonlarını dağıtma](azure-stack-deploy-template-command-line.md)

[Kullanıcı izinlerini yönetme](azure-stack-manage-permissions.md)

