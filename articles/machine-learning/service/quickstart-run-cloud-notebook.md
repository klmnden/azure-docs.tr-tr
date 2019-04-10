---
title: Hızlı Başlangıç bulutta bir not defteri çalıştırma
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile çalışmaya başlama. Yönetilen notebook sunucusu bulutta çalışma alanınızı kullanın.  Çalışma alanınızda denemeler, eğitmek ve makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
author: sdgilley
ms.author: sgilley
ms.date: 03/21/2019
ms.custom: seodec18
ms.openlocfilehash: 0672d90a25bc4c879d28512ab212f98f29efbf3b
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358208"
---
# <a name="quickstart-use-a-cloud-based-notebook-server-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak için bir bulut tabanlı bir not defteri sunucusu kullan

Bu makalede, Azure Machine Learning hizmetinde oturum kodu çalıştırmak için Azure not defterlerini kullanma [çalışma](concept-azure-machine-learning-architecture.md). Çalışma alanınızda denemeler, eğitmek ve Machine Learning ile makine öğrenimi modelleri dağıtmak için kullandığınız bulutta temel taşıdır. 

Bu hızlı başlangıçta bulut kaynakları kullanılmaktadır ve bu nedenle herhangi bir yükleme yapmanıza gerek yoktur. Bunun yerine ortamınızda kullanmak için bkz: [hızlı başlangıç: Azure Machine Learning'i kullanmaya başlamak için kendi notebook sunucusu kullanmak](quickstart-run-local-notebook.md).  
 
Bu hızlı başlangıçta, aşağıdaki eylemleri gerçekleştirin:

* Python ile çalışma alanınızda bir Jupyter not defterine bağlanın. Not defterini pi ve günlükleri hataları her yinelemede, tahmin kodunu içerir. 
* Oturum hata değerlerini çalışma alanınızda görüntüleyin.

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](https://aka.ms/AMLFree) bugün.

## <a name="prerequisite"></a>Önkoşul

1. [Bir Azure Machine Learning çalışma alanı oluşturma](setup-create-workspace.md#portal) tane yoksa.

1. Çalışma alanınızda açın [Azure portalında](https://portal.azure.com/).  Bkz. nasıl [çalışma alanınızı bulmak](how-to-manage-workspace.md#view).

## <a name="use-your-workspace"></a>Çalışma alanınızla kullanmak

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]



Bir çalışma alanı, makine öğrenimi betiklerini yönetmenize nasıl yardımcı olduğunu öğrenin. Bu bölümde, aşağıdaki adımları uygulayın:

* Azure Notebooks'da bir not defteri açma.
* Günlüğe kaydedilen bazı değerler oluşturan kodu çalıştırma.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Bu örnek, çalışma alanının betikte oluşturulan bilgileri izlemenize nasıl yardımcı olduğunu gösterir. 

### <a name="open-a-notebook"></a>Not defterini açma 

[Azure not defterleri](https://notebooks.azure.com) Machine Learning çalıştırmak için gereken her şeyi ile önceden yapılandırılmış Jupyter not defterleri için ücretsiz bulut platformu sağlar. Çalışma alanınızda, Azure Machine Learning hizmeti çalışma alanında kullanmaya başlamak için bu platform başlatabilirsiniz.

1. Çalışma alanı genel bakış sayfasında **alma başlatıldı Azure not defterleri** ilk denemenizi Azure not defterlerinde denemek için.  Azure not defterleri, ücretsiz bulutta çalıştırmadan Jupyter Notebook sağlayan ayrı bir hizmettir.  Bu bağlantı hizmeti kullandığınızda, çalışma alanınıza bağlanma hakkında bilgi Azure not defterlerinde oluşturduğunuz kitaplığa eklenir.

   ![Çalışma alanını keşfedin](./media/quickstart-run-cloud-notebook/explore-aml.png)

1. Azure Not Defterleri'nda oturum açın.  Azure portalında oturum açmak için kullandığınız hesapla oturum açmanız emin olun. Oturum açabilmeniz için kuruluşunuzda [yönetici onayı](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) gerekli olabilir.

1. Siz oturum açtıktan sonra, yeni bir sekme açılır ve `Clone Library` istemi görüntülenir. Bu kitaplık kopyalama not defterleri ve diğer dosyaları bir dizi Azure not defterleri hesabınızda yükler.  Bu dosyaları Azure Machine Learning yeteneklerini keşfetmenize yardımcı olur.

1. Onay kutusunu temizleyin **genel** böylece çalışma alanı bilgilerinizi başkalarıyla paylaşmayın.

1. Seçin **kopya**.

   ![Bir kitaplık kopyalama](./media/quickstart-run-cloud-notebook/clone.png)

1. Proje durumu durdurulduğunu görüyorsanız tıklayın **ücretsiz bilgisayarda** ücretsiz notebook sunucusu kullanmak için.

    ![Proje üzerinde ücretsiz işlem çalıştırma](./media/quickstart-run-cloud-notebook/run-project.png)

### <a name="run-the-notebook"></a>Not defterini çalıştırma

Bu proje için dosya listesinde, gördüğünüz bir `config.json` dosya. Bu yapılandırma dosyası, çalışma alanı bilgilerini Azure portalında oluşturduğunuz içerir.  Bu dosya bağlanmak ve bilgi çalışma alanınıza eklemek için kodunuzu sağlar.

1. Seçin **01.run experiment.ipynb** not defterini açın.

1. Durum alanı çekirdek başlatıldı kadar beklenecek söyler.  Çekirdek hazır olduğunda ileti kaybolur.

    ![Çekirdek başlatmak bekle](./media/quickstart-run-cloud-notebook/wait-for-kernel.png)

1. Çekirdek başlatıldıktan sonra bir hücre, bir zaman kullanarak çalıştırın **Shift + Enter**. Veya **hücreleri** > **tümünü Çalıştır** tüm not defterlerini çalıştırmak için. Bir yıldız işareti gördüğünüzde __*__, hücrede yanında hücre hala çalışıyor. Hücredeki kodun çalışması tamamlandığında bir sayı görünür. 

1. Azure aboneliğinizin kimlik doğrulaması yapmak için not defterindeki yönergeleri izleyin.

Not defterinde çalıştıran tüm hücreleri bitirdikten sonra çalışma alanınızda günlüğe kaydedilen değerleri görüntüleyebilirsiniz.

## <a name="view-logged-values"></a>Günlüğe kaydedilen değerleri görüntüleme

1. Çıkışı `run` hücre geri çalışma alanınızda deneme sonuçlarını görüntülemek için Azure portalına bir bağlantı içerir. 

    ![Denemeleri görüntüleme](./media/quickstart-run-cloud-notebook/view-exp.png)

1. Tıklayın **Azure portalı bağlantısını** çalışma alanınızda çalıştırma hakkında bilgi görüntülemek için.  Bu bağlantı, Azure portalında çalışma alanınızı açar.

1. Çizimler gördüğünüz günlüğe kaydedilen değerler otomatik olarak çalışma alanında oluşturulur. Aynı ad parametresine sahip birden fazla değeri günlüğe kaydettiğinizde otomatik olarak bir çizim oluşturulur.

   ![Geçmişi görüntüleme](./media/quickstart-run-cloud-notebook/web-results.png)

Yaklaşık PI koda rastgele değerler kullandığından, çizimleri farklı değerler gösterilir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu korumakla birlikte tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntülemek ve seçmek **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Deneme ve model dağıtımı için gerekli kaynakları oluşturdunuz. Ayrıca bir defterde bulunan bazı kodları da çalıştırdınız. Buluttaki çalışma alanınızda bu koddan gelen çalıştırma geçmişini de incelediniz.

Ayrıntılı iş akışı deneyimi için eğitmek ve model dağıtma için Machine Learning öğreticileri izleyin:  

> [!div class="nextstepaction"]
> [Öğretici: Bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)