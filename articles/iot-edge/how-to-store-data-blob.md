---
title: Blok blobları cihazlarda - Azure IOT Edge Store | Microsoft Docs
description: Katmanlama ve yaşam süresi özelliklerini anlamanıza, desteklenen blob depolama işlemleri görmek ve blob depolama hesabınıza bağlanın.
author: kgremban
manager: philmea
ms.author: kgremban
ms.reviewer: arduppal
ms.date: 05/21/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 396af2dfd9fc53c080163a27e376328c1369d5e1
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65991463"
---
# <a name="store-data-at-the-edge-with-azure-blob-storage-on-iot-edge-preview"></a>IOT Edge (Önizleme) Azure Blob Depolama ile uçta veri Store

IOT Edge üzerinde Azure Blob Depolama sağlayan bir [blok blobu](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs#about-block-blobs) uçta depolama çözümü. Bir blob depolama modülü IOT Edge Cihazınızda bir Azure blok blob hizmeti gibi davranır, ancak blok blobları, IOT Edge Cihazınızda yerel olarak depolanır. Aynı Azure depolama SDK'ın yöntemleri kullanarak bloblarınızın erişebilir veya zaten kullanılan blob API çağrılarını engelle.

> [!NOTE]
> IOT Edge üzerinde Azure Blob Depolama, içinde [genel Önizleme](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu modül ile birlikte gelen **katmanlama** ve **yaşam süresi** özellikleri.

> [!NOTE]
> Şu anda katmanlama ve yaşam süresi işlevi yalnızca Linux AMD64 ve Linux ARM32 kullanılabilir.

**Katmanlama** otomatik olarak, yerel blob depolamadan aralıklı Internet bağlantısı desteği ile verileri azure'a karşıya yüklemeye izin veren yapılandırılabilir bir işlevdir. İzin verir:

- Kapat Kapat katmanlama özelliği
- Verileri Azure NewestFirst veya OldestFirst gibi kopyalandığı sırayı seçin
- Verilerinizi karşıya istediğiniz Azure depolama hesabı belirtin.
- Azure'a yüklemek istediğiniz kapsayıcılarını belirtin. Bu modül, hem kaynak hem de hedef kapsayıcı adları belirtmenize olanak sağlar.
- BLOB katmanlama tam (kullanarak `Put Blob` işlemi) ve düzeyi katmanlama engelleyin (kullanarak `Put Block` ve `Put Block List` işlemleri).

Bu modül, blob bloklarını oluşuyorsa blok düzeyinde, katmanlama kullanır. Bazı yaygın senaryolar şunlardır:

- Uygulamanız bazı bloklarını daha önce yüklenen bir blobu güncelleştirir, bu modül, yalnızca güncelleştirilmiş engeller ve tüm blob karşıya yükler.
- Modül blob karşıya yükleme ve bağlantı tekrar tekrar yalnızca kalan engeller ve tüm blob karşıya yüklemeleri olduğunda internet bağlantısı, kaybolduktan.

Bir blob karşıya yükleme sırasında bir beklenmeyen işlem sonlandırma (örneğin, güç kesintisi) durumda modülü tekrar çevrimiçi olduğunda karşıya yükleme için son tüm blokların yeniden yüklenecek.

**Yaşam süresi** (TTL) olan yapılandırılabilir bir işlevsellik (dakika cinsinden ölçülür) belirtilen süre dolduğunda burada modülü otomatik olarak bloblarınızın yerel depolama alanından siler. TTL sağlar:

- Kapat Kapat katmanlama özelliği
- TTL dakika cinsinden belirtin.

Veri, videoları, resimleri, Finans verileri, hastane verilerini veya buluta aktarılan veya yerel olarak işlenen, daha sonra yerel olarak depolanması için gereken tüm verileri olduğu gibi senaryoları bu modülü kullanmak için iyi örneklerdir.

Bu makalede, Azure Blob Depolama, blob hizmeti, IOT Edge Cihazınızda çalışan IOT Edge kapsayıcısı üzerinde dağıtmak için yönergeler sağlar.

> [!NOTE]
> "Otomatik-videoda kullanılan süre sonu" ve "otomatik katmanlama" şartları "katmanlama" ve "zaman yaşam" ile değiştirilmiştir.

Hızlı bir giriş için videoyu izleyin
> [!VIDEO https://www.youtube.com/embed/wkprcfVidyM]

## <a name="prerequisites"></a>Önkoşullar

Bir Azure IoT Edge cihazı:

- Hızlı Başlangıç adımları izleyerek bir IOT Edge cihazı geliştirme makinenizde veya bir sanal makine kullanabilirsiniz [Linux](quickstart-linux.md) veya [Windows cihazları](quickstart.md).

- IOT Edge modülü Azure Blob Depolama, aşağıdaki cihaz yapılandırmalarını destekler:

  | İşletim sistemi | Mimari |
  | ---------------- | ----- | ----- |
  | Ubuntu Server 16.04 | AMD64 |
  | Ubuntu Server 18.04 | AMD64 |
  | Windows 10 IoT Core (Ekim güncelleştirme) | AMD64 |
  | Windows 10 IOT Enterprise (Ekim güncelleştirme) | AMD64 |
  | Windows Server 2019 | AMD64 |
  | Raspbian Uzat | ARM32 |

Bulut kaynakları:

Azure'da standart katman [IoT Hub'ı](../iot-hub/iot-hub-create-through-portal.md).

## <a name="tiering-and-time-to-live-properties"></a>Katmanlama ve yaşam süresi özellikleri

Katman ayarlama için istenen özellikleri ve yaşam süresi özelliklerini kullanın. Bunlar ayarlanabilir dağıtımı sırasında veya yeniden dağıtmaya gerek kalmadan modül ikizi düzenleyerek daha sonra değiştirildi. "Modül İkizi" denetimi için önerdiğimiz `reported configuration` ve `configurationValidation` değerleri doğru şekilde yayılır emin olmak için.

### <a name="tiering-properties"></a>Katmanlama özellikleri

Bu ayar adı `tieringSettings`

| Alan | Olası Değerler | Açıklama |
| ----- | ----- | ---- |
| tieringOn | TRUE, false | Varsayılan olarak ayarlanmış `false`etkinleştirmek istiyorsanız üzerinde kümesine `true`|
| backlogPolicy | NewestFirst, OldestFirst | Verileri Azure'a kopyalandığı sırasını seçmenize olanak sağlar. Varsayılan olarak ayarlanmış `OldestFirst`. Sırası blobun son değiştirme zamanına göre belirlenir |
| remoteStorageConnectionString |  | `"DefaultEndpointsProtocol=https;AccountName=<your Azure Storage Account Name>;AccountKey=<your Azure Storage Account Key>;EndpointSuffix=<your end point suffix>"` Verilerinizi istediğiniz Azure depolama hesabı belirtmenizi sağlar bir bağlantı dizesi yüklenir. Belirtin `Azure Storage Account Name`, `Azure Storage Account Key`, `End point suffix`. Uygun EndpointSuffix burada veri karşıya yüklenecek, Azure ekleyin, Global Azure, Azure kamu ve Microsoft Azure Stack için değişir. |
| tieredContainers | `"<source container name1>": {"target": "<target container name>"}`,<br><br> `"<source container name1>": {"target": "%h-%d-%m-%c"}`, <br><br> `"<source container name1>": {"target": "%d-%c"}` | Azure'a yüklemek istediğiniz kapsayıcı adları belirtmenizi sağlar. Bu modül, hem kaynak hem de hedef kapsayıcı adları belirtmenize olanak sağlar. Hedef kapsayıcı adı belirtmezseniz, otomatik olarak kapsayıcı adı olarak atar `<IoTHubName>-<IotEdgeDeviceName>-<ModuleName>-<ContainerName>`. Hedef kapsayıcı adı için şablon dizeleri oluşturmak, olası değerler sütununu kontrol edin. <br>* %h -> IOT hub'ı adı (3-50 karakter). <br>* IOT cihaz kimliği (1 129 karakter için) -> %d. <br>* %m -> modül adı (1 ile 64 karakter arasında). <br>* Kaynak kapsayıcı adı (3 63 karakter için) -> %c. <br><br>En büyük boyutunu kapsayıcı adı 63 karakterden, kesim kapsayıcısının boyutunu aşarsa, otomatik olarak hedef kapsayıcı adı her bölüm (IoTHubName, IotEdgeDeviceName, modül adı, ContainerName) 15 karakter atanırken 63 karakterden oluşabilir. |

### <a name="time-to-live-properties"></a>Yaşam süresi özellikleri

Bu ayar adı `ttlSettings`

| Alan | Olası Değerler | Açıklama |
| ----- | ----- | ---- |
| ttlOn | TRUE, false | Varsayılan olarak ayarlanmış `false`etkinleştirmek istiyorsanız üzerinde kümesine `true`|
| timeToLiveInMinutes | `<minutes>` | TTL dakika cinsinden belirtin. TTL süresi dolduğunda modülü otomatik olarak, BLOB'ları yerel depolama alanından siler |

## <a name="configure-log-files"></a>Günlük dosyalarını yapılandırma

Günlük dosyaları, modül için yapılandırma hakkında daha fazla bilgi için bkz: [üretim en iyi](https://docs.microsoft.com/azure/iot-edge/production-checklist#set-up-logs-and-diagnostics).

## <a name="connect-to-your-blob-storage-module"></a>Blob depolama modülüne bağlayın

Hesap adı ve IOT Edge Cihazınızda blob depolamaya erişmek, bir modül için yapılandırılan hesap anahtarı kullanabilirsiniz.

IOT Edge Cihazınızı herhangi bir depolama alanı için blob uç nokta olarak belirtmek için yaptığınız istekleri. Yapabilecekleriniz [bir açık depolama uç noktası için bir bağlantı dizesi oluşturma](../storage/common/storage-configure-connection-string.md#create-a-connection-string-for-an-explicit-storage-endpoint) IOT Edge cihaz bilgileri ve yapılandırdığınız hesabı adını kullanarak.

- Azure Blob Depolama IOT Edge modülü üzerinde çalıştığı olarak aynı cihaza dağıtılan modülleri için blob uç noktadır: `http://<module name>:11002/<account name>`.
- Burada Azure Blob Depolama IOT Edge modülü çalıştıran ve ardından blob sizin kurulumunuza bağlı olarak daha farklı bir cihaz üzerinde dağıtılan modüller için uç nokta şunlardan biridir:
  - `http://<device IP >:11002/<account name>`
  - `http://<IoT Edge device hostname>:11002/<account name>`
  - `http://<fully qualified domain name>:11002/<account name>`

## <a name="azure-blob-storage-quickstart-samples"></a>Azure Blob Depolama hızlı başlangıç örnekleri

Çeşitli dillerde örnek kodlar sağladık hızlı başlangıçlar Azure Blob Depolama belgeleri içerir. IOT Edge üzerinde Azure Blob Depolama, blob depolama modülünüzde bağlanmak için blob uç noktası değiştirerek test etmek için bu örnekleri yeniden çalıştırabilirsiniz.

Şu hızlı başlangıçlarda olarak blob depolama modülü ile birlikte IOT Edge modülleri dağıtabilirsiniz böylece IOT Edge ile de desteklenen dilleri kullanın:

- [.NET](../storage/blobs/storage-quickstart-blobs-dotnet.md)
- [Java](../storage/blobs/storage-quickstart-blobs-java.md)
- [Python](../storage/blobs/storage-quickstart-blobs-python.md)
- [Node.js](../storage/blobs/storage-quickstart-blobs-nodejs.md)

## <a name="connect-to-your-local-storage-with-azure-storage-explorer"></a>Azure Depolama Gezgini ile yerel depolama bağlanma

Kullanabileceğiniz **Azure Depolama Gezgini** yerel depolama hesabınıza bağlanmak için. Bu yalnızca kullanılabilir [Azure Depolama Gezgini sürüm 1.5.0](https://github.com/Microsoft/AzureStorageExplorer/releases/tag/v1.5.0).

> [!NOTE]
> Bir bağlantı için bir yerel depolama hesabı ekleme veya yerel depolama hesabındaki kapsayıcıları oluşturma gibi aşağıdaki adımları gerçekleştirirken hatalarla karşılaşabilirsiniz. Lütfen yoksay ve yenileyin.

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
- Blobları listele
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

## <a name="feedback"></a>Geri Bildirim

Geri bildiriminiz bizim için bu modülü ve özelliklerini kullanışlı ve kullanımı kolay hale getirmek için önemlidir. Lütfen görüşlerinizi paylaşın ve nasıl geliştirebileceğimizi bize bildirin.

Adresinden bize ulaşın absiotfeedback@microsoft.com

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi edinin [IOT Edge üzerinde Azure Blob Depolama dağıtma](how-to-deploy-blob.md)
