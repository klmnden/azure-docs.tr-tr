---
title: Bir bulut tabanlı IOT Uzaktan izleme çözümü - Azure'ı deneyin | Microsoft Docs
description: Bu hızlı başlangıçta Uzaktan İzleme Azure IoT çözümünü dağıtacak ve çözüm panosunda oturum açıp işlevleri kullanacaksınız.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/25/2019
ms.author: dobett
ms.openlocfilehash: 32e2d3f9e8bbd63944188355774558ca5ea7bd9d
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/18/2019
ms.locfileid: "58792510"
---
# <a name="quickstart-try-a-cloud-based-remote-monitoring-solution"></a>Hızlı Başlangıç: Bir bulut tabanlı uzaktan izleme çözümünü deneyin

Bu hızlı başlangıçta Azure IoT Uzaktan İzleme çözümü hızlandırıcısını dağıtmayı öğreneceksiniz. Bu bulut tabanlı çözümde **Dashboard** (Pano) sayfasını kullanarak sanal cihazları harita üzerinde görselleştirecek ve **Maintenance** (Bakım) sayfasını kullanarak sanal bir soğutucu cihazından gelen basınç uyarısına müdahale edeceksiniz. Bu çözüm hızlandırıcısını, kendi uygulamanızın başlangıç noktası veya bir öğrenme aracı olarak kullanabilirsiniz.

İlk dağıtımda, Contoso adlı bir şirket için bir çözüm hızlandırıcısı yapılandırılır. Contoso'da çalışan bir operatör olarak, farklı fiziksel ortamlara dağıtılmış olan soğutucular gibi farklı türdeki cihazları yönetiyorsunuz. Soğutucu cihazı Uzaktan İzleme çözümü hızlandırıcısına sıcaklık, nem ve basınç telemetri verilerini göndermektedir.

Bu hızlı başlangıçta dağıtan bir **temel** maliyetlerinizi azaltırken diğer yandan çözüm Hızlandırıcısını sınama ve tanıtma amaçlarıyla sürümü. Dağıtabileceğiniz farklı sürümleri hakkında daha fazla bilgi için bkz. [temel ve standart dağıtımları](iot-accelerators-remote-monitoring-deploy-cli.md#basic-and-standard-deployments).

Bu hızlı başlangıcı tamamlamak etkin bir Azure aboneliğinizin olması gerekir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="deploy-the-solution"></a>Çözümü dağıtma

Çözüm hızlandırıcısını Azure aboneliğinize dağıttığınızda ayarlamanız gereken yapılandırma seçenekleri vardır.

Azure hesabınızın kimlik bilgilerini kullanarak [azureiotsolutions.com](https://www.azureiotsolutions.com/Accelerators) adresinden oturum açın.

Tıklayın **Uzaktan izleme** Döşe. Üzerinde **Uzaktan izleme** sayfasında **şimdi deneyin**:

![Uzaktan İzlemeyi Seçme](./media/quickstart-remote-monitoring-deploy/remotemonitoring.png)

Seçin  **C# mikro Hizmetler** olarak **dağıtım seçenekleri**. Java ve C# uygulamalara aynı özelliklere sahip.

Uzaktan İzleme çözümü hızlandırıcınız için benzersiz bir **Solution name** (Çözüm adı) değeri girin. Bu hızlı başlangıçta biz bizim arıyoruz **contoso-rm**.

Çözüm hızlandırıcısını dağıtırken kullanmak istediğiniz **Subscription** (Abonelik) ve **Region** (Bölge) seçimini yapın. Genelde size en yakın bölgeyi seçmeniz gerekir. Bu hızlı başlangıçta kullanıyoruz **Doğu ABD**.
Seçebileceğiniz **Visual Studio Enterprise**, ancak siz bir [genel yönetici veya kullanıcı](iot-accelerators-permissions.md) Bunu yapmak için.

Dağıtımınıza başlamak için tıklatın **Oluştur**. Bu işlemin çalışması en az beş dakika sürer:

![Uzaktan İzleme çözümünün ayrıntıları](./media/quickstart-remote-monitoring-deploy/createform.png)

## <a name="sign-in-to-the-solution"></a>Çözümde oturum açma

Azure aboneliğinize dağıtım tamamlandığında, çözüm dosyasında yeşil bir onay işareti ve **Hazır** yazısı görürsünüz. Artık Uzaktan İzleme çözümü hızlandırıcınızda oturum açabilirsiniz.

**Provisioned solutions** (Sağlanan çözümler) sayfasında yeni Uzaktan İzleme çözümü hızlandırıcınıza tıklayın:

![Yeni çözümü seçme](./media/quickstart-remote-monitoring-deploy/choosenew.png)

Açılan panelden Uzaktan İzleme çözümü hızlandırıcınızla ilgili bilgileri görüntüleyebilirsiniz. Seçin **çözüm hızlandırıcınız Git** Uzaktan izleme çözüm hızlandırıcınız görüntülemek için:

![Çözüm paneli](./media/quickstart-remote-monitoring-deploy/solutionpanel.png)

İzin isteğini kabul etmek için **Accept** (Kabul et) öğesine tıklayın. Uzaktan İzleme çözümü panosu tarayıcınızda görüntülenir:

[![Çözüm panosu](./media/quickstart-remote-monitoring-deploy/solutiondashboard-inline.png)](./media/quickstart-remote-monitoring-deploy/solutiondashboard-expanded.png#lightbox)

## <a name="view-your-devices"></a>Cihazlarınızı görüntüleme

Çözüm panosu Contoso'nun simülasyon cihazlarıyla ilgili aşağıdaki bilgileri görüntüler:

* **Device statistics** (Cihaz istatistikleri) panelinde uyarıların özeti ve toplam cihaz sayısı gösterilir. Varsayılan dağıtımda Contoso farklı türlerde 10 sanal cihaza sahiptir.

* **Device locations** (Cihaz konumları) panelinde cihazların bulunduğu fiziksel konumlar gösterilir. Cihazda uyarılar olduğunda raptiyenin rengi değişir.

* **Alerts** (Uyarılar) panelinde cihazlarınızdan gelen uyarılar gösterilir.

* **Telemetry** (Telemetri) panelinde cihazlarınızdan gelen telemetri verileri gösterilir. Üstteki telemetri türlerine tıklayarak farklı telemetri akışlarını görüntüleyebilirsiniz.

* **Analytics** (Analiz) panelinde cihazlarınızdaki uyarılar hakkında toplu bilgiler gösterilir.

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

**İşler** panelinde **Çalıştırma yöntemi**’ni ve ardından **EmergencyValveRelease** yöntemini seçin. **ChillerPressureRelease** iş adını ekleyin ve **Uygula**’ya tıklayın. Bu ayarlar sizin için hemen yürütülen bir iş oluşturur.

İş durumunu görüntülemek için **Maintenance** (Bakım) sayfasına dönün ve **Jobs** (İşler) görünümünde iş listesini görüntüleyin. İşlemi çalıştırıldı görebilmek için önce birkaç saniye beklemeniz gerekebilir:

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


![Çözümü sil](media/quickstart-remote-monitoring-deploy/deletesolution-page.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta Uzaktan İzleme çözümü hızlandırıcısını dağıttınız ve varsayılan Contoso dağıtımındaki sanal cihazları kullanarak bir izleme görevini tamamladınız.

Simülasyon cihazları kullanan çözüm hızlandırıcısı hakkında daha fazla bilgi için aşağıdaki öğreticiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: IOT cihazlarınızı izleme](iot-accelerators-remote-monitoring-monitor.md)