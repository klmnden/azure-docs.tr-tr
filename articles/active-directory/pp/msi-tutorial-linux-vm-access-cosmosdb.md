---
title: "Bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI), Azure Cosmos DB erişmek için bir Linux VM üzerinde kullanın."
description: "Azure Cosmos DB erişmek için bir Linux VM üzerinde bir User-Assigned yönetilen hizmet kimliği (MSI) kullanarak sürecinde anlatan öğretici."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/14/2018
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: dbb5e9e8f9accd618599010ab2bbb4a8760e534f
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/16/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-cosmos-db"></a>Bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI), Azure Cosmos DB erişmek için bir Linux VM üzerinde kullanın. 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğretici oluşturup bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) bir Linux sanal makinenin kullanın, ardından Azure Cosmos DB erişmek için kullandığınız gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yönetilen hizmet kimliği (MSI) atanmış bir kullanıcı oluşturun
> * Linux sanal makine için kullanıcı tarafından atanan MSI atayın
> * Azure Cosmos DB örneğine MSI erişim
> * Kullanıcı tarafından atanan MSI kimliğini kullanarak bir erişim belirteci alın ve Azure Cosmos DB erişmek için kullanın

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Bu öğreticide CLI komut dosyası örnekleri çalıştırmak için iki seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](~/articles/cloud-shell/overview.md) Azure portalından veya aracılığıyla **deneyin** her kod bloğunun sağ üst köşesinde bulunan düğmesini.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Oturum açmak için Azure portalında [ https://portal.azure.com ](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için yeni bir Linux VM oluşturun. Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1. Tıklatın **+ kaynak oluşturma** Azure portalında sol üst köşesinde bulundu.
2. Seçin **işlem**ve ardından **Ubuntu Server 16.04 LTS VM**.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

    ![MSI Linux VM](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makine açılır.
5. Altında **kaynak grubu**, seçin **Yeni Oluştur** ve bu VM için kaynak grubunun adını yazın. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyutları görmek için seçin **tüm görüntüle** veya desteklenen disk türü filtresini değiştirin. İçinde **ayarları** dikey penceresinde, Varsayılanları tutun ve tıklatın **Tamam**.

## <a name="create-a-user-assigned-msi"></a>Kullanıcı tarafından atanan bir MSI oluşturma

1. CLI konsol (yerine bir Azure bulut kabuk oturumu) kullanıyorsanız, Azure'da oturum açma tarafından başlatın. Altında yeni MSI oluşturmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

    ```azurecli
    az login
    ```

2. Bir kullanıcı tarafından atanan MSI kullanarak oluşturduğunuz [az kimliği oluşturma](/cli/azure/identity#az_identity_create). `-g` Parametresi, burada MSI oluşturulur, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizi ile:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Kullanıcı tarafından atanan MSI oluşturulan, aşağıdaki örneğe benzer için yanıt ayrıntılarını içerir (Not `clientId` ve `id` daha sonraki adımlarda kullanıldıkları gibi bir MSI için değerleri):

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "c0833082-6cc3-4a26-a1b1-c4b5f90a981f",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Kullanıcı tarafından atanan MSI Linux VM'NİZDE atayın

Sistem tarafından atanan bir MSI, bir kullanıcı tarafından atanan MSI birden çok Azure kaynaklarına istemciler tarafından kullanılabilir. Bu öğretici için bunu tek bir VM öğesine atayın. Ayrıca birden fazla VM atayabilirsiniz.

Kullanıcı tarafından atanan MSI kullanarak, Linux VM atamak [az vm Ata-identity](/cli/azure/vm#az_vm_assign_identity). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. Kullanım `id` özelliği döndürülen için önceki adımda `--identities` parametre değeri:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma 

Zaten yoksa, şimdi Cosmos DB hesabı oluşturun. Ayrıca, bu adımı atlayın ve tercih ederseniz, varolan bir Cosmos DB hesabı kullanabilirsiniz. 

1. Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. Tıklatın **veritabanları**, ardından **Azure Cosmos DB**ve yeni bir "yeni hesabı" panel görüntüler.
3. Girin bir **kimliği** daha sonra kullanmak Cosmos DB hesabı.  
4. **API** "SQL" olarak ayarlanmalıdır. Bu öğreticide açıklanan yaklaşım diğer kullanılabilir API türleriyle kullanılabilir, ancak bu öğreticideki adımlar SQL API'yi yer almaktadır.
5. Olun **abonelik** ve **kaynak grubu** VM'nizi oluşturduğunuzda önceki adımda belirttiğiniz olanlarla eşleşmesi.  Seçin bir **konumu** Cosmos DB burada kullanılabilir.
6. **Oluştur**’a tıklayın.

## <a name="create-a-collection-in-the-cosmos-db-account"></a>Cosmos DB hesabında bir koleksiyon oluşturma

Ardından, sonraki adımlarda sorgulayabilirsiniz Cosmos DB hesabındaki bir veri toplama ekleyin.

1. Yeni oluşturulan Cosmos DB hesabınıza gidin.
2. Üzerinde **genel bakış** sekmesini tıklatın **+/ topluluk Ekle** düğmesi ve bir "çıkışı slayt panel topluluk Ekle".
3. Koleksiyon veritabanı kimliği, koleksiyon kimliği, select depolama kapasitesi sağlar, bölüm anahtarı girin, verimlilik değeri girin ve ardından **Tamam**.  Bu öğretici için "Test" koleksiyon kimliği ve veritabanı kimliği seçin olarak sabit depolama kapasitesi ve en düşük işlemesi (400 RU/s) kullanmak yeterli olur.

## <a name="grant-your-user-assigned-msi-access-to-the-cosmos-db-account-access-keys"></a>Cosmos DB hesap erişim tuşları, kullanıcı atanan MSI erişim

Cosmos DB yerel olarak Azure AD kimlik doğrulamasını desteklemez.  Ancak, bir MSI Kaynak Yöneticisi'nden Cosmos DB erişim tuşu almanızı sonra Cosmos DB erişmek için anahtarı kullanın.  Bu adımda, kullanıcı MSI erişim anahtarları Cosmos DB hesabına atanan verin.

İlk Azure kaynağı Azure CLI kullanarak Yöneticisi'nde Cosmos DB hesabına MSI kimlik erişim verin. İçin değerleri güncelleştirmek `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<COSMOS DB ACCOUNT NAME>` ortamınız için. Değiştir `<MSI PRINCIPALID>` ile `principalId` özellik tarafından döndürülen `az identity create` komutunu [kullanıcı tarafından atanan bir MSI oluşturma](#create-a-user-assigned-msi).  Cosmos DB erişim tuşlarını kullanırken iki düzeyde ayrıntı düzeyi destekler: okuma/yazma erişimi hesabı ve salt okunur erişim hesabı.  Ata `DocumentDB Account Contributor` hesabının okuma/yazma anahtarlarını almak ya da atamak istiyorsanız, rol `Cosmos DB Account Reader Role` hesabı için salt okunur anahtarlarını almak istiyorsanız, rol:

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

## <a name="get-an-access-token-using-the-user-assigned-msi-and-use-it-to-call-azure-resource-manager"></a>Atanan kullanıcı MSI kullanarak bir erişim belirteci alın ve Azure Resource Manager çağırmak için kullanın

Öğretici kalanı için daha önce oluşturduğunuz sanal makineden çalışır.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/install_guide). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](../../virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında gidin **sanal makineleri**gidin, Linux sanal makine, daha sonra **genel bakış** sayfasında **Bağlan** üstünde. VM'nize bağlanmak için dizesini kopyalayın. 
2. VM'nize SSH istemcisini kullanarak bağlanın.  
3. Ardından, girin istenir, **parola** oluştururken eklenen **Linux VM**. Ardından başarıyla oturum açmanız.  
4. Azure Resource Manager için bir erişim belirteci almak üzere CURL kullanın.  

    CURL istek ve yanıt erişim belirteci için aşağıda verilmiştir.  Değiştir <CLIENT ID> ile ClientID, kullanıcı değeri MSI atanan: 
    
    ```bash
    curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com/&client_id=<MSI CLIENT ID>" 
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

Artık Kaynak Yöneticisi'ni önceki bölümde biz alınan erişim belirteci kullanarak çağırmak için CURL kullanın Cosmos DB hesap erişim anahtarı alınamadı. Biz erişim tuşu sahip olduğunda, biz Cosmos DB sorgulayabilirsiniz. Değiştirdiğinizden emin olun `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<COSMOS DB ACCOUNT NAME>` parametre değerlerini kendi değerlere sahip. Değiştir `<ACCESS TOKEN>` daha önce erişim belirteci ile değer.  Okuma/yazma anahtarları almak istiyorsanız, anahtar işlem türü kullanın `listKeys`.  Salt okunur anahtarları almak istiyorsanız, anahtar işlem türü kullanın `readonlykeys`:

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

- MSI genel bakış için bkz: [yönetilen hizmet kimliği (MSI) Azure kaynakları için](msi-overview.md).

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
