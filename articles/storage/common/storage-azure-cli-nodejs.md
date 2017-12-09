---
title: Azure Storage ile Azure CLI 1.0 kullanma | Microsoft Docs
description: "Azure komut satırı arabirimi (Azure CLI) 1.0 oluşturmak ve depolama hesaplarını yönetme ve çalışmak için Azure Storage ile Azure BLOB'ları ve dosyalar ile kullanmayı öğrenin. Azure CLI platformlar arası bir araçtır"
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.openlocfilehash: 772417012e4c6aa519e83177bd8e93778f6af3b5
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="using-the-azure-cli-10-with-azure-storage"></a>Azure Storage ile Azure CLI 1.0 kullanma

## <a name="overview"></a>Genel Bakış

Azure CLI, Azure platformu ile çalışmak için platformlar arası komutları açık kaynak kümesi sağlar. Bulunan aynı işlevlerinin çoğunu sağlayan [Azure portal](https://portal.azure.com) de gibi zengin veri erişim işlevselliği.

Bu kılavuzda, biz nasıl kullanılacağını ele alacağız [Azure komut satırı arabirimi (Azure CLI)](../../cli-install-nodejs.md) çeşitli Azure Storage ile geliştirme ve yönetim görevlerini gerçekleştirmek için. İndirin ve yükleyin veya bu kılavuzu kullanmadan önce en son Azure CLI yükseltme öneririz.

Bu kılavuz, Azure Storage temel kavramlarını anladığınızı varsayar. Kılavuz bir sayıda Azure Storage ile Azure CLI'ın kullanımını göstermek için betik sağlar. Her komut dosyası çalıştırılmadan önce yapılandırmanıza göre komut dosyası değişkenleri güncelleştirdiğinizden emin olun.

> [!NOTE]
> Kılavuzu Klasik depolama hesapları için Azure CLI komut ve komut dosyası örnekler verilmektedir. Bkz: [Mac, Linux ve Windows Azure kaynak yönetimi için Azure CLI kullanarak](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) Resource Manager depolama hesapları için Azure CLI komutları için.
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>5 dakika içinde Azure Storage ve Azure CLI kullanmaya başlama
Bu kılavuzda Ubuntu örnekler için kullanılır, ancak diğer işletim sistemi platformlarını benzer şekilde gerçekleştirmeniz gerekir.

**Yeni Azure:** Microsoft Azure aboneliği ve bu abonelikle ilişkili bir Microsoft hesabı alın. Azure satın alma seçenekleri hakkında daha fazla bilgi için bkz: [ücretsiz deneme](https://azure.microsoft.com/pricing/free-trial/), [satın alma seçenekleri](https://azure.microsoft.com/pricing/purchase-options/), ve [üye teklifleri](https://azure.microsoft.com/pricing/member-offers/) (MSDN, Microsoft iş ortağı ağı ve, BizSpark üyeleri için ve diğer Microsoft programları).

Bkz: [Azure Active Directory'de (Azure AD) yönetici rolleri atama](https://msdn.microsoft.com/library/azure/hh531793.aspx) Azure abonelikleri hakkında daha fazla bilgi için.

**Microsoft Azure aboneliği ve hesabı oluşturduktan sonra:**

1. Özetlenen yönergeleri izleyerek Azure CLI yükleyip [Azure CLI yükleme](../../cli-install-nodejs.md).
2. Azure CLI yüklendikten sonra Azure CLI komutlara erişmek için azure komut, komut satırı arabiriminden (Bash, Terminal, komut istemi) kullanmanız mümkün olacaktır. Tür _azure_ komutunu ve aşağıdaki çıktı görmeniz gerekir.

    ![Azure komut çıktısı](./media/storage-azure-cli/azure_command.png)   
3. Komut satırı arabirimini yazın `azure storage` tüm azure depolama komutları listelemek ve işlevlerin bir ilk izlenim Azure CLI almak için sağlar. Komut adı ile yazabilirsiniz **-h** parametresi (örneğin, `azure storage share create -h`) komut sözdizimi ayrıntılarını görmek için.
4. Şimdi, Azure depolama alanına erişmek için temel Azure CLI komutları gösterir basit bir komut dosyası sunuyoruz. Komut dosyası, depolama hesabı ve anahtarı için iki değişkenleri ayarlamak için ilk isteyecektir. Ardından, betik yeni bir kapsayıcı bu yeni depolama hesabı oluşturun ve varolan bir görüntü dosyası (blob) kapsayıcıya karşıya yükleme. Komut dosyası bu kapsayıcıdaki tüm blob'lara listeler sonra yerel bilgisayarda var olan hedef dizinine görüntü dosyasını indirir.

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

5. Yerel bilgisayarınızda, tercih edilen metin düzenleyicisi (örneğin VIM) açın. Yukarıdaki komut, metin düzenleyicisi yazın.
6. Şimdi, yapılandırma ayarlarınızı temel alan komut dosyası değişkenleri güncelleştirmeniz gerekir.

   * **< Storage_account_name >** komut dosyasında verilen ad kullanın veya depolama hesabınız için yeni bir ad girin. **Önemli:** depolama hesabı adının Azure'da benzersiz olması gerekir. Çok küçük olmalıdır!
   * **< Storage_account_key >** , depolama hesabının erişim anahtarı.
   * **< Container_name >** komut dosyasında verilen ad kullanın veya, kapsayıcı için yeni bir ad girin.
   * **< İmage_to_upload >** , yerel bilgisayarınızda bir resim yolu gibi girin: "~ / images/HelloWorld.png".
   * **< Destination_folder >** gibi Azure Storage'dan indirilen dosyaları depolamak için bir yerel dizinin yolunu girin: "~/downloadImages".
7. VIM gerekli değişkenlerde güncelleştirdikten sonra tuş bileşimlerini basın `ESC`, `:`, `wq!` komut dosyasını kaydetmek için.
8. Bu komut dosyasını çalıştırmak için betik dosyası adı bash konsolunda yazın. Bu betik çalıştıktan sonra indirilen görüntü dosyasını içeren bir yerel hedef klasör olmalıdır. Aşağıdaki ekran görüntüsünde bir örnek çıkış şunları gösterir:

Betik çalıştıktan sonra indirilen görüntü dosyasını içeren bir yerel hedef klasör olmalıdır.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Depolama hesaplarını Azure CLI ile yönetme
### <a name="connect-to-your-azure-subscription"></a>Azure aboneliğinize bağlanma
Depolama komutların çoğu Azure aboneliği çalışır ancak Azure CLI üzerinden aboneliğinize bağlanmak için önerilir. Aboneliğiniz ile birlikte çalışmak için Azure CLI yapılandırmak için adımları [Azure clı'dan Azure aboneliğine Bağlan](/cli/azure/authenticate-azure-cli).

### <a name="create-a-new-storage-account"></a>Yeni bir depolama hesabı oluştur
Azure depolama kullanan bir depolama hesabı gerekir. Aboneliğinize bağlanmak için bilgisayarınızı yapılandırdıktan sonra yeni bir Azure depolama hesabı oluşturabilirsiniz.

```azurecli
azure storage account create <account_name>
```

Depolama hesabınızın adını uzunluğu 3 ile 24 karakter arasında olmalı ve sayı ve yalnızca küçük harf kullanmanız gerekir.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Ortam değişkenleri varsayılan bir Azure depolama hesabı ayarlama
Aboneliğinizde birden çok depolama hesabı olabilir. Bunlardan birini seçin ve aynı oturum tüm depolama komutları için ortam değişkenleri içindeki ayarlayın. Bu depolama hesabı belirtmeden Azure CLI depolama komutlarını çalıştırın ve açıkça anahtar sağlar.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Başka bir yolu varsayılan depolama hesabı bağlantı dizesi kullanıyor. İlk olarak bağlantı dizesini komutla alın:

```azurecli
azure storage account connectionstring show <account_name>
```

Ardından çıktı bağlantı dizesini kopyalayın ve ortam değişkenine ayarlayın:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Oluşturma ve BLOB'ları yönetme
Azure Blob Depolama, büyük miktarlarda herhangi bir yere HTTP veya HTTPS aracılığıyla erişilebilen metin veya ikili veriler gibi yapılandırılmamış verileri depolamak için bir hizmettir. Bu bölümde, zaten Azure Blob Depolama kavramlarını olduğunu varsayar. Ayrıntılı bilgi için bkz: [.NET kullanarak Azure Blob storage'ı kullanmaya başlama](../blobs/storage-dotnet-how-to-use-blobs.md) ve [Blob hizmeti kavramları](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Bir kapsayıcı oluşturma
Azure depolama her blob bir kapsayıcıda olmalıdır. Özel bir kapsayıcı kullanılarak oluşturabilirsiniz `azure storage container create` komutu:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Anonim okuma erişimini üç düzeyi vardır: **kapalı**, **Blob**, ve **kapsayıcı**. Bloblar için anonim erişimi engellemek için izni parametre kümesini **devre dışı**. Varsayılan olarak yeni kapsayıcı özeldir ve yalnızca hesap sahibi tarafından erişilebilir. Anonim izin vermek için herkese okuma erişimi blob kaynaklarına ancak kapsayıcı meta verileri için veya kapsayıcıdaki blobları listesine izin parametre kümesine **Blob**. Kaynaklar, kapsayıcı meta verileri ve kapsayıcıdaki blobları listesi blob tam ortak okuma erişimi sağlamak üzere izin parametre kümesini **kapsayıcı**. Daha fazla bilgi için bkz: [kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Bir kapsayıcıya bir blob yükleme
Azure Blob Storage blok blobları ve sayfa bloblarını destekler. Daha fazla bilgi için bkz: [anlama blok Blobları, ekleme Blobları ve sayfa Bloblarını](http://msdn.microsoft.com/library/azure/ee691964.aspx).

BLOB'ları bir kapsayıcıya karşıya yüklemek için kullanabileceğiniz `azure storage blob upload`. Varsayılan olarak, bu komutu bir blok blobuna yerel dosyaları karşıya yükleme. Blob türü belirtmek için kullanabileceğiniz `--blobtype` parametresi.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Kapsayıcıdan BLOB indirmek
Aşağıdaki örnek, bir kapsayıcıdan BLOB indirmek gösterilmiştir.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>BLOB kopyalama
Blobları, depolama hesaplarıyla bölgeler içinde veya bunların arasında zaman uyumsuz olarak kopyalayabilirsiniz.

Aşağıdaki örnekte blobların bir depolama hesabından diğerine nasıl kopyalandığı gösterilmektedir. Bu örnekte BLOB'lar nerede genel olarak, bir kapsayıcı oluşturuyoruz anonim olarak erişilebilir.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

Bu örnek bir zaman uyumsuz kopya gerçekleştirir. Çalıştırarak her kopyalama işlemi durumunu izleyebilirsiniz `azure storage blob copy show` işlemi.

Kaynak URL kopyalama işlemi sağlanan Not gerekir genel olarak erişilebilir ya da bir SAS (paylaşılan erişim imzası) belirteci içerir.

### <a name="delete-a-blob"></a>Blob silme
Bir blobu silmek için kullanın komutu altında:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Oluşturun ve dosya paylaşımlarını yönetmek
Azure dosyaları standart SMB protokolünü kullanan uygulamalar için paylaşılan depolama alanı sunar. Microsoft Azure sanal makineler ve bulut Hizmetleri yanı sıra, şirket içi uygulamalara bağlı paylaşımlar üzerinden dosya verileri paylaşabilir. Dosya paylaşımları ve dosya verilerini Azure CLI aracılığıyla yönetebilirsiniz. Azure dosyaları hakkında daha fazla bilgi için bkz: [Azure dosyaları giriş](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma
Bir Azure dosya paylaşımı azure'da SMB dosya paylaşımı değil. Tüm dizin ve dosyaların bir dosya paylaşımı oluşturulmalıdır. Bir hesapta sınırsız sayıda paylaşım olabilir ve bir paylaşım dosyaları, depolama hesabının kapasite sınırlarına kadar sınırsız sayıda depolayabilirsiniz. Aşağıdaki örnek adlı bir dosya paylaşımı oluşturur **paylaşımım**.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Dizin oluşturma
Bir dizin, bir Azure dosya paylaşımı için isteğe bağlı hiyerarşik bir yapı sağlar. Aşağıdaki örnek adlı bir dizin oluşturur **Dizinim** dosya paylaşımında.

```azurecli
azure storage directory create myshare myDir
```

Bu dizin yolu, birden çok düzeyi içerebilir Not *örneğin*, **bir / b**. Ancak, tüm üst dizinleri var olduğundan emin olmalısınız. Örneğin, yol **bir / b**, dizin oluşturmanız gerekir **bir** ilk olarak, ardından dizin oluşturma **b**.

### <a name="upload-a-local-file-to-directory"></a>Dizine yerel bir dosya karşıya yükleme
Aşağıdaki örnekte bir dosyadan yükler **~/temp/samplefile.txt** için **Dizinim** dizin. Dosya yolunu yerel makinenizdeki geçerli bir dosyaya işaret şekilde düzenleyin:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Bir dosya paylaşımında boyutu 1 TB'ye kadar olabileceğini unutmayın.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Dosya paylaşım kök veya dizinde Listele
Bir paylaşım kök veya aşağıdaki komutu kullanarak bir dizinde alt dizinlerin ve dosyaların listeleyebilirsiniz:

```azurecli
azure storage file list myshare myDir
```

Dizin adı listeleme işlemi için isteğe bağlı olduğunu unutmayın. Atlanırsa, komut paylaşımının kök dizinin içeriğini listeler.

### <a name="copy-files"></a>Dosyaları kopyalama
Azure CLI 0.9.8 sürümü ile başlayarak, bir dosyaya kopyalayabilirsiniz başka bir dosyaya, bir dosyayı bir bloba veya bir blobu bir dosyaya. Aşağıda CLI komutları kullanarak bu kopyalama işlemlerini gerçekleştirmek nasıl göstermektedir. Yeni dizine dosya kopyalamak için:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

Bir blob dosya dizinine kopyalamak için:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Sonraki Adımlar

Burada depolama kaynaklarla çalışmak için Azure CLI 1.0 komut başvurusu bulabilirsiniz:

* [Kaynak Yöneticisi modunda Azure CLI komutları](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Azure Hizmet Yönetimi modunda Azure CLI komutları](../../cli-install-nodejs.md)

Ayrıca denemek ister [Azure CLI 2.0](../storage-azure-cli.md), Resource Manager dağıtım modeli ile kullanmak için Python içinde yazılmış bizim nesil CLI.
