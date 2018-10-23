---
title: 'Hızlı başlangıç: Azure portalda Machine Learning hizmeti çalışma alanı oluşturma - Azure Machine Learning'
description: Azure Machine Learning çalışma alanı oluşturmak için Azure portalını kullanın. Bu çalışma alanı Azure Machine Learning ile bulutta makine öğrenmesi modellerini denemek, eğitmek ve dağıtmak için kullanabileceğiniz temel bileşenlerden biridir.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: rastala
ms.author: roastala
ms.date: 09/24/2018
ms.openlocfilehash: 14bd85a23e2630a1cf2a8b5621d669c4c6748168
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376627"
---
# <a name="quickstart-use-the-azure-portal-to-get-started-with-azure-machine-learning"></a>Hızlı başlangıç: Azure portalı kullanarak Azure Machine Learning'i kullanmaya başlama

Bu hızlı başlangıçta Azure Machine Learning çalışma alanı oluşturmak için Azure portalını kullanacaksınız. Bu çalışma alanı Machine Learning ile bulutta makine öğrenmesi modellerini denemek, eğitmek ve dağıtmak için kullanabileceğiniz temel bileşenlerden biridir. 

Bu öğreticide şunları yaptınız:

* Azure aboneliğinizde çalışma alanı oluşturma.
* Bir Azure not defterinde Python ile deneme ve birden çok yinelemeden değerleri günlüğe kaydetme.
* Günlüğe kaydedilen değerleri çalışma alanınızda görüntüleme.

Aşağıdaki Azure kaynakları, bölgesel kullanıma sunulduğunda çalışma alanınıza otomatik olarak eklenir:

  - [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
  - [Azure Depolama](https://azure.microsoft.com/services/storage/)
  - [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
  - [Azure Anahtar Kasası.](https://azure.microsoft.com/services/key-vault/)

Oluşturduğunuz kaynaklar, diğer Machine Learning hizmeti öğreticileri ve nasıl yapılır makalelerinde önkoşul olarak kullanılabilir. Diğer Azure hizmetlerinde de olduğu gibi, Machine Learning hizmetiyle ilişkilendirilmiş bazı kaynakların sınırları vardır. Örnek olarak Azure Batch AI kümesi boyutu verilebilir. Varsayılan sınırlar ve kotanızı artırma hakkında bilgi için [bu makaleyi](how-to-manage-quotas.md) inceleyin.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


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

Siz oturum açtıktan sonra, yeni bir sekme açılır ve `Clone Library` istemi görüntülenir. `Clone` seçeneğini belirleyin


### <a name="run-the-notebook"></a>Not defterini çalıştırma

İki not defterinin yanı sıra bir de `config.json` dosyası görürsünüz. Bu yapılandırma dosyası, oluşturduğunuz çalışma alanıyla ilgili bilgileri içerir.  

Not defterini açmak için `01.run-experiment.ipynb` öğesini seçin.

Hücreleri teker teker çalıştırmak için `Shift`+`Enter` kısayolunu kullanın. Dilerseniz `Cells` > `Run All` menüsünü kullanarak not defterinin tamamını da çalıştırabilirsiniz. Yıldız [*], ilgili hücrenin çalıştığını gösterir. Hücredeki kodun çalışması tamamlandığında bir sayı görünür.

Oturum açmanız istenebilir. İletideki kodu kopyalayın. Ardından bağlantıyı seçin ve kodu yeni pencereye yapıştırın. Kodun başında veya sonunda boşluk kopyalamamaya dikkat edin. Azure portalda kullandığınız hesapla oturum açın.

 ![Oturum açma](./media/quickstart-get-started/login.png)

Not defterinde, ikinci hücre çalışma alanınıza bağlanmak için `config.json` dosyasından alınır.
```
ws = Workspace.from_config()
```

Üçüncü kod hücresi "my-first-experiment" adlı bir deneme başlatır. Çalışma alanınıza dönüp çalıştırma hakkındaki bilgileri aramak için bu adı kullanın.

```
experiment = Experiment(workspace_object=ws, name = "my-first-experiment")
```

Not defterinin son hücresindeki değerlerin bir günlük dosyasına yazıldığına dikkat edin.

```
# Log final results
run.log("Final estimate: ",pi_estimate)
run.log("Final error: ",math.pi-pi_estimate)
```

Kod çalıştırıldıktan sonra bu değerleri çalışma alanınızda görüntüleyebilirsiniz.

## <a name="view-logged-values"></a>Günlüğe kaydedilen değerleri görüntüleme

Not defterindeki tüm hücreleri çalıştırdıktan sonra portal sayfasına dönün.  

`View Experiments` öğesini seçin.

![Denemeleri görüntüleme](./media/quickstart-get-started/view_exp.png)

`Reports` açılan listesini kapatın.

`my-first-experiment` öğesini seçin.

Az önce gerçekleştirdiğiniz çalışma hakkındaki bilgilere bakın. Sayfayı kaydırarak çalıştırma tablosunu bulun. Çalıştırma sayısı bağlantısını seçin.

 ![Çalıştırma geçmişi bağlantısı](./media/quickstart-get-started/report.png)

Günlüğe kaydedilen verilerden otomatik olarak oluşturulan çizimlere bakın.  

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
