---
title: Bir SAS kimlik bilgisi kullanarak Azure Depolama'ya erişmek için bir Linux VM MSI kullanma
description: Bir Linux VM yönetilen hizmet kimliği (MSI) Azure depolama, depolama hesabı erişim anahtarı yerine SAS kimlik bilgisi kullanarak erişmek için nasıl kullanılacağını gösteren öğretici.
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
ms.openlocfilehash: fc7c5b4ab025666fc7fa1d9073198ec90d8e71c3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611036"
---
# <a name="use-a-linux-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Azure depolama bir SAS kimlik bilgisi erişmek için bir Linux VM yönetilen hizmet Kimliği'ni kullanın

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Bu öğreticide bir Linux sanal makinesi için Yönetilen hizmet kimliği (MSI) etkinleştirin ve ardından depolama paylaşılan erişim imzası (SAS) kimlik bilgisi elde etmek için MSI kullanma gösterilmektedir. Özellikle, bir [hizmet SAS kimlik bilgisi](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Hizmet SAS sınırlı bir süre ve bir hesap erişim anahtarına gösterme olmadan (Bu örnekte, blob hizmeti), belirli bir hizmet için bir depolama hesabındaki nesnelere sınırlı erişim izni verme olanağı sağlar. Her zaman olduğu gibi depolama SDK'sı kullanırken, örneğin, depolama işlemleri yaparken bir SAS kimlik bilgisini kullanabilirsiniz. Bu öğreticide, yükleme ve Azure depolama CLI kullanarak bir blob indirme gösterir. Şunların nasıl yapılır:


> [!div class="checklist"]
> * Azure'da bir Linux sanal makinesine MSI etkinleştir 
> * Bir depolama hesabı SAS Kaynak Yöneticisi'nde, VM erişimi verme 
> * Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve Kaynak Yöneticisi'nden SAS almak için kullanın 

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.


## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makinesi oluşturma

Bu öğretici için yeni bir Linux VM'yi oluştururuz. Mevcut VM'yi MSI de etkinleştirebilirsiniz.

1. Tıklayın **+/ yeni hizmet oluşturma** düğmesi Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarı** veya **parola**. Oluşturulan kimlik bilgilerini, VM'de oturum açmak izin verin.

    ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makinenin açılır.
5. Yeni bir seçilecek **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. Sanal makine için boyutu seçin. Daha fazla boyut görmek için seçin **tümünü görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="enable-msi-on-your-vm"></a>Vm'nizde MSI etkinleştir

Bir sanal makine MSI, kimlik bilgilerini kodunuza koyma gereksinimi olmadan Azure AD'den erişim belirteci alma olanak tanır. Perde MSI etkinleştirmesine iki şeyi yapar: VM'NİZDE MSI VM uzantısı yükler ve sanal makine için Yönetilen hizmet kimliği sağlar.  

1. Yeni sanal makinenize kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. Sol taraftaki "ayarlar" VM altında tıklayın **yapılandırma**.
3. Kaydolun ve MSI etkinleştirmek için **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıkladığınız olun **Kaydet** yapılandırmayı kaydetmek için.

    ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Hangi uzantıların denetlemek istiyorsanız, sanal makinede olan, tıklayın **uzantıları**. MSI etkin olduğunda **ManagedIdentityExtensionforLinux** listesinde görünür.

    ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Zaten yoksa, artık bir depolama hesabı oluşturur.  Ayrıca, bu adımı atlayıp mevcut bir depolama hesabı anahtarları, VM MSI erişimi verme. 

1. Tıklayın **+/ yeni hizmet oluşturma** düğmesi Azure portalının sol üst köşedeki üzerinde bulunamadı.
2. Tıklayın **depolama**, ardından **depolama hesabı**, ve "depolama hesabı oluştur" yeni bir panel görüntülenir.
3. Girin bir **adı** daha sonra kullanacağınız depolama hesabı için.  
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

    ![Depolama kapsayıcısı oluşturma](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>Bir SAS depolama kullanmak için sanal makinenizin MSI erişimi verme 

Azure depolama, yerel olarak Azure AD kimlik doğrulamasını desteklemez.  Ancak, Kaynak Yöneticisi'nden depolama SAS almak için bir MSI kullanma sonra depolamaya erişmek için SAS'ı kullanın.  Bu adımda, depolama hesabınıza SAS, VM MSI erişimi verme.   

1. Yeni oluşturulan depolama hesabına geri git...   
2. Tıklayın **erişim denetimi (IAM)** sol bölmede bağlantı.  
3. Tıklayın **+ Ekle** VM'niz için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Ayarlama **rol** "Depolama hesabı Katılımcısı", sayfanın sağ tarafındaki için. 
5. Sonraki açılır menüden ayarlamak **erişim Ata** "Sanal makinesi" kaynak.  
6. Ardından, uygun abonelik listelendiğinden emin olmak **abonelik** açılır listesinde, ardından ayarlayın **kaynak grubu** "Tüm kaynak gruplarına".  
7. Son olarak, altında **seçin** açılır menüde Linux sanal makinenizi seçin, ardından tıklayın **Kaydet**.  

    ![Alt resim metni](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Sanal makinenin kimliğini kullanarak bir erişim belirteci alma ve Azure Resource Manager'ı çağırmak için kullanın

Bu öğreticinin geri kalanında için daha önce oluşturduğumuz VM'den çalışacağız.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt sistemi](https://msdn.microsoft.com/commandline/wsl/install_guide). SSH istemcinizin anahtarları yapılandırılıyor yardıma ihtiyacınız varsa bkz [azure'da Windows ile SSH kullanma anahtarları nasıl](~/articles/virtual-machines/linux/ssh-from-windows.md), veya [oluşturmak ve azure'da Linux VM'ler için SSH ortak ve özel anahtar çifti kullanmak nasıl](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında gidin **sanal makineler**gidin, Linux sanal makinesi, ardından **genel bakış** sayfasında **Connect** en üstünde. VM'nize bağlanmak için dizesini kopyalayın. 
2. SSH istemciniz kullanarak VM'nize bağlanın.  
3. Ardından, sizden girmek için istenir, **parola** oluştururken eklediğiniz **Linux VM**. Sonra başarıyla oturum açmanız.  
4. Azure Resource Manager için bir erişim belirteci almak için CURL kullanın.  

    CURL istek ve yanıt için erişim belirteci aşağıda verilmiştir:
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true    
    ```
    
    > [!NOTE]
    > Önceki istekte "kaynak" parametresinin değerini, Azure AD tarafından beklenen tam bir eşleşme olması gerekir. Azure Resource Manager kaynak kimliği kullanıldığında, URI üzerinde sonunda eğik çizgi içermelidir.
    > Şu yanıtı, konuyu uzatmamak amacıyla kısalttık olarak access_token öğesi.
    
    ```bash
    {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
     ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrıları yapmak için Azure Resource Manager'dan bir SAS kimlik bilgileri Al

Artık depolama SAS kimlik bilgisi oluşturmak için önceki bölümde biz alınan erişim belirteci kullanarak Resource Manager'ı çağırmak için CURL kullanın. Biz SAS kimlik bilgilerini aldıktan sonra biz depolama karşıya yükleme/indirme işlemleri çağırabilirsiniz.

Bu istek için SAS kimlik bilgisi oluşturmak için aşağıdaki HTTP istek parametrelerini kullanacağız:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite.  Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Bu parametreler, SAS kimlik bilgisi için istek POST gövdesinde dahil edilir. Bir SAS kimlik bilgisi oluşturmak için parametreler hakkında daha fazla bilgi için bkz. [listesi hizmet SAS REST başvurusu](/rest/api/storagerp/storageaccounts/listservicesas).

SAS kimlik bilgisi almak için aşağıdaki CURL isteği'ni kullanın. Değiştirdiğinizden emin olun `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, `<CONTAINER NAME>`, ve `<EXPIRATION TIME>` parametre değerlerini kendi değerlerinizle. Değiştirin `<ACCESS TOKEN>` daha önce aldığınız erişim belirtecini değeri:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listServiceSas/?api-version=2017-06-01 -X POST -d "{\"canonicalizedResource\":\"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>\",\"signedResource\":\"c\",\"signedPermission\":\"rcw\",\"signedProtocol\":\"https\",\"signedExpiry\":\"<EXPIRATION TIME>\"}" -H "Authorization: Bearer <ACCESS TOKEN>"
```

> [!NOTE]
> Önceki URL'de metin büyük/küçük harfe duyarlıdır, bu nedenle büyük-küçük, kaynak gruplarınız için uygun şekilde yansıtmak için kullanıyorsanız emin olun. Ayrıca, bu bir POST isteği bir GET isteği olduğunu bilmek önemlidir.

SAS kimlik bilgisi CURL yanıtı döndürür:  

```bash 
{"serviceSasToken":"sv=2015-04-05&sr=c&spr=https&st=2017-09-22T00%3A10%3A00Z&se=2017-09-22T02%3A00%3A00Z&sp=rcw&sig=QcVwljccgWcNMbe9roAJbD8J5oEkYoq%2F0cUPlgriBn0%3D"} 
```

Blob depolama kapsayıcısına karşıya yüklemek için örnek blob dosyası oluşturun. Bir Linux VM üzerinde aşağıdaki komutu kullanarak bunu yapabilirsiniz. 

```bash
echo "This is a test file." > test.txt
```

Ardından, CLI'de kimlik doğrulaması `az storage` komutunu kullanarak SAS kimlik bilgisi ve dosyayı blob kapsayıcısına yükleyin. Bu adım için şunları yapmanız gerekir [en son Azure CLI'yı yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) henüz yapmadıysanız, VM'de.

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

Ayrıca SAS kimlik bilgileri ile kimlik doğrulaması ve Azure CLI kullanarak dosyayı indirebilirsiniz. 

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

- MSI genel bakış için bkz. [yönetilen hizmet Kimliği'ne genel bakış](msi-overview.md).
- Bu öğreticiyi yapma hakkında bilgi için bkz. depolama hesabı anahtarı kullanma, [Azure depolamaya erişmek için bir Linux VM yönetilen hizmet kimliği kullan](msi-tutorial-linux-vm-access-storage.md)
- Azure depolama hesabı SAS özelliği hakkında daha fazla bilgi için bkz:
  - [Paylaşılan erişim imzaları (SAS) kullanma](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Hizmet SAS oluşturma](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Aşağıdaki yorum bölümünde geri bildirim sağlamak ve geliştirmek ve içeriklerimizde şekil yardımcı kullanın.
