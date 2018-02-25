---
title: "Bir C# modül Azure IOT Edge ile hata ayıklamak için Visual Studio Code kullanma | Microsoft Docs"
description: "Bir C# modül Azure IOT kenar Visual Studio Code ile hata ayıklama."
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 5ed517cf8d70cd279a55b79ad448709116cf511b
ms.sourcegitcommit: fbba5027fa76674b64294f47baef85b669de04b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/24/2018
---
# <a name="use-visual-studio-code-to-debug-a-c-module-with-azure-iot-edge"></a>Bir C# modül Azure IOT Edge ile hata ayıklamak için Visual Studio Code kullanma
Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio Code](https://code.visualstudio.com/) Azure IOT kenar modüllerinizi hata ayıklamak için ana geliştirme aracı olarak.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. Geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz veya başka bir fiziksel cihaz IOT sınır cihazı olabilir.

Bu kılavuza başlamadan önce aşağıdaki Eğitmeni tamamlayın:
- [C# modül Azure IOT Edge geliştirmek için Visual Studio Code kullanma](how-to-vscode-develop-csharp-module.md)

Önceki öğreticiyi tamamladıktan sonra aşağıdaki öğeleri hazır olmalıdır:
- Geliştirme makinenizde çalıştıran yerel Docker kayıt defteri. Prototip oluşturma ve sınama amacıyla budur.
- `Program.cs` Dosyasıyla Son filtresi modülü kodu.
- Güncelleştirilmiş `deployment.json` algılayıcı ve filtre modülleri için dosya.
- Geliştirme makinenizde çalışan bir IOT kenar çalışma zamanı.

## <a name="build-your-iot-edge-module-for-debugging"></a>Hata ayıklama için IOT kenar modülünüzün derleme
1. Hata ayıklamayı başlatmak için kullanmak **dockerfile.debug** Docker görüntünüzü yeniden oluşturun ve IOT kenar çözümünüzü yeniden dağıtmak için. Visual Studio Code Explorer'da açmak için Docker klasörü seçin. Ardından **linux x64** klasörünü sağ tıklatın **Dockerfile.debug**ve seçin **yapı IOT kenar modülü Docker görüntü**.

    ![VS Code Gezgini'nin ekran görüntüsü](./media/how-to-debug-csharp-module/build-debug-image.png)

3. İçinde **Klasör Seç** penceresinde göz atın veya girin **./bin/Debug/netcoreapp2.0/publish**. Ardından **EXE_DIR olarak klasör seç**.
4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin: `<your container registry address>/filtermodule:latest`. Yerel kayıt defterine dağıtıyorsanız, olmalıdır: `localhost:5000/filtermodule:latest`.
5. Görüntü Docker deponuza iletin. Kullanım **kenar: anında IOT kenar modülü Docker görüntü** komut ve VS Code pencerenin üstündeki açılır metin kutusuna resim URL'si girin. Önceki adımda kullanılan aynı resim URL'si kullanın.
6. Yeniden kullanabilir `deployment.json` yeniden dağıtılamadı. Komut palette yazıp seçin **kenar: yeniden kenar** ile hata ayıklama sürümü, filtresi modülü almak için.

## <a name="start-debugging-in-vs-code"></a>VS Code'da hata ayıklamayı Başlat
1. VS Code hata ayıklama penceresine gidin. Tuşuna **F5**seçip **IOT Edge(.NET Core)**.

    ![VS Code ekran hata ayıklama penceresi](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. İçinde `launch.json`, Gözat **hata ayıklama IOT kenar özel modül (.NET Core)** bölümü. Altında **pipeArgs**, doldurmak `<container_name>`. Olmalıdır `filtermodule` bu öğreticideki.

    ![Ekran görüntüsü VS Code Launch.json'u](./media/how-to-debug-csharp-module/add-container-name.png)

3. Gözat **Program.cs**. İçinde bir kesme noktası ekleme `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Tuşuna **F5** yeniden ve ekleme işlemini seçin. Bu öğreticide, işlem adı olmalıdır `FilterModule.dll`.

    ![VS Code ekran hata ayıklama penceresi](./media/how-to-debug-csharp-module/attach-process.png)

5. VS Code hata ayıklama penceresinde sol panelinde değişkenleri görebilirsiniz. 

> [!NOTE]
> Önceki örnekte, .NET Core IOT kenar modülleri kapsayıcılarında hata ayıklama gösterilmektedir. Hata ayıklama sürümünde dayalı `Dockerfile.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı yansımanıza derlenirken hatalarla. C# modüllerinizi hata ayıklama tamamladıktan sonra doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` VSDBG üretime hazır IOT kenar modüller olmadan.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir IOT kenar modülü oluşturulan ve hata ayıklama için dağıtılır. VS kodda hata ayıklama başlatıldı. VS code'da Azure IOT kenar geliştirirken diğer senaryolar hakkında bilgi için bkz: 

> [!div class="nextstepaction"]
> [Geliştirme ve C# VS Code modülünde dağıtma](how-to-vscode-develop-csharp-module.md)
