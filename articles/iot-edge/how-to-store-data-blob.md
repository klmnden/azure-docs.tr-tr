---
title: Blok blobları cihazlarda - Azure IOT Edge Store | Microsoft Docs
description: Katmanlama ve yaşam süresi özelliklerini anlamanıza, desteklenen blob depolama işlemleri görmek ve blob depolama hesabınıza bağlanın.
author: arduppal
manager: mchad
ms.author: arduppal
ms.reviewer: arduppal
ms.date: 06/19/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: bb6cd43c77c31874115250d13f8d4067b3db7b36
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67804981"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>IOT Edge (Önizleme) Azure Blob Depolama ile uçta veri Store

IOT Edge üzerinde Azure Blob Depolama sağlayan bir [blok blobu](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) uçta depolama çözümü. Bir blob depolama modülü IOT Edge Cihazınızda bir Azure blok blob hizmeti gibi davranır, ancak blok blobları, IOT Edge Cihazınızda yerel olarak depolanır. Aynı Azure depolama SDK'ın yöntemleri kullanarak bloblarınızın erişebilir veya zaten kullanılan blob API çağrılarını engelle.

Bu modül ile birlikte gelen **deviceToCloudUpload** ve **deviceAutoDelete** özellikleri.
> [!NOTE]
> IOT Edge üzerinde Azure Blob Depolama, içinde [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Hızlı bir giriş için videoyu izleyin
> [!VIDEO https://www.youtube.com/embed/QhCYCvu3tiM]

**deviceToCloudUpload** otomatik olarak, yerel blob depolamadan aralıklı Internet bağlantısı desteği ile verileri azure'a karşıya yüklemeye izin veren yapılandırılabilir bir işlevdir. İzin verir:

- Kapat deviceToCloudUpload özelliğini açın.
- Verileri Azure NewestFirst veya OldestFirst gibi kopyalandığı sırasını seçin.
- Verilerinizi karşıya istediğiniz Azure depolama hesabı belirtin.
- Azure'a yüklemek istediğiniz kapsayıcılarını belirtin. Bu modül, hem kaynak hem de hedef kapsayıcı adları belirtmenize olanak sağlar.
- Bulut depolama için karşıya yükleme tamamlandıktan sonra BLOB'ları hemen silme olanağı seçin
- Blobun karşıya tam (kullanarak `Put Blob` işlemi) ve blok düzeyinde karşıya yükleme (kullanarak `Put Block` ve `Put Block List` işlemleri).

Bu modül, blob bloklarını oluşur blok düzeyinde karşıya yükleme, kullanır. Bazı yaygın senaryolar şunlardır:

- Uygulamanız bazı bloklarını daha önce yüklenen bir blobu güncelleştirir, bu modül, yalnızca güncelleştirilmiş engeller ve tüm blob karşıya yükler.
- Modül blob karşıya yükleme ve bağlantı tekrar tekrar yalnızca kalan engeller ve tüm blob karşıya yüklemeleri olduğunda internet bağlantısı, kaybolduktan.

Bir blob karşıya yükleme sırasında bir beklenmeyen işlem sonlandırma (örneğin, güç kesintisi) durumda modülü tekrar çevrimiçi olduğunda karşıya yükleme için son tüm blokların yeniden yüklenecek.

**deviceAutoDelete** bir yapılandırılabilir burada modülü otomatik olarak siler, BLOB'ları yerel depolama alanından (dakika cinsinden ölçülür) belirtilen süre dolduğunda bir işlevdir. İzin verir:

- Kapat deviceAutoDelete özelliğini açın.
- BLOB'ları otomatik olarak silineceğini belirten sonra süreyi dakika cinsinden (deleteAfterMinutes) belirtin.
- Blob deleteAfterMinutes değeri dolarsa yüklenirken bekletme özelliği seçin.

Veri, videoları, resimleri, Finans verileri, hastane verilerini veya buluta aktarılan veya yerel olarak işlenen, daha sonra yerel olarak depolanması için gereken tüm verileri olduğu gibi senaryoları bu modülü kullanmak için iyi örneklerdir.

Bu makalede, Azure Blob Depolama, blob hizmeti, IOT Edge Cihazınızda çalışan IOT Edge kapsayıcısı üzerinde ilgili kavramlar açıklanır.

## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

- Hızlı Başlangıç adımları izleyerek bir IOT Edge cihazı geliştirme makinenizde veya bir sanal makine kullanabilirsiniz [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md).

- IOT Edge modülü Azure Blob Depolama, aşağıdaki cihaz yapılandırmalarını destekler:

  | İşletim sistemi | AMD64 | ARM32v7 | ARM64 |
  | ---------------- | ----- | ----- | ---- |
  | Raspbian Uzat | Hayır | Evet | Hayır |  
  | Ubuntu Server 16.04 | Evet | Hayır | Evet |
  | Ubuntu Server 18.04 | Evet | Hayır | Evet |
  | Windows 10 IOT Enterprise, derleme 17763 | Evet | Hayır | Hayır |
  | Windows Server 2019, derleme 17763 | Evet | Hayır | Hayır |
  

Bulut kaynakları:

Azure'da standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).

## <a name="devicetocloudupload-and-deviceautodelete-properties"></a>deviceToCloudUpload ve deviceAutoDelete özellikleri

İstenen özellikleri deviceToCloudUploadProperties ve deviceAutoDeleteProperties ayarlamak için kullanın. Bunlar ayarlanabilir dağıtımı sırasında veya yeniden dağıtmaya gerek kalmadan modül ikizi düzenleyerek daha sonra değiştirildi. "Modül İkizi" denetimi için önerdiğimiz `reported configuration` ve `configurationValidation` değerleri doğru şekilde yayılır emin olmak için.

### <a name="devicetoclouduploadproperties"></a>deviceToCloudUploadProperties

Bu ayar adı `deviceToCloudUploadProperties`

| Alan | Olası Değerler | Açıklama | Ortam değişkeni |
| ----- | ----- | ---- | ---- |
| uploadOn | true, false | Varsayılan olarak ayarlanmış `false`etkinleştirmek istiyorsanız üzerinde kümesine `true`| `deviceToCloudUploadProperties__uploadOn={false,true}` |
| uploadOrder | NewestFirst, OldestFirst | Verileri Azure'a kopyalandığı sırasını seçmenize olanak sağlar. Varsayılan olarak ayarlanmış `OldestFirst`. Sırası blobun son değiştirme zamanına göre belirlenir | `deviceToCloudUploadProperties__uploadOrder={NewestFirst,OldestFirst}` |
| cloudStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` Verilerinizi istediğiniz Azure depolama hesabı belirtmenizi sağlar bir bağlantı dizesi yüklenir. Belirtin `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix`. Uygun EndpointSuffix burada veri karşıya yüklenecek, Azure ekleyin, Global Azure, Azure kamu ve Microsoft Azure Stack için değişir. | `deviceToCloudUploadProperties__cloudStorageConnectionString=<connection string>` |
| storageContainersForUpload | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Azure'a yüklemek istediğiniz kapsayıcı adları belirtmenizi sağlar. Bu modül, hem kaynak hem de hedef kapsayıcı adları belirtmenize olanak sağlar. Hedef kapsayıcı adı belirtmezseniz, otomatik olarak kapsayıcı adı olarak atar `<IoTHubName>-<IotEdgeDeviceID>-<ModuleName>-<SourceContainerName>`. Hedef kapsayıcı adı için şablon dizeleri oluşturmak, olası değerler sütununu kontrol edin. <br>* %h -> IOT hub'ı adı (3-50 karakter). <br>* IOT Edge cihaz Kimliğine (1 129 karakter için) -> %d. <br>* %m -> modül adı (1 ile 64 karakter arasında). <br>* Kaynak kapsayıcı adı (3 63 karakter için) -> %c. <br><br>En fazla kapsayıcı adı 63 karakterden 63 karakterden, kesim kapsayıcısının boyutunu aşarsa, otomatik olarak hedef kapsayıcı adı her bölüm (IoTHubName, IotEdgeDeviceID, modül adı, SourceContainerName) 15'e atanırken boyutudur karakter. | `deviceToCloudUploadProperties__storageContainersForUpload__<sourceName>__target: <targetName>` |
| deleteAfterUpload | true, false | Varsayılan olarak ayarlanmış `false`. Bu ayarlandığında `true`, bulut depolama için karşıya yükleme tamamlandığında otomatik olarak tüm veriler silinir | `deviceToCloudUploadProperties__deleteAfterUpload={false,true}` |


### <a name="deviceautodeleteproperties"></a>deviceAutoDeleteProperties

Bu ayar adı `deviceAutoDeleteProperties`

| Alan | Olası Değerler | Açıklama | Ortam değişkeni |
| ----- | ----- | ---- | ---- |
| deleteOn | true, false | Varsayılan olarak ayarlanmış `false`etkinleştirmek istiyorsanız üzerinde kümesine `true`| `deviceAutoDeleteProperties__deleteOn={false,true}` |
| deleteAfterMinutes | `<minutes>` | Süreyi dakika cinsinden belirtin. Bu değer süresi dolduğunda modülü otomatik olarak, BLOB'ları yerel depolama alanından siler | `deviceAutoDeleteProperties__ deleteAfterMinutes=<minutes>` |
| retainWhileUploading | true, false | Varsayılan olarak ayarlanmış `true`, ve deleteAfterMinutes süresi dolarsa, bulut depolama alanına yüklenirken blob korur. Ayarlayabilirsiniz `false` ve deleteAfterMinutes dolduğu anda verileri siler. Not: Bu özellik uploadOn çalışacak şekilde ayarlanması true| `deviceAutoDeleteProperties__retainWhileUploading={false,true}` |

## <a name="configure-log-files"></a>Günlük dosyalarını yapılandırma

Günlük dosyaları, modül için yapılandırma hakkında daha fazla bilgi için bkz: [üretim en iyi](https://docs.microsoft.com/azure/iot-edge/production-checklist#set-up-logs-and-diagnostics).

## <a name="connect-to-your-blob-storage-module"></a>Blob depolama modülüne bağlayın

Hesap adı ve IOT Edge Cihazınızda blob depolamaya erişmek, bir modül için yapılandırılan hesap anahtarı kullanabilirsiniz.

IOT Edge Cihazınızı herhangi bir depolama alanı için blob uç nokta olarak belirtmek için yaptığınız istekleri. Yapabilecekleriniz [bir açık depolama uç noktası için bir bağlantı dizesi oluşturma](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) IOT Edge cihaz bilgileri ve yapılandırdığınız hesabı adını kullanarak.

- Azure Blob Depolama IOT Edge modülü üzerinde çalıştığı olarak aynı cihaza dağıtılan modülleri için blob uç noktadır: `http://<module name>:11002/<account name>`.
- Cihazı gibi dış modül veya uygulamasından veri trafiği erişebilmesi dış modüllerde ya da uygulamalar için burada IOT Edge modülü Azure Blob Depolama çalışan ve ardından, ağ kurulumuna dayalı olarak daha farklı bir cihaz üzerinde çalışan Azure Blob Depolama, IOT Edge modülü üzerinde çalıştırma, blob uç noktası şunlardan biridir:
  - `http://<device IP >:11002/<account name>`
  - `http://<IoT Edge device hostname>:11002/<account name>`
  - `http://<fully qualified domain name>:11002/<account name>`

## <a name="azure-blob-storage-quickstart-samples"></a>Azure Blob Depolama hızlı başlangıç örnekleri

Azure Blob Depolama belgeleri, çeşitli dillerde hızlı başlangıç örnek kodu içerir. IOT Edge üzerinde Azure Blob Depolama, blob uç noktanın yerel blob depolama modülüne bağlayın değiştirerek test etmek için bu örnekleri yeniden çalıştırabilirsiniz.

Aşağıdaki Hızlı Başlangıç örnekleri, blob depolama modülü ile birlikte IOT Edge modülleri olarak dağıtabilirsiniz böylece IOT Edge ile de desteklenen dilleri kullanın:

- [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [Java](../storage/blobs/storage-quickstart-blobs-java.md)
- [Python](../storage/blobs/storage-quickstart-blobs-python.md)
- [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="connect-to-your-local-storage-with-azure-storage-explorer"></a>Azure Depolama Gezgini ile yerel depolama bağlanma

Kullanabileceğiniz [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) yerel depolama hesabınıza bağlanmak için.

1. Azure Depolama Gezgini indirip yükleme

1. Bağlantı dizesi kullanarak Azure Depolama'ya Bağlan

1. Bağlantı dizesini belirtin: `DefaultEndpointsProtocol=http;BlobEndpoint=http://<host device name>:11002/<your local account name>;AccountName=<your local account name>;AccountKey=<your local account key>;`

1. Bağlanmak için adımlara geçin.

1. Kapsayıcı içinde yerel depolama hesabınızı oluşturun

1. Blok blobları olarak dosyalarını yüklemeye başlayın.
   > [!NOTE]
   > Bu modül, sayfa bloblarını desteklemez.

1. Verileri karşıya yüklemekte olduğunuz Burada, Azure depolama hesaplarınıza bağlanmak seçebilirsiniz. Bu, tek bir görünümde, yerel depolama hesabı ve Azure depolama hesabı için sağlar

## <a name="supported-storage-operations"></a>Desteklenen depolama işlemleri

BLOB Depolama modülleri IOT Edge üzerinde aynı Azure depolama SDK'ları kullanın ve blok blob uç noktaları için Azure depolama API 2017-04-17 sürümünü tutarlıdır. Sonraki sürümlerde, müşteri gereksinimlerine bağlıdır.

IOT Edge üzerinde Azure Blob Depolama tarafından desteklenen tüm Azure Blob Depolama işlemleri olduğundan, bu bölümde her durumunu listeler.

### <a name="account"></a>Hesap

Desteklenen:

- Kapsayıcıları listeleme

Desteklenmeyen:

- Alma ve blob hizmeti özelliklerini ayarla
- Denetim öncesi blob isteği
- BLOB hizmeti istatistikleri alın
- Hesap bilgilerini alma

### <a name="containers"></a>Kapsayıcılar

Desteklenen:

- Oluşturma ve kapsayıcı silme
- Kapsayıcı özellikleri ve meta verileri alma
- Blobları listeleme
- Alma ve kapsayıcı ACL ayarlama
- Kümesi kapsayıcı meta verileri

Desteklenmeyen:

- Kira kapsayıcı

### <a name="blobs"></a>Bloblar

Desteklenen:

- Yerleştirme, alma ve blob silme
- Alma ve blob özelliklerini ayarlama
- Alma ve blob meta verileri ayarlama

Desteklenmeyen:

- Kira blob'u
- Blob anlık görüntüsü
- Kopyalama ve kopyalama blob durdurma
- Blobu silme işlemi geri
- Blob katmanı Ayarla

### <a name="block-blobs"></a>Blok blobları

Desteklenen:

- Blok yerleştirme
- Koy ve engelleme listesine alın

Desteklenmeyen:

- URL'den blok yerleştirme

## <a name="release-notes"></a>Sürüm Notları

İşte [sürüm notları docker hub'da](https://hub.docker.com/_/microsoft-azure-blob-storage) bu modül için

## <a name="feedback"></a>Geri Bildirim

Geri bildiriminiz bizim için bu modülü ve özelliklerini kullanışlı ve kullanımı kolay hale getirmek için önemlidir. Lütfen görüşlerinizi paylaşın ve nasıl geliştirebileceğimizi bize bildirin.

Adresinden bize ulaşın absiotfeedback@microsoft.com

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [IOT Edge üzerinde Azure Blob Depolama dağıtma](how-to-deploy-blob.md)
