---
title: Öğretici - Azure anahtar Kasası'nda gizli dizileri depolamak için bir Linux sanal makinesi ve bir Python uygulamasını kullanın | Microsoft Docs
description: Bu öğreticide, Azure Key Vault'tan bir gizli dizi okumak için bir Python uygulamasının nasıl yapılandırılacağını öğrenin.
services: key-vault
author: mbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: b7077653ec959f99491cecd71573c091772448f4
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60461085"
---
# <a name="tutorial-use-a-linux-vm-and-a-python-app-to-store-secrets-in-azure-key-vault"></a>Öğretici: Gizli dizileri Azure Key Vault'ta depolamak için bir Linux VM ile bir Python uygulaması'nı kullanın

Azure Key Vault, API anahtarları gibi gizli dizileri koruyun ve bağlantı dizeleri, uygulamalarınıza, hizmetlerinize ve BT kaynaklarına erişmek için gereken veritabanı yardımcı olur.

Bu öğreticide, Azure kaynakları için yönetilen kimliklerle bilgilerini Azure Key Vault'tan okumak için bir Azure web uygulaması ayarlayın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma
> * Anahtar kasanızda bir gizli dizi Store
> * Linux sanal makinesi oluşturma
> * Etkinleştirme bir [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) sanal makine için
> * Anahtar kasasından verileri okumak konsol uygulaması için gerekli izinleri verin
> * Key vault'ta bir gizli dizi alma

Tüm daha fazla ayrıntıya geçmeden önce anladığınızdan emin olun [anahtar kasası ile ilgili temel kavramlar](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Önkoşullar

* [Git](https://git-scm.com/downloads).
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
* [Azure CLI Sürüm 2.0.4 veya sonraki](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) veya Azure Cloud Shell.

[!INCLUDE [Azure Cloud Shell](../../includes/cloud-shell-try-it.md)]

## <a name="understand-managed-service-identity"></a>Yönetilen hizmet kimliği anlama

Azure Key Vault, kodunuzda kalmayacak kimlik bilgilerini güvenli bir şekilde depolayabilirsiniz. Bunları almak için Azure anahtar Kasası'na kimlik doğrulaması gerekir. Ancak, anahtar Kasası'na kimlik doğrulaması için bir kimlik bilgisi gerekir. Klasik bir önyükleme sorun var. Azure ve Azure Active Directory (Azure AD) yönetilen hizmet kimliği (MSI) işlemleri başlatmak daha basit hale getirir önyükleme bir kimlik sağlar.

Sanal makineler, App Service ve işlevler gibi bir Azure hizmeti için MSI etkinleştirdiğinizde, Azure hizmet sorumlusu hizmet örneği için Azure AD'de oluşturur. Hizmet sorumlusu kimlik bilgileri hizmet örneğine ekler.

![MSI](media/MSI.png)

Ardından, kodunuzu bir erişim belirteci almak için Azure kaynak üzerinde kullanılabilir olan bir yerel meta veri hizmeti çağırır. Kodunuzu yerel MSI uç noktasından bir Azure anahtar kasası hizmetinde kimlik doğrularken alır erişim belirtecini kullanır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmanız için şunu girin:

```azurecli-interactive
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

Kullanarak bir kaynak grubu oluşturma `az group create` aşağıdaki kodla Batı ABD konumunda komutu. Değiştirin `YourResourceGroupName` ile kendi tercih ettiğiniz bir ad.

```azurecli-interactive
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Bu kaynak grubu öğretici boyunca kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Ardından, önceki adımda oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturun. Şu bilgileri belirtin:

* Anahtar kasası adı: Ad 3-24 karakter dizesi olmalı ve yalnızca 0-9, içermelidir a-z, A-Z ve tire (-).
* Kaynak grubu adı.
* Konum: **Batı ABD**.

```azurecli-interactive
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Bir SQL bağlantı dizesi saklamak isteyebilirsiniz veya her ikisi de olması gereken herhangi bir bilgi güvenli ve uygulamanız için kullanılabilir tutulur.

Anahtar kasasında *AppSecret* adlı bir gizli dizi oluşturmak için aşağıdaki komutları yazın. Bu gizli dizi **MySecret** değerini depolar.

```azurecli-interactive
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-linux-virtual-machine"></a>Linux sanal makinesi oluşturma

Kullanarak bir VM oluşturma `az vm create` komutu.

Aşağıdaki örnek, **myVM** adlı bir VM oluşturur ve **azureuser** adlı bir kullanıcı hesabı ekler. `--generate-ssh-keys` Parametre otomatik olarak bir SSH anahtarı oluşturur ve varsayılan anahtar konumunda koyar (**~/.ssh**). Bunun yerine belirli bir anahtar kümesini oluşturmak için kullanın `--ssh-key-value` seçeneği.

```azurecli-interactive
az vm create \
  --resource-group myResourceGroup \
  --name myVM \
  --image UbuntuLTS \
  --admin-username azureuser \
  --generate-ssh-keys
```

VM’yi ve destekleyici kaynakları oluşturmak birkaç dakika sürer. Aşağıdaki örnek çıktıda, VM oluşturma başarılı olduğunu gösterir:

```azurecli
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

Kendi Not `publicIpAddress` VM'nizi çıktıda. Bu adres, sonraki adımlarda VM'ye erişmek için kullanacaksınız.

## <a name="assign-an-identity-to-the-vm"></a>Bir kimlik VM'ye atayın

Aşağıdaki komutu çalıştırarak sanal makine için sistem tarafından atanan bir kimlik oluşturun:

```azurecli-interactive
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Komut çıktısı aşağıdaki gibidir.

```azurecli
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

Not `systemAssignedIdentity`. Sonraki adım kullanırsınız.

## <a name="give-the-vm-identity-permission-to-key-vault"></a>Anahtar Kasası'na VM kimlik izin ver

Artık oluşturduğunuz kimliğine anahtar kasası izin verebilirsiniz. Şu komutu çalıştırın:

```azurecli-interactive
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-in-to-the-vm"></a>VM'de oturum açın

Bir terminal kullanarak sanal makineye oturum açın.

```terminal
ssh azureuser@<PublicIpAddress>
```

## <a name="install-python-library-on-the-vm"></a>VM'de Python Kitaplığı'nı yükleyin

İndirme ve yükleme [istekleri](https://pypi.org/project/requests/2.7.0/) HTTP GET çağrıları yapmak için Python kitaplığı.

## <a name="create-edit-and-run-the-sample-python-app"></a>Oluşturun, düzenleyin ve örnek Python uygulamasını çalıştırın

Adlı bir Python dosyası oluşturun **Sample.py**.

Sample.PY açın ve aşağıdaki kodu içerecek şekilde düzenleyin:

```python
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

Yukarıdaki kod, iki adımlı bir işlem gerçekleştirir:

   1. Bir belirteç VM'de yerel MSI uç noktasından getirir. Uç nokta, ardından Azure Active Directory'den bir belirteç getirir.
   1. Anahtar kasası için belirteç geçirir ve gizli anahtarı getirir.

Aşağıdaki komutu çalıştırın. Gizli değer görmeniz gerekir.

```console
python Sample.py
```

Bu öğreticide, bir Linux sanal makinesinde çalışan bir Python uygulaması ile Azure anahtar kasası kullanmayı öğrendiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında kaynak grubunu, sanal makine ve tüm ilgili kaynakları silin. Bunu yapmak için kaynak grubunu seçin ve sanal makine için seçin **Sil**.

Anahtar kasasını kullanarak silme `az keyvault delete` komutu:

```azurecli-interactive
az keyvault delete --name
                   [--resource-group]
                   [--subscription]
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure anahtar kasası REST API'si](https://docs.microsoft.com/rest/api/keyvault/)
