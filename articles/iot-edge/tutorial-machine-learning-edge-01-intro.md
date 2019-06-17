---
title: Machine Learning, Azure IOT Edge üzerinde ayrıntılı kılavuz | Microsoft Docs
description: Bir uçtan uca makine öğrenimi edge senaryoyu oluşturmak gereken çeşitli görevleri kılavuzluk üst düzey bir öğretici.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 06/13/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 916e48752431be41ff150c2ac84e66eb1e98e81f
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67057753"
---
# <a name="tutorial-an-end-to-end-solution-using-azure-machine-learning-and-iot-edge"></a>Öğretici: Azure Machine Learning ve IOT Edge kullanarak bir uçtan uca çözüm

Genellikle, IOT uygulamaları akıllı bir Bulutu ve akıllı bir ucu yararlanmak istiyorsanız. Bu öğreticide, biz bir machine learning modeli bulutta IOT cihazlarından toplanan verilerle eğitim aracılığıyla IOT Edge, bakımını yapma ve model düzenli olarak iyileştirme için bu modeli dağıtma konusunda rehberlik.

Birincil amacı, Bu öğretici, machine learning, özellikle uçta ile IOT verilerini işleme tanıtmak sağlamaktır. Biz genel makine öğrenme iş akışı birçok yönden dokunma, ancak bu öğreticide ayrıntılı bir giriş makine öğrenimi kullanılmaya yönelik değildir. Bir durumda giriş noktası olarak biz kullanım örneği için yüksek oranda iyileştirilmiş bir model oluşturmak çalışmayın – yalnızca oluşturma ve IOT veri işleme için uygun bir modeli kullanma işlemini göstermek için yeterli desteklemiyoruz.

## <a name="target-audience-and-roles"></a>Hedef kitle ve rolleri

Bu makaleler kümesini IOT geliştirme veya machine learning'de bir deneyiminiz olmaksızın geliştiricilere yöneliktir. Dağıtma makine öğrenimi ucuna bağlanmak çok çeşitli teknolojiler konusunda bilgi gerektirir. Bu nedenle, Bu öğretici, bu teknolojiler birlikte bir IOT çözümü için birleştirme bir şekilde göstermek için tüm uçtan uca senaryo kapsar. Gerçek bir ortamda, bu görevler birkaç kişiyle farklı uzmanlıklar arasında dağıtılmış olabilir. Örneğin, veri bilimcileri analiz modelleri tasarlanmış olmasına rağmen geliştiricilerin kod, cihaz veya Bulut üzerinde odaklanın. Bu öğreticiyi başarıyla tamamlamak bir geliştirici etkinleştirmek için ek yönergeler öngörülerle sağladık ve umuyoruz daha fazla bilgi için bağlantılar ne, neden yanı sıra yapılması anlamak yeterlidir.

Alternatif olarak, işlenmesi için tam uzmanlığınızı getirme öğreticiyi birlikte uygulamak için iş arkadaşlarınızla farklı rolleri güçlerini birleştirdi ve bir takım olarak öğelerin birbirine nasıl uyduğunu öğrenin.

Her iki durumda da, kullanıcılar yönlendirmenize yardımcı olmak için bu öğreticideki her bir makaleyi kullanıcı rolünü belirtir. Bu rolleri şunlardır:

* Bulut geliştirme (DevOps kapasitede çalışan bir bulut Geliştirici dahil)
* Veri analizi

## <a name="use-case-predictive-maintenance"></a>Kullanım örneği: Tahmine dayalı bakım

Biz bu senaryo ve konferansta Prognostics ve sistem durumu yönetimi (PHM08) 2008'de sunulan bir kullanım örneği temel. Kalan faydalı ömrü (RUL) tahmin turbofan uçak motorları birtakım olmaktır. Bu veriler, C-MAPSS MAPSS (modüler Aero Propulsion sistemi benzetimi) yazılımının ticari sürümü kullanılarak oluşturuldu. Bu yazılım rahatça durumu, Denetim ve altyapısı parametreleri benzetimini yapmak için esnek turbofan engine benzetimi ortamı sağlar.

Bu öğreticide kullanılan veri alınır [Turbofan engine performans düşüşü simülasyonu veri kümesi](https://ti.arc.nasa.gov/tech/dash/groups/pcoe/prognostic-data-repository/#turbofan).

Benioku dosyası:

***Deneysel senaryosu***

*Veri kümeleri birden fazla çok değişkenli zaman serisi oluşur. Her bir veri kümesi, eğitim ve test altkümelere, daha fazla ayrılmıştır. Her zaman serisi farklı altyapısından – yani, verilerin aynı türde altyapıları filosundan olması kabul edilebilir. Her motor ilk wear ve kullanıcıya bilinmeyen değişim üretim farklı derece başlar. Bu giyim ve Varyasyon olarak kabul edilir normal, yani, bir hata koşulu sayılmaz. Altyapısı performansı üzerinde önemli bir etkiye sahip üç işletimsel ayarları vardır. Bu ayarlar, verileri de dahil edilir. Veri ile algılayıcı gürültü contaminated.*

*Altyapısı, her zaman serisi başlangıcında normal olarak çalışıyor ve belirli bir noktada bir arıza sırasında serisi geliştirir. Eğitim kümesine hata boyutları sistem hatası kadar büyür. Test kümesinde sistem hatasından önce biraz zaman zaman serisi sona erer. Yarışma amacı, test kümedeki başka bir deyişle, altyapı çalışmaya devam edeceği son döngüsünden sonra işletimsel döngüsü sayısını hatasından önce sitede kalan işletimsel döngüsü sayısını tahmin etmektir. Ayrıca test verilerini gerçek kalan kullanım ömrü (RUL) değerlerle oluşan bir vektörü sağlanır.*

Verileri bir yarışmaya yayımlanan olduğundan, makine öğrenimi modelleri türetmek için çeşitli yaklaşımlar bağımsız olarak yayımlandı. Örnekler üzerinde çalışmak için süreci anlamak ve belirli bir makine öğrenme modelinin oluşturmada yer alan akıl faydalı olduğunu bulduk. Örneğin bakın:

[Uçak motoru hatası tahmin modeli](https://github.com/jancervenka/turbofan_failure) GitHub kullanıcı jancervenka tarafından.

[Turbofan engine düşüşü](https://github.com/hankroark/Turbofan-Engine-Degradation) GitHub kullanıcı hankroark tarafından.

## <a name="process"></a>Process

Aşağıdaki resimde, biz Bu öğreticide izleyin kaba adımları göstermektedir:

![İşlem adımları için Mimari diyagramı](media/tutorial-machine-learning-edge-01-intro/tutorial-steps-overview.png)

1. **Eğitim verileri toplama**: Eğitim verileri toplayarak işlemi başlar. Bazı durumlarda, veri zaten toplanmış ve bir veritabanı veya veri dosyalarının form bulunur. IOT senaryoları için özellikle diğer durumlarda, verilerin IOT cihazlardan ve sensörlerden toplanan ve bulutta depolanmış gerekir.

   Proje dosyalarını NASA cihaz verileri buluta gönderiyor bir basit cihaz simülatörü eklemek için turbofan altyapıları koleksiyonunu yoksa varsayıyoruz.

1. **Veri hazırlama**. Çoğu durumda, ham veriler cihazlardan ve sensörlerden toplanan olarak makine öğrenimi için hazırlık gerektirir. Bu adım veri temizleme, veri biçimlendirme içerebilir veya ek bilgi makine öğrenimi eklemesine ön işleme kapalı anahtarını.

   Uçak motoru makine, verilerimiz için veriler üzerinde gerçek gözlemler göre örnek her veri noktasına açık zamanı hatası saatleri hesaplama veri hazırlığı kapsar. Makine öğrenme algoritmasına gerçek algılayıcı veri desenleri ve Tahmini kalan yaşam süresi altyapısı arasındaki bağlantıları bulmak için bu bilgileri sağlar. Yüksek oranda etki alanına özgü adımdır.

1. **Machine learning modeli derlemeyi**. Hazırlanan verileri temel alan, biz artık farklı makine öğrenimi algoritmaları ve modelleri eğitme ve birbirine sonuçları karşılaştırmak için parameterizations denemeler yapabilirsiniz.

   Bu durumda, test etmek için biz altyapıları birtakım gözlemlenen gerçek sonucu model tarafından hesaplanan tahmin edilen sonucu karşılaştırın. Azure Machine Learning'de biz modeli kayıt defterinde oluştururuz modellerinin farklı yinelemeleri yönetebilirsiniz.

1. **Modeli dağıtma**. Size sunduğumuz başarı ölçütleri karşılayan bir modeli oluşturduktan sonra dağıtım için geçebiliriz. Bu REST çağrılarını kullanarak verilerle iletilir ve analiz sonuçları döndüren web service uygulamanıza modeli sarmalayıp içerir. Web hizmeti uygulaması, ardından sırayla bulutta veya IOT Edge modülü olarak dağıtılabilir bir docker kapsayıcısına paketlenmiştir. Bu örnekte, IOT Edge dağıtımı odaklanıyoruz.

1. **Model iyileştirmek ve korumak**. Model dağıtıldıktan sonra işimizi yapılmaz. Çoğu durumda, düzenli aralıklarla bu verileri buluta yükleyin ve veri toplamaya devam etmek istiyoruz. Ardından bu verileri yeniden eğitme ve biz sonra IOT Edge için yeniden dağıtım yapabileceklerini modelimizi geliştirmek için kullanabiliriz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için kaynaklar oluşturma hakları olan bir Azure aboneliğine erişiminiz gerekir. Bu öğreticide kullanılan hizmetlerden bazıları Azure ücret uygulanabilir. Azure aboneliğiniz zaten yoksa, kullanmaya başlamak mümkün olabilir bir [Azure ücretsiz hesabı](https://azure.microsoft.com/offers/ms-azr-0044p/).

Ayrıca burada oluşturan bir Azure sanal makinesi, geliştirme makinenize olarak ayarlamak için komut dosyalarını çalıştırabileceğiniz yüklü PowerShell ile bir makine gerekir.

Bu belgede, biz aşağıdaki araçları kullanın:

* Bir Azure IOT hub verilerini yakalama için

* Azure not defterleri bizim veri hazırlama ve machine learning denemesi için ana ön ucu olarak. Python kodu bir not defteri örnek verilerinin bir alt kümesi üzerinde çalışan, veri hazırlama sırasında hızlı yinelemeli ve etkileşimli bir döngü almak için harika bir yoludur. Jupyter not defterleri, uygun ölçekte işlem arka betiklerin hazırlamak için de kullanılabilir.

* Azure Machine Learning için uygun ölçekte makine öğrenimi ve makine öğrenimi görüntü oluşturma için bir arka uç olarak. Biz, Jupyter not defterlerinde test ve hazırlanmış betikler kullanarak Azure Machine Learning arka uç sürücü.

* Machine learning görüntüsünün kapalı bulut uygulaması için Azure IOT Edge

Kuşkusuz, diğer seçeneği vardır. Bazı senaryolarda, örneğin, IOT Central bir kod içermeyen alternatif olarak IOT cihazlarından ilk eğitim verilerini yakalamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, aşağıdaki bölümlere ayrılmıştır:

1. Geliştirme makineniz ve Azure Hizmetleri ayarlayın.
2. Eğitim verileri makine öğrenme modülü için oluşturur.
3. Eğitim ve makine öğrenme modülü dağıtın.
4. Saydam bir ağ geçidi olarak görev yapacak bir IOT Edge cihazı yapılandırın.
5. Oluşturun ve IOT Edge modüllerini dağıtmak.
6. IOT Edge cihazınıza veri gönderin.

Bir geliştirme makinesi kurmak ve Azure kaynaklarınızı sağlamak için sonraki makaleye geçin.

> [!div class="nextstepaction"]
> [Machine learning IOT Edge üzerinde için bir ortamı ayarlama](tutorial-machine-learning-edge-02-prepare-environment.md)
