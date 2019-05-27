---
title: Azure Cosmos DB hizmetine erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma
description: Windows VM üzerinde bir sistem tarafından atanan yönetilen hizmet kimliği kullanarak Azure Cosmos DB'ye erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: daveba
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e5858fe392629d61b3f0b8833db3af959a16a8b
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "66227694"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB hizmetine erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide, Cosmos DB'ye erişmek amacıyla, Windows sanal makinesi (VM) için sistem tarafından atanan bir yönetilen kimliği nasıl kullanacağınız gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Cosmos DB hesabı oluşturma
> * Windows VM sistem tarafından atanan yönetilen kimliğine Cosmos DB hesabı erişim anahtarları için erişim verme
> * Windows VM’nin sistem tarafından atanan yönetilen kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapma
> * Cosmos DB çağrıları yapmak için Azure Resource Manager'dan erişim anahtarları alma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- En son sürümünü yükleyin [Azure PowerShell](/powershell/azure/install-az-ps)

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

## <a name="grant-windows-vm-system-assigned-managed-identity-access-to-the-cosmos-db-account-access-keys"></a>Windows VM sistem tarafından atanan yönetilen kimliğine Cosmos DB hesabı erişim anahtarları için erişim verme

Cosmos DB Azure AD kimlik doğrulamayı yerel olarak desteklemez. Bununla birlikte, Kaynak Yöneticisi'nden Cosmos DB erişim anahtarını almak için sistem tarafından atanan bir yönetilen kimliği kullanabilir ve ardından anahtarı kullanarak Cosmos DB'ye erişebilirsiniz. Bu adımda, Windows VM sistem tarafından atanan yönetilen kimliğinize Cosmos DB hesabının anahtarları için erişim verirsiniz.

PowerShell kullanarak Azure Resource Manager'da Cosmos DB hesabına Windows VM sistem tarafından atanan yönetilen kimliği erişimi vermek için, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` ve `<COSMOS DB ACCOUNT NAME>` değerlerini ortamınıza uygun olarak güncelleştirin. Cosmos DB, erişim anahtarları kullanılırken iki ayrıntı düzeyini destekler: hesaba okuma/yazma erişimi ve hesaba salt okuma erişimi.  Hesap için okuma/yazma anahtarları almak istiyorsanız `DocumentDB Account Contributor` rolünü veya hesap için salt okuma anahtarları almak istiyorsanız `Cosmos DB Account Reader Role` rolünü atayın.  Bu öğretici için `Cosmos DB Account Reader Role` rolünü atayın:

```azurepowershell
$spID = (Get-AzVM -ResourceGroupName myRG -Name myVM).identity.principalid
New-AzRoleAssignment -ObjectId $spID -RoleDefinitionName "Cosmos DB Account Reader Role" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>"
```
## <a name="get-an-access-token-using-the-windows-vm-system-assigned-managed-identity-to-call-azure-resource-manager"></a>Windows VM’nin sistem tarafından atanan yönetilen kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapma

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğumuz VM'den çalışacağız. 

En son sürümünü yüklemek ihtiyacınız olacak [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) Windows vm'nizde.

1. Azure portalında **Sanal Makineler**'e gidin, Windows sanal makinenize gidin ve ardından **Genel Bakış** sayfasında üst kısımdaki **Bağlan**'a tıklayın. 
2. Windows VM'sini oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin. 
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın.
4. PowerShell’in Invoke-WebRequest komutunu kullanarak, Azure kaynakları uç noktası için yerel yönetilen kimliklere Azure Resource Manager için erişim belirteci alma isteğinde bulunun.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
   ```

   > [!NOTE]
   > "Resource" parametre değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir. Azure Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz.
    
   Ardından, $response nesnesinde JavaScript Nesne Gösterimi (JSON) biçimlendirilmiş dizesi olarak depolanan “Content” öğesini ayıklayın. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```
   Ardından, yanıttan erişim belirtecini ayıklayın.
    
   ```powershell
   $ArmToken = $content.access_token
   ```

## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Cosmos DB çağrıları yapmak için Azure Resource Manager'dan erişim anahtarları alma

Şimdi Cosmos DB hesabı erişim anahtarını almak için önceki bölümde alınan erişim belirtecini kullanarak Resource Manager'ı çağırmak için PowerShell kullanın. Erişim anahtarını aldıktan sonra Cosmos DB'yi sorgulayabiliriz. `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` ve `<COSMOS DB ACCOUNT NAME>` parametre değerlerini kendi değerlerinizden değiştirmeyi unutmayın. `<ACCESS TOKEN>` değerini daha önce aldığınız erişim belirteciyle değiştirin.  Okuma/yazma anahtarlarını almak istiyorsanız, `listKeys` anahtar işlem türünü kullanın.  Salt okuma anahtarlarını almak istiyorsanız, `readonlykeys` anahtar işlem türünü kullanın:

```powershell
Invoke-WebRequest -Uri 'https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>/listKeys/?api-version=2016-03-31' -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
Yanıt Anahtarların listesini verir.  Örneğin, salt okuma anahtarlarını alıyorsanız:

```powershell
{"primaryReadonlyMasterKey":"bWpDxS...dzQ==",
"secondaryReadonlyMasterKey":"38v5ns...7bA=="}
```
Artık Cosmos DB hesabı için erişim anahtarınız olduğundan, bunu Cosmos DB SDK'sına geçirebilir ve hesaba erişmek için çağrılar yapabilirsiniz.  Hızlı bir örnek olarak, erişim anahtarını Azure CLI'ye geçirebilirsiniz.  Azure portalındaki Cosmos DB hesabı dikey penceresinin **Genel Bakış** sekmesinden `<COSMOS DB CONNECTION URL>` değerini alabilirsiniz.  `<ACCESS KEY>` değerini yukarıda elde ettiğiniz değerle değiştirin:

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

Bu öğreticide Cosmos DB'ye erişmek için Windows VM sistem tarafından atanan kimliğini kullanmayı öğrendiniz.  Cosmos DB hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Cosmos DB'ye genel bakış](/azure/cosmos-db/introduction)


