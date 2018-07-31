---
title: Azure'da bulut tabanlı IoT uzaktan izleme çözümü deneme | Microsoft Docs
description: Bu hızlı başlangıçta Uzaktan İzleme Azure IoT çözümünü dağıtacak ve çözüm panosunda oturum açıp işlevleri kullanacaksınız.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 07/12/2018
ms.author: dobett
ms.openlocfilehash: 50005e38214bf22aa664c2d2b0cc4f86da412818
ms.sourcegitcommit: 727a0d5b3301fe20f20b7de698e5225633191b06
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/19/2018
ms.locfileid: "39144503"
---
# <a name="quickstart-try-a-cloud-based-remote-monitoring-solution"></a>Hızlı başlangıç: Bulut tabanlı uzaktan izleme çözümünü deneme

Bu hızlı başlangıçta Azure IoT Uzaktan İzleme çözümü hızlandırıcısını dağıtarak bulut tabanlı uzaktan izleme çözümü simülasyonu çalıştırmayı öğreneceksiniz. Çözüm hızlandırıcısını dağıttıktan sonra çözümün **Dashboard** (Pano) sayfasını kullanarak sanal cihazları harita üzerinde görselleştirecek ve **Maintenance** (Bakım) sayfasını kullanarak sanal bir soğutucu cihazından gelen basınç uyarısına müdahale edeceksiniz. Bu çözüm hızlandırıcısını, kendi uygulamanızın başlangıç noktası veya bir öğrenme aracı olarak kullanabilirsiniz.

İlk dağıtımda, Contoso adlı bir şirket için Uzaktan İzleme çözümü hızlandırıcısı yapılandırılır. Contoso, farklı fiziksel ortamlara dağıtılmış olan soğutucular gibi farklı türdeki cihazları yönetmektedir. Soğutucu cihazı Uzaktan İzleme çözümü hızlandırıcısına sıcaklık, nem ve basınç telemetri verilerini göndermektedir.

Bu hızlı başlangıcı tamamlamak etkin bir Azure aboneliğinizin olması gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Çözüm hızlandırıcısını Azure aboneliğinize dağıttığınızda ayarlamanız gereken yapılandırma seçenekleri vardır.

Azure hesabınızın kimlik bilgilerini kullanarak [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) adresinden oturum açın.

**Uzaktan İzleme** kutucuğunda **Şimdi Deneyin**'e tıklayın.

![Uzaktan İzlemeyi Seçme](./media/quickstart-remote-monitoring-deploy/remotemonitoring.png)

**Create Remote Monitoring solution** (Uzaktan İzleme çözümü oluştur) sayfasında **Basic** (Temel) dağıtımlardan birini seçin. Çözüm hızlandırıcısını nasıl çalıştığını görmek için veya deneme amacıyla dağıtıyorsanız, maliyetleri minimumda tutmak için **Basic** (Temel) seçeneğini belirleyin.

Dil olarak **.NET**'i seçin. Java ve .NET uygulamaları aynı özelliklere sahiptir.

Uzaktan İzleme çözümü hızlandırıcınız için benzersiz bir **Solution name** (Çözüm adı) değeri girin. Bu hızlı başlangıçta kendimizinkini **contoso-rm2** olarak adlandırdık.

Çözüm hızlandırıcısını dağıtırken kullanmak istediğiniz **Subscription** (Abonelik) ve **Region** (Bölge) seçimini yapın. Genelde size en yakın bölgeyi seçmeniz gerekir. Bu hızlı başlangıçta **Visual Studio Enterprise** ve **Batı Avrupa** kullanıyoruz. Abonelikte [genel yönetici veya kullanıcı](iot-accelerators-permissions.md) olmanız gerekir.

Dağıtımı başlatmak için **Create Solution** (Çözüm Oluştur) öğesine tıklayın. Bu işlemin çalışması en az beş dakika sürer:

![Uzaktan İzleme çözümünün ayrıntıları](./media/quickstart-remote-monitoring-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Çözümde oturum açma

Azure aboneliğinize dağıtım tamamlandığında, çözüm dosyasında yeşil bir onay işareti ve **Hazır** yazısı görürsünüz. Artık Uzaktan İzleme çözümü hızlandırıcınızda oturum açabilirsiniz.

**Provisioned solutions** (Sağlanan çözümler) sayfasında yeni Uzaktan İzleme çözümü hızlandırıcınıza tıklayın:

![Yeni çözümü seçme](./media/quickstart-remote-monitoring-deploy/choosenew.png)

Açılan panelden Uzaktan İzleme çözümü hızlandırıcınızla ilgili bilgileri görüntüleyebilirsiniz. Uzaktan İzleme çözümü hızlandırıcısını görüntülemek için **Solution dashboard** (Çözüm panosu) öğesini seçin:

![Çözüm paneli](./media/quickstart-remote-monitoring-deploy/solutionpanel.png)

İzin isteğini kabul etmek için **Accept** (Kabul et) öğesine tıklayın. Uzaktan İzleme çözümü panosu tarayıcınızda görüntülenir:

[![Çözüm panosu](./media/quickstart-remote-monitoring-deploy/solutiondashboard-inline.png)](./media/quickstart-remote-monitoring-deploy/solutiondashboard-expanded.png#lightbox)

## <a name="view-your-devices"></a>Cihazlarınızı görüntüleme

Çözüm panosu Contoso'nun simülasyon cihazlarıyla ilgili aşağıdaki bilgileri görüntüler:

* **Device statistics** (Cihaz istatistikleri) bölümünde uyarıların özeti ve toplam cihaz sayısı gösterilir. Varsayılan dağıtımda Contoso farklı türlerde 10 sanal cihaza sahiptir.

* **Device locations** (Cihaz konumları) bölümünde cihazların bulunduğu fiziksel konumlar gösterilir. Cihazda uyarılar olduğunda raptiyenin rengi değişir.

* **Alerts** (Uyarılar) bölümünde cihazlarınızdan gelen uyarılar gösterilir.

* **Telemetry** (Telemetri) bölümünde cihazlarınızdan gelen telemetri verileri gösterilir. Üstteki telemetri türlerine tıklayarak farklı telemetri akışlarını görüntüleyebilirsiniz.

* **Analytics** (Analiz) bölümünde cihazlarınızdaki uyarılar hakkında toplu bilgiler gösterilir.

## <a name="respond-to-an-alert"></a>Uyarıya yanıt verme

Contoso'da bir operatör olarak cihazlarınızı çözüm panosundan izleyebilirsiniz. **Device statistics** (Cihaz istatistikleri) panelinde birçok kritik uyarı olduğunu, **Alerts** (Uyarılar) panelinde ise çoğunun bir soğutucu cihazından geldiği gösterilmektedir. Contoso'nun soğutucu cihazlarında 250 PSI üzerindeki iç basınç değeri cihazın düzgün çalışmadığını göstermektedir.

### <a name="identify-the-issue"></a>Sorunu tanımlama

**Dashboard** (Pano) sayfasının **Alerts** (Uyarılar) panelinde **Chiller pressure too high** (Soğutucu basıncı çok yüksek) uyarısını görebilirsiniz. Soğutucu harita üzerinde kırmızı raptiyeyle gösterilmektedir (haritayı kaydırmanız ve yakınlaştırmanız gerekebilir):

[![Panoda basınç uyarısı ve harita üzerinde cihaz gösterilir](./media/quickstart-remote-monitoring-deploy/dashboardalarm-inline.png)](./media/quickstart-remote-monitoring-deploy/dashboardalarm-expanded.png#lightbox)

**Alerts** (Uyarılar) panelinde, **Explore** (Keşfet) sütununda **Chiller pressure too high** (Soğutucu basıncı çok yüksek) kuralının yanındaki **...** simgesine tıklayın. Bu eylem sizi uyarıyı tetikleyen kuralın ayrıntılarını görüntüleyebileceğiniz **Maintenance** (Bakım) sayfasına yönlendirir.

**Chiller pressure too high** (Soğutucu basıncı çok yüksek) bakım sayfasında uyarıları tetikleyen kuralın ayrıntıları gösterilir. Bu sayfada uyarıların tetiklendiği zaman ve bunları tetikleyen cihaz da görüntülenir:

[![Tetiklenen uyarıların listesini gösteren Maintenance (Bakım) sayfası](./media/quickstart-remote-monitoring-deploy/maintenancealarmlist-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenancealarmlist-expanded.png#lightbox)

Uyarıyı tetikleyen sorunu ve ilgili cihazı tanımladınız. Operatör olarak gerçekleştirmeniz gereken sonraki adımlar uyarıyı onaylamak ve sorunu gidermektir.

### <a name="fix-the-issue"></a>Sorunu giderme

Diğer operatörlere bu uyarının üzerinde çalıştığınızı göstermek için uyarıyı seçin ve **Alert status** (Uyarı durumu) bilgisini **Acknowledged** (Onaylandı) olarak değiştirin:

[![Uyarıyı seçme ve onaylama](./media/quickstart-remote-monitoring-deploy/maintenanceacknowledge-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceacknowledge-expanded.png#lightbox)

Durum sütunundaki değer **Acknowledged** (Onaylandı) olarak değiştirilir.

Soğutucu üzerinde işlem yapmak için **Related information** (İlgili bilgiler) bölümüne inin, **Alerted devices** (Uyarıya sahip cihazlar) listesinden soğutucu cihazını seçin ve **Jobs** (İşler) öğesini seçin:

[![Cihazı seçme ve eylem zamanlama](./media/quickstart-remote-monitoring-deploy/maintenanceschedule-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceschedule-expanded.png#lightbox)

**İşler** panelinde **Çalıştırma yöntemi**’ni ve ardından **EmergencyValveRelease** yöntemini seçin. **ChillerPressureRelease** iş adını ekleyin ve **Uygula**’ya tıklayın. Bu ayarlar hemen yürütülen bir iş oluşturur.

İş durumunu görüntülemek için **Maintenance** (Bakım) sayfasına dönün ve **Jobs** (İşler) görünümünde iş listesini görüntüleyin. Birkaç saniye bekledikten sonra soğutucu üzerindeki basınç valfinin açılması işinin çalıştırıldığını görebilirsiniz:

[![Jobs (İşler) görünümünde işlerin durumu](./media/quickstart-remote-monitoring-deploy/maintenancerunningjob-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenancerunningjob-expanded.png#lightbox)

### <a name="check-the-pressure-is-back-to-normal"></a>Basıncın normale dönüp dönmediğini kontrol etme

Soğutucudan gelen basınç telemetrisini görüntülemek için **Dashboard** (Pano) sayfasına gidin, telemetri panelinden **Pressure** (Basınç) girişini seçin ve **chiller-02.0** adlı cihazın basınç değerini kontrol edin:

[![Basınç normale döndü](./media/quickstart-remote-monitoring-deploy/pressurenormal-inline.png)](./media/quickstart-remote-monitoring-deploy/pressurenormal-expanded.png#lightbox)

Olayı kapatmak için **Maintenance** (Bakım) sayfasına gidin, uyarıyı seçin ve durumunu **Closed** (Kapalı) olarak ayarlayın:

[![Uyarısı seçin ve kapatın](./media/quickstart-remote-monitoring-deploy/maintenanceclose-inline.png)](./media/quickstart-remote-monitoring-deploy/maintenanceclose-expanded.png#lightbox)

Durum sütunundaki değer **Closed** (Kapalı) olarak değiştirilir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğreticilere devam etmeyi planlıyorsanız Uzaktan İzleme çözümü hızlandırıcısı dağıtımını bırakın.

Çözüm hızlandırıcısına ihtiyacınız kalmadıysa [Sağlanan çözümler](https://www.azureiotsolutions.com/Accelerators#dashboard) sayfasında hızlandırıcıyı seçip **Çözümü Sil**’e tıklayarak bunu silebilirsiniz:

![Çözümü sil](media/quickstart-remote-monitoring-deploy/deletesolution.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Uzaktan İzleme çözümü hızlandırıcısını dağıttınız ve varsayılan Contoso dağıtımındaki sanal cihazları kullanarak bir izleme görevini tamamladınız.

Simülasyon cihazları kullanan çözüm hızlandırıcısı hakkında daha fazla bilgi için aşağıdaki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: IoT cihazlarınızı izleme](iot-accelerators-remote-monitoring-monitor.md)