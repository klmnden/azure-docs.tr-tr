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
ms.date: 11/16/2016
ms.author: dobett
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: f86a70a5207f19063e9992325c8f8d696ca7823e


---
# <a name="tutorial-get-started-with-the-preconfigured-solutions"></a>Öğretici: Önceden yapılandırılmış çözümleri kullanmaya başlama
## <a name="introduction"></a>Giriş
Azure IoT Paketi [önceden yapılandırılmış çözümleri][lnk-preconfigured-solutions], ortak IoT iş senaryolarını uygulayan uçtan uca çözümler sunmak için birden çok Azure IoT hizmetini birleştirir. Önceden yapılandırılmış *uzaktan izleme* çözümü cihazlarınıza bağlanır ve cihazları izler. Cihazlarınızdan alınan veri akışını analiz etmek ve işlemleri bu veri akışına otomatik olarak yanıt verecek hale getirerek iş sonuçlarını iyileştirmek için bu çözümü kullanabilirsiniz.

Bu öğretici, önceden yapılandırılmış uzaktan izleme çözümünün nasıl hazırlanacağını gösterir. Ayrıca, uzaktan izleme çözümünün temel özelliklerinde rehberlik sağlar. Bu özelliklerin birçoğuna önceden yapılandırılmış çözüm ile birlikte dağıtılan çözüm panosundan erişebilirsiniz:

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

## <a name="view-the-device-list"></a>Cihaz listesini görüntüleme
Cihaz listesi, çözümde kayıtlı olan tüm cihazları gösterir. Meta verileri görüntüleyebilir ve düzenleyebilir, cihazları ekleyebilir veya kaldırabilir ve cihazlara komutlar gönderebilirsiniz.

1. Bu çözüm için *cihaz listesini* göstermek üzere sol menüdeki **Cihazlar**'a tıklayın.
   
   ![Panoda cihaz listesi][img-devicelist]
2. Cihaz listesi, hazırlama işlemi tarafından oluşturulan dört sanal cihaz olduğunu gösterir.
3. Cihaz ayrıntılarını görüntülemek için cihaz listesindeki bir cihaza tıklayın.
   
   ![Panoda cihaz ayrıntıları][img-devicedetails]

**Cihaz Ayrıntıları** panelinde üç bölüm bulunur:

* **Eylemler** bölümü cihazda gerçekleştirebileceğiniz eylemleri listeler. Cihazı devre dışı bırakırsanız artık telemetri göndermesine veya komutları almasına izin verilmez. Bir cihazı devre dışı bırakırsanız daha sonra yeniden etkinleştirebilirsiniz. Bir telemetri değeri bir eşiği aştığında bir uyarıyı tetikleyen cihaz ile ilişkili bir kural ekleyebilirsiniz. Ayrıca, bir cihaza komut gönderebilirsiniz. Bir cihaz ilk kez bağlandığında, yanıt verebileceği komutları çözüme söyler.
* **Cihaz Özelliklerini** bölümü cihazın meta verilerini listeler. Bu meta verilerin bazıları cihazın kendisinden gelir (üretici gibi) ve bazıları da çözüm tarafından oluşturulur (oluşturma tarihi gibi). Cihaz meta verilerini buradan düzenleyebilirsiniz.
* **Kimlik Doğrulama Anahtarları** bölümü cihazın çözümle kimlik doğrulamada kullanabileceği anahtarları listeler.

## <a name="send-a-command-to-a-device"></a>Bir cihaza bir komut gönderme
Cihaz ayrıntıları bölmesi, belirli bir cihazın desteklediği komutların tümünü gösterir ve bir cihaza komut göndermenizi sağlar. Bir cihaz ilk kez başlatıldığında, desteklediği komutlar hakkında çözüme bilgi gönderir.

1. Seçilen cihaz için cihaz ayrıntıları bölmesinde **Komutlar**'a tıklayın.
   
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

## <a name="edit-the-device-metadata"></a>Cihaz meta verilerini düzenleme
Bir cihaz çözüme ilk kez bağlandığında çözüme meta verilerini gönderir. Çözüm panosu aracılığıyla cihaz meta verilerini düzenlediğinizde yeni meta veri değerleri cihaza gönderilir ve yeni değerler çözüm DocumentDB veritabanına depolanır. Daha fazla bilgi için bkz. [Cihaz kimliği kayıt defteri ve DocumentDB][lnk-devicemetadata].

1. Cihaz listesine geri gidin.
2. **Cihazlar Listesi**'nde yeni cihazınızı seçin ve ardından **Cihaz Özelliklerini** düzenlemek için **Düzenle**'ye tıklayın.
   
   ![Cihaz meta verilerini düzenleme][img-editdevice]
3. Aşağı kaydırın ve enlem ve boylam değerlerinde değişiklik yapın. Ardından, **Değişiklikleri cihaz kayıt defterine kaydet**'e tıklayın.
   
    ![Cihaz meta verilerini düzenleme][img-editdevice2]
4. Panoya geri gidin; cihazın konumu haritada değişmiştir:
   
    ![Cihaz meta verilerini düzenleme][img-editdevice3]

## <a name="add-a-rule-for-the-new-device"></a>Yeni cihaz için bir kural ekleme
Yeni eklediğiniz yeni cihaz için hiçbir kural bulunmamaktadır. Bu bölümde, yeni cihaz tarafından bildirilen sıcaklık 47 dereceyi aştığında uyarı tetikleyen bir kural ekleyeceksiniz. Başlamadan önce, panoda yeni cihaz için telemetri geçmişinin, cihaz sıcaklığının hiçbir zaman 45 dereceyi aşmadığını gösterdiğine dikkat edin.

1. Cihaz listesine geri gidin.
2. **Cihazlar Listesi**'nde yeni cihazınızı seçin ve ardından cihaz için bir kural eklemek üzere **Kural ekle**'ye tıklayın.
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

## <a name="other-features"></a>Diğer özellikler
Çözüm portalını kullanarak model numarası gibi belirli özelliklere sahip cihazları arayabilirsiniz:

![Cihaz arama][img-search]

Bir Cihazı devre dışı bırakabilir, devre dışı kaldıktan sonra da kaldırabilirsiniz:

![Bir cihazı devre dışı bırakma ve kaldırma][img-disable]

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
[img-pingcommand]: media/iot-suite-getstarted-preconfigured-solutions/pingcommand.png
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
[img-search]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_07.png
[img-disable]: media/iot-suite-getstarted-preconfigured-solutions/solutionportal_08.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-devicemetadata]: iot-suite-what-are-preconfigured-solutions.md#device-identity-registry-and-documentdb
[lnk-logicapptutorial]: iot-suite-logic-apps-tutorial.md
[lnk-rm-walkthrough]: iot-suite-remote-monitoring-sample-walkthrough.md
[lnk-connect-rm]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md



<!--HONumber=Dec16_HO1-->


