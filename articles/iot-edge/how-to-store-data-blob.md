---
title: Azure IOT Edge cihazlarında Azure Blob Depolama | Microsoft Docs
description: IOT Edge cihazınıza uçta veri depolamak için bir Azure Blob Depolama modülü dağıtın.
author: kgremban
manager: timlt
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 10/03/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 6fdfc1002528fa48145e577dfee3eac935f31fcd
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344855"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>IOT Edge (Önizleme) Azure Blob Depolama ile uçta veri Store

IOT Edge üzerinde Azure Web günlüğü depolama blok blobu depolama çözümü uçta sağlar. Bir blob depolama modülü IOT Edge Cihazınızda bir Azure blob hizmeti gibi davranır, ancak blok blobları, IOT Edge Cihazınızda yerel olarak depolanır. Aynı Azure depolama SDK'ın yöntemleri kullanarak bloblarınızın erişebilir veya zaten kullanılan blob API çağrılarını engelle. 

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

IOT Edge üzerinde Azure Blob Depolama, üç standart kapsayıcı görüntüleri, iki Linux kapsayıcıları (AMD64 ve ARM32 mimarileri) için ve biri Windows kapsayıcıları (AMD64) sağlar. Bu modül görüntülerden birini blob depolama, IOT Edge cihazına dağıtmak için kullandığınız zaman üç parça bir modül örneğinin cihazınız için yapılandırma bilgilerini sağlar:

* Bir **hesap adı** ve **hesap anahtarı**. Blob depolama modülleri Azure depolama ile tutarlı kalmak için erişimi yönetmek için hesap adlarını ve hesabı anahtarları kullanın. Hesap adları, küçük harf ve sayı ile uzun üç için yirmi dört karakter arasında olmalıdır. Hesabı anahtarları, base64 olarak kodlanmış ve uzunluğu 64 baytın olması gerekir. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64).
* A **yerel depolama seçeneği**. Böylece modül durdurur veya yeniden blobları kalıcı blob storage modülünde blobları IOT Edge cihaz üzerinde yerel olarak depolar. Mevcut bir bildirmek [birim](https://docs.docker.com/storage/volumes
) veya burada blobların depolanması Cihazınızda yerel klasör yolu. 

Modüller IOT Edge cihazına dağıtmak için birkaç yolu vardır ve bunların tümünü IOT Edge modülleri Azure Blob Depolama için çalışır. Visual Studio kod şablonları ve Azure Portalı'nı kullanmak için iki basit yöntemler şunlardır. 

### <a name="azure-portal"></a>Azure portal

BLOB Depolama, Azure portalı üzerinden dağıtmak için adımları izleyin. [dağıtma Azure IOT Edge modülleri Azure portalından](how-to-deploy-modules-portal.md). Önce modülünüzde dağıtmak, resim URI'sini kopyalayın ve kapsayıcısı hazırlama Git oluşturduğunuz kapsayıcı işletim sisteminize bağlı seçenekleri. Bu değerleri kullanmak **bir dağıtım bildirimi yapılandırma** dağıtım makalesindeki bölümüne bakın. 

Görüntü URI'si için blob depolama modülü sağlayın: **mcr.microsoft.com/azure-blob-storage:latest**. 
   
Aşağıdaki JSON şablon için kullandığınız **kapsayıcı oluşturma seçenekleri** alan. JSON, depolama hesabı adı, depolama hesabı anahtarı ve depolama dizini bağlama ile yapılandırın.  
   
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
   
JSON oluşturma seçeneklerinde güncelleştirme `\<your storage account name\>` herhangi bir ada sahip. Güncelleştirme `\<your storage account key\>` 64 baytlık base64 anahtarına sahip. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64) seçin, bayt uzunluğunu verir. Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.

JSON oluşturma seçeneklerinde güncelleştirme `<storage directory bind>` kapsayıcı işletim sisteminize bağlı olarak. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu.  

   * Linux kapsayıcıları:  **\<depolama yolu >: / blobroot**. Örneğin, / srv/containerdata: / blobroot. Veya, birim my: / blobroot. 
   * Windows kapsayıcıları:  **\<depolama yolu >: C: / BlobRoot**. Örneğin, C: / ContainerData:C: / BlobRoot. Veya, my-birim: C: / blobroot.
   
   > [!CAUTION]
   > Değişmez "/ blobroot" Linux ve "C:/BlobRoot" için Windows için  **\<depolama dizini bağlama >** değerleri.

IOT Edge üzerinde Azure Blob depolamaya erişmek için kayıt defteri kimlik bilgilerini sağlamanız gerekmez ve dağıtımınız için tüm rotalar bildirmenize gerek yoktur. 

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

8. Depolama hesabı adı için herhangi bir ad sağlayın ve depolama hesabı anahtarı bir 64 baytlık base64 anahtarı sağlayın. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız. 

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

