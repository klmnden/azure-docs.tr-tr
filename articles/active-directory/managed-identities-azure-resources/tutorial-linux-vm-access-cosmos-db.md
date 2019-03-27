---
title: Azure Cosmos DB'ye erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma
description: Linux VM üzerinde bir sistem tarafından atanmış yönetilen kimlik kullanarak Azure Cosmos DB'ye erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6b7e778e04901e830cdbc463d889621565c175a0
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2019
ms.locfileid: "58448157"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-managed-identity-to-access-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB'ye erişmek için Linux VM sistem tarafından atanan yönetilen kimliği kullanma 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


Bu öğreticide Azure Cosmos DB'ye erişmek amacıyla bir Linux sanal makinesi (VM) için sistem tarafından atanmış yönetilen bir kimliği nasıl kullanacağınız gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Cosmos DB hesabı oluşturma
> * Cosmos DB hesabında koleksiyon oluşturma
> * Azure Cosmos DB örneğine sistem tarafından atanan yönetilen kimlik erişimi verme
> * Linux VM sistem tarafından atanan yönetilen kimliğinin `principalID` değerini alma
> * Erişim belirteci alma ve bunu kullanarak Azure Resource Manager çağrısı yapma
> * Cosmos DB çağrıları yapmak için Azure Resource Manager'dan erişim anahtarları alma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

Bu öğreticideki CLI betiği örneklerini çalıştırmak için iki seçeneğiniz vardır:

- Azure portaldan veya her kod bloğunun sağ üst köşesinde yer alan **Deneyin** düğmesi aracılığıyla [Azure Cloud Shell](~/articles/cloud-shell/overview.md)'i kullanın.
- Yerel bir CLI konsolu kullanmayı tercih ediyorsanız [en son CLI 2.0 sürümünü yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya üstü).

## <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma 

Henüz Cosmos DB hesabınız yoksa, bir hesap oluşturun. Bu adımı atlayabilir ve varolan bir Cosmos DB hesabını kullanabilirsiniz. 

1. Azure portalının sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2. **Veritabanları**'na ve sonra da **Azure Cosmos DB**'ye tıklayın; yeni bir "Yeni hesap" paneli görüntülenir.
3. Cosmos DB hesabı için daha sonra kullanacağınız bir **Kimlik** girin.  
4. **API** olarak "SQL" ayarlanmalıdır. Bu öğreticide açıklanan yaklaşım varolan diğer API türleriyle kullanılabilir, ama bu öğreticideki adımlar SQL API'ye yöneliktir.
5. **Abonelik** ve **Kaynak Grubu** değerlerinin, önceki adımda VM'nizi oluştururken belirttiklerinizle eşleştiğinden emin olun.  Cosmos DB'nin kullanılabileceği **Konum**'u seçin.
6. **Oluştur**’a tıklayın.

## <a name="create-a-collection-in-the-cosmos-db-account"></a>Cosmos DB hesabında koleksiyon oluşturma

Ardından, Cosmos DB hesabına sonraki adımlarda sorgulayabileceğiniz bir veri koleksiyonu ekleyin.

1. Yeni oluşturduğunuz Cosmos DB hesabınıza gidin.
2. **Genel Bakış** sekmesinde **+/Koleksiyon Ekle** düğmesine tıklayın; "Koleksiyon Ekle" paneli belirir.
3. Koleksiyona bir veritabanı kimliği ve koleksiyon kimliği girin, depolama kapasitesini seçin, bölüm anahtarı girin, aktarım hızı değeri girin ve sonra da **Tamam**'a tıklayın.  Bu öğretici için, veritabanı kimliği ve koleksiyon kimliği olarak "Test" kullanmak, sabit bir depolama kapasitesi ve en düşük aktarım hızı (400 RU/s) seçmek yeterlidir.  

## <a name="retrieve-the-principalid-of-the-linux-vms-system-assigned-managed-identity"></a>Linux VM sistem tarafından atanan yönetilen kimliğinin `principalID` değerini alma

Aşağıdaki bölümde Kaynak Yöneticisi'nden Cosmos DB hesabı erişim anahtarlarına erişim elde etmek için, Linux VM sistem tarafından atanan yönetilen kimliğinin `principalID` değerini almanız gerekir.  `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` (VM'nizin içinde bulunduğu kaynak grubu) ve `<VM NAME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın.

```azurecli-interactive
az resource show --id /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe> --api-version 2017-12-01
```
Yanıt, sistem tarafından atanan yönetilen kimliğin ayrıntılarını içerir (principalID değerini not alın çünkü sonraki bölümde kullanılacaktır):

```bash  
{
    "id": "/subscriptions/<SUBSCRIPTION ID>/<RESOURCE GROUP>/providers/Microsoft.Compute/virtualMachines/<VM NAMe>",
  "identity": {
    "principalId": "6891c322-314a-4e85-b129-52cf2daf47bd",
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533f8",
    "type": "SystemAssigned"
 }

```
## <a name="grant-your-linux-vms-system-assigned-identity-access-to-the-cosmos-db-account-access-keys"></a>Linux VM sistem tarafından atanan yönetilen kimliğine Cosmos DB hesabı erişim anahtarları için erişim verme

Cosmos DB Azure AD kimlik doğrulamayı yerel olarak desteklemez. Bununla birlikte, Kaynak Yöneticisi'nden Cosmos DB erişim anahtarını almak için bir yönetilen kimliği kullanabilir ve ardından anahtarı kullanarak Cosmos DB'ye erişebilirsiniz. Bu adımda, sistem tarafından atanan yönetilen kimliğinize Cosmos DB hesabının anahtarları için erişim verirsiniz.

Azure CLI kullanarak Azure Resource Manager'da Cosmos DB hesabına sistem tarafından atanan yönetilen kimliği erişimi vermek için, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` ve `<COSMOS DB ACCOUNT NAME>` değerlerini ortamınıza uygun olarak güncelleştirin. Değiştirin `<MI PRINCIPALID>` ile `principalId` özelliği tarafından döndürülen `az resource show` almak Linux sanal makineleri mı Principalıd komutu.  Cosmos DB, erişim anahtarları kullanılırken iki ayrıntı düzeyini destekler: hesaba okuma/yazma erişimi ve hesaba salt okuma erişimi.  Hesap için okuma/yazma anahtarları almak istiyorsanız `DocumentDB Account Contributor` rolünü veya hesap için salt okuma anahtarları almak istiyorsanız `Cosmos DB Account Reader Role` rolünü atayın:

```azurecli-interactive
az role assignment create --assignee <MI PRINCIPALID> --role '<ROLE NAME>' --scope "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMODS DB ACCOUNT NAME>"
```

Yanıt, oluşturulan rol atamasının ayrıntılarını içerir:

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

## <a name="get-an-access-token-using-the-linux-vms-system-assigned-managed-identity-and-use-it-to-call-azure-resource-manager"></a>Linux VM’nin sistem tarafından atanan yönetilen kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğunuz VM'den çalışın.

Bu adımları tamamlamak bir SSH istemciniz olmalıdır. Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/install_guide)'ndeki SSH istemcisini kullanabilirsiniz. SSH istemcinizin anahtarlarını yapılandırmak için yardıma ihtiyacınız olursa, bkz. [Azure'da Windows ile SSH anahtarlarını kullanma](../../virtual-machines/linux/ssh-from-windows.md) veya [Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında **Sanal Makineler**'e gidin, Linux sanal makinenize gidin ve ardından **Genel Bakış** sayfasında üst kısımdaki **Bağlan**'a tıklayın. VM'nize bağlanma dizesini kopyalayın. 
2. SSH istemcinizi kullanarak VM'nize bağlanın.  
3. Ardından, **Linux VM'sini** oluştururken eklediğiniz **Parolanızı** girmeniz istenir. Bundan sonra başarıyla oturum açabilmeniz gerekir.  
4. Azure Resource Manager'ın erişim anahtarını almak için CURL kullanın: 
     
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true   
    ```
 
    > [!NOTE]
    > Önceki istekte, "resource" parametresinin değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir. Azure Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz.
    > Aşağıdaki yanıtta, access_token öğesi kısaltılmıştır.
    
    ```bash
    {"access_token":"eyJ0eXAiOi...",
     "expires_in":"3599",
     "expires_on":"1518503375",
     "not_before":"1518499475",
     "resource":"https://management.azure.com/",
     "token_type":"Bearer",
     "client_id":"1ef89848-e14b-465f-8780-bf541d325cd5"}
     ```
    
## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Cosmos DB çağrıları yapmak için Azure Resource Manager'dan erişim anahtarları alma  

Şimdi Cosmos DB hesabı erişim anahtarını almak için önceki bölümde alınan erişim belirtecini kullanarak Resource Manager'ı çağırmak için CURL kullanın. Erişim anahtarını aldıktan sonra Cosmos DB'yi sorgulayabiliriz. `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` ve `<COSMOS DB ACCOUNT NAME>` parametre değerlerini kendi değerlerinizden değiştirmeyi unutmayın. `<ACCESS TOKEN>` değerini daha önce aldığınız erişim belirteciyle değiştirin.  Okuma/yazma anahtarlarını almak istiyorsanız, `listKeys` anahtar işlem türünü kullanın.  Salt okuma anahtarlarını almak istiyorsanız, `readonlykeys` anahtar işlem türünü kullanın:

```bash 
curl 'https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.DocumentDB/databaseAccounts/<COSMOS DB ACCOUNT NAME>/<KEY OPERATION TYPE>?api-version=2016-03-31' -X POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> Önceki URL'nin metni büyük/küçük harfe duyarlıdır; bu nedenle Kaynak Gruplarınız için büyük/küçük harf kullanımının bunu düzgün yansıttığından emin olun. Ayrıca, bir GET isteği değil POST isteği olduğunu bilmeniz ve -d ile NULL olabilecek uzunluk sınırını yakalamak üzere bir değer geçirmeniz de önemlidir.  

CURL yanıtı Anahtarların listesini verir.  Örneğin, salt okuma anahtarlarını alıyorsanız:  

```bash 
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```

Artık Cosmos DB hesabı için erişim anahtarınız olduğundan, bunu Cosmos DB SDK'sına geçirebilir ve hesaba erişmek için çağrılar yapabilirsiniz.  Hızlı bir örnek olarak, erişim anahtarını Azure CLI'ye geçirebilirsiniz.  Azure portalındaki Cosmos DB hesabı dikey penceresinin **Genel Bakış** sekmesinden <COSMOS DB CONNECTION URL> değerini alabilirsiniz.  <ACCESS KEY> değerini yukarıda elde ettiğiniz değerle değiştirin:

```bash
az cosmosdb collection show -c <COLLECTION ID> -d <DATABASE ID> --url-connection "<COSMOS DB CONNECTION URL>" --key <ACCESS KEY>
```

Bu CLI komutu koleksiyon hakkındaki ayrıntıları döndürür:

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

Bu öğreticide, Cosmos DB'ye erişmek için Linux sanal makinesinde sistem tarafından atanan yönetilen kimliği kullanmayı öğrendiniz.  Cosmos DB hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Cosmos DB'ye genel bakış](/azure/cosmos-db/introduction)

