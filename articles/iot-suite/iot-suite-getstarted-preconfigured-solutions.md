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
ms.sourcegitcommit: 9e1bcba086a9f70c689a5d7d7713a8ecdc764492
ms.openlocfilehash: 8248e0a02cb0775a87f0c8130e53b98f8bcfe581
ms.lasthandoff: 02/27/2017


---
# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Öğretici: Önceden yapılandırılmış çözümleri kullanmaya başlama
## <a name="introduction"></a>Giriş
Azure IoT Paketi [önceden yapılandırılmış çözümleri][lnk-preconfigured-solutions], ortak IoT iş senaryolarını uygulayan uçtan uca çözümler sunmak için birden çok Azure IoT hizmetini birleştirir. Önceden yapılandırılmış *uzaktan izleme* çözümü cihazlarınıza bağlanır ve cihazları izler. Cihazlarınızdan alınan veri akışını analiz etmek ve işlemleri bu veri akışına otomatik olarak yanıt verecek hale getirerek iş sonuçlarını iyileştirmek için bu çözümü kullanabilirsiniz.

Bu öğretici, önceden yapılandırılmış uzaktan izleme çözümünün nasıl hazırlanacağını gösterir. Ayrıca, önceden yapılandırılmış çözümün temel özelliklerinde rehberlik sağlar. Bu özelliklerin birçoğuna önceden yapılandırılmış çözümün bir parçası olarak dağıtılan çözüm panosundan erişebilirsiniz:

![Önceden yapılandırılmış uzaktan izleme panosu][img-dashboard]

Bu öğreticiyi tamamlamak için etkin bir Azure aboneliğinizin olması gerekir.

> [!NOTE]
> Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk_free_trial].
> 
> 

[!INCLUDE [iot-suite-provision-remote-monitoring](../../includes/iot-suite-provision-remote-monitoring.md)]

## <a name="view-the-solution-dashboard"></a>Çözüm panosunu görüntüleme
Çözüm panosu, dağıtılan çözümü yönetmenizi sağlar. Örneğin, telemetriyi görüntüleyebilir, cihazları ekleyebilir ve kuralları yapılandırabilirsiniz.

1. Hazırlama tamamlandığında ve önceden yapılandırılmış çözümünüzün kutucuğu **Hazır**'ı gösterdiğinde, uzaktan izleme çözümü portalınızı yeni bir sekmede açmak için **Başlat**'a tıklayın.
   
   ![Önceden yapılandırılmış çözümü başlatma][img-launch-solution]
2. Varsayılan olarak, çözüm portalı *çözüm panosunu* gösterir. Sol taraftaki menüyü kullanarak diğer görünümleri seçebilirsiniz.
   
   ![Önceden yapılandırılmış uzaktan izleme panosu][img-dashboard]

Pano aşağıdaki bilgileri gösterir:

* Harita, çözüme bağlı olan her bir cihazın konumunu görüntüler. Çözümü ilk kez çalıştırdığınızda, dört sanal cihaz bulunur. Sanal cihazlar Azure WebJobs olarak uygulanır ve çözüm de haritada bilgileri çizmek için Bing Haritaları API'sini kullanır.
* **Telemetri Geçmişi** paneli, seçilen yakın gerçek zamanlı cihazdan nem ve sıcaklık telemetrisini çizer ve maksimum, minimum ve ortalama nem gibi toplu verileri görüntüler.
* **Uyarı Geçmişi** paneli bir telemetri değeri bir eşiği aştığında yeni uyarı etkinliklerini gösterir. Önceden yapılandırılmış çözüm tarafından oluşturulan örneklere ek olarak kendi alarmlarınızı tanımlayabilirsiniz.
* **İşler** panelinde, zamanlanmış işler hakkında bilgi gösterilir. **Yönetim işleri** sayfasından kendi işlerinizi zamanlayabilirsiniz.

## <a name="view-the-device-list"></a>Cihaz listesini görüntüleme
*Cihaz listesi*, çözümde kayıtlı olan tüm cihazları gösterir. Cihaz listesinden cihaz meta verilerini görüntüleyip düzenleyebilir, cihaz ekleyip kaldırabilir ve cihazlarda yöntemler çağırabilirsiniz.

1. Bu çözümün cihaz listesini görüntülemek için sol menüdeki **Cihazlar**'a tıklayın.
   
   ![Panoda cihaz listesi][img-devicelist]
2. Cihaz listesi başlangıçta hazırlama işlemi tarafından oluşturulan dört sanal cihazı gösterir. Çözüme başka sanal ve fiziksel cihazlar ekleyebilirsiniz.
3. **Sütun düzenleyicisi**’ne tıklayarak cihaz listesinde gösterilen bilgileri özelleştirebilirsiniz. Bildirilen özellik ve etiket değerlerini gösteren sütunlar ekleyip kaldırabilirsiniz. Ayrıca, sütunları yeniden sıralayabilir ve yeniden adlandırabilirsiniz:
   
   ![Panoda sütun düzenleyicisi][img-columneditor]
4. Cihaz ayrıntılarını görüntülemek için cihaz listesindeki bir cihaza tıklayın.
   
   ![Panoda cihaz ayrıntıları][img-devicedetails]

**Cihaz Ayrıntıları** panelinde altı bölüm bulunur:

* Cihaz simgesini özelleştirmenizi, cihazı devre dışı bırakmanızı, kural eklemenizi, bir yöntem çağırmanızı veya bir komut göndermenizi sağlayan bağlantı koleksiyonu. Komutlar (cihazdan buluta iletiler) ile yöntemlerin (doğrudan yöntemler) bir karşılaştırması için bkz. [Buluttan cihaza iletişim kılavuzu][lnk-c2d-guidance].
* **Cihaz İkizi - Etiketler** bölümünde, cihazın etiket değerlerini düzenleyebilirsiniz. Cihaz listesindeki etiket değerlerini görüntüleyebilir ve etiket değerlerini kullanarak cihaz listesini filtreleyebilirsiniz.
* **Cihaz İkizi - İstenen Özellikler** bölümünde, cihaza gönderilecek özellik değerlerini ayarlayabilirsiniz.
* **Cihaz İkizi - Bildirilen Özellikler** bölümünde cihazdan gönderilen özellik değerleri gösterilir.
* **Cihaz Özellikleri** bölümünde cihaz kimliği ve kimlik doğrulama anahtarları gibi kimlik kayıt defteri bilgileri gösterilir.
* **Son İşler** bölümünde yakın zamanda bu cihazı hedefleyen tüm işlere ilişkin bilgiler gösterilir.

## <a name="customize-the-device-icon"></a>Cihaz simgesini özelleştirme

Cihaz listesinde gösterilen cihaz simgesini, **Cihaz Ayrıntıları** panelinden aşağıdaki gibi özelleştirebilirsiniz:

1. Bir cihazın **Görüntü düzenleme** panelini açmak için kalem simgesine tıklayın:
   
   ![Cihaz görüntüsü düzenleyicisini açma][img-startimageedit]
2. Karşıya yeni bir görüntü yükleyip veya var olan görüntülerden birini seçip **Kaydet**’e tıklayın:
   
   ![Cihaz görüntüsü düzenleyicisini düzenleme][img-imageedit]
3. Seçtiğiniz görüntü, cihazın **Simge** sütununda gösterilir.

> [!NOTE]
> Görüntü, blob depolama alanına depolanır. Cihaz ikizindeki etiket, blob depolama alanındaki görüntünün bir bağlantısını içerir.
> 
> 

## <a name="invoke-a-method-on-a-device"></a>Cihazda yöntem çağırma
**Cihaz Ayrıntıları** panelinden cihaz üzerinde yöntem çağırabilirsiniz. Bir cihaz ilk kez başlatıldığında, desteklediği yöntemler hakkında çözüme bilgi gönderir.

1. Seçilen cihazın **Cihaz Ayrıntıları** bölmesinde **Yöntemler**'e tıklayın:
   
   ![Panodaki cihaz yöntemleri][img-devicemethods]
2. Yöntem listesinden **Yeniden Başlat**’ı seçin.
3. **Yöntemi Çağır**’a tıklayın.
4. Yöntem geçmişinde yöntem çağırma durumunu görebilirsiniz.
   
   ![Panoda yöntem durumu][img-pingmethod]

Çözüm, çağırdığı her bir yöntemin durumunu takip eder. Cihaz yöntemi tamamladığında, yöntem geçmişi tablosunda yeni bir giriş görürsünüz.

Bazı yöntemler, cihaz üzerinde zaman uyumsuz işler başlatır. Örneğin, **InitiateFirmwareUpdate** yöntemi güncelleştirmeyi gerçekleştirmek üzere zaman uyumsuz bir görev başlatır. Cihaz, üretici yazılımı güncelleştirmesi devam ederken güncelleştirme durumunu bildirmek üzere bildirilen özellikleri kullanır.

## <a name="send-a-command-to-a-device"></a>Bir cihaza bir komut gönderme
**Cihaz Ayrıntıları** panelinden cihaza komut gönderebilirsiniz. Bir cihaz ilk kez başlatıldığında, desteklediği komutlar hakkında çözüme bilgi gönderir.

1. Seçilen cihaz için **Cihaz Ayrıntıları** bölmesinde **Komutlar**'a tıklayın:
   
   ![Panoda cihaz komutları][img-devicecommands]
2. Komut listesinden **PingDevice**'ı seçin.
3. **Komut Gönder**'e tıklayın.
4. Komutun durumunu komut geçmişinde görebilirsiniz.
   
   ![Panoda komut durumu][img-pingcommand]

Çözüm, gönderdiği her bir komutun durumunu takip eder. Sonuç başlangıçta **Beklemede**'dir. Cihaz komutu yürüttüğünü raporladığında, sonuç **Başarılı** olarak ayarlanır.

## <a name="add-a-new-simulated-device"></a>Yeni bir sanal cihaz ekleme
Önceden yapılandırılmış çözümü dağıttığınızda cihaz listesinde görebileceğiniz dört örnek cihazı otomatik olarak hazırlarsınız. Bu cihazlar bir Azure WebJob içinde çalışan *sanal cihazlardır*. Sanal cihazlar herhangi bir gerçek ve fiziksel cihaza dağıtmaya gerek olmadan önceden yapılandırılmış çözümü denemenizi kolaylaştırır. Çözüme gerçek bir cihaz bağlamak istemiyorsanız [Cihazınızı önceden yapılandırılmış uzaktan izleme çözümüne bağlama][lnk-connect-rm] öğreticisine bakın.

Aşağıdaki adımlar bir sanal cihazın çözüme nasıl ekleneceğini göstermektedir:

1. Cihaz listesine geri gidin.
2. Bir cihaz eklemek için sol alt köşedeki **+ Cihaz Ekle** seçeneğine tıklayın.
   
   ![Önceden yapılandırılmış çözüme cihaz ekleme][img-adddevice]
3. **Sanal Cihaz** kutucuğundaki **Yeni Ekle**'ye tıklayın.
   
   ![Panoda yeni cihaz ayrıntılarını ayarlama][img-addnew]
   
   Yeni bir sanal cihaz oluşturmaya ek olarak, bir **Özel Cihaz** oluşturmayı seçerseniz fiziksel bir cihaz da ekleyebilirsiniz. Çözüme fiziksel cihazlar bağlama hakkında daha fazla bilgi edinmek için bkz. [Cihazınızı önceden yapılandırılmış IoT paketi uzak izleme çözümüne bağlama][lnk-connect-rm].
4. **Kendi Cihaz Kimliğimi tanımlamama izin ver**'i seçin ve **mydevice_01** gibi benzersiz bir cihaz kimliği girin.
5. **Oluştur**'a tıklayın.
   
   ![Yeni bir cihaz kaydetme][img-definedevice]
6. **Bir sanal cihaz ekleme**'nin 3. adımında cihaz listesine geri dönmek için **Bitti**'ye tıklayın.
7. Cihazınızın **Çalışıyor** olduğunu cihaz listesinde görüntüleyebilirsiniz.
   
    ![Cihaz listesinde yeni cihazı görüntüleme][img-runningnew]
8. Panodaki yeni cihazınızdan sanal telemetriyi de görüntüleyebilirsiniz:
   
    ![Yeni cihazdan telemetri görüntüleme][img-runningnew-2]

## <a name="device-properties"></a>Cihaz özellikleri
Önceden yapılandırılmış uzaktan izleme çözümü, cihazlar ile çözüm arka ucu arasında cihaz meta verilerini eşitlemek üzere [cihaz ikizlerini][lnk-device-twin] kullanır. Cihaz ikizi, IoT Hub'da tek bir cihazın özellik değerlerini depolayan bir JSON belgesidir. Cihazlar, meta verileri *bildirilen özellikler* halinde cihaz ikizinde depolanmak üzere çözüm arka ucuna düzenli olarak gönderir. Çözüm arka ucu, meta veri güncelleştirmelerini cihazlara göndermek üzere cihaz ikizinde *istenen özellikleri* ayarlayabilir. Bildirilen özellikler, cihaz tarafından gönderilen en son meta veri değerlerini gösterir. Daha fazla bilgi için bkz. [Cihaz kimliği kayıt defteri, cihaz ikizi ve DocumentDB][lnk-devicemetadata].

> [!NOTE]
> Çözüm ayrıca komutlar ve yöntemlerle ilgili cihaza özgü verileri depolamak üzere bir DocumentDB veritabanı kullanır.
> 
> 

1. Cihaz listesine geri gidin.
2. **Cihazlar Listesi**'nde yeni cihazınızı seçin ve ardından **Cihaz İkizi - İstenen Özellikler**’i düzenlemek için **Düzenle**'ye tıklayın:
   
   ![İstenen cihaz özelliklerini düzenleme][img-editdevice]
3. **İstenen Özellik Adı** değerini **Enlem** olarak belirleyin ve değeri **47.639521** olarak ayarlayın. Ardından, **Değişiklikleri Cihaz Kayıt Defterine Kaydet**'e tıklayın:
   
    ![İstenen cihaz özelliğini düzenleme][img-editdevice2]
4. **Cihaz Ayrıntıları** panelinde yeni enlem değeri başlangıçta istenen bir özellik olarak gösterilir ve eski enlem değeri bildirilen özellik olarak görünür:
   
    ![Bildirilen özelliği görüntüleme][img-editdevice3]
5. Şu anda, önceden yapılandırılmış çözümdeki sanal cihazlar yalnızca istenen **Desired.Config.TemperatureMeanValue** ve **Desired.Config.TelemetryInterval** özelliklerini işlemektedir. Gerçek bir cihaz, istenen tüm özellikleri IoT hub’ından okur, yapılandırmasında gerekli değişiklikleri yapar ve yeni değerleri bildirilen özellik olarak hub’a bildirir.

**Cihaz Ayrıntıları** panelinde **Cihaz İkizi - Etiketler** ayarını, **Cihaz İkizi - İstenen Özellikler** ayarı ile aynı şekilde düzenleyebilirsiniz. Ancak, istenmeyen özelliklerin aksine, etiketler cihazla eşitlenmez. Etiketler yalnızca IoT hub’ındaki cihaz ikizinde bulunur. Etiketler, cihaz listesinde özel filtreler oluşturmak için faydalıdır.

## <a name="sort-the-device-list"></a>Cihaz listesini sıralama

Bir sütun başlığına tıklayarak cihaz listesini sıralayabilirsiniz. İlk tıklama artan sıraya göre düzenlerken, ikinci tıklama azalan sıraya göre düzenler:

![Cihaz listesini sıralama][img-sortdevices]

## <a name="filter-the-device-list"></a>Cihaz listesini filtreleme

Cihaz listesinde, hub’ınıza bağlı cihazların özelleştirilmiş bir listesini görüntülemek üzere filtreler oluşturabilir, kaydedebilir ve yeniden yükleyebilirsiniz. Bir filtre oluşturmak için:

1. Cihaz listesinin üzerindeki filtre düzenleme simgesine tıklayın:
   
   ![Filtre düzenleyicisini açın][img-editfiltericon]
2. **Filtre düzenleyicisi**’nde cihaz listesini filtrelemek için alan, işleç ve değerler ekleyin. Filtrenizi daraltmak için birden fazla yan tümce ekleyebilirsiniz. **Filtre**’ye tıklayarak filtrenizi uygulayın:
   
   ![Filtre oluşturma][img-filtereditor]
3. Bu örnekte liste, üreticiye ve model numarasına göre filtrelenmiştir:
   
   ![Filtrelenmiş liste][img-filterelist]
4. Filtrenizi özel bir adla kaydetmek için **Farklı kaydet** simgesine tıklayın:
   
   ![Filtre kaydetme][img-savefilter]
5. Daha önce kaydettiğiniz bir filtreyi yeniden uygulamak için **Kaydedilmiş filtreyi aç** simgesine tıklayın:
   
   ![Filtre açma][img-openfilter]

Cihaz kimliği, cihaz durumu, istenen özellikler, bildirilen özellikler ve etiketlere göre filtreler oluşturabilirsiniz.

> [!NOTE]
> **Filtre düzenleyicisi**’nde **Gelişmiş görünüm**’ü kullanarak sorgu metnini doğrudan düzenleyebilirsiniz.
> 
> 

## <a name="add-a-rule-for-the-new-device"></a>Yeni cihaz için bir kural ekleme
Yeni eklediğiniz yeni cihaz için hiçbir kural bulunmamaktadır. Bu bölümde, yeni cihaz tarafından bildirilen sıcaklık 47 dereceyi aştığında uyarı tetikleyen bir kural ekleyeceksiniz. Başlamadan önce, panoda yeni cihaz için telemetri geçmişinin, cihaz sıcaklığının hiçbir zaman 45 dereceyi aşmadığını gösterdiğine dikkat edin.

1. Cihaz listesine geri gidin.
2. Cihaz için bir kural eklemek üzere **Cihazlar Listesi**'nde yeni cihazınızı seçin ve ardından **Kural ekle**'ye tıklayın.
3. Veri alanı olarak **Temperature** ve sıcaklık 47 dereceyi aştığında çıktı olarak **AlarmTemp** kullanan bir kural oluşturun:
   
    ![Cihaz kuralı ekleme][img-adddevicerule]
4. Yaptığınız değişiklikleri kaydetmek için **Kuralları Kaydet ve Görüntüle**'ye tıklayın.
5. Yeni cihaz için cihaz ayrıntıları bölmesinde **Komutlar**'a tıklayın.
   
   ![Cihaz kuralı ekleme][img-adddevicerule2]
6. Komut listesinden **ChangeSetPointTemp**'i seçin ve **SetPointTemp**'i 45 olarak ayarlayın. Ardından **Komut Gönder**'e tıklayın:
   
   ![Cihaz kuralı ekleme][img-adddevicerule3]
7. Çözüm panosuna geri gidin. Kısa bir süre sonra yeni cihazınız tarafından raporlanan sıcaklık 47 derece eşiğini aştığında **Uyarı Geçmişi** bölmesinde yeni bir giriş görürsünüz:
   
   ![Cihaz kuralı ekleme][img-adddevicerule4]
8. Panonun **Kurallar** sayfasında tüm kurallarınızı gözden geçirebilir ve düzenleyebilirsiniz:
   
    ![Cihaz kurallarını listeleme][img-rules]
9. Panonun **Eylemler** sayfasında bir kurala yanıt olarak gerçekleştirilebilecek tüm eylemleri gözden geçirebilir ve düzenleyebilirsiniz:
   
    ![Cihaz eylemlerini listeleme][img-actions]

> [!NOTE]
> Bir kurala yanıt olarak bir e-posta iletisi veya SMS gönderebilen veya bir [Mantıksal Uygulama][lnk-logic-apps] aracılığıyla bir iş kolu sistemiyle tümleştirilebilen eylemler tanımlanabilir. Daha fazla bilgi için bkz. [Mantıksal Uygulamanızı önceden yapılandırılmış Azure IoT Paketi Uzaktan İzleme çözümüne bağlama][lnk-logicapptutorial].
> 
> 

## <a name="disable-and-remove-devices"></a>Cihazları devre dışı bırakma ve kaldırma
Bir Cihazı devre dışı bırakabilir, devre dışı kaldıktan sonra da kaldırabilirsiniz:

![Bir cihazı devre dışı bırakma ve kaldırma][img-disable]

## <a name="run-jobs"></a>İşleri çalıştırma
Cihazlarınız üzerinde toplu işlemler gerçekleştirmek için iş zamanlayabilirsiniz. Cihaz listesi için bir iş oluşturun. Bu liste tüm cihazlarınızı içerebilir veya cihaz listesindeki [filtreleme araçları](#filter-the-device-list) kullanılarak filtrelenmiş bir liste oluşturulabilir. Bir iş, listedeki her cihaz üzerinde bir yöntemi çağırabilir veya cihaz listesindeki her cihaz için cihaz ikizini güncelleştirebilir.

### <a name="create-a-job-to-invoke-a-method"></a>Yöntem çağırmak için iş oluşturma

Aşağıdaki adımlar bir listedeki her cihaz üzerinde üretici yazılımı güncelleştirme yöntemini çağıran bir iş oluşturma işlemini gösterir. Yöntem yalnızca yöntemi destekleyen cihazlarda çağrılır:

1. Üretici yazılımı güncelleştirmesini alacak cihazların listesini oluşturmak için, cihaz listesindeki filtreleme araçlarını kullanın:
   
   ![Filtre düzenleyicisini açın][img-editfiltericon]
2. Filtrelenmiş listenizde **İş zamanlayıcısı**’na tıklayın:
   
   ![İş zamanlayıcısını açın][img-clickjobscheduler]
3. **İş zamanla** panelinde **Yöntemi Çağır**’a tıklayın.
4. **Yöntem Çağır** sayfasına çağrılacak yöntemin ayrıntılarını girip **Zamanla**’ya tıklayın:
   
   ![Yöntem işini yapılandırma][img-invokemethodjob]

**InitiateFirmwareUpdate** yöntemi cihaz üzerinde zaman uyumsuz bir görev başlatır ve hemen döndürür. Üretici yazılımı güncelleştirme işlemi bundan sonra çalışan güncelleştirme işlemiyle ilgili rapor oluşturmak üzere bildirilen özellikleri kullanır.

### <a name="create-a-job-to-edit-the-device-twin"></a>Cihaz ikizini düzenlemek için iş oluşturma

Aşağıdaki adımlar bir listedeki her cihaz üzerinde cihaz ikizini düzenleyen bir iş oluşturma işlemini gösterir:

1. Cihaz ikizi düzenlemelerini alacak cihazların listesini oluşturmak için, cihaz listesindeki filtreleme araçlarını kullanın:
   
   ![Filtre düzenleyicisini açın][img-editfiltericon]
2. Filtrelenmiş listenizde **İş zamanlayıcısı**’na tıklayın:
   
   ![İş zamanlayıcısını açın][img-clickjobscheduler]
3. **İş zamanla** panelinde **Cihaz İkizini Düzenle**’ye tıklayın.
4. **Cihaz İkizini Düzenle** sayfasına **İstenen Özellikler** ve **Etiketler** ile ilgili ayrıntıları girip **Zamanla**’ya tıklayın:
   
   ![Yöntem işini yapılandırma][img-edittwinjob]

### <a name="monitor-the-job"></a>İş izleme
Zamanladığınız işlerin durumunu **Yönetim İşleri** sayfasında izleyebilirsiniz. **İş Ayrıntıları** panelinde seçili işle ilgili bilgiler gösterilir:
   
   ![İş durumunu görüntüleme][img-jobstatus]

Ayrıca **Pano** üzerinden işlere ilişkin bilgileri görüntüleyebilirsiniz:
   
   ![Panoda işleri görüntüleme][img-jobdashboard]


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
> 
> 

## <a name="next-steps"></a>Sonraki Adımlar
Çalışan bir önceden yapılandırılmış çözüm dağıttığınıza göre aşağıdaki makaleleri okuyarak IoT Paketi ile çalışmaya başlayabilirsiniz:

* [Uzaktan izleme önceden yapılandırılmış çözüm kılavuzu][lnk-rm-walkthrough]
* [Cihazınızı önceden yapılandırılmış uzaktan izleme çözümüne bağlama][lnk-connect-rm]
* [azureiotsuite.com sitesindeki izinler][lnk-permissions]

[img-launch-solution]: media/iot-suite-getstarted-preconfigured-solutions/launch.png
[img-dashboard]: media/iot-suite-getstarted-preconfigured-solutions/dashboard.png
[img-devicelist]: media/iot-suite-getstarted-preconfigured-solutions/devicelist.png
[img-devicedetails]: media/iot-suite-getstarted-preconfigured-solutions/devicedetails.png
[img-devicecommands]: media/iot-suite-getstarted-preconfigured-solutions/devicecommands.png
[img-devicemethods]: media/iot-suite-getstarted-preconfigured-solutions/devicemethods.png
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
[img-pingmethod]: media/iot-suite-getstarted-preconfigured-solutions/pingmethod.png
[img-adddevice]: media/iot-suite-getstarted-preconfigured-solutions/adddevice.png
[img-addnew]: media/iot-suite-getstarted-preconfigured-solutions/addnew.png
[img-definedevice]: media/iot-suite-getstarted-preconfigured-solutions/definedevice.png
[img-runningnew]: media/iot-suite-getstarted-preconfigured-solutions/runningnew.png
[img-runningnew-2]: media/iot-suite-getstarted-preconfigured-solutions/runningnew2.png
[img-rules]: media/iot-suite-getstarted-preconfigured-solutions/rules.png
[img-editdevice]: media/iot-suite-getstarted-preconfigured-solutions/editdevice.png
[img-editdevice2]: media/iot-suite-getstarted-preconfigured-solutions/editdevice2.png
[img-editdevice3]: media/iot-suite-getstarted-preconfigured-solutions/editdevice3.png
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
[img-sortdevices]: media/iot-suite-getstarted-preconfigured-solutions/sortdevices.png
[img-editfiltericon]: media/iot-suite-getstarted-preconfigured-solutions/editfiltericon.png
[img-filtereditor]: media/iot-suite-getstarted-preconfigured-solutions/filtereditor.png
[img-filterelist]: media/iot-suite-getstarted-preconfigured-solutions/filteredlist.png
[img-savefilter]: media/iot-suite-getstarted-preconfigured-solutions/savefilter.png
[img-openfilter]:  media/iot-suite-getstarted-preconfigured-solutions/openfilter.png
[img-clickjobscheduler]: media/iot-suite-getstarted-preconfigured-solutions/clickscheduler.png
[img-invokemethodjob]: media/iot-suite-getstarted-preconfigured-solutions/invokemethodjob.png
[img-edittwinjob]: media/iot-suite-getstarted-preconfigured-solutions/edittwinjob.png
[img-jobstatus]: media/iot-suite-getstarted-preconfigured-solutions/jobstatus.png
[img-jobdashboard]: media/iot-suite-getstarted-preconfigured-solutions/jobdashboard.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-device-twin-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-c2d-guidance]: ../iot-hub/iot-hub-devguide-c2d-guidance.md
[lnk-device-twin]: ../iot-hub/iot-hub-devguide-device-twins.md
