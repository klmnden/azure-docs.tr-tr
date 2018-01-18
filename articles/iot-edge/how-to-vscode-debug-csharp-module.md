---
title: "Bir C# modül Azure IOT Edge ile hata ayıklamak için Visual Studio Code kullanma | Microsoft Docs"
description: "VS code'da Azure IOT kenarıyla bir C# modülü hata ayıklama"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/06/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 1ab67cd8aaf59cde3157fcb877ce13f10cb432bb
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2018
---
# <a name="use-visual-studio-code-to-debug-c-module-with-azure-iot-edge"></a>Visual Studio Code hata ayıklama C# modülü için Azure IOT Edge kullanın.
Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio Code](https://code.visualstudio.com/) IOT kenar modüllerinizi hata ayıklamak için ana geliştirme aracı olarak.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. IOT sınır cihazı başka bir fiziksel aygıt olabilir veya geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz.

Bu kılavuza başlamadan önce aşağıdaki öğreticiler tamamlanmış olduğundan emin olun.
- [C# modül Azure IOT Edge geliştirmek için Visual Studio Code kullanma](how-to-vscode-develop-csharp-module.md)

Sonra önceki öğreticiyi tamamlamak, aşağıdaki öğeleri hazır olması gerekir,
- Geliştirme makinenizde çalıştıran yerel Docker kayıt defteri. Prototip ve test amacıyla için yerel bir Docker kayıt kullanmak için önerilir.
- `Program.cs` Son filtresi modülü kod ile dosya.
- Güncelleştirilmiş `deployment.json` algılayıcı modülü ve filtresi modülü için dosya.
- Geliştirme makinenizde çalışan bir kenar çalışma zamanı.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Amaç hata ayıklama için IOT kenar modülünüzün derleme
1. Hata ayıklamayı başlatmak için kullanmanız gerekir **dockerfile.debug** docker görüntünüzü oluşturmak ve kenar çözümünüzü yeniden dağıtmak için. VS Code Explorer'da Docker klasörünü açmak için tıklatın. Ardından `linux-x64` klasörünü sağ tıklatın **Dockerfile.debug**ve'ı tıklatın **yapı IOT kenar modülü Docker görüntü**.

    ![Hata ayıklama yansıması oluştur](./media/how-to-debug-csharp-module/build-debug-image.png)

3. İçinde **Klasör Seç** penceresinde göz atın veya girin `./bin/Debug/netcoreapp2.0/publish`. Tıklatın **EXE_DIR Klasör Seç**.
4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin: `<your container registry address>/filtermodule:latest`. Yerel kayıt defterine dağıtıyorsanız, olmalıdır `localhost:5000/filtermodule:latest`.
5. Görüntü Docker deponuza iletin. Kullanım **kenar: anında IOT kenar modülü Docker görüntü** komut ve VS Code pencerenin üstündeki açılır metin kutusuna resim URL'si girin. Yukarıdaki adım içinde kullanılan aynı resim URL'si kullanın.
6. Yeniden kullanabilir `deployment.json` yeniden dağıtılamadı. Palet komutta yazıp seçin **kenar: yeniden kenar** ile hata ayıklama sürümü, filtresi modülü almak için.

## <a name="start-debugging-in-vs-code"></a>VS Code'da hata ayıklamayı Başlat
1. VS Code hata ayıklama penceresine gidin. Tuşuna **F5** seçip **IOT Edge(.Net Core)**

    ![F5 tuşuna basın](./media/how-to-debug-csharp-module/f5-debug-option.png)

2. İçinde `launch.json`, gitmek **hata ayıklama IOT kenar özel modül (.NET Core)** bölümünde ve doldurmak `<container_name>`altında `pipeArgs`. Olmalıdır `filtermodule` bu öğreticideki.

    ![PipeArgs değiştirme](./media/how-to-debug-csharp-module/f5-debug-option.png)

3. Program.cs için gidin. İçinde bir kesme noktası ekleme `method static async Task<MessageResponse> FilterModule(Message message, object userContext)`.
4. Tuşuna **F5** yeniden. Ve işlem eklemek için seçin. Bu öğreticide, işlem adı olmalıdır`FilterModule.dll`

    ![İşlem ekleme](./media/how-to-debug-csharp-module/attach-process.png)

5. VS kodda hata ayıklama pencerede, sol panelinde değişkenleri görebilirsiniz. 

> [!NOTE]
> Yukarıdaki örnek gösterir hata ayıklama .net nasıl kapsayıcılarında çekirdek IOT kenar modüller. Hata ayıklama sürümünde dayalı `Dockerfile.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı yansımanıza derlenirken hatalarla. Doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` VSDBG C# modüllerinizi hata ayıklama işlemini tamamladıktan sonra üretime hazır IOT kenar modüller olmadan.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir IOT kenar modülü oluşturulan ve hata ayıklama amaçla dağıtılan ve VS kodda hata ayıklama başlatıldı. VS code'da Azure IOT kenar geliştirirken diğer senaryolar hakkında bilgi edinmek için aşağıdaki öğreticileri herhangi birini açın devam edebilirsiniz. 

> [!div class="nextstepaction"]
> [Geliştirme ve C# VS Code modülünde dağıtma](how-to-vscode-develop-csharp-module.md)
