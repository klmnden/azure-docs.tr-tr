---
title: Azure portalı üzerinden hızlı başlangıç
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning hizmeti ile çalışmaya başlama. Deneme, eğitmek ve makine öğrenimi modelleri dağıtmak için kullandığınız bulut ıaas'yi bloğunda bir çalışma alanı oluşturmak için Azure portalını kullanın.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 14c500d77cc0e67aaade5e6be490f599f39bfad5
ms.sourcegitcommit: 9f87a992c77bf8e3927486f8d7d1ca46aa13e849
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/28/2018
ms.locfileid: "53807729"
---
# <a name="quickstart-use-the-azure-portal-to-get-started-with-azure-machine-learning"></a>Hızlı Başlangıç: Azure Machine Learning'i kullanmaya başlamak için Azure portalını kullanma

Bu hızlı başlangıçta Azure Machine Learning çalışma alanı oluşturmak için Azure portalını kullanacaksınız. Bu çalışma alanı Machine Learning ile bulutta makine öğrenmesi modellerini denemek, eğitmek ve dağıtmak için kullanabileceğiniz temel bileşenlerden biridir. Bu hızlı başlangıçta bulut kaynakları kullanılmaktadır ve bu nedenle herhangi bir yükleme yapmanıza gerek yoktur. Bunun yerine kendi Jupyter Notebook sunucusu yapılandırmak için bkz [hızlı başlangıç: Azure Machine Learning'i kullanmaya başlamak için Python kullanma](quickstart-create-workspace-with-python.md).  
 
> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2F9Ad]

Bu hızlı başlangıçta, aşağıdaki eylemleri gerçekleştirin:

* Azure aboneliğinizde çalışma alanı oluşturma.
* Python ile Azure bir not defteri ve günlük değerleri arasında birden çok yineleme deneyin.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Aşağıdaki Azure kaynakları, bölgesel kullanıma sunulduğunda çalışma alanınıza otomatik olarak eklenir:

  - [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
  - [Azure Depolama](https://azure.microsoft.com/services/storage/)
  - [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
  - [Azure Anahtar Kasası.](https://azure.microsoft.com/services/key-vault/)

Oluşturduğunuz kaynaklar, diğer Machine Learning hizmeti öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir. Diğer Azure hizmetlerinde de olduğu gibi, Machine Learning hizmetiyle ilişkilendirilmiş bazı kaynakların sınırları vardır. İşlem kümesi boyutu buna bir örnektir. Daha fazla bilgi edinin [varsayılan limitler ve kotanızı artırmak nasıl](how-to-manage-quotas.md).

Azure aboneliğiniz yoksa başlamadan önce ücretsiz bir hesap oluşturun. Deneyin [Azure Machine Learning hizmetinin ücretsiz veya Ücretli sürümüne](http://aka.ms/AMLFree) bugün.


## <a name="create-a-workspace"></a>Çalışma alanı oluşturma 

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Çalışma alanı sayfasında `Explore your Azure Machine Learning service Workspace` öğesini seçin.

 ![Çalışma alanını keşfedin](./media/quickstart-get-started/explore_aml.png)


## <a name="use-the-workspace"></a>Çalışma alanını kullanma

Şimdi çalışma alanının makine öğrenmesi betiklerinizin yönetimine nasıl yardımcı olduğunu göreceksiniz. Bu bölümde, aşağıdaki adımları uygulayın:

* Azure Notebooks'da bir not defteri açma.
* Günlüğe kaydedilen bazı değerler oluşturan kodu çalıştırma.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Bu örnek, çalışma alanının betikte oluşturulan bilgileri izlemenize nasıl yardımcı olduğunu gösterir. 

### <a name="open-a-notebook"></a>Not defterini açma 

Azure not defterleri, Machine Learning çalıştırmak için gereken her şeyi ile önceden yapılandırılmış Jupyter not defterleri için ücretsiz bulut platformu sağlar.  

İlk denemenizi oluşturmak için `Open Azure Notebooks` öğesini seçin.

 ![Azure Notebooks'u açın](./media/quickstart-get-started/explore_ws.png)

Oturum açabilmeniz için kuruluşunuzda [yönetici onayı](https://notebooks.azure.com/help/signing-up/work-or-school-account/admin-consent) gerekli olabilir.

Azure not defterleri Azure portalında oturum açmak için kullanılan hesap ile oturum açın.  Siz oturum açtıktan sonra, yeni bir sekme açılır ve `Clone Library` istemi görüntülenir. `Clone` öğesini seçin.


### <a name="run-the-notebook"></a>Not defterini çalıştırma

İki not defterinin yanı sıra bir de `config.json` dosyası görürsünüz. Bu yapılandırma dosyası, oluşturduğunuz çalışma alanıyla ilgili bilgileri içerir.  

Not defterini açmak için `01.run-experiment.ipynb` öğesini seçin.

Hücreleri birer birer (Shift + Enter) çalıştırın. Dilerseniz `Cells` > `Run All` menüsünü kullanarak not defterinin tamamını da çalıştırabilirsiniz. Bir yıldız işareti gördüğünüzde __*__, hücrede yanında çalıştığından. Hücredeki kodun çalışması tamamlandığında bir sayı görünür. 

Not defterinde çalıştıran tüm hücreleri bitirdikten sonra çalışma alanınızda günlüğe kaydedilen değerleri görüntüleyebilirsiniz.

## <a name="view-logged-values"></a>Günlüğe kaydedilen değerleri görüntüleme

Not defterindeki tüm hücreleri çalıştırdıktan sonra portal sayfasına dönün.  

`View Experiments` öğesini seçin.

![Denemeleri görüntüleme](./media/quickstart-get-started/view_exp.png)

`Reports` açılan listesini kapatın.

`my-first-experiment` öğesini seçin.

Çalıştırma hakkında bilgi yalnızca yaptığınız bakın. Sayfayı kaydırarak çalıştırma tablosunu bulun. Çalıştırma sayısı bağlantısını seçin.

 ![Çalıştırma geçmişi bağlantısı](./media/quickstart-get-started/report.png)

Günlüğe kaydedilen verilerden otomatik olarak oluşturulan çizimlere bakın. Aynı ad parametresine sahip birden fazla değeri günlüğe kaydettiğinizde otomatik olarak bir çizim oluşturulur.

   ![Geçmişi görüntüleme](./media/quickstart-get-started/plots.png)

Yaklaşık PI koda rastgele değerler kullandığından, çizimleri farklı değerler gösterilir.  

## <a name="clean-up-resources"></a>Kaynakları temizleme 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

Ayrıca, kaynak grubunu korumakla birlikte tek bir çalışma alanını silebilirsiniz. Çalışma alanı özelliklerini görüntülemek ve seçmek **Sil**.

## <a name="next-steps"></a>Sonraki adımlar

Deneme ve model dağıtımı için gerekli kaynakları oluşturdunuz. Ayrıca bir defterde bulunan bazı kodları da çalıştırdınız. Buluttaki çalışma alanınızda bu koddan gelen çalıştırma geçmişini de incelediniz.

Ayrıntılı iş akışı deneyimi için eğitmek ve model dağıtma için Machine Learning öğreticileri izleyin:  

> [!div class="nextstepaction"]
> [Öğretici: Bir görüntü sınıflandırma modeli eğitme](tutorial-train-models-with-aml.md)
