---
title: Azure Machine Learning ile yapay ZEKA uzantısı için Visual Studio Code araçları kullanma
description: Yapay ZEKA ve eğitim başlatma ve makine öğrenimi ve derin öğrenme modelleri VS code'da Azure Machine Learning hizmeti ile dağıtmak için Visual Studio Code araçları hakkında bilgi edinin.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: a2f37cffb37ce7008c3e372763784240e0d4250b
ms.sourcegitcommit: 9e179a577533ab3b2c0c7a4899ae13a7a0d5252b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2018
ms.locfileid: "49945561"
---
# <a name="vs-code-tools-for-ai-get-started-with-azure-machine-learning-from-visual-studio-code"></a>AI için VS Code araçları: Visual Studio code'dan Azure Machine Learning ile çalışmaya başlama

Bu makalede, Visual Studio Code (VS Code) uzantısı hakkında bilgi edineceksiniz **yapay ZEKA Araçları**ve eğitim başlatma ve makine öğrenimi ve derin öğrenme modelleri VS code'da Azure Machine Learning hizmeti ile dağıtın.

Kullanım, veri, eğitin ve test ML hazırlama için Azure Machine Learning hizmeti kullanmak için Visual Studio code'da AI uzantısı için araçları modeller üzerinde yerel ve uzak hedef işlem, bu modelleri dağıtma ve özel Ölçümler ve denemeleri izleyin.

## <a name="prerequisite"></a>Önkoşul

+ Visual Studio Code yüklenmesi gerekir. VS Code masaüstünüzde çalışan bir hafif ama güçlü bir kaynak kod düzenleyicidir. Python ve daha fazlası için yerleşik destek ile birlikte gelir.  [VS Code yükleme öğrenin](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Python 3.5 veya daha fazla yükleme](https://www.anaconda.com/download/).

+ Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="install-vs-code-tools-for-ai-extension"></a>VS Code araçları için yapay ZEKA uzantısını yükleme

Yüklediğinizde **yapay ZEKA Araçları** uzantısı (internet erişimi varsa) iki daha fazla uzantı otomatik olarak yüklenir. Bunlar [Azure hesabı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısı ve [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) uzantısı

Azure Machine Learning ile çalışmak için Python IDE'ye VS Code etkinleştirmek ihtiyacımız var. Çalışma [Visual Studio Code içindeki Python](https://code.visualstudio.com/docs/languages/python), araçları ile yapay ZEKA için otomatik olarak yüklenen Microsoft Python uzantı gerektirir. Uzantı, VS Code mükemmel bir IDE sağlar ve tüm işletim sistemlerinde Python yorumlayıcılarını çeşitli ile çalışır. Otomatik Tamamlama ve IntelliSense, linting, hata ayıklama ve birim testi birlikte conda ortamları ve sanal dahil olmak üzere, Python ortamları arasında kolayca geçiş olanağı sağlamak için VS Code'nın gücü tümünün yararlanır. Çalışan, düzenleme, bu kılavuzun denetleyin ve Python kodunda hata ayıklama, bkz: [Python Hello World Öğreticisi](https://code.visualstudio.com/docs/python/python-tutorial)

**Yapay ZEKA uzantısı için araçları yüklemek için:**

1. VS Code'u başlatın.

1. Bir tarayıcıda ziyaret edin: http://aka.ms/vscodetoolsforai. 

1. Bu web sayfasında tıklatın **yükleme**. 

1. Uzantı sekmesini tıklatın **yükleme**.

1. Hoş Geldiniz sekme VS Code'da uzantısı açar ve Azure simgesi etkinlik çubuğunuza eklenir.

   ![Visual Studio Code etkinlik çubuğundaki Azure simgesi](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. İletişim kutusunda **oturum** ve Azure kimlik doğrulaması yapmak için ekrandaki istemi izleyin. 
   
   VS Code araçları ile birlikte, yapay ZEKA için yüklü olduğu, Azure hesabı uzantısını Azure hesabınızla kimlik doğrulaması yardımcı olur. Komutları listesini görmek [Azure hesabı uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) sayfası.

> [!Tip] 
> Kullanıma [Intellicode uzantısı VS Code (Önizleme) için](https://go.microsoft.com/fwlink/?linkid=2006060). Intellicode geçerli kod bağlama göre en uygun Otomatik Tamamlama çıkarımını yapma gibi IntelliSense Python için bir dizi yapay ZEKA destekli özelliği sağlar.

## <a name="install-the-sdk"></a>SDK yükle

1. Python 3.5 veya üzeri yüklü ve VS Code tarafından tanınan olduğundan emin olun. Şimdi yüklemek, VS Code'u yeniden başlatın ve bölümündeki yönergeleri kullanarak bir Python yorumlayıcısı seçin https://code.visualstudio.com/docs/python/python-tutorial.

1. VS Code'da komut paletini açın **Ctrl + Shift + P**.

1. Türü 'Azure ML pip bulmak için SDK'sını Yükle' için SDK yükleme komutu. Azure Machine Learning ile çalışmak için Visual Studio Code önkoşullarına sahiptir yerel ve özel bir Python ortamı oluşturulur.
   ![Yükleme](./media/vscode-tools-for-ai/install-sdk.png)

1. Tümleşik terminal penceresinde, kullanılacak Python yorumlayıcısı belirtin ya da ziyaret **Enter** , varsayılan Python yorumlayıcısı kullanılacak.

   ![Yükleme](./media/vscode-tools-for-ai/python.png)

## <a name="get-started-with-azure-machine-learning"></a>Azure Machine Learning kullanmaya başlayın

VS Code kullanarak ML modelleri dağıtma, oluşturmanız gerekir ve eğitim başlamadan önce bir [Azure Machine Learning hizmeti çalışma alanında](concept-azure-machine-learning-architecture.md#workspace) bulutta verilerinizin modelleriniz ve kaynakları içerir. Oluşturun ve bu çalışma alanında ilk denemenizi oluşturma hakkında bilgi edinin.

1. Visual Studio Code etkinlik çubuğundaki Azure simgesine tıklayın. Azure: Machine Learning kenar çubuğu görünür.

   ![Yükleme](./media/vscode-tools-for-ai/createworkspace.gif)

1. Azure aboneliğinize sağ tıklayıp **çalışma alanı oluştur**. Bir liste görüntülenir. Animasyonlu görüntüde, abonelik adı 'OpenMind Studio' ve 'MyWorkspace' çalışma alanıdır. 

1. Listeden mevcut bir kaynak grubunu seçin veya Sihirbazı içinde komut paletini kullanarak yeni bir tane oluşturun.

1. Yeni bir çalışma alanınız için benzersiz ve açık bir ad alanına yazın. Ekran görüntülerinde 'MyWorkspace' çalışma alanı olarak adlandırılır.

1. ENTER tuşuna basın ve yeni bir çalışma alanı oluşturulur. Abonelik adı altındaki ağacın görüntülenir.

1. Çalışma alanı adına sağ tıklayın ve seçin **deney oluşturma** bağlam menüsünden.  Denemeleri, Azure Machine Learning kullanarak gerçekleştirdiğiniz çalıştırmaların izler.

1. Alanda denemenizi bir ad girin. Ekran görüntülerinde denemeyi 'MNIST' olarak adlandırılır.
 
1. ENTER tuşuna basın ve yeni bir deneme oluşturulur. Çalışma alanı adı altındaki ağacın görüntülenir.

1. Deneme adına sağ tıklayın ve seçin **bir yerel klasör ekleme**. Bu klasör, yerel Python betiklerini içermelidir. Klasörü, bulutta deneme sonra bağlandı. 

   Artık her denemenizi denemenizi ile önemli ölçümlerinizin tamamını deneme geçmişinde depolanacak ve eğittiğiniz modeli otomatik olarak Azure Machine Learning için karşıya yüklenen ve, deneme ölçüm ve günlükleri ile depolanan çalışır.

   [![VS code'da bir klasör ekleme](./media/vscode-tools-for-ai/attachfolder.gif)](./media/vscode-tools-for-ai/attachfolder.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Klavye kısayollarını kullanma

VS Code çoğunu gibi Azure Machine Learning özellikleri VS code'da klavyeden erişilebilir. Bilmeniz gereken en önemli tuş bileşimi, Ctrl + Shift + komut paletini getiren P ' dir. Buradan, tüm klavye kısayolları en yaygın işlemler de dahil olmak üzere VS Code işlevselliğini erişebilirsiniz.

[![AI için VS Code araçları için klavye kısayolları](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Şimdi, Azure Machine Learning ile çalışmak için Visual Studio Code kullanabilirsiniz.

Bilgi nasıl [işlem hedeflerini oluşturma eğitmek ve Visual Studio code'da model](how-to-vscode-train-deploy.md).