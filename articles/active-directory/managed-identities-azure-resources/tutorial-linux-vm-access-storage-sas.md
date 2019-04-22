---
title: SAS kimlik bilgisini kullanarak Azure Depolama'ya erişmek için Linux VM sistem tarafından atanan yönetilen kimliğini kullanma
description: Depolama hesabı erişim anahtarı yerine SAS kimlik bilgilerini kullanarak Azure Depolama'ya erişmek için Linux VM sistem tarafından atanan yönetilen kimliğini kullanma işlemini gösteren bir öğretici.
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
ms.date: 11/20/2017
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: 06fa483a34efa3a9486e04d894a3139d17b157b4
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "59273966"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-identity-to-access-azure-storage-via-a-sas-credential"></a>Öğretici: Azure depolama bir SAS kimlik bilgisi erişmek için bir Linux VM sistem tarafından atanan kimliği kullanın.

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğreticide depolama alanı Paylaşılan Erişim İmzası (SAS) kimlik bilgilerini almak üzere bir Linux sanal makinesi (VM) için sistem tarafından atanan yönetilen bir kimliği nasıl kullanacağınız gösterilmektedir. Özellikle, bir [Hizmet SAS kimlik bilgileri](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

> [!NOTE]
> Bu öğreticide oluşturulan SAS anahtarı kısıtlı/VM'ye bağlı değil olmalı.  

Hizmet SAS, bir hesap erişim anahtarı göstermeden sınırlı bir süre boyunca ve belirli bir hizmet için (bizim durumumuzda blob hizmeti) depolama hesabında yer alan nesnelere sınırlı erişim vermeye olanağı tanır. Depolama işlemleri yaparken, örneğin Depolama SDK'sını kullanırken SAS kimlik bilgilerini olağan şekilde kullanabilirsiniz. Bu öğreticide, Azure Depolama CLI kullanarak bir blobu karşıya yükleme ve indirme işlemini göstereceğiz. Şunları öğrenirsiniz:


> [!div class="checklist"]
> * Depolama hesabı oluşturma
> * Depolama hesabında bir blob kapsayıcısı oluşturma
> * VM'nize Resource Manager'da yer alan depolama hesabı SAS için erişim verme 
> * VM'nizin kimliğini kullanarak erişim belirteci alma ve Resource Manager'dan SAS almak için bu belirteci kullanma 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Henüz bir depolama hesabınız yoksa, şimdi oluşturacaksınız.  Ayrıca bu adımı atlayabilir ve VM sistem tarafından atanan yönetilen kimliğinize mevcut depolama hesabının anahtarları için erişim verebilirsiniz. 

1. Azure portalın sol üst köşesinde bulunan **+/Yeni hizmet oluştur** düğmesine tıklayın.
2. **Depolama**'ya ve **Depolama Hesabı**'na tıklayın; yeni bir "Depolama hesabı oluştur" paneli görüntülenir.
3. Daha sonra kullanacağınız depolama hesabı için bir **Ad** girin.  
4. **Dağıtım modeli** ve **Hesap türü** sırasıyla "Kaynak yöneticisi" ve "Genel amaçlı" olarak ayarlanmalıdır. 
5. **Abonelik** ve **Kaynak Grubu** değerlerinin, önceki adımda VM'nizi oluştururken belirttiklerinizle eşleştiğinden emin olun.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluşturma](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabında bir blob kapsayıcısı oluşturma

Daha sonra yeni depolama hesabına dosya yükleyecek ve indireceğiz. Dosyalar için blob depolaması gerektiğinden, dosyanın depolanacağı bir blob kapsayıcısı oluşturmalıyız.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Sol tarafta, "Blob hizmeti" öğesinin altındaki **Kapsayıcılar** bağlantısına tıklayın.
3. Sayfanın en üstündeki **+ Kapsayıcı**'ya tıklayın; "Yeni kapsayıcı" paneli belirir.
4. Kapsayıcıya bir ad verin, erişim düzeyini seçin ve ardından **Tamam**'a tıklayın. Belirttiğiniz ad bu öğreticide daha sonra kullanılacaktır. 

    ![Depolama kapsayıcısı oluşturma](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-system-assigned-managed-identity-access-to-use-a-storage-sas"></a>Depolama SAS değerini kullanmak için VM'nize sistem tarafından atanan yönetilen kimliği erişimi verme 

Azure Depolama Azure AD kimlik doğrulamayı yerel olarak desteklemez.  Bununla birlikte, Resource Manager'dan depolama SAS almak için VM'nizin sistem tarafından atanan yönetilen kimliğini kullanabilir ve ardından SAS kullanarak depolamaya erişebilirsiniz.  Bu adımda, VM sistem tarafından atanan yönetilen kimliğinize depolama hesabının SAS değeri için erişim verirsiniz.   

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Sol bölmedeki **Erişim denetimi (IAM)** bağlantısına tıklayın.  
3. Tıklayın **+ rol ataması Ekle** VM'niz için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Sayfanın sağ tarafında, **Rol** olarak "Depolama Hesabı Katılımcısı" seçeneğini ayarlayın. 
5. Sonraki açılan listede **Erişimin atanacağı hedef** olarak "Sanal Makine" seçeneğini ayarlayın.  
6. Ardından, uygun aboneliğin **Abonelik**’te listelendiğinden emin olun ve sonra **Kaynak Grubu**’nu "Tüm kaynak grupları" olarak ayarlayın.  
7. Son olarak, **Seç**'in altındaki açılan listede Linux Sanal Makinenizi seçin ve **Kaydet**'e tıklayın.  

    ![Alternatif resim metni](./media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>VM kimliğini kullanarak erişim belirteci alma ve Azure Resource Manager çağrısı yapmak için bunu kullanma

Bu öğreticinin kalan bölümünde, daha önce oluşturmuş olduğumuz VM'den çalışacağız.

Bu adımları tamamlamak için bir SSH istemciniz olmalıdır. Windows kullanıyorsanız, [Linux için Windows Alt Sistemi](https://msdn.microsoft.com/commandline/wsl/install_guide)'ndeki SSH istemcisini kullanabilirsiniz. SSH istemcinizin anahtarlarını yapılandırmak için yardıma ihtiyacınız olursa, bkz. [Azure'da Windows ile SSH anahtarlarını kullanma](../../virtual-machines/linux/ssh-from-windows.md) veya [Azure’da Linux VM’ler için SSH ortak ve özel anahtar çifti oluşturma](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında **Sanal Makineler**'e gidin, Linux sanal makinenize gidin ve ardından **Genel Bakış** sayfasında üst kısımdaki **Bağlan**'a tıklayın. VM'nize bağlanma dizesini kopyalayın. 
2. SSH istemcinizi kullanarak VM'nize bağlanın.  
3. Ardından, **Linux VM'sini** oluştururken eklediğiniz **Parolanızı** girmeniz istenecektir. Bundan sonra başarıyla oturum açabilmeniz gerekir.  
4. Azure Resource Manager'ın erişim anahtarını almak için CURL kullanın.  

    Erişim belirteci için CURL istek ve yanıtı aşağıda yer alır:
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true    
    ```
    
    > [!NOTE]
    > Önceki istekte, "resource" parametresinin değeri Azure AD'nin beklediği değerle tam olarak eşleşmelidir. Azure Resource Manager kaynak kimliği kullanıldığında, URI'nin sonundaki eğik çizgiyi de eklemelisiniz.
    > Aşağıdaki yanıtta, access_token öğesi kısaltılmıştır.
    
    ```bash
    {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
     ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrıları yapmak için Azure Resource Manager'dan SAS kimlik bilgileri alma

Şimdi bir depolama SAS kimliği oluşturmak için önceki bölümde aldığımız erişim belirtecini kullanarak Resource Manager'ı çağırmak için CURL kullanın. SAS kimlik bilgilerini aldıktan sonra, depolama karşıya yükleme/indirme işlemlerini çağırabiliriz.

Bu istek için, aşağıdaki HTTP istek parametrelerini kullanarak SAS kimlik bilgilerini oluşturacağız:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite.  Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Bu parametreler SAS kimlik bilgileri için isteğin POST gövdesine eklenmiştir. SAS kimliği oluştururken kullanılan parametreler hakkında daha fazla bilgi için bkz. [Liste Hizmeti SAS REST başvurusu](/rest/api/storagerp/storageaccounts/listservicesas).

SAS kimlik bilgilerini almak için aşağıdaki CURL isteğini kullanın. `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, `<CONTAINER NAME>` ve `<EXPIRATION TIME>` parametre değerlerini kendi değerlerinizle değiştirmeyi unutmayın. `<ACCESS TOKEN>` değerini daha önce aldığınız erişim belirteciyle değiştirin:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listServiceSas/?api-version=2017-06-01 -X POST -d "{\"canonicalizedResource\":\"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>\",\"signedResource\":\"c\",\"signedPermission\":\"rcw\",\"signedProtocol\":\"https\",\"signedExpiry\":\"<EXPIRATION TIME>\"}" -H "Authorization: Bearer <ACCESS TOKEN>"
```

> [!NOTE]
> Önceki URL'nin metni büyük/küçük harfe duyarlıdır; bu nedenle Kaynak Gruplarınız için büyük/küçük harf kullanımının bunu düzgün yansıttığından emin olun. Ayrıca, bunun bir GET isteği değil POST isteği olduğunu bilmek de önemlidir.

CURL yanıtı SAS kimlik bilgilerini döndürür:  

```bash 
{"serviceSasToken":"sv=2015-04-05&sr=c&spr=https&st=2017-09-22T00%3A10%3A00Z&se=2017-09-22T02%3A00%3A00Z&sp=rcw&sig=QcVwljccgWcNMbe9roAJbD8J5oEkYoq%2F0cUPlgriBn0%3D"} 
```

Blob depolama kapsayıcısına yüklemek için örnek bir blob dosyası oluşturun. Linux VM’sinde bu işlemi aşağıdaki komutla gerçekleştirebilirsiniz. 

```bash
echo "This is a test file." > test.txt
```

Ardından SAS kimlik bilgilerini kullanarak CLI `az storage` komutuyla kimlik doğrulaması yapın ve dosyayı blob kapsayıcısına yükleyin. Bu adım için, daha önceden yüklemediyseniz VM’inize [en son Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) sürümünü yüklemeniz gerekir.

```azurecli-interactive
 az storage blob upload --container-name 
                        --file 
                        --name
                        --account-name 
                        --sas-token
```

Yanıt: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-21T03:58:56+00:00"
}
```

Ayrıca dosyayı Azure CLI kullanarak indirebilir ve kimlik doğrulamasını SAS kimlik bilgileriyle yapabilirsiniz. 

İstek: 

```azurecli-interactive
az storage blob download --container-name
                         --file 
                         --name 
                         --account-name
                         --sas-token
```

Yanıt: 

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

Bu öğreticide, SAS kimlik bilgisi kullanarak Azure Depolama'ya erişmek için Linux VM sistem tarafından atanan yönetilen kimlik kullanmayı öğrendiniz.  Azure Depolama SAS hakkında daha fazla bilgi edinmek için bkz:

> [!div class="nextstepaction"]
>[Paylaşılan erişim imzaları (SAS) kullanma](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)
