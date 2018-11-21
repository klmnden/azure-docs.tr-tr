---
title: Öğretici - Azure Key Vault ile Azure Linux sanal makinesine Python kullanma | Microsoft Docs
description: Öğretici Anahtar Kasasından gizli dizi okumak için bir ASP.NET Core uygulaması yapılandırma
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
ms.openlocfilehash: 863f3a8c18eab5b42b5f1377ca0f91f9740c06e7
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2018
ms.locfileid: "52267270"
---
# <a name="tutorial-how-to-use-azure-key-vault-with-azure-linux-virtual-machine-in-python"></a>Öğretici: Azure Key Vault ile Azure Linux sanal makinesine Python kullanmayı

Azure Key Vault uygulamalarınıza, hizmetlerinize ve BT kaynaklarınıza erişmek için gereken API Anahtarları, Veritabanı Bağlantısı dizeleri gibi gizli dizileri korumanıza yardımcı olur.

Bu öğreticide, bir Azure web uygulamasının Azure kaynakları için yönetilen kimlikleri kullanarak Azure Key Vault’tan bilgi okumasını sağlamak için gerekli adımları uygulayacaksınız. Bu öğreticide [Azure Web Apps](../app-service/app-service-web-overview.md) temel alınmıştır. Şunları yapmayı öğreneceksiniz:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Anahtar kasasına bir gizli dizi depolama.
> * Anahtar kasasından bir gizli dizi alma.
> * Bir Azure sanal makinesi oluşturun.
> * Etkinleştirme bir [yönetilen kimliği](../active-directory/managed-identities-azure-resources/overview.md) sanal makine için.
> * Anahtar kasasından verileri okumak konsol uygulaması için gerekli izinleri verin.
> * Key Vault'tan gizli dizilerini alma

İlerlemeden önce lütfen [temel kavramları](key-vault-whatis.md#basic-concepts) okuyun.

## <a name="prerequisites"></a>Önkoşullar
* Tüm platformlar:
  * Git ([indir](https://git-scm.com/downloads)).
  * Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.
  * [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) 2.0.4 veya sonraki sürümü. Bu uygulama Windows, Mac ve Linux’ta kullanılabilir.

Bu öğreticide, yönetilen hizmet kimliğini kullanma hale getirir.

## <a name="what-is-managed-service-identity-and-how-does-it-work"></a>Yönetilen Hizmet Kimliği nedir ve nasıl çalışır?
Daha fazla ilerlemeden önce, MSI'yi anlayalım. Azure Key Vault kimlik bilgilerini güvenle depolayabilir ve bunlar kodunuzda yer almaz, ama bunları almak için Azure Key Vault'ta kimliğinizi doğrulamanız gerekir. Key Vault'ta kimlik doğrulaması yapmak için kimlik bilgilerine ihtiyacınız vardır! Klasik bir önyükleme sorunu. Azure'un ve Azure AD'nin büyüsü sayesinde MSI işleri başlatmayı çok basitleştiren bir “önyükleme kimliği” sağlar.

Şöyle çalışır! Sanal Makineler, App Service veya İşlevler gibi bir Azure hizmeti için MSI'yi etkinleştirdiğinizde, Azure hizmetin örneği için Azure Active Directory'de bir [Hizmet Sorumlusu](key-vault-whatis.md#basic-concepts) oluşturur ve Hizmet Sorumlusunun kimlik bilgilerini hizmetin örneğine ekler. 

![MSI](media/MSI.png)

Ardından, kodunuz erişim belirtecini almak için Azure kaynağında sağlanan bir yerel meta veri hizmetini çağırır.
Kodunuz yerel MSI_ENDPOINT'ten aldığı erişim belirtecini kullanarak Azure Key Vault hizmetinde kimlik doğrulaması yapar. 

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

* Anahtar kasası adı: Adı 3-24 karakterden oluşan bir dize olmalı ve yalnızca (0-9, a-z, A-Z ve -) karakterlerini içermelidir.
* Kaynak grubu adı.
* Konum: **West US**.

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```
Bu noktada Azure hesabınız, bu yeni anahtar kasasında herhangi bir işlemi gerçekleştirmeye yetkili olan tek hesaptır.

## <a name="add-a-secret-to-the-key-vault"></a>Anahtar kasasına gizli dizi ekleme

Bunun nasıl çalıştığını göstermemize yardımcı olması için bir gizli dizi ekliyoruz. Güvenle korumanız ancak uygulamanıza da sağlamanız gereken bir SQL bağlantı dizesini veya başka bir bilgiyi depoluyor olabilirsiniz.

Anahtar kasasında **AppSecret** adlı bir gizli dizi oluşturmak için aşağıdaki komutları yazın. Bu gizli dizi **MySecret** değerini depolar.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

## <a name="create-a-virtual-machine"></a>Bir Sanal Makine Oluşturun
İzleyin aşağıdaki bağlantıları bir Windows sanal makine oluşturmak için

[Azure CLI](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-cli) 

[PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-powershell)

[Portal](https://docs.microsoft.com/azure/virtual-machines/windows/quick-create-portal)

## <a name="assign-identity-to-virtual-machine"></a>Sanal makineye kimlik atama
Bu adımda Azure CLI aşağıdaki komutu çalıştırarak sanal makineye atanan kimliği bir sistem oluşturuyoruz

```
az vm identity assign --name <NameOfYourVirtualMachine> --resource-group <YourResourceGroupName>
```

Lütfen aşağıda gösterilen systemAssignedIdentity unutmayın. Yukarıdaki komut çıktısı olacaktır 

```
{
  "systemAssignedIdentity": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
  "userAssignedIdentities": {}
}
```

## <a name="give-virtual-machine-identity-permission-to-key-vault"></a>Anahtar Kasası'na sanal makine kimliğini izin ver
Şimdi biz yukarıdaki Key Vault kimlik izni aşağıdaki komutu çalıştırarak oluşturduğunuz verebilirsiniz

```
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <VMSystemAssignedIdentity> --secret-permissions get list
```

## <a name="login-to-the-virtual-machine"></a>Sanal makineye oturum açma

Bu takip edebilirsiniz [Öğreticisi](https://docs.microsoft.com/azure/virtual-machines/windows/connect-logon)

## <a name="create-and-run-sample-python-app"></a>Oluşturma ve örnek Python uygulamasını çalıştırma

Aşağıda yalnızca bir örnek dosya "Sample.py" olarak adlandırılır. Kullandığı [istekleri](http://docs.python-requests.org/master/) HTTP GET çağrıları yapmak için kitaplık.

## <a name="edit-samplepy"></a>Sample.PY Düzenle
Dosya ve kopyalama Sample.py açık oluşturduktan sonra aşağıdaki kodu

```
The below is a 2 step process. 
1. Fetch a token from the local MSI endpoint on the VM which in turn fetches a token from Azure Active Directory
2. Pass the token to Key Vault and fetch your secret 
```
    # importing the requests library 
    import requests 

    # Step 1: Fetch an access token from a Managed Identity enabled azure resource      
    # Note that the resource here is https://vault.azure.net for public cloud and api-version is 2018-02-01
    MSI_ENDPOINT = "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net"
    r = requests.get(MSI_ENDPOINT, headers = {"Metadata" : "true"}) 
      
    # extracting data in json format 
    # This request gets a access_token from Azure Active Directory using the local MSI endpoint
    data = r.json() 
    
    # Step 2: Pass the access_token received from previous HTTP GET call to Key Vault
    KeyVaultURL = "https://prashanthwinvmvault.vault.azure.net/secrets/RandomSecret?api-version=2016-10-01"
    kvSecret = requests.get(url = KeyVaultURL, headers = {"Authorization": "Bearer " + data["access_token"]})
    
    print(kvSecret.json()["value"])
```

By running you should see the secret value 
```
Python Sample.py
```

The above code shows you how to do operations with Azure Key Vault in an Azure Windows Virtual Machine. 

## Next steps

> [!div class="nextstepaction"]
> [Azure Key Vault REST API](https://docs.microsoft.com/rest/api/keyvault/)
