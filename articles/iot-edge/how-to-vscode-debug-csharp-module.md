---
title: C# modülleri Azure IOT Edge ile hata ayıklama | Microsoft Docs
description: Bir C# modül Azure IOT kenar Visual Studio Code ile hata ayıklamak için Visual Studio Code kullanın.
services: iot-edge
keywords: ''
author: shizn
manager: timlt
ms.author: xshi
ms.date: 03/18/2018
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 035cb129bc3933c10f430b593226108f5d160972
ms.sourcegitcommit: d78bcecd983ca2a7473fff23371c8cfed0d89627
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Bir C# modül Azure IOT Edge ile hata ayıklamak için Visual Studio Code kullanma
Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio (VS) kod](https://code.visualstudio.com/) Azure IOT kenar modüllerinizi hata ayıklamak için ana geliştirme aracı olarak.

## <a name="prerequisites"></a>Önkoşullar
Bu makalede, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. Geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz veya başka bir fiziksel cihaz IOT sınır cihazı olabilir.

> [!NOTE]
> C# linux amd64 kapsayıcıları modülünde yalnızca ayıklayabilirsiniz.

Bu makaledeki yönergeleri izlemeden önce bölümündeki adımları tamamlamanız [IOT kenar çözümünü birden fazla modülü Visual Studio Code ile geliştirme](tutorial-multiple-modules-in-vscode.md). Bundan sonra aşağıdaki öğeleri hazır olmalıdır:
- Geliştirme makinenizde çalıştıran yerel Docker kayıt defteri. Prototip ve test amacıyla için yerel bir Docker kayıt kullanmak için önerilir. Kapsayıcı kayıt defterinde güncelleştirebilirsiniz `module.json` her modül klasöründe dosyasında.
- Bir IOT kenar çözüm proje çalışma alanı içindeki Modül alt C# ile.
- `Program.cs` Dosyasıyla son modülü kodu.
- Geliştirme makinenizde çalışan bir kenar çalışma zamanı.

## <a name="build-your-iot-edge-c-module-for-debugging"></a>Hata ayıklama için IOT kenar C# modülünüzün derleme
1. Hata ayıklamayı başlatmak için kullanmanız gerekir **Dockerfile.amd64.debug** docker görüntünüzü oluşturmak ve kenar çözümünüzü yeniden dağıtmak için. VS Code Explorer'da gidin `deployment.template.json` dosya. İşlev resim URL'si ekleyerek güncelleştirme bir `.debug` uçtaki.

2. Çözümü yeniden derleyin. VS Code komutu palette yazın ve şu komutu çalıştırın **kenar: derleme IOT uç çözümünün**.

3. Azure IOT Hub cihazları Gezgini'nde bir IOT kenar cihaz kimliği sağ tıklayın ve ardından seçin **sınır cihazı için dağıtımı oluşturma**. Seçin `deployment.json` dosyasını `config` klasör. Daha sonra dağıtım başarıyla VS code'da kimliği tümleşik bir dağıtımı ile terminal oluşturulan görebilirsiniz.

Kapsayıcı durumunuzu VS Code Docker Gezgini'nde veya Çalıştır tarafından kontrol edebilirsiniz `docker images` Terminal komutu.

## <a name="start-debugging-c-module-in-vs-code"></a>C# VS Code modülünde hata ayıklamayı Başlat
1. VS Code tutar hata ayıklama yapılandırma bilgilerini bir `launch.json` içinde bulunan dosyasını bir `.vscode` çalışma alanınızdaki klasör. Bu `launch.json` dosya üretilen yeni bir IOT kenar çözüm oluşturduğunuzda. Hata ayıklama destekleyen yeni bir modül eklediğiniz her zaman güncelleştirir. Hata ayıklama görünümüne gidin ve karşılık gelen hata ayıklama yapılandırma dosyasını seçin.
    ![Select hata ayıklama yapılandırması](./media/how-to-debug-csharp-function/select-debug-configuration.jpg)

2. `program.cs` sayfasına gidin. Bu dosyada bir kesme noktası ekleyin.

3. Tıklatın **hata ayıklamayı Başlat** düğmesini veya tuşuna **F5**ve ekleme işlemini seçin.

4. VS kodda hata ayıklama Görünümü'nde sol panelinde değişkenleri görebilirsiniz. 

> [!NOTE]
> Önceki örnekte, .NET Core IOT kenar modülleri kapsayıcılarında hata ayıklama gösterilmektedir. Hata ayıklama sürümünde dayalı `Dockerfile.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı yansımanıza derlenirken hatalarla. C# modüllerinizi hata ayıklama tamamladıktan sonra doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` VSDBG üretime hazır IOT kenar modüller olmadan.

## <a name="next-steps"></a>Sonraki adımlar

[Azure IOT kenarıyla Azure işlevleri hata ayıklamak için Visual Studio Code kullanma](how-to-vscode-debug-azure-function.md)

