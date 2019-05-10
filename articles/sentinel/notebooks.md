---
title: Azure önizlemesindeki Gözcü not defterlerini kullanarak avcılık özellikleri | Microsoft Docs
description: Bu makalede, Azure Gözcü avcılık özellikleriyle not defterlerini kullanmayı açıklar.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 1721d0da-c91e-4c96-82de-5c7458df566b
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: ffe3ae5b6aa26d154928a74e51864a0574b82c68
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65228625"
---
# <a name="use-jupyter-notebooks-to-hunt-for-security-threats"></a>Güvenlik tehditleri için hunt için Jupyter not defterleri kullanma

> [!IMPORTANT]
> Azure Sentinel şu anda genel Önizleme aşamasındadır.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Gözcü temelini veri deposudur; Bu, sorgulama, dinamik şema ve ölçek çok büyük veri hacimleri için yüksek performanslı birleştirir. Bu veri deposuna erişmek için ortak bir API Gözcü Azure portalı ve tüm Gözcü Azure Araçları'nı kullanın. Aynı API da gibi dış araçlar için kullanılabilir [Jupyter](https://jupyter.org/) Not defterlerinin ve Python. Portalda birçok ortak görevleri gerçekleştirilebilme olsa da Jupyter bu verilerle yapabilecekleriniz kapsamını genişletir. Tam programlama kitaplıkları makine öğrenimi, Görselleştirme ve veri analizi için büyük bir koleksiyonu ile birleştirir. Bu öznitelikler, ilgi çekici bir aracı için güvenlik araştırması ve avcılık Jupyter olun.

![Örneğin not defteri](./media/notebooks/sentinel-nb-mapandtimeline.png)

Oluşturma ve verilerinizi analiz etmek için not defterlerini yürütme kolaylaştırır ki Gözcü Azure portalında oturum Jupyter deneyimi entegre ettik. *Kqlmagic* kitaplığı, Azure Gözcü sorguları alabilir ve doğrudan bir not defteri içinde çalıştırmasına olanak sağlayan bir tutkal gibidir sağlar. Sorgular kullanım [Kusto sorgu dili](https://kusto.azurewebsites.net/docs/query/index.html). Bazı dizüstü bilgisayarlar, Microsoft'un güvenlik analistleri, bazıları tarafından geliştirilen Azure Gözcü ile paketlenmiştir. Bu not defterlerini bazıları belirli bir senaryo oluşturulur ve olarak kullanılabilir-olduğu. Örnek olarak diğer teknikleri ve kopyalama veya kullanılmak üzere kendi not defterlerinde uyum özellikleri göstermek için tasarlanmıştır. Diğer dizüstü bilgisayarlar, GitHub Azure Gözcü topluluğundan da alınabilir.

Tümleşik Jupyter deneyimi kullanır [Azure not defterleri](https://notebooks.azure.com/) depolamak için paylaşma ve Not Defterleri yürütün. Yerel olarak (bir Python ortamını ve Jupyter bilgisayarınızda varsa) da bu not defterlerini çalıştırabilirsiniz ya da Azure Databricks gibi diğer JupterHub ortamlarda.

Not defterlerini iki bileşeni vardır:

- Burada girin ve çalıştırma sorgular ve kodu, tarayıcı tabanlı arabirim ve Yürütme sonuçlarını görüntüleyen.
- bir *çekirdek* ayrıştırma ve kod yürütme için sorumlu. 

Azure not defterlerinde, bu çekirdek Azure üzerinde çalışır. *ücretsiz bulut bilgi işlem ve depolama* varsayılan olarak. Not defterlerinizi karmaşık makine öğrenimi modelleri dahil etmek veya görselleştirmeler düşünün kullanarak, daha güçlü, özel kaynaklar gibi işlem [veri bilimi sanal makineleri](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) (DSVM). Bunları paylaşmayı seçmediğiniz sürece hesabınızda not defterlerinde özel tutulur.

Gözcü Azure not defterleri pandas, matplotlib, bokeh ve diğerleri gibi birçok popüler Python kitaplıkları kullanın. Diğer Python paketleri içinden seçim yapabileceğiniz gibi alanları kapsayan çok sayıda vardır:

- görselleştirmeler ve grafikler
- veri işleme ve analiz
- istatistik ve sayısal hesaplama
- Makine öğrenimi ve derin öğrenme

Adlı bir pakette bazı açık kaynaklı Jupyter güvenlik araçları da kullanıma sunduk [msticpy](https://github.com/Microsoft/msticpy/). Bu paket, dahil edilen not defterlerini birçoğu kullanılır. Msticpy araçları, özellikle avcılık not defterleri oluşturma konusunda yardımcı olmak için tasarlanmıştır ve araştırma ve etkin bir şekilde yeni özellikler ve geliştirmeler üzerinde çalışıyoruz.

İlk not defterlerini içerir:

- **Araştırma - işlem uyarıları destekli**: Etkilenen konaklarında etkinliğini analiz ederek uyarıları kolayca önceliklendirmek sağlar.
- **AVA - Windows konak Gezgini destekli**: Bir konakta hesap etkinliği, işlem yürütme sayısı, ağ etkinliği ve diğer olayları keşfetmek sağlar.  
- **AVA - Office365 keşfetmeye destekli**: O365 için birden fazla veri kümesinde şüpheli Office 365 etkinlik hunt.

[Azure Gözcü topluluk GitHub deposu](https://github.com/Azure/Azure-Sentinel) konumun gelecekteki tüm Gözcü Azure not defterleri için Microsoft tarafından oluşturulan veya topluluk tarafından katkıda bulunulan.

## <a name="run-a-notebook"></a>Bir not defteri çalıştırma

Aşağıdaki örnekte, bir Azure not defterleri proje Gözcü Azure portalından proje not defterleri ile doldurma oluştururuz. Bu not defterlerini kullanmadan önce not defterini bir kopyasını alın ve kopya çalışmak için iyi bir fikirdir. Çalışma kopyaları verilerinizi yazmadan not defterlerini gelecek sürümleri için güvenli bir şekilde güncelleştirmenize olanak tanır.

1. Gözcü Azure portalında **not defterlerini** Gezinti menüsünde. Yeni bir Azure not defterleri projesi oluşturmak için tıklayın **kopya Azure Gözcü not defterlerini** veya mevcut not defterlerinizi açmak için tıklayın projeleri **defterlerinizi için Git**.
  
   ![not defterlerini seçin](./media/notebooks/sentinel-az-notebooks-home.png)

2. Seçerseniz, **kopya Azure not defterleri Gözcü** önceki adımda, aşağıdaki iletişim kutusu görünür. Tıklayın **alma** için GitHub deposunu Azure not defterleri projenize kopyalayın. Azure not defterleri hesabınız yoksa, oluşturun ve oturum açmanız istenir.

   ![İçeri aktarma not defteri](./media/notebooks/sentinel-nb-signin-and-clone.png)

3. Yeni bir proje oluştururken, projeyi adlandırın - yeni bir varsayılan adı veya türü kullanmak gerekir. Denetleme **kopya yinelemeli olarak** seçeneği - bu seçenek, bağlantılı GitHub depoları için ifade eder. Tıklayarak **alma** tamamlanması birkaç dakika sürebilir GitHub içerik kopyalama başlar.

   ![İçeri aktarma not defteri](./media/notebooks/sentinel-create-nb-project.png)

4. Açık **not defterlerini** not defterlerini görmek için klasör. Her bir not defteri hunt veya araştırma out taşınma adımlarında size kılavuzluk eder. Dizüstü veya basit yapılandırma yordamı aracılığıyla kitaplıkları ve dizüstü gereken diğer bağımlılıklar yüklenebilir. Not Defteri projenize geri Gözcü Azure aboneliğinizi bölümlere yapılandırma, önceki adımlarda otomatik olarak sağlanır. Not defterlerinizi Gözcü Azure Log Analytics çalışma alanınızı karşı çalıştırmak hazır olursunuz.

   ![Depoyu içeri aktarma](./media/notebooks/sentinel-open-notebook1.png)

5. Not defterini açın. Ücretsiz işlem (vurgulu) not defterlerini çalıştırmak için varsayılan olarak seçilidir. Bir DSVM (yukarıya bakın) kullanmak için yapılandırdıysanız, DSVM seçin ve ilk not defterini açmadan önce kimlik doğrulaması. Bir not açmak için tıklayın.

   ![Not defterini seçin](./media/notebooks/sentinel-open-notebook2.png)

6. Python sürümü seçin. Not defterini ilk kez açtığınızda, bir çekirdek sürümü seçmek için isteyebilir. Aksi durumda, çekirdek kullanacak şekilde seçin. Python 3.6 veya sonraki bir sürümü olmalıdır seçilen çekirdek (üst not defteri penceresinin sağ).

   ![Not defterini seçin](./media/notebooks/sentinel-select-kernel.png)

Azure Gözcü verilerde sorgulama için hızlı bir giriş için bakmak [GetStarted](https://github.com/Azure/Azure-Sentinel/blob/master/Notebooks/Get%20Started.ipynb) ana not defterlerini klasöründe dizüstü bilgisayar. Ek Örnek Not Defterleri bulunabilir **örnek not defterleri** alt. Hedeflenen çıktıyı görmek daha kolaydır, böylece örnek not defterleri ile verileri, kaydedildi (bunları görüntüleme öneririz [nbviewer](https://nbviewer.jupyter.org/)). **HowTos** klasörü, açıklayan, örneğin not defterlerini içerir: Python sürümünü varsayılan ayar, bir DSVM yapılandırma, Azure Gözcü oluşturma yer işaretlerini bir not defteri ve diğer konular.

Bu not defterlerini çizimler ve kendi not defterlerinizi geliştirmede kullanabileceğiniz kod örnekleri ve her iki faydalı bir araç olarak tasarlanmıştır.

Not defterleri, hata raporlarını veya geliştirmeleri ve mevcut not defterlerini eklemeler öneriler, özellikleri, istekleri katkıda olup olmadığını, geri bildirim, bizim için çok önemli. Git [Azure Gözcü topluluk GitHub](https://github.com/Azure/Azure-Sentinel) bir sorun veya çatal oluşturma ve bir katkı karşıya yükleyin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Gözcü Jupyter not defterleri ile çalışmaya başlamak öğrendiniz. Azure Gözcü hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Proaktif olarak tehditleri hunt](hunting.md)
- [Aramaya çalışırken ilginç bilgileri kaydetmek için yer işaretlerini kullanma](bookmarks.md)