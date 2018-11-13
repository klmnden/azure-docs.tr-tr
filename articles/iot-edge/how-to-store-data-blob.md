---
title: Azure IOT Edge cihazlarında Azure Blob Depolama | Microsoft Docs
description: IOT Edge cihazınıza uçta veri depolamak için bir Azure Blob Depolama modülü dağıtın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 10/03/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: fa88ff46b4fb93d55aa0087cca0e6184f3e087a0
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2018
ms.locfileid: "51567290"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>IOT Edge (Önizleme) Azure Blob Depolama ile uçta veri Store

IOT Edge üzerinde Azure Blob Depolama sağlayan bir [blok blobu](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) uçta depolama çözümü. Bir blob depolama modülü IOT Edge Cihazınızda bir Azure blok blob hizmeti gibi davranır, ancak blok blobları, IOT Edge Cihazınızda yerel olarak depolanır. Aynı Azure depolama SDK'ın yöntemleri kullanarak bloblarınızın erişebilir veya zaten kullanılan blob API çağrılarını engelle. 

Burada veri videoları, resimleri, Finans verileri, hastane verilerini veya yerel olarak işlenen veya aktarılan buluta yerel olarak, daha sonra depolanması için gereken herhangi bir veri gibi senaryoları bu modülü kullanmak için iyi örneklerdir.

Bu makalede, Azure Blob Depolama, blob hizmeti, IOT Edge Cihazınızda çalışan IOT Edge kapsayıcısı üzerinde dağıtmak için yönergeler sağlar. 

>[!NOTE]
>IOT Edge üzerinde Azure Blob Depolama, içinde [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

* [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md) için hızlı başlangıç adımlarını izleyerek dağıtım makinenizi veya sanal makinenizi bir Edge cihazı olarak kullanabilirsiniz.
* IOT Edge modülü Azure Blob Depolama, aşağıdaki cihaz yapılandırmalarını destekler:

   | İşletim sistemi | Mimari |
   | ---------------- | ------------ |
   | Ubuntu Server 16.04 | AMD64 |
   | Ubuntu Server 18.04 | AMD64 |
   | Windows 10 IoT Core (Ekim güncelleştirme) | AMD64 |
   | Windows 10 IOT Enterprise (Ekim güncelleştirme) | AMD64 |
   | Windows Server 2019 | AMD64 |
   | Raspbian Uzat | ARM32 |

Bulut kaynakları:

* Azure'da standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md). 


## <a name="deploy-blob-storage-to-your-device"></a>BLOB Depolama, cihazınıza dağıtma

Modüller IOT Edge cihazına dağıtmak için birkaç yolu vardır ve bunların tümünü IOT Edge modülleri Azure Blob Depolama için çalışır. Visual Studio kod şablonları ve Azure Portalı'nı kullanmak için iki basit yöntemler şunlardır. 

### <a name="azure-portal"></a>Azure portal

#### <a name="find-the-module"></a>Modül Bul

Blob depolama modülü bulmak için iki yoldan biriyle seçin:

1. Azure portalı Ara "Azure Blob Depolama üzerinde IOT Edge". Ve **seçin** arama sonucu öğesi
2. Azure Portalı'ndan Market'e gidin ve "Nesnelerin interneti hakkında"'i tıklatın. "IOT Edge modülleri" bölümünde "Azure Blob Depolama üzerinde IOT Edge" seçin. Tıklatıp **oluştur**

#### <a name="steps-to-deploy"></a>Dağıtma adımları

**IOT Edge modülü için hedef cihazlar**

1. "Abonelik" IOT Hub'ınıza dağıtıldığı seçin.
2. "IOT Hub'ınıza" seçin.
3. "IOT Edge cihazı, bu modül dağıtmak için istediğiniz adı" sağlar. Cihazınızı bulmak için "Cihaz bulunamadı" kullanmayı da tercih edebilirsiniz.
4. **Oluştur**’a tıklayın.

**Modülleri ayarlama**

1. "Modül Ekle" bölümünde "Altında dağıtım modülleri" adı "İle AzureBlobStorageonIoTEdge" ile başlayarak, modül zaten listede bulabilirsiniz. 
2. **Seçin** "Dağıtım modülleri" listesinden blob depolama modülü. "IOT Edge özel modüller" yan paneli açılır.
3. **Ad**: Burada modülünün adı değiştirebilirsiniz.
4. **Görüntü URI'si**: URI değiştirin **mcr.microsoft.com/azure-blob-storage:latest**
5. **Kapsayıcı oluşturma seçenekleri**: aşağıdaki JSON'u değerlerinizi içeren düzenleme ve Portal sayfasında JSON ile değiştirin:
   
   ```json
   {
       "Env":[
           "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
           "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
       ],
       "HostConfig":{
           "Binds":[
               "<storage directory bind>"
           ],
           "PortBindings":{
               "11002/tcp":[{"HostPort":"11002"}]
           }
       }
   }
   ```   
   
    * Güncelleştirme `<your storage account name>`. Hesap adları, küçük harf ve sayı ile uzun üç için yirmi dört karakter arasında olmalıdır.
    * Güncelleştirme `<your storage account key>` 64 baytlık base64 anahtarına sahip. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.
    * Güncelleştirme `<storage directory bind>`. Kapsayıcı işletim sistemine bağlı olarak. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu.  

       * Linux kapsayıcıları:  **\<depolama yolu >: / blobroot**. Örneğin, / srv/containerdata: / blobroot. Veya, birim my: / blobroot. 
       * Windows kapsayıcıları:  **\<depolama yolu >: C: / BlobRoot**. Örneğin, C: / ContainerData:C: / BlobRoot. Veya, my-birim: C: / blobroot.
   
   > [!CAUTION]
   > Değişmez "/ blobroot" Linux ve "C:/BlobRoot" için Windows için  **\<depolama dizini bağlama >** değerleri.

    ![Modül değerlerini güncelleştirin](./media/how-to-store-data-blob/edit-module.png)

6. **Kaydet** "IOT Edge özel modüller" değerler
7. Tıklayın **sonraki** "modülleri ayarlama" bölümünde
8. Tıklayın **sonraki** "Yolları belirtin" bölümünde
9. Gözden geçirdikten sonra **Gönder** "Gözden geçirme dağıtımı" bölümünde.
10. IOT hub'ına cihaz blob storage modülünde çalıştığından emin olun 

### <a name="visual-studio-code-templates"></a>Visual Studio Code şablonları

Azure IOT Edge, uç çözümleri geliştirmenize yardımcı olması için Visual Studio code'da şablonları sağlar. Bu adımları sahip olmanızı gerektirir [Visual Studio Code](https://code.visualstudio.com/) geliştirme makinenizde yüklü ve yapılandırılmış [Azure IOT Edge uzantısını](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge).

Bir blob depolama modülü ile yeni bir IOT Edge çözüm oluşturmak için aşağıdaki adımları kullanın ve dağıtım bildirimini yapılandırın. 

1. Seçin **görünümü** > **komut paleti**. 

2. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm yeni**. 

3. Yeni bir çözüm oluşturmak için istemleri takip edin: 

   1. **Klasör seçin** -yeni çözümü oluşturmak istediğiniz klasöre göz atın.  
   
   2. **Bir çözüm adı sağlayın** - çözümünüz için bir ad girin veya varsayılan değeri kabul edin.
   
   3. **Select modülü şablonu** -seçin **varolan bir modülle (Enter tam görüntü URL'si)**.
   
   4. **Bir modül adı sağlayın** -gibi modül için tanınabilir bir ad girin **azureBlobStorage**.
   
   5. **İçin modülü Docker görüntüsü sağlayın** -resim URI'sini girin: **mcr.microsoft.com/azure-blob-storage:latest**

VS Code, sağlanan bir IOT Edge çözümü oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır. 

Çözüm şablonu, blob depolama modülü görüntüsünü içeren bir dağıtım bildirimi şablonu oluşturur, ancak yapılandırmanız gerekir modülün oluşturma seçenekleri. 

1. Açık **deployment.template.json** yeni çözüm çalışma alanı ve Bul **modülleri** bölümü. 

2. Silme **tempSensor** haliyle modülü, bu dağıtım için gerekli değildir. 

3. Aşağıdaki kodu kopyalayıp **createOptions** modülünüzün blob depolama alanı: 

   ```json
   {\"Env\": [\"LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME\",\" LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY\"],\"HostConfig\": {\"Binds\": [\"<storage directory bind>\"],\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"11002\"}]}}}
   ```

   ![Güncelleştirme modülü oluşturma seçenekleri](./media/how-to-store-data-blob/create-options.png)

4. JSON oluşturma seçeneklerinde güncelleştirme `<storage directory bind>` kapsayıcı işletim sisteminize bağlı olarak. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu.  

   * Linux kapsayıcıları:  **\<depolama yolu >: / blobroot**. Örneğin, / srv/containerdata: / blobroot. Veya, birim my: / blobroot.
   * Windows kapsayıcıları:  **\<depolama yolu >: C: / BlobRoot**. Örneğin, C: / ContainerData:C: / BlobRoot. Veya, my-birim: C: / blobroot.
   
   > [!CAUTION]
   > Değişmez "/ blobroot" Linux ve "C:/BlobRoot" için Windows için  **\<depolama dizini bağlama >** değerleri.

5. Kaydet **deployment.template.json**.

6. Açık **.env** çözüm çalışma alanınızdaki. 

7. Genel kullanıma açık olduğundan herhangi bir kapsayıcı kayıt defteri değeri için blob depolama resim girmeniz gerekmez. Bunun yerine, iki yeni ortam değişkenlerini ekleyin: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

8. İçin değer sağlamanız `STORAGE_ACCOUNT_NAME`, hesabı adları küçük harf ve sayı ile uzun üç için yirmi dört karakter olmalıdır. Ve sağlamak için bir 64 baytlık base64 anahtar `STORAGE_ACCOUNT_KEY`. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız. 

9. Kaydet **.env**. 

10. Sağ **deployment.template.json** seçip **oluşturmak IOT Edge dağıtım bildirimi**. 

Visual Studio Code deployment.template.json ve .env sağlanan ve yeni bir dağıtım bildirimi dosyası oluşturmak için kullandığı bilgileri alır. Yeni bir dağıtım bildirimi oluşturulan **config** çözüm çalışma alanınızda bir klasör. Bu dosyayı açtıktan sonra adımları izleyebilirsiniz [Visual Studio Code için Azure IOT Edge'e dağıtma modüllerden](how-to-deploy-modules-vscode.md) veya [Azure CLI 2.0 ile Azure IOT Edge'e dağıtma modülleri](how-to-deploy-modules-cli.md).

## <a name="connect-to-your-blob-storage-module"></a>Blob depolama modülüne bağlayın

Hesap adı ve IOT Edge Cihazınızda blob depolamaya erişmek, bir modül için yapılandırılan hesap anahtarı kullanabilirsiniz. 

IOT Edge Cihazınızı herhangi bir depolama alanı için blob uç nokta olarak belirtmek için yaptığınız istekleri. Yapabilecekleriniz [bir açık depolama uç noktası için bir bağlantı dizesi oluşturma](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) IOT Edge cihaz bilgileri ve yapılandırdığınız hesabı adını kullanarak. 

1. "Azure Blob Depolama üzerinde IOT Edge" çalıştığı aynı uç cihaza dağıtılan modülleri için blob uç noktadır: `http://<Module Name>:11002/<account name>`. 
2. Burada "Azure Blob Depolama üzerinde IOT Edge" çalışıyor ve ardından blob uç noktası, kurulumunuza olarak sınır cihazı değerinden farklı edge cihazı üzerinde dağıtılan modüller için: `http://<device IP >:11002/<account name>` veya `http://<IoT Edge device hostname>:11002/<account name>` veya `http://<FQDN>:11002/<account name>`

## <a name="logs"></a>Günlükler

Kapsayıcının içinde günlükleri altında bulabilirsiniz: 
* Linux için: /blobroot/logs/platformblob.log

## <a name="deploy-multiple-instances"></a>Birden fazla örnek dağıtın

IOT Edge üzerinde Azure Blob Depolama birden çok örneğini dağıtmak istiyorsanız, yalnızca modül bağlar hostport değerini değiştirmeniz gerekir. Blob depolama modülleri, bağlantı noktasını 11002 kapsayıcıdaki her zaman kullanıma sunma, ancak konakta bağlı hangi bağlantı noktası bildirebilirsiniz. 

Düzenleme modülü hostport değerini değeri değiştirmek için seçenekler oluşturun:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Ek blob depolama modüllerini bağladığınızda, uç nokta güncel ana bilgisayar bağlantı noktasına işaret edecek şekilde değiştirin. 

### <a name="try-it-out"></a>Deneyin

Çeşitli dillerde örnek kodlar sağladık hızlı başlangıçlar Azure Blob Depolama belgeleri içerir. IOT Edge üzerinde Azure Blob Depolama, blob uç noktası, blob depolama modülünüzde işaret edecek şekilde değiştirerek test etmek için bu örnekleri yeniden çalıştırabilirsiniz.

Şu hızlı başlangıçlarda olarak blob depolama modülü ile birlikte IOT Edge modülleri dağıtabilirsiniz böylece IOT Edge ile de desteklenen dilleri kullanın:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="supported-storage-operations"></a>Desteklenen depolama işlemleri

BLOB Depolama modülleri IOT Edge üzerinde aynı Azure depolama SDK'ları kullanın ve Azure depolama API'SİNİN blok blob uç noktalar için 2018-03-28 sürümü tutarlıdır. Sonraki sürümlerde, müşteri gereksinimlerine bağlıdır. 

Tüm Azure Blob Depolama işlemleri, IOT Edge üzerinde Azure Blob Depolama tarafından desteklenir. Hangi işlemlerin bir misiniz olmayan aşağıdaki bölümlerde ayrıntılı desteklenmiyor. 

### <a name="account"></a>Hesap

Desteklenen: 
* Kapsayıcıları listeleme

Desteklenmeyen: 
* Alma ve blob hizmeti özelliklerini ayarla
* Denetim öncesi blob isteği
* BLOB hizmeti istatistikleri alın
* Hesap bilgilerini alma

### <a name="containers"></a>Kapsayıcılar

Desteklenen: 
* Oluşturma ve kapsayıcı silme
* Kapsayıcı özellikleri ve meta verileri alma
* Blobları listeleme

Desteklenmeyen: 
* Alma ve kapsayıcı ACL ayarlama
* Kira kapsayıcı
* Kümesi kapsayıcı meta verileri

### <a name="blobs"></a>Bloblar

Desteklenen: 
* Yerleştirme, alma ve blob silme
* Alma ve blob özelliklerini ayarlama
* Alma ve blob meta verileri ayarlama

Desteklenmeyen: 
* Kira blob'u
* Blob anlık görüntüsü
* Kopyalama ve kopyalama blob durdurma
* Blobu silme işlemi geri
* Blob katmanı Ayarla

### <a name="block-blobs"></a>Blok blobları

Desteklenen: 
* Blok yerleştirme:-blok boyutu 4 MB değerinden küçük veya eşit olmalıdır
* Koy ve engelleme listesine alın

Desteklenmeyen:
* URL'den blok yerleştirme

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md)

