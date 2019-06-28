---
title: Sanal cihaz verileri - Machine Learning, Azure IOT Edge üzerinde oluşturmak | Microsoft Docs
description: Daha sonra bir machine learning modeli eğitmek için kullanılabilir sanal telemetri oluşturan sanal cihazları oluşturun.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a93b1128fe1ea0e03efc9060f2c3c4a93145f838
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67432848"
---
# <a name="tutorial-generate-simulated-device-data"></a>Öğretici: Simülasyon cihazı verileri üretme

> [!NOTE]
> Bu makale bir serinin IOT Edge üzerinde Azure Machine Learning'i kullanma hakkında bir öğretici için parçasıdır. Bu makalede doğrudan gelmiş, başlangıç öneriyoruz [makaleyi](tutorial-machine-learning-edge-01-intro.md) serisindeki en iyi sonuçlar için.

Bu makalede, makine öğrenimi eğitim verileri, IOT Hub'ına telemetri gönderen bir cihazın benzetimini yapmak için kullanırız. Giriş belirtildiği gibi bu uçtan uca öğreticide [Turbofan engine performans düşüşü simülasyonu veri kümesi](https://c3.nasa.gov/dashlink/resources/139/) verileri eğitim ve test amacıyla uçak motorları kümesinden benzetimini yapmak için.

Eşlik eden readme.txt biliyoruz:

* Birden çok çok değişkenli zaman serisi verileri oluşur
* Her bir veri kümesi, eğitim ve test altkümelere, ayrılmıştır
* Her zaman serisi farklı bir altyapısı olan
* Her motor ilk wear ve değişim üretim derecesi ile başlar.

Bu öğreticide, tek bir veri kümesi (FD003) eğitim veri kümesini kullanırız.

Gerçekte her motor bağımsız IOT cihazına olacaktır. Kullanılabilir İnternet'e bağlı turbofan altyapıları koleksiyonunu yoktur varsayıldığında, bu cihazlar için bir yazılım stand-in oluşturulacak.

Simülatör olduğu bir C# program program aracılığıyla sanal cihazlar IOT Hub'ınızla kaydolmak için IOT hub'ı API'lerini kullanır. Biz bu sağlanan NASA veri alt kümesinden her cihaz için verileri okumak ve bunları sanal IOT cihazı ile IOT hub'ınıza gönderir. Öğreticinin bu bölümü için tüm kod deposu DeviceHarness dizininde bulunabilir.

Yazılmış bir .NET core projesi DeviceHarness projedir C# oluşan dört sınıfları:

* **Programı:** Giriş noktası için yürütme kullanıcı girişini ve genel koordinasyon işlenmesinden sorumludur.
* **TrainingFileManager:** okuma ve seçili veri dosyası ayrıştırılırken sorumludur.
* **CycleData:** tek bir satır veri dosyasındaki ileti biçimine dönüştürülen temsil eder.
* **TurbofanDevice:** veri tek bir cihazı (zaman serisi) karşılık gelen bir IOT cihazı oluşturma ve IOT cihazı aracılığıyla IOT Hub'ına veri gönderen sorumlu.

Bu makalede açıklanan görevlerin tamamlanması yaklaşık 20 dakika sürer.

Bu adımda işe gerçek eşdeğer büyük olasılıkla cihaz geliştiriciler ve bulut geliştiricileri tarafından gerçekleştirilen.

## <a name="configure-visual-studio-code-and-build-deviceharness-project"></a>Visual Studio Code'u yapılandırma ve DeviceHarness proje

1. Önceki makalede gösterildiği gibi sanal makinenize Uzak Masaüstü oturumu açın.

1. Visual Studio Code'u açın.

1. Visual Studio Code'da seçin **dosya** > **Klasör Aç...** .

1. İçinde **klasör** metin girin `C:\source\IoTEdgeAndMlSample\DeviceHarness` tıklatıp **Klasör Seç** düğmesi.

   OmniSharp hatalar çıkış penceresinde görünüyorsa kaldırmanız gerekir C# uzantısı, kapatıp VS Code yükleme C# uzantısı ve ardından yeniden yükleme penceresi.

1. Uzantıları bu makinede ilk kez kullanıyorsanız bu yana bazı uzantılar güncelleştirin ve bunların bağımlılıklarını yükleyin. Uzantıyı güncelleştir istenebilir. Bu durumda, seçin **Reload Window**.

1. DeviceHarness için gerekli varlıkları eklemeniz istenir. Seçin **Evet** bunları eklemek için.

   * Bildirim görünmesi birkaç saniye sürebilir.
   * Bu bildirim kaçırdıysanız, "zil" simgesini sağ alt köşesindeki denetleyin.

   ![VS kod uzantısı açılan menüsü](media/tutorial-machine-learning-edge-03-generate-data/add-required-assets.png)

1. Seçin **geri** Paket bağımlılıklarını geri yüklemek için.

   ![VS Code geri yükleme istemi](media/tutorial-machine-learning-edge-03-generate-data/restore-package-dependencies.png)

1. Ortamınızı düzgün bir derlemesi tetikleyerek ayarlandığını doğrulamak `Ctrl + Shift + B` veya **Terminal** > **derleme görevi Çalıştır**.

1. Yapı görevinin çalışması için seçmeniz istenir. Seçin **yapı**.

1. Derleme çalıştırır ve bir başarı iletisi çıkarır.

   ![Derleme başarılı çıkış iletisi](media/tutorial-machine-learning-edge-03-generate-data/build-success.png)

1. Bunu seçerek varsayılan derleme görevi derleme yapabileceğiniz **Terminal** > **varsayılan derleme görevi Yapılandır...**  seçip **derleme** isteminde.

## <a name="connect-to-iot-hub-and-run-deviceharness"></a>IOT Hub'ına bağlanmak ve DeviceHarness çalıştırın

Derleme projesi sahibiz, bağlantı dizesini erişmek ve veri oluşturma ilerlemesini izlemek için IOT hub'ınıza bağlanın.

### <a name="sign-in-to-azure-in-visual-studio-code"></a>Visual Studio code'da azure'da oturum açın

1. Komut paletini açıp Azure aboneliğinizi Visual Studio code'da oturum `Ctrl + Shift + P` veya **görünümü** > **komut paleti...** .

1. Komut istemi arama için ve seçin, **Azure: Oturum**.

1. Bir tarayıcı penceresi açılır ve sizden kimlik bilgilerinizi ister. Bir başarı sayfasına yönlendirilirsiniz, tarayıcıyı kapatabilirsiniz.

### <a name="connect-to-your-iot-hub-and-retrieve-hub-connection-string"></a>IOT hub'ınıza bağlanmak ve hub'ı bağlantı dizesi alma

1. Visual Studio Code Gezgini'nin alt bölümünde seçin **Azure IOT Hub cihazları** genişletmek için çerçeve.

1. Genişletilmiş çerçeve içinde tıklayarak **IOT Hub'ı seçin**.

1. İstendiğinde, Azure aboneliğinizi ve IOT hub'ı seçin.

1. İçine tıklayın **Azure IOT Hub cihazları** çerçeve ve tıklayın **...**  diğer eylemler için. Seçin **kopyalama IOT Hub bağlantı dizesine**.

   ![IOT Hub bağlantı dizesini kopyalayın](media/tutorial-machine-learning-edge-03-generate-data/copy-hub-connection-string.png)

### <a name="run-the-deviceharness-project"></a>DeviceHarness projeyi Çalıştır

1. Seçin **görünümü** > **Terminal** Visual Studio Code Terminali açın.

   Bir komut istemi görmüyorsanız Enter'ı seçin.

1. Girin `dotnet run` terminalde.

1. IOT Hub bağlantı dizesini istendiğinde, önceki bölümünde kopyaladığınız bağlantı dizesini yapıştırın.

1. İçinde **Azure IOT Hub cihazları** çerçeve, yenile düğmesine tıklayın.

   ![IOT Hub cihaz listesini Yenile](media/tutorial-machine-learning-edge-03-generate-data/refresh-hub-device-list.png)

1. Cihazlar IOT Hub'ına eklenir ve cihazları yeşil renkte verileri göstermek için gösterilmediğini o cihaza gönderilen unutmayın.

1. Herhangi bir cihazda sağ tıklatıp seçerek hub'a gönderilen iletileri görüntüleyebileceğiniz **D2C iletisini İzlemeyi Başlat**. Visual Studio code'da çıkış bölmesinde iletileri gösterir.

1. Tıklayarak izlemeyi durdurmak **Azure IOT hub'ı Araç Seti** seçin ve çıkış bölmesinde **D2C iletisini İzlemeyi Durdur**.

1. Birkaç dakika sürer tamamlanmaya kadar çalışmasına izin vermek.

## <a name="check-iot-hub-for-activity"></a>Onay etkinliği için IOT Hub

IOT hub'ınıza DeviceHarness tarafından gönderilen verilerin geçti. Veri merkeziniz, Azure portalını kullanarak ulaştığını doğrulamak kolay bir işlemdir.

1. Açık [Azure portalında](https://portal.azure.com/) ve IOT hub'ınıza gidin.

1. Genel bakış sayfasında bulunan verilerin hub'ına gönderildiği görmeniz gerekir:  

   ![Buluta gönderilen iletilerin IOT hub'ında cihaz görünümü](media/tutorial-machine-learning-edge-03-generate-data/iot-hub-usage.png)

## <a name="validate-data-in-azure-storage"></a>Azure Depolama'da verileri doğrulama

Önceki makalede oluşturduğumuz depolama kapsayıcısı için IOT hub'ınıza gönderdik veri yönlendirildi. Verileri depolama hesabımız bakalım.

1. Azure portalında depolama hesabınıza gidin.

1. Depolama hesabı Gezgin seçin **Depolama Gezgini (Önizleme)** .

1. Depolama Gezgini'nde seçin **Blob kapsayıcıları** ardından **devicedata**.

1. IOT hub'ı, sonra yıl, ay, gün ve saat adı için klasör üzerinde içerik bölmesinde tıklayın. Ne zaman veri yazıldığı dakikaları temsil eden birkaç klasörü göreceksiniz.

   ![Blob depolama alanındaki klasörleri görüntüle](media/tutorial-machine-learning-edge-03-generate-data/confirm-data-storage-results.png)

1. Etiketli veri dosyalarını bulmak için bu klasörleri birine tıklayın **00** ve **01** bölümüne karşılık gelen.

1. Dosyaları yazılır [Avro](https://avro.apache.org/) biçimi ancak bu dosyalardan biri çift başka bir tarayıcı sekmesinde açılır ve kısmen veri işleme. Bunun yerine bir programda dosyasını açmanız istenir, VS Code seçebilirsiniz ve doğru bir şekilde işlenir.

1. Okuma veya şu anda verileri yorumlamak için gerek yoktur; sonraki makalede bunu yapacağız.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir grup sanal cihazı oluşturma ve bizim IOT hub'ı aracılığıyla ve bir Azure depolama kapsayıcısına bu cihazların veri göndermek için bir .NET Core projesi kullanılır. Bu proje, burada fiziksel cihazları sensör okumaları, işletimsel ayarları, başarısız sinyaller ve modları, vb. bir IOT Hub'ına ve ileriye doğru seçkin bir depolama alanına dahil olmak üzere veri gönderir ve bir gerçek dünya senaryoları benzetimini yapar. Yeterli veri toplandığında, biz sonraki makalede gösterilecektir cihaz, kalan faydalı ömrü (RUL) tahmin modellerinin eğitilmesi için kullanırız.

Bir machine learning ile veri modeli eğitmek için sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Eğitmek ve bir Azure Machine Learning modelini dağıtma](tutorial-machine-learning-edge-04-train-model.md)
