---
title: Ortam - Azure IOT Edge üzerinde Machine Learning ayarlama | Microsoft Docs
description: Geliştirme ve dağıtım için machine learning uç modüllerinin için ortamınızı hazırlama.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: fd3b5766ec2bd8d1babf847598f1fbe5b6511ce7
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67432839"
---
# <a name="tutorial-set-up-an-environment-for-machine-learning-on-iot-edge"></a>Öğretici: Machine learning IOT Edge üzerinde için bir ortamı ayarlama

> [!NOTE]
> Bu makale bir serinin IOT Edge üzerinde Azure Machine Learning'i kullanma hakkında bir öğretici için parçasıdır. Bu makalede doğrudan gelmiş, başlangıç öneriyoruz [makaleyi](tutorial-machine-learning-edge-01-intro.md) serisindeki en iyi sonuçlar için.

Uçtan uca Azure Machine Learning Öğreticisi IOT Edge üzerinde bu makaleden ortamınızı geliştirme ve dağıtım için hazırlanmanıza yardımcı olur. İlk olarak, bir geliştirme makinesi, ihtiyacınız olan tüm araçları ile ayarlayın. Daha sonra gerekli bulut kaynakları, Azure içinde oluşturun.

## <a name="set-up-a-development-machine"></a>Bir geliştirme makinesini ayarlama

Bu adım genellikle bir bulut geliştiricisi tarafından gerçekleştirilir. Bazı yazılım ayrıca bir veri Bilimcisi için yararlı olabilir.

Bu makale boyunca kodlama, derleme, yapılandırma ve IOT Edge modülleri ve IOT cihazları dağıtma dahil olmak üzere çeşitli Geliştirici görevlerini gerçekleştiririz. Kullanım kolaylığı için bir Azure sanal makinesi zaten yapılandırılmış önkoşulları birçoğu ile oluşturan bir PowerShell Betiği oluşturduk. Oluşturduğumuz VM'nin işleyebilmesi gerekir [iç içe sanallaştırma](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization), DS8V3 makine boyutu seçtik neden olduğu.

Geliştirme VM ile ayarlanır:

* Windows 10
* [Chocolatey](https://chocolatey.org/)
* [Windows için docker Masaüstü](https://www.docker.com/products/docker-desktop)
* [Windows için Git](https://gitforwindows.org/)
* [Windows için Git kimlik bilgileri Yöneticisi](https://github.com/Microsoft/Git-Credential-Manager-for-Windows)
* [.Net Core SDK](https://dotnet.microsoft.com/)
* [Python 3](https://www.python.org/)
* [Visual Studio Code](https://code.visualstudio.com/)
* [Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azps-1.1.0)
* [VS Code uzantılarını](https://marketplace.visualstudio.com/search?target=VSCode)
  * [Azure IOT araçları](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools)
  * [Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)
  * [C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
  * [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker)
  * [PowerShell](https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell)

VM Geliştirici kesinlikle gerekli değil: tüm geliştirme araçları, yerel makine üzerinde çalıştırılabilir. Ancak, VM düzeyinde oyun alanı sağlamak için kullanmanız önerilir.

Oluşturmak ve sanal makineyi yapılandırmak için yaklaşık 30 dakika sürer.

### <a name="get-the-script"></a>Betik alın

Kopyala veya indir PowerShell betiğini [makine öğrenimi ve IOT Edge](https://github.com/Azure-Samples/IoTEdgeAndMlSample) örnek depo.

### <a name="create-an-azure-virtual-machine"></a>Bir Azure sanal makinesi oluşturun

DevVM dizin, bu öğreticiyi tamamlamak için uygun bir Azure sanal makinesi oluşturmak için gereken dosyaları içerir.

1. Yönetici olarak PowerShell'i açın ve kodu karşıdan yüklediğiniz dizine gidin. Kök dizinine kaynak anılacaktır `<srcdir>`.

    ```powershell
    cd <srcdir>\IoTEdgeAndMlSample\DevVM
    ```

2. Betiklerinin yürütülmesine izin vermek için aşağıdaki komutu çalıştırın. Seçin **Tümüne Evet** istendiğinde.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope Process
    ```

3. Oluştur-AzureDevVM.ps1 bu dizinden çalıştırın.

    ```powershell
    .\Create-AzureDevVm.ps1
    ```

    * İstendiğinde, aşağıdaki bilgileri sağlayın:
      * **Azure abonelik kimliği**: Azure portalında bulunabilir, abonelik kimliği
      * **Kaynak grubu adı**: Azure'da yeni veya mevcut bir kaynak grubu adı
      * **Konum**: Sanal makine oluşturulacağı bir Azure konumu seçin. Örneğin, westus2 veya northeurope. Daha fazla bilgi için [Azure konumları](https://azure.microsoft.com/global-infrastructure/locations/).
      * **AdminUsername**: Oluşturma ve sanal makinede kullanmak istediğiniz yönetici hesabı için unutmayacağınız bir ad sağlayın.
      * **AdminPassword**: Sanal makinede yönetici hesabı için bir parola ayarlayın.

    * Azure PowerShell'in yüklü değilse, betik yükleyecek [Az Azure PowerShell Modülü](https://docs.microsoft.com/powershell/azure/new-azureps-module-az?view=azps-1.1.0)

    * Azure'da oturum açmanız istenir.

    * Betik bilgileri, sanal Makinenizin oluşturulması için onaylar. Tuşuna `y` veya `Enter` devam etmek için.

Aşağıdaki adımları yürütme sırasında birkaç dakikalığına komut dosyasını çalıştırır:

* Henüz yoksa kaynak grubu oluşturun
* Sanal makineyi dağıtma
* Hyper-V sanal makine etkinleştirme
* Örnek depoyu kopyalamak ve yazılım geliştirme gereksinimini yükleyin
* VM'yi yeniden başlatın
* VM'ye bağlanmak için RDP dosyasını masaüstünüzde oluşturma

### <a name="set-auto-shutdown-schedule"></a>Otomatik kapatma zamanlamayı ayarlama

Maliyet azaltmanıza yardımcı olmak için VM 1900 Yinelenecek şekilde ayarlanmış bir otomatik kapanma zamanlaması ile oluşturuldu. Konum ve zamanlamaya bağlı olarak bu zamanlama güncelleştirmeniz gerekebilir. Kapatma zamanlamalarını güncelleştirmek için:

1. [Azure Portal](https://portal.azure.com) oturum açın.

2. Önceki bölümde belirttiğiniz kaynak grubunda sanal makinenize gidin.

3. Seçin **otomatik kapatma** yan Gezgin üzerinde.

4. Yeni bir kapatma zaman girin **zamanlanmış kapatma** veya değiştirme **saat dilimi** ardından **Kaydet**.

### <a name="connect-and-configure-development-machine"></a>Bağlanın ve geliştirme makinesi yapılandırın

Bu öğreticiyi tamamlamak için gereken yazılım yüklemesinin tamamlanması için ihtiyacımız olan bir sanal makine oluşturduk, şimdi.

#### <a name="start-a-remote-desktop-session"></a>Uzak Masaüstü oturumu Başlat

1. VM oluşturma komut masaüstünüzdeki bir RDP dosyası oluşturuldu.

2. Adlı dosyayı çift tıklayarak  **\<Azure VM adının\>.rdp**.

3. Uzak bağlantı yayımcısının bilinmeyen olduğunu belirten bir iletişim kutusu ile sunulur. Tıklayın **bu bilgisayara bağlantıları için sorma** onay kutusunu seçip **Connect**.

4. İstendiğinde, VM ve tıklatın ayarlamak için komut dosyası çalıştırılırken kullanılan AdminPassword sağlayın **Tamam**.

5. VM için sertifikayı kabul istenir. Seçin **bu bilgisayara bağlantıları için sorma** ve **Evet**.

#### <a name="install-visual-studio-code-extensions"></a>Visual Studio Code uzantılarını yükleme

Geliştirme makineye bağlandıktan sonra bazı kullanışlı uzantılar Visual Studio deneyiminizi geliştirme yapmak için kodu ekleyin.

1. Bir PowerShell penceresinde gidin **C:\\kaynak\\IoTEdgeAndMlSample\\DevVM**.

2. Yazarak sanal makinede yürütülecek komut dosyaları sağlar.

    ```powershell
    Set-ExecutionPolicy Bypass -Scope CurrentUser -Force
    ```

3. Betiği çalıştırın.

    ```powershell
    .\Enable-CodeExtensions.ps1
    ```

4. Komut dosyası, VS code uzantılarını yükleme birkaç dakika için çalışır:

    * Azure IoT Araçları
    * Python
    * C#
    * Docker
    * PowerShell

## <a name="set-up-iot-hub-and-storage"></a>IOT hub'ı ve depolamayı ayarlayın

Bu adımlar, genellikle bir bulut geliştiricisi tarafından gerçekleştirilir.

Azure IOT Hub herhangi bir IOT uygulaması kalbidir. Bu, IOT cihazları ve Bulutu arasında güvenli iletişim işler. IOT Edge makine öğrenimi çözümünüzü işlemi için ana koordinasyon noktasıdır.

* IOT Hub, IOT cihazlarından gelen verileri diğer aşağı akış hizmetleriyle yönlendirmek için rotalar kullanır. Biz IOT Hub cihaz verilerini Azure Depolama'ya burada bizim kalan faydalı ömrü (RUL) sınıflandırıcı eğitmek için Azure Machine Learning tarafından tüketilebilecek göndermek için rotalar yararlanır.

* Öğreticide daha sonra yapılandırmak ve müşterilerimizin Azure IOT Edge cihazı yönetmek için IOT hub'ı kullanacağız.

Bu bölümde, bir Azure IOT hub ve Azure depolama hesabı oluşturmak için bir betik kullanın. Ardından, Azure portalını kullanarak bir Azure depolama Blob kapsayıcısını hub tarafından alınan veri ileten bir yol yapılandırın. Bu adımları tamamlanması yaklaşık 10 dakika sürer.

### <a name="create-cloud-resources"></a>Bulut kaynaklarını oluşturma

1. Geliştirme makinenizde bir PowerShell penceresi açın.

1. Iothub dizine geçin.

    ```powershell
    cd C:\source\IoTEdgeAndMlSample\IoTHub
    ```

1. Oluşturma betiği çalıştırın. VM geliştirme oluştururken yaptığınız gibi aynı değerleri için abonelik kimliği, konum ve kaynak grubu kullanın.

    ```powershell
    .\New-HubAndStorage.ps1 -SubscriptionId <subscription id> -Location
    <location> -ResourceGroupName <resource group>
    ```

    * Azure'da oturum açmanız istenir.
    * Komut, bilgi hub'ı ve depolama hesabınızın oluşturulması için onaylar. Tuşuna `y` veya `Enter` devam etmek için.

Betiği çalıştırmak için yaklaşık iki dakika sürer. İşlem tamamlandıktan sonra komut dosyasını hub ve depolama hesabı adını çıkarır.

### <a name="review-route-to-storage-in-iot-hub"></a>IOT Hub'ındaki depolama yolu gözden geçirin

IOT hub'ı oluşturma bir parçası olarak, önceki bölümde karşılaştık betik özel bir uç noktası ve yol oluşturuldu. IOT hub'ı yollar, bir sorgu ifadesi ve bir uç nokta oluşur. Bir ileti ifade eşleşirse, veriler yol boyunca ilişkili uç noktasına gönderilir. Uç noktaları, Event Hubs, Service Bus kuyrukları ve konuları olabilir. Bu durumda, bir depolama hesabındaki Blob kapsayıcısına uç noktadır. Şimdi bizim betiği tarafından oluşturulan rota gözden geçirmek için Azure portalını kullanın.

1. [Azure portalı](https://portal.azure.com) açın.

1. Tüm hizmetleri soldaki Gezgin seçin, IOT arama kutusuna yazın ve seçin **IOT hub'ı**.

1. IOT hub'ı önceki adımda oluşturulan seçin.

1. IOT hub'ı yan Gezgin'de seçin **ileti yönlendirme**.

1. İleti yönlendirme sayfası, iki sekme içerir **yollar** ve **özel uç noktalar**. Seçin **özel uç noktalar** sekmesi.

1. Altında **Blob Depolama**seçin **turbofanDeviceStorage**.

1. Bu uç nokta için bir blob kapsayıcısını işaret eden Not adlı **devicedata** olarak adlandırılmış son adımda oluşturduğunuz depolama hesabına **iotedgeandml\<benzersiz soneki\>** .

1. Ayrıca unutmayın **Blob dosya adı biçimi** adı içerisindeki son öğe olarak, bunun yerine bölüm yerleştirmek için varsayılan biçimi değiştirilmiştir. Bu öğreticinin ilerleyen bölümlerinde Azure not defterleri ile yapacağız dosya işlemleri için daha kullanışlı bir biçimidir buluyoruz.

1. Geri dönmek için uç nokta ayrıntıları dikey pencereyi kapatmadan **ileti yönlendirme** sayfası.

1. Seçin **yollar** sekmesi.

1. Adlı rota seçin **turbofanDeviceDataToStorage**.

1. Rotanın uç noktası Not **turbofanDeviceStorage** özel uç nokta.

1. Konum ayarlamak için yönlendirme sorgusu **true**. Yani tüm cihaz telemetri iletilerini bu rota eşleşir ve bu nedenle tüm iletiler için gönderilecek **turbofanDeviceStorage** uç noktası.

1. Yol ayrıntıları kapatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, bir IOT hub'ı oluşturuldu ve bir Azure depolama hesabı için bir yol yapılandırılmış. Sonraki makalede, veri kümesinden sanal cihazlar IOT hub'ı üzerinden depolama hesabına göndereceğiz. Öğreticide daha sonra IOT Edge cihazı ve modülleri yapılandırdık sonra iptal eder yollar yeniden ziyaret ve yönlendirme sorgu biraz daha.

Machine Learning bu bölümünde IOT Edge öğretici kapsamında adımlar hakkında daha fazla bilgi için bkz:

* [Azure IoT Temel Konuları](https://docs.microsoft.com/azure/iot-fundamentals/)
* [IOT Hub ile ileti yönlendirmeyi yapılandırma](../iot-hub/tutorial-routing.md)
* [Azure portalını kullanarak IOT hub oluşturma](../iot-hub/iot-hub-create-through-portal.md)

İzlemek için bir sanal cihaz oluşturmak için sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Cihaz verileri üretme](tutorial-machine-learning-edge-03-generate-data.md)
