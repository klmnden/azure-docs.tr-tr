---
title: Visual Studio Code - Azure IOT Edge modülleri birden çok hata ayıklama | Microsoft Docs
description: Birden çok modül Azure IOT Edge ile hata ayıklamak için Visual Studio Code'u kullanma
author: shizn
manager: philmea
ms.author: xshi
ms.date: 06/27/2018
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 0c421eb1532536a3d11013073f0474f5ac37311b
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100472"
---
# <a name="use-visual-studio-code-to-debug-multiple-modules-with-azure-iot-edge"></a>Birden çok modül Azure IOT Edge ile hata ayıklamak için Visual Studio Code'u kullanma
Bu makale kullanmaya yönelik ayrıntılı yönergeler sağlar [Visual Studio (VS) kod](https://code.visualstudio.com/) birden çok modül IOT Edge üzerinde hata ayıklamak için.

## <a name="prerequisites"></a>Önkoşullar
Bu öğreticiyi tamamlamak [bir IOT Edge çözümü Visual Studio code'da birden çok modül ile geliştirme](tutorial-multiple-modules-in-vscode.md) ve IOT Edge Cihazınızda çalışan en az iki modül olduğundan emin olun.

## <a name="multi-target-and-remote-debugging-in-vs-code"></a>VS Code'da birden çok hedef ve uzaktan hata ayıklama
Kapsayıcının geliştirme makinenizde veya uzak fiziksel bir IOT Edge cihazı çalışıyor olsun, VS Code ve Azure IOT Edge uzantısı ile modülü işlemi bir kapsayıcıya ekleyebilirsiniz. Ayrı kapsayıcıları tek bir işlemde birden çok kapsayıcılarda çalıştırılan modüller aslında ekleme birden çok hata ayıklayın. VS Code [birden çok hedef hata ayıklama](https://code.visualstudio.com/docs/editor/debugging#_multitarget-debugging) birden çok modül hata ayıklarken kullanılabilir.

   > [!TIP]
   > Modülü kapsayıcınızı uzak bir fiziksel IOT Edge cihazının çalışıyorsa, kurulum için gerek duyabileceğiniz [Docker Machine](https://docs.docker.com/machine/overview/) böylece uzak Docker ana bilgisayarları için Docker altyapısı geliştirme makinenizde konuşabilirsiniz.

### <a name="build-your-iot-edge-modules-for-debugging-purpose"></a>IOT Edge modülleri için hata ayıklama amaçlı oluşturun.
1. Birden çok modül hata ayıklamayı başlatmak için kullanmanız gerekir **Dockerfile.amd64.debug** docker görüntülerinizi oluşturmak ve Edge çözümünüzü yeniden dağıtmak için. VS Code Gezgininde gidin `deployment.template.json` dosya. Ekleyerek, resim URL'lerini güncelleştirme bir `.debug` uçtaki. İhtiyacınız olan iki modül görüntüleri `.debug` en az. Önceki öğreticide çözümden üzerinde çalışıyorsanız, olmalıdır bir C# işlevleri modülü ve C# modülü. Bu iki görüntü URL'lerini ekleyerek güncelleştirme bir `.debug` uçtaki ve bu dosyayı kaydedin. 
2. Çözümünüzü yeniden oluşturun. VS Code komut paleti yazın ve şu komutu çalıştırın **Azure IOT Edge: IOT Edge çözüm**.
3. Azure IOT Hub cihazları Gezgini'nde bir IOT Edge cihaz Kimliğine sağ tıklayın ve ardından **Edge cihazı için dağıtım oluşturma**. Seçin `deployment.json` altında dosya `config` klasör. Dağıtım başarıyla kimliği VS Code tümleşik bir dağıtım ile terminal oluşturulduktan sonra görebilirsiniz.

VS Code Docker Gezgini veya çalıştırarak, kapsayıcı durumu kontrol edebilirsiniz `docker ps` terminalde komutu.

### <a name="start-debugging-c-function-in-vs-code"></a>Hata Ayıklamayı Başlat C# VS code'da işlevi
1. VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` dosya bulunan bir `.vscode` çalışma alanınızda bir klasör. Bu `launch.json` dosya yeni bir IOT Edge çözümü oluşturduğunuz zaman oluşturulduğu. Bu, her seferinde hata ayıklama destekleyen yeni Modül Ekle güncelleştirir. Hata ayıklama görünümüne gidin ve karşılık gelen hata ayıklama yapılandırma dosyası seçin C# işlevleri modülü uzaktan hata ayıklama.
2. `run.csx` sayfasına gidin. İşlev bir kesme noktası ekleyin.
3. Tıklayın **hata ayıklamayı Başlat** düğme veya basın **F5**, ekleme işlemini seçin.
4. VS Code hata ayıklama Görünümü'nde Sol paneldeki değişkenlerinde görebilirsiniz. 

### <a name="start-debugging-c-module-at-the-same-time-in-vs-code"></a>Hata Ayıklamayı Başlat C# VS code'da aynı anda Modülü
1. VS Code komut paleti yazın ve "Çalışma alanı: Yinelenen çalışma yeni pencerede" komutunu çalıştırın. Yeni bir VS Code penceresinin aynı çalışma alanı ile başlar.
2. Hata ayıklama görünümüne gidin ve karşılık gelen hata ayıklama yapılandırma dosyası seçin C# modülü uzaktan hata ayıklama.
3. `program.cs` sayfasına gidin. İçinde bir kesme noktası ekleyin C# modülü.
4. Tıklayın **hata ayıklamayı Başlat** düğme veya basın **F5**, ekleme işlemini seçin.
5. VS Code hata ayıklama Görünümü'nde Sol paneldeki değişkenlerinde görebilirsiniz. 

### <a name="see-variables-in-multiple-debugging-windows"></a>Birden çok hata ayıklama pencerelerinde değişkenleri bakın
1. Artık en az iki hata ayıklama oturumu iki VS Code penceresinin içinde çalışan vardır. Bir kesme noktası isabet.
2. Tuşuna `F10` veya Step Over düğmesine tıklayın **hata ayıklama araç çubuğu**.
3. Başka bir VS Code penceresinin kesme noktasına ulaşırsınız. 
4. Devam iki adımı, windows hata ayıklama birden çok VS code'da birden çok modül değişkenlerinden görebilirsiniz.

> [!NOTE]
> Yukarıdaki örnekte gösterir nasıl birden çok modül Azure IOT Edge ile hata ayıklama için. Hata ayıklama sürümünde temel `Dockerfile.amd64.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı görüntünüzü oluşturma sırasında. Doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` olmadan, hata ayıklama işlemini tamamladıktan sonra üretime hazır IOT Edge işlevinin VSDBG, C# işlevi.

## <a name="next-steps"></a>Sonraki adımlar

Yerleşik modülünüzde aldıktan sonra bilgi nasıl [Visual Studio Code için Azure IOT Edge'e dağıtma modüllerden](how-to-deploy-modules-vscode.md) 0
