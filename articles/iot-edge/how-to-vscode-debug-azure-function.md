---
title: "Azure IOT kenarıyla Azure işlevleri hata ayıklamak için Visual Studio Code kullanma | Microsoft Docs"
description: "C# debug VS code'da Azure IOT kenarıyla Azure işlevleri"
services: iot-edge
keywords: 
author: shizn
manager: timlt
ms.author: xshi
ms.date: 12/20/2017
ms.topic: article
ms.service: iot-edge
ms.openlocfilehash: 4344a450d218a7424cd055cf086c1e4865c9af10
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/21/2017
---
# <a name="use-visual-studio-code-to-debug-azure-functions-with-azure-iot-edge"></a>Azure IOT kenarıyla Azure işlevleri hata ayıklamak için Visual Studio Code kullanma

Bu makalede kullanmaya yönelik ayrıntılı yönergeler sağlanmaktadır [Visual Studio Code](https://code.visualstudio.com/) IOT sınır, Azure işlevlerini hata ayıklamak için ana geliştirme aracı olarak.

## <a name="prerequisites"></a>Önkoşullar
Bu öğretici, bir bilgisayar veya geliştirme makine olarak Windows veya Linux çalıştıran sanal makine kullandığınızı varsayar. IOT sınır cihazı başka bir fiziksel aygıt olabilir veya geliştirme makinenizde IOT kenar Cihazınızı benzetimini yapabilirsiniz.

Bu kılavuza başlamadan önce aşağıdaki öğreticiler tamamlanmış olduğundan emin olun.
- [Visual Studio Code geliştirmek ve Azure IOT kenarına Azure işlevleri dağıtmak için kullanın](how-to-vscode-develop-azure-function.md)

Sonra önceki öğreticiyi tamamlamak, aşağıdaki öğeleri hazır olması gerekir,
- Geliştirme makinenizde çalıştıran yerel Docker kayıt defteri. Prototip ve test amacıyla için yerel bir Docker kayıt kullanmak için önerilir.
- `run.csx` Son filtre işlev kodu dosyasıyla.
- Güncelleştirilmiş `deployment.json` algılayıcı modülü ve filtre işlev modülü için dosya.
- Geliştirme makinenizde çalışan bir kenar çalışma zamanı.

## <a name="build-your-iot-edge-module-for-debugging-purpose"></a>Amaç hata ayıklama için IOT kenar modülünüzün derleme
1. Hata ayıklamayı başlatmak için kullanmanız gerekir **dockerfile.debug** docker görüntünüzü oluşturmak ve kenar çözümünüzü yeniden dağıtmak için. VS Code Explorer'da Docker klasörünü açmak için tıklatın. Ardından `linux-x64` klasörünü sağ tıklatın **Dockerfile.debug**ve'ı tıklatın **yapı IOT kenar modülü Docker görüntü**.
3. İçinde **klasörü seçin** penceresinde gidin **FilterFunction** proje ve tıklatın **EXE_DIR olarak klasör seç**.
4. VS Code pencerenin üstündeki açılır metin kutusuna görüntü adı girin. Örneğin: `<your container registry address>/filterfunction:latest`. Yerel kayıt defterine dağıtıyorsanız, olmalıdır `localhost:5000/filterfunction:latest`.
5. Görüntü Docker deponuza iletin. Kullanım **kenar: anında IOT kenar modülü Docker görüntü** komut ve VS Code pencerenin üstündeki açılır metin kutusuna resim URL'si girin. Yukarıdaki adım içinde kullanılan aynı resim URL'si kullanın.
6. Yeniden kullanabilir `deployment.json` yeniden dağıtılamadı. Palet komutta yazıp seçin **kenar: yeniden kenar** ile hata ayıklama sürümü çalıştıran filtre işlevinizi almak için.

## <a name="start-debugging-in-vs-code"></a>VS Code'da hata ayıklamayı Başlat
1. VS Code hata ayıklama penceresine gidin. Tuşuna **F5** seçip **IOT Edge(.Net Core)**
2. İçinde `launch.json`, gitmek **hata ayıklama IOT kenar işlevi (.NET Core)** bölümünde ve doldurmak `<container_name>`altında `pipeArgs`. Olmalıdır `filterfunction` bu öğreticideki.
3. İçin Run.csx gidin. Bir kesme noktası işlevinde ekleyin.
4. Tuşuna **F5** yeniden. Ve işlem eklemek için seçin.
5. VS kodda hata ayıklama pencerede, sol panelinde değişkenleri görebilirsiniz. 

> [!NOTE]
> Yukarıdaki örnek gösterir hata ayıklama .net nasıl kapsayıcılarında çekirdek IOT kenar işlevi. Hata ayıklama sürümünde dayalı `Dockerfile.debug`, içeren VSDBG (.NET Core komut satırı hata ayıklayıcı) kapsayıcı yansımanıza derlenirken hatalarla. Doğrudan kullanabilir veya özelleştirebilirsiniz öneririz `Dockerfile` C# işlevinizi hata ayıklama işlemini tamamladıktan sonra üretime hazır IOT kenar işlevi için VSDBG olmadan.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide bir Azure işlevi oluşturulan ve hata ayıklama amaçla IOT kenara dağıtılan ve VS kodda hata ayıklama başlatıldı. VS code'da Azure IOT kenar geliştirirken diğer senaryolar hakkında bilgi edinmek için aşağıdaki öğreticileri herhangi birini açın devam edebilirsiniz. 

> [!div class="nextstepaction"]
> [Geliştirme ve C# VS Code modülünde dağıtma](how-to-vscode-develop-csharp-module.md)

