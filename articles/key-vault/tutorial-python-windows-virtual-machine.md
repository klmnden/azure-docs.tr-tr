---
title: Öğretici - Python bir Windows sanal makinesi ile Azure anahtar kasası | Microsoft Docs
description: Bu öğreticide, anahtar kasasından gizli dizi okumak için bir ASP.NET core uygulaması yapılandırın.
services: key-vault
author: msmbaldwin
manager: rajvijan
ms.service: key-vault
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 985380fd24e0db697f9dc9b1c5b2e5b8af2e6cbf
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228095"
---
# <a name="tutorial-use-azure-key-vault-with-a-windows-virtual-machine-in-python"></a>Öğretici: Bir Windows sanal makinesi Python ile Azure anahtar kasası

Azure Key Vault API anahtarları, uygulamalar, hizmetlerinizi ve BT kaynaklarına erişmek için gereken veritabanı bağlantı dizeleri gibi gizli dizileri korumanıza yardımcı olur.

Bu öğreticide, Azure Key Vault'tan bilgileri okumak için bir konsol uygulaması alma konusunda bilgi edinin. Bunu yapmak için Azure kaynakları için yönetilen bir kimlik kullanın. 

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Gizli anahtar Kasası'na ekleyin.
> * Anahtar kasasından bir gizli dizi alma.
> * Bir Azure sanal makinesi oluşturun.
> * Yönetilen bir kimlik etkinleştirin.
> * VM kimliği için izinleri atayın.

Başlamadan önce okumanız [Key Vault temel kavramlarını](key-vault-whatis.md#basic-concepts). 

Azure aboneliğiniz yoksa, oluşturun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Önkoşullar

Windows, Mac ve Linux için:
  * [Git](https://git-scm.com/downloads)
  * Bu öğretici, Azure CLI'yi yerel olarak çalıştırmanızı gerektirir. Sonraki bir sürümünün yüklü veya Azure CLI 2.0.4 sürüm olmalıdır. Sürümü bulmak için `az --version` komutunu çalıştırın. CLI’yı yüklemeniz veya yükseltmeniz gerekiyorsa bkz. [Azure CLI 2.0’ı yükleme](https://review.docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="about-managed-service-identity"></a>Yönetilen Hizmet Kimliği hakkında

Kodunuzda görüntülenmez için azure Key Vault kimlik bilgilerini güvenli bir şekilde, depolar. Ancak, Azure Key Vault'a anahtarlarınızı almak için kimlik doğrulaması gerekir. Anahtar Kasası'na kimlik doğrulaması için bir kimlik bilgisi gerekir. Bu, klasik bir önyükleme ikilemle olur. Yönetilen hizmet kimliği (MSI) sağlayarak bu sorunu çözer bir _bootstrap kimlik_ , bu süreci kolaylaştırır.

Azure sanal makineler, Azure App Service veya Azure işlevleri gibi bir Azure hizmeti için MSI'yı etkinleştirdiğinizde, Azure oluşturur bir [hizmet sorumlusu](key-vault-whatis.md#basic-concepts). MSI, Azure Active Directory'de (Azure AD) hizmet örneği için bunu yapar ve bu örneğe hizmet sorumlusu kimlik bilgileri ekler. 

![MSI](media/MSI.png)

Ardından, bir erişim belirteci almak için kodunuzu Azure kaynak üzerinde kullanılabilir olan bir yerel meta veri hizmeti çağırır. Bir Azure anahtar kasası hizmetinde kimlik doğrulaması için kodunuzu yerel MSI uç noktasından alır ve bir erişim belirteci kullanır. 

## <a name="log-in-to-azure"></a>Azure'da oturum açma

Azure CLI'yi kullanarak Azure'da oturum açmak için, şunları girin:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturun

Azure kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal kapsayıcıdır.

[az group create](/cli/azure/group#az-group-create) komutunu kullanarak bir kaynak grubu oluşturun. 

Kaynak grubu adı seçin ve yer tutucuyu doldurun. Aşağıdaki örnekte Batı ABD konumunda bir kaynak grubu oluşturulur:

```azurecli
# To list locations: az account list-locations --output table
az group create --name "<YourResourceGroupName>" --location "West US"
```

Bu öğretici boyunca yeni oluşturulan kaynak grubunuzun kullanırsınız.

## <a name="create-a-key-vault"></a>Bir anahtar kasası oluşturma

Önceki adımda oluşturduğunuz kaynak grubunda bir anahtar kasası oluşturmak için aşağıdaki bilgileri sağlayın:

* Anahtar kasası adı: bir dizenin yalnızca rakam (0-9) içerebilir ve 3-24 karakter harf (a-z, A-Z) ve tire (-)
* Kaynak grubu adı
* Konum: **Batı ABD**

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Bu noktada, Azure hesabınızı kullanarak bu yeni bir anahtar kasası işlemleri gerçekleştirmek için yetkili tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Gizli dizi, bir SQL bağlantı dizesi veya güvenli ve uygulamanız için kullanılabilir durumda tutmak için gereken herhangi bir bilgi olabilir.

Anahtar Kasası'nda bir gizli dizi oluşturmak için çağrılan **AppSecret**, aşağıdaki komutu girin:

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Bu gizli dizinin değerini depolar **ettiyseniz**.

## <a name="create-a-virtual-machine"></a>Bir sanal makine oluştur
Aşağıdaki yöntemlerden birini kullanarak bir sanal makine oluşturabilirsiniz:

* [Azure CLI](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli)
* [PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)
* [Azure portalı](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-an-identity-to-the-vm"></a>Bir kimlik VM'ye atayın
Bu adımda, Azure CLI içinde aşağıdaki komutu çalıştırarak sanal makine için sistem tarafından atanan bir kimlik oluşturun:

```azurecli
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Aşağıdaki kodda gösterilen sistem tarafından atanan kimliğini not edin. Yukarıdaki komut çıktısı şu şekilde olur: 

```azurecli
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="assign-permissions-to-the-vm-identity"></a>VM kimliği için izinler atama
Artık aşağıdaki komutu çalıştırarak anahtar kasanız için önceden oluşturulmuş kimlik izinler atayabilirsiniz:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="log-on-to-the-virtual-machine"></a>Sanal makinede oturum açma

Sanal makineye oturum açmak için yönergeleri izleyin. [bağlanma ve Windows çalıştıran bir Azure sanal makinede oturum açma](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon).

## <a name="create-and-run-a-sample-python-app"></a>Oluşturma ve örnek Python uygulamasını çalıştırma

Adlı bir örnek dosya sonraki bölümüdür *Sample.py*. Bunu kullanan bir [istekleri](http://docs.python-requests.org/en/master/) HTTP GET çağrıları yapmak için kitaplık.

## <a name="edit-samplepy"></a>Sample.PY Düzenle

Oluşturduktan sonra *Sample.py*dosyasını açın ve sonra bu bölümde kodu kopyalayın. 

Kod, iki adımlı bir işlem sunar:
1. Bir belirteç VM'de yerel MSI uç noktasından getirin.  
  Bunun yapılması ayrıca Azure AD'den bir belirtecini getirir.
1. Anahtar kasanızı belirtecin geçip ve sonra gizli anahtarı getirilemedi. 

```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource.      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets an access_token from Azure AD by using the local MSI endpoint.
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to your key vault.
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

Aşağıdaki kodu çalıştırarak gizli değer görüntüleyebilirsiniz: 

```
python Sample.py
```

Yukarıdaki kod, bir Windows sanal makinesinde Azure Key Vault ile işlemleri yapıp gösterilir. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında sanal makine ve anahtar kasanıza silin.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure anahtar kasası REST API'si](https://docs.microsoft.com/rest/api/keyvault/)
