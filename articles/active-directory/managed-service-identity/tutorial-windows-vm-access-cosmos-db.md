---
title: Azure Cosmos DB erişmek için bir Windows VM MSI kullanın
description: Azure Cosmos DB erişmek için bir Windows VM üzerinde bir System-Assigned yönetilen hizmet kimliği (MSI) kullanarak sürecinde anlatan öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: arluca
ms.openlocfilehash: 0e62e11e0f86714b79cf2bc84bbb505b03df1ba6
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="use-a-windows-vm-msi-to-access-azure-cosmos-db"></a>Azure Cosmos DB erişmek için bir Windows VM MSI kullanın

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici nasıl oluşturulacağı ve bir Windows VM MSI Cosmos DB erişmek için kullandığınız gösterir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Oluşturma Windows VM bir MSI etkin 
> * Cosmos DB hesabı oluşturma
> * Cosmos DB hesabı erişim anahtarlarını GRANT Windows VM MSI erişimi
> * Azure Resource Manager çağırmak için Windows VM MSI kullanarak bir erişim belirteci alın
> * Cosmos DB çağrı yapmak için Azure Resource Manager gelen erişim anahtarı alma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturma

Bu öğretici için yeni bir Windows VM oluşturun.  Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3. Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgileri İşte oluşturulan sanal makineye oturum açma.
4. Uygun seçin **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** içinde sanal makine oluşturmak, seçim **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar sayfasında, Varsayılanları tutun ve tıklatın **Tamam**.

   ![Alt görüntü metin](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir 

Bir sanal makine MSI kodunuza kimlik bilgileri almaya gerek kalmadan Azure AD'den erişim belirteçleri almak etkinleştirir. Perde arkasında MSI Azure portal aracılığıyla bir sanal makinede etkinleştirilmesi iki işlemi yapar: yönetilen bir kimlik oluşturmak üzere Azure AD ile VM kaydeder ve VM kimliğini yapılandırır.

1. Seçin **sanal makine** MSI etkinleştirmek istediğiniz.  
2. Sol gezinti çubuğunda **yapılandırma**. 
3. Gördüğünüz **yönetilen hizmet kimliği**. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No 
4. Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.  
   ![Alt görüntü metin](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-cosmos-db-account"></a>Cosmos DB hesabı oluşturma 

Zaten yoksa, bir Cosmos DB hesabı oluşturun. Bu adımı atlayın ve var olan bir Cosmos DB hesabını kullanın. 

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

## <a name="grant-windows-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Cosmos DB hesabı erişim anahtarlarını GRANT Windows VM MSI erişimi

Cosmos DB yerel olarak Azure AD kimlik doğrulamasını desteklemez. Ancak, bir MSI Kaynak Yöneticisi'nden Cosmos DB erişim tuşunu kullanın ve anahtar Cosmos DB erişmek için kullanılır. Bu adımda, Cosmos DB hesabı anahtarları, MSI erişim izni.

Cosmos DB hesabı Azure kaynağı PowerShell kullanarak Yöneticisi'nde MSI kimlik erişim vermek için değerleri güncelleştirmek `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<COSMOS DB ACCOUNT NAME>` ortamınız için. Değiştir `<MSI PRINCIPALID>` ile `principalId` özellik tarafından döndürülen `az resource show` komutunu [Linux VM MSI Principalıd almak](#retrieve-the-principalID-of-the-linux-VM's-MSI).  Cosmos DB erişim tuşlarını kullanırken iki düzeyde ayrıntı düzeyi destekler: okuma/yazma erişimi hesabı ve salt okunur erişim hesabı.  Ata `DocumentDB Account Contributor` hesabının okuma/yazma anahtarlarını almak ya da atamak istiyorsanız, rol `Cosmos DB Account Reader Role` hesabı için salt okunur anahtarlarını almak istiyorsanız, rol:

```azurepowershell
$spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
```

## <a name="get-an-access-token-using-the-windows-vms-msi-to-call-azure-resource-manager"></a>Azure Resource Manager çağırmak için Windows VM MSI kullanarak bir erişim belirteci alın

Öğretici kalanı için size daha önce oluşturduğumuz sanal makineden çalışmaz. 

Bu bölümünde Azure Resource Manager PowerShell cmdlet'lerini kullanmanız gerekecektir.  Yüklü, yoksa [en son sürümü karşıdan](https://docs.microsoft.com/powershell/azure/overview) devam etmeden önce.

Ayrıca en son sürümünü yüklemeniz gerekir [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) Windows VM üzerinde.

1. Azure portalında gidin **sanal makineleri**gidin Windows sanal makinenizi, ardından **genel bakış** sayfasında **Bağlan** üstünde. 
2. Girin, **kullanıcıadı** ve **parola** Windows VM oluşturduğunuzda, eklediğiniz için. 
3. Oluşturduğunuza göre bir **Uzak Masaüstü Bağlantısı** sanal makineyle PowerShell uzak oturum açın.
4. PowerShell'in Invoke-WebRequest kullanarak, Azure kaynak yöneticisi için bir erişim belirteci almak üzere yerel MSI uç nokta için bir isteği oluşturun.

    ```powershell
        $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```

    > [!NOTE]
    > "Kaynak" parametresinin değeri, Azure AD tarafından beklenen bir tam eşleşme olmalıdır. Azure Resource Manager kaynak kimliği'ni kullanırken eğik URI üzerinde eklemeniz gerekir.
    
    Ardından, bir JavaScript nesne gösterimi (JSON) biçimlendirilmiş dize $response nesnesi olarak depolanan "İçerik" öğesi ayıklayın. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Ardından, erişim belirteci yanıttan ayıklayın.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-access-keys-from-azure-resource-manager-to-make-cosmos-db-calls"></a>Cosmos DB çağrı yapmak için Azure Resource Manager gelen erişim anahtarı alma

Artık Kaynak Yöneticisi'ni Cosmos DB hesap erişim anahtarı almak için önceki bölümde alınan erişim belirteci kullanarak çağırmak için PowerShell kullanın. Biz erişim tuşu sahip olduğunda, biz Cosmos DB sorgulayabilirsiniz. Değiştirdiğinizden emin olun `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<COSMOS DB ACCOUNT NAME>` parametre değerlerini kendi değerlere sahip. Değiştir `<ACCESS TOKEN>` daha önce erişim belirteci ile değer.  Okuma/yazma anahtarları almak istiyorsanız, anahtar işlem türü kullanın `listKeys`.  Salt okunur anahtarları almak istiyorsanız, anahtar işlem türü kullanın `readonlykeys`:

```powershell
Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
Yanıt size anahtarlarının listesi.  Örneğin, salt okunur anahtarları alırsanız:

```powershell
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

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](overview.md).
