---
title: Azure depolamaya erişmek için bir kullanıcı tarafından atanan bir Linux VM MSI kullanma
description: Öğretici Azure depolamaya erişmek için bir Linux VM'de bir kullanıcıya atanmış yönetilen hizmet kimliği (MSI) kullanma işlemi gösterilmektedir.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: arluca
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 4a1a2d0c40012649f6cd89193fd3f704f325e38a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611053"
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-storage"></a>Bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI), Azure depolama alanına erişmek için bir Linux VM üzerinde kullanın.

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide oluşturmak ve bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) gelen bir Linux sanal makinesini kullanın ve ardından Azure Depolama'ya erişmek için bunu kullanmaya gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yönetilen hizmet kimliği (MSI) atanmış bir kullanıcı oluşturun
> * Linux sanal makinesi için kullanıcı tarafından atanan MSI atayın
> * Azure depolama örneğini MSI erişimi verme
> * Kullanıcı tarafından atanan MSI kimlik kullanarak bir erişim belirteci alma ve Azure depolamaya erişmek için kullanın

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Bu öğreticide CLI betiği örnekleri çalıştırmak için iki seçeneğiniz vardır:

- Kullanım [Azure Cloud Shell](~/articles/cloud-shell/overview.md) Azure portalından veya "Try It" düğmesi aracılığıyla, her kod bloğunun sağ üst köşesinde bulunur.
- [CLI 2. 0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya sonraki) yerel bir CLI konsol kullanmak istiyorsanız.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makinesi oluşturma

İlk olarak, yeni bir Linux VM oluşturun. Dilerseniz de mevcut VM'deki MSI etkinleştirebilirsiniz.

1. Tıklayın **+/ yeni hizmet oluşturma** düğmesi Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarı** veya **parola**. Oluşturulan kimlik bilgilerini, VM'de oturum açmak izin verin.

    ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makinenin açılır.
5. Yeni bir seçilecek **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. Sanal makine için boyutu seçin. Daha fazla boyut görmek için seçin **tümünü görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="create-a-user-assigned-msi"></a>Kullanıcı tarafından atanan bir MSI oluşturma

1. CLI konsol (yerine bir Azure Cloud Shell oturumu) kullanıyorsanız Azure'da oturum açma tarafından başlatın. Altında yeni MSI oluşturmak istediğiniz Azure aboneliği ile ilişkili olan bir hesabı kullanın:

    ```azurecli
    az login
    ```

2. Bir kullanıcı tarafından atanan MSI kullanarak oluşturduğunuz [az kimliği oluşturma](/cli/azure/identity#az_identity_create). `-g` Parametresi MSI oluşturulduğu, kaynak grubunu belirtir ve `-n` parametre adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizle:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Yanıt, kullanıcı tarafından atanan MSI oluşturulan, aşağıdaki örneğe benzer ayrıntıları içerir. Not `id` sonraki adımda kullanılacağından, bir MSI için değer:

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "9012",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Linux VM'nize, kullanıcı tarafından atanan MSI atayın

Sistem tarafından atanan bir MSI, kullanıcı tarafından atanan bir MSI birden çok Azure kaynaklarına istemciler tarafından kullanılabilir. Bu öğretici için bunu tek bir VM'ye atayın. Ayrıca birden fazla VM'ye atayabilirsiniz.

Kullanıcı tarafından atanan MSI kullanarak Linux VM'nize atama [az vm Ata-identity](/cli/azure/vm#az-vm-identity-assign). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlerinizle. Kullanım `id` özelliği döndürülen için önceki adımda `--identities` parametre değeri:

```azurecli-interactive
az vm identity assign -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Zaten yoksa, artık bir depolama hesabı oluşturun. Ayrıca, bu adımı atlayın ve tercih ederseniz, mevcut bir depolama hesabını kullanabilirsiniz. 

1. Tıklayın **+/ yeni hizmet oluşturma** düğmesi Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. Tıklayın **depolama**, ardından **depolama hesabı**, ve "depolama hesabı oluştur" yeni bir panel görüntüler.
3. Girin bir **adı** depolama hesabı, daha sonra kullanmak için.  
4. **Dağıtım modeli** ve **hesap türü** sırasıyla "Resource manager" ve "Genel amaç" için ayarlanmalıdır. 
5. Olun **abonelik** ve **kaynak grubu** VM'nizi oluşturduğunuzda önceki adımda belirttiğiniz olanlarla eşleşmesi.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabındaki bir blob kapsayıcısı oluşturma

Dosyaları blob depolama gerektirdiğinden dosyasının depolanacağı bir blob kapsayıcısı oluşturmak gerekir. Daha sonra karşıya yükleme ve yeni depolama hesabındaki blob kapsayıcısına bir dosya indirin.

1. Yeni oluşturulan depolama hesabına geri gidin.
2. Tıklayın **kapsayıcıları** sol tarafında "Blob hizmeti" altında bağlantı
3. Tıklayın **+ kapsayıcı** sayfasına ve "yeni bir kapsayıcı" üst kısmındaki çıkış paneli slaytlar.
4. Kapsayıcıya bir ad verin, bir erişim düzeyi seçin ve ardından tıklayın **Tamam**. Belirtilen ad aynı zamanda öğreticinin ilerleyen bölümlerinde kullanılır. 

    ![Depolama kapsayıcısı oluşturma](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Ardından kapsayıcı adına tıklayarak yeni oluşturduğunuz kapsayıcıya bir dosya yüklemek **karşıya**, bir dosyayı seçtikten sonra tıklayın **karşıya**.

    ![Metin dosyasını karşıya yükleyin](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-user-assigned-msi-access-to-an-azure-storage-container"></a>Bir Azure depolama kapsayıcısı için kullanıcı atanan MSI erişimi verme

MSI kullanarak kodunuzu Azure AD kimlik doğrulamasını destekleyen kaynakların kimliğini doğrulamak için erişim belirteçleri elde edebilirsiniz. Bu öğreticide, Azure depolama kullanın.

Öncelikle bir Azure depolama kapsayıcısı MSI kimlik erişimi verin. Bu durumda, daha önce oluşturduğunuz kapsayıcıya kullanın. İçin değerleri güncelleştirmek `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, ve `<CONTAINER NAME>` ortamınız için uygun şekilde. Ayrıca, değiştirin `<MSI PRINCIPALID>` ile `principalId` özelliği tarafından döndürülen `az identity create` komutunu [kullanıcı tarafından atanan bir MSI oluşturma](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role 'Reader' --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/blobServices/default/containers/<CONTAINER NAME>"
```

Yanıt oluşturulan rol ataması ayrıntılarını içerir:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.storage/storageAccounts/<STORAGE ACCOUNT NAME>/blogServices/default/<CONTAINER NAME>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-user-assigned-msis-identity-and-use-it-to-call-azure-storage"></a>Kullanıcı tarafından atanan MSI kimlik kullanarak bir erişim belirteci alma ve Azure depolama çağırmak için kullanın

Bu öğreticinin geri kalanında için daha önce oluşturduğunuz sanal makineden çalışması gerekir.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinizin anahtarları yapılandırılıyor yardıma ihtiyacınız varsa bkz [azure'da Windows ile SSH kullanma anahtarları nasıl](~/articles/virtual-machines/linux/ssh-from-windows.md), veya [oluşturmak ve azure'da Linux VM'ler için SSH ortak ve özel anahtar çifti kullanmak nasıl](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında gidin **sanal makineler**gidin, Linux sanal makinesi, ardından **genel bakış** sayfasında **Connect** en üstünde. VM'nize bağlanmak için dizesini kopyalayın.
2. **Connect** VM'ye SSH istemcisine sahip. 
3. Terminal penceresinde CURL, kullanarak Azure depolama için bir erişim belirteci almak için yerel MSI uç noktasına bir istek olun.

   CURL isteği bir erişim belirteci almak için aşağıdaki örnekte gösterilir. Değiştirdiğinizden emin olun `<CLIENT ID>` ile `clientId` özelliği tarafından döndürülen `az identity create` komutunu [kullanıcı tarafından atanan bir MSI oluşturma](#create-a-user-assigned-msi):
   
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com/&client_id=<MSI CLIENT ID>" 
   ```

   > [!NOTE]
   > Değerini önceki isteğindeki `resource` parametresi, Azure AD tarafından beklenen değer için bir tam eşleşme olmalıdır. Azure depolama kaynak kimliği kullanıldığında, URI üzerinde sonunda eğik çizgi içermelidir.
   > Şu yanıtı, konuyu uzatmamak amacıyla kısalttık olarak access_token öğesi.

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
   "refresh_token":"",
   "expires_in":"3599",
   "expires_on":"1504130527",
   "not_before":"1504126627",
   "resource":"https://storage.azure.com",
   "token_type":"Bearer"}
   ```

4. Örneğin kapsayıcıya daha önce yüklenmiş örnek dosyanın içeriğini okumak Azure depolama alanına erişmek için artık erişim belirtecini kullanın. Değerleri Değiştir `<STORAGE ACCOUNT>`, `<CONTAINER NAME>`, ve `<FILE NAME>` daha önce belirttiğiniz değerleri ve `<ACCESS TOKEN>` önceki adımda döndürülen belirteci ile.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Yanıt, dosyanın içeriğini içerir:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Sonraki adımlar

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Bir depolama SAS kimlik bilgisi kullanarak bu öğreticiyi yapma hakkında bilgi için bkz: [bir SAS kimlik bilgisi Azure depolamaya erişmek için bir Linux VM yönetilen hizmet kimliği kullan](msi-tutorial-linux-vm-access-storage-sas.md)
- Azure depolama hesabı SAS özelliği hakkında daha fazla bilgi için bkz:
  - [Paylaşılan erişim imzaları (SAS) kullanma](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Hizmet SAS oluşturma](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.





