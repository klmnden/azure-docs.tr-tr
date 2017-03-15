---
title: "Önceden yapılandırılmış çözümleri kullanmaya başlama | Microsoft Belgeleri"
description: "Önceden yapılandırılmış bir Azure IoT Paketi çözümünün nasıl dağıtılacağını öğrenmek için bu öğreticiyi izleyin."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 6ab38d1a-b564-469e-8a87-e597aa51d0f7
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/15/2017
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: e68815c2dafc596c3560ad3fcb2a7bf96d29182b
ms.lasthandoff: 03/06/2017


---
# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Öğretici: Önceden yapılandırılmış çözümleri kullanmaya başlama
## <a name="introduction"></a>Giriş
Azure IoT Paketi [önceden yapılandırılmış çözümleri][lnk-preconfigured-solutions], ortak IoT iş senaryolarını uygulayan uçtan uca çözümler sunmak için birden çok Azure IoT hizmetini birleştirir. Önceden yapılandırılmış *uzaktan izleme* çözümü cihazlarınıza bağlanır ve cihazları izler. Cihazlarınızdan alınan veri akışını analiz etmek ve işlemleri bu veri akışına otomatik olarak yanıt verecek hale getirerek iş sonuçlarını iyileştirmek için bu çözümü kullanabilirsiniz.

Bu öğretici, önceden yapılandırılmış uzaktan izleme çözümünün nasıl hazırlanacağını gösterir. Ayrıca, önceden yapılandırılmış çözümün temel özelliklerinde rehberlik sağlar. Bu özelliklerin birçoğuna önceden yapılandırılmış çözümün bir parçası olarak dağıtılan çözüm *panosundan* erişebilirsiniz:

![Önceden yapılandırılmış uzaktan izleme panosu][img-dashboard]

Bu öğreticiyi tamamlamak için etkin bir Azure aboneliğinizin olması gerekir.

> [!NOTE]
> Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk_free_trial].
> 
> 

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="scenario-overview"></a>Senaryoya genel bakış

Önceden yapılandırılmış uzaktan izleme çözümünü dağıttığınızda, genel bir uzaktan izleme senaryosunu adım adım görmenize olanak sağlayan kaynaklar ile önceden doldurulur. Bu senaryoda, çözüme bağlı çeşitli cihazlar beklenmeyen sıcaklık değerleri bildirmektedir. Aşağıdaki bölümlerde şunları nasıl yapacağınız açıklanır:

* Beklenmeyen sıcaklık değerleri gönderen cihazları belirleme.
* Bu cihazları daha ayrıntılı telemetri göndermek için yapılandırma.
* Bu cihazlarda üretici yazılımını güncelleştirerek sorunu giderme.
* Eyleminizin sorunu çözdüğünü doğrulama.

Bu senaryonun temel bir özelliği, çözüm panosu kullanılarak tüm bu eylemlerin uzaktan gerçekleştirilebiliyor olmasıdır. Cihazlara fiziksel erişiminizin olması gerekmez.

## <a name="view-the-solution-dashboard"></a>Çözüm panosunu görüntüleme

Çözüm panosu, dağıtılan çözümü yönetmenizi sağlar. Örneğin, telemetriyi görüntüleyebilir, cihazları ekleyebilir ve kuralları yapılandırabilirsiniz.

1. Sağlama tamamlandığında ve önceden yapılandırılmış çözümünüzün kutucuğu **Hazır**’ı gösterdiğinde, uzaktan izleme çözümü portalınızı yeni bir sekmede açmak için **Başlat**’ı seçin.

    ![Önceden yapılandırılmış çözümü başlatma][img-launch-solution]

1. Varsayılan olarak, çözüm portalı *panoyu* gösterir. Sayfanın sol tarafındaki menüyü kullanarak çözüm portalının diğer alanlarına gidebilirsiniz.

    ![Önceden yapılandırılmış uzaktan izleme panosu][img-menu]

Pano aşağıdaki bilgileri gösterir:

* Çözüme bağlı olan her bir cihazın konumunu görüntüleyen bir harita. Çözümü ilk kez çalıştırdığınızda, 25 sanal cihaz bulunur. Sanal cihazlar Azure WebJobs olarak uygulanır ve çözüm de haritada bilgileri çizmek için Bing Haritaları API'sini kullanır. Eşlemeyi nasıl dinamik hale getireceğinizi öğrenmek için [SSS][lnk-faq] bölümüne bakın.
* Seçilen yakın gerçek zamanlı cihazdan nem ve sıcaklık telemetrisini çizen ve maksimum, minimum ve ortalama nem gibi toplu verileri görüntüleyen bir **Telemetri Geçmişi** paneli.
* Bir telemetri değeri bir eşiği aştığında yeni uyarı etkinliklerini gösteren bir **Uyarı Geçmişi** paneli. Önceden yapılandırılmış çözüm tarafından oluşturulan örneklere ek olarak kendi alarmlarınızı tanımlayabilirsiniz.
* Zamanlanmış işler hakkında bilgiler gösteren bir **İşler** paneli. **Yönetim işleri** sayfasından kendi işlerinizi zamanlayabilirsiniz.

## <a name="view-alarms"></a>Uyarıları görüntüleme

Uyarı geçmişi paneli beş cihazın beklenenden yüksek telemetri değerleri bildirdiğini gösteriyor.

![Çözüm panosunda TODO Uyarı geçmişi][img-alarms]

> [!NOTE]
> Bu uyarılar önceden yapılandırılmış çözüme dahil edilen bir kural tarafından oluşturulur. Bu kural, bir cihaz tarafından gönderilen sıcaklık değeri 60’ı aştığında bir uyarı oluşturur. Sol menüde [Kurallar](#add-a-rule) ve [Eylemler](#add-an-action)’i seçerek kendi kural ve eylemlerinizi tanımlayabilirsiniz.

## <a name="view-devices"></a>Cihazları görüntüleme

*Cihazlar* listesi, çözümde kayıtlı olan tüm cihazları gösterir. Cihaz listesinden cihaz meta verilerini görüntüleyip düzenleyebilir, cihaz ekleyip kaldırabilir ve cihazlarda yöntemler çağırabilirsiniz. Cihaz listesindeki cihazları filtreleyebilir ve sıralayabilirsiniz. Ayrıca cihaz listesinde gösterilen sütunları özelleştirebilirsiniz.

1. Bu çözüm için cihaz listesini görüntülemek için **Cihazlar**’ı seçin.

   ![Çözüm portalında cihaz listesini görüntüleme][img-devicelist]

1. Cihaz listesi başlangıçta sağlama işlemi tarafından oluşturulan 25 sanal cihazı gösterir. Çözüme başka sanal ve fiziksel cihazlar ekleyebilirsiniz.

1. Cihaz ayrıntılarını görüntülemek için cihaz listesindeki bir cihazı seçin.

   ![Çözüm portalında cihaz ayrıntılarını görüntüleme][img-devicedetails]

**Cihaz Ayrıntıları** panelinde altı bölüm bulunur:

* Cihaz simgesini özelleştirmenizi, cihazı devre dışı bırakmanızı, kural eklemenizi, bir yöntem çağırmanızı veya bir komut göndermenizi sağlayan bağlantı koleksiyonu. Komutlar (cihazdan buluta iletiler) ile yöntemlerin (doğrudan yöntemler) bir karşılaştırması için bkz. [Buluttan cihaza iletişim kılavuzu][lnk-c2d-guidance].
* **Cihaz İkizi - Etiketler** bölümünde, cihazın etiket değerlerini düzenleyebilirsiniz. Cihaz listesindeki etiket değerlerini görüntüleyebilir ve etiket değerlerini kullanarak cihaz listesini filtreleyebilirsiniz.
* **Cihaz İkizi - İstenen Özellikler** bölümünde, cihaza gönderilecek özellik değerlerini ayarlayabilirsiniz.
* **Cihaz İkizi - Bildirilen Özellikler** bölümünde cihazdan gönderilen özellik değerleri gösterilir.
* **Cihaz Özellikleri** bölümünde cihaz kimliği ve kimlik doğrulama anahtarları gibi kimlik kayıt defteri bilgileri gösterilir.
* **Son İşler** bölümünde yakın zamanda bu cihazı hedefleyen tüm işlere ilişkin bilgiler gösterilir.

## <a name="filter-the-device-list"></a>Cihaz listesini filtreleme

Yalnızca beklenmeyen sıcaklık değerleri gönderen cihazları görüntülemek için bir filtre kullanabilirsiniz. Önceden yapılandırılmış uzaktan izleme çözümü 60 değerinden yüksek ortalama sıcaklığa sahip cihazları göstermek için **Sistem durumu kötü olan cihazlar** filtresini içerir. Ayrıca [kendi filtrelerinizi oluşturabilirsiniz](#add-a-filter).

1. Kullanılabilir filtrelerin bir listesini görüntülemek için **Kayıtlı filtre aç**’ı seçin. Ardından filtreyi uygulamak için **Sistem durumu kötü olan cihazlar**’ı seçin:

    ![Filtrelerin listesini görüntüleme][img-unhealthy-filter]

1. Cihaz listesi artık yalnızca ortalama sıcaklık değeri 60’ın üzerinde olan cihazları gösterir.

    ![Sistem durumu kötü olan cihazları gösteren filtrelenmiş cihaz listesini görüntüleme][img-filtered-unhealthy-list]

## <a name="update-desired-properties"></a>İstenen özellikleri güncelleştirme

Düzeltilmesi gerekebilen bir cihaz kümesi belirlediniz. Ancak 15 saniyelik veri sıklığının sorunu açık bir şekilde tanılamak için yeterli olmadığına karar verdiniz. Telemetri sıklığını beş saniye olarak değiştirmek sorunu daha iyi tanılamak için daha fazla veri noktasına sahip olmanızı sağlar. Bu yapılandırma değişikliğini çözüm portalından uzak cihazlarınıza gönderebilirsiniz. Değişikliği bir kez yapıp etkisini değerlendirerek daha sonra sonuçlara uygun şekilde eylem gerçekleştirebilirsiniz.

Etkilenen cihazlar için **TelemetryInterval** istenen özelliğini değiştiren bir iş çalıştırmak için bu adımları izleyin. Cihazlar yeni **TelemetryInterval** özellik değerini aldığında, her 15 saniyede bir yerine her beş saniyede bir telemetri gönderecek şekilde yapılandırmalarını değiştirirler:

1. Cihaz listesinde sistem durumu iyi olmayan cihazların listesini görüntülerken, **İş Zamanlayıcı**’yı ve ardından **Cihaz İkizini Düzenle**’yi seçin.

1. İşe **Telemetri aralığını değiştir** adını verin.

1. **İstenen Özellik** adının **desired.Config.TelemetryInterval** değerini beş saniye olarak değiştirin.

1. **Zamanlama**’yı seçin.

    ![TelemetryInterval özelliğini beş saniye olarak değiştirme][img-change-interval]

1. İşin ilerleyişini portaldaki **Yönetim İşleri** sayfasında izleyebilirsiniz.

> [!NOTE]
> Tek cihaz için istenen özellik değerini değiştirmek istiyorsanız, bir iş çalıştırmak yerine **Cihaz Ayrıntıları** panelindeki **İstenen Özellikler** bölümünü kullanın.

Bu iş, filtre tarafından seçilen tüm cihazlar için cihaz ikizinde **TelemetryInterval** istenen özelliğinin değerini ayarlar. Cihazlar bu değeri cihaz ikizinden alarak davranışlarını güncelleştirir. Bir cihaz, cihaz ikizinden istenen bir özelliği alıp işlediğinde, karşılık gelen bildirilen değer özelliğini ayarlar.

## <a name="invoke-methods"></a>Çağırma yöntemleri

İş çalışırken, sistem durumu kötü olan cihazlar listesindeki tüm cihazların eski (1.6’dan önceki) üretici yazılımına sahip olduğunu fark edersiniz.

![Sistem durumu kötü olan cihazlar için bildirilen üretici yazılımı sürümünü görüntüleme][img-old-firmware]

Sistem durumu iyi olan diğer cihazların yakın zamanda 2.0 sürümüne güncelleştirildiğini bildiğiniz için, beklenmeyen sıcaklık değerlerinin kaynağı bu üretici yazılımı sürümü olabilir. Eski üretici yazılımı sürümlerine sahip cihazları belirlemek için yerleşik **Eski üretici yazılımı cihazları** filtresini kullanabilirsiniz. Daha sonra, portaldan hala eski üretici yazılımı sürümlerini çalıştıran tüm cihazları uzaktan güncelleştirebilirsiniz:

1. Kullanılabilir filtrelerin bir listesini görüntülemek için **Kayıtlı filtre aç**’ı seçin. Ardından filtreyi uygulamak için **Eski üretici yazılımı cihazları**’nı seçin:

    ![Filtrelerin listesini görüntüleme][img-old-filter]

1. Cihaz listesi artık yalnızca eski üretici yazılımı sürümlerine sahip cihazları gösterir. Bu liste **Sistem durumu kötü olan cihazlar** filtresi tarafından belirlenen beş cihazı ve üç ek cihazı içerir:

    ![Eski cihazları gösteren filtrelenmiş cihaz listesini görüntüleme][img-filtered-old-list]

1. **İş Zamanlayıcı**’yı ve ardından **Invoke Yöntemi**’ni seçin.

1. **İş Adı**’nı **Üretici yazılımını 2.0 sürümüne güncelleştirme** olarak ayarlayın.

1. **Yöntem** olarak **InitiateFirmwareUpdate**’i seçin.

1. **FwPackageUri** parametresini **https://iotrmassets.blob.core.windows.net/firmwares/FW20.bin** olarak ayarlayın.

1. **Zamanlama**’yı seçin. Artık varsayılan ayar işin çalıştırılmasıdır.

    ![Seçili cihazların üretici yazılımını güncelleştirmek için iş oluşturma][img-method-update]

> [!NOTE]
> Tek bir cihazda bir yöntem çağırmak istiyorsanız, bir iş çalıştırmak yerine **Cihaz Ayrıntıları** panelinde **Yöntemler**’i seçin.

Bu iş, filtre ile seçilen tüm cihazlarda **InitiateFirmwareUpdate** doğrudan yöntemini çağırır. Cihazlar hemen IoT Hub’a yanıt verir ve ardından üretici yazılımı güncelleştirme işlemini zaman uyumsuz olarak başlatır. Cihaz aşağıdaki ekran görüntülerinde gösterildiği gibi bildirilen özellik değerleri aracılığıyla üretici yazılımı güncelleştirme süreci hakkında durum bilgileri sağlar. Cihaz ve iş listelerindeki bilgileri güncelleştirmek için **Yenile** simgesini seçin:

![Üretici yazılımı güncelleştirme listesinin çalıştığını gösteren iş listesi][img-update-1]
![Üretici yazılımının güncelleştirme durumunu gösteren cihaz listesi][img-update-2]
![Üretici yazılımı güncelleştirme listesinin tamamladığını gösteren iş listesi][img-update-3]

> [!NOTE]
> Bir üretim ortamında, işleri belirlenen bir bakım penceresi sırasında çalışmak için zamanlayabilirsiniz.

## <a name="scenario-review"></a>Senaryo gözden geçirme

Bu senaryoda, panodaki uyarı geçmişini ve bir filtre kullanarak uzak cihazlarınızdan bazılarında olası bir sorun belirlediniz. Daha sonra sorunu tanılamaya yardımcı olmak üzere daha fazla bilgi sağlamak için bir filtre ve iş kullanarak cihazları uzaktan yapılandırdınız. Son olarak, etkilenen cihazlarda bakım zamanlamak için bir filtre ve iş kullandınız. Panoya dönerseniz, artık çözümünüzdeki cihazlardan gelen herhangi bir alarm olmadığını görürsünüz. Üretici yazılımının çözümünüzdeki tüm cihazlarda güncel olduğundan ve sistem durumu kötü başka cihaz olmadığından emin olmak için bir filtre kullanabilirsiniz:

![Tüm cihazlarda güncel üretici yazılımı olduğunu gösteren filtre][img-updated]

![Tüm cihazların sistem durumunun iyi olduğunu gösteren filtre][img-healthy]

## <a name="other-features"></a>Diğer özellikler

Aşağıdaki bölümlerde, önceden yapılandırılmış uzaktan izleme çözümünün önceki senaryonun parçası olarak bahsedilmeyen bazı ek özellikleri açıklanmaktadır.

### <a name="customize-columns"></a>Sütunları özelleştirme

**Sütun düzenleyicisi**’ni seçerek cihaz listesinde gösterilen bilgileri özelleştirebilirsiniz. Bildirilen özellik ve etiket değerlerini gösteren sütunlar ekleyip kaldırabilirsiniz. Ayrıca, sütunları yeniden sıralayabilir ve yeniden adlandırabilirsiniz:

   ![Cihaz listesinde sütun düzenleyicisi][img-columneditor]

### <a name="customize-the-device-icon"></a>Cihaz simgesini özelleştirme

Cihaz listesinde gösterilen cihaz simgesini, **Cihaz Ayrıntıları** panelinden aşağıdaki gibi özelleştirebilirsiniz:

1. Bir cihazın **Görüntü düzenleme** panelini açmak için kalem simgesini seçin:

   ![Cihaz görüntüsü düzenleyicisini açma][img-startimageedit]

1. Karşıya yeni bir görüntü yükleyip veya var olan görüntülerden birini seçip **Kaydet**’i seçin:

   ![Cihaz görüntüsü düzenleyicisini düzenleme][img-imageedit]

1. Seçtiğiniz görüntü, cihazın **Simge** sütununda gösterilir.

> [!NOTE]
> Görüntü, blob depolama alanına depolanır. Cihaz ikizindeki etiket, blob depolama alanındaki görüntünün bir bağlantısını içerir.

### <a name="add-a-device"></a>Cihaz ekleme

Önceden yapılandırılmış çözümü dağıttığınızda cihaz listesinde görebileceğiniz 25 örnek cihazı otomatik olarak hazırlarsınız. Bu cihazlar bir Azure WebJob içinde çalışan *sanal cihazlardır*. Sanal cihazlar herhangi bir gerçek ve fiziksel cihaza dağıtmaya gerek olmadan önceden yapılandırılmış çözümü denemenizi kolaylaştırır. Çözüme gerçek bir cihaz bağlamak istemiyorsanız [Cihazınızı önceden yapılandırılmış uzaktan izleme çözümüne bağlama][lnk-connect-rm] öğreticisine bakın.

Aşağıdaki adımlar bir sanal cihazın çözüme nasıl ekleneceğini göstermektedir:

1. Cihaz listesine geri gidin.

1. Bir cihaz eklemek için sol alt köşedeki **+ Cihaz Ekle** seçeneğini belirleyin.

   ![Önceden yapılandırılmış çözüme cihaz ekleme][img-adddevice]

1. **Sanal Cihaz** kutucuğundaki **Yeni Ekle**’yi seçin.

   ![Panoda yeni cihaz ayrıntılarını ayarlama][img-addnew]

   Yeni bir sanal cihaz oluşturmaya ek olarak, bir **Özel Cihaz** oluşturmayı seçerseniz fiziksel bir cihaz da ekleyebilirsiniz. Çözüme fiziksel cihazlar bağlama hakkında daha fazla bilgi edinmek için bkz. [Cihazınızı önceden yapılandırılmış IoT paketi uzak izleme çözümüne bağlama][lnk-connect-rm].

1. **Kendi Cihaz Kimliğimi tanımlamama izin ver**'i seçin ve **mydevice_01** gibi benzersiz bir cihaz kimliği girin.

1. **Oluştur**’u seçin.

   ![Yeni bir cihaz kaydetme][img-definedevice]

1. **Bir sanal cihaz ekleme**’nin 3. adımında cihaz listesine geri dönmek için **Bitti**’yi seçin.

1. Cihazınızın **Çalışıyor** olduğunu cihaz listesinde görüntüleyebilirsiniz.

    ![Cihaz listesinde yeni cihazı görüntüleme][img-runningnew]

1. Panodaki yeni cihazınızdan sanal telemetriyi de görüntüleyebilirsiniz:

    ![Yeni cihazdan telemetri görüntüleme][img-runningnew-2]

### <a name="disable-and-delete-a-device"></a>Bir cihazı devre dışı bırakma ve silme

Bir Cihazı devre dışı bırakabilir, devre dışı kaldıktan sonra da kaldırabilirsiniz:

![Bir cihazı devre dışı bırakma ve kaldırma][img-disable]

### <a name="add-a-rule"></a>Kural ekleme

Yeni eklediğiniz yeni cihaz için hiçbir kural bulunmamaktadır. Bu bölümde, yeni cihaz tarafından bildirilen sıcaklık 47 dereceyi aştığında uyarı tetikleyen bir kural ekleyeceksiniz. Başlamadan önce, panoda yeni cihaz için telemetri geçmişinin, cihaz sıcaklığının hiçbir zaman 45 dereceyi aşmadığını gösterdiğine dikkat edin.

1. Cihaz listesine geri gidin.

1. Cihaz için bir kural eklemek üzere **Cihazlar Listesi**’nde yeni cihazınızı seçin ve ardından **Kural ekle**’yi seçin.

1. Veri alanı olarak **Temperature** ve sıcaklık 47 dereceyi aştığında çıktı olarak **AlarmTemp** kullanan bir kural oluşturun:

    ![Cihaz kuralı ekleme][img-adddevicerule]

1. Yaptığınız değişiklikleri kaydetmek için **Kuralları Kaydet ve Görüntüle**’yi seçin.

1. Yeni cihaz için cihaz ayrıntıları bölmesinde **Komutlar**’ı seçin.

    ![Cihaz kuralı ekleme][img-adddevicerule2]

1. Komut listesinden **ChangeSetPointTemp**'i seçin ve **SetPointTemp**'i 45 olarak ayarlayın. Ardından **Komut Gönder**’i seçin:

    ![Cihaz kuralı ekleme][img-adddevicerule3]

1. Panoya geri gidin. Kısa bir süre sonra yeni cihazınız tarafından raporlanan sıcaklık 47 derece eşiğini aştığında **Uyarı Geçmişi** bölmesinde yeni bir giriş görürsünüz:

    ![Cihaz kuralı ekleme][img-adddevicerule4]

1. Panonun **Kurallar** sayfasında tüm kurallarınızı gözden geçirebilir ve düzenleyebilirsiniz:

    ![Cihaz kurallarını listeleme][img-rules]

1. Panonun **Eylemler** sayfasında bir kurala yanıt olarak gerçekleştirilebilecek tüm eylemleri gözden geçirebilir ve düzenleyebilirsiniz:

    ![Cihaz eylemlerini listeleme][img-actions]

> [!NOTE]
> Bir kurala yanıt olarak bir e-posta iletisi veya SMS gönderebilen veya bir [Mantıksal Uygulama][lnk-logic-apps] aracılığıyla bir iş kolu sistemiyle tümleştirilebilen eylemler tanımlanabilir. Daha fazla bilgi için bkz. [Mantıksal Uygulamanızı önceden yapılandırılmış Azure IoT Paketi Uzaktan İzleme çözümüne bağlama][lnk-logicapptutorial].

### <a name="manage-filters"></a>Filtreleri yönetme

Cihaz listesinde, hub’ınıza bağlı cihazların özelleştirilmiş bir listesini görüntülemek üzere filtreler oluşturabilir, kaydedebilir ve yeniden yükleyebilirsiniz. Bir filtre oluşturmak için:

1. Cihaz listesinin üzerindeki filtre düzenleme simgesini seçin:

    ![Filtre düzenleyicisini açın][img-editfiltericon]

1. **Filtre düzenleyicisi**’nde cihaz listesini filtrelemek için alan, işleç ve değerler ekleyin. Filtrenizi daraltmak için birden fazla yan tümce ekleyebilirsiniz. Filtreyi uygulamak için **Filtre**’ye tıklayın:

    ![Filtre oluşturma][img-filtereditor]

1. Bu örnekte liste, üreticiye ve model numarasına göre filtrelenmiştir:

    ![Filtrelenmiş liste][img-filterelist]

1. Filtrenizi özel bir adla kaydetmek için **Farklı kaydet** simgesini seçin:

    ![Filtre kaydetme][img-savefilter]

1. Daha önce kaydettiğiniz bir filtreyi yeniden uygulamak için **Kaydedilmiş filtreyi aç** simgesini seçin:

    ![Filtre açma][img-openfilter]

Cihaz kimliği, cihaz durumu, istenen özellikler, bildirilen özellikler ve etiketlere göre filtreler oluşturabilirsiniz. Bir cihaza kendi özel etiketlerinizi **Cihaz Ayrıntıları** panelinin **Etiketler** bölümünden ekleyebilir veya birden fazla cihazda etiketleri güncelleştirmek için bir iş çalıştırabilirsiniz.

> [!NOTE]
> **Filtre düzenleyicisi**’nde **Gelişmiş görünüm**’ü kullanarak sorgu metnini doğrudan düzenleyebilirsiniz.

### <a name="commands"></a>Komutlar

**Cihaz Ayrıntıları** panelinden cihaza komut gönderebilirsiniz. Bir cihaz ilk kez başlatıldığında, desteklediği komutlar hakkında çözüme bilgi gönderir. Komutlar ve metotlar arasındaki farklar için bkz. [Azure IoT Hub buluttan cihaza seçenekleri][lnk-c2d-guidance].

1. Seçilen cihaz için **Cihaz Ayrıntıları** bölmesinde **Komutlar**’ı seçin:

   ![Panoda cihaz komutları][img-devicecommands]

1. Komut listesinden **PingDevice**'ı seçin.

1. **Komut Gönder**’i seçin.

1. Komutun durumunu komut geçmişinde görebilirsiniz.

   ![Panoda komut durumu][img-pingcommand]

Çözüm, gönderdiği her bir komutun durumunu takip eder. Sonuç başlangıçta **Beklemede**'dir. Cihaz komutu yürüttüğünü raporladığında, sonuç **Başarılı** olarak ayarlanır.

## <a name="behind-the-scenes"></a>Arka planda

Önceden yapılandırılmış bir çözümü dağıttığınızda, dağıtım işlemi seçtiğiniz Azure aboneliğinde birden çok kaynak oluşturur. Bu kaynakları Azure [portalında][lnk-portal] görüntüleyebilirsiniz. Dağıtım işlemi, önceden yapılandırılmış çözümünüz için seçtiğiniz adı temel alan bir ada sahip bir **kaynak grubu** oluşturur:

![Azure portalında önceden yapılandırılmış çözüm][img-portal]

Her bir kaynağın ayarlarını, kaynak grubundaki kaynaklar listesinde seçerek görüntüleyebilirsiniz.

Önceden yapılandırılmış çözüm için kaynak kodunu da görüntüleyebilirsiniz. Önceden yapılandırılmış uzaktan izleme çözümünün kaynak kodu [azure-iot-remote-monitoring][lnk-rmgithub] GitHub deposundadır:

* **DeviceAdministration** klasörü, pano için kaynak kodunu içerir.
* **Simulator** klasörü, sanal cihaz için kaynak kodunu içerir.
* **EventProcessor** klasörü, gelen telemetriyi işleyen arka uç işleme için kaynak kodunu içerir.

İşiniz bittiğinde önceden yapılandırılmış çözümü [azureiotsuite.com][lnk-azureiotsuite] sitesindeki Azure aboneliğinizden silebilirsiniz. Bu site, önceden yapılandırılmış çözümü oluşturduğunuzda sağlanan tüm kaynakları kolayca silmenize imkan tanır.

> [!NOTE]
> Önceden yapılandırılmış çözümle ilgili her şeyi sildiğinizden emin olmak için bu öğeleri [azureiotsuite.com][lnk-azureiotsuite] sitesinde silin; kaynak grubunu portaldan silmeyin.

## <a name="next-steps"></a>Sonraki Adımlar

Çalışan bir önceden yapılandırılmış çözüm dağıttığınıza göre aşağıdaki makaleleri okuyarak IoT Paketi ile çalışmaya başlayabilirsiniz:

* [Uzaktan izleme önceden yapılandırılmış çözüm kılavuzu][lnk-rm-walkthrough]
* [Cihazınızı önceden yapılandırılmış uzaktan izleme çözümüne bağlama][lnk-connect-rm]
* [azureiotsuite.com sitesindeki izinler][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-menu]: media/iot-suite-getstarted-preconfigured-solutions/menu.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-alarms]: media/iot-suite-getstarted-preconfigured-solutions/alarms.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-adddevicerule]: media/iot-suite-getstarted-preconfigured-solutions/addrule.png
[img-adddevicerule2]: media/iot-suite-getstarted-preconfigured-solutions/addrule2.png
[img-adddevicerule3]: media/iot-suite-getstarted-preconfigured-solutions/addrule3.png
[img-adddevicerule4]: media/iot-suite-getstarted-preconfigured-solutions/addrule4.png
[img-actions]: media/iot-suite-getstarted-preconfigured-solutions/actions.png
[img-portal]: media/iot-suite-getstarted-preconfigured-solutions/portal.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png
[img-columneditor]: media/iot-suite-getstarted-preconfigured-solutions/columneditor.png
[img-startimageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit1.png
[img-imageedit]: media/iot-suite-getstarted-preconfigured-solutions/imagedit2.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-unhealthy-filter]: media/iot-suite-getstarted-preconfigured-solutions/unhealthyfilter.png
[img-filtered-unhealthy-list]: media/iot-suite-getstarted-preconfigured-solutions/unhealthylist.png
[img-change-interval]: media/iot-suite-getstarted-preconfigured-solutions/changeinterval.png
[img-old-firmware]: media/iot-suite-getstarted-preconfigured-solutions/noticeold.png
[img-old-filter]: media/iot-suite-getstarted-preconfigured-solutions/oldfilter.png
[img-filtered-old-list]: media/iot-suite-getstarted-preconfigured-solutions/oldlist.png
[img-method-update]: media/iot-suite-getstarted-preconfigured-solutions/methodupdate.png
[img-update-1]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate1.png
[img-update-2]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate2.png
[img-update-3]: media/iot-suite-getstarted-preconfigured-solutions/jobupdate3.png
[img-updated]: media/iot-suite-getstarted-preconfigured-solutions/updated.png
[img-healthy]: media/iot-suite-getstarted-preconfigured-solutions/healthy.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-faq]: iot-suite-faq.md
