---
title: Sınıflandırma Azure Machine Learning Workbench içinde CNTK kullanarak görüntü | Microsoft Docs
description: Eğitim, değerlendirin ve Azure ML Workbench kullanarak özel görüntü sınıflandırma model dağıtma.
services: machine-learning
documentationcenter: ''
author: PatrickBue
ms.author: pabuehle
manager: mwinkle
ms.reviewer: marhamil, mldocs, garyericson, jasonwhowell
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 10/17/2017
ms.openlocfilehash: 48c21638fe5756e6527288ed0fdc73dd9e331afd
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/04/2018
ms.locfileid: "35622220"
---
# <a name="image-classification-using-azure-machine-learning-workbench"></a>Azure Machine Learning Workbench'i kullanarak görüntü sınıflandırması

Görüntü sınıflandırma yaklaşım, çok sayıda görüntü işleme sorunları çözmek için kullanılabilir.
Bunlar hangi soruları cevaplamak modeller oluşturma: *görüntüde mevcut bir nesnenin?* burada nesne örneğin olabilir *köpek*, *araba*, veya  *Sevk*. Veya gibi daha karmaşık sorular: *hangi sınıfın göz Hastalık önem derecesi, bu hastanın retinal taramasıyla evinced?*.

Bu tür problemleri çözmek Bu öğretici adresleri. Kendi görüntü sınıflandırma modeli kullanarak dağıtma eğitme ve değerlendirme nasıl göstereceğiz [Microsoft Cognitive Toolkit (CNTK) ](https://docs.microsoft.com/cognitive-toolkit/) derin öğrenme için.
Sağlanan örnek görüntüler, ancak Okuyucu, ayrıca kendi veri kümesi getirin ve kendi özel modelleri eğitme.

Görüntü işleme çözümleri, geleneksel olarak el ile tanımlamak ve sözde uygulamak için uzman gerekli *özellikleri*, istenen bilgileri resimlerdeki vurgulayın.
El ile bu yaklaşımı değiştirilen ünlü ile 2012 [AlexNet](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf) [1] derin öğrenme kağıt ve şu anda, derin sinir ağları (DNN) otomatik olarak bu özellikleri bulmak için kullanılır.
Yalnızca görüntü sınıflandırması, ancak ayrıca nesne algılama ve görüntü benzerlik gibi diğer görüntü işleme sorunlarını alanında büyük bir geliştirme için Dnn'leri gerektiriyordu.


## <a name="link-to-the-gallery-github-repository"></a>Galeri GitHub deponuza bağlayın
[https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK](https://github.com/Azure/MachineLearningSamples-ImageClassificationUsingCNTK)

## <a name="overview"></a>Genel Bakış

Bu öğretici, üç bölüme ayrılır:

- 1. Bölüm eğitmek, değerlendirin ve önceden eğitilmiş bir DNN özelliği Oluşturucu kullanarak ve eğitim bir SVM üzerinde çıkışını bir görüntü sınıflandırma sistemi dağıtma işlemi gösterilmektedir.
- 2. kısım daha sonra doğruluğunu artırmak Örneğin, sabit bir özelliği Oluşturucu kullanmak yerine DNN iyileştirme gösterilmektedir.
- Bölüm 3 sağlanan örnek görüntüleri yerine kendi veri kümesini kullanmak nasıl etkinleştireceğinizi de açıklar ve gerekirse, ağ değiştirilene kendi veri kümesi oluşturmak nasıl görüntüler.

Machine learning ve CNTK önceki deneyimiyle gerekli olmamasına karşın, temel ilkelerini anlaşılması için yararlıdır. Eğitim zaman, öğreticide bildirilen vb. doğruluğu, yalnızca başvuru sayılardır ve kodu çalışırken gerçek değerler neredeyse kesindir farklılık gösterir.


## <a name="prerequisites"></a>Önkoşullar

Bu örneği çalıştırmak için gereken önkoşullar aşağıdaki gibidir:

1. Bir [Azure hesabı](https://azure.microsoft.com/free/) (ücretsiz denemeler kullanılabilir).
2. [Azure Machine Learning Workbench](../service/overview-what-is-azure-ml.md) aşağıdaki [hızlı başlangıç Yükleme Kılavuzu](../service/quickstart-installation.md) programı yüklemek ve bir çalışma alanı oluşturun.  
3. Bir Windows makine. Windows işletim sistemi, Workbench yalnızca Windows ve MacOS, Microsoft Bilişsel Araç Seti sırasında desteklediğinden gereklidir (ayrıntılı öğrenme kitaplık olarak kullandığımız) yalnızca destekleyen Windows ve Linux.
4. 2. bölümünde açıklanan DNN iyileştirme için gereklidir ancak adanmış bir GPU SVM eğitim bölüm 1, yürütmek için gerekli değildir. Ardından, güçlü GPU alınamadığından, birden fazla GPU üzerinde eğitmek istediğiniz veya bir Windows makine yok, Windows işletim sistemi ile Azure'nın ayrıntılı öğrenme sanal makinesi kullanmayı düşünün. Bkz: [burada](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning) 1-tıklatma dağıtım kılavuzu. Dağıtıldıktan sonra bir Uzak Masaüstü bağlantısı aracılığıyla sanal makineye bağlanmak, var. Workbench'i yükleme ve kod VM'den yerel olarak yürütün.
5. OpenCV gibi çeşitli Python kitaplıkları yüklü olması gerekir. Tıklayın *komut istemini Aç* gelen *dosya* menüde Workbench ve bu bağımlılıkları yüklemek için aşağıdaki komutları çalıştırın:  
    - `pip install https://cntk.ai/PythonWheel/GPU/cntk-2.2-cp35-cp35m-win_amd64.whl`  
    - `pip install opencv_python-3.3.1-cp35-cp35m-win_amd64.whl` OpenCV indirdikten sonra gelen Tekerlek http://www.lfd.uci.edu/~gohlke/pythonlibs/ (tam dosya adı ve sürümü değiştirebilirsiniz)
    - `conda install pillow`
    - `pip install -U numpy`
    - `pip install bqplot`
    - `jupyter nbextension enable --py --sys-prefix bqplot`
    - `jupyter nbextension enable --py widgetsnbextension`

### <a name="troubleshooting--known-bugs"></a>Sorun giderme / bilinen hatalar
- 2. bölüm için bir GPU gereklidir ve aksi takdirde hata "toplu normalleştirme eğitim CPU üzerinde henüz uygulanmadı" DNN iyileştirmek çalışırken atılır.
- DNN eğitim sırasında bellek yetersiz hatalar önlenmiş minibatch boyutunu azaltarak (değişken `cntk_mb_size` içinde `PARAMETERS.py`).
- CNTK 2.2 kullanan kodu test edilmiştir ve olmayan ya da yalnızca küçük eski (up) v2.0 için üzerinde de çalışır ve daha yeni sürümleri değiştirir.
- Makalenin yazıldığı sırada, Azure Machine Learning Workbench, not defterlerini 5 MB değerinden daha büyük gösteren sorunlarla karşılaştı. Bu büyük boyuttaki not defterlerini oluşabilir not defteri ile kaydedilirse, tüm çıktı görüntülenen hücre. Bu hatayla karşılaşırsanız, ardından Workbench içinde Dosya menüsünden komut istemini açın, yürütme `jupyter notebook`, Not defterini açın, clear tüm çıktı ve not defteri kaydedin. Bu adımları gerçekleştirdikten sonra not defterini düzgün bir şekilde Azure Machine Learning Workbench içinde yeniden açılır.
- Bu örnekte sağlanan tüm betikleri yerel olarak yürütülecek olan ve örneğin docker uzak ortamda üzerinde değil. Tüm Not defterlerinin yerel proje çekirdeğe "PROJECTNAME yerel" adıyla (örn: "myImgClassUsingCNTK yerel") kümesi çekirdeği ile yürütülmesi gerekir.

    
## <a name="create-a-new-workbench-project"></a>Workbench yeni bir proje oluşturun

Bu örnekte, şablon olarak kullanarak yeni bir proje oluşturmak için:
1.  Azure Machine Learning Workbench’i açın.
2.  Üzerinde **projeleri** sayfasında **+** açıp seçmek **yeni proje**.
3.  İçinde **yeni proje oluştur** bölmesinde, yeni projeniz için bilgileri doldurun.
4.  İçinde **proje şablonlarında Ara** arama kutusuna "görüntü sınıflandırma" yazın ve şablonu seçin.
5.  **Oluştur**’a tıklayın.

Bu adımları gerçekleştiren, aşağıda gösterilen Proje yapısı oluşturur. Proje dizinine, Azure Machine Learning Workbench (çalıştırma geçmişi etkinleştirmek için) her bir çalıştırmanın sonra bu klasör bir kopyasını oluşturduğundan, 25'ten az MBayt olacak şekilde sınırlıdır. Tüm görüntü ve geçici dosyalar dizini gelen ve bu nedenle, kaydedilen *~/Desktop/imgClassificationUsingCntk_data* (olarak adlandırılan *DATA_DIR* bu belgedeki).

  Klasör| Açıklama
  ---|---
  aml_config /|                           Azure Machine Learning Workbench yapılandırma dosyalarını içeren dizin
  kitaplıkları /|                              Tüm Python ve Jupyter yardımcı işlevleri içeren dizin
  dizüstü /|                              Tüm Not defterlerinin içeren dizin
  Kaynak /|                              Tüm kaynaklar (örneğin url şekilde görüntüleri için) içeren dizin
  betikleri /|                              Tüm betikleri içeren dizin
  PARAMETERS.py|                       Python betiğini tüm parametrelerini belirtme
  Benioku.MD|                           Bu Benioku Belgesi


## <a name="data-description"></a>Veri açıklaması

Bu öğreticide, örnek kadar 428 görüntülerini oluşan bir üst gövdesi giysi doku dataset çalışıyor olarak kullanılır. Her görüntü üç farklı dokular (noktalı, şeritli, leopard) biri olarak açıklanıyor. Biz, görüntülerinin sayısını küçük tutulur, böylece Bu öğretici hızlı yürütülebilir. Ancak, kod iyi test edilmiş ve on binlerce görüntüleri veya daha fazla ile çalışır. Bing resim arama kullanarak scraped ve elle-ek açıklama içinde anlatıldığı gibi tüm görüntüleri [3. Kısım](#using-a-custom-dataset). URL'ler ile bunların ilgili öznitelikler listelenir görüntü */resources/fashionTextureUrls.tsv* dosya.

Betik `0_downloadData.py` tüm görüntülere indirir *DATA_DIR/resimler/fashionTexture/* dizin. Büyük olasılıkla bozuk 428 URL'leri bazılarıdır. Bu bir sorun değildir ve sadece biz biraz daha az görüntüler, eğitim ve test için sahip oldukları anlamına gelir. Bu örnekte sağlanan tüm betikleri yerel olarak yürütülecek olan ve örneğin docker uzak ortamda üzerinde değil.

Aşağıdaki şekilde şeritli (Orta) ve (sağdaki) leopard (solda), noktalı öznitelikler için örnekler gösterilmektedir. Ek açıklamalar göre üst gövdesi giysi öğesi yapıldığını.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/examples_all.jpg"  alt="alt text" width="700">
</p>


## <a name="part-1---model-training-and-evaluation"></a>Bölüm 1 - Model eğitimi ve değerlendirmesi

Bu öğreticinin ilk bölümünde, biz kullanır, ancak değiştirmez, önceden eğitilmiş bir derin sinir ağı bir sistem eğitim. Bu önceden eğitilmiş DNN bir özelliği Oluşturucu kullanılır ve doğrusal SVM özniteliği tahmin etmek için eğitildi (noktalı, şeritli veya leopard) belirli bir görüntüsü.

Artık bu yaklaşım ayrıntılı, adım adım ve hangi betiklerin yürütülmesi gerekir show açıkladığımız. Hangi dosyaların yazılır ve için burada yazılan incelemek için her adımdan sonra öneririz.

Tüm önemli parametreleri belirtilir ve kısa bir açıklama sağlanan, tek bir yerde: `PARAMETERS.py` dosya.




### <a name="step-1-data-preparation"></a>1. adım: Verileri hazırlama
`Script: 1_prepareData.py. Notebook: showImages.ipynb`

Not defterini `showImages.ipynb` görüntüleri görselleştirin ve gerektiğinde, ek açıklama düzeltmek için kullanılabilir. Not Defteri çalıştırmak için bu seçeneği gösteriliyorsa, üzerinde "Başlat Notebook sunucusu" değiştirme "PROJECTNAME yerel" (örn: "myImgClassUsingCNTK yerel"), adla yerel proje çekirdeğe tıklatın Azure Machine Learning workbench'te açın ve sonra tüm hücreleri yürütün dizüstü bilgisayar. Not defterini görüntülenecek büyük olduğunu şikayetçi hata alırsanız, bu belgedeki sorun giderme bölümüne bakın.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showImages.jpg" alt="alt text" width="700"/>
</p>

Şimdi adlı betiğini yürütün `1_prepareData.py`, hangi atar tüm görüntüleri ya da eğitim veya test. Bu atama birbirini dışlayan - eğitim görüntü test etmek için ya da tam tersi de kullanılır. Varsayılan olarak, bir rastgele % 75'ini her öznitelik sınıfı görüntülerden eğitim için atanmış olan ve kalan % 25 oranında test atanır. Betiği tarafından oluşturulan tüm verileri kaydedilir *DATA_DIR/proc/fashionTexture/* klasör.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_1_white.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-2-refining-the-deep-neural-network"></a>2. adım: derin sinir ağı iyileştirme
`Script: 2_refineDNN.py`

Biz bu öğreticinin 1 bölümünde açıklandığı gibi önceden eğitilmiş DNN sabit tutulur (diğer bir deyişle, daraltılmış değildir). Bununla birlikte, adlandırılan bu betik `2_refineDNN.py` bölüm 1, önceden eğitilmiş yüklenirken hala yürütülür [ResNet](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf) [2] model ve, örneğin, daha yüksek girdi görüntüsünün çözümlemesi için izin verecek şekilde değiştirir. Bu adım (saniye) hızlı ve bir GPU gerektirmez.

Öğreticinin 2 PARAMETERS.py değişiklik dosya nedenleri `2_refineDNN.py` ayrıca önceden eğitilmiş DNN iyileştirmek için betik. Varsayılan olarak, geliştirme sırasında 45 eğitim dönemlerinde çalıştırıyoruz.

Her iki durumda da, son modelin ardından dosyasına yazılır *DATA_DIR/proc/fashionTexture/cntk_fixed.model*.

### <a name="step-3-evaluate-dnn-for-all-images"></a>3. adım: DNN tüm görüntüler için değerlendirme
`Script: 3_runDNN.py`

Son adım (büyük olasılıkla daraltılmış) DNN özellik kazandırın için artık görüntülerimizi kullanabilir. Görüntü DNN giriş olarak göz önünde bulundurulduğunda, çıkış sondan katmanı modelinin 512 float öğesinden alınır. Bu görüntünün kendisi çok daha küçük boyutlu vektördür. Bununla birlikte, içeren (ve gerekir bile vurgula) giysi öğenin noktalı, yoksa, Şerit, görüntünün özniteliği tanımak ilgili görüntü veya leopard doku tüm bilgiler.

Tüm DNN görüntü gösterimleri dosyasına kaydedilir *DATA_DIR/proc/fashionTexture/cntkFiles/features.pickle*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_4_white.jpg" alt="alt text" width="700"/>
</p>


### <a name="step-4-support-vector-machine-training"></a>4. adım: Destekli vektör makinesi eğitim
`Script: 4_trainSVM.py`

512-float'ların son adımda hesaplanan gösterimleri SVM sınıflandırıcı eğitmek için kullanılan artık: bir resim giriş olarak göz önünde bulundurulduğunda, SVM bulunması her bir öznitelik için bir puan verir. Örnek veri kümemize bu puan için 'şeritli', 'noktalı' ve 'leopard' için anlamına gelir.

Betik `4_trainSVM.py` SVM en yüksek doğruluğa sahip olmaya devam eğitim resmi yükler ve düzenleme (slack) parametresinin C farklı değerler için bir SVM eğitir. Sınıflandırma doğruluğu konsolda yazdırılır ve Workbench'te çizilen. Belirtilen doku verileri bu değerleri yaklaşık %100 ve % 88 sırasıyla olması gerekir. Son olarak, eğitilen SVM dosyasına yazılır *DATA_DIR/proc/fashionTexture/cntkFiles/svm.np*.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/vienna_svm_log_zoom.jpg" alt="alt text" width="700"/>
</p>



### <a name="step-5-evaluation-and-visualization"></a>5. adım: Değerlendirme ve görselleştirme
`Script: 5_evaluate.py. Notebook: showResults.ipynb`

Betik kullanarak eğitilen bir görüntü sınıflandırıcı doğruluğunu ölçülebilir `5_evaluate.py`. Betik puanları eğitilen SVM sınıflandırıcı kullanılarak, tüm test görüntülerinin en yüksek puanı ve karşılaştırır çığır truth ek açıklamalar tahmin edilen özniteliklerle özniteliğiyle atar her görüntü.

Betik çıktısı `5_evaluate.py` aşağıda gösterilmiştir. Her bir sınıfın sınıflandırma doğruluğu, tam bir sınama kümesi ('genel doğruluğu') ve ortalama kesinlik yanı sıra bireysel doğruluk ('genel sınıfı için ortalama kesinlik') hesaplanır. % 100 olası en yüksek doğruluğa ve en kötü %0 karşılık gelir. Rasgele tahmin ortalama üretmek 1 sınıfı ortalama doğruluğunu öznitelik sayısı: Bu örnekte bu kesinlik %33.33 olacaktır. Bu sonuçları önemli ölçüde daha yüksek bir giriş çözünürlüğü gibi kullanırken geliştirmek `rf_inputResoluton = 1000`, ancak daha uzun DNN hesaplama süreleri çoğaltamaz.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_6_white.jpg" alt="alt text" width="700"/>
</p>

Doğruluk ek olarak, ilgili alanı altında-eğrisi ile (solda); ROC eğrisi çizilir ve karışıklık matrisi (sağdaki) gösterilir:

<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat.jpg" alt="alt text" width="700"/>
</p>

Son olarak, Not defterini `showResults.py` test resimler arasında gezinmek ve ilgili sınıflandırma puanlarını görselleştirmenize olanak sağlanır. Adım 1 ' açıklandığı gibi bu örnekteki her bir dizüstü bilgisayarın yerel proje çekirdek "PROJECTNAME yerel" adıyla kullanması gerekir:
<p align="center">
<img src="media/scenario-image-classification-using-cntk/notebook_showResults.jpg" alt="alt text" width="700"/>
</p>





### <a name="step-6-deployment"></a>Adım 6: dağıtım
`Scripts: 6_callWebservice.py, deploymain.py. Notebook: deploy.ipynb`

Eğitilen sistem artık bir REST API olarak yayımlanabilir. Dağıtım not defterinde açıklanan `deploy.ipynb`, Azure Machine Learning Workbench işlevinin temel ("PROJECTNAME yerel" adlı yerel proje çekirdek çekirdek ayarlamayı unutmayın). Ayrıca mükemmel dağıtımı bölümüne bakın [IRİS öğreticisini](tutorial-classifying-iris-part-3.md) daha fazla dağıtım için ilgili bilgiler.

Dağıtıldıktan sonra web hizmeti betiği kullanılarak çağrılabilir `6_callWebservice.py`. Web hizmetinin IP adresini (yerel veya bulutta) betik ilk olarak gerektiğini unutmayın. Not defterini `deploy.ipynb` bu IP adresini bulmak açıklanmaktadır.








## <a name="part-2---accuracy-improvements"></a>Bölüm 2 - doğruluğu geliştirmeleri

Bölüm 1'de, biz bir doğrusal destekli vektör makinesi üzerinde derin sinir ağı 512 float çıktısı eğitim ile görüntü sınıflandırma nasıl oluşturulacağını gösterir. Bu DNN görüntüleri milyonlarca üzerinde önceden eğitilen ve sondan katmanı özellik vektör döndürdü. DNN olarak kullanıldığından, bu yaklaşım hızlı-bağlıdır, ancak yine de genellikle iyi sonuçlar verir.

Biz artık 1. Bölüm modelden doğruluğunu artırmak için çeşitli yollar sunar. En önemlisi, sabit tutmak yerine DNN daraltın.

### <a name="dnn-refinement"></a>DNN iyileştirme

Bir SVM yerine bir sınıflandırma doğrudan sinir ağı yapabilirsiniz. Bu işlem, giriş olarak sondan katmandan 512-float'ların alan önceden eğitilmiş DNN yeni bir son katmanı ekleyerek gerçekleştirilir. Tam ağ yayılımı kullanarak eğitilebileceği de artık DNN sınıflandırma yapmanın avantajı olmasıdır. Bu yaklaşım genellikle önceden eğitilmiş DNN olarak kullanmaya kıyasla çok daha iyi sınıflandırma doğruluk doğurur-olduğu, ancak çok daha uzun eğitim süresini (gpu'su bile) çoğaltamaz.

Bir SVM yerine sinir ağı eğitiliyor yapılır değişkeni değiştirerek `classifier` içinde `PARAMETERS.py` gelen `svm` için `dnn`. Ardından, 1. bölümünde açıklandığı gibi veri hazırlığı (1. adım) ve SVM eğitim (4. adım) dışındaki tüm betikleri yeniden yürütülmesi gerekir. DNN iyileştirme, bir GPU gerektirir. hiçbir GPU bulunamazsa veya GPU (örneğin bir önceki CNTK çalıştırma tarafından) kilitliyse sonra betik `2_refineDNN.py` bir hata oluşturur. DNN eğitim throw bellek yetersiz hatası minibatch boyutunu azaltarak önlenebilir bazı Gpu'lar üzerindeki (değişken `cntk_mb_size` içinde `PARAMETERS.py`).

Alıştırma tamamlandıktan sonra daraltılmış modeli kaydedilen *DATA_DIR/proc/fashionTexture/cntk_refined.model*, ve bir çizim eğitim ve test sınıflandırma hataları eğitim sırasında nasıl değiştiğini gösterir. Eğitim kümesi hatasında bir sınama kümesi üzerinde çok daha küçük olan bu çizim, unutmayın. Bu sözde aşırı sığdırma davranışı, örneğin, düşme oranı daha yüksek bir değer kullanılarak azaltılabilir `rf_dropoutRate`.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/output_script_3_plot.png" alt="alt text" height="300"/>
</p>

Aşağıdaki çizim görüldüğü gibi DNN iyileştirme sağlanan bir veri kümesini kullanarak doğruluğu (Kısım 1) önce %88.92 karşı %92.35 olur. Özellikle, 'noktalı' görüntüleri, bir ROC alanı altında-eğrisi 0,98 iyileştirme vs ile birlikte önemli ölçüde artırır. 0,94 önce. Küçük bir veri kümesi kullanıyoruz ve bu nedenle kodu çalıştıran gerçek doğruluk farklıdır. Bu uyuşmazlık, eğitim ve test etme görüntülerin rastgele bölme gibi stokastik etkileri kaynaklanır.
<p align="center">
<img src="media/scenario-image-classification-using-cntk/roc_confMat_dnn.jpg" alt="alt text" width="700"/>
</p>

### <a name="run-history-tracking"></a>Çalıştırma geçmişini izleme

Azure Machine Learning Workbench depoları olan iki veya daha fazla çalıştırma karşılaştırması izin vermek için Azure üzerinde çalıştırma geçmişi her hafta parçalayın bile. Bu ayrıntılı olarak açıklanan [Iris öğreticisini](tutorial-classifying-iris-part-2.md). Aşağıdaki ekran görüntülerinde komut iki çalıştırma biz karşılaştırma burada gösterilmiştir `5_evaluate.py`, ya da DNN iyileştirme kullanarak diğer bir deyişle, `classifier = "dnn"`(çalışma sayı 148) veya diğer bir deyişle, eğitim SVM `classifier = "svm"` (çalışma sayı 150).

İlk ekran görüntüsünde, DNN iyileştirme SVM eğitim tüm sınıflar için daha iyi doğruluk neden olur. İkinci ekran görüntüsüne sınıflandırıcı neydi dahil olmak üzere izlendiğini tüm ölçümleri gösterir. Bu izleme betiğin tamamlanmasının `5_evaluate.py` Azure Machine Learning Workbench Günlükçü çağırarak. Ayrıca, betik ROC eğrisi ve karışıklık matrisi de kaydeder *çıkarır* klasör. Bu *çıkarır* klasördür özel içeriği ayrıca Workbench geçmişi özelliği tarafından izlenir ve çıkış dosyalarının olup yerel kopyaları üzerine bağımsız olarak istediği zaman, bu nedenle erişilebilir.

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison1.jpg" alt="alt text" width="700"/>
</p>

<p align="center">
<img src="media/scenario-image-classification-using-cntk/run_comparison2b.jpg" alt="alt text" width="700"/>
</p>


### <a name="parameter-tuning"></a>Parametre ayarlama

Çoğu makine öğrenme projeleri için doğru olduğundan, iyi sonuçlar için yeni bir veri kümesi alma ayarlama yanı sıra farklı tasarım kararlarına değerlendirme dikkatli parametresi gerektirir. Bu görevler için önemli olan tüm parametreleri belirtilir ve kısa bir açıklama sağlanan, tek bir yerde: `PARAMETERS.py` dosya.

Geliştirmeleri için en olası meydan veren noktaları bazıları şunlardır:

- Veri Kalitesi: eğitim ve test kümelerine sahip yüksek kaliteli emin olun. Diğer bir deyişle, görüntüleri kaldırıldı doğru açıklamalı, belirsiz (örneğin çizgiler ve noktalar giysi öğeleri) görüntüleridir ve öznitelikleri karşılıklı olarak birbirini dışlar (diğer bir deyişle, seçilen her bir görüntü için tam olarak bir öznitelik ait olacak şekilde).

- Görüntüde faiz, nesne küçükse görüntü sınıflandırma yaklaşımları düzgün çalışmıyor bilinmektedir. Bu gibi durumlarda, bu konuda açıklandığı gibi bir nesne algılama yaklaşımı kullanarak göz önünde bulundurun [öğretici](https://github.com/Azure/ObjectDetectionUsingCntk).
- DNN iyileştirme: doğru hale getirmek için tartışmasız en önemli öğrenme oranı parametredir `rf_lrPerMb`. Eğitim doğruluğu (Bölüm 2'de ilk Şekil) ayarlarsanız, 0-5 yakın değil %, en olası olduğu nedeniyle yanlış bir öğrenme oranı. İle başlayan diğer parametreler `rf_` daha az önemlidir. Genellikle, eğitim hata katlanarak azaltma ve bu eğitim sonra %0 yakın olması gerekir.

- Giriş çözünürlüğü: varsayılan görüntü çözünürlüğünü 224 x 224 pikseldir. Daha yüksek görüntü çözünürlüğü (parametre: `rf_inputResoluton`), örneğin, 448 x 448 veya 896 x 896 piksel genellikle önemli doğruluğu artırır ancak DNN iyileştirme yavaşlatır. **Daha yüksek görüntü çözünürlüğü neredeyse bedeli olduğundan ve neredeyse her zaman doğruluğu artırır**.

- DNN atlayarak sığdırma: DNN iyileştirme sırasında eğitim ve test doğruluğu arasında büyük bir boşluk kaçının (ilk şekil bölüm 2). Bu boşluğu çıkarma oranları kullanılarak azaltılabilir `rf_dropoutRate` 0,5 ya da daha fazlasına ve regularizer ağırlık artırarak `rf_l2RegWeight`. Çıkarma yüksek oran kullanılarak DNN giriş görüntü çözünürlüğünü yüksekse, özellikle yararlı olabilir.

- Daha ayrıntılı Dnn'leri değiştirerek kullanmayı deneyin `rf_pretrainedModelFilename` gelen `ResNet_18.model` ya da `ResNet_34.model` veya `ResNet_50.model`. Resnet-50 model yalnızca daha kapsamlı değildir, ancak çıktısını sondan katmanın olduğundan boyutu 2048 float (vs. 512 float ResNet-18 ve ResNet-34 modellerin). Artan bu boyut SVM sınıflandırıcı eğitimindeki özellikle yararlı olabilir.

## <a name="part-3---custom-dataset"></a>Bölüm 3 - özel veri kümesi

Bölüm 1 ve 2, eğitim ve belirtilen üst gövdesi giysi dokular görüntüleri kullanarak bir görüntü sınıflandırma modeli değerlendirilir. Şimdi nasıl bunun yerine özel bir kullanıcı tarafından sağlanan veri kullanılacağını göstereceğiz. Veya yoksa, nasıl oluşturup Bing kullanarak bir veri kümesi gibi ek açıklama Resim arayın.

### <a name="using-a-custom-dataset"></a>Özel bir veri kümesi kullanma

İlk olarak, şimdi giysi doku veri klasör yapısını göz sahip. Not nasıl tüm görüntüler için farklı öznitelikler ilgili alt klasörlerinde *noktalı*, *, leopard ve *şeritli* en *DATA_DIR/resimler/fashionTexture/*. Ayrıca nasıl görüntü klasörü adı da meydana unutmayın `PARAMETERS.py` dosyası:
```python
datasetName = "fashionTexture"
```

Özel bir veri kümesini kullanarak yeniden oluştururken bu klasör yapısı olarak basit göre öznitelik ve bu alt klasörler yeni bir kullanıcı tarafından belirtilen dizine kopyalamak için alt klasörlerdeki tüm görüntüleri nerede *DATA_DIR/resimler/newDataSetName/*. Gereken tek kod değişikliği ayarlamaktır `datasetName` değişkenini *newDataSetName*. 1-5 betikleri ardından sırayla yürütülür ve tüm ara dosyaları yazılır *DATA_DIR/proc/newDataSetName/*. Herhangi bir kod değişikliği gerekli değildir.

Her bir görüntü için tam olarak bir öznitelik atanabilir önemlidir. Örneğin, bir 'leopard' görüntüsü 'hayvan için' ayrıca ait olduğundan öznitelikleri 'donatarak' ve 'leopard' yanlış olur. Ayrıca, belirsiz ve bu nedenle ek açıklama zor olan görüntüleri silmenizde yarar vardır.



### <a name="image-scraping-and-annotation"></a>Görüntü değiştirilene ve ek açıklama

Eğitim ve test amacıyla ek açıklamalı görüntüleri yeterince çok sayıda toplama zor olabilir. Bu sorunu çözmenin yollarından biri, Internet'ten görüntüleri scrape sağlamaktır. Örneğin, Bing resim arama sonuçları sorgu için aşağıya bakın *şeritli t-shirt*. Beklendiği gibi çoğu görüntüleri gerçekten şeritli tişörtler. (Örneğin, sütun 1, 1; satır veya sütun 3, satır 2) birkaç yanlış veya belirsiz görüntüleri tanımlanır ve kolayca kaldırıldı:
<p align="center">
<img src="media/scenario-image-classification-using-cntk/bing_search_striped.jpg" alt="alt text" width="600"/>
</p>

Büyük ve farklı bir veri kümesi oluşturmak için birden çok sorgu kullanılmalıdır. Örneğin, 7\*3 = 21 sorgular oluşturulan tüm birleşimlerini {blouse hoodie, pullover, sweater, gömlek, t-shirt, vest} giysi öğeleri ve özniteliklerinin {şeritli, noktalı, leopard} kullanılarak otomatik olarak. Sorgu başına ilk 50 görüntüleri indirme ardından neden en fazla 21 * 50 = 1050 görüntüler.

Bing görüntü arama karşıdan görüntüleri el ile yükleme yerine bunu kullanmayı çok daha kolaydır [Bilişsel hizmetler Bing resim arama API'si](https://www.microsoft.com/cognitive-services/bing-image-search-api) resim URL'leri bir sorgu dizesi verilmiş bir dizi döndürür.

İndirilen resmi tam ya da çoğaltmaları bazıları (örneğin, yalnızca görüntü çözünürlüğünü veya jpg yapıtları farklı). Eğitim ve test bölme içermez aynı görüntüleri böylece bu yinelemeleri kaldırılması gerekir. Yinelenen görüntüleri kaldırma gerçekleştirilebilir iki adımda çalışan bir karma tabanlı yaklaşımı sayesinde: (i) karma dize tüm görüntüler için; ilk olarak hesaplanır (ii görüntüleri üzerinden bir ikinci geçiş) bu görüntülerin değil henüz görüldü karma dize tutulur. Diğer tüm görüntüleri atılır. Bulduk `dhash` Python kitaplığı yaklaşımda `imagehash` ve bu konuda açıklanan [blog](http://www.hackerfactor.com/blog/index.php?/archives/529-Kind-of-Like-That.html) parametresiyle de gerçekleştirilecek `hash_size` 16 olarak ayarlayın. Gerçek yinelenenleri çoğunu kaldırıldı sürece yanlış bazı yinelenmeyen görüntüleri kaldırmak için Tamam olduğunu.





## <a name="conclusion"></a>Sonuç

Bu örnekte anahtar bazı önemli noktalar şunlardır:
- Görüntü sınıflandırma modellerini dağıtmak eğitme ve değerlendirme için kod.
- Sağlanan tanıtım görüntüler, ancak (kendi görüntü veri kümesini kullanmak için kolayca uyarlanabilir bir satır değiştirme).
- Aktarım öğrenmeyi göre yüksek doğruluk modelleri eğitmek için uygulanan durumu resim Uzman özellikleri.
- Azure Machine Learning Workbench ve Jupyter not defteri ile etkileşimli model geliştirme.


## <a name="references"></a>Başvurular

[1] Alex Krizhevsky ve Ilya Sutskever Geoffrey E. Hinton [ _derin Evrişimsel sinir ağları ile Imagenet sınıflandırma_](https://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf). NIPS 2012.  
[2] Kaiming He, Xiangyu Zhang Shaoqing olan Ren ve Güneş, Jian [ _derin fazlalık için resim tanıma öğrenme_](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/He_Deep_Residual_Learning_CVPR_2016_paper.pdf). CVPR 2016.
