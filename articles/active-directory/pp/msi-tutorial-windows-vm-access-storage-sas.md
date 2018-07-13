---
title: Bir SAS kimlik bilgisi kullanarak Azure Depolama'ya erişmek için bir Windows VM MSI kullanma
description: Bir Windows VM yönetilen hizmet kimliği (MSI) Azure depolama, depolama hesabı erişim anahtarı yerine SAS kimlik bilgisi kullanarak erişmek için nasıl kullanılacağını gösteren öğretici.
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
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: a96e0948f652e49afb955eaf32f14cf14a10df34
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007409"
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Azure depolama bir SAS kimlik bilgisi erişmek için bir Windows VM yönetilen hizmet Kimliği'ni kullanın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Windows sanal makinesi için Yönetilen hizmet kimliği (MSI) etkinleştirin ve ardından depolama paylaşılan erişim imzası (SAS) kimlik bilgisi elde etmek için MSI kullanma gösterilmektedir. Özellikle, bir [hizmet SAS kimlik bilgisi](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Hizmet SAS, bir hesap erişim anahtarına sokmadan sınırlı süre için bir depolama hesabındaki nesnelere ve belirli bir hizmete (Bu örnekte, blob hizmeti), sınırlı erişim olanağı sağlar. Her zaman olduğu gibi depolama SDK'sı kullanırken, örneğin, depolama işlemleri yaparken bir SAS kimlik bilgisini kullanabilirsiniz. Bu öğreticide, yükleme ve Azure Storage PowerShell kullanarak bir blob indirme gösterir. Şunların nasıl yapılır:


> [!div class="checklist"]
> * Bir Windows sanal makinesinde MSI etkinleştir 
> * Bir depolama hesabı SAS Kaynak Yöneticisi'nde, VM erişimi verme 
> * Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve Kaynak Yöneticisi'nden SAS almak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturun

Bu öğretici için yeni bir Windows VM'yi oluştururuz. Mevcut VM'yi MSI de etkinleştirebilirsiniz.

1.  Tıklayın **+/ yeni hizmet oluşturma** düğmesi Azure portalının sol üst köşedeki üzerinde bulunamadı.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgilerini İşte oluşturulan sanal makineye oturum açma.
4.  Uygun seçin **abonelik** sanal makinenin açılır.
5.  Yeni bir seçilecek **kaynak grubu** oluşturulması, seçmek üzere sanal makineye istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  Sanal makine için boyutu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>Vm'nizde MSI etkinleştir

Bir sanal makine MSI, kimlik bilgilerini kodunuza koyma gereksinimi olmadan Azure AD'den erişim belirteci alma olanak tanır. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve sanal makine için MSI sağlar.  

1. Yeni sanal makinenize kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. VM Sol paneldeki "ayarlar" altında tıklayın **yapılandırma**.
3. Kaydolun ve MSI etkinleştirmek için **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıkladığınız olun **Kaydet** yapılandırmayı kaydetmek için.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Hangi uzantıların denetlemek istiyorsanız, sanal makinede olan, tıklayın **uzantıları**. MSI etkin olduğunda **ManagedIdentityExtensionforWindows** listesinde görünür.

    ![Alt resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Zaten yoksa, artık bir depolama hesabı oluşturur. Ayrıca, bu adımı atlayıp mevcut bir depolama hesabı SAS kimlik bilgisi için VM MSI erişimi verme. 

1. Tıklayın **+/ yeni hizmet oluşturma** düğmesi Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. Tıklayın **depolama**, ardından **depolama hesabı**, ve "depolama hesabı oluştur" yeni bir panel görüntülenir.
3. Daha sonra kullanacağınız depolama hesabı için bir ad girin.  
4. **Dağıtım modeli** ve **hesap türü** sırasıyla "Resource manager" ve "Genel amaç" için ayarlanmalıdır. 
5. Olun **abonelik** ve **kaynak grubu** VM'nizi oluşturduğunuzda önceki adımda belirttiğiniz olanlarla eşleşmesi.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabındaki bir blob kapsayıcısı oluşturma

Daha sonra karşıya eder ve yeni depolama hesabı için bir dosya indirin. Dosyaları blob depolama gerektirdiğinden dosyasının depolanacağı bir blob kapsayıcısı oluşturmak gerekir.

1. Yeni oluşturulan depolama hesabına geri gidin.
2. Tıklayın **kapsayıcıları** bağlantı sol bölmede, "Blob hizmeti."
3. Tıklayın **+ kapsayıcı** sayfasına ve "yeni bir kapsayıcı" üst kısmındaki çıkış paneli slaytlar.
4. Kapsayıcıya bir ad verin, bir erişim düzeyi seçin ve ardından tıklayın **Tamam**. Belirttiğiniz adı öğreticinin ilerleyen bölümlerinde kullanılacaktır. 

    ! [Depolama container]~/articles/active-directory/(media/msi-tutorial-linux-vm-access-storage/create-blob-container.png) oluştur

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>Bir SAS depolama kullanmak için sanal makinenizin MSI erişimi verme 

Azure depolama, yerel olarak Azure AD kimlik doğrulamasını desteklemez.  Ancak, Kaynak Yöneticisi'nden depolama SAS almak için bir MSI kullanma sonra depolamaya erişmek için SAS'ı kullanın.  Bu adımda, depolama hesabınıza SAS, VM MSI erişimi verme.   

1. Yeni oluşturulan depolama hesabına geri gidin.   
2. Tıklayın **erişim denetimi (IAM)** sol bölmede bağlantı.  
3. Tıklayın **+ Ekle** VM'niz için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Ayarlama **rol** "Depolama hesabı Katılımcısı", sayfanın sağ tarafındaki için.  
5. Sonraki açılır menüden ayarlamak **erişim Ata** "Sanal makinesi" kaynak.  
6. Ardından, uygun abonelik listelendiğinden emin olmak **abonelik** açılır listesinde, ardından ayarlayın **kaynak grubu** "Tüm kaynak gruplarına".  
7. Son olarak, altında **seçin** açılır menüde, Windows sanal makinenizi seçin, ardından tıklayın **Kaydet**. 

    ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve Azure Resource Manager'ı çağırmak için kullanın 

Bu öğreticinin geri kalanında için daha önce oluşturduğumuz VM'den çalışacağız.

Bu bölümünde Azure Resource Manager PowerShell cmdlet'lerini kullanmanız gerekecektir.  Yüklü, yoksa [son sürümünü indirin](https://docs.microsoft.com/powershell/azure/overview) devam etmeden önce.

1. Azure portalında gidin **sanal makineler**gidin Windows sanal makinenizi, sonra **genel bakış** sayfasında **Connect** en üstünde.
2. Girin, **kullanıcıadı** ve **parola** Windows sanal Makineyi oluştururken, eklediğiniz için. 
3. Sizin oluşturduğunuz bir **Uzak Masaüstü Bağlantısı** sanal makineyle PowerShell uzak oturumu açın. 
4. PowerShell'in Invoke-WebRequest kullanarak, Azure Resource Manager için bir erişim belirteci almak için yerel MSI uç noktasına bir istek olun.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > "Kaynak" parametresinin değerini, Azure AD tarafından beklenen değer için bir tam eşleşme olmalıdır. Azure Resource Manager kaynak kimliği kullanıldığında, URI üzerinde sonunda eğik çizgi içermelidir.
    
    Ardından, $response nesneyi JavaScript nesne gösterimi (JSON) biçimlendirilen dizesinde olarak depolanan "İçerik" öğesi ayıklayın. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Ardından, erişim belirtecini yanıttan ayıklayın.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrıları yapmak için Azure Resource Manager'dan bir SAS kimlik bilgileri Al 

Artık depolama SAS kimlik bilgisi oluşturmak için önceki bölümde biz alınan erişim belirteci kullanarak Resource Manager'ı çağırmak için PowerShell kullanın. Biz SAS kimlik bilgilerini aldıktan sonra depolama işlemleri diyoruz.

Bu istek için SAS kimlik bilgisi oluşturmak için aşağıdaki HTTP istek parametrelerini kullanacağız:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite. Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Bu parametreler, SAS kimlik bilgisi için istek POST gövdesinde dahil edilir. Bir SAS kimlik bilgisi oluşturmak için parametreler hakkında daha fazla bilgi için bkz. [listesi hizmet SAS REST başvurusu](/rest/api/storagerp/storageaccounts/listservicesas).

İlk olarak, Parametreler JSON biçimine dönüştür ve ardından depolama birimi çağrısı `listServiceSas` SAS oluşturmak için uç nokta kimlik bilgisi:

```powershell
$params = @{canonicalizedResource="/blob/<STORAGE-ACCOUNT-NAME>/<CONTAINER-NAME>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="2017-09-23T00:00:00Z"}
$jsonParams = $params | ConvertTo-Json
```

```powershell
$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT-NAME>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}
```
> [!NOTE] 
> URL büyük/küçük harfe duyarlıdır, dolayısıyla sağlamak büyük harf "G" içinde "resourceGroups." içeren kaynak grubunu, adlı daha önce kullanılan tam aynı çalışması kullanın 

Şimdi biz SAS kimlik bilgisi yanıttan ayıklayın:

```powershell
$sasContent = $sasResponse.Content | ConvertFrom-Json
$sasCred = $sasContent.serviceSasToken
```

SAS kimlik bilgileri incelemek, şöyle bir şey görürsünüz:

```powershell
PS C:\> $sasCred
sv=2015-04-05&sr=c&spr=https&se=2017-09-23T00%3A00%3A00Z&sp=rcw&sig=JVhIWG48nmxqhTIuN0uiFBppdzhwHdehdYan1W%2F4O0E%3D
```

Ardından "test.txt" adlı bir dosya oluşturun. SAS kimlik bilgisi ile kimlik doğrulaması kullanacağınızı `New-AzureStorageContent` cmdlet'i, dosyanın bizim blob kapsayıcısına yükleyin ve ardından dosyayı indirin.

```bash
echo "This is a test text file." > test.txt
```

Azure depolama cmdlet'leri ilk olarak, yüklediğinizden emin olun'ı kullanarak `Install-Module Azure.Storage`. Ardından az önce oluşturduğunuz kullanarak blob karşıya `Set-AzureStorageBlobContent` PowerShell cmdlet:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT-NAME> -SasToken $sasCred
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Yanıt:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Yüklediğiniz, kullanarak blob da indirebilirsiniz `Get-AzureStorageBlobContent` PowerShell cmdlet:

```powershell
Get-AzureStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Yanıt:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```


## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Bu öğreticiyi yapma hakkında bilgi için bkz. depolama hesabı anahtarı kullanma, [Azure depolamaya erişmek için bir Windows VM yönetilen hizmet kimliği kullan](msi-tutorial-windows-vm-access-storage.md)
- Azure depolama hesabı SAS özelliği hakkında daha fazla bilgi için bkz:
  - [Paylaşılan erişim imzaları (SAS) kullanma](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Hizmet SAS oluşturma](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.


