---
title: Kapsayıcı desteği
titleSuffix: Azure Cognitive Services
description: Docker kapsayıcıları yakın Bilişsel hizmetler verilerinize nasıl edinebildiğini öğrenin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.topic: article
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: d9f226213215f66b53eb1ef248fd47f7b6dfee5a
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67705371"
---
# <a name="container-support-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler kapsayıcı desteği

Azure Bilişsel hizmetler kapsayıcı desteği sayesinde geliştiriciler, Azure'da kullanılabilen zengin API'leri kullanmak için ve nerede dağıtmak ve gelir hizmetlerini barındırmak esneklik sağlar [Docker kapsayıcıları](https://www.docker.com/what-container). Kapsayıcı desteği şu anda bir alt kümesini Azure Bilişsel bölümlerini dahil olmak üzere hizmetler için önizleme modunda kullanılabilir:

* [Anomali algılayıcısı][ad-containers]
* [Görüntü İşleme][cv-containers]
* [Yüz tanıma][fa-containers]
* [Form tanıyıcı][fr-containers]
* [Language Understanding (LUIS)][lu-containers]
* [Konuşma Hizmeti API’si][sp-containers]
* [Metin Analizi][ta-containers]

<!--
* [Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409)
-->

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
|[Anomali algılayıcısı][ad-containers] |F0, S0|**Anomali algılayıcısı** |Anomali algılayıcısı API'si, izleme ve machine learning ile zaman serisi verilerinizdeki prosesler algılamak sağlar.<br>[Erişim isteği](https://aka.ms/adcontainer)|
|[Görüntü İşleme][cv-containers] |F0, S1|**Metin tanıma** |Farklı yüzey ve arka planlar, giriş ve posterler kartvizitler gibi çeşitli nesne görüntülerdeki yazdırılan metin ayıklar.<br/><br/>**Önemli:** Metni Tanı kapsayıcı şu anda yalnızca İngilizce ile çalışır.<br>[Erişim isteği](Computer-vision/computer-vision-how-to-install-containers.md#request-access-to-the-private-container-registry)|
|[Yüz tanıma][fa-containers] |F0, S0|**Yüz tanıma** |Görüntülerdeki İnsan yüzlerini algılar ve yüz yer işareti (örneğin, noses ve gözler), cinsiyet, geçerlilik süresi ve diğer makine tahmin yüz özellikleri dahil olmak üzere, öznitelikleri tanımlar. Yüz algılama ek olarak, iki yüzün aynı görüntü ya da farklı görüntüleri bir güven puanı kullanarak aynı olduğundan veya bir benzeyen olmadığını görmek için bir veritabanında yüzleri karşılaştırın veya aynı yüz zaten kontrol edebilirsiniz. Bu gibi durumlarda, benzer yüzlerden de paylaşılan visual nitelikler kullanarak gruplar halinde düzenleyebilirsiniz.<br>[Erişim isteği](Face/face-how-to-install-containers.md#request-access-to-the-private-container-registry) |
|[Form tanıyıcı][fr-containers] |F0, S0|**Form tanıyıcı** |Formu anlama tanımlamak ve anahtar-değer çiftleri ve tabloları formlardan ayıklamak için makine öğrenimi teknolojisi geçerlidir.<br>[Erişim isteği](https://aka.ms/FormRecognizerContainerRequestAccess)|
|[LUIS][lu-containers] |F0, S0|**LUIS** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2043204&clcid=0x409))|Bir eğitilen veya yayımlanmış dil anlama modeli olarak da bilinen bir LUIS uygulaması bir docker kapsayıcısına yükler ve kapsayıcının API uç noktalardan gelen sorgu tahminler elde etmek için erişim sağlar. Kapsayıcıdan sorgu günlüklerini toplamak ve bu geri yükleme [LUIS portalı](https://www.luis.ai) uygulamanın tahmin doğruluğunu artırmak için.|
|[Konuşma Hizmeti API’si][sp-containers] |F0, S0|**Konuşmayı metne dönüştürme** |Sürekli, gerçek zamanlı konuşmaları metne dönüştürür.<br>[Erişim isteği](https://aka.ms/speechcontainerspreview/)|
|[Konuşma Hizmeti API’si][sp-containers] |F0, S0|**Metin okuma** |Metni, doğal sesli konuşmaya dönüştürür.<br>[Erişim isteği](https://aka.ms/speechcontainerspreview/)|
|[Metin Analizi][ta-containers] |F0, S|**Anahtar ifade ayıklama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018757&clcid=0x409)) |Ana noktaları belirleyin, anahtar ifadeleri ayıklar. Örneğin, "The food was delicious and there were wonderful staff" (Yemekler lezzetliydi ve personel harikaydı) giriş metni olduğunda API, "food" (yemek) ve "wonderful staff" (personel harikaydı) ana konuşma noktalarını döndürür. |
|[Metin Analizi][ta-containers]|F0, S|**Dil algılama** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018759&clcid=0x409)) |En fazla 120 dil için hangi dil giriş metni yazılır ve rapor istekte gönderilen her belge için bir tek dil kodu algılar. Dil kodu, puanın ağırlığını belirten bir puanla eşleştirilir. |
|[Metin Analizi][ta-containers]|F0, S|**Yaklaşım analizi** ([görüntü](https://go.microsoft.com/fwlink/?linkid=2018654&clcid=0x409)) |Ham metin pozitif veya negatif yaklaşım hakkında ipuçları için analiz eder. API, her belge için 0 ile 1 arasında bir yaklaşım puanı döndürür ve 1 en pozitif değerdir. Analiz modelleri metin ve doğal dil Microsoft teknolojilerinin kapsamlı bir gövdesi kullanarak önceden eğitilir. API, [seçili dillerde](./text-analytics/language-support.md) sağladığınız ham metni analiz edip puanlayabilir ve sonuçları doğrudan çağrıyı yapan uygulamaya döndürebilir. |

<!--
|[Personalizer](https://go.microsoft.com/fwlink/?linkid=2083923&clcid=0x409) |F0, S0|**Personalizer** ([image](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409))|Azure Personalizer is a cloud-based API service that allows you to choose the best experience to show to your users, learning from their real-time behavior.|
-->

Ayrıca, bazı kapsayıcıları Bilişsel hizmetler desteklenmektedir [ **hepsi bir arada sunan** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAllInOne) kaynak anahtarı. Tek bir Bilişsel hizmetler hepsi bir arada kaynak oluşturabilir ve aynı faturalama anahtarının desteklenen hizmetler aşağıdaki hizmetler için kullanın:

* Görüntü İşleme
* Yüz
* LUIS
* Metin Analizi

## <a name="container-availability-in-azure-cognitive-services"></a>Azure Bilişsel hizmetler kapsayıcı kullanılabilirlik

Azure aboneliğiniz üzerinden genel kullanıma açık Azure Bilişsel hizmetler kapsayıcıları ve Docker kapsayıcı görüntülerini Microsoft kapsayıcı kayıt defteri veya Docker Hub çekilebilir. Kullanabileceğiniz [docker isteği](https://docs.docker.com/engine/reference/commandline/pull/) uygun kayıt defterinden bir kapsayıcı görüntüsü indirilemedi komutu.

> [!IMPORTANT]
> Şu anda aşağıdaki kapsayıcılar, doldurun ve sorularınız varsa, şirketinizin ve kapsayıcıları uygulamak istediğiniz kullanım örneği hakkında bir soru gönderin, erişmek için bir kaydolma işlemini tamamlamanız gerekir. Erişim izni ve da sağlanan kimlik bilgileri sonra Azure Container Registry tarafından barındırılan bir özel kapsayıcı kayıt defterinden kapsayıcı görüntülerini çeker.
> * [Anomali algılayıcısı](Anomaly-Detector/anomaly-detector-container-howto.md#request-access-to-the-container-registry)
> * [Yüz tanıma](Face/face-how-to-install-containers.md)
> * [Form tanıyıcı](form-recognizer/form-recognizer-container-howto.md#request-access-to-the-container-registry)
> * [Metin tanıma](Computer-vision/computer-vision-how-to-install-containers.md)
> * [Konuşmayı metne ve metin okuma](Speech-Service/speech-container-howto.md#request-access-to-the-container-registry)

[!INCLUDE [Container repositories and images](containers/includes/cognitive-services-container-images.md)]

## <a name="prerequisites"></a>Önkoşullar

Azure Bilişsel hizmetler kapsayıcıları kullanmadan önce aşağıdaki önkoşulları karşılamanız gerekir:

**Docker altyapısı**: Docker altyapısının yerel olarak yüklü olması gerekir. Docker üzerinde Docker ortamını yapılandıran paketler sağlar [macOS](https://docs.docker.com/docker-for-mac/), [Linux](https://docs.docker.com/engine/installation/#supported-platforms), ve [Windows](https://docs.docker.com/docker-for-windows/). Windows Docker Linux kapsayıcıları destekleyecek şekilde yapılandırılması gerekir. Docker kapsayıcıları da dağıtılabilir doğrudan [Azure Kubernetes hizmeti](../aks/index.yml) veya [Azure Container Instances](../container-instances/index.yml).

Docker, kapsayıcılar ile bağlanma ve faturalama verileri Azure'a göndermek izin verecek şekilde yapılandırılmalıdır.

**Microsoft Container Registry ve Docker konusunda**: Bir temel kavramlarını hem Microsoft Container Registry ve Docker kayıt defterleri, havuzları, kapsayıcılar ve kapsayıcı görüntülerinin yanı sıra temel bilgi gibi olmalıdır `docker` komutları.

Docker ve kapsayıcı temelleri hakkında bilgi için bkz: [Docker'a genel bakış](https://docs.docker.com/engine/docker-overview/).

Kapsayıcılara sunucu ve bellek ayırma gereksinimleri dahil olmak üzere kendi gereksinimleriyle de sahip olabilir.

[!INCLUDE [Discoverability of more container information](../../includes/cognitive-services-containers-discoverability.md)]

## <a name="next-steps"></a>Sonraki adımlar

Hakkında bilgi edinin [kapsayıcı tarifleri](containers/container-reuse-recipe.md) Bilişsel hizmetler ile kullanabilirsiniz.

Yükleme ve Azure Bilişsel hizmetler, kapsayıcılar tarafından sağlanan işlevselliği keşfedin:

* [Anomali algılayıcısı kapsayıcıları][ad-containers]
* [Bilgisayar işleme kapsayıcıları][cv-containers]
* [Yüz tanıma kapsayıcıları][fa-containers]
* [Form tanıyıcı kapsayıcıları][fr-containers]
* [Language Understanding (LUIS) kapsayıcıları][lu-containers]
* [Konuşma hizmeti API'sini kapsayıcıları][sp-containers]
* [Metin analizi kapsayıcıları][ta-containers]

<!--* [Personalizer containers](https://go.microsoft.com/fwlink/?linkid=2083928&clcid=0x409)
-->


[ad-containers]: anomaly-Detector/anomaly-detector-container-howto.md
[cv-containers]: computer-vision/computer-vision-how-to-install-containers.md
[fa-containers]: face/face-how-to-install-containers.md
[fr-containers]: form-recognizer/form-recognizer-container-howto.md
[lu-containers]: luis/luis-container-howto.md
[sp-containers]: speech-service/speech-container-howto.md
[ta-containers]: text-analytics/how-tos/text-analytics-how-to-install-containers.md