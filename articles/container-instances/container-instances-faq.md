---
title: Azure Container Instances - sık sorulan sorular
description: Azure Container Instances servisine ilgili sık sorulan soruların yanıtları
services: container-instances
author: dkkapur
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 4/25/2019
ms.author: dekapur
ms.openlocfilehash: 99882bd23d7b94afc550247172e5b70deb23bec9
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65791391"
---
# <a name="frequently-asked-questions-about-azure-container-instances"></a>Azure Container Instances hakkında sık sorulan sorular

Bu makalede, Azure Container Instances hakkında sık sorulan sorular yöneliktir.

## <a name="deployment"></a>Dağıtım

### <a name="how-large-can-my-container-image-be"></a>Kapsayıcı Görüntümü ne kadar büyük olabilir?

Bir Azure Container Instances'a dağıtılabilir bir kapsayıcı görüntüsünde boyutu üst sınırı 15 GB'dir. Daha büyük görüntüleri dağıtmak mümkün olabilir şu tam kullanılabilirliğine bağlı olarak, dağıtma, ancak bu garanti edilmez.

Kapsayıcı görüntünüzü boyutu, bu nedenle genellikle kapsayıcı görüntülerinizi olabildiğince küçük tutmak istediğiniz dağıtmak için ne kadar geçen etkiler.

### <a name="how-can-i-speed-up-the-deployment-of-my-container"></a>My kapsayıcı dağıtımını nasıl hız kazandırabilir?

Dağıtım kaç kez ana determinantlar birini görüntü boyutu olduğundan, boyutunu azaltma yolları için bakın. Gereksinim veya görüntü katmanları (daha açık bir temel işletim sistemi görüntüsünü seçerek) boyutunu yoksa katmanları kaldırın. Linux kapsayıcıları çalıştırıyorsanız, örneğin, kullanarak Alpine tam bir Ubuntu sunucusu yerine, temel görüntü düşünün. Benzer şekilde, Windows kapsayıcıları için bir temel Nano sunucu görüntüsü mümkün olduğunda kullanın. 

Azure kapsayıcı görüntüleri, aracılığıyla kullanılabilen önceden önbelleğe alınmış görüntüleri listesi de denetlemelisiniz [önbelleğe görüntüleri listeleme](/rest/api/container-instances/listcachedimages) API. Bir görüntü katmanı için önceden önbelleğe alınmış görüntülerden birini dışarı geçiş yapmak mümkün olabilir. 

Daha fazla bilgi bkz [ayrıntılı yönergeler](container-instances-troubleshooting.md#container-takes-a-long-time-to-start) üzerinde kapsayıcı başlangıç zamanı azaltır.

### <a name="what-windows-base-os-images-are-supported"></a>Hangi Windows temel işletim sistemi görüntüleri destekleniyor mu?

#### <a name="windows-server-2016-base-images"></a>Temel Windows Server 2016 görüntüleri

* [Nano Server](https://hub.docker.com/_/microsoft-windows-nanoserver): `10.0.14393.x`, `sac2016`
* [Windows Sunucu Çekirdeği](https://hub.docker.com/_/microsoft-windows-servercore): `ltsc2016`,  `10.0.14393.x`

> [!NOTE]
> Yarı Yıllık kanal sürüm 1709 veya 1803 dayalı Windows görüntüleri desteklenmez.

#### <a name="windows-server-2019-and-client-base-images-preview"></a>Windows Server 2019 ve istemci temel görüntüleri (Önizleme)

* [Nano Server](https://hub.docker.com/_/microsoft-windows-nanoserver): `1809`, `10.0.17763.x`
* [Windows Sunucu Çekirdeği](https://hub.docker.com/_/microsoft-windows-servercore): `ltsc2019`, `1809`, `10.0.17763.x`
* [Windows](https://hub.docker.com/_/microsoft-windows): `1809`, `10.0.17763.x` 

### <a name="what-net-or-net-core-image-layer-should-i-use-in-my-container"></a>My kapsayıcısında hangi .NET veya .NET Core görüntü katmanı kullanmam gerekir? 

Gereksinimlerinizi karşılayan en küçük görüntüyü kullanın. Linux için kullanabileceğinizi bir *çalışma zamanı alpine* .NET Core 2.1 yayımlandıktan sonra desteklenen .NET Core görüntüsü. Tam .NET Framework kullanıyorsanız Windows için ardından bir Windows Server Core görüntüsü kullanmanız gerekir (yalnızca çalışma zamanı görüntü gibi *4.7.2-windowsservercore-ltsc2016*). Yalnızca çalışma zamanı görüntüleri daha küçüktür, ancak .NET SDK'sı gerektiren iş yükleri desteklemez.

## <a name="availability-and-quotas"></a>Kullanılabilirlik ve kotalar

### <a name="how-many-cores-and-memory-should-i-allocate-for-my-containers-or-the-container-group"></a>Ne kadar çekirdek ve bellek miyim my kapsayıcılar veya kapsayıcı grubu için ayırmanız gerekir?

Bu gerçekten, iş yüküne bağlıdır. Küçükten başlayabilir ve bunu nasıl kapsayıcıları görmek için performans testi. [CPU ve bellek kaynağı kullanımı izleme](container-instances-monitor.md)ve ardından çekirdekleri veya bellek kapsayıcıda dağıtma işlemleri türüne göre ekleyin. 

Ayrıca denetlediğinizden emin olun [kaynak kullanılabilirliğini](container-instances-region-availability.md#availability---general) dağıttığınız CPU Çekirdeği ve kapsayıcı grubu başına bellek üst sınırları için bölge için. 

### <a name="what-underlying-infrastructure-does-aci-run-on"></a>Hangi altyapının ACI çalışıyor mu?

Azure Container Instances, biz kapsayıcılarınızı geliştirmeye odaklı istediğiniz ve altyapı konusunda endişelenmenize gerek kalmaz sunucusuz bir kapsayıcılar-isteğe bağlı hizmeti olması amaçlayan! Merak olanlar için veya isteme performansını karşılaştırmalar yapmak için ACI çeşitli SKU'ları, Azure Vm'lerinin kümelerinde öncelikle F ve D serisi çalıştırır. Geliştirme ve hizmet iyileştirme indikçe gelecekte değiştirmek için bekliyoruz. 

### <a name="i-want-to-deploy-thousand-of-cores-on-aci---can-i-get-my-quota-increased"></a>Bin ACI çekirdeklerde dağıtmak istiyorum - artan my kota alabilir miyim?
 
Evet (bazen). Bkz: [kotalar ve sınırlar](container-instances-quotas.md) makale için geçerli kotalar ve sınırlayan isteği ile artırılabilir.

### <a name="can-i-deploy-with-more-than-4-cores-and-16-gb-of-ram"></a>4'ten fazla çekirdeğe ve 16 GB RAM ile dağıtabilir miyim?

Henüz değil. Şu anda, bir kapsayıcı grubu için sınırları şunlardır. Belirli gereksinimleri veya isteği ile Azure desteğine başvurun. 

### <a name="when-will-aci-be-in-a-specific-region"></a>ACI, belirli bir bölgede ne zaman sunulacaktır?

Geçerli bölge kullanılabilirliği yayımlandığında [burada](container-instances-region-availability.md#availability---general). Belirli bir bölge için bir gereksinimi varsa, Azure desteğine başvurun.

## <a name="features-and-scenarios"></a>Özellikler ve senaryolar

### <a name="how-do-i-scale-a-container-group"></a>Nasıl bir kapsayıcı grubu ölçeklendirme?

Şu anda, ölçeklendirme kapsayıcılar veya kapsayıcı grupları için kullanılabilir değildir. Daha fazla örnek çalıştırmanız gerekiyorsa, API'mizi otomatikleştirin ve daha fazla kapsayıcı grubu oluşturma hizmeti istekleri oluşturmak için kullanın. 

### <a name="what-features-are-available-to-instances-running-in-a-custom-vnet"></a>Özel bir Vnet'te çalışan örnekler için hangi özelliklerin kullanılabilir?

Tercih ettiğiniz bir Azure sanal ağında kapsayıcılı grupları dağıtma ve özel IP'ler kapsayıcı grupları, Azure kaynaklarınızda sanal ağ içinde trafiği yönlendirmek için temsilci. Kapsayıcı grubu dağıtımının bir sanal ağa şu anda Önizleme aşamasındadır ve bu özellik bazı yönleri genel kullanıma (GA) açılmadan önce değişebilir. Bkz: [Önizleme sınırlamaları](container-instances-vnet.md#preview-limitations) güncel bilgiler.

## <a name="pricing"></a>Fiyatlandırma

### <a name="when-does-the-meter-start-running"></a>Ölçüm çalıştıran ne zaman başlar?

Kapsayıcı grubu süresi (yeni bir dağıtım için) ilk kapsayıcınızın görüntüsünü çekmeye başlayın ya da kapsayıcı grubunun durduruluncaya kadar kapsayıcı grubunuzun (zaten dağıtıldıysa) yeniden süreye göre hesaplanır. Ayrıntıları görmek [Container Instances fiyatlandırma](https://azure.microsoft.com/pricing/details/container-instances/).

### <a name="do-i-stop-being-charged-when-my-containers-are-stopped"></a>My kapsayıcıları durdurulduğunda ücretlendirildiğiniz Durdur?

Ölçüm, tüm kapsayıcı grubunuzun durdurulduktan sonra çalışmayı durdurur. Kapsayıcı grubundaki kapsayıcı çalıştırma sürece, biz kaynakları kapsayıcıları yeniden başlatmak istediğiniz durumda tutun. 

## <a name="next-steps"></a>Sonraki adımlar

* [Daha fazla bilgi edinin](container-instances-overview.md) Azure Container Instances hakkında bilgi.
* [Sık karşılaşılan sorunları giderme](container-instances-troubleshooting.md) Azure Container ınstances'da.