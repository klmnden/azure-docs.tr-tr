---
title: VS code'da Azure IOT kenar için birden fazla modülü hata ayıklama | Microsoft Docs
description: Birden fazla modülü ile Azure IOT kenar hata ayıklamak için Visual Studio Code kullanma
author: shizn
manager: ''
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 38c66129ac8244d18b0f94c1485c785015e29ab4
ms.sourcegitcommit: 0c490934b5596204d175be89af6b45aafc7ff730
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2018
ms.locfileid: "37049600"
---
# <a name="use-visual-studio-code-to-debug-multiple-modules-with-azure-iot-edge"></a>Birden fazla modülü ile Azure IOT kenar hata ayıklamak için Visual Studio Code kullanma
Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio (VS) kod](https://code.visualstudio.com/) IOT kenar üzerinde birden fazla modülü hata ayıklamak için.

## <a name="prerequisites"></a>Önkoşullar
Öğreticiyi tamamlamak [IOT kenar çözümünü birden fazla modülü Visual Studio Code ile geliştirme](tutorial-multiple-modules-in-vscode.md) ve IOT kenar cihazda çalışan en az iki modülleri olduğundan emin olun.

## <a name="multi-target-and-remote-debugging-in-vs-code"></a>Birden çok hedef ve uzak VS kodda hata ayıklama
VS Code ve Azure IOT kenar uzantısıyla kapsayıcı geliştirme makinenizde veya uzak bir fiziksel IOT sınır cihazı olarak çalışıp çalışmadığını modülü işlemi bir kapsayıcıda ekleyebilirsiniz. Hata ayıklama birden çok modülleri kapsayıcılarında çalıştıran birden fazla işlem ayrı kapsayıcılarında gerçekte ekleniyor. VS Code'da [çok hedef hata ayıklama](https://code.visualstudio.com/docs/editor/debugging#_multitarget-debugging) birden çok modülleri hata ayıklama sırasında kullanılabilir.

   > [!TIP]
   > Modül kapsayıcı uzak bir fiziksel IOT kenar cihazı çalışıyorsa, kurulum için gerekebilecek [Docker makine](https://docs.docker.com/machine/overview/) böylece geliştirme makinenizde Docker altyapısına uzaktan Docker ana iletişim kurabilirsiniz.

### <a name="build-your-iot-edge-modules-for-debugging-purpose"></a>Hata ayıklama amaçla modülleri IOT kenar derleme
1. Kullanmanıza gerek çok modülü hata ayıklamayı başlatmak için **Dockerfile.amd64.debug** docker görüntülerinizi oluşturmak ve kenar çözümünüzü yeniden dağıtmak için. VS Code Explorer'da gidin `deployment.template.json` dosya. Görüntü URL'nizde ekleyerek güncelleştirme bir `.debug` uçtaki. İki modülü görüntülerle gereksinim `.debug` en az. Önceki öğretici çözümden üzerinde çalışıyorsanız, C# işlevleri modülü ve bir C# modül sahip olmalıdır. Bu iki görüntü URL'leri ekleyerek güncelleştirme bir `.debug` sonuna ve bu dosyayı kaydedin. 
2. Çözümü yeniden derleyin. VS Code komutu palette yazın ve şu komutu çalıştırın **Azure IOT kenar: derleme IOT uç çözümünün**.
3. Azure IOT Hub cihazları Gezgini'nde bir IOT kenar cihaz kimliği sağ tıklayın ve ardından seçin **sınır cihazı için dağıtımı oluşturma**. Seçin `deployment.json` altında dosya `config` klasör. Daha sonra dağıtım başarıyla VS code'da kimliği tümleşik bir dağıtımı ile terminal oluşturulan görebilirsiniz.

Kapsayıcı durumunuzu VS Code Docker Gezgini'nde veya çalıştırarak denetleyebilirsiniz `docker ps` Terminal komutu.

### <a name="start-debugging-c-function-in-vs-code"></a>C# VS Code işlevinde hata ayıklamayı Başlat
1. VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` içinde bulunan dosyasını bir `.vscode` çalışma alanınızdaki klasör. Bu `launch.json` dosya üretilen yeni bir IOT kenar çözüm oluşturduğunuzda. Hata ayıklama destekleyen yeni bir modül eklediğiniz her zaman güncelleştirir. Hata ayıklama görünümüne gidin ve C# işlevleri modülü uzaktan hata ayıklama için karşılık gelen hata ayıklama yapılandırma dosyasını seçin.
2. `run.csx` sayfasına gidin. Bir kesme noktası işlevinde ekleyin.
3. Tıklatın **hata ayıklamayı Başlat** düğmesini veya tuşuna **F5**ve ekleme işlemini seçin.
4. VS kodda hata ayıklama Görünümü'nde sol panelinde değişkenleri görebilirsiniz. 

### <a name="start-debugging-c-module-at-the-same-time-in-vs-code"></a>VS code'da aynı anda C# modülü hata ayıklamayı Başlat
1. VS Code komutu paletindeki yazın ve "Çalışma: Yinelenen çalışma yeni pencerede" komutunu çalıştırın. Yeni bir VS Code penceresi ile aynı çalışma başlatır.
2. Hata ayıklama görünümüne gidin ve C# modül uzaktan hata ayıklama için karşılık gelen hata ayıklama yapılandırma dosyasını seçin.
3. `program.cs` sayfasına gidin. C# modülünde bir kesme noktası ekleyin.
4. Tıklatın **hata ayıklamayı Başlat** düğmesini veya tuşuna **F5**ve ekleme işlemini seçin.
5. VS kodda hata ayıklama Görünümü'nde sol panelinde değişkenleri görebilirsiniz. 

### <a name="see-variables-in-multiple-debugging-windows"></a>Birden çok hata ayıklama windows değişkenlerde bakın
1. Artık en az iki hata ayıklama oturumunun iki VS Code penceresinde çalıştıran sahipsiniz. Bir kesme noktası isabet.
2. Tuşuna `F10` veya Step Over düğmesini **Debug araç**.
3. Başka bir VS Code penceresinde kesme noktası isabet. 
4. Devam Yukarıdaki iki adımları, windows hata ayıklama birden çok VS code'da birden çok modüllerden değişkenleri görebilirsiniz.

> [!NOTE]
> Yukarıdaki örnek gösterir nasıl birden fazla modülü ile Azure IOT kenar hata ayıklama için. Hata ayıklama sürümünde dayalı `Dockerfile.amd64.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı yansımanıza derlenirken hatalarla. Doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` C# işlevinizi hata ayıklama işlemini tamamladıktan sonra üretime hazır IOT kenar işlevi için VSDBG olmadan.

## <a name="next-steps"></a>Sonraki adımlar

Yerleşik modülünüzün olduktan sonra bilgi nasıl [Visual Studio Code dağıtmak Azure IOT kenar modüllerden](how-to-deploy-modules-vscode.md) 0
