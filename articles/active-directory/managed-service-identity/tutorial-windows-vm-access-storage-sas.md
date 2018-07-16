---
title: Bir SAS kimlik bilgisi kullanarak Azure Depolama'ya erişmek için bir Windows VM MSI kullanma
description: Bir Windows VM yönetilen hizmet kimliği (MSI) Azure depolama, depolama hesabı erişim anahtarı yerine SAS kimlik bilgisi kullanarak erişmek için nasıl kullanılacağını gösteren öğretici.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 89bbf0bff107cd297f69c0bf5a4017959ea238cd
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39043984"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Öğretici: Azure depolama bir SAS kimlik bilgisi erişmek için bir Windows VM yönetilen hizmet kimliği kullanın.

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide bir Windows sanal makinesi için Yönetilen hizmet kimliği (MSI) etkinleştirin ve ardından depolama paylaşılan erişim imzası (SAS) kimlik bilgisi elde etmek için MSI kullanma gösterilmektedir. Özellikle, bir [hizmet SAS kimlik bilgisi](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Hizmet SAS, bir hesap erişim anahtarına sokmadan sınırlı süre için bir depolama hesabındaki nesnelere ve belirli bir hizmete (Bu örnekte, blob hizmeti), sınırlı erişim olanağı sağlar. Her zaman olduğu gibi depolama SDK'sı kullanırken, örneğin, depolama işlemleri yaparken bir SAS kimlik bilgisini kullanabilirsiniz. Bu öğreticide, yükleme ve Azure Storage PowerShell kullanarak bir blob indirme gösterir. Şunların nasıl yapılır:


> [!div class="checklist"]
> * Bir Windows sanal makinesinde MSI etkinleştir 
> * Bir depolama hesabı SAS Kaynak Yöneticisi'nde, VM erişimi verme 
> * Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve Kaynak Yöneticisi'nden SAS almak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda Windows sanal makinesi oluşturma

Bu öğretici için, yeni bir Windows VM oluşturuyoruz. Ayrıca mevcut bir VM'de MSI'yi etkinleştirebilirsiniz.

1.  Azure portalının sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. Burada oluşturulan **Kullanıcı adı** ve **Parola**, sanal makinede oturum açmak için kullandığınız kimlik bilgileridir.
4.  Açılan listede sanal makine için uygun **Aboneliği** seçin.
5.  İçinde sanal makinenin oluşturulmasını istediğiniz yeni bir **Kaynak Grubu** seçmek için, **Yeni Oluştur**'u seçin. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM'nin boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alternatif resim metni](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>VM'nizde MSI'yi etkinleştirme

Sanal Makine MSI'si kodunuza kimlik bilgileri yerleştirmeniz gerekmeden Azure AD'den erişim belirteçlerini almanıza olanak tanır. MSI'nin etkinleştirilmesi arka planda iki işlem yapar: yönetilen kimliğini oluşturmak için VM'nizi Azure Active Directory'ye kaydeder ve kimliği VM'de yapılandırır.

1. Yeni sanal makinenize kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. VM Sol paneldeki "ayarlar" altında tıklayın **yapılandırma**.
3. MSI'yi kaydetmek ve etkinleştirmek için **Evet**'i seçin, devre dışı bırakmak istiyorsanız Hayır'ı seçin.
4. Yapılandırmayı kaydetmek için **Kaydet**’e tıkladığınızdan emin olun.

    ![Alternatif resim metni](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Zaten yoksa, artık bir depolama hesabı oluşturur. Ayrıca, bu adımı atlayıp mevcut bir depolama hesabı SAS kimlik bilgisi için VM MSI erişimi verme. 

1. Azure portalının sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2. Tıklayın **depolama**, ardından **depolama hesabı**, ve "depolama hesabı oluştur" yeni bir panel görüntülenir.
3. Daha sonra kullanacağınız depolama hesabı için bir ad girin.  
4. **Dağıtım modeli** ve **hesap türü** sırasıyla "Resource manager" ve "Genel amaç" için ayarlanmalıdır. 
5. **Abonelik** ve **Kaynak Grubu** değerlerinin, önceki adımda VM'nizi oluştururken belirttiklerinizle eşleştiğinden emin olun.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabındaki bir blob kapsayıcısı oluşturma

Daha sonra karşıya eder ve yeni depolama hesabı için bir dosya indirin. Dosyaları blob depolama gerektirdiğinden dosyasının depolanacağı bir blob kapsayıcısı oluşturmak gerekir.

1. Yeni oluşturulan depolama hesabına geri gidin.
2. Tıklayın **kapsayıcıları** bağlantı sol bölmede, "Blob hizmeti."
3. Tıklayın **+ kapsayıcı** sayfasına ve "yeni bir kapsayıcı" üst kısmındaki çıkış paneli slaytlar.
4. Kapsayıcıya bir ad verin, bir erişim düzeyi seçin ve ardından tıklayın **Tamam**. Belirttiğiniz adı öğreticinin ilerleyen bölümlerinde kullanılacaktır. 

    ![Depolama kapsayıcısı oluşturma](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>Bir SAS depolama kullanmak için sanal makinenizin MSI erişimi verme 

Azure depolama, yerel olarak Azure AD kimlik doğrulamasını desteklemez.  Ancak, Kaynak Yöneticisi'nden depolama SAS almak için bir MSI kullanma sonra depolamaya erişmek için SAS'ı kullanın.  Bu adımda, depolama hesabınıza SAS, VM MSI erişimi verme.   

1. Yeni oluşturulan depolama hesabına geri gidin.   
2. Tıklayın **erişim denetimi (IAM)** sol bölmede bağlantı.  
3. Tıklayın **+ Ekle** VM'niz için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Ayarlama **rol** "Depolama hesabı Katılımcısı", sayfanın sağ tarafındaki için.  
5. Sonraki açılır menüden ayarlamak **erişim Ata** "Sanal makinesi" kaynak.  
6. Ardından, uygun abonelik listelendiğinden emin olmak **abonelik** açılır listesinde, ardından ayarlayın **kaynak grubu** "Tüm kaynak gruplarına".  
7. Son olarak, altında **seçin** açılır menüde, Windows sanal makinenizi seçin, ardından tıklayın **Kaydet**. 

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve Azure Resource Manager'ı çağırmak için kullanın 

Bu öğreticinin geri kalanında için daha önce oluşturduğumuz VM'den çalışacağız.

Bu bölümünde Azure Resource Manager PowerShell cmdlet'lerini kullanmanız gerekecektir.  Yüklü, yoksa [son sürümünü indirin](https://docs.microsoft.com/powershell/azure/overview) devam etmeden önce.

1. Azure portalında gidin **sanal makineler**gidin Windows sanal makinenizi, sonra **genel bakış** sayfasında **Connect** en üstünde.
2. Windows VM'sini oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin. 
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın. 
4. PowerShell'in Invoke-WebRequest kullanarak, Azure Resource Manager için bir erişim belirteci almak için yerel MSI uç noktasına bir istek olun.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > "Kaynak" parametresinin değerini, Azure AD tarafından beklenen değer için bir tam eşleşme olmalıdır. Azure Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz.
    
    Ardından, $response nesneyi JavaScript nesne gösterimi (JSON) biçimlendirilen dizesinde olarak depolanan "İçerik" öğesi ayıklayın. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Ardından, yanıttan erişim belirtecini ayıklayın.
    
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

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir SAS kimlik bilgisi kullanarak Azure Depolama'ya erişmek için Yönetilen hizmet kimliği oluşturma öğrendiniz.  Azure depolama SAS bakın hakkında daha fazla bilgi için:

> [!div class="nextstepaction"]
>[Paylaşılan erişim imzaları (SAS) kullanma](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)


