---
title: Azure Storage erişmek için bir Linux VM üzerinde MSI atanmış bir kullanıcı kullanın
description: Azure depolama alanına erişmek için bir Linux VM üzerinde bir kullanıcı atanan yönetilen hizmet kimliği (MSI) kullanarak sürecinde anlatan öğretici.
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
ms.openlocfilehash: dd82f1757d9c5a5fc8fb110cc36ec9f4bbd73e8a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-storage"></a>Bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI), Azure Storage erişmek için bir Linux VM üzerinde kullanın.

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğretici oluşturup bir kullanıcı tarafından atanan yönetilen hizmet kimliği (MSI) bir Linux sanal makinenin kullanın, ardından Azure Storage erişmek için kullandığınız gösterilmektedir. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Yönetilen hizmet kimliği (MSI) atanmış bir kullanıcı oluşturun
> * Linux sanal makine için kullanıcı tarafından atanan MSI atayın
> * Azure Storage örneğine MSI erişim
> * Kullanıcı tarafından atanan MSI kimliğini kullanarak bir erişim belirteci alın ve Azure Storage erişmek için kullanın

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Bu öğreticide CLI komut dosyası örnekleri çalıştırmak için iki seçeneğiniz vardır:

- Kullanım [Azure bulut Kabuk](~/articles/cloud-shell/overview.md) Azure portalından veya "deneyin" düğmesini, aracılığıyla her kod bloğunun sağ üst köşesinde bulunan.
- [CLI 2.0'ın en son sürümünü yüklemek](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 veya sonrası) yerel CLI konsol kullanmayı tercih ederseniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

İlk olarak, yeni bir Linux VM oluşturun. İsterseniz var olan bir VM üzerinde MSI de etkinleştirebilirsiniz.

1. Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

    ![Alt görüntü metin](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyutları görmek için seçin **tüm görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="create-a-user-assigned-msi"></a>Kullanıcı tarafından atanan bir MSI oluşturma

1. CLI konsol (yerine bir Azure bulut kabuk oturumu) kullanıyorsanız, Azure'da oturum açma tarafından başlatın. Altında yeni MSI oluşturmak istediğiniz Azure aboneliğiyle ilişkili olan bir hesabı kullanın:

    ```azurecli
    az login
    ```

2. Bir kullanıcı tarafından atanan MSI kullanarak oluşturduğunuz [az kimliği oluşturma](/cli/azure/identity#az_identity_create). `-g` Parametresi, burada MSI oluşturulur, kaynak grubu belirtir ve `-n` parametresi adını belirtir. Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<MSI NAME>` parametre değerlerini kendi değerlerinizi ile:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Yanıt, kullanıcı tarafından atanan MSI oluşturulan, aşağıdaki örneğe benzer şekilde ayrıntıları içerir. Not `id` sonraki adımda kullanılacağından, MSI, değer:

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

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>Kullanıcı tarafından atanan MSI Linux VM'NİZDE atayın

Sistem tarafından atanan bir MSI, bir kullanıcı tarafından atanan MSI birden çok Azure kaynaklarına istemciler tarafından kullanılabilir. Bu öğretici için bunu tek bir VM öğesine atayın. Ayrıca birden fazla VM atayabilirsiniz.

Kullanıcı tarafından atanan MSI kullanarak, Linux VM atamak [az vm Ata-identity](/cli/azure/vm#az_vm_assign_identity). Değiştirdiğinizden emin olun `<RESOURCE GROUP>` ve `<VM NAME>` parametre değerlerini kendi değerlere sahip. Kullanım `id` özelliği döndürülen için önceki adımda `--identities` parametre değeri:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Şimdi zaten yoksa, bir depolama hesabı oluşturun. Ayrıca, bu adımı atlayın ve tercih ederseniz, varolan bir depolama hesabını kullanabilirsiniz. 

1. Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. Tıklatın **depolama**, ardından **depolama hesabı**, ve yeni bir "depolama hesabı oluşturma" panelinde görüntülenir.
3. Girin bir **adı** daha sonra kullandığınız depolama hesabı için.  
4. **Dağıtım modeli** ve **tür hesap** "Resource manager" ve "Genel amaçlı", sırasıyla ayarlanmalıdır. 
5. Olun **abonelik** ve **kaynak grubu** VM'nizi oluşturduğunuzda önceki adımda belirttiğiniz olanlarla eşleşmesi.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabında blob kapsayıcısı oluşturma

Dosya blob depolama gerektirdiğinden dosyasının depolanacağı bir blob kapsayıcısı oluşturmanız gerekir. Ardından indirin ve blob kapsayıcısında yeni depolama hesabı için bir dosya yükleyin.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Tıklatın **kapsayıcıları** "Blob hizmeti" altındaki sol bağlantı
3. Tıklatın **+ kapsayıcı** sayfa ve "yeni bir kapsayıcı" üst kısmında çıkış paneli slayt.
4. Kapsayıcı bir ad verin, erişim düzeyi seçin ve ardından **Tamam**. Belirtilen ad, daha sonra öğreticide de kullanılır. 

    ![Depolama kapsayıcısı oluşturma](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Kapsayıcı adına sonra tıklatarak yeni oluşturulan bir kapsayıcıya bir dosyayı karşıya **karşıya**, ardından bir dosya seçin ve ardından **karşıya**.

    ![Metin dosyasını karşıya yükle](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-user-assigned-msi-access-to-an-azure-storage-container"></a>Bir Azure depolama kapsayıcısı, kullanıcı atanan MSI erişim

Bir MSI kullanarak kodunuzu Azure AD kimlik doğrulamasını destekleyen kaynaklar için kimlik doğrulaması için erişim belirteçleri elde edebilirsiniz. Bu öğreticide, Azure depolama kullanın.

İlk Azure Storage kapsayıcısı MSI kimlik erişim izni. Bu durumda, daha önce oluşturduğunuz kapsayıcı kullanın. İçin değerleri güncelleştirmek `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, ve `<CONTAINER NAME>` ortamınız için uygun şekilde. Ayrıca, yerine `<MSI PRINCIPALID>` ile `principalId` özellik tarafından döndürülen `az identity create` komutunu [kullanıcı tarafından atanan bir MSI oluşturmak](#create-a-user-assigned-msi):

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

## <a name="get-an-access-token-using-the-user-assigned-msis-identity-and-use-it-to-call-azure-storage"></a>Kullanıcı tarafından atanan MSI kimliğini kullanarak bir erişim belirteci alın ve Azure Storage çağırmak için kullanın

Öğretici kalanı için daha önce oluşturduğunuz sanal makineden çalışması gerekir.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/about). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](~/articles/virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında gidin **sanal makineleri**gidin, Linux sanal makine, daha sonra **genel bakış** sayfasında **Bağlan** üstünde. VM'nize bağlanmak için dizesini kopyalayın.
2. **Connect** tercih ettiğiniz SSH istemcisi ile VM. 
3. Terminal penceresinde CURL, kullanarak Azure Storage için bir erişim belirteci almak üzere yerel MSI uç nokta için bir isteği oluşturun.

   Bir erişim belirteci almak üzere CURL isteği aşağıdaki örnekte gösterilir. Değiştirdiğinizden emin olun `<CLIENT ID>` ile `clientId` özellik tarafından döndürülen `az identity create` komutunu [kullanıcı tarafından atanan bir MSI oluşturmak](#create-a-user-assigned-msi):
   
   ```bash
   curl -H Metadata:true "http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com/&client_id=<MSI CLIENT ID>" 
   ```

   > [!NOTE]
   > Değerini önceki isteğindeki `resource` parametresi, Azure AD tarafından beklenen bir tam eşleşme olmalıdır. Azure Storage kaynak kimliği'ni kullanırken eğik URI üzerinde eklemeniz gerekir.
   > Aşağıdaki yanıtı, okumanızdır kısaltılmış olarak access_token öğesi.

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
   "refresh_token":"",
   "expires_in":"3599",
   "expires_on":"1504130527",
   "not_before":"1504126627",
   "resource":"https://storage.azure.com",
   "token_type":"Bearer"}
   ```

4. Şimdi, örneğin, daha önce kapsayıcıya karşıya örnek dosyanın içeriğini okumak Azure Storage erişmek için erişim belirteci kullanın. Değerlerini değiştirmek `<STORAGE ACCOUNT>`, `<CONTAINER NAME>`, ve `<FILE NAME>` daha önce belirttiğiniz değerleri içeren ve `<ACCESS TOKEN>` önceki adımda döndürülen belirteci ile.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Yanıt dosyasının içeriğini içerir:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Sonraki adımlar

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Depolama SAS kimlik bilgilerini kullanarak bu öğreticiyi yapmak öğrenmek için bkz: [bir SAS kimlik bilgisi Azure depolama erişmek için bir Linux VM yönetilen hizmet kimliği kullanın](msi-tutorial-linux-vm-access-storage-sas.md)
- Azure depolama hesabı SAS özelliği hakkında daha fazla bilgi için bkz:
  - [Paylaşılan erişim imzaları (SAS) kullanma](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Hizmet SAS oluşturma](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.





