---
title: Blok blobları cihazlarda - Azure IOT Edge Store | Microsoft Docs
description: IOT Edge cihazınıza uçta veri depolamak için bir Azure Blob Depolama modülü dağıtın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 03/07/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 3a0df408e70ed61355ffba319f6261f90d8e4348
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2019
ms.locfileid: "58499167"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>IOT Edge (Önizleme) Azure Blob Depolama ile uçta veri Store

IOT Edge üzerinde Azure Blob Depolama sağlayan bir [blok blobu](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) uçta depolama çözümü. Bir blob depolama modülü IOT Edge Cihazınızda bir Azure blok blob hizmeti gibi davranır, ancak blok blobları, IOT Edge Cihazınızda yerel olarak depolanır. Aynı Azure depolama SDK'ın yöntemleri kullanarak bloblarınızın erişebilir veya zaten kullanılan blob API çağrılarını engelle. 

Bu modül ile birlikte gelen **otomatik katmanlama** ve **otomatik sona erme** özellikleri.

> [!NOTE]
> Şu anda otomatik katmanlama ve otomatik bitiş işlevsellik yalnızca Linux AMD64 ve Linux ARM32 kullanılabilir.

**Otomatik katmanlama** otomatik olarak, yerel blob depolamadan aralıklı Internet bağlantısı desteği ile verileri azure'a karşıya yüklemeye izin veren yapılandırılabilir bir işlevdir. İzin verir:
- Kapat Kapat katmanlama özelliği
- NewestFirst veya OldestFirst gibi Azure, veri olacak sırasını kopyalanmasını seçin
- Verilerinizi karşıya istediğiniz Azure depolama hesabı belirtin.
- Azure'a yüklemek istediğiniz kapsayıcılarını belirtin. Bu modül, hem kaynak hem de hedef kapsayıcı adları belirtmenize olanak sağlar.
- BLOB katmanlama tam (kullanarak `Put Blob` işlemi) ve düzeyi katmanlama engelleyin (kullanarak `Put Block` ve `Put Block List` işlemleri).

Bu modül, blob bloklarını oluşuyorsa blok düzeyinde, katmanlama kullanır. Bazı yaygın senaryolar şunlardır:
- Uygulamanız bazı bloklarını daha önce yüklenen bir blobu güncelleştirir, bu modül, yalnızca güncelleştirilmiş engeller ve tüm blob yükleyeceksiniz.
- Modül blob karşıya yükleme ve bağlantıyı tekrar tekrar yalnızca kalan engeller ve tüm blob yükleyeceksiniz olduğunda internet bağlantısı çalıştırmayı, geçer.

Bir blob karşıya yükleme sırasında bir beklenmeyen işlem sonlandırma (örneğin, güç kesintisi) durumda modülü tekrar çevrimiçi olduğunda karşıya yükleme için son tüm blokların yeniden yüklenecek.

**Otomatik-sona erme** bir yapılandırılabilir burada bu modülü otomatik olarak siler, BLOB'ları yerel depolama alanından yaşam süresi (TTL) süresi dolduğunda bir işlevdir. Bu işlem birkaç dakika içinde ölçülür. İzin verir:
- Kapat otomatik sona erme özelliğini Aç
- TTL dakika cinsinden belirtin.

Veri, videoları, resimleri, Finans verileri, hastane verilerini veya buluta aktarılan veya yerel olarak işlenen, daha sonra yerel olarak depolanması için gereken tüm verileri olduğu gibi senaryoları bu modülü kullanmak için iyi örneklerdir.

Bu makalede, Azure Blob Depolama, blob hizmeti, IOT Edge Cihazınızda çalışan IOT Edge kapsayıcısı üzerinde dağıtmak için yönergeler sağlar. 

>[!NOTE]
>IOT Edge üzerinde Azure Blob Depolama, içinde [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 

Hızlı bir giriş için videoyu izleyin
> [!VIDEO https://www.youtube.com/embed/wkprcfVidyM]

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

Azure marketi, IOT Edge, IOT Edge üzerinde Azure Blob Depolama da dahil olmak üzere doğrudan IOT Edge cihazlarınıza, dağıtılan modülleri sağlar. Azure portalından modülü dağıtmak için aşağıdaki adımları izleyin.

1. İçinde [Azure portalında](https://portal.azure.com), "Azure Blob Depolama üzerinde IOT Edge" arayın. Ve **seçin** marketten arama sonucu.

   ![Market araması modülü oluşturma](./media/how-to-store-data-blob/marketplace-module.png)

2. Bu modül almak için bir IOT Edge cihazı seçin. Üzerinde **hedef cihazlar IOT Edge modülü için** sayfasında, aşağıdaki bilgileri sağlayın:

   1. Seçin **abonelik** kullanmakta olduğunuz IOT hub'ı içerir.

   2. Seçin, **IOT hub'ı**.

   3. Biliyorsanız, **IOT Edge cihaz adı**, metin kutusuna girin. Ya da seçin **cihaz bulma** IOT Edge cihazlarının IOT hub'ınızda bir listesinden seçmek için. 
   
   4. **Oluştur**’u seçin.

   Azure Marketi'nden bir IOT Edge modülü seçtiniz ve modül almak için bir IOT Edge cihazı seçmiş göre tam olarak nasıl modülü dağıtılacak tanımlamanıza yardımcı olan üç adımlık Sihirbazı yönlendirilirsiniz.

3. İçinde **Ekle modülleri** adım kümesi modülleri Sihirbazı'nın dikkat **AzureBlobStorageonIoTEdge** modülü altında listelenen zaten **dağıtım modülleri**. 

2. Blob depolama modülü, modül ayrıntıları açmak için dağıtım modülleri listesinden seçin. 

   ![Modül ayrıntıları açmak için modül adı seçin](./media/how-to-store-data-blob/open-module-details.png)

3. Üzerinde **özel IOT Edge modülleri** sayfasında, aşağıdaki adımlarla IOT Edge modülü Azure Blob Depolama güncelleştirin:

   1. Modül değiştirme **adı** küçük harfli olması için. Modülü isterseniz yeniden adlandırın veya kullanın `azureblobstorageoniotedge`. 

      >[!IMPORTANT]
      >Azure IOT Edge modülleri çağrı yapmak ve depolama SDK'sı varsayılan olarak küçük harfe duyarlıdır. IOT Edge modülü Azure Blob Depolama bağlantılarınızı kesintiye emin olmak için küçük bir ad verin. 

   2. Varsayılan **kapsayıcı oluşturma seçenekleri** kapsayıcınızı gereken bağlantı noktası bağlamaları, ancak aynı zamanda depolama hesap bilgilerinizi ve depolama dizini için bir bağlama Cihazınızda eklemeniz gerekir. Portalında JSON'u aşağıdaki JSON ile üzerine yaz:
    
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
   3. Kopyaladığınız JSON aşağıdaki bilgilerle güncelleştirin: 

      * Değiştirin `<your storage account name>` hatırlayabileceğiniz bir ad. Hesap adları, küçük harf ve sayı ile uzun üç için yirmi dört karakter arasında olmalıdır.
      * Değiştirin `<your storage account key>` 64 baytlık base64 anahtarına sahip. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız.
      * Değiştirin `<storage directory bind>` kapsayıcı işletim sisteminize bağlı olarak. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu. Depolama dizini bağlama bir konum sağlayan Cihazınızda bir modül kümesini konumda eşlenir. 

         * Linux kapsayıcıları:  **\<depolama yolu >: / blobroot**. Örneğin, / srv/containerdata: / blobroot. Veya, birim my: / blobroot. 
         * Windows kapsayıcıları:  **\<depolama yolu >: C: / BlobRoot**. Örneğin, C: / ContainerData:C: / BlobRoot. Veya, my-birim: C: / blobroot.
   
      > [!IMPORTANT]
      > İkinci yarısında depolama dizininin modülünde belirli bir konuma işaret değeri bağlama değiştirmeyin. Depolama dizini bağlama ile her zaman bitmelidir **: / blobroot** Linux kapsayıcıları için ve **: C: / BlobRoot** Windows kapsayıcıları için.

      ![Güncelleştirme modülü kapsayıcı oluşturma seçenekleri - portal](./media/how-to-store-data-blob/edit-module.png)

   4. Ayarlama [otomatik katmanlama ve otomatik sona erme](#configure-auto-tiering-and-auto-expiration-via-azure-portal) istenen özellikleri. Listesi [otomatik katmanlama](#auto-tiering-properties) ve [otomatik sona erme](#auto-expiration-properties) özellikler ve olası değerleri. 

   5. **Kaydet**’i seçin. 

4. Seçin **sonraki** sihirbazın sonraki adıma devam etmek için.
5. İçinde **yolları belirtin** seçin, adım **sonraki**.
6. İçinde **gözden geçirme dağıtım** seçin, adım **Gönder**.
7. Dağıtım gönderdikten sonra geri **IOT Edge** IOT hub'ınızın sayfası. Ayrıntılarını dağıtımı ile hedeflenen IOT Edge cihazı seçin. 
8. Cihaz ayrıntılarında blob storage modülünde her ikisi de olarak listelendiğini doğrulayın **dağıtımda belirtilen** ve **cihaz tarafından bildirilen**. Bu cihazda çalışmaya ve IOT Hub'ına geri bildirilen modülü için birkaç dakika sürebilir. Güncelleştirilmiş bir durum görmek için sayfayı yenileyin. 

### <a name="visual-studio-code-templates"></a>Visual Studio Code şablonları

Azure IOT Edge, uç çözümleri geliştirmenize yardımcı olması için Visual Studio code'da şablonları sağlar. Bu adımları sahip olmanızı gerektirir [Visual Studio Code](https://code.visualstudio.com/) geliştirme makinenizde yüklü ve yapılandırılmış [Azure IOT Araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

Bir blob depolama modülü ile yeni bir IOT Edge çözüm oluşturmak için aşağıdaki adımları kullanın ve dağıtım bildirimini yapılandırın. 

1. Seçin **görünümü** > **komut paleti**. 

2. Komut Paleti'nde girin ve şu komutu çalıştırın **Azure IOT Edge: Yeni bir IOT Edge çözüm**. Çözümünüzü oluşturmak için komut paletindeki yönergeleri izleyin.

   | Alan | Değer |
   | ----- | ----- |
   | Klasör seçin | Geliştirme makinenizde VS Code'un çözüm dosyalarını oluşturmak için kullanacağı konumu seçin. |
   | Çözüm adı sağlayın | Çözümünüz için açıklayıcı bir ad girin veya varsayılan değerleri kabul **EdgeSolution**. |
   | Modül şablonunu seçin | Seçin **varolan bir modülle (Enter tam görüntü URL'si)**. |
   | Modül adı sağlayın | Bir tüm küçük adı, modül için gibi girin **azureblobstorage**.<br><br>IOT Edge modülü, Azure Blob Depolama için küçük bir ad kullanmak önemlidir. IOT Edge modüllerini başvururken büyük/küçük harfe ve küçük harfler için depolama SDK'sı varsayılan olarak. |
   | İçin modülü Docker görüntüsü sağlayın | Resim URI'sini girin: **mcr.microsoft.com/azure-blob-storage:latest** |

   VS Code, sağlanan bir IOT Edge çözümü oluşturur ve ardından yeni bir pencerede yüklenir bilgilerini alır. Çözüm şablonu, blob depolama modülü görüntüsünü içeren bir dağıtım bildirimi şablonu oluşturur, ancak yapılandırmanız gerekir modülün oluşturma seçenekleri. 

3. Açık **deployment.template.json** yeni çözüm çalışma alanı ve Bul **modülleri** bölümü. Şu yapılandırma değişiklikleri yapın:

   1. Silme **tempSensor** haliyle modülü, bu dağıtım için gerekli değildir. 

   2. Aşağıdaki kodu kopyalayıp **createOptions** modülünüzün blob depolama alanı: 

      ```json
      "Env": [
        "LOCAL_STORAGE_ACCOUNT_NAME=$STORAGE_ACCOUNT_NAME","LOCAL_STORAGE_ACCOUNT_KEY=$STORAGE_ACCOUNT_KEY"
      ],
      "HostConfig":{
        "Binds": ["<storage directory bind>"],
        "PortBindings":{
          "11002/tcp": [{"HostPort":"11002"}]
        }
      }
      ```

      ![Modül createOptions - VS Code güncelleştir](./media/how-to-store-data-blob/create-options.png)

4. JSON oluşturma seçeneklerinde güncelleştirme `<storage directory bind>` kapsayıcı işletim sisteminize bağlı olarak. Adını sağlayın bir [birim](https://docs.docker.com/storage/volumes/) veya istediğiniz verileri depolamak için blob modülü IOT Edge Cihazınızda dizinine mutlak yolu. Depolama dizini bağlama bir konum sağlayan Cihazınızda bir modül kümesini konumda eşlenir.  

   * Linux kapsayıcıları:  **\<depolama yolu >: / blobroot**. Örneğin, / srv/containerdata: / blobroot. Veya, birim my: / blobroot.
   * Windows kapsayıcıları:  **\<depolama yolu >: C: / BlobRoot**. Örneğin, C: / ContainerData:C: / BlobRoot. Veya, my-birim: C: / blobroot.
   
   > [!IMPORTANT]
   > İkinci yarısında depolama dizininin modülünde belirli bir konuma işaret değeri bağlama değiştirmeyin. Depolama dizini bağlama ile her zaman bitmelidir **: / blobroot** Linux kapsayıcıları için ve **: C: / BlobRoot** Windows kapsayıcıları için.

5. Yapılandırma [otomatik katmanlama ve otomatik sona erme](#configure-auto-tiering-and-auto-expiration-via-vscode). Listesi [otomatik katmanlama](#auto-tiering-properties) ve [otomatik sona erme](#auto-expiration-properties) özellikleri

6. **deployment.template.json** dosyasını kaydedin.

7. Açık **.env** çözüm çalışma alanınızdaki dosya. 

8. Kapsayıcı kayıt defteri kimlik bilgilerini almak için .env dosyasında ayarlandı, ancak genel kullanıma açık olduğundan, blob depolama resim için gerekmez. Bunun yerine, dosyanın iki yeni ortam değişkenlerini değiştirin: 

   ```env
   STORAGE_ACCOUNT_NAME=
   STORAGE_ACCOUNT_KEY=
   ```

9. İçin bir değer girin `STORAGE_ACCOUNT_NAME`, hesabı adları küçük harf ve sayı ile uzun üç için yirmi dört karakter olmalıdır. Bir 64 baytlık base64 anahtarı için `STORAGE_ACCOUNT_KEY`. Bir anahtar gibi araçlarla oluşturabilirsiniz [GeneratePlus](https://generate.plus/en/base64?gp_base64_base[length]=64). Diğer modüllerden blob depolamaya erişmek için bu kimlik bilgilerini kullanacaksınız. 

   Boşluk veya tırnak sağladığınız değerler dahil değildir. 

10. **.env** dosyasını kaydedin. 

11. Sağ **deployment.template.json** seçip **oluşturmak IOT Edge dağıtım bildirimi**. 

12. Visual Studio Code deployment.template.json ve .env sağlanan ve yeni bir dağıtım bildirimi dosyası oluşturmak için kullandığı bilgileri alır. Yeni bir dağıtım bildirimi oluşturulan **config** çözüm çalışma alanınızda bir klasör. Bu dosyayı açtıktan sonra adımları izleyebilirsiniz [Visual Studio Code için Azure IOT Edge'e dağıtma modüllerden](how-to-deploy-modules-vscode.md) veya [Azure CLI 2.0 ile Azure IOT Edge'e dağıtma modülleri](how-to-deploy-modules-cli.md).

## <a name="auto-tiering-and-auto-expiration-properties-and-configuration"></a>Otomatik katmanlama ve otomatik süre sonu özellikleri ve yapılandırma

Otomatik katmanlama ve otomatik süre sonu özellikleri ayarlamak için istenen özellikleri kullanın. Bunlar ayarlanabilir dağıtımı sırasında veya yeniden dağıtmaya gerek kalmadan modül ikizi düzenleyerek daha sonra değiştirildi. "Modül İkizi" denetimi için önerdiğimiz `reported configuration` ve `configurationValidation` değerleri doğru şekilde yayılır emin olmak için.

### <a name="auto-tiering-properties"></a>Otomatik katmanlama özellikleri 
Bu ayar adı `tieringSettings`

| Alan | Olası Değerler | Açıklama |
| ----- | ----- | ---- |
| tieringOn | TRUE, false | Varsayılan olarak ayarlanmış `false`etkinleştirmek istiyorsanız üzerinde kümesine `true`|
| backlogPolicy | NewestFirst, OldestFirst | Hangi veri olacak sırasını Azure'a kopyalanan seçmenize olanak sağlar. Varsayılan olarak ayarlanmış `OldestFirst`. Sırası blobun son değiştirme zamanına göre belirlenir |
| remoteStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` Verilerinizi istediğiniz Azure depolama hesabı belirtmenizi sağlar bir bağlantı dizesi yüklenir. Belirtin `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix`. Uygun EndpointSuffix burada veri karşıya yüklenecek, Azure ekleyin, Global Azure, Azure kamu ve Microsoft Azure Stack için değişir. |
| tieredContainers | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Azure'a yüklemek istediğiniz kapsayıcı adları belirtmenizi sağlar. Bu modül, hem kaynak hem de hedef kapsayıcı adları belirtmenize olanak sağlar. Hedef kapsayıcı adı belirtmezseniz, otomatik olarak kapsayıcı adı olarak atar `<IoTHubName>-<IotEdgeDeviceName>-<ModuleName>-<ContainerName>`. Hedef kapsayıcı adı, kullanıma alma olası değerler sütunu için şablon dizeleri oluşturabilirsiniz. <br>* %h -> IOT hub'ı adı (3-50 karakter). <br>* IOT cihaz kimliği (1 129 karakter için) -> %d. <br>* %m -> modül adı (1 ile 64 karakter arasında). <br>* Kaynak kapsayıcı adı (3 63 karakter için) -> %c. <br><br>En büyük boyutunu kapsayıcı adı 63 karakterden, kesim kapsayıcısının boyutunu aşarsa, otomatik olarak hedef kapsayıcı adı her bölüm (IoTHubName, IotEdgeDeviceName, modül adı, ContainerName) 15 karakter atanırken 63 karakterden oluşabilir. |

### <a name="auto-expiration-properties"></a>Otomatik süre sonu özellikleri
Bu ayar adı `ttlSettings`

| Alan | Olası Değerler | Açıklama |
| ----- | ----- | ---- |
| ttlOn | TRUE, false | Varsayılan olarak ayarlanmış `false`etkinleştirmek istiyorsanız üzerinde kümesine `true`|
| timeToLiveInMinutes | `<minutes>` | TTL dakika cinsinden belirtin. TTL süresi dolduğunda modülü otomatik olarak, BLOB'ları yerel depolama alanından siler |

### <a name="configure-auto-tiering-and-auto-expiration-via-azure-portal"></a>Azure portalı üzerinden otomatik katmanlama ve otomatik zaman aşımı yapılandırma

Otomatik katmanlama etkinleştirmek için istediğiniz özellikleri ayarlayın ve otomatik süresi, bu değerleri ayarlayabilirsiniz:

- **İlk dağıtım sırasında**: JSON'da kopyalama **istenen özellikler kümesi modül ikizi** kutusu. Uygun değeri ile her bir özellik yapılandırmak, kaydedin ve dağıtım ile devam edin.

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

  ![Otomatik katmanlama ve otomatik sona erme özelliklerini ayarlama](./media/how-to-store-data-blob/iotedge_custom_module.png)

- **Modül, "Modül İkizi kimlik" özelliği aracılığıyla dağıtıldıktan sonra**: "Modülü kimlik İkizine" Bu modülün gidin, istenen Özellikler altında JSON dosyasını kopyalayın, her bir özellik uygun değerle yapılandırın ve kaydedin. "Modül İkizi kimlik" Json özelliği ekleyin veya güncelleştirin her zaman emin olun istenen `reported configuration` bölümü, değişiklikleri yansıtır ve `configurationValidation` bölümünde her bir özellik için bildirmektedir.

   ```json 
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

   ```

![katmanlama + ttl module_identity_twin](./media/how-to-store-data-blob/module_identity_twin.png) 

### <a name="configure-auto-tiering-and-auto-expiration-via-vscode"></a>Otomatik katmanlama ve sona erme otomatik VSCode aracılığıyla yapılandırma

- **İlk dağıtım sırasında**: Ekleme Bu modül için istenen özelliklerini tanımlamak için deployment.template.json JSON'da aşağıda. Her bir özellik uygun değerle yapılandırın ve kaydedin.

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

Bu modül için istenen özellikler örneği aşağıdadır: ![azureblobstorageoniotedge - VS Code için istenen özelliklerini ayarlama](./media/how-to-store-data-blob/tiering_ttl.png)

- **Modül, "Modül İkizi" dağıtıldıktan sonra**: [Modül İkizi Düzenle](https://github.com/Microsoft/vscode-azure-iot-toolkit/wiki/Edit-Module-Twin) Bu modülün istenen Özellikler altında JSON dosyasını kopyalayın, her bir özellik uygun değerle yapılandırın ve kaydedin. "Modül İkizi" Json özelliği ekleyin veya güncelleştirin her zaman emin olun istenen `reported configuration` bölümü, değişiklikleri yansıtır ve `configurationValidation` bölümünde her bir özellik için bildirmektedir.

   ```json 
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

   ```
## <a name="logs"></a>Günlükler

Lütfen için yönergeleri izleyin [IOT Edge modülleri, docker günlüklerini Yapılandır](production-checklist.md#set-up-logs-and-diagnostics)

## <a name="connect-to-your-blob-storage-module"></a>Blob depolama modülüne bağlayın

Hesap adı ve IOT Edge Cihazınızda blob depolamaya erişmek, bir modül için yapılandırılan hesap anahtarı kullanabilirsiniz. 

IOT Edge Cihazınızı herhangi bir depolama alanı için blob uç nokta olarak belirtmek için yaptığınız istekleri. Yapabilecekleriniz [bir açık depolama uç noktası için bir bağlantı dizesi oluşturma](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) IOT Edge cihaz bilgileri ve yapılandırdığınız hesabı adını kullanarak. 

1. "Azure Blob Depolama üzerinde IOT Edge" çalıştığı aynı uç cihaza dağıtılan modülleri için blob uç noktadır: `http://<module name>:11002/<account name>`. 
2. Modüller için dağıtılan burada "Azure Blob Depolama üzerinde IOT Edge" çalışıyor ve ardından blob uç noktası, kurulumunuza olarak sınır cihazı değerinden farklı edge cihazı üzerindeki: `http://<device IP >:11002/<account name>` veya `http://<IoT Edge device hostname>:11002/<account name>` veya `http://<FQDN>:11002/<account name>`

## <a name="deploy-multiple-instances"></a>Birden fazla örnek dağıtın

IOT Edge üzerinde Azure Blob Depolama birden çok örneğini dağıtmak istiyorsanız, farklı depolama yolu girin ve modülü bağlar hostport değerini değiştirmeniz gerekir. Blob depolama modülleri, bağlantı noktasını 11002 kapsayıcıdaki her zaman kullanıma sunma, ancak konakta bağlı hangi bağlantı noktası bildirebilirsiniz. 

Düzenleme modülü hostport değerini değeri değiştirmek için seçenekler oluşturun:

```json
\"PortBindings\": {\"11002/tcp\": [{\"HostPort\":\"<port number>\"}]}
```

Ek blob depolama modüllerini bağladığınızda, uç nokta güncel ana bilgisayar bağlantı noktasına işaret edecek şekilde değiştirin. 

## <a name="try-it-out"></a>Deneyin

### <a name="azure-blob-storage-quickstart-samples"></a>Azure Blob Depolama hızlı başlangıcı örnekleri
Çeşitli dillerde örnek kodlar sağladık hızlı başlangıçlar Azure Blob Depolama belgeleri içerir. IOT Edge üzerinde Azure Blob Depolama, blob uç noktası, blob depolama modülünüzde işaret edecek şekilde değiştirerek test etmek için bu örnekleri yeniden çalıştırabilirsiniz. Adımlarını izleyin [, blob depolama modülüne bağlayın](#connect-to-your-blob-storage-module)

Şu hızlı başlangıçlarda olarak blob depolama modülü ile birlikte IOT Edge modülleri dağıtabilirsiniz böylece IOT Edge ile de desteklenen dilleri kullanın:

* [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
* [Java](../storage/blobs/storage-quickstart-blobs-java.md)
* [Python](../storage/blobs/storage-quickstart-blobs-python.md)
* [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md) 

### <a name="azure-storage-explorer"></a>Azure Depolama Gezgini
"Azure depolama, yerel depolama hesabınıza bağlanmak için Gezgini" de deneyebilirsiniz. İle çalışır [Azure Depolama Gezgini sürüm 1.5.0](https://github.com/Microsoft/AzureStorageExplorer/releases/tag/v1.5.0).

> [!NOTE]
> Bir bağlantı için bir yerel depolama hesabı ekleme veya yerel depolama hesabındaki kapsayıcıları oluşturma gibi aşağıdaki adımları gerçekleştirirken hatalarla karşılaşabilirsiniz. Lütfen yoksay ve yenileyin. 

1. Azure Depolama Gezgini indirip yükleme
2. Bağlantı dizesi kullanarak Azure Depolama'ya Bağlan
3. Bağlantı dizesini belirtin: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`
4. Bağlanmak için adımlara geçin.
5. Kapsayıcı içinde yerel depolama hesabınızı oluşturun
6. Blok blobları olarak dosyalarını yüklemeye başlayın.
   > [!NOTE]
   > Sayfa blobları yüklemek için onay kutusunun işaretini kaldırın. Bu modül, sayfa bloblarını desteklemez. Bu .iso, .vhd, .vhdx veya büyük dosyaları gibi dosyalar karşıya yüklenirken komut istemi alırsınız.

7. Verileri karşıya yüklemekte olduğunuz Burada, Azure depolama hesaplarınıza bağlanmak seçebilirsiniz. Bu, tek bir görünümde, yerel depolama hesabı ve Azure depolama hesabı için sağlar

## <a name="supported-storage-operations"></a>Desteklenen depolama işlemleri

BLOB Depolama modülleri IOT Edge üzerinde aynı Azure depolama SDK'ları kullanın ve blok blob uç noktaları için Azure depolama API 2017-04-17 sürümünü tutarlıdır. Sonraki sürümlerde, müşteri gereksinimlerine bağlıdır.

Tüm Azure Blob Depolama işlemleri, IOT Edge üzerinde Azure Blob Depolama tarafından desteklenir. Aşağıdaki bölümde, desteklenen ve desteklenmeyen işlemleri listeler.

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
* Alma ve kapsayıcı ACL ayarlama
* Kümesi kapsayıcı meta verileri

Desteklenmeyen:
* Kira kapsayıcı

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
* Blok yerleştirme
* Koy ve engelleme listesine alın

Desteklenmeyen:
* URL'den blok yerleştirme

## <a name="feedback"></a>Geri bildirim:
Geri bildiriminiz bizim için bu modülü ve özelliklerini kullanışlı ve kullanımı kolay hale getirmek için çok önemlidir. Lütfen görüşlerinizi paylaşın ve nasıl geliştirebileceğimizi bize bildirin.

Adresinden bize ulaşın absiotfeedback@microsoft.com 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [Azure Blob Depolama](../storage/blobs/storage-blobs-introduction.md)

