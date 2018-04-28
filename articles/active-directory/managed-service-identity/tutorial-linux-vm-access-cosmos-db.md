---
title: Azure Cosmos DB erişmek için bir Linux VM MSI kullanın
description: Azure Cosmos DB erişmek için bir Linux VM üzerinde bir System-Assigned yönetilen hizmet kimliği (MSI) kullanarak sürecinde anlatan öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: skwan
ms.openlocfilehash: 692bc5eb401ccda36ef42006de509144170f7757
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="use-a-linux-vm-msi-to-access-azure-cosmos-db"></a>Azure Cosmos DB erişmek için bir Linux VM MSI kullanın 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


Bu öğretici oluşturma ve bir Linux VM MSI kullanma gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Etkin MSI ile bir Linux VM oluşturma
> * Cosmos DB hesabı oluşturma
> * Cosmos DB hesabında bir koleksiyon oluşturma
> * Azure Cosmos DB örneğine MSI erişim
> * Alma `principalID` , Linux VM msi
> * Erişim belirteci almak ve Azure Resource Manager çağırmak için kullanın
> * Cosmos DB çağrı yapmak için Azure Resource Manager gelen erişim anahtarı alma

## <a name="prerequisites"></a>Önkoşullar

Bir Azure hesabınız yoksa [ücretsiz bir hesap için kaydolun](https://azure.microsoft.com) devam etmeden önce.

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Bu öğreticide CLI komut dosyası örnekleri çalıştırmak için iki seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](~/articles/cloud-shell/overview.md) Azure portalından veya aracılığıyla **deneyin** her kod bloğunun sağ üst köşesinde bulunan düğmesini.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için yeni bir oluşturma MSI Linux VM etkin.

MSI etkin bir VM oluşturmak için:

1. Yerel bir konsolda Azure CLI kullanıyorsanız, ilk kez Azure kullanarak oturum [az oturum açma](/cli/azure/reference-index#az_login). Altında VM dağıtmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

   ```azurecli-interactive
   az login
   ```

2. Oluşturma bir [kaynak grubu](../../azure-resource-manager/resource-group-overview.md#terminology) kapsama ve VM'nizi ve kullanarak kaynaklarıyla ilgili dağıtımı için [az grubu oluşturma](/cli/azure/group/#az_group_create). Bunun yerine kullanmak istediğiniz kaynak grubu zaten varsa bu adımı atlayabilirsiniz:

   ```azurecli-interactive 
   az group create --name myResourceGroup --location westus
   ```

3. Kullanarak bir VM oluşturun [az vm oluşturma](/cli/azure/vm/#az_vm_create). Aşağıdaki örnek, adlandırılmış bir VM'nin oluşturur *myVM* tarafından istendiği gibi bir MSI ile `--assign-identity` parametresi. `--admin-username` Ve `--admin-password` parametreleri sanal makine oturum açma için yönetici kullanıcı adı ve parola hesabı belirtin. Bu değerleri, ortamınız için uygun şekilde güncelleştirin: 

   ```azurecli-interactive 
   az vm create --resource-group myResourceGroup --name myVM --image win2016datacenter --generate-ssh-keys --assign-identity --admin-username azureuser --admin-password myPassword12

## Create a Cosmos DB account 

If you don't already have one, create a Cosmos DB account. You can skip this step and use an existing Cosmos DB account. 

1. Click the **+/Create new service** button found on the upper left-hand corner of the Azure portal.
2. Click **Databases**, then **Azure Cosmos DB**, and a new "New account" panel  displays.
3. Enter an **ID** for the Cosmos DB account, which you use later.  
4. **API** should be set to "SQL." The approach described in this tutorial can be used with the other available API types, but the steps in this tutorial are for the SQL API.
5. Ensure the **Subscription** and **Resource Group** match the ones you specified when you created your VM in the previous step.  Select a **Location** where Cosmos DB is available.
6. Click **Create**.

## Create a collection in the Cosmos DB account

Next, add a data collection in the Cosmos DB account that you can query in later steps.

1. Navigate to your newly created Cosmos DB account.
2. On the **Overview** tab click the **+/Add Collection** button, and an "Add Collection" panel slides out.
3. Give the collection a database ID, collection ID, select a storage capacity, enter a partition key, enter a throughput value, then click **OK**.  For this tutorial, it is sufficient to use "Test" as the database ID and collection ID, select a fixed storage capacity and lowest throughput (400 RU/s).  

## Retrieve the `principalID` of the Linux VM's MSI

To gain access to the Cosmos DB account access keys from the Resource Manager in the following section, you need to retrieve the `principalID` of the Linux VM's MSI.  Be sure to replace the `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` (resource group in which you VM resides), and `<VM NAME>` parameter values with your own values.

```azurecli-interactive
az resource show --id /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe> --api-version 2017-12-01
```
Yanıt sistem atanan MSI (sonraki bölümde Principalıd olarak kullanılan unutmayın) ayrıntılarını içerir:

```bash  
{
    "id": "/subscriptions/<SUBSCRIPTION ID>/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe>",
  "identity": {
    "principalId": "6891c322-314a-4e85-b129-52cf2daf47bd",
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533f8",
    "type": "SystemAssigned"
 }

```
## <a name="grant-your-linux-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Cosmos DB hesap erişim anahtarı, Linux VM MSI erişim

Cosmos DB yerel olarak Azure AD kimlik doğrulamasını desteklemez. Ancak, bir MSI Kaynak Yöneticisi'nden Cosmos DB erişim tuşu almanızı sonra Cosmos DB erişmek için anahtarı kullanın. Bu adımda, Cosmos DB hesabı anahtarları, MSI erişim izni.

Cosmos DB hesabı Azure kaynağı Azure CLI kullanarak Yöneticisi'nde MSI kimlik erişim vermek için değerleri güncelleştirmek `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<COSMOS DB ACCOUNT NAME>` ortamınız için. Değiştir `<MSI PRINCIPALID>` ile `principalId` özellik tarafından döndürülen `az resource show` komutunu [Linux VM MSI Principalıd almak](#retrieve-the-principalID-of-the-linux-VM's-MSI).  Cosmos DB erişim tuşlarını kullanırken iki düzeyde ayrıntı düzeyi destekler: okuma/yazma erişimi hesabı ve salt okunur erişim hesabı.  Ata `DocumentDB Account Contributor` hesabının okuma/yazma anahtarlarını almak ya da atamak istiyorsanız, rol `Cosmos DB Account Reader Role` hesabı için salt okunur anahtarlarını almak istiyorsanız, rol:

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

Yanıt oluşturulan rol ataması ayrıntılarını içerir:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>/providers/Microsoft.Authorization/roleAssignments/5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "name": "5b44e628-394e-4e7b-bbc3-d6cd4f28f15b",
  "properties": {
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/fbdf93bf-df7d-467e-a4d2-9458aa1360c8",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-linux-vms-msi-and-use-it-to-call-azure-resource-manager"></a>Linux VM MSI kullanarak bir erişim belirteci alın ve Azure Resource Manager çağırmak için kullanın

Öğretici kalanı için daha önce oluşturduğunuz sanal makineden çalışır.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/install_guide). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](../../virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında gidin **sanal makineleri**gidin, Linux sanal makine, daha sonra **genel bakış** sayfasında **Bağlan** üstünde. VM'nize bağlanmak için dizesini kopyalayın. 
2. VM'nize SSH istemcisini kullanarak bağlanın.  
3. Ardından, girin istenir, **parola** oluştururken eklenen **Linux VM**. Ardından başarıyla oturum açmanız.  
4. Azure Resource Manager için bir erişim belirteci almak üzere CURL kullanın: 
     
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
 
    > [!NOTE]
    > Önceki istek, "kaynak" parametresinin değeri Azure AD tarafından beklenen bir tam eşleşme olmalıdır. Azure Resource Manager kaynak kimliği'ni kullanırken eğik URI üzerinde eklemeniz gerekir.
    > Aşağıdaki yanıtı, okumanızdır kısaltılmış olarak access_token öğesi.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Cosmos DB çağrı yapmak için Azure Resource Manager gelen erişim anahtarı alma  

Şimdi CURL Kaynak Yöneticisi'ni Cosmos DB hesap erişim anahtarı almak için önceki bölümde alınan erişim belirteci kullanarak çağırmak için kullanın. Biz erişim tuşu sahip olduğunda, biz Cosmos DB sorgulayabilirsiniz. Değiştirdiğinizden emin olun `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<COSMOS DB ACCOUNT NAME>` parametre değerlerini kendi değerlere sahip. Değiştir `<ACCESS TOKEN>` daha önce erişim belirteci ile değer.  Okuma/yazma anahtarları almak istiyorsanız, anahtar işlem türü kullanın `listKeys`.  Salt okunur anahtarları almak istiyorsanız, anahtar işlem türü kullanın `readonlykeys`:

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> Önceki URL metindeki büyük/küçük harfe duyarlıdır, buna göre yansıtacak şekilde büyük-küçük, kaynak grupları için kullanıyorsanız, bu nedenle olun. Ayrıca, bu bir POST isteği GET isteği olduğunu biliyor ve uzunluk sınırı - NULL olabilir d ile yakalamak için bir değer geçirmek sağlamak önemlidir.  

CURL yanıt anahtarları listesini verir.  Örneğin, salt okunur anahtarları alırsanız:  

```bash 
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```

Cosmos DB hesabı için erişim anahtarı sahip olduğunuza göre geçirmek için Cosmos DB SDK ve hesabınıza erişmeniz için çağrıları yapma.  Hızlı bir örnek için Azure CLI için erişim anahtarı geçirebilirsiniz.  Alma <COSMOS DB CONNECTION URL> gelen **genel bakış** Cosmos DB hesabı dikey Azure portalında sekmesinde.  Değiştir <ACCESS KEY> yukarıda elde ettiğiniz değere sahip:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Bu CLI komut toplama ile ilgili ayrıntıları verir:

```bash
{
  "collection": {
    "_conflicts": "conflicts/",
    "_docs": "docs/",
    "_etag": "\"00006700-0000-0000-0000-5a8271e90000\"",
    "_rid": "Es5SAM2FDwA=",
    "_self": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/",
    "_sprocs": "sprocs/",
    "_triggers": "triggers/",
    "_ts": 1518498281,
    "_udfs": "udfs/",
    "id": "Test",
    "indexingPolicy": {
      "automatic": true,
      "excludedPaths": [],
      "includedPaths": [
        {
          "indexes": [
            {
              "dataType": "Number",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "String",
              "kind": "Range",
              "precision": -1
            },
            {
              "dataType": "Point",
              "kind": "Spatial"
            }
          ],
          "path": "/*"
        }
      ],
      "indexingMode": "consistent"
    }
  },
  "offer": {
    "_etag": "\"00006800-0000-0000-0000-5a8271ea0000\"",
    "_rid": "f4V+",
    "_self": "offers/f4V+/",
    "_ts": 1518498282,
    "content": {
      "offerIsRUPerMinuteThroughputEnabled": false,
      "offerThroughput": 400
    },
    "id": "f4V+",
    "offerResourceId": "Es5SAM2FDwA=",
    "offerType": "Invalid",
    "offerVersion": "V2",
    "resource": "dbs/Es5SAA==/colls/Es5SAM2FDwA=/"
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- MSI genel bakış için bkz: [yönetilen hizmet kimliği (MSI) Azure kaynakları için](overview.md).

