---
title: Azure Storage erişmek için bir Linux VM MSI kullanın
description: Azure depolama alanına erişmek için bir Linux VM yönetilen hizmet kimliği (MSI) kullanarak sürecinde anlatan öğretici.
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
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 35197500b04b7ff4124a6400f609cb5fdd2a5cb0
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="use-a-linux-vm-managed-service-identity-to-access-azure-storage-via-access-key"></a>Azure depolama erişim tuşu erişmek için bir Linux VM yönetilen hizmet kimliği kullanın

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Bu öğretici bir Linux sanal makine için Yönetilen hizmet kimliği (MSI) etkinleştirmek ve depolama hesabı erişim anahtarlarını almak için bu kimlik kullanmak gösterilmektedir. Her zamanki gibi depolama SDK'yı kullanarak, örneğin depolama işlemleri yaparken depolama erişim tuşunu kullanabilirsiniz. Bu öğretici için karşıya yükleme ve Azure CLI kullanarak blob'lara indirin. Şunları öğreneceksiniz nasıl yapılır:

> [!div class="checklist"]
> * Bir Linux sanal makinede MSI etkinleştir 
> * Depolama hesabı erişim anahtarlarını Kaynak Yöneticisi'nde, VM erişim 
> * VM kimliğini kullanarak bir erişim belirteci alın ve Kaynak Yöneticisi'nden depolama erişim tuşlarını almak için kullanın  

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Azure'da oturum açma
[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.


## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Yeni bir kaynak grubunda bir Linux sanal makine oluşturun

Bu öğretici için yeni bir Linux VM oluşturun. Mevcut bir VM'yi üzerinde MSI de etkinleştirebilirsiniz.

1. Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. **İşlem**'i ve ardından **Ubuntu Server 16.04 LTS**'yi seçin.
3. Sanal makine bilgilerini girin. İçin **kimlik doğrulama türü**seçin **SSH ortak anahtarını** veya **parola**. Oluşturulan kimlik bilgileri, VM'ye oturum açmak izin verir.

    ![Alt görüntü metin](../media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Seçin bir **abonelik** sanal makine açılır.
5. Yeni bir seçmek için **kaynak grubu** sanal makinenin oluşturulması, seçmek istediğiniz **Yeni Oluştur**. İşlem tamamlandığında **Tamam**’a tıklayın.
6. VM boyutunu seçin. Daha fazla boyutları görmek için seçin **tüm görüntüle** veya desteklenen disk türü filtresini değiştirin. Ayarlar dikey penceresinde varsayılan değerleri koruyun ve **Tamam**'a tıklayın.

## <a name="enable-msi-on-your-vm"></a>MSI VM üzerinde etkinleştir

Bir sanal makine MSI erişim belirteçleri, kimlik bilgileri kodunuza koyma gereksinimi olmadan Azure AD'den almanızı sağlar. Yönetilen hizmet kimliği bir VM'de etkinleştirme iki şey yapar: yazmaçlar yönetilen kimliğini ve oluşturmak için Azure Active Directory ile VM VM kimliğini yapılandırır.  

1. Yeni bir sanal makine kaynak grubuna gidin ve önceki adımda oluşturduğunuz sanal makineyi seçin.
2. Sol taraftaki "ayarlar" VM altında tıklatın **yapılandırma**.
3. Kaydolun ve MSI etkinleştirmek için seçin **Evet**, devre dışı bırakmak istiyorsanız seçin No
4. Tıklattığınız olun **kaydetmek** yapılandırmayı kaydetmek için.

    ![Alt görüntü metin](../media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-storage-account"></a>Depolama hesabı oluşturma 

Zaten yoksa, şimdi bir depolama hesabı oluşturacak.  Ayrıca, bu adımı atlayın ve var olan bir depolama hesabı anahtarları, VM MSI erişim. 

1. Tıklatın **+/ yeni hizmet oluşturma** düğme Azure portalında sol üst köşesinde bulundu.
2. Tıklatın **depolama**, ardından **depolama hesabı**, ve yeni bir "depolama hesabı oluşturma" panelinde görüntülenir.
3. Girin bir **adı** daha sonra kullanacaksınız depolama hesabı için.  
4. **Dağıtım modeli** ve **tür hesap** "Resource manager" ve "Genel amaçlı", sırasıyla ayarlanmalıdır. 
5. Olun **abonelik** ve **kaynak grubu** VM'nizi oluşturduğunuzda önceki adımda belirttiğiniz olanlarla eşleşmesi.
6. **Oluştur**’a tıklayın.

    ![Yeni depolama hesabı oluştur](../media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Depolama hesabında blob kapsayıcısı oluşturma

Daha sonra biz karşıya yükleyin ve yeni depolama hesabı dosya indirme. Dosyaları blob depolama gerektirdiğinden, biz dosyasının depolanacağı bir blob kapsayıcısı oluşturmanız gerekir.

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Tıklatın **kapsayıcıları** "Blob hizmeti" altındaki sol bağlantı
3. Tıklatın **+ kapsayıcı** sayfa ve "yeni bir kapsayıcı" üst kısmında çıkış paneli slayt.
4. Kapsayıcı bir ad verin, erişim düzeyi seçin ve ardından **Tamam**. Belirtilen ad daha sonra öğreticide kullanılır. 

    ![Depolama kapsayıcısı oluşturma](../media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-storage-account-access-keys"></a>Depolama hesabı erişim anahtarları kullanmak için VM MSI erişim

Azure Storage, Azure AD kimlik doğrulaması yerel olarak desteklemez.  Ancak, bir MSI depolama hesabı erişim anahtarlarını Kaynak Yöneticisi'nden almanızı sonra depolama erişmek için bir anahtar kullanın.  Bu adımda, VM MSI anahtarları depolama hesabınıza erişim.   

1. Yeni oluşturulan depolama hesabınıza geri gidin.
2. Tıklatın **erişim denetimi (IAM)** sol panelinde bağlantı.  
3. Tıklatın **+ Ekle** VM için yeni bir rol ataması eklemek için sayfanın en üstünde
4. Ayarlama **rol** "Depolama hesabı anahtarı işleci hizmeti rolü", sayfanın sağ tarafında. 
5. Sonraki açılır listede ayarlamak **atamak için erişim** "Sanal makine" kaynak.  
6. Ardından, uygun abonelik listelenir olun **abonelik** açılan listesinde, daha sonra ayarlamak **kaynak grubu** "Tüm kaynak gruplarına".  
7. Son olarak, altında **seçin** açılır listede, Linux sanal makine seçin ve ardından **kaydetmek**. 

    ![Alt görüntü metin](../media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>VM kimliğini kullanarak bir erişim belirteci alın ve Azure Resource Manager çağırmak için kullanın

Öğretici kalanı için size daha önce oluşturduğumuz sanal makineden çalışmaz.

Bu adımları tamamlamak için bir SSH istemcisi gerekir. Windows kullanıyorsanız, SSH İstemcisi'nde kullanabileceğiniz [Linux için Windows alt](https://msdn.microsoft.com/commandline/wsl/install_guide). SSH istemcinin anahtarları yapılandırma yardıma gereksinim duyarsanız, bkz: [kullanmak SSH anahtarları nasıl Windows Azure üzerinde ile](../../virtual-machines/linux/ssh-from-windows.md), veya [nasıl oluşturulacağı ve Linux VM'ler için Azure'da bir SSH ortak ve özel anahtar çifti kullanılmak](../../virtual-machines/linux/mac-create-ssh-keys.md).

1. Azure portalında gidin **sanal makineleri**gidin, Linux sanal makine, daha sonra **genel bakış** sayfasında **Bağlan** üstünde. VM'nize bağlanmak için dizesini kopyalayın. 
2. VM'nize SSH istemcisini kullanarak bağlanın.  
3. Ardından, girmek için istenir, **parola** oluştururken eklenen **Linux VM**. Ardından başarıyla oturum açmanız.  
4. Azure Resource Manager için bir erişim belirteci almak üzere CURL kullanın.  

    CURL istek ve yanıt için erişim belirteci aşağıdadır:
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -H Metadata:true
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
    
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Depolama çağrı yapmak için Azure Resource Manager alanından depolama hesabı erişim anahtarlarını alma  

Artık Kaynak Yöneticisi'ni önceki bölümde biz alınan erişim belirteci kullanarak çağırmak için CURL kullanın depolama erişim anahtarı alınamadı. Depolama erişim tuşu sahip olduğumuz sonra biz depolama yükleme/indirme işlemleri çağırabilirsiniz. Değiştirdiğinizden emin olun `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, ve `<STORAGE ACCOUNT NAME>` parametre değerlerini kendi değerlere sahip. Değiştir `<ACCESS TOKEN>` daha önce erişim belirteci ile değer:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listKeys?api-version=2016-12-01 --request POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> Önceki URL metinde büyük küçük harfe duyarlıdır, buna göre yansıtacak şekilde büyük-küçük, kaynak grupları için kullanıyorsanız, bu nedenle olun. Ayrıca, bu bir POST isteği GET isteği olduğunu biliyor ve uzunluk sınırı - NULL olabilir d ile yakalamak için bir değer geçirmek sağlamak önemlidir.  

CURL yanıt anahtarlarının listesi sağlar:  

```bash 
{"keys":[{"keyName":"key1","permissions":"Full","value":"iqDPNt..."},{"keyName":"key2","permissions":"Full","value":"U+uI0B..."}]} 
```
Blob depolama kapsayıcınızı karşıya yüklemek için örnek bir blob dosyası oluşturun. Bir Linux VM şu komutla bunu yapabilirsiniz. 

```bash
echo "This is a test file." > test.txt
```

Ardından, CLI ile kimlik doğrulaması `az storage` komutu kullanılarak depolama erişim tuşu ve blob kapsayıcısına dosyasını karşıya yükleyin. Bu adım için yapmanız gerekir [en son Azure CLI yükleme](https://docs.microsoft.com/cli/azure/install-azure-cli) henüz yapmadıysanız, VM üzerinde.
 

```azurecli-interactive
az storage blob upload -c <CONTAINER NAME> -n test.txt -f test.txt --account-name <STORAGE ACCOUNT NAME> --account-key <STORAGE ACCOUNT KEY>
```

Yanıtı: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-12T03:58:56+00:00"
}
```

Ayrıca, Azure CLI kullanarak ve depolama erişim anahtarı ile kimlik doğrulaması dosyayı indirebilirsiniz. 

İsteği: 

```azurecli-interactive
az storage blob download -c <CONTAINER NAME> -n test.txt -f test-download.txt --account-name <STORAGE ACCOUNT NAME> --account-key <STORAGE ACCOUNT KEY>
```

Yanıtı: 

```JSON
{
  "content": null,
  "metadata": {},
  "name": "test.txt",
  "properties": {
    "appendBlobCommittedBlockCount": null,
    "blobType": "BlockBlob",
    "contentLength": 21,
    "contentRange": "bytes 0-20/21",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentMd5": "LSghAvpnElYyfUdn7CO8aw==",
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
    "etag": "\"0x8D5067F30D0C283\"",
    "lastModified": "2017-09-28T14:42:49+00:00",
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

- MSI genel bakış için bkz: [yönetilen hizmet Kimliği'ne genel bakış](overview.md).
- Depolama SAS kimlik bilgilerini kullanarak bu öğreticiyi yapmak öğrenmek için bkz: [bir SAS kimlik bilgisi Azure depolama erişmek için bir Linux VM yönetilen hizmet kimliği kullanın](tutorial-linux-vm-access-storage-sas.md)
- Azure depolama hesabı SAS özelliği hakkında daha fazla bilgi için bkz:
  - [Paylaşılan erişim imzaları (SAS) kullanma](/azure/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Hizmet SAS oluşturma](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Geri bildirim sağlamak ve iyileştirmek ve içeriği şekil yardımcı olmak için aşağıdaki açıklamaları bölümü kullanın.
