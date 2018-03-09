---
title: "Azure Storage SAS kimlik bilgilerini kullanarak erişmek için bir Linux VM MSI kullanın"
description: "Azure depolama, depolama hesabının erişim anahtarı yerine SAS kimlik bilgilerini kullanarak erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanmayı gösterir Öğreticisi."
services: active-directory
documentationcenter: 
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: d612e71b7a765a2243be54964a56f5be7bfdc09b
ms.sourcegitcommit: 0b02e180f02ca3acbfb2f91ca3e36989df0f2d9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/07/2018
---
# <a name="use-a-linux-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Azure Storage bir SAS kimlik bilgisi erişmek için bir Linux VM yönetilen hizmet kimliği kullanın

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici bir Linux sanal makine için Yönetilen hizmet kimliği (MSI) etkinleştirmek, sonra bir depolama paylaşılan erişim imzası (SAS) kimlik bilgisi almak için MSI kullanın gösterilmektedir. Özellikle, bir [hizmet SAS kimlik bilgisi](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Hizmet SAS sınırlı bir süre ve hesap erişim anahtarı gösterme olmadan (Bu örnekte, blob hizmeti), belirli bir hizmet için bir depolama hesabındaki nesnelere sınırlı erişimi verme olanağı sağlar. Her zamanki gibi depolama SDK'yı kullanarak, örneğin depolama işlemleri yaparken bir SAS kimlik bilgisi kullanabilirsiniz. Bu öğretici için karşıya yükleme ve Azure depolama CLI kullanarak bir blob indirme göstermektedir. Şunları öğreneceksiniz nasıl yapılır:


> [!div class="checklist"]
> * Bir Linux sanal makinede MSI etkinleştir 
> * Bir depolama hesabı SAS Kaynak Yöneticisi'nde, VM erişim 
> * VM kimliğini kullanarak bir erişim belirteci alın ve Kaynak Yöneticisi'nden SAS almak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresindeki Azure portalında oturum açın.


## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için yeni bir Linux VM oluşturun. Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1. Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

    ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyutları görmek için seçin **tüm görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir

Bir sanal makine MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. Perde arkasında MSI etkinleştirme iki işlemi yapar: MSI VM uzantısı, VM yükler ve VM için Yönetilen hizmet kimliği sağlar.  

1. Yeni bir sanal makine kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. Sol taraftaki "ayarlar" VM altında tıklatın **yapılandırma**.
3. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.

    ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Denetlemek isterseniz, hangi uzantıları VM, tıklatın **uzantıları**. MSI etkinleştirilirse, **ManagedIdentityExtensionforLinux** listede görüntülenir.

    ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Zaten yoksa, şimdi bir depolama hesabı oluşturacak.  Ayrıca, bu adımı atlayın ve var olan bir depolama hesabı anahtarları, VM MSI erişim. 

1. Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. Tıklatın **depolama**, ardından **depolama hesabı**, ve yeni bir "depolama hesabı oluşturma" panelinde görüntülenir.
3. Girin bir **adı** daha sonra kullanacaksınız depolama hesabı için.  
4. **Dağıtım modeli** ve **tür hesap** "Resource manager" ve "Genel amaçlı", sırasıyla ayarlanmalıdır. 
5. Olun **abonelik** ve **kaynak grubu** VM'nizi oluşturduğunuzda önceki adımda belirttiğiniz olanlarla eşleşmesi.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabında blob kapsayıcısı oluşturma

Daha sonra biz karşıya yükleyin ve yeni depolama hesabı dosya indirme. Dosyaları blob depolama gerektirdiğinden, biz dosyasının depolanacağı bir blob kapsayıcısı oluşturmanız gerekir.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Tıklatın **kapsayıcıları** bağlantı sol panelinde, "Blob hizmeti."
3. Tıklatın **+ kapsayıcı** sayfa ve "yeni bir kapsayıcı" üst kısmında çıkış paneli slayt.
4. Kapsayıcı bir ad verin, erişim düzeyi seçin ve ardından **Tamam**. Belirtilen ad daha sonra öğreticide kullanılır. 

    ![Depolama kapsayıcısı oluşturma](media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>SAS depolama kullanmak için VM MSI erişim 

Azure Storage, Azure AD kimlik doğrulaması yerel olarak desteklemez.  Ancak, bir MSI depolama SAS Kaynak Yöneticisi'nden almanızı sonra SAS depolama erişmek için kullanın.  Bu adımda, depolama hesabınıza SAS VM MSI erişim verin.   

1. Yeni oluşturulan depolama hesabınıza geri gidin...   
2. Tıklatın **erişim denetimi (IAM)** sol panelinde bağlantı.  
3. Tıklatın **+ Ekle** VM için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Ayarlama **rol** "Depolama hesabı katkıda bulunan", sayfanın sağ tarafında için. 
5. Sonraki açılır listede ayarlamak **atamak için erişim** "Sanal makine" kaynak.  
6. Ardından, uygun abonelik listelenir olun **abonelik** açılan listesinde, daha sonra ayarlamak **kaynak grubu** "Tüm kaynak gruplarına".  
7. Son olarak, altında **seçin** açılır listede, Linux sanal makine seçin ve ardından **kaydetmek**.  

    ![Alt görüntü metin](media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>VM kimliğini kullanarak bir erişim belirteci alın ve Azure Resource Manager çağırmak için kullanın

Öğretici kalanı için size daha önce oluşturduğumuz sanal makineden çalışmaz.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/install_guide). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](../virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](../virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında gidin **sanal makineleri**gidin, Linux sanal makine, daha sonra **genel bakış** sayfasında **Bağlan** üstünde. VM'nize bağlanmak için dizesini kopyalayın. 
2. VM'nize SSH istemcisini kullanarak bağlanın.  
3. Ardından, girmek için istenir, **parola** oluştururken eklenen **Linux VM**. Ardından başarıyla oturum açmanız.  
4. Azure Resource Manager için bir erişim belirteci almak üzere CURL kullanın.  

    CURL istek ve yanıt için erişim belirteci aşağıdadır:
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true    
    ```
    
    > [!NOTE]
    > Önceki istek, "kaynak" parametresinin değeri Azure AD tarafından beklenen bir tam eşleşme olmalıdır. Azure Resource Manager kaynak kimliği'ni kullanırken eğik URI üzerinde eklemeniz gerekir.
    > Aşağıdaki yanıtı, okumanızdır kısaltılmış olarak access_token öğesi.
    
    ```bash
    {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
     ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrı yapmak için Azure Resource Manager gelen bir SAS kimlik bilgilerini alamıyor

Şimdi CURL Resource Manager depolama SAS kimlik bilgisi oluşturmak için önceki bölümde biz alınan erişim belirteci kullanarak çağırmak için kullanın. Biz SAS kimlik bilgisi olduktan sonra biz depolama yükleme/indirme işlemleri çağırabilirsiniz.

Bu istek için SAS kimlik bilgisi oluşturmak için izleme HTTP istek parametreleri kullanırız:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite.  Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Bu parametreler, SAS kimlik bilgisi iste POST gövdesinde dahil edilir. Bir SAS kimlik bilgisi oluşturmak için parametreler hakkında daha fazla bilgi için bkz: [listesi hizmet SAS REST başvurusu](/rest/api/storagerp/storageaccounts/listservicesas).

SAS kimlik bilgisi almak için aşağıdaki CURL isteği kullanın. Değiştirdiğinizden emin olun `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, `<CONTAINER NAME>`, ve `<EXPIRATION TIME>` parametre değerlerini kendi değerlere sahip. Değiştir `<ACCESS TOKEN>` daha önce erişim belirteci ile değer:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listServiceSas/?api-version=2017-06-01 -X POST -d "{\"canonicalizedResource\":\"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>\",\"signedResource\":\"c\",\"signedPermission\":\"rcw\",\"signedProtocol\":\"https\",\"signedExpiry\":\"<EXPIRATION TIME>\"}" -H "Authorization: Bearer <ACCESS TOKEN>"
```

> [!NOTE]
> Önceki URL metinde büyük küçük harfe duyarlıdır, buna göre yansıtacak şekilde büyük-küçük, kaynak grupları için kullanıyorsanız, bu nedenle olun. Ayrıca, bu bir POST isteği GET isteği olduğunu bilmeniz önemlidir.

CURL yanıt SAS kimlik bilgisi döndürür:  

```bash 
{"serviceSasToken":"sv=2015-04-05&sr=c&spr=https&st=2017-09-22T00%3A10%3A00Z&se=2017-09-22T02%3A00%3A00Z&sp=rcw&sig=QcVwljccgWcNMbe9roAJbD8J5oEkYoq%2F0cUPlgriBn0%3D"} 
```

Blob depolama kapsayıcınızı karşıya yüklemek için örnek bir blob dosyası oluşturun. Bir Linux VM şu komutla bunu yapabilirsiniz. 

```bash
echo "This is a test file." > test.txt
```

Ardından, CLI ile kimlik doğrulaması `az storage` SAS kimlik bilgilerini kullanarak komut ve blob kapsayıcısına dosyasını karşıya yükleyin. Bu adım için yapmanız gerekir [en son Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) henüz yapmadıysanız, VM üzerinde.

```azurecli-interactive
 az storage blob upload --container-name 
                        --file 
                        --name
                        --account-name 
                        --sas-token
```

Yanıtı: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-21T03:58:56+00:00"
}
```

Ayrıca, Azure CLI kullanarak ve SAS kimlik bilgileriyle kimlik doğrulaması dosyayı indirebilirsiniz. 

İsteği: 

```azurecli-interactive
az storage blob download --container-name
                         --file 
                         --name 
                         --account-name
                         --sas-token
```

Yanıtı: 

```JSON
{
  "content": null,
  "metadata": {},
  "name": "testblob",
  "properties": {
    "appendBlobCommittedBlockCount": null,
    "blobType": "BlockBlob",
    "contentLength": 16,
    "contentRange": "bytes 0-15/16",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentMd5": "Aryr///Rb+D8JQ8IytleDA==",
      "contentType": "text/plain"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D4F9929765C139\"",
    "lastModified": "2017-09-21T03:58:56+00:00",
    "lease": {
      "duration": null,
      "state": "available",
      "status": "unlocked"
    },
    "pageBlobSequenceNumber": null,
    "serverEncrypted": false
  },
  "snapshot": null
}
```

## <a name="next-steps"></a>Sonraki adımlar

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](../active-directory/msi-overview.md).
- Bu öğreticiyi yapılacağı hakkında bilgi edinmek için bir depolama hesabı anahtarı kullanarak, bkz: [Azure Storage erişmek için bir Linux VM yönetilen hizmet kimliği kullanın](msi-tutorial-linux-vm-access-storage.md)
- Azure depolama hesabı SAS özelliği hakkında daha fazla bilgi için bkz:
  - [Paylaşılan erişim imzaları (SAS) kullanma](/azure/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Hizmet SAS oluşturma](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
