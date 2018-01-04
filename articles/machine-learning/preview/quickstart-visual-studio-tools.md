---
title: "Azure’da Machine Learning için Visual Studio Araçları hızlı başlangıç makalesi | Microsoft Docs"
description: "Bu makalede, deneme oluşturma, modeli deneme ve web hizmetini faaliyete geçirmeye gibi işlemlerle birlikte Machine Learning için Visual Studio Araçlarını kullanmaya nasıl başlayacağınız açıklanmaktadır."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/15/2017
ms.openlocfilehash: bbcb2ea5a7ceeb976f590393608cc29c67d9a49e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2018
---
# <a name="visual-studio-tools-for-ai"></a>AI için Visual Studio Araçları
AI için Visual Studio Araçları, Derin Öğrenme / AI çözümlerinin derlenmesini, test edilmesini ve dağıtılmasını sağlayan bir geliştirme uzantısıdır. Başta bir çalıştırma geçmişi görünümü olmak üzere önceki eğitimlerin performans ve özel ölçüm ayrıntılarını vererek, Azure Machine Learning ile sorunsuz bir tümleştirme sağlar. [Microsoft Bilişsel Araç Seti (önceki adıyla CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit), [Google TensorFlow](https://www.tensorflow.org) ve diğer derin öğrenme çerçeveleriyle yeni projeye göz atma ve projeyi önyükleme olanağı tanıyan bir örnek gezgini görünümü sağlar. Son olarak, Azure Sanal Makineler veya GPU ile Linux sunucuları gibi uzak ortamlarda modelleri denemek üzere işleri göndermenizi sağlayan işlem hedeflerine yönelik bir gezgin sağlar. Ayrıca [Azure Batch AI (Önizleme)](https://docs.microsoft.com/azure/batch-ai/) için kolaylaştırılmış erişim sağlar.
 
## <a name="getting-started"></a>Başlarken 
Başlamak için öncelikle [Visual Studio](https://www.visualstudio.com/downloads/)’yu indirip yüklemeniz gerekir. Visual Studio’yu açtıktan sonra aşağıdaki adımları uygulayın:
1. Visual Studio’da menü çubuğuna tıklayıp "Uzantılar ve Güncelleştirmeler…" öğesini seçin
2. "Çevrimiçi" sekmesine tıklayıp "Visual Studio Market’te Ara" öğesini seçin.
3. “AI için Visual Studio” ifadesini arayın. 
3. **İndir** düğmesine tıklayın. 
4. Yükleme sonrasında Visual Studio’yu yeniden başlatın. 

Visual Studio yeniden yüklendikten sonra uzantı etkinleştirilir. [Uzantıları bulma hakkında daha fazla bilgi edinin](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions).

> [!NOTE]
> AI için Visual Studio Araçları, Visual Studio 2015 veya 2017, Professional veya Enterprise sürümlerini gerektirir. Apple OSX sürümünü desteklemez. 


## <a name="exploring-project-samples"></a>Proje örneklerini keşfetme
AI için Visual Studio Araçları bir örnek gezgini içerir. Örnek gezgini, örneği yalnızca birkaç tıklama ile bulup denemeyi daha kolay hale getirir. Gezgini açmak için aşağıdakileri yapın:   
1. Menü çubuğunda **AI Araçları**’na tıklayın.
2. "Azure Machine Learning Galerisi" öğesine tıklayın.

Tüm Azure ML Örneklerini içeren bir sekme açılır.

## <a name="creating-a-new-project-from-the-sample-explorer"></a>Örnek gezgininden yeni proje oluşturma 
Farklı örneklere göz atabilir ve bu örnekler hakkında daha fazla bilgi alabilirsiniz. "Iris Sınıflandırma" örneğini buluna kadar göz atalım. Bu örneği temel alan yeni bir proje oluşturmak için aşağıdakileri yapın:
1. Proje örneğinde **yükle** düğmesine tıkladığınızda yeni bir iletişim kutusu açılır. 
2. Bir kaynak grubu, hesap ve çalışma alanı seçin.
3. Proje türünü Genel olarak bırakabilirsiniz.
4. Bir proje yolu ve proje adı girin ve ardından Enter tuşuna basın. 
5. Çözümü kaydetmek isteyip istemediğinizi soran bir iletişim kutusu açılır; Kaydet’e tıklayın. 

Tamamlandıktan sonra yeni bir Visual Studio örneğinde yeni bir proje açılır. 

> [!TIP]
> Azure kaynağınıza erişmek için oturum açmanız gerekir. Katıştırılmış terminalden "az login" komutunu girin ve yönergeleri izleyin. 

## <a name="submitting-experiment-with-the-new-project"></a>Yeni proje ile deneme gönderme
Visual Studio’da yeni proje açılırken, bir işlem hedefine (yerel veya docker ile VM) bir iş gönderin.
İşi göndermek için aşağıdakileri yapın: 
1. Çözüm gezgininde göndermek istediğiniz dosyaya sağ tıklayın ve **Başlangıç Dosyası Olarak Ayarla**’yı seçin.
2. Proje adını seçin, sağ tıklayın ve **İşi Gönder...** öğesini seçin.
3. Betiğinizi yürütecek kümeyi seçmenize olanak tanıyan yeni bir iletişim kutusu (ya da işlem hedefi) açılır.
4. **Gönder**’e tıklayın

İş gönderildikten sonra katıştırılmış terminal çalıştırmaların ilerlemesini gösterir.

## <a name="view-list-of-jobs"></a>İşlerin listesini görüntüleme
İş gönderildikten sonra işleri çalıştırma geçmişinden listeleyebilirsiniz.
1. **Sunucu Gezgini**’nde **AI Araçları**’na tıklayın.
2. Sonra **Azure Machine Learning**’i seçin
3. **İşler** menüsüne tıklayın.

İş gezgininde bu proje için gönderilen tüm denemeler listelenir. 

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
İş gezini görünümü açıkken listedeki birinci çalıştırmaya tıklayın.
Bunu yaptığınızda İş Özeti paneli ve Günlükler ve Çıktılar paneli yüklenir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Machine Learning’i IDE ile çalışacak şekilde yapılandırma](./how-to-configure-your-IDE.md)
