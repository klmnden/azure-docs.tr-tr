---
title: "Azure’da Machine Learning için Visual Studio Code Araçları için hızlı başlangıç makalesi | Microsoft Docs"
description: "Bu makalede, deneme oluşturma, modeli deneme ve web hizmetini faaliyete geçirmeye gibi işlemlerle birlikte Machine Learning için Visual Studio Code Araçlarını kullanmaya nasıl başlayacağınız açıklanmaktadır."
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: get-started-article
ms.date: 09/12/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 680c1afab1af31cfef51b1c82d2db49f452ba6ab
ms.contentlocale: tr-tr
ms.lasthandoff: 09/25/2017

---

# <a name="visual-studio-code-tools-for-ai"></a>AI için Visual Studio Code Araçları
AI için Visual Studio Code Araçları, Derin Öğrenme / AI çözümlerinin derlenmesini, test edilmesini ve dağıtılmasını sağlayan bir geliştirme uzantısıdır. Başta bir çalıştırma geçmişi görünümü olmak üzere önceki eğitimlerin performans ve özel ölçüm ayrıntılarını vererek, Azure Machine Learning ile sorunsuz bir tümleştirme sağlar. [Microsoft Bilişsel Araç Seti (önceki adıyla CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit), [Google TensorFlow](https://www.tensorflow.org) ve diğer derin öğrenme çerçeveleriyle yeni projeye göz atma ve projeyi önyükleme olanağı tanıyan bir örnek gezgini görünümü sağlar. Son olarak, Azure Sanal Makineler veya GPU ile Linux sunucuları gibi uzak ortamlarda modelleri denemek üzere işleri göndermenizi sağlayan işlem hedeflerine yönelik bir gezgin sağlar. 
 
## <a name="getting-started"></a>Başlarken 
Başlamak için öncelikle [Visual Studio Code](https://code.visualstudio.com/Download)’u indirip yükleyin. Visual Studio Code’u açtıktan sonra aşağıdaki adımları uygulayın:
1. Etkinlik çubuğundaki uzantı simgesine tıklayın. 
2. “AI için Visual Studio Code Araçları” ifadesini arayın. 
3. **Yükle** düğmesine tıklayın. 
4. Yükleme sonrasında **Yeniden Yükle** düğmesine tıklayın. 

Visual Studio Code yeniden yüklendikten sonra uzantı etkinleştirilir. [Uzantıları yükleme hakkında daha fazla bilgi edinin](https://code.visualstudio.com/docs/editor/extension-gallery).

## <a name="exploring-project-samples"></a>Proje örneklerini keşfetme
AI için Visual Studio Code Araçları bir örnek gezgini içerir. Örnek gezgini, örneği yalnızca birkaç tıklama ile bulup denemeyi daha kolay hale getirir. Gezgini açmak için aşağıdakileri yapın:   
1. Komut paletini açın (Görüntüle > **Komutu Paleti** veya **Ctrl+Shift+P**).
2. "AI Örneği" ifadesini girin. 
3. "AI: Azure ML Örnek Gezginini Aç" önerisini aldığınızda seçip enter tuşuna basın. 

Alternatif olarak, örnek gezgini simgesine tıklayabilirsiniz.

## <a name="creating-a-new-project-from-the-sample-explorer"></a>Örnek gezgininden yeni proje oluşturma 
Farklı örneklere göz atabilir ve bu örnekler hakkında daha fazla bilgi alabilirsiniz. "Iris Sınıflandırma" örneğini buluna kadar göz atalım. Bu örneği temel alan yeni bir proje oluşturmak için aşağıdakileri yapın:
1. Proje örneğinde yükle düğmesine tıklayın, yeni proje oluşturma adımlarında dize kılavuzluk edilirken istenen komutlara dikkat edin. 
2. Proje için bir ad seçin, örneğin "Iris".
3. Projenizin oluşturulacağı klasör yolunu seçip enter tuşuna basın. 
4. Var olan bir çalışma alanını seçip enter tuşuna basın.

Proje oluşturulur.

> [!TIP]
> Azure kaynağınıza erişmek için oturum açmanız gerekir. Katıştırılmış terminalden "az login" komutunu girin ve yönergeleri izleyin. 

## <a name="submitting-experiment-with-the-new-project"></a>Yeni proje ile deneme gönderme
Yeni proje Visual Studio’da açılırken, farklı işlem hedefimize (yerel ve docker ile VM) bir iş göndeririz.
AI için Visual Studio Code Araçları, bir denemeyi göndermek için birden çok yol sağlar. 
1. Açılır Menü (sağ tıklayın) - **AI: İşi Gönder**.
2. Komut paletinden: "AI: İşi Gönder".
3. Alternatif olarak, katıştırılmış terminal kullanarak Azure CLI Machine Learning Komutları ile komutu doğrudan çalıştırabilirsiniz.

iris_sklearn.py dosyasını açın, sağ tıklayın ve **AI: İşi Gönder** öğesini seçin.
1. Platformunuzu seçin: "Azure Machine Learning".
2. Çalıştırma yapılandırmanızı seçin: "Docker-Python."

> [!NOTE]
> İlk kez bir iş gönderiyorsanız, "Machine Learning yapılandırması bulunamadı, oluşturuluyor..." iletisini alırsınız. Bir JSON dosyası açıldığında kaydedin (**Ctrl+S**).

İş gönderildikten sonra katıştırılmış terminal çalıştırmaların ilerlemesini gösterir. 

## <a name="view-list-of-jobs"></a>İşlerin listesini görüntüleme
İşler gönderildikten sonra işleri çalıştırma geçmişinden listeleyebilirsiniz.
1. Komut paletini açın (Görüntüle > **Komutu Paleti** veya **Ctrl+Shift+P**).
2. "AI Listesi" ifadesini girin.
3. "AI: İşleri Listele" önerisini aldığınızda seçip enter tuşuna basın.
4. "Azure Machine Learning" platformunu seçin.

İş Listesi Görünümü açılır ve tüm çalıştırmalar ile bazı ilgili bilgileri gösterir.

## <a name="view-job-details"></a>İş ayrıntılarını görüntüleme
İş Listesi Görünümü hala açıkken listedeki birincil çalıştırmaya tıklayın.
Bir işin sonuçlarına ilişkin ayrıntılara inmek için üstteki **iş kimliğine** tıklayarak ayrıntılı bilgileri görüntüleyin. 

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Azure Machine Learning’i IDE ile çalışacak şekilde yapılandırma](./how-to-configure-your-IDE.md)

