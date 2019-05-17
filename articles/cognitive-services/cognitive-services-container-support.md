---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Docker kapsayıcıları yakın Bilişsel hizmetler verilerinize nasıl edinebildiğini öğrenin.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 05/15/2019
ms.author: diberry
ms.openlocfilehash: ea74dbd6cffa34e4462433cdfc51e7d5abac9a41
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65796662"
---
# <a name="container-support-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler kapsayıcı desteği

Azure Bilişsel hizmetler kapsayıcı desteği sayesinde geliştiriciler, Azure'da kullanılabilen zengin API'leri kullanmak için ve nerede dağıtmak ve gelir hizmetlerini barındırmak esneklik sağlar [Docker kapsayıcıları](https://www.docker.com/what-container). Kapsayıcı desteği şu anda bir alt kümesini Azure Bilişsel bölümlerini dahil olmak üzere hizmetler için önizleme modunda kullanılabilir:

* [Anomali algılayıcısı](Anomaly-Detector/overview.md)
* [Görüntü İşleme](Computer-vision/Home.md)
* [Yüz tanıma](Face/Overview.md)
* [Form tanıyıcı](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409)
* [Language Understanding'i](LUIS/luis-container-howto.md) (LUIS)
* [Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409)
* [Konuşma Hizmeti API’si](https://go.microsoft.com/fwlink/?linkid=2083926&clcid=0x409)
* [Metin Analizi](text-analytics/overview.md)

Kapsayıcı içinde bir uygulama veya onun bağımlılıklarını & yapılandırması gibi hizmet birlikte bir kapsayıcı görüntüsüne paketlenmiştir yazılım dağıtımı için bir yaklaşımdır. Çok az kayıpla veya hiç değişiklik yapmadan kapsayıcı görüntüsü, bir kapsayıcı konağı üzerinde dağıtılabilir. Birbirine ve bir sanal makine değerinden daha küçük bir kaplama alanı ile temel işletim sistemi, yalıtılmış kapsayıcılardır. Kapsayıcılar için kısa vadeli görevleri kapsayıcı görüntülerinden oluşturulan ve artık gerekli olmadığında kaldırıldı.

Aşağıdaki videoda, Bilişsel hizmetler kapsayıcı kullanmayı gösterir.

[![Bilişsel hizmetler için kapsayıcı Tanıtımı](./media/index/containers-video-image.png)](https://azure.microsoft.com/resources/videos/containers-support-of-cognitive-services)

Bilişsel hizmetler kaynakları bulunur [Microsoft Azure](https://azure.microsoft.com). Oturum [Azure portalında](https://portal.azure.com/) oluşturma ve bu hizmetler için Azure kaynaklarını keşfedin.

## <a name="features-and-benefits"></a>Özellikler ve avantajlar

- **Veriler üzerinde denetim**: Burada bu Bilişsel hizmetler, veri işlem seçme özgürlüğü sağlar. Bu, verileri buluta Gönder olamaz, ancak Bilişsel hizmetler teknoloji erişmesi gereken müşteriler için gereklidir. Tutarlılık, veri, yönetim, kimlik ve güvenlik arasında Karma ortamlarda – destekler.
- **Model güncelleştirmelerini denetime**: Müşteriler, sürüm oluşturma ve kendi çözümlerinde dağıtılan modelleri güncelleştirme konusunda esneklik sağlar.
- **Taşınabilir mimarisi**: Azure'da, şirket içi ve uç dağıtılabilir bir taşınabilir uygulama mimarisi oluşturulmasını sağlar. Kapsayıcıları doğrudan dağıtılabilir [Azure Kubernetes hizmeti](../aks/index.yml), [Azure Container Instances](../container-instances/index.yml), veya bir [Kubernetes](https://kubernetes.io/) kümesi dağıtıldı için [Azure Yığın](/azure-stack/operator). Daha fazla bilgi için [Azure Stack dağıtma Kubernetes](/azure-stack/user/azure-stack-solution-template-kubernetes-deploy).
- **Yüksek performans / düşük gecikme süresi**: Müşterilerin, yüksek aktarım hızı ve düşük gecikme süresi gereksinimlerine, uygulama mantığı ve verileri yakın fiziksel olarak çalıştırmak, Bilişsel hizmetler etkinleştirerek ölçeklendirme olanağı sağlar. Kapsayıcılar, saniye başına işlem (TPS) cap değil ve gerekli donanım kaynakları sağlarsanız, isteğe bağlı işlemek için yukarı ve dışarı ölçeklendirme yapılabilir. 


## <a name="containers-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler, kapsayıcılar

Azure Bilişsel hizmetler kapsayıcılar, Docker kapsayıcıları, aşağıdaki dizi her biri, Azure Bilişsel hizmetler hizmetlerden işlevlerinin bir alt kümesini içeren sağlar:

| Hizmet | Desteklenen bir fiyatlandırma katmanı | Kapsayıcı | Açıklama |
|---------|----------|----------|-------------|
|[Anomali algılayıcısı](https://go.microsoft.com/fwlink/?linkid=2083925&clcid=0x409) |F0, S0|**Anomali algılayıcısı** |Anomali algılayıcısı API'si, izleme ve machine learning ile zaman serisi verilerinizdeki prosesler algılamak sağlar.<br>[Erişim isteği](https://aka.ms/adcontainer)|
|[Görüntü İşleme](Computer-vision/computer-vision-how-to-install-containers.md) |F0, S1|**Metin tanıma** |Farklı yüzey ve arka planlar, giriş ve posterler kartvizitler gibi çeşitli nesne görüntülerdeki yazdırılan metin ayıklar.<br/><br/>**Önemli:** Metni Tanı kapsayıcı şu anda yalnızca İngilizce ile çalışır.<br>[Erişim isteği](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Yüz tanıma](Face/face-how-to-install-containers.md) |F0, S0|**Yüz tanıma** |Görüntülerdeki İnsan yüzlerini algılar ve yüz yer işareti (örneğin, noses ve gözler), cinsiyet, geçerlilik süresi ve diğer makine tahmin yüz özellikleri dahil olmak üzere, öznitelikleri tanımlar. Yüz algılama ek olarak, iki yüzün aynı görüntü ya da farklı görüntüleri bir güven puanı kullanarak aynı olduğundan veya bir benzeyen olmadığını görmek için bir veritabanında yüzleri karşılaştırın veya aynı yüz zaten kontrol edebilirsiniz. Bu gibi durumlarda, benzer yüzlerden de paylaşılan visual nitelikler kullanarak gruplar halinde düzenleyebilirsiniz.<br>[Erişim isteği](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[Form tanıyıcı](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409) |F0, S0|**Form tanıyıcı** |Formu anlama tanımlamak ve anahtar-değer çiftleri ve tabloları formlardan ayıklamak için makine öğrenimi teknolojisi geçerlidir.<br>[Erişim isteği](https://aka.ms/FormRecognizerContainerRequestAccess)|
|[LUIS](LUIS/luis-container-howto.md) |F0, S0|**LUIS** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2043204&clcid=0x409))|Bir eğitilen veya yayımlanmış dil anlama modeli olarak da bilinen bir LUIS uygulaması bir docker kapsayıcısına yükler ve kapsayıcının API uç noktalardan gelen sorgu tahminler elde etmek için erişim sağlar. Kapsayıcıdan sorgu günlüklerini toplamak ve bu geri yükleme [LUIS portalı](https://www.luis.ai) uygulamanın tahmin doğruluğunu artırmak için.|
|[Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409) |F0, S0|**Personalizer** ([image](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409))|Azure Kişiselleştirme, kullanıcıların gerçek zamanlı davranışlarından öğrenerek onlara gösterilebilecek en iyi deneyimi seçmenizi sağlayan bulut tabanlı bir API’dir.|
|[Konuşma Hizmeti API’si](https://go.microsoft.com/fwlink/?linkid=2083926&clcid=0x409) |F0, S0|**Konuşmayı metne dönüştürme** |Sürekli, gerçek zamanlı konuşmaları metne dönüştürür.<br>[Erişim isteği](https://aka.ms/speechcontainerspreview/)|
|[Konuşma Hizmeti API’si](https://go.microsoft.com/fwlink/?linkid=2083926&clcid=0x409) |F0, S0|**Metin okuma** |Metni, doğal sesli konuşmaya dönüştürür.<br>[Erişim isteği](https://aka.ms/speechcontainerspreview/)|
|[Metin Analizi](text-analytics/how-tos/text-analytics-how-to-install-containers.md) |F0, S|**Anahtar ifade ayıklama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018757&clcid=0x409)) |Ana noktaları belirleyin, anahtar ifadeleri ayıklar. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür. |
|[Metin Analizi](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|F0, S|**Dil algılama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018759&clcid=0x409)) |En fazla 120 dil için hangi dil giriş metni yazılır ve rapor istekte gönderilen her belge için bir tek dil kodu algılar. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. |
|[Metin Analizi](text-analytics/how-tos/text-analytics-how-to-install-containers.md)|F0, S|**Yaklaşım analizi** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018654&clcid=0x409)) |Ham metin pozitif veya negatif yaklaşım hakkında ipuçları için analiz eder. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir. Analiz modelleri metin ve doğal dil Microsoft teknolojilerinin kapsamlı bir gövdesi kullanarak önceden eğitilir. API, [seçili dillerde](./text-analytics/language-support.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir. |

Ayrıca, bazı kapsayıcıları Bilişsel hizmetler desteklenmektedir [ **hepsi bir arada sunan** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) kaynak anahtarı. Tek bir Bilişsel hizmetler hepsi bir arada kaynak oluşturabilir ve aynı faturalama anahtarının desteklenen hizmetler aşağıdaki hizmetler için kullanın:

* Görüntü İşleme
* Yüz
* LUIS
* Metin Analizi

## <a name="container-availability-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler kapsayıcı kullanılabilirlik

Azure aboneliğiniz üzerinden genel kullanıma açık Azure Bilişsel hizmetler kapsayıcıları ve Docker kapsayıcı görüntülerini Microsoft kapsayıcı kayıt defteri veya Docker Hub çekilebilir. Kullanabileceğiniz [docker isteği](https://docs.docker.com/engine/reference/commandline/pull/) uygun kayıt defterinden bir kapsayıcı görüntüsü indirilemedi komutu.

> [!IMPORTANT]
> Şu anda aşağıdaki kapsayıcılar, doldurun ve sorularınız varsa, şirketinizin ve kapsayıcıları uygulamak istediğiniz kullanım örneği hakkında bir soru gönderin, erişmek için bir kaydolma işlemini tamamlamanız gerekir. Erişim izni ve da sağlanan kimlik bilgileri sonra Azure Container Registry tarafından barındırılan bir özel kapsayıcı kayıt defterinden tipini ve metin tanıma kapsayıcılar için kapsayıcı görüntülerini çeker.
> * [Anomali dectector](Anomaly-Detector/anomaly-detector-container-howto.md#request-access-to-the-container-registry)
> * [Yüz tanıma](Face/face-how-to-install-containers.md)
> * [Form tanıyıcı](form-recognizer/form-recognizer-container-howto.md#request-access-to-the-container-registry)
> * [Metin tanıma](Computer-vision/computer-vision-how-to-install-containers.md)
> * [Konuşmayı metne ve metin okuma](Speech-Service/speech-container-howto.md#request-access-to-the-container-registry)

## <a name="prerequisites"></a>Önkoşullar

Azure Bilişsel hizmetler kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

**Docker altyapısı**: Docker altyapısının yerel olarak yüklü olması gerekir. Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), ve [Windows](https://docs.docker.com/docker-for-windows/). Windows Docker Linux kapsayıcıları destekleyecek şekilde yapılandırılması gerekir. Docker kapsayıcıları da dağıtılabilir doğrudan [Azure Kubernetes hizmeti](../aks/index.yml) veya [Azure Container Instances](../container-instances/index.yml).

Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır.

**Microsoft Container Registry ve Docker konusunda**: Bir temel kavramlarını hem Microsoft Container Registry ve Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.

Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).

Kapsayıcılara sunucu ve bellek ayırma gereksinimleri dahil olmak üzere kendi gereksinimleriyle de sahip olabilir.

## <a name="developer-samples"></a>Geliştirici örnekleri

Geliştirici Örnekleri kullanıma sunduğumuz [GitHub deposu](https://github.com/Azure-Samples/cognitive-services-containers-samples).

## <a name="view-webinar"></a>Web seminerini izleyin 

Birleştirme [Web Semineri](https://info.microsoft.com/AP-AZUREPLAT-WBNR-FY19-03Mar-20-Seamlesslyintegrateapplicationsdataandprocesses-MCW0012283_02OnDemandRegistration-ForminBody.html) hakkında bilgi edinmek için:

* Bilişsel hizmetler, Docker kullanan herhangi bir makineye dağıtma
* Bilişsel hizmetler AKS'ye dağıtma

## <a name="next-steps"></a>Sonraki adımlar

Yükleme ve Azure Bilişsel hizmetler, kapsayıcılar tarafından sağlanan işlevselliği keşfedin:

* [Anomali algılayıcısı kapsayıcıları](Anomaly-Detector/anomaly-detector-container-howto.md)
* [Bilgisayar işleme kapsayıcıları](Computer-vision/computer-vision-how-to-install-containers.md)
* [Yüz tanıma kapsayıcıları](Face/face-how-to-install-containers.md)
* [Form tanıyıcı kapsayıcıları](https://go.microsoft.com/fwlink/?linkid=2083826&clcid=0x409)
* [Language Understanding (LUIS) kapsayıcıları](LUIS/luis-container-howto.md)
* [Personalizer kapsayıcıları](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409)
* [Konuşma hizmeti API'sini kapsayıcıları](https://go.microsoft.com/fwlink/?linkid=2083926&clcid=0x409)
* [Metin analizi kapsayıcıları](text-analytics/how-tos/text-analytics-how-to-install-containers.md)