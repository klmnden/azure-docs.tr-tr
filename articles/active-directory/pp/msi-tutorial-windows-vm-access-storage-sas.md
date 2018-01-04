---
title: "Azure Storage SAS kimlik bilgilerini kullanarak erişmek için bir Windows VM MSI kullanın"
description: "Azure depolama, depolama hesabının erişim anahtarı yerine SAS kimlik bilgilerini kullanarak erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanmayı gösterir Öğreticisi."
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 0c6150c01c8ca31bba748741b2285cb4f29beaa6
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2017
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Azure Storage bir SAS kimlik bilgisi erişmek için bir Windows VM yönetilen hizmet kimliği kullanın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğretici bir Windows sanal makine için Yönetilen hizmet kimliği (MSI) etkinleştirmek, sonra bir depolama paylaşılan erişim imzası (SAS) kimlik bilgisi almak için MSI kullanın gösterilmektedir. Özellikle, bir [hizmet SAS kimlik bilgisi](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Hizmet SAS hesap erişim anahtarı sokmadan sınırlı bir süre için bir depolama hesabındaki nesnelere ve belirli bir hizmet (Bu örnekte, blob hizmeti), sınırlı erişim olanağı sağlar. Her zamanki gibi depolama SDK'yı kullanarak, örneğin depolama işlemleri yaparken bir SAS kimlik bilgisi kullanabilirsiniz. Bu öğretici için karşıya yükleme ve Azure Storage PowerShell kullanarak bir blob indirme göstermektedir. Şunları öğreneceksiniz nasıl yapılır:


> [!div class="checklist"]
> * Bir Windows sanal makinesinde MSI etkinleştir 
> * Bir depolama hesabı SAS Kaynak Yöneticisi'nde, VM erişim 
> * VM kimliğini kullanarak bir erişim belirteci alın ve Kaynak Yöneticisi'nden SAS almak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresindeki Azure portalında oturum açın.

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Windows sanal makine oluşturma

Bu öğretici için yeni bir Windows VM oluşturun. Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1.  Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2.  **İşlem**'i seçin ve sonra da **Windows Server 2016 Datacenter**'ı seçin. 
3.  Sanal makine bilgilerini girin. **Kullanıcıadı** ve **parola** için kullandığınız kimlik bilgileri İşte oluşturulan sanal makineye oturum açma.
4.  Uygun seçin **abonelik** sanal makine açılır.
5.  Yeni bir seçmek için **kaynak grubu** oluşturulması, seçmek için sanal makine için istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6.  VM boyutunu seçin. Daha fazla boyut görmek için **Tümünü görüntüle**’yi seçin veya **Desteklenen disk türü** filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

    ![Alt görüntü metin](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir

Bir sanal makine MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. Perde arkasında MSI etkinleştirme iki işlemi yapar: MSI VM uzantısı, VM yükler ve sanal makine için MSI sağlar.  

1. Yeni bir sanal makine kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. VM Sol paneldeki "ayarlar" altında tıklatın **yapılandırma**.
3. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.

    ![Alt görüntü metin](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Denetlemek isterseniz, hangi uzantıları VM, tıklatın **uzantıları**. MSI etkinleştirilirse, **ManagedIdentityExtensionforWindows** listede görüntülenir.

    ![Alt görüntü metin](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Zaten yoksa, şimdi bir depolama hesabı oluşturacak. Ayrıca, bu adımı atlayın ve VM MSI erişim mevcut bir depolama hesabını SAS kimlik. 

1. Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. Tıklatın **depolama**, ardından **depolama hesabı**, ve yeni bir "depolama hesabı oluşturma" panelinde görüntülenir.
3. Daha sonra kullanacaksınız depolama hesabı için bir ad girin.  
4. **Dağıtım modeli** ve **tür hesap** "Resource manager" ve "Genel amaçlı", sırasıyla ayarlanmalıdır. 
5. Olun **abonelik** ve **kaynak grubu** VM'nizi oluşturduğunuzda önceki adımda belirttiğiniz olanlarla eşleşmesi.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabında blob kapsayıcısı oluşturma

Daha sonra biz karşıya yükleyin ve yeni depolama hesabı dosya indirme. Dosyaları blob depolama gerektirdiğinden, biz dosyasının depolanacağı bir blob kapsayıcısı oluşturmanız gerekir.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Tıklatın **kapsayıcıları** bağlantı sol panelinde, "Blob hizmeti."
3. Tıklatın **+ kapsayıcı** sayfa ve "yeni bir kapsayıcı" üst kısmında çıkış paneli slayt.
4. Kapsayıcı bir ad verin, erişim düzeyi seçin ve ardından **Tamam**. Belirtilen ad daha sonra öğreticide kullanılır. 

    ! [Depolama container]~/articles/active-directory/(media/msi-tutorial-linux-vm-access-storage/create-blob-container.png) oluştur

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>SAS depolama kullanmak için VM MSI erişim 

Azure Storage, Azure AD kimlik doğrulaması yerel olarak desteklemez.  Ancak, bir MSI depolama SAS Kaynak Yöneticisi'nden almanızı sonra SAS depolama erişmek için kullanın.  Bu adımda, depolama hesabınıza SAS VM MSI erişim verin.   

1. Yeni oluşturulan depolama hesabınıza geri gidin.   
2. Tıklatın **erişim denetimi (IAM)** sol panelinde bağlantı.  
3. Tıklatın **+ Ekle** VM için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Ayarlama **rol** "Depolama hesabı katkıda bulunan", sayfanın sağ tarafında için.  
5. Sonraki açılır listede ayarlamak **atamak için erişim** "Sanal makine" kaynak.  
6. Ardından, uygun abonelik listelenir olun **abonelik** açılan listesinde, daha sonra ayarlamak **kaynak grubu** "Tüm kaynak gruplarına".  
7. Son olarak, altında **seçin** açılır listede, Windows sanal makine seçin ve ardından **kaydetmek**. 

    ![Alt görüntü metin](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>VM kimliğini kullanarak bir erişim belirteci alın ve Azure Resource Manager çağırmak için kullanın 

Öğretici kalanı için size daha önce oluşturduğumuz sanal makineden çalışmaz.

Bu bölümünde Azure Resource Manager PowerShell cmdlet'lerini kullanmanız gerekecektir.  Yüklü, yoksa [en son sürümü karşıdan](https://docs.microsoft.com/powershell/azure/overview) devam etmeden önce.

1. Azure portalında gidin **sanal makineleri**gidin Windows sanal makinenizi, ardından **genel bakış** sayfasında **Bağlan** üstünde.
2. Girin, **kullanıcıadı** ve **parola** Windows VM oluşturduğunuzda, eklediğiniz için. 
3. Oluşturduğunuza göre bir **Uzak Masaüstü Bağlantısı** sanal makineyle PowerShell uzak oturum açın. 
4. PowerShell'in Invoke-WebRequest kullanarak, Azure kaynak yöneticisi için bir erişim belirteci almak üzere yerel MSI uç nokta için bir isteği oluşturun.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
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

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrı yapmak için Azure Resource Manager gelen bir SAS kimlik bilgilerini alamıyor 

Şimdi Resource Manager depolama SAS kimlik bilgisi oluşturmak için önceki bölümde biz alınan erişim belirteci kullanarak çağırmak için PowerShell kullanın. Biz SAS kimlik bilgisi olduktan sonra biz depolama işlemleri çağırabilirsiniz.

Bu istek için SAS kimlik bilgisi oluşturmak için izleme HTTP istek parametreleri kullanırız:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite. Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Bu parametreler, SAS kimlik bilgisi iste POST gövdesinde dahil edilir. Bir SAS kimlik bilgisi oluşturmak için parametreler hakkında daha fazla bilgi için bkz: [listesi hizmet SAS REST başvurusu](/rest/api/storagerp/storageaccounts/listservicesas).

İlk olarak, parametreleri JSON biçimine dönüştür ardından depolama birimi çağrısı `listServiceSas` SAS oluşturmak için uç noktası kimlik bilgileri:

```powershell
$params = @{canonicalizedResource="/blob/<STORAGE-ACCOUNT-NAME>/<CONTAINER-NAME>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="2017-09-23T00:00:00Z"}
$jsonParams = $params | ConvertTo-Json
```

```powershell
$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT-NAME>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}
```
> [!NOTE] 
> URL büyük/küçük harfe duyarlıdır, bu nedenle olun "ResourceGroups." olarak büyük harf "G" gibi kaynak grubu adında daha önce kullanılan tam aynı durumda kullanın 

Şimdi biz SAS kimlik bilgisi yanıttan ayıklayın:

```powershell
$sasContent = $sasResponse.Content | ConvertFrom-Json
$sasCred = $sasContent.serviceSasToken
```

SAS ki inceleyin, şöyle bir şey görürsünüz:

```powershell
PS C:\> $sasCred
sv=2015-04-05&sr=c&spr=https&se=2017-09-23T00%3A00%3A00Z&sp=rcw&sig=JVhIWG48nmxqhTIuN0uiFBppdzhwHdehdYan1W%2F4O0E%3D
```

Sonraki "sınama.txt" adlı bir dosya oluşturun. Kimlik doğrulaması yapmak için SAS kimlik bilgisi kullanmak `New-AzureStorageContent` cmdlet'ini bizim blob kapsayıcısına dosyasını karşıya yükleyin, ardından dosyasını indirin.

```bash
echo "This is a test text file." > test.txt
```

Kullanarak Azure depolama cmdlet'leri ilk olarak, yüklediğinizden emin olun `Install-Module Azure.Storage`. Az önce oluşturduğunuz kullanarak blob karşıya yükleme `Set-AzureStorageBlobContent` PowerShell cmdlet:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT-NAME> -SasToken $sasCred
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Yanıtı:

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

Ayrıca, yeni karşıya yüklediğiniz, kullanarak blob indirebilirsiniz `Get-AzureStorageBlobContent` PowerShell cmdlet:

```powershell
Get-AzureStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Yanıtı:

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

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Bu öğreticiyi yapılacağı hakkında bilgi edinmek için bir depolama hesabı anahtarı kullanarak, bkz: [Azure Storage erişmek için bir Windows VM yönetilen hizmet kimliği kullanın](msi-tutorial-windows-vm-access-storage.md)
- Azure depolama hesabı SAS özelliği hakkında daha fazla bilgi için bkz:
  - [Paylaşılan erişim imzaları (SAS) kullanma](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Hizmet SAS oluşturma](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.


