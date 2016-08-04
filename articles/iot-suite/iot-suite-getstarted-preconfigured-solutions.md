<properties
    pageTitle="Önceden yapılandırılmış çözümleri kullanmaya başlama | Microsoft Azure"
    description="Önceden yapılandırılmış bir Azure IoT Paketi çözümünün nasıl dağıtılacağını öğrenmek için bu öğreticiyi izleyin."
    services=""
    suite="iot-suite"
    documentationCenter=""
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-suite"
     ms.devlang="na"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="05/25/2016"
     ms.author="dobett"/>

# Öğretici: Önceden yapılandırılmış çözümleri kullanmaya başlama

## Giriş

Azure IoT Paketi [önceden yapılandırılmış çözümleri][lnk-preconfigured-solutions], ortak IoT iş senaryolarını uygulayan uçtan uca çözümler sunmak için birden çok Azure IoT hizmetini birleştirir.

Bu öğretici, önceden yapılandırılmış *uzaktan izleme* çözümünün nasıl sağlanacağını gösterir. Ayrıca, önceden yapılandırılmış uzaktan izleme çözümünün temel özelliklerinde rehberlik sağlar.

Bu öğreticiyi tamamlamak için etkin bir Azure aboneliğine ihtiyacınız vardır.

> [AZURE.NOTE]  Hesabınız yoksa yalnızca birkaç dakika içinde ücretsiz bir deneme sürümü hesabı oluşturabilirsiniz. Ayrıntılı bilgi için bkz. [Azure Ücretsiz Deneme Sürümü][lnk_free_trial].

## Önceden yapılandırılmış uzaktan izleme çözümünü sağlama

1.  Azure hesabı kimlik bilgilerinizi kullanarak [azureiotsuite.com][lnk-azureiotsuite] adresinde oturum açın ve yeni bir çözüm oluşturmak için **+** seçeneğine tıklayın.

    > [AZURE.NOTE] Bir çözümü sağlamak için gereken izinlerle ilgili sorun yaşıyorsanız rehberlik için bkz. [Azureiotsuite.com sitesindeki izinler][lnk-permissions].

2.  **Uzaktan izleme** kutucuğunda **Seç**'e tıklayın.

3.  Önceden yapılandırılmış uzaktan izleme çözümünüz için bir **Çözüm adı** girin.

4.  Çözümü sağlamak için kullanmak istediğiniz **Bölge** ve **Abonelik** seçimini yapın.

5.  Hazırlama işlemini başlatmak için **Çözümü Oluştur**'a tıklayın. İşlemin çalışması genellikle birkaç dakika sürer.

## Hazırlama işleminin tamamlanmasını bekleme

1. Çözümünüzün **Hazırlama** durumuna sahip olan kutucuğuna tıklayın.
 
2. Azure hizmetleri Azure aboneliğinize dağıtılırken **Hazırlama durumlarına** dikkat edin.

3. Hazırlama tamamlandığında durum **Hazır** olarak değişir.

4. Kutucuğa tıkladığınızda sağ bölmede çözümünüzün ayrıntılarını görürsünüz.

> [AZURE.NOTE] Önceden yapılandırılmış çözümün dağıtılmasında sorunlarla karşılaşıyorsanız bkz. [Azureiotsuite.com sitesindeki izinler][lnk-permissions] ve [SSS][lnk-faq]. Sorunlar devam ederse lütfen [portalda][lnk-portal] bir hizmet bileti oluşturun.

Görmeyi beklediğiniz ancak çözümünüz için listelenmemiş ayrıntılar mı var? [User Voice](https://feedback.azure.com/forums/321918-azure-iot)'da bize özellik önerileri verin.

## Uzaktan izleme çözüm panosunu görüntüleme

Çözüm panosu, dağıtılan çözümü yönetmenizi sağlar. Örneğin, telemetriyi görüntüleyebilir, cihazları ekleyebilir ve kuralları yapılandırabilirsiniz.

1.  Hazırlama tamamlandığında ve önceden yapılandırılmış çözümünüzün kutucuğu **Hazır**'ı gösterdiğinde, uzaktan izleme çözümü portalınızı yeni bir sekmede açmak için **Başlat**'a tıklayın.

    ![][img-launch-solution]

2.  Varsayılan olarak, çözüm portalı *çözüm panosunu* gösterir. Sol taraftaki menüyü kullanarak diğer görünümleri seçebilirsiniz.

    ![][img-dashboard]

Pano aşağıdaki bilgileri gösterir:

- Harita, çözüme bağlı olan her bir cihazın konumunu görüntüler. Çözümü ilk kez çalıştırdığınızda, dört sanal cihaz bulunur. Sanal cihazlar Azure WebJobs olarak uygulanır ve çözüm de haritada bilgileri çizmek için Bing Haritaları API'sini kullanır.
- **Telemetri Geçmişi** paneli, seçilen yakın gerçek zamanlı cihazdan nem ve sıcaklık telemetrisini çizer ve maksimum, minimum ve ortalama nem gibi toplu verileri görüntüler.
- **Uyarı Geçmişi** paneli bir telemetri değeri bir eşiği aştığında yeni uyarı etkinliklerini gösterir. Önceden yapılandırılmış çözüm tarafından oluşturulan örneklere ek olarak kendi alarmlarınızı tanımlayabilirsiniz.

## Çözüm cihazı listesini görüntüleme

Cihaz listesi, çözümde kayıtlı olan tüm cihazları gösterir. Meta verileri görüntüleyebilir ve düzenleyebilir, cihazları ekleyebilir veya kaldırabilir ve cihazlara komutlar gönderebilirsiniz.

1.  Bu çözüm için *cihaz listesini* göstermek üzere sol menüdeki **Cihazlar**'a tıklayın.

    ![][img-devicelist]

2.  Cihaz listesi, hazırlama işlemi tarafından oluşturulan dört sanal cihaz olduğunu gösterir.

3.  Cihaz ayrıntılarını görüntülemek için cihaz listesindeki bir cihaza tıklayın.

    ![][img-devicedetails]

**Cihaz Ayrıntıları** panelinde üç bölüm bulunur:

- **Eylemler** bölümü cihazda gerçekleştirebileceğiniz eylemleri listeler. Cihazı devre dışı bırakırsanız artık telemetri göndermesine veya komutları almasına izin verilmez. Bir cihazı devre dışı bırakırsanız daha sonra yeniden etkinleştirebilirsiniz. Bir telemetri değeri bir eşiği aştığında bir uyarıyı tetikleyen cihaz ile ilişkili bir kural ekleyebilirsiniz. Ayrıca, bir cihaza komut gönderebilirsiniz. Bir cihaz ilk kez bağlandığında, yanıt verebileceği komutları çözüme söyler.
- **Cihaz Özelliklerini** bölümü cihazın meta verilerini listeler. Bu meta verilerin bazıları cihazın kendisinden gelir (üretici gibi) ve bazıları da çözüm tarafından oluşturulur (oluşturma tarihi gibi). Cihaz meta verilerini buradan düzenleyebilirsiniz.
- **Kimlik Doğrulama Anahtarları** bölümü cihazın çözümle kimlik doğrulamada kullanabileceği anahtarları listeler.

## Bir cihaza bir komut gönderme

Cihaz ayrıntıları bölmesi, belirli bir cihazın desteklediği komutların tümünü gösterir ve bir cihaza komut göndermenizi sağlar. Bir cihaz ilk kez başlatıldığında, desteklediği komutlar hakkında çözüme bilgi gönderir.

1.  Seçilen cihaz için cihaz ayrıntıları bölmesinde **Komutlar**'a tıklayın.

    ![][img-devicecommands]

2.  Komut listesinden **PingDevice**'ı seçin.

3.  **Komut Gönder**'e tıklayın.

4.  Komutun durumunu komut geçmişinde görebilirsiniz.

    ![][img-pingcommand]

Çözüm, gönderdiği her bir komutun durumunu takip eder. Sonuç başlangıçta **Beklemede**'dir. Cihaz komutu yürüttüğünü raporladığında, sonuç **Başarılı** olarak ayarlanır.

## Yeni bir sanal cihaz ekleme

1.  Cihaz listesine geri gidin.

2.  Yeni bir cihaz eklemek için sol alt köşedeki **+ Bir Cihaz Ekle**'ye tıklayın.

    ![][img-adddevice]

3.  **Sanal Cihaz** kutucuğundaki **Yeni Ekle**'ye tıklayın.

    ![][img-addnew]
    
    Yeni bir sanal cihaz oluşturmaya ek olarak, bir **Özel Cihaz** oluşturmayı seçerseniz fiziksel bir cihaz da ekleyebilirsiniz. Bunun hakkında daha fazla bilgi edinmek için bkz. [Cihazınızı önceden yapılandırılmış IoT paketi uzak izleme çözümüne bağlama][lnk-connecting-devices].

4.  **Kendi Cihaz Kimliğimi tanımlamama izin ver**'i seçin ve **mydevice_01** gibi benzersiz bir cihaz kimliği girin.

5.  **Oluştur**'a tıklayın.

    ![][img-definedevice]

6. **Bir sanal cihaz ekleme**'nin 3. adımında cihaz listesine geri dönmek için **Bitti**'ye tıklayın.

7. Cihazınızın **Çalışıyor** olduğunu cihaz listesinde görüntüleyebilirsiniz.

    ![][img-runningnew]

8. Panodaki yeni cihazınızdan sanal telemetriyi de görüntüleyebilirsiniz:

    ![][img-runningnew-2]

## Cihaz meta verilerini düzenleme

1.  Cihaz listesine geri gidin.

2.  **Cihazlar Listesi**'nde yeni cihazınızı seçin ve ardından **Cihaz Özelliklerini** düzenlemek için **Düzenle**'ye tıklayın.

    ![][img-editdevice]

3. Aşağı kaydırın ve enlem ve boylam değerlerinde değişiklik yapın. Ardından, **Değişiklikleri cihaz kayıt defterine kaydet**'e tıklayın.

    ![][img-editdevice2]

4. Panoya geri gidin; cihazın konumu haritada değişmiştir:

    ![][img-editdevice3]

## Yeni cihaz için bir kural ekleme

Yeni eklediğiniz yeni cihaz için hiçbir kural bulunmamaktadır. Bu bölümde yeni cihaz tarafından raporlanan sıcaklık 47 dereceyi aştığında uyarı tetikleyen bir kural ekleyeceksiniz. Başlamadan önce, panoda yeni cihaz için telemetri geçmişinin, cihaz sıcaklığının hiçbir zaman 45 dereceyi aşmadığını gösterdiğine dikkat edin.

1.  Cihaz listesine geri gidin.

2.  **Cihazlar Listesi**'nde yeni cihazınızı seçin ve ardından cihaz için yeni bir kural eklemek üzere **Kural ekle**'ye tıklayın.

3. Veri alanı olarak **Temperature** ve sıcaklık 47 dereceyi aştığında çıktı olarak **AlarmTemp** kullanan bir kural oluşturun:

    ![][img-adddevicerule]

4. Yaptığınız değişiklikleri kaydetmek için **Kuralları Kaydet ve Görüntüle**'ye tıklayın.

5.  Yeni cihaz için cihaz ayrıntıları bölmesinde **Komutlar**'a tıklayın.

    ![][img-adddevicerule2]

6.  Komut listesinden **ChangeSetPointTemp**'i seçin ve **SetPointTemp**'i 45 olarak ayarlayın. Ardından **Komut Gönder**'e tıklayın:

    ![][img-adddevicerule3]

7.  Çözüm panosuna geri gidin. Kısa bir süre sonra yeni cihazınız tarafından raporlanan sıcaklık 47 derece eşiğini aştığında **Uyarı Geçmişi** bölmesinde yeni bir giriş görürsünüz:

    ![][img-adddevicerule4]

8. Panonun **Kurallar** sayfasında tüm kurallarınızı gözden geçirebilir ve düzenleyebilirsiniz:

    ![][img-rules]

9. Panonun **Eylemler** sayfasında bir kurala yanıt olarak gerçekleştirilebilecek tüm eylemleri gözden geçirebilir ve düzenleyebilirsiniz:

    ![][img-actions]

> [AZURE.NOTE] Bir kurala yanıt olarak bir e-posta iletisi veya SMS gönderebilen veya bir [Mantıksal Uygulama][lnk-logic-apps] aracılığıyla bir iş kolu sistemiyle tümleşebilen eylemler tanımlamak mümkündür.

## Arka planda

Önceden yapılandırılmış bir çözümü dağıttığınızda, dağıtım işlemi seçtiğiniz Azure aboneliğinde birden çok kaynak oluşturur. Bu kaynakları Azure [portalında][lnk-portal] görüntüleyebilirsiniz. Dağıtım işlemi, önceden yapılandırılmış çözümünüz için seçtiğiniz ada dayalı bir ada sahip bir **kaynak grubu** oluşturur 

![][img-portal]

Her bir kaynağın ayarlarını, kaynak grubundaki kaynaklar listesinde seçerek görüntüleyebilirsiniz. Yukarıdaki ekran görüntüsü, önceden yapılandırılmış çözümde kullanılan IoT hub'ının ayarlarını gösterir.

Önceden yapılandırılmış çözüm için kaynak kodunu da görüntüleyebilirsiniz. Önceden yapılandırılmış uzaktan izleme çözümünün kaynak kodu [azure-iot-remote-monitoring][lnk-rmgithub] konumundadır:

- **DeviceAdministration** klasörü, pano için kaynak kodunu içerir.
- **Simulator** klasörü, sanal cihaz için kaynak kodunu içerir.
- **EventProcessor** klasörü, gelen telemetriyi işleyen arka uç işleme için kaynak kodunu içerir.

İşiniz bittiğinde, önceden yapılandırılmış çözümü Azure aboneliğinizden [azureiotsuite.com][lnk-azureiotsuite] sitesinde silebilirsiniz; böylece önceden yapılandırılmış çözümü oluşturduğunuzda sağlanan tüm kaynakları kolaylıkla silebilmeniz sağlanır.

> [AZURE.NOTE] Önceden yapılandırılmış çözümle ilgili her şeyi sildiğinizden emin olmak için [azureiotsuite.com][lnk-azureiotsuite] konumundan silin ve portaldaki kaynak grubunu silmekle kalmayın.

## Sonraki Adımlar

Artık çalışan bir önceden çözüm oluşturmuş olduğunuza göre, aşağıdaki rehberlere geçebilirsiniz:

-   [Önceden yapılandırılmış çözümleri özelleştirme kılavuzu][lnk-customize]
-   [Önceden yapılandırılmış çözümde tahmine dayalı bakıma genel bakış][lnk-predictive]

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

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-predictive]: iot-suite-predictive-overview.md
[lnk-connecting-devices]: iot-suite-connecting-devices.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-logic-apps]: https://azure.microsoft.com/documentation/services/app-service/logic/
[lnk-portal]: http://portal.azure.com/
[lnk-rmgithub]: https://github.com/Azure/azure-iot-remote-monitoring
[lnk-faq]: iot-suite-faq.md



<!----HONumber=Jun16_HO2-->


