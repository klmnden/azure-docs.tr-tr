---
title: 'Hızlı Başlangıç: Python - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Python için Speech SDK'sı kullanarak bir konuşma metin konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: chlandsi
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: quickstart
ms.date: 1/16/2019
ms.author: chlandsi
ms.openlocfilehash: 40869457ce933368e17a2054dfca50fc4505fa22
ms.sourcegitcommit: ba9f95cf821c5af8e24425fd8ce6985b998c2982
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2019
ms.locfileid: "54381581"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-python"></a>Hızlı Başlangıç: Python için Speech SDK'sı ile Konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Python için Speech SDK'sı aracılığıyla konuşma hizmeti kullanmayı gösterir. Bu, mikrofon girişinin konuşma tanıma nasıl oluşturulduğunu gösterir.

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce önkoşullarının listesi aşağıda verilmiştir:

* Konuşma hizmeti için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).
* [Python 3.5 (64-bit)](https://www.python.org/downloads/) veya üzeri.
* Python konuşma SDK paketi (X macOS sürüm 10.12 veya sonrası) Mac ve Linux (Ubuntu 16.04 veya 18.04 x64) (x64) Windows için kullanılabilir.
* Ubuntu üzerinde gerekli paketleri yüklemek için aşağıdaki komutları çalıştırın:

  ```sh
  sudo apt-get update
  sudo apt-get install build-essential libssl1.0.0 libcurl3 libasound2 wget
  ```

* Ayrıca Windows üzerinde gerekir [Microsoft Visual C++ yeniden dağıtılabilir için Visual Studio 2017](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) platformunuz için.

## <a name="install-the-speech-sdk"></a>Konuşma SDK'sını yükleme

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bilişsel hizmetler konuşma SDK'sı Python paketini yüklenebilir [Pypı](https://pypi.org/) komut satırında bu komutu kullanarak:

```sh
pip install azure-cognitiveservices-speech
```

Bilişsel Hizmetler Konuşma SDK'sının geçerli sürümü: `1.2.0`.

## <a name="support-and-updates"></a>Destek ve güncelleştirmeler

Konuşma SDK'sı Python paket güncelleştirmelerini Pypı dağıtılan ve açıklanan [sürüm notları](./releasenotes.md) sayfası.
Yeni bir sürüm varsa, kendisine komutuyla güncelleştirebilirsiniz `pip install --upgrade azure-cognitiveservices-speech`.
Hangi sürümünün inceleyerek şu anda yüklü olduğunu denetleyebilirsiniz `azure.cognitiveservices.speech.__version__` değişkeni.

Bir sorun olması ya da bir özellik eksik göz varsa bizim [destek sayfasını](./support.md).

## <a name="create-a-python-application-using-the-speech-sdk"></a>Konuşma SDK'sını kullanarak bir Python uygulaması oluşturma

### <a name="run-the-sample"></a>Örneği çalıştırma

Ya da kopyalayabilirsiniz [kod](#quickstart-code) bir kaynak dosyası için bu hızlı başlangıcı `quickstart.py` ve IDE'nizi veya konsolunda çalıştırın

```sh
python quickstart.py
```

Bu hızlı başlangıç öğreticisinde olarak indirebilirsiniz bir [Jupyter](https://jupyter.org) not defterinden [Bilişsel hizmetler konuşma örnekleri depomuzdan](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) ve not defteri olarak çalıştırın.

### <a name="sample-code"></a>Örnek kod

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Yükleme ve Speech SDK'sı, Visual Studio Code ile kullanma

1. [İndirme](https://www.python.org/downloads/) ve Python'ın 64 bit sürümünü (3.5 veya sonraki sürümler) bilgisayarınıza yükleyin.
1. [İndirme](https://code.visualstudio.com/Download) ve Visual Studio Code yükleyin.
1. Visual Studio Code'u açın ve Python uzantısı seçerek yüklemek **dosya** > **tercihleri** > **uzantıları** menüsünde ve "Python" için arama.
   ![Python uzantısını yükle](media/sdk/qs-python-vscode-python-extension.png)
1. Örneğin Windows Explorer'ı kullanarak, projede depolamak için bir klasör oluşturun.
1. Visual Studio Code'da tıklayarak **dosya** simgesine ve ardından oluşturduğunuz klasörünü açın.
   ![Klasör Aç](media/sdk/qs-python-vscode-python-open-folder.png)
1. Yeni bir Python kaynak dosyası oluşturmak `speechsdk.py`, yeni dosya simgesine tıklayarak.
   ![Dosya oluşturma](media/sdk/qs-python-vscode-python-newfile.png)
1. Kopyalama, yapıştırma ve Kaydet [Python kodu](#quickstart-code) için yeni oluşturulan dosya.
1. Konuşma hizmeti abonelik bilgilerinizi ekleyin.
1. Python yorumlayıcısı zaten seçili değilse, pencerenin altındaki durum çubuğunda sol tarafında görüntülenir.
   Aksi takdirde, açarak kullanılabilir Python yorumlayıcılarını listesini getirebilir miyim **komut paleti** (`Ctrl+Shift+P`) yazarak **Python: Yorumlayıcıyı seçme**, uygun bir tane seçin.
1. Konuşma SDK'sı Python paket, seçtiğiniz Python yorumlayıcısı için henüz yüklü değilse, bu kolayca gelen Visual Studio Code içinde yapılabilir.
   Konuşma SDK paketini yüklemek için komut paletini tekrar getirerek bir terminal açın (`Ctrl+Shift+P`) yazarak **Terminal: Yeni tümleşik Terminalini oluşturma**.
   Açılan terminalde komutu girin `python -m pip install azure-cognitiveservices-speech`, ya da sisteminize uygun komutu.
1. Örnek kodu çalıştırmak için herhangi bir yerde Düzenleyicisi içinde sağ tıklayıp **Python dosyasında çalıştırmak Terminal**.
   Birkaç Kelimeyle kez istenir ve transcribed metin kısa bir süre içinde daha sonra görüntülenmelidir varsayalım.
   ![Örneği çalıştırma](media/sdk/qs-python-vscode-python-run.png)

Bu yönergeleri izleyerek bir sorun varsa, daha kapsamlı başvuran [Visual Studio kod Python Eğitmen](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde Python örnekleri keşfedin](https://aka.ms/csspeech/samples)
