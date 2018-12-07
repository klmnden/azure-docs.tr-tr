---
title: 'Hızlı Başlangıç: Kullanan Azure portal ile başlaması Azure Machine Learning hizmeti'
description: Azure Machine Learning hizmeti ile çalışmaya başlama. Deneme, eğitmek ve makine öğrenimi modelleri dağıtmak için kullandığınız bulut ıaas'yi bloğunda bir çalışma alanı oluşturmak için Azure portalını kullanın.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 12/04/2018
ms.custom: seodec12
ms.openlocfilehash: 3b874abc3896b9e6520500370d09e685d49ff82c
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/07/2018
ms.locfileid: "53015714"
---
# <a name="quickstart-use-the-azure-portal-to-get-started-with-azure-machine-learning"></a>Hızlı başlangıç: Azure portalı kullanarak Azure Machine Learning'i kullanmaya başlama

Bu hızlı başlangıçta Azure Machine Learning çalışma alanı oluşturmak için Azure portalını kullanacaksınız. Bu çalışma alanı Machine Learning ile bulutta makine öğrenmesi modellerini denemek, eğitmek ve dağıtmak için kullanabileceğiniz temel bileşenlerden biridir. Bu hızlı başlangıçta bulut kaynakları kullanılmaktadır ve bu nedenle herhangi bir yükleme yapmanıza gerek yoktur. Kendi Jupyter notebook sunucunuzu yapılandırmak istiyorsanız [Hızlı başlangıç: Machine Learning hizmetini kullanmaya başlamak için Python'ı kullanma](quickstart-create-workspace-with-python.md) sayfasına bakın.

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]

Bu hızlı başlangıçta:

* Azure aboneliğinizde çalışma alanı oluşturma.
* Bir Azure not defterinde Python ile deneme ve birden çok yinelemeden değerleri günlüğe kaydetme.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Aşağıdaki Azure kaynakları, bölgesel kullanıma sunulduğunda çalışma alanınıza otomatik olarak eklenir:

  - [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
  - [Azure Depolama](https://azure.microsoft.com/services/storage/)
  - [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
  - [Azure Anahtar Kasası.](https://azure.microsoft.com/services/key-vault/)

Oluşturduğunuz kaynaklar, diğer Machine Learning hizmeti öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir. Diğer Azure hizmetleriyle şekilde küme boyutu gibi Machine Learning ile ilişkili belirli kaynaklar barındırabileceğiniz işlem. Daha fazla bilgi edinin [varsayılan limitler ve kotanızı artırmak nasıl](how-to-manage-quotas.md).

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLfree) oluşturun.


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Çalışma alanı sayfasında `Explore your Azure Machine Learning service workspace` öğesini seçin.

 ![Çalışma alanını keşfetme](./media/quickstart-get-started/explore_aml.png)


## <a name="use-the-workspace"></a>Çalışma alanını kullanma

Şimdi çalışma alanının makine öğrenmesi betiklerinizin yönetimine nasıl yardımcı olduğunu göreceksiniz. Bu bölümde şunları yapacaksınız:

* Azure Notebooks'da bir not defteri açma.
* Günlüğe kaydedilen bazı değerler oluşturan kodu çalıştırma.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Bu örnek, çalışma alanının betikte oluşturulan bilgileri izlemenize nasıl yardımcı olduğunu gösterir. 

### <a name="open-a-notebook"></a>Not defterini açma 

Azure Notebooks, Machine Learning'i çalıştırmak için gereken her şeyle önceden yapılandırılmıştır ve Jupyter not defterleri için ücretsiz bir bulut platformu sağlar.  

İlk denemenizi oluşturmak için `Open Azure Notebooks` öğesini seçin.

 ![Azure Notebooks'u açın](./media/quickstart-get-started/explore_ws.png)

Oturum açabilmeniz için kuruluşunuzda [yönetici onayı](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) gerekli olabilir.

Siz oturum açtıktan sonra, yeni bir sekme açılır ve `Clone Library` istemi görüntülenir. `Clone` öğesini seçin.


### <a name="run-the-notebook"></a>Not defterini çalıştırma

İki not defterinin yanı sıra bir de `config.json` dosyası görürsünüz. Bu yapılandırma dosyası, oluşturduğunuz çalışma alanıyla ilgili bilgileri içerir.  

Not defterini açmak için `01.run-experiment.ipynb` öğesini seçin.

Hücreleri teker teker çalıştırmak için `Shift`+`Enter` kısayolunu kullanın. Dilerseniz `Cells` > `Run All` menüsünü kullanarak not defterinin tamamını da çalıştırabilirsiniz. Yıldız [*], ilgili hücrenin çalıştığını gösterir. Hücredeki kodun çalışması tamamlandığında bir sayı görünür. 

Not defterindeki tüm hücreleri çalıştırmayı tamamladıktan sonra günlüğe alınan değerleri çalışma alanınızda görüntüleyebilirsiniz.

## <a name="view-logged-values"></a>Günlüğe kaydedilen değerleri görüntüleme

Not defterindeki tüm hücreleri çalıştırdıktan sonra portal sayfasına dönün.  

`View Experiments` öğesini seçin.

![Denemeleri görüntüleme](./media/quickstart-get-started/view_exp.png)

`Reports` açılan listesini kapatın.

`my-first-experiment` öğesini seçin.

Az önce gerçekleştirdiğiniz çalışma hakkındaki bilgilere bakın. Sayfayı kaydırarak çalıştırma tablosunu bulun. Çalıştırma sayısı bağlantısını seçin.

 ![Çalıştırma geçmişi bağlantısı](./media/quickstart-get-started/report.png)

Günlüğe kaydedilen verilerden otomatik olarak oluşturulan çizimlere bakın. Aynı ad parametresine sahip birden fazla değeri günlüğe kaydettiğinizde otomatik olarak bir çizim oluşturulur.

   ![Geçmişi görüntüleme](./media/quickstart-get-started/plots.png)

Yaklaşık pi değerini belirleme kodu rastgele değerler kullandığından çizimlerinizde farklı değerler görünecektir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Dilerseniz kaynak grubunu koruyabilir ancak tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntüleyin ve **Sil**'i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Deneme ve model dağıtımı için gerekli kaynakları oluşturdunuz. Ayrıca bir defterde bulunan bazı kodları da çalıştırdınız. Buluttaki çalışma alanınızda bu koddan gelen çalıştırma geçmişini de incelediniz.

Ayrıntılı bir iş akışı deneyimi için, Machine Learning öğreticilerini izleyerek bir modeli eğitin ve dağıtın.  

> [!div class="nextstepaction"]
> [Öğretici: Görüntü sınıflandırma modelini eğitme](tutorial-train-models-with-aml.md)
