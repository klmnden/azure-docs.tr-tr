---
title: Azure depolamaya erişmek için bir Windows VM MSI kullanma
description: Öğretici Azure depolamaya erişmek için bir Windows VM yönetilen hizmet kimliği (MSI) kullanma işlemi gösterilmektedir.
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
ms.openlocfilehash: 81b508661ac7195f690739fe7961691ddbedc9b0
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049369"
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-access-key"></a>Azure depolama erişim anahtarı erişmek için bir Windows VM yönetilen hizmet Kimliği'ni kullanın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Windows sanal makinesi için Yönetilen hizmet kimliği (MSI) etkinleştirin ve ardından depolama hesabı erişim anahtarlarını almak için bu kimliği kullanın gösterilmektedir. Depolama erişim anahtarlarını zamanki depolama SDK'sı kullanırken, örneğin, depolama işlemleri yaparken kullanabilirsiniz. Bu öğretici için karşıya yükleme ve Azure Storage PowerShell kullanarak blobları indirin. Şunların nasıl yapılır:


> [!div class="checklist"]
> * Bir Windows sanal makinesinde MSI etkinleştir 
> * Depolama hesabı erişim anahtarlarını Kaynak Yöneticisi'nde, VM erişimi verme 
> * Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve Kaynak Yöneticisi'nden depolama erişim anahtarlarını almak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

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

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>VM'nizde MSI'yi etkinleştirme

Sanal Makine MSI'si kodunuza kimlik bilgileri yerleştirmeniz gerekmeden Azure AD'den erişim belirteçlerini almanıza olanak tanır. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve sanal makine için MSI sağlar.  

1. Yeni sanal makinenize kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. Sol taraftaki "ayarlar" VM altında tıklayın **yapılandırma**.
3. MSI'yi kaydetmek ve etkinleştirmek için **Evet**'i seçin, devre dışı bırakmak istiyorsanız Hayır'ı seçin.
4. Yapılandırmayı kaydetmek için **Kaydet**’e tıkladığınızdan emin olun.

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Hangi uzantıların denetlemek istiyorsanız, sanal makinede olan, tıklayın **uzantıları**. MSI etkin olduğunda **ManagedIdentityExtensionforWindows** listesinde görünür.

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Zaten yoksa, artık bir depolama hesabı oluşturur. Ayrıca, bu adımı atlayıp mevcut bir depolama hesabı anahtarları, VM MSI erişimi verme. 

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
2. Tıklayın **kapsayıcıları** sol tarafında "Blob hizmeti" altında bağlantı
3. Tıklayın **+ kapsayıcı** sayfasına ve "yeni bir kapsayıcı" üst kısmındaki çıkış paneli slaytlar.
4. Kapsayıcıya bir ad verin, bir erişim düzeyi seçin ve ardından tıklayın **Tamam**. Belirttiğiniz adı öğreticinin ilerleyen bölümlerinde kullanılacaktır. 

    ![Depolama kapsayıcısı oluşturma](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-storage-account-access-keys"></a>Depolama hesabı erişim anahtarlarını kullanmak için sanal makinenizin MSI erişimi verme 

Azure depolama, yerel olarak Azure AD kimlik doğrulamasını desteklemez.  Ancak, Kaynak Yöneticisi'nden depolama hesabı erişim anahtarlarını almak için bir MSI kullanma ardından depolamaya erişmek için bir anahtar kullanın.  Bu adımda, depolama hesabınıza anahtarlara, VM MSI erişimi verme.   

1. Yeni oluşturulan depolama hesabına geri gidin.  
2. Tıklayın **erişim denetimi (IAM)** sol bölmede bağlantı.  
3. Tıklayın **+ Ekle** VM'niz için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Ayarlama **rol** "Depolama hesabı anahtarı işleci hizmet rolü", sayfanın sağ tarafındaki için. 
5. Sonraki açılır menüden ayarlamak **erişim Ata** "Sanal makinesi" kaynak.  
6. Ardından, uygun abonelik listelendiğinden emin olmak **abonelik** açılır listesinde, ardından ayarlayın **kaynak grubu** "Tüm kaynak gruplarına".  
7. Son olarak, altında **seçin** açılır menüde, Windows sanal makinenizi seçin, ardından tıklayın **Kaydet**. 

    ![Alternatif resim metni](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve Azure Resource Manager'ı çağırmak için kullanın 

Bu öğreticinin geri kalanında için daha önce oluşturduğumuz VM'den çalışacağız. 

Bu bölümünde Azure Resource Manager PowerShell cmdlet'lerini kullanmanız gerekecektir.  Yüklü, yoksa [son sürümünü indirin](https://docs.microsoft.com/powershell/azure/overview) devam etmeden önce.

1. Azure portalında gidin **sanal makineler**gidin Windows sanal makinenizi, sonra **genel bakış** sayfasında **Connect** en üstünde. 
2. Windows VM'sini oluştururken eklendiğiniz hesabın **Kullanıcı adı** ve **Parola** değerlerini girin. 
3. Artık sanal makineyle **Uzak Masaüstü Bağlantısı**'nı oluşturduğunuza göre, uzak oturumda PowerShell'i açın.
4. PowerShell'in Invoke-WebRequest kullanarak, Azure Resource Manager için bir erişim belirteci almak için yerel MSI uç noktasına bir istek olun.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
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
 
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrıları yapmak için Azure Resource Manager'dan depolama hesabı erişim anahtarlarını alma  

Artık Kaynak Yöneticisi'ni önceki bölümde, biz alınan erişim belirteci kullanarak çağırmak için PowerShell kullanma depolama erişim anahtarı alınamadı. Biz depolama erişim anahtarı oluşturduktan sonra depolama karşıya yükleme/indirme işlemleri diyoruz.

```powershell
$keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
> [!NOTE] 
> URL büyük/küçük harfe duyarlıdır, dolayısıyla sağlamak büyük harf "G" içinde "resourceGroups." içeren kaynak grubunu, adlı daha önce kullanılan tam aynı çalışması kullanın 

```powershell
$keysContent = $keysResponse.Content | ConvertFrom-Json
$key = $keysContent.keys[0].value
```

Ardından "test.txt" adlı bir dosya oluşturun. Depolama erişim anahtarı ile kimlik doğrulaması kullanacağınızı `New-AzureStorageContent` cmdlet'i, dosyanın bizim blob kapsayıcısına yükleyin ve ardından dosyayı indirin.

```bash
echo "This is a test text file." > test.txt
```

Azure depolama cmdlet'leri ilk olarak, yüklediğinizden emin olun'ı kullanarak `Install-Module Azure.Storage`. Ardından az önce oluşturduğunuz kullanarak blob karşıya `Set-AzureStorageBlobContent` PowerShell cmdlet:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT> -StorageAccountKey $key
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
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
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```


## <a name="related-content"></a>İlgili içerik

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Bir depolama SAS kimlik bilgisi kullanarak bu öğreticiyi yapma hakkında bilgi için bkz: [bir SAS kimlik bilgisi Azure depolamaya erişmek için bir Windows VM yönetilen hizmet kimliği kullan](msi-tutorial-windows-vm-access-storage-sas.md)
- Azure depolama hesabı SAS özelliği hakkında daha fazla bilgi için bkz:
  - [Paylaşılan erişim imzaları (SAS) kullanma](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Hizmet SAS oluşturma](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın


