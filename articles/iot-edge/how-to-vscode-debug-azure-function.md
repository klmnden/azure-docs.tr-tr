---
title: Azure IOT kenarıyla Azure işlevleri hata ayıklamak için Visual Studio Code kullanma | Microsoft Docs
description: C# debug VS code'da Azure IOT kenarıyla Azure işlevleri
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 3/20/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 8bd3513e932540f2dd710e0ac1536e757c00a3e1
ms.sourcegitcommit: ca05dd10784c0651da12c4d58fb9ad40fdcd9b10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/03/2018
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Azure IOT kenarıyla Azure işlevleri hata ayıklamak için Visual Studio Code kullanma

Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio Code](https://code.visualstudio.com/) IOT sınır, Azure işlevlerini hata ayıklamak için ana geliştirme aracı olarak.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. IOT sınır cihazı başka bir fiziksel aygıt olabilir veya geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz.

> [!NOTE]
> C# işlevleri linux amd64 kapsayıcılarında yalnızca ayıklayabilirsiniz.

Bu makaledeki yönergeleri izlemeden önce bölümündeki adımları tamamlamanız [IOT kenar çözümünü birden fazla modülü Visual Studio Code ile geliştirme](tutorial-multiple-modules-in-vscode.md). Bundan sonra aşağıdaki öğeleri hazır olmalıdır:
- Geliştirme makinenizde çalıştıran yerel Docker kayıt defteri. Prototip ve test amacıyla için yerel bir Docker kayıt kullanmak için önerilir. Kapsayıcı kayıt defterinde güncelleştirebilirsiniz `module.json` her modül klasöründe dosyasında.
- Bir IOT kenar çözüm proje çalışma alanı içindeki bir Azure işlevi modülü alt ile.
- `run.csx` İşlevi kodunuzu dosyasıyla.
- Geliştirme makinenizde çalışan bir kenar çalışma zamanı.

## <a name="build-your-iot-edge-function-module-for-debugging-purpose"></a>Amaç hata ayıklama için IOT kenar işlevi modülünüzün derleme
1. Hata ayıklamayı başlatmak için kullanmanız gerekir **Dockerfile.amd64.debug** docker görüntünüzü oluşturmak ve kenar çözümünüzü yeniden dağıtmak için. VS Code Explorer'da gidin `deployment.template.json` dosya. İşlev resim URL'si ekleyerek güncelleştirme bir `.debug` uçtaki.

    ![Hata ayıklama yansıması oluştur](./media/how-to-debug-csharp-function/build-debug-image.png)

2. Çözümü yeniden derleyin. VS Code komutu palette yazın ve şu komutu çalıştırın **kenar: derleme IOT uç çözümünün**.

3. Azure IOT Hub cihazları Gezgini'nde bir IOT kenar cihaz kimliği sağ tıklayın ve ardından seçin **sınır cihazı için dağıtımı oluşturma**. Seçin `deployment.json` altında `config` klasör. Daha sonra dağıtım başarıyla VS code'da kimliği tümleşik bir dağıtımı ile terminal oluşturulan görebilirsiniz.

> [!NOTE]
> Kapsayıcı durumunuzu VS Code Docker Gezgini'nde veya Çalıştır tarafından kontrol edebilirsiniz `docker images` Terminal komutu.

## <a name="start-debugging-c-function-in-vs-code"></a>C# VS Code işlevinde hata ayıklamayı Başlat
1. VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` içinde bulunan dosyasını bir `.vscode` çalışma alanınızdaki klasör. Bu `launch.json` yeni bir IOT kenar çözüm oluşturulurken dosyası oluşturuldu. Ve hata ayıklama desteği yeni bir modül eklediğiniz her sefer güncelleştirilir. Hata ayıklama görünümüne gidin ve karşılık gelen hata ayıklama yapılandırma dosyasını seçin.
    ![Select hata ayıklama yapılandırması](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. `run.csx` sayfasına gidin. Bir kesme noktası işlevinde ekleyin.

3. Hata Ayıklamayı Başlat düğmesine veya tuşuna tıklayın **F5**ve ekleme işlemini seçin.

4. VS kodda hata ayıklama Görünümü'nde sol panelinde değişkenleri görebilirsiniz. 


> [!NOTE]
> Yukarıdaki örnek gösterir hata ayıklama .net nasıl kapsayıcılarında çekirdek IOT kenar işlevi. Hata ayıklama sürümünde dayalı `Dockerfile.amd64.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı yansımanıza derlenirken hatalarla. Doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` C# işlevinizi hata ayıklama işlemini tamamladıktan sonra üretime hazır IOT kenar işlevi için VSDBG olmadan.

## <a name="next-steps"></a>Sonraki adımlar


[Bir C# modül Azure IOT Edge ile hata ayıklamak için Visual Studio Code kullanma](how-to-vscode-debug-csharp-module.md)

