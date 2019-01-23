---
title: Öğretici - Azure sanal Makine'yi Python ile Azure anahtar kasası | Microsoft Docs
description: Bu öğreticide, bir anahtar kasasından gizli dizi okumak için bir Python uygulaması yapılandırma
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 1e364003093d5e37a75830386cafe855b0bdcad2
ms.sourcegitcommit: cf88cf2cbe94293b0542714a98833be001471c08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2019
ms.locfileid: "54467410"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-virtual-machine-in-python"></a>Öğretici: Bir Azure sanal makinesinde Python ile Azure Key Vault kullanma

Azure Key Vault, API anahtarları gibi gizli dizileri koruyun ve bağlantı dizeleri, uygulamalarınıza, hizmetlerinize ve BT kaynaklarına erişmek için gereken veritabanı yardımcı olur.

Bu öğreticide, Azure kaynakları için yönetilen kimliklerle bilgilerini Azure Key Vault'tan okumak için bir Azure web uygulaması için adımları izleyin. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir gizli dizi depolama.
> * Bir Azure sanal makinesi oluşturun.
> * Etkinleştirme bir [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) sanal makine için.
> * Anahtar kasasından verileri okumak konsol uygulaması için gerekli izinleri verin.
> * Anahtar kasasından bir gizli dizi alma.

Tüm daha fazla ayrıntıya geçmeden önce okuyun [anahtar kasası ile ilgili temel kavramlar](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Önkoşullar
Tüm platformlar için şunlar gerekir:

* Git ([indir](https://git-scm.com/downloads)).
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 veya sonraki sürümü. Windows, Mac ve Linux için kullanılabilir.

### <a name="managed-service-identity-and-how-it-works"></a>Yönetilen hizmet kimliği ve nasıl çalışır?
Bu öğreticide, yönetilen hizmet kimliği (MSI), yararlanır.

Azure Key Vault, kodunuzda kalmayacak kimlik bilgilerini güvenli bir şekilde depolayabilirsiniz. Bunları almak için Key Vault kimlik doğrulaması gerekir. Anahtar Kasası'na kimlik doğrulaması için bir kimlik bilgisi gerekir. Klasik bir önyükleme sorun olmasıdır. Azure ve Azure Active Directory (Azure AD) "işlemleri başlatmak daha basit hale getirir önyükleme bir kimlik" MSI sağlar.

Azure sanal makineler, App Service ve işlevler gibi bir Azure hizmeti için MSI etkinleştirdiğinizde, oluşturur bir [hizmet sorumlusu](key-vault-whatis.md#basic-concepts) Azure AD'de hizmet örneği için. Azure, hizmet sorumlusu kimlik bilgileri hizmet örneğine ekler. 

![MSI](media/MSI.png)

Ardından, kodunuzu bir erişim belirteci almak için Azure kaynak üzerinde kullanılabilir olan bir yerel meta veri hizmeti çağırır.
Kodunuzu yerel MSI uç noktasından bir Azure anahtar kasası hizmetinde kimlik doğrularken alır erişim belirtecini kullanır. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun. Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kaynak grubu adı seçin ve yer tutucuyu doldurun.
Aşağıdaki örnekte Batı ABD konumunda bir kaynak grubu oluşturulur:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Az önce oluşturduğunuz kaynak grubu bu makale boyunca kullanılır.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Ardından, önceki adımda oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturursunuz. Şu bilgileri belirtin:

* Anahtar kasası adı: Ad 3-24 karakter dizesi olmalı ve yalnızca 0-9, içermelidir a-z, A-Z ve tire (-).
* Kaynak grubu adı.
* Konum: **Batı ABD**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Güvenle korumanız ancak uygulamanıza da sağlamanız gereken bir SQL bağlantı dizesini veya başka bir bilgiyi depoluyor olabilirsiniz.

Anahtar kasasında *AppSecret* adlı bir gizli dizi oluşturmak için aşağıdaki komutları yazın. Bu gizli dizi **MySecret** değerini depolar.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>Sanal makine oluşturma

Kullanarak bir VM oluşturma [az vm oluşturma](/cli/azure/vm) komutu.

Aşağıdaki örnek, *myVM* adlı bir VM oluşturur ve *azureuser* adlı bir kullanıcı hesabı ekler. `--generate-ssh-keys` Parametre otomatik olarak bir SSH anahtarı oluşturur ve varsayılan anahtar konumunda koyar (*~/.ssh*). Bunun yerine belirli bir anahtar kümesini kullanmak için `--ssh-key-value` seçeneğini kullanın.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer. Aşağıdaki örnek çıktıda, VM oluşturma başarılı olduğunu gösterir:

```
{
  "fqdns": "",
  "id": "/subscriptions/<guid>/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-00-00-00-00-00",
  "powerState": "VM running",
  "privateIpAddress": "XX.XX.XX.XX",
  "publicIpAddress": "XX.XX.XXX.XXX",
  "resourceGroup": "myResourceGroup"
}
```

Kendi Not `publicIpAddress` VM'nizi çıktıda değeri. Sonraki adımlarda bu adres, VM’ye erişmek için kullanılır.

## <a name="assign-an-identity-to-the-virtual-machine"></a>Sanal makine için bir kimlik atama
Bu adımda, bir sanal makine için sistem tarafından atanan kimliği oluşturuyoruz. Azure CLI içinde aşağıdaki komutu çalıştırın:

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Komut çıktısı aşağıdaki gibidir. Değerini not edin **systemAssignedIdentity**. 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-the-virtual-machine-identity-permission-to-the-key-vault"></a>Anahtar kasası için sanal makine kimliği izin ver
Şimdi biz anahtar kasası kimliği izni verebilirsiniz. Şu komutu çalıştırın:

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-virtual-machine"></a>Sanal makinede oturum açın

Sanal makineye izleyerek oturum açma [Bu öğreticide](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

## <a name="create-and-run-the-sample-python-app"></a>Oluşturma ve örnek Python uygulamasını çalıştırma

Aşağıdaki örnek dosyası adlı *Sample.py*. Kullandığı [istekleri](https://pypi.org/project/requests/2.7.0/) HTTP GET çağrıları yapmak için kitaplık.

## <a name="edit-samplepy"></a>Sample.PY Düzenle
Sample.py oluşturduktan sonra dosyayı açın ve aşağıdaki kodu kopyalayın. Kod iki adımlı bir işlemdir: 
1. Bir belirteç VM'de yerel MSI uç noktasından getirin. Uç nokta, ardından Azure Active Directory'den bir belirteç getirir.
2. Anahtar Kasası'na belirtecin geçip ve gizli anahtarı getirilemedi. 

```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from an MSI-enabled Azure resource      
    # Note that the resource here is https://vault.azure.net for the public cloud, and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # Extracting data in JSON format 
    # This request gets an access token from Azure Active Directory by using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access token received from the previous HTTP GET call to the key vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Aşağıdaki komutu çalıştırarak, gizli değer görmeniz gerekir: 

```
python Sample.py
```

Yukarıdaki kod, bir Windows sanal makinesinde Azure Key Vault ile işlemleri yapıp gösterilir. 

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure anahtar kasası REST API'si](https://docs.microsoft.com/rest/api/keyvault/)
