---
title: Azure Cosmos DB’ye erişmek için Windows VM MSI kullanma
description: Windows VM üzerinde bir Sistem Tarafından Atanmış Yönetilen Hizmet Kimliği (MSI) kullanarak Azure Cosmos DB'ye erişme işleminde size yol gösteren bir öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2018
ms.author: daveba
ms.openlocfilehash: ed1aff411ae5446688fe717ddc4570ea756c4c1e
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2018
ms.locfileid: "37904280"
---
# <a name="tutorial-use-a-windows-vm-msi-to-access-azure-cosmos-db"></a>Öğretici: Azure Cosmos DB’ye erişmek için Windows VM MSI kullanma

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide Cosmos DB’ye erişmek için Windows VM MSI oluşturma ve kullanma işlemi gösterilir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * MSI etkin Windows VM oluşturma 
> * Cosmos DB hesabı oluşturma
> * Windows VM MSI'sine Cosmos DB hesabı erişim anahtarları için erişim verme
> * Windows VM MSI'sini kullanarak Azure Resource Manager çağrısı yapmak için erişim belirteci alma
> * Cosmos DB çağrıları yapmak için Azure Resource Manager'dan erişim anahtarları alma

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]


## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda Windows sanal makinesi oluşturma

Bu öğretici için, yeni bir Windows VM oluşturuyoruz.  Ayrıca mevcut bir VM'de MSI'yi etkinleştirebilirsiniz.

1. Azure portalının sol üst köşesinde bulunan **Kaynak oluştur** düğmesine tıklayın.
2. **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3. Sanal makine bilgilerini girin. Burada oluşturulan **Kullanıcı adı** ve **Parola**, sanal makinede oturum açmak için kullandığınız kimlik bilgileridir.
4. Açılan listede sanal makine için uygun **Aboneliği** seçin.
5. İçinde sanal makinenizi oluşturacağınız yeni bir **Kaynak Grubu** seçmek için **Yeni Oluştur**'u seçin. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM'nin boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

   ![Alternatif resim metni](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>VM'nizde MSI'yi etkinleştirme 

Sanal Makine MSI'si kodunuza kimlik bilgileri yerleştirmeniz gerekmeden Azure AD'den erişim belirteçlerini almanıza olanak tanır. Azure portalından bir Sanal Makinede MSI'yi etkinleştirmek aslında şu iki şeyi gerçekleştirir: Bir yönetilen kimlik oluşturmak için VM’nizi Azure AD’ye kaydeder ve kimliği VM’de yapılandırır.

1. MSI'yi etkinleştirmek istediğiniz **Sanal Makine**'yi seçin.  
2. Sol gezinti çubuğunda **Yapılandırma**'ya tıklayın. 
3. **Yönetilen Hizmet Kimliği**'ni görürsünüz. MSI'yi kaydetmek ve etkinleştirmek için **Evet**'i seçin, devre dışı bırakmak istiyorsanız Hayır'ı seçin. 
4. Yapılandırmayı kaydetmek için **Kaydet**’e tıkladığınızdan emin olun.  
   ![Alternatif resim metni](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

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

## <a name="grant-windows-vm-msi-access-to-the-cosmos-db-account-access-keys"></a>Windows VM MSI'sine Cosmos DB hesabı erişim anahtarları için erişim verme

Cosmos DB Azure AD kimlik doğrulamayı yerel olarak desteklemez. Bununla birlikte, Kaynak Yöneticisi'nden Cosmos DB erişim anahtarını almak için bir MSI kullanabilir ve ardından anahtarı kullanarak Cosmos DB'ye erişebilirsiniz. Bu adımda, MSI'nize Cosmos DB hesabının anahtarları için erişim verirsiniz.

PowerShell kullanarak Azure Resource Manager'da Cosmos DB hesabına MSI kimliği erişimi vermek için, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>` ve `<COSMOS DB ACCOUNT NAME>` değerlerini ortamınıza uygun olarak güncelleştirin. `<MSI PRINCIPALID>` değerini, [Linux VM MSI'sinin principalID değerini alma](#retrieve-the-principalID-of-the-linux-VM's-MSI) bölümünde `az resource show` komutu tarafından döndürülen `principalId` özelliğiyle değiştirin.  Cosmos DB, erişim anahtarları kullanılırken iki ayrıntı düzeyini destekler: hesaba okuma/yazma erişimi ve hesaba salt okuma erişimi.  Hesap için okuma/yazma anahtarları almak istiyorsanız `DocumentDB Account Contributor` rolünü veya hesap için salt okuma anahtarları almak istiyorsanız `Cosmos DB Account Reader Role` rolünü atayın:

```azurepowershell
$spID = (Get-AzureRMVM -ResourceGroupName myRG -Name myVM).identity.principalid
New-AzureRmRoleAssignment -ObjectId $spID -RoleDefinitionName "Reader" -Scope "/subscriptions/<mySubscriptionID>/resourceGroups/<myResourceGroup>/providers/Microsoft.Storage/storageAccounts/<myStorageAcct>"
```

## <a name="get-an-access-token-using-the-windows-vms-msi-to-call-azure-resource-manager"></a>Windows VM MSI'sini kullanarak Azure Resource Manager çağrısı yapmak için erişim belirteci alma

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğunuz VM'den çalışacağız. 

Bu bölümde Azure Resource Manager PowerShell cmdlet’lerini kullanmanız gerekir.  Makinenizde yüklü değilse, devam etmeden önce [en son sürümü indirin](https://docs.microsoft.com/powershell/azure/overview).

Ayrıca Windows VM’inize [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)’ın en son sürümünü yüklemeniz gerekir.

1. Azure portalında **Sanal Makineler**'e gidin, Windows sanal makinenize gidin ve ardından **Genel Bakış** sayfasında üst kısımdaki **Bağlan**'a tıklayın. 
2. Windows VM'sini oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin. 
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın.
4. PowerShell’in Invoke-WebRequest komutunu kullanarak, yerel MSI uç noktasına Azure Resource Manager için erişim belirteci alma isteğinde bulunun.

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
Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.DocumentDb/databaseAccounts/<COSMOS DB ACCOUNT NAME>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
Yanıt Anahtarların listesini verir.  Örneğin, salt okuma anahtarlarını alıyorsanız:

```powershell
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

Bu öğreticide, Cosmos DB'ye erişmek için Windows Yönetilen Hizmet Kimliği oluşturmayı öğrendiniz.  Cosmos DB hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Azure Cosmos DB'ye genel bakış](/azure/cosmos-db/introduction)


