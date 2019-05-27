---
title: Azure Blob Storage modülünde cihazlara - Azure IOT Edge dağıtma | Microsoft Docs
description: IOT Edge cihazınıza uçta veri depolamak için bir Azure Blob Depolama modülü dağıtın.
author: kgremban
ms.author: kgremban
ms.date: 05/21/2019
ms.topic: article
ms.service: iot-edge
ms.custom: seodec18
ms.reviewer: arduppal
manager: philmea
ms.openlocfilehash: d844e81de9cfb556e91ab5c0d5a8074c822cce0a
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65990463"
---
# <a name="deploy-the-azure-blob-storage-on-iot-edge-module-to-your-device"></a>IOT Edge modülü Azure Blob Depolama, cihazınıza dağıtma

Modüller IOT Edge cihazına dağıtmak için çeşitli yollar vardır ve bunların tümünü IOT Edge modülleri Azure Blob Depolama için çalışır. Visual Studio kod şablonları ve Azure Portalı'nı kullanmak için iki basit yöntemler şunlardır.

## <a name="prerequisites"></a>Önkoşullar

- Bir [IOT hub'ı](../iot-hub/iot-hub-create-through-portal.md) Azure aboneliğinizdeki.
- Bir [IOT Edge cihazı](how-to-register-device-portal.md) yüklü olan bir IOT Edge çalışma zamanı ile.
- [Visual Studio Code](https://code.visualstudio.com/) ve [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools) Visual Studio Code'dan dağıtımı.

## <a name="deploy-from-the-azure-portal"></a>Azure portalından dağıtma

Azure portalında bir dağıtım bildirimi oluşturmak ve IOT Edge cihazına dağıtım gönderme sırasında size kılavuzluk eder.

### <a name="select-your-device"></a>Cihazınızı seçin

1. Oturum [Azure portalında](https://portal.azure.com) ve IOT hub'ınıza gidin.
1. Seçin **IOT Edge** menüsünde.
1. Hedef cihazın cihazlar listesinden numarasını tıklayın.
1. **Modülleri Ayarlama**'yı seçin.

### <a name="configure-a-deployment-manifest"></a>Bir dağıtım bildirimi yapılandırma

Bir dağıtım bildirimi dağıtmak için modülleri ve modül ikizlerini istenen özellikleri arasında verilerin nasıl aktığını modüllerine açıklayan bir JSON belgesidir. Azure portalı, JSON belgesini el ile oluşturmak yerine bir dağıtım bildirimi oluşturmada size yol gösterir. bir sihirbaz vardır. Bu üç adım vardır: **Modül Ekle**, **yolları belirtin**, ve **gözden geçirin, dağıtım**.

#### <a name="add-modules"></a>Modül ekle

1. İçinde **dağıtım modülleri** sayfasında bölümünü **Ekle**.

1. Modülleri aşağı açılan listesinde türlerinden seçin **IOT Edge Modülü**.

1. Modül için bir ad belirtin ve ardından kapsayıcı görüntüsünü belirtin:

   - **Ad** -azureblobstorageoniotedge
   - **Görüntü URI'si** -mcr.microsoft.com/azure-blob-storage:latest

   > [!IMPORTANT]
   > Azure IOT Edge modülleri çağrı yapmak ve depolama SDK'sı de küçük harfe varsayılanlarını duyarlıdır. Olsa da modülün adı [Azure Marketi](how-to-deploy-modules-portal.md#deploy-modules-from-azure-marketplace) olan **AzureBlobStorageonIoTEdge**, yardımcı olur, IOT Edge modülü Azure Blob Depolama bağlantılarınızı emin olmak için küçük adının değiştirilmesi kesintiye.

1. Varsayılan **kapsayıcı oluşturma seçenekleri** değerlerini kapsayıcınızın gereken bağlantı noktası bağlamaları tanımlamak, ancak aynı zamanda Cihazınızda depolama hesap bilgilerinizi ve depolama dizini için bir bağlama eklemek gerekir. ' % S'varsayılan JSON Portalı'nda, aşağıdaki JSON ile değiştirin:

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

1. Kopyaladığınız JSON aşağıdaki bilgilerle güncelleştirin:

   - Değiştirin `<your storage account name>` hatırlayabileceğiniz bir ad. Hesabı adları 3 ila 24 karakter uzunluğunda, küçük harf ve sayı ile olmalıdır. Boşluk.

   - Değiştirin `<your storage account key>` 64 baytlık base64 anahtarına sahip. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.

   - Değiştirin `<storage directory bind>` kapsayıcı işletim sisteminize göre. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu. Depolama dizini bağlama bir konum sağlayan Cihazınızda bir modül kümesini konumda eşlenir.

     - Linux kapsayıcıları için biçimdir  *\<depolama yolu >: / blobroot*. Örneğin, **/srv/containerdata: / blobroot** veya **my-volume: / blobroot**.
     - Windows kapsayıcıları için biçimdir  *\<depolama yolu >: C: / BlobRoot*. Örneğin, **C: / ContainerData:C: / BlobRoot** veya **my-birim: C: / blobroot**.

     > [!IMPORTANT]
     > İkinci yarısında depolama dizininin modülünde belirli bir konuma işaret değeri bağlama değiştirmeyin. Depolama dizini bağlama ile her zaman bitmelidir **: / blobroot** Linux kapsayıcıları için ve **: C: / BlobRoot** Windows kapsayıcıları için.

    ![Güncelleştirme modülü kapsayıcı oluşturma seçenekleri - portal](./media/how-to-store-data-blob/edit-module.png)

1. Ayarlama [katmanlama](how-to-store-data-blob.md#tiering-properties) ve [yaşam süresi](how-to-store-data-blob.md#time-to-live-properties) aşağıdaki JSON kopyalayıp yapıştırarak modülünüzde özelliklerini **istenen özellikler kümesi modül ikizi** kutusu. Her bir özellik ile uygun bir değer yapılandırmak, kaydedin ve dağıtım ile devam edin.

   ```json
   {
     "properties.desired": {
       "ttlSettings": {
         "ttlOn": <true, false>,
         "timeToLiveInMinutes": <timeToLiveInMinutes>
       },
       "tieringSettings": {
         "tieringOn": <true, false>,
         "backlogPolicy": "<NewestFirst, OldestFirst>",
         "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>; EndpointSuffix=<your end point suffix>",
         "tieredContainers": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         }
       }
     }
   }

      ```

   ![katmanlama ve yaşam süresi özelliklerini ayarlama](./media/how-to-store-data-blob/iotedge_custom_module.png)

   Modülünüzün dağıtıldıktan sonra katman ayarlamayı ve TTL yapılandırma hakkında daha fazla bilgi için bkz: [modül İkizi Düzenle](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). İstenen özellikleri hakkında daha fazla bilgi için bkz: [tanımlayın veya güncelleştirme istenen özelliklerini](module-composition.md#define-or-update-desired-properties).

1. **Kaydet**’i seçin.

1. Seçin **sonraki** yollar bölüme geçmek için.

#### <a name="specify-routes"></a>Rota belirtme

Varsayılan yolları tutun ve seçin **sonraki** gözden geçirme bölüme geçmek için.

#### <a name="review-deployment"></a>Dağıtım gözden geçirin

Oluşturulan gözden geçirme bölümü gösterir, JSON dağıtım bildirimi önceki iki bölümlerde seçimlerinize göre. Ayrıca vardır iki modül bildirildi, ekleyemiyor: **$edgeAgent** ve **$edgeHub**. Bu iki modül oluşturan [IOT Edge çalışma zamanı](iot-edge-runtime.md) ve her dağıtımda gerekli varsayılanlardır.

Dağıtım bilgilerinizi gözden geçirin ve ardından **Gönder**.

### <a name="verify-your-deployment"></a>Dağıtımınızı doğrulayın

Dağıtım gönderdikten sonra geri **IOT Edge** IOT hub'ınızın sayfası.

1. Ayrıntılarını dağıtımı ile hedeflenen IOT Edge cihazı seçin.
1. Cihaz ayrıntılarında blob storage modülünde her ikisi de olarak listelendiğini doğrulayın **dağıtımda belirtilen** ve **cihaz tarafından bildirilen**.

Bu cihazda çalışmaya ve IOT Hub'ına geri bildirilen modülü için birkaç dakika sürebilir. Güncelleştirilmiş bir durum görmek için sayfayı yenileyin.

## <a name="deploy-from-visual-studio-code"></a>Visual Studio kodunu dağıtma

Azure IOT Edge, uç çözümleri geliştirmenize yardımcı olması için Visual Studio code'da şablonları sağlar. Bir blob depolama modülü ile yeni bir IOT Edge çözümü oluşturma ve dağıtım bildirimini yapılandırmak için aşağıdaki adımları kullanın.

1. Seçin **görünümü** > **komut paleti**.

1. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**.

   ![Yeni IOT Edge çözümü çalıştırın](./media/how-to-develop-csharp-module/new-solution.png)

   Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde çözüm dosyalarını oluşturmak Visual Studio Code için konum seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | Seçin **varolan bir modülle (Enter tam görüntü URL'si)**. |
   | Modül adı sağlayın | Bir tüm küçük adı, modül için gibi girin **azureblobstorage**.<br /><br />IOT Edge modülü, Azure Blob Depolama için küçük bir ad kullanmak önemlidir. IOT Edge modüllerini başvururken büyük/küçük harfe ve küçük harfler için depolama SDK'sı varsayılan olarak. |
   | İçin modülü Docker görüntüsü sağlayın | Resim URI'sini girin: **mcr.microsoft.com/azure-blob-storage:latest** |

   Visual Studio Code, sağlanan bir IOT Edge çözümü oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır. Çözüm şablonu, blob depolama modülü görüntüsünü içeren bir dağıtım bildirimi şablonu oluşturur, ancak yapılandırmanız gerekir modülün oluşturma seçenekleri.

1. Açık *deployment.template.json* yeni çözüm çalışma alanı ve Bul **modülleri** bölümü. Şu yapılandırma değişiklikleri yapın:

   1. Silme **tempSensor** haliyle modülü, bu dağıtım için gerekli değildir.

   1. Aşağıdaki kodu kopyalayıp `createOptions` alan:

      ```json
      "Env":[
       "LOCAL_STORAGE_ACCOUNT_NAME=<your storage account name>",
       "LOCAL_STORAGE_ACCOUNT_KEY=<your storage account key>"
      ],
      "HostConfig":{
        "Binds": ["<storage directory bind>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Modül createOptions - Visual Studio Code güncelleştir](./media/how-to-store-data-blob/create-options.png)

1. Değiştirin `<your storage account name>` hatırlayabileceğiniz bir ad. Hesabı adları 3 ila 24 karakter uzunluğunda, küçük harf ve sayı ile olmalıdır. Boşluk.

1. Değiştirin `<your storage account key>` 64 baytlık base64 anahtarına sahip. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.

1. Değiştirin `<storage directory bind>` kapsayıcı işletim sisteminize göre. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu. Depolama dizini bağlama bir konum sağlayan Cihazınızda bir modül kümesini konumda eşlenir.  

      - Linux kapsayıcıları için biçimdir  *\<depolama yolu >: / blobroot*. Örneğin, **/srv/containerdata: / blobroot** veya **my-volume: / blobroot**.
      - Windows kapsayıcıları için biçimdir  *\<depolama yolu >: C: / BlobRoot*. Örneğin, **C: / ContainerData:C: / BlobRoot** veya **my-birim: C: / blobroot**.

      > [!IMPORTANT]
      > İkinci yarısında depolama dizininin modülünde belirli bir konuma işaret değeri bağlama değiştirmeyin. Depolama dizini bağlama ile her zaman bitmelidir **: / blobroot** Linux kapsayıcıları için ve **: C: / BlobRoot** Windows kapsayıcıları için.

1. Yapılandırma [katmanlama](how-to-store-data-blob.md#tiering-properties) ve [yaşam süresi](how-to-store-data-blob.md#time-to-live-properties) aşağıdaki JSON ekleyerek modülünüzde özelliklerini *deployment.template.json* dosya. Her bir özellik ile uygun bir değer yapılandırın ve dosyayı kaydedin.

   ```json
   "<your azureblobstorageoniotedge module name>":{
     "properties.desired": {
       "ttlSettings": {
         "ttlOn": <true, false>,
         "timeToLiveInMinutes": <timeToLiveInMinutes>
       },
       "tieringSettings": {
         "tieringOn": <true, false>,
         "backlogPolicy": "<NewestFirst, OldestFirst>",
         "remoteStorageConnectionString": "DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>",
         "tieredContainers": {
           "<source container name1>": {
             "target": "<target container name1>"
           }
         }
       }
     }
   }
   ```

   ![azureblobstorageoniotedge - Visual Studio Code için istenen özelliklerini ayarlama](./media/how-to-store-data-blob/tiering_ttl.png)

   Modülünüzün dağıtıldıktan sonra katman ayarlamayı ve TTL yapılandırma hakkında daha fazla bilgi için bkz: [modül İkizi Düzenle](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin). Kapsayıcı hakkında daha fazla bilgi için oluşturma seçenekleri, ilke ve istenen durum görür yeniden [EdgeAgent istenen özelliklerini](module-edgeagent-edgehub.md#edgeagent-desired-properties).

1. *deployment.template.json* dosyasını kaydedin.

1. Sağ **deployment.template.json** seçip **oluşturmak IOT Edge dağıtım bildirimi**.

1. Visual Studio Code içinde sağlanan bilgileri alan *deployment.template.json* ve yeni bir dağıtım bildirimi dosyası oluşturmak için kullanır. Yeni bir dağıtım bildirimi oluşturulan **config** çözüm çalışma alanınızda bir klasör. Bu dosyayı açtıktan sonra adımları izleyebilirsiniz [Visual Studio Code için Azure IOT Edge'e dağıtma modüllerden](how-to-deploy-modules-vscode.md) veya [Azure CLI 2.0 ile Azure IOT Edge'e dağıtma modülleri](how-to-deploy-modules-cli.md).

## <a name="deploy-multiple-module-instances"></a>Birden çok modül örneği dağıtma

IOT Edge modülü Azure Blob Depolama birden çok örneğini dağıtmak istiyorsanız, farklı depolama yolu sağlayın ve değiştirmek için ihtiyaç duyduğunuz `HostPort` modülü bağlar değeri. Blob depolama modülleri, bağlantı noktasını 11002 kapsayıcıdaki her zaman kullanıma sunma, ancak konakta bağlı hangi bağlantı noktası bildirebilirsiniz.

Düzen **kapsayıcı oluşturma seçenekleri** (Azure portalında) veya **createOptions** alan (içinde *deployment.template.json* Visual Studio code'da dosya) değiştirmekiçin`HostPort` değeri:

```json
"PortBindings":{
  "11002/tcp": [{"HostPort":"<port number>"}]
}
```

Ek blob depolama modüllerini bağladığınızda, uç nokta güncel ana bilgisayar bağlantı noktasına işaret edecek şekilde değiştirin.

## <a name="next-steps"></a>Sonraki adımlar

Nasıl iş dağıtım bildirimleri ve bunların nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [nasıl IOT Edge modülleri, yapılandırılmış, yeniden kaldırılabilir ve anlamak](module-composition.md).
