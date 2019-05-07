---
title: Azure Klasik CLI, Azure depolama ile kullanma | Microsoft Docs
description: Azure Klasik komut satırı arabirimi (CLI) oluşturma ve depolama hesaplarını yönetme ve Azure BLOB'ları ve dosyalarla çalışmak için Azure Depolama'yı kullanmayı öğrenin.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 01/30/2017
ms.author: tamram
ms.reviewer: seguler
ms.subservice: common
ms.openlocfilehash: 88f713c5695e2453edc58d072899aa417f0514af
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147034"
---
# <a name="using-the-azure-classic-cli-with-azure-storage"></a>Azure Klasik CLI ile Azure depolama kullanma

## <a name="overview"></a>Genel Bakış

Klasik Azure CLI, Azure platformu ile birlikte çalışmaya yönelik platformlar arası komut bir dizi açık kaynaklı sağlar. Bulunan aynı işlevselliğinin sağlar [Azure portalında](https://portal.azure.com) da zengin veri erişim işlevleri.

Bu kılavuzda, biz nasıl kullanılacağı hakkında bilgi edineceksiniz [Azure Klasik CLI](../../cli-install-nodejs.md) çeşitli Azure Storage ile geliştirme ve yönetim görevlerini gerçekleştirmek için. İndirin ve yükleyin veya bu kılavuzu kullanmadan önce Klasik en yeni CLI sürümüne yükseltme öneririz.

Bu kılavuzda, Azure Depolama'nın temel kavramlarını anladığınızı varsayar. Bir dizi betiği Azure depolama ile klasik CLI kullanımını göstermek için kılavuz sağlar. Her komut dosyasını çalıştırmadan önce yapılandırmasına göre komut dosyası değişkenleri güncelleştirdiğinizden emin olun.

> [!NOTE]
> Kılavuz, Klasik depolama hesapları için Azure Klasik CLI komutu ve betik örnekleri sağlar. Bkz: [Mac, Linux ve Windows Azure kaynak yönetimi ile Azure CLI kullanarak](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) Resource Manager depolama hesapları için Azure Klasik CLI komutlarına ait.
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-classic-cli-in-5-minutes"></a>Azure depolama ve Azure Klasik CLI ile 5 dakika içinde kullanmaya başlayın
Bu kılavuzda Ubuntu örnekler için kullanılır, ancak diğer işletim sistemi platformları benzer şekilde gerçekleştirmeniz gerekir.

**Azure'da yeni:** Microsoft Azure aboneliği ve bu abonelikle ilişkili bir Microsoft hesabı alın. Azure satın alma seçenekleri hakkında daha fazla bilgi için bkz: [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/), [satın alma seçenekleri](https://azure.microsoft.com/pricing/purchase-options/), ve [üye Tekliflerimizi](https://azure.microsoft.com/pricing/member-offers/) (MSDN, Microsoft Partner Network ve BizSpark üyeleri için ve başka Microsoft programlarını).

Bkz: [Azure Active Directory'de (Azure AD) yönetici rolleri atama](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) Azure abonelikleri hakkında daha fazla bilgi.

**Microsoft Azure aboneliği ve hesabı oluşturduktan sonra:**

1. Bölümünde açıklanan yönergeleri izleyerek Azure Klasik CLI yükleyip [Klasik Azure CLI yükleme](../../cli-install-nodejs.md).
2. Klasik CLI'yı yüklendikten sonra Klasik CLI komutlarına erişmek için azure komut satırı arabiriminizde (Bash, Terminal, komut istemi) komutu kullanmanız mümkün olacaktır. Tür _azure_ komutunu aşağıdaki çıktıyı görmelisiniz.

    ![Azure komut çıktısı](./media/storage-azure-cli/azure_command.png)   
3. Komut satırı arabiriminde yazın `azure storage` tüm azure depolama komutları listelemek ve işlevlerini bir ilk izlenim Klasik CLI sürümünü edinmek için sağlar. Komut adı yazabilirsiniz **-h** parametresi (örneğin, `azure storage share create -h`) komut söz dizimini ayrıntılarını görmek için.
4. Şimdi, biz, Azure depolama alanına erişmek için temel Klasik CLI komutlarını gösteren basit bir komut dosyası verin. Betik ilk olarak, depolama hesabınız ve anahtarı için iki değişkenlerini ayarlamak için sorar. Ardından, betik bu yeni bir depolama hesabında yeni bir kapsayıcı oluşturur ve var olan bir görüntü dosyası (blob), kapsayıcıya yüklemek. Betik bu kapsayıcıdaki tüm blobları listeler sonra yerel bilgisayarda var olan hedef dizine görüntü dosyasını indirir.

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating the container..."
    azure storage container create $container_name

    echo "Uploading the image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing the blobs..."
    azure storage blob list $container_name

    echo "Downloading the image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. Yerel bilgisayarınızda, tercih edilen bir metin düzenleyicisi (örneğin vim) açın. Yukarıdaki betik metin düzenleyicinize yazın.
6. Şimdi, yapılandırma ayarlarınızı temel alan betik değişkenlerini güncelleştirmeniz gerekir.

   * **< Depolama_hesabı_adı >** belirtilen ada komut dosyasını kullanın veya depolama hesabınız için yeni bir ad girin. **Önemli:** Depolama hesabı adı Azure'da benzersiz olması gerekir. Çok küçük olmalıdır!
   * **< Storage_account_key >** depolama hesabınızın erişim anahtarı.
   * **< Container_name >** belirtilen ada komut dosyasını kullanın veya kapsayıcınız için yeni bir ad girin.
   * **< İmage_to_upload >** yerel bilgisayarınızda, bir resim yolu gibi girin: "~ / images/HelloWorld.png".
   * **< Destination_folder >** gibi Azure Storage'dan indirilen dosyaları depolamak için bir yerel dizin için bir yol girin: "~/downloadImages".
7. Tuş birleşimleri vim gerekli değişkenlerinde güncelleştirdikten sonra basın `ESC`, `:`, `wq!` betiği kaydedilemiyor.
8. Bu komut dosyasını çalıştırmak için betik dosyası adı bash konsolunda yazın. Bu komut dosyası çalıştırıldıktan sonra indirilen görüntü dosyasını içeren bir yerel hedef klasör olmalıdır. Aşağıdaki anlık görüntüde bir örnek çıktı gösterilmektedir:

Betik çalıştıktan sonra indirilen görüntü dosyasını içeren bir yerel hedef klasör olmalıdır.

## <a name="manage-storage-accounts-with-the-azure-classic-cli"></a>Depolama hesaplarını Azure Klasik CLI ile yönetme
### <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma
Depolama komutların çoğu bir Azure aboneliği olmadan çalışır ancak klasik CLI üzerinden aboneliğinize bağlanmak için öneririz.

### <a name="create-a-new-storage-account"></a>Yeni depolama hesabı oluşturma
Azure depolama kullanmak için bir depolama hesabı gerekir. Aboneliğinize bağlanmak için bilgisayarınızı yeniden yapılandırdıktan sonra yeni bir Azure depolama hesabı oluşturabilirsiniz.

```azurecli
azure storage account create <account_name>
```

Depolama hesabınızın adı 3 ila 24 karakter uzunluğunda olmalı ve sayı ve yalnızca küçük harflerden oluşmalıdır.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Varsayılan Azure depolama hesabı içinde ortam değişkenlerini ayarlama
Aboneliğinizde birden fazla depolama hesabı olabilir. Bunlardan birini seçin ve aynı oturum içindeki tüm depolama komutları için ortam değişkenlerini ayarlayın. Bu, depolama hesabı belirtmeden Klasik CLI depolama komutları çalıştırabilir ve açıkça anahtar sağlar.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Başka bir yolu varsayılan depolama hesabı bağlantı dizesi kullanıyor. İlk olarak bağlantı dizesini komutuyla alın:

```azurecli
azure storage account connectionstring show <account_name>
```

Ardından çıktı bağlantı dizesini kopyalayın ve ortam değişkenini ayarlayın:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Oluşturma ve BLOB'ları yönetme
Azure Blob Depolama, büyük miktarlarda herhangi bir HTTP veya HTTPS aracılığıyla dünyanın erişilebilen metin veya ikili veriler gibi yapılandırılmamış verileri depolamak için kullanılan bir hizmettir. Bu bölümde, zaten ile Azure Blob Depolama kavramları hakkında bilgi sahibi olduğunuz varsayılır. Ayrıntılı bilgi için bkz. [.NET kullanarak Azure Blob depolamayı kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md) ve [Blob hizmeti kavramları](https://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma
Her blob Azure depolamadaki bir kapsayıcıda olmalıdır. Kullanarak bir özel kapsayıcı oluşturabilirsiniz `azure storage container create` komutu:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Anonim okuma erişimini üç düzeyi vardır: **Kapalı**, **Blob**, ve **kapsayıcı**. Bloblar için anonim erişimi engellemek için izni parametre ayarlamak **kapalı**. Varsayılan olarak yeni kapsayıcı özeldir ve yalnızca hesap sahibi tarafından erişilebilir. Anonim izin vermek için genel okuma erişimini blob kaynakları, ancak kapsayıcı meta verileri için veya kapsayıcıdaki blobları listesine izin parametre kümesine **Blob**. İzni parametresi kaynakları, kapsayıcı meta verileri ve kapsayıcıdaki blobların listesi BLOB tam genel okuma erişimi sağlamak istiyorsanız kümesine **kapsayıcı**. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Azure Blob Storage blok blobları ve sayfa bloblarını destekler. Daha fazla bilgi için [anlama blok Blobları, ekleme Blobları ve sayfa Blobları](https://msdn.microsoft.com/library/azure/ee691964.aspx).

Bir kapsayıcıya BLOB'ları karşıya yüklemek için kullanabileceğiniz `azure storage blob upload`. Varsayılan olarak, bu komut, bir blok blobuna yerel dosyaları karşıya yükler. Blob türü belirtmek için kullanabileceğiniz `--blobtype` parametresi.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Kapsayıcıdan BLOB indirme
Aşağıdaki örnek, bir kapsayıcıdaki blobları indirmek gösterilmektedir.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Blobları kopyalama
Blobları, depolama hesaplarıyla bölgeler içinde veya bunların arasında zaman uyumsuz olarak kopyalayabilirsiniz.

Aşağıdaki örnekte blobların bir depolama hesabından diğerine nasıl kopyalandığı gösterilmektedir. Bu örnekte bloblar nerede genel olarak, bir kapsayıcı oluşturacağız anonim olarak erişilebilir.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

Bu örnek, bir zaman uyumsuz kopya gerçekleştirir. Çalıştırarak her kopyalama işleminin durumunu izleyebilirsiniz `azure storage blob copy show` işlemi.

Kaynak URL'sini kopyalama işlemi için sağlanan Not genel olarak erişilebilir veya SAS (paylaşılan erişim imzası) belirteci içerir.

### <a name="delete-a-blob"></a>Blob silme
Bir blobu silmek için kullanın. aşağıdaki komutu:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Oluşturma ve dosya paylaşımlarını yönetme
Azure dosyaları standart SMB protokolünü kullanarak uygulamalara paylaşılan depolama sağlar. Şirket içi uygulamaların yanı sıra Microsoft Azure sanal makineler ve bulut Hizmetleri, bağlanan paylaşımlar yoluyla dosya verileri paylaşabilir. Dosya paylaşımları ve dosya verilerini Klasik CLI aracılığıyla yönetebilirsiniz. Azure dosyaları hakkında daha fazla bilgi için bkz. [Azure dosyaları'na giriş](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Bir Azure dosya paylaşımı azure'daki bir SMB dosyası paylaşımıdır. Tüm dizin ve dosyaların bir dosya paylaşımı oluşturulması gerekir. Bir hesapta sınırsız sayıda paylaşım olabilir ve dosyaları, depolama hesabının kapasite sınırları sınırsız sayıda paylaşım depolayabilir. Aşağıdaki örnekte adlı bir dosya paylaşımı oluşturur **myshare**.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Dizin oluşturma
Bir dizin, bir Azure dosya paylaşımı için isteğe bağlı bir hiyerarşik yapısı sağlar. Aşağıdaki örnekte adlı bir dizin oluşturur **Dizinim** Dosya paylaşımındaki.

```azurecli
azure storage directory create myshare myDir
```

Bu dizin yolu, birden çok düzeyi içerebilir Not *örn*, **bir / b**. Ancak, tüm üst dizinlerin var emin olmanız gerekir. Örneğin, yol **bir / b**, dizin oluşturmanız gerekir **bir** önce sonra dizin oluşturmak **b**.

### <a name="upload-a-local-file-to-directory"></a>Dizine yerel bir dosya yükleyin
Aşağıdaki örnek, bir dosyadan yükler **~/temp/samplefile.txt** için **Dizinim** dizin. Dosya yolunu yerel makinenizdeki geçerli bir dosyaya işaret şekilde düzenleyin:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Paylaşımdaki bir dosya boyutu 1 TB'ye kadar olabileceğini unutmayın.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Dosya paylaşım kök veya dizinde Listele
Paylaşım kök veya aşağıdaki komutu kullanarak bir dizin alt dizinler ve dosyalar listeleyebilirsiniz:

```azurecli
azure storage file list myshare myDir
```

Dizin adı listeleme işlemi için isteğe bağlı olduğunu unutmayın. Atlanırsa, komut paylaşım kök dizininin içeriğini listeler.

### <a name="copy-files"></a>Dosyaları kopyalama
Klasik CLI 0.9.8 sürümünden itibaren bir dosyaya kopyalayıp başka bir dosyaya, bir dosyayı bir bloba veya bir blobu bir dosyaya için. Aşağıdaki CLI komutları kullanarak bu kopyalama işlemlerini nasıl gerçekleştireceğinizi gösterir. Bir dosyasını yeni dizine kopyalamak için:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

Blob dosya dizinine kopyalamak için:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Sonraki Adımlar

Burada depolama kaynakları ile çalışmak için Azure Klasik CLI komut başvurusu bulabilirsiniz:

* [Resource Manager modunda Klasik Azure CLI komutları](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Azure Hizmet Yönetimi modunda Klasik Azure CLI komutları](../../cli-install-nodejs.md)

En son sürümünü denemek da beğenebilirsiniz [Azure CLI](../storage-azure-cli.md), Resource Manager dağıtım modeli ile kullanmak için.
