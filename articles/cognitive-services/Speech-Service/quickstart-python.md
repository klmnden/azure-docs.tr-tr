---
title: 'Hızlı Başlangıç: Python - konuşma Hizmetleri konuşma tanıma'
titleSuffix: Azure Cognitive Services
description: Python için Speech SDK'sı kullanan bir konuşma metin konsol uygulaması oluşturmak için bu kılavuzu kullanın. İşiniz bittiğinde konuşmayı metne gerçek zamanlı dönüştürmek için bilgisayarınızın mikrofonunu kullanabilirsiniz.
services: cognitive-services
author: chlandsi
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 05/02/2019
ms.author: chlandsi
ms.openlocfilehash: 34af7544a7678dfd8c8f870369bf0b4b1083b96d
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65020709"
---
# <a name="quickstart-recognize-speech-with-the-speech-sdk-for-python"></a>Hızlı Başlangıç: Python için Speech SDK'sı ile Konuşma tanıma

[!INCLUDE [Selector](../../../includes/cognitive-services-speech-service-quickstart-selector.md)]

Bu makalede, Python için konuşma Hizmetleri Speech SDK'sı aracılığıyla kullanmayı gösterir. Bu, mikrofon girişinin konuşma tanıma nasıl oluşturulduğunu gösterir.

## <a name="prerequisites"></a>Önkoşullar

* Konuşma Hizmetleri için bir Azure aboneliği anahtarı. [Ücretsiz edinin](get-started.md).
* [Python 3.5 veya sonraki sürümler](https://www.python.org/downloads/).
* Bu işletim sistemleri için Python Speech SDK'sı paket kullanılabilir:
    * Windows: x64 ve x86.
    * Mac: X macOS sürüm 10.12 veya üzeri.
    * Linux: Ubuntu 16.04, Ubuntu, 18.04 x64 Debian 9.
* Linux üzerinde gerekli paketleri yüklemek için şu komutları çalıştırın:

  * Ubuntu üzerinde:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.0 libasound2
    ```

  * Debian 9:

    ```sh
    sudo apt-get update
    sudo apt-get install build-essential libssl1.0.2 libasound2
    ```

* Windows üzerinde ihtiyacınız [Microsoft Visual C++ yeniden dağıtılabilir Visual Studio 2017 için](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) platformunuz için.

## <a name="install-the-speech-sdk"></a>Konuşma SDK'sını yükleme

[!INCLUDE [License Notice](../../../includes/cognitive-services-speech-service-license-notice.md)]

Bu komut Python paketinden yükler [Pypı](https://pypi.org/) Speech SDK'sı için:

```sh
pip install azure-cognitiveservices-speech
```

## <a name="support-and-updates"></a>Destek ve güncelleştirmeler

Konuşma SDK'sı Python paket güncelleştirmeleri Pypı dağıtılan ve içinde bildirilen [sürüm notları](./releasenotes.md).
Yeni bir sürüm varsa, kendisine komutuyla güncelleştirebilirsiniz `pip install --upgrade azure-cognitiveservices-speech`.
Hangi sürümünün inceleyerek şu anda yüklü olduğunu denetleyin `azure.cognitiveservices.speech.__version__` değişkeni.

Bir sorununuz veya, bir özellik eksik bkz [destek ve Yardım seçeneklerini](./support.md).

## <a name="create-a-python-application-that-uses-the-speech-sdk"></a>Konuşma SDK'sını kullanan bir Python uygulaması oluşturma

### <a name="run-the-sample"></a>Örneği çalıştırma

Kopyalayabilirsiniz [örnek kod](#sample-code) bir kaynak dosyası için bu hızlı başlangıcı `quickstart.py` ve IDE'nizi veya konsolunda çalıştırın:

```sh
python quickstart.py
```

Bu hızlı başlangıç öğreticisinde olarak indirebilirsiniz bir [Jupyter](https://jupyter.org) not defterinden [Speech SDK'sı örnek depoyu](https://github.com/Azure-Samples/cognitive-services-speech-sdk/) ve not defteri olarak çalıştırın.

### <a name="sample-code"></a>Örnek kod

[!code-python[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/python/quickstart.py#code)]

### <a name="install-and-use-the-speech-sdk-with-visual-studio-code"></a>Yükleme ve Speech SDK'sı, Visual Studio Code ile kullanma

1. Bir 64 bit sürümünü yükleyip [Python](https://www.python.org/downloads/), 3.5 veya üzeri, bilgisayarınızda.
1. İndirme ve yükleme [Visual Studio Code](https://code.visualstudio.com/Download).
1. Visual Studio Code'u açın ve Python uzantısını yükleyin. Seçin **dosya** > **tercihleri** > **uzantıları** menüsünde. Arama **Python**.

   ![Python uzantısını yükle](media/sdk/qs-python-vscode-python-extension.png)

1. Projede depolamak için bir klasör oluşturun. Windows Gezgini'ni kullanarak bir örnektir.
1. Visual Studio Code'da seçin **dosya** simgesi. Ardından, oluşturduğunuz klasörü açın.

   ![Klasör Aç](media/sdk/qs-python-vscode-python-open-folder.png)

1. Yeni bir Python kaynak dosyası oluşturma `speechsdk.py`, yeni dosya simgesini seçerek.

   ![Dosya oluşturma](media/sdk/qs-python-vscode-python-newfile.png)

1. Kopyalama, yapıştırma ve Kaydet [Python kodu](#sample-code) için yeni oluşturulan dosya.
1. Konuşma Hizmetleri abonelik bilgilerinizi ekleyin.
1. Seçili olduğunda, pencerenin alt kısmındaki durum çubuğunun sol tarafındaki bir Python yorumlayıcısı görüntüler.
   Aksi takdirde, mevcut Python yorumlayıcılarını listesini taşıyın. (Ctrl + Shift + P) komut paletini açın ve girin **Python: Yorumlayıcıyı seçme**. Bir uygun olanı seçin.
1. Visual Studio Code içinde konuşma SDK'sı Python paketi yükleyebilirsiniz. Bunun için Python yorumlayıcısı seçtiğiniz henüz yüklü değil.
   Konuşma SDK paketini yüklemek için bir terminal açın. Komut paletini yeniden (Ctrl + Shift + P) getirin ve girin **Terminal: Yeni tümleşik Terminalini oluşturma**.
   Açılan terminalde komutu girin `python -m pip install azure-cognitiveservices-speech` veya sisteminiz için uygun komutu.
1. Örnek kodu çalıştırma için Düzenleyicisi içinde bir yere sağ tıklayın. Seçin **Python dosyası Terminal içinde Çalıştır**.
   Sorulduğunda, birkaç sözcük konuşun. Kısa bir süre sonra transcribed metni görüntüler.

   ![Bir örneği çalıştırma](media/sdk/qs-python-vscode-python-run.png)

Bu yönergeleri izleyerek sorunları varsa, daha kapsamlı başvuran [Visual Studio kod Python Eğitmen](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [GitHub üzerinde Python örnekleri keşfedin](https://aka.ms/csspeech/samples)
