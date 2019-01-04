---
title: Visual Studio kodu ile kullanma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning için Visual Studio Code yükleme ve Azure Machine Learning'de basit bir deneme oluşturma hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 902c659d2c51d69f2e9d0ef3a7401326e0e530eb
ms.sourcegitcommit: 25936232821e1e5a88843136044eb71e28911928
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/04/2019
ms.locfileid: "54013154"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Visual Studio Code için Azure Machine Learning'i kullanmaya başlayın

Bu makalede, nasıl yükleneceğini öğreneceksiniz **Visual Studio Code için Azure Machine Learning** uzantısı ve Azure Machine Learning hizmeti Visual Studio code'da (VS Code) ile ilk denemenizi oluşturun.

Azure Machine Learning uzantısı, veri, eğitin ve test makine öğrenimi modellerini yerel ve uzak işlem hedeflerde hazırlama, bu modelleri dağıtma ve özel Ölçümler ve deneyler izlemek için Azure Machine Learning hizmeti kullanmak için Visual Studio code'da kullanın.

## <a name="prerequisite"></a>Önkoşul


+ Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.

+ Visual Studio Code yüklenmesi gerekir. VS Code masaüstünüzde çalışan bir hafif ama güçlü bir kaynak kod düzenleyicidir. Python ve daha fazlası için yerleşik destek ile birlikte gelir.  [VS Code yükleme öğrenin](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Python 3.5 veya daha fazla yükleme](https://www.anaconda.com/download/).


## <a name="install-the-azure-machine-learning-for-vs-code-extension"></a>Azure Machine Learning için VS Code uzantısı yükleme

Yüklediğinizde **Azure Machine Learning** uzantısı (internet erişimi varsa) iki daha fazla uzantı otomatik olarak yüklenir. Bunlar [Azure hesabı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısı ve [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) uzantısı

Azure Machine Learning ile çalışmak için Python IDE'ye VS Code etkinleştirmek ihtiyacımız var. Çalışma [Visual Studio Code içindeki Python](https://code.visualstudio.com/docs/languages/python), Azure Machine Learning uzantısı ile otomatik olarak yüklenen Microsoft Python uzantı gerektirir. Uzantı, VS Code mükemmel bir IDE sağlar ve tüm işletim sistemlerinde Python yorumlayıcılarını çeşitli ile çalışır. Otomatik Tamamlama ve IntelliSense, linting, hata ayıklama ve birim testi birlikte conda ortamları ve sanal dahil olmak üzere, Python ortamları arasında kolayca geçiş olanağı sağlamak için VS Code'nın gücü tümünün yararlanır. Çalışan, düzenleme, bu kılavuzun denetleyin ve Python kodunda hata ayıklama, bkz: [Python Hello World Öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial)

**Azure Machine Learning uzantıyı yüklemek için:**

1. VS Code'u başlatın.

1. Bir tarayıcıda ziyaret edin: [Azure Machine Learning için Visual Studio Code (Önizleme)](https://aka.ms/vscodetoolsforai) uzantısı

1. Bu web sayfasında tıklatın **yükleme**. 

1. Uzantı sekmesini tıklatın **yükleme**.

1. Hoş Geldiniz sekme VS Code'da uzantısı açar ve Azure simgesi etkinlik çubuğunuza eklenir.

   ![Visual Studio Code etkinlik çubuğundaki Azure simgesi](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. İletişim kutusunda **oturum** ve Azure kimlik doğrulaması yapmak için ekrandaki istemi izleyin. 
   
   VS Code uzantısı için Azure Machine Learning ile birlikte yüklenen Azure hesabı uzantısını Azure hesabınızla kimlik doğrulaması yardımcı olur. Komutları listesini görmek [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) sayfası.

> [!Tip] 
> Kullanıma [Intellicode uzantısı VS Code (Önizleme) için](https://go.microsoft.com/fwlink/?linkid=2006060). Intellicode geçerli kod bağlama göre en uygun Otomatik Tamamlama çıkarımını yapma gibi IntelliSense Python için bir dizi yapay ZEKA destekli özelliği sağlar.

## <a name="install-the-sdk"></a>SDK yükle

1. Python 3.5 veya üzeri yüklü ve VS Code tarafından tanınan olduğundan emin olun. Şimdi yüklemek, VS Code'u yeniden başlatın ve bölümündeki yönergeleri kullanarak bir Python yorumlayıcısı seçin https://code.visualstudio.com/docs/python/python-tutorial.

1. VS Code'da komut paletini açın **Ctrl + Shift + P**.

1. Türü 'Azure ML pip bulmak için SDK'sını Yükle' için SDK yükleme komutu. Azure Machine Learning ile çalışmak için Visual Studio Code önkoşullarına sahiptir yerel ve özel bir Python ortamı oluşturulur.

   ![Python için Azure Machine Learning SDK'sını yükleyin](./media/vscode-tools-for-ai/install-sdk.png)

1. Tümleşik terminal penceresinde, kullanılacak Python yorumlayıcısı belirtin ya da ziyaret **Enter** , varsayılan Python yorumlayıcısı kullanılacak.

   ![Yorumlayıcı seçin](./media/vscode-tools-for-ai/python.png)

## <a name="get-started-with-azure-machine-learning"></a>Azure Machine Learning kullanmaya başlayın

VS Code kullanarak makine öğrenimi modelleri dağıtma, oluşturmanız gerekir ve eğitim başlamadan önce bir [Azure Machine Learning hizmeti çalışma alanında](concept-azure-machine-learning-architecture.md#workspace) bulutta verilerinizin modelleriniz ve kaynakları içerir. Oluşturun ve bu çalışma alanında ilk denemenizi oluşturma hakkında bilgi edinin.

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure Machine Learning Kenar çubuğunda görünür.

   [![Yükleme](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Azure aboneliğinize sağ tıklayıp **çalışma alanı oluştur**. Bir liste görüntülenir. Animasyonlu görüntüde, abonelik adı 'Ücretsiz deneme sürümü' ve 'TeamWorkspace' çalışma alanıdır. 

1. Listeden mevcut bir kaynak grubunu seçin veya Sihirbazı içinde komut paletini kullanarak yeni bir tane oluşturun.

1. Yeni bir çalışma alanınız için benzersiz ve açık bir ad alanına yazın. Ekran görüntülerinde 'TeamWorkspace' çalışma alanı olarak adlandırılır.

1. ENTER tuşuna basın ve yeni bir çalışma alanı oluşturulur. Abonelik adı altındaki ağacın görüntülenir.

1. Deneme düğümüne sağ tıklayın ve seçin **deney oluşturma** bağlam menüsünden.  Denemeleri, Azure Machine Learning kullanarak gerçekleştirdiğiniz çalıştırmaların izler.

1. Alanda denemenizi bir ad girin. Ekran görüntülerinde denemeyi 'MNIST' olarak adlandırılır.
 
1. ENTER tuşuna basın ve yeni bir deneme oluşturulur. Çalışma alanı adı altındaki ağacın görüntülenir.

1. Bir çalışma alanında bir denemeyi sağ tıklayın ve 'Etkin deneyim olarak Ayarla' seçin. **'Active'** deneme denemeyi olduğundan şu anda kullanmakta olduğunuz ve bulutta bu deneme için VS code'da açık klasörünüze bağlanacak. Bu klasör, yerel Python betiklerini içermelidir.

   Artık her denemenizi denemenizi ile önemli ölçümlerinizin tamamını deneme geçmişinde depolanacak ve eğittiğiniz modeli otomatik olarak Azure Machine Learning için karşıya yüklenen ve, deneme ölçüm ve günlükleri ile depolanan çalışır.

   [![VS code'da bir klasör ekleme](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Klavye kısayollarını kullanma

VS Code çoğunu gibi Azure Machine Learning özellikleri VS code'da klavyeden erişilebilir. Bilmeniz gereken en önemli tuş bileşimi, Ctrl + Shift + komut paletini getiren P ' dir. Buradan, tüm klavye kısayolları en yaygın işlemler de dahil olmak üzere VS Code işlevselliğini erişebilirsiniz.

[![VS Code için Azure Machine Learning için klavye kısayolları](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, Azure Machine Learning ile çalışmak için Visual Studio Code kullanabilirsiniz.

Bilgi nasıl [işlem hedeflerini oluşturma eğitmek ve Visual Studio code'da model](how-to-vscode-train-deploy.md).
