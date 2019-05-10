---
title: Azure Depolama’ya erişmek için Windows VM sistem tarafından atanan yönetilen kimliği kullanma
description: Windows VM üzerinde bir sistem tarafından atanmış yönetilen kimlik kullanarak Azure Depolama'ya erişme işleminde size yol gösteren bir öğretici.
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
ms.date: 01/24/2019
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 147ee2450a6a67f8ca02149105533401d038a53a
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65191091"
---
# <a name="tutorial-use-a-windows-vm-system-assigned-managed-identity-to-access-azure-storage-via-access-key"></a>Öğretici: Azure depolama erişim anahtarı erişmek için bir Windows VM sistem tarafından atanan yönetilen kimliği kullanma

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]


> [!IMPORTANT] 
> Azure depolama artık Azure AD kimlik doğrulamasını destekler. En iyi uygulama, kullanın [Azure AD kimlik doğrulaması](tutorial-vm-windows-access-storage.md) erişim anahtarları yerine. 


Bu öğreticide, depolama hesabı erişim anahtarlarını almak amacıyla, Windows sanal makinesi (VM) için sistem tarafından atanmış yönetilen bir kimliği nasıl kullanacağınız gösterilmektedir. Depolama işlemleri yaparken, örneğin Depolama SDK'sını kullanırken depolama erişim anahtarlarını olağan şekilde kullanabilirsiniz. Bu öğreticide, Azure Depolama PowerShell kullanarak blob'ları karşıya yüklüyor ve indiriyoruz. Şunları öğrenirsiniz:


> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * VM’inize Resource Manager’da yer alan depolama hesabı erişim anahtarı için erişim verme 
> * VM'nizin kimliğini kullanarak erişim belirteci alma ve Resource Manager'dan depolama erişim anahtarlarını almak için bu belirteci kullanma 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

[!INCLUDE [updated-for-az.md](../../../includes/updated-for-az.md)]

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Henüz bir depolama hesabınız yoksa, şimdi oluşturacaksınız. Ayrıca bu adımı atlayabilir ve VM sistem tarafından atanan yönetilen kimliğinize mevcut depolama hesabının anahtarları için erişim verebilirsiniz. 

1. Azure portalın sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2. **Depolama**'ya ve **Depolama Hesabı**'na tıklayın; yeni bir "Depolama hesabı oluştur" paneli görüntülenir.
3. Daha sonra kullanacağınız depolama hesabı için bir ad girin.  
4. **Dağıtım modeli** ve **Hesap türü** sırasıyla "Kaynak yöneticisi" ve "Genel amaçlı" olarak ayarlanmalıdır. 
5. **Abonelik** ve **Kaynak Grubu** değerlerinin, önceki adımda VM'nizi oluştururken belirttiklerinizle eşleştiğinden emin olun.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabında bir blob kapsayıcısı oluşturma

Daha sonra yeni depolama hesabına dosya yükleyecek ve indireceğiz. Dosyalar için blob depolaması gerektiğinden, dosyanın depolanacağı bir blob kapsayıcısı oluşturmalıyız.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Sol tarafta, "Blob hizmeti" öğesinin altındaki **Kapsayıcılar** bağlantısına tıklayın.
3. Sayfanın en üstündeki **+ Kapsayıcı**'ya tıklayın; "Yeni kapsayıcı" paneli belirir.
4. Kapsayıcıya bir ad verin, erişim düzeyini seçin ve ardından **Tamam**'a tıklayın. Belirttiğiniz ad bu öğreticide daha sonra kullanılacaktır. 

    ![Depolama kapsayıcısı oluşturma](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-system-assigned-managed-identity-access-to-use-storage-account-access-keys"></a>Depolama hesabı erişim anahtarlarını kullanmak için VM'nize sistem tarafından atanan yönetilen kimliği erişimi verme 

Azure Depolama Azure AD kimlik doğrulamayı yerel olarak desteklemez.  Bununla birlikte, Resource Manager'dan depolama hesabı erişim anahtarını almak için VM’nizin sistem tarafından atanan yönetilen kimliğini kullanabilir ve ardından anahtarı kullanarak depolamaya erişebilirsiniz.  Bu adımda, VM sistem tarafından atanan yönetilen kimliğinize depolama hesabının anahtarları için erişim verirsiniz.   

1. Yeni oluşturulan depolama hesabınıza geri gidin.  
2. Sol bölmedeki **Erişim denetimi (IAM)** bağlantısına tıklayın.  
3. Tıklayın **+ rol ataması Ekle** VM'niz için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Sayfanın sağ tarafında, **Rol** olarak  "Depolama Hesabı Anahtarı İşleci Hizmet Rolü" seçeneğini ayarlayın. 
5. Sonraki açılan listede **Erişimin atanacağı hedef** olarak "Sanal Makine" seçeneğini ayarlayın.  
6. Ardından, uygun aboneliğin **Abonelik**’te listelendiğinden emin olun ve sonra **Kaynak Grubu**’nu "Tüm kaynak grupları" olarak ayarlayın.  
7. Son olarak, **Seç**'in altındaki açılan listede Windows Sanal Makinenizi seçin ve **Kaydet**'e tıklayın. 

    ![Alternatif resim metni](./media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-system-assigned-managed-identity-to-call-azure-resource-manager"></a>VM’nin sistem tarafından atanan yönetilen kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapma 

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğumuz VM'den çalışacağız. 

Bu bölümde Azure Resource Manager PowerShell cmdlet’lerini kullanmanız gerekir.  Bunu yüklemediyseniz, devam etmeden önce [en son sürümünü indirin](https://docs.microsoft.com/powershell/azure/overview).

1. Azure portalında **Sanal Makineler**'e gidin, Windows sanal makinenize gidin ve ardından **Genel Bakış** sayfasında üst kısımdaki **Bağlan**'a tıklayın. 
2. Windows VM'sini oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin. 
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın.
4. PowerShell’in Invoke-WebRequest komutunu kullanarak, Azure kaynakları uç noktası için yerel yönetilen kimliğe Azure Resource Manager için erişim belirteci alma isteğinde bulunun.

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
 
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrıları yapmak için Azure Resource Manager'dan depolama hesabı erişim anahtarları alma  

Şimdi depolama erişim anahtarını almak için önceki bölümde aldığımız erişim belirtecini kullanarak Resource Manager'ı çağırmak için PowerShell kullanın. Depolama erişim anahtarımızı aldıktan sonra, depolama karşıya yükleme/indirme işlemlerini çağırabiliriz.

```powershell
$keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
> [!NOTE] 
> URL büyük/küçük harfe duyarlıdır; dolayısıyla daha önce Kaynak Grubunu adlandırırken kullandığınız büyük/küçük harf düzenini kullanmaya dikkat edin ("resourceGroups" adındaki büyük "G" harfi dahil). 

```powershell
$keysContent = $keysResponse.Content | ConvertFrom-Json
$key = $keysContent.keys[0].value
```

Sonra "test.txt" adlı bir dosya oluştururuz. Ardından depolama erişim anahtarını kullanarak `New-AzStorageContent` cmdlet'iyle kimlik doğrulaması yapın, dosyayı blob kapsayıcımıza yükleyin ve sonra dosyayı indirin.

```bash
echo "This is a test text file." > test.txt
```

Önce, `Install-Module Az.Storage` kullanarak Azure Depolama cmdlet'lerini yüklediğinizden emin olun. Sonra `Set-AzStorageBlobContent` PowerShell cmdlet'ini kullanarak yeni oluşturduğunuz blobu karşıya yükleyin:

```powershell
$ctx = New-AzStorageContext -StorageAccountName <STORAGE-ACCOUNT> -StorageAccountKey $key
Set-AzStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Yanıt:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

Ayrıca `Get-AzStorageBlobContent` PowerShell cmdlet'ini kullanarak yeni karşıya yüklediğiniz blobu da indirebilirsiniz:

```powershell
Get-AzStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Yanıt:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, erişim anahtarı kullanarak Azure Depolama'ya erişmek için sistem tarafından atanan yönetilen kimlik oluşturmayı öğrendiniz.  Azure Depolama erişim anahtarları hakkında daha fazla bilgi için bkz:

> [!div class="nextstepaction"]
>[Depolama erişim anahtarlarınızı yönetme](/azure/storage/common/storage-create-storage-account)

