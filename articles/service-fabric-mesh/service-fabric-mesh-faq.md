---
title: Azure Service Fabric Mesh için sık sorulan sorular | Microsoft Docs
description: Azure Service Fabric Mesh için sık sorulan sorular ve yanıtlar hakkında bilgi edinin.
services: service-fabric-mesh
keywords: ''
author: chackdan
ms.author: chackdan
ms.date: 4/23/2019
ms.topic: troubleshooting
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: 950f9ac89b9d3224db29b32fe2d1e403ccc98116
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143284"
---
# <a name="commonly-asked-service-fabric-mesh-questions"></a>Sık sorulan sorular Service Fabric Mesh

Azure Service Fabric Mesh, geliştiricilerin sanal makineleri, depolama alanını veya ağ bileşenlerini yönetmeden mikro hizmet uygulamaları dağıtmasını sağlayan tam olarak yönetilen bir hizmettir. Bu makalede sık sorulan soruların yanıtları bulunur.

## <a name="how-do-i-report-an-issue-or-ask-a-question"></a>Nasıl bir sorun bildirin veya soru sormak?

Soru sorun, Microsoft mühendisinin cevaplar ve raporlayın [service-fabric-kafes-preview GitHub deposunu](https://aka.ms/sfmeshissues).

## <a name="quota-and-cost"></a>Kota ve maliyet

### <a name="what-is-the-cost-of-participating-in-the-preview"></a>Önizlemede katılım maliyeti nedir?

Şu anda kafes Önizleme uygulamalar veya kapsayıcıları dağıtmak için ücretlendirme yoktur. Lütfen etkinleştirme faturalandırma için Mayıs ayında güncelleştirmeleri izleyin. Dağıtma ve bunları bırakmamaya kaynakları silmeniz etmenizi öneririz ancak bunları test etkin bir şekilde ettiğiniz sürece çalışıyor.

### <a name="is-there-a-quota-limit-of-the-number-of-cores-and-ram"></a>Çekirdek sayısını ve RAM'i kota sınırı var mı?

Evet. Her abonelik için kotalar şunlardır:

- Uygulama sayısı: 5
- Uygulama başına çekirdek sayısı: 12
- Uygulama başına toplam RAM: 48 GB
- Ağ ve giriş uç noktaları: 5
- Ekleyebileceğiniz azure birimler: 10
- Hizmet yineleme sayısı: 3
- 4 çekirdek ve 16 GB RAM, dağıtabileceğiniz en büyük kapsayıcı sınırlıdır.
- 0,5 çekirdek, en çok 6 çekirdek artışlarla kapsayıcılarınızı kısmi çekirdek ayırabilirsiniz.

### <a name="how-long-can-i-leave-my-application-deployed"></a>Dağıtılan uygulamamı ne kadar süreyle bildirebilirim?

Biz, şu anda iki gün için bir uygulamanın ömrü sınırladınız. Önizleme için ayrılan serbest çekirdeğe kullanımını en üst düzeye çıkarmak budur. Sonuç olarak, yalnızca belirli bir dağıtım 48 saat için sürekli olarak çalışmasına izin verilen sonra da kapatın, geçen süre.

Bunu görürseniz, sistem çalıştırarak kapatabilirler olduğunu doğrulayabilirsiniz `az mesh app show` Azure CLI komutu. Döndürüp döndürmediğini görmek için işaretleyin `"status": "Failed", "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue."` 

Örneğin: 

```cli
~$ az mesh app show --resource-group myResourceGroup --name helloWorldApp
{
  "debugParams": null,
  "description": "Service Fabric Mesh HelloWorld Application!",
  "diagnostics": null,
  "healthState": "Ok",
  "id": "/subscriptions/1134234-b756-4979-84re-09d671c0c345/resourcegroups/myResourceGroup/providers/Microsoft.ServiceFabricMesh/applications/helloWorldApp",
  "location": "eastus",
  "name": "helloWorldApp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "serviceNames": [
    "helloWorldService"
  ],
  "services": null,
  "status": "Failed",
  "statusDetails": "Stopped resource due to max lifetime policies for an application during preview. Delete the resource to continue.",
  "tags": {},
  "type": "Microsoft.ServiceFabricMesh/applications",
  "unhealthyEvaluation": null
}
```

Kaynak grubunu silmek için kullanın `az group delete <nameOfResourceGroup>` komutu.

## <a name="deployments"></a>Dağıtımlar

### <a name="what-container-images-are-supported"></a>Hangi kapsayıcı görüntülerinin destekleniyor mu?

Windows Fall Creators Update (1709 sürümü) makinesinde geliştiriyorsanız, yalnızca Windows sürüm 1709 docker görüntülerini kullanabilirsiniz.

Bir Windows üzerinde geliştirme yapıyorsanız 10 Nisan 2018 Güncelleştirmesi (sürüm 1803) makine, Windows sürüm 1709 veya Windows sürümü 1803 docker görüntülerini kullanabilirsiniz.

Aşağıdaki kapsayıcı işletim sistemi görüntüleri, hizmetleri dağıtmak için kullanılabilir:

- Windows - windowsservercore ve nanoserver
    - Windows Server 1709
    - Windows Server 1803
    - Windows Server 1809
    - Windows Server 2019 LTSC
- Linux
    - Hiçbir bilinen sınırlamalar

> [!NOTE]
> Visual Studio için Kafes araç, Windows Server 2019 ve 1809 kapsayıcıları dağıtma henüz desteklemiyor.

### <a name="what-types-of-applications-can-i-deploy"></a>Hangi tür uygulama miyim dağıtabilir miyim? 

Uygulama kaynağı (kotaları hakkında daha fazla bilgi için yukarıya bakın) kısıtlamalara uyan kapsayıcıları çalışır yerleştirildiği herhangi bir şey dağıtabilirsiniz. Belirlersek kafes geçersiz iş yüklerini çalıştırmak için kullandığınız ya da (yani araştırma) sistemi ayrıcalıkların, ardından dağıtımları ve engelleme listesi hizmette çalışmasını aboneliğinizi sonlandırmak için hakkımızı saklı tutarız. Belirli bir iş yükü çalıştırma herhangi bir sorunuz varsa lütfen bize ulaşın. 

## <a name="developer-experience-issues"></a>Geliştirici deneyimi sorunları

### <a name="dns-resolution-from-a-container-doesnt-work"></a>DNS çözümlemesi bir kapsayıcısından çalışmıyor

Service Fabric DNS hizmeti için bir kapsayıcı yapılan DNS sorgularının giden belirli koşullar altında başarısız olabilir. Bu incelenmektedir. Azaltmak için:

- Windows Fall Creators update (1709 sürümü) kullanın veya temel kapsayıcı görüntünüzü olarak daha yüksek.
- Tek başına bir hizmet adı işe yaramazsa, tam adı deneyin: ServiceName.ApplicationName.
- Hizmetiniz için Docker dosyasına ekleyin `EXPOSE <port>` bağlantı noktası, bağlantı noktası olduğunda, hizmetinizi üzerinde karşı savunmasız bırakıyorsunuz. Örneğin:

```Dockerfile
EXPOSE 80
```

### <a name="dns-does-not-work-the-same-as-it-does-for-service-fabric-development-clusters-and-in-mesh"></a>Service Fabric geliştirme kümeleri ve kafes yaptığı gibi DNS aynı çalışmıyor.

Hizmetleri yerel geliştirme kümenizde Azure Mesh içinde farklı başvurmanız gerekir.

Yerel geliştirme kümenizde kullanmak `{serviceName}.{applicationName}`. Azure Service Fabric Mesh, kullanın `{servicename}`. 

Azure ağı uygulamalarda DNS çözümlemesi şu anda desteklemiyor.

Windows 10'da bir Service Fabric geliştirme kümesi çalıştıran ile diğer bilinen DNS sorunları için bkz: [Windows kapsayıcıları hata ayıklama](/azure/service-fabric/service-fabric-how-to-debug-windows-containers) ve [bilinen DNS sorunları](https://docs.microsoft.com/azure/service-fabric/service-fabric-dnsservice#known-issues).

### <a name="networking"></a>Ağ

ServiceFabric ağ NAT uygulamanızı yerel makinenizde çalışan kullanırken kaybolabilir. Bu durumun geçerli olup olmadığını tanılamak için bir komut isteminden aşağıdaki komutu çalıştırın:

`docker network ls` Not olmadığını `servicefabric_nat` listelenir.  Aksi durumda, ardından aşağıdaki komutu çalıştırın: `docker network create -d=nat --subnet 10.128.0.0/24 --gateway 10.128.0.1 servicefabric_nat`

Uygulama zaten yerel olarak ve kötü bir durumda dağıtılması durumunda bile bu sorunu ele alınacaktır.

### <a name="issues-running-multiple-apps"></a>Birden fazla uygulama çalıştıran sorunları

CPU kullanılabilirlik ve tüm uygulamalar arasında düzeltilmekte olan sınırları karşılaşabilirsiniz. Azaltmak için:
- Beş düğümlü bir küme oluşturun.
- Dağıtılan uygulama Hizmetleri'nde CPU kullanımı azaltın. Örneğin, hizmetinizin service.yaml dosyasında değişiklik `cpu: 1.0` için `cpu: 0.5`

Bir tek düğümlü kümeye birden çok uygulama dağıtılamaz. Azaltmak için:
- Birden fazla uygulama için yerel bir küme dağıtılırken beş düğümlü bir küme kullanın.
- Değil, şu anda sınadığınız uygulamaları kaldırın.

### <a name="vs-tooling-has-limited-support-for-windows-containers"></a>Windows kapsayıcıları için destek araçları VS sınırlıdır

Visual Studio Araçları, yalnızca bir temel işletim sistemi sürümü Windows Server 1709 ve 1803 bugün Windows kapsayıcıları dağıtma destekler. 

## <a name="feature-gaps-and-other-known-issues"></a>Özellik boşluklarına ve ilgili bilinen diğer sorunlar

### <a name="after-deploying-my-application-the-network-resource-associated-with-it-does-not-have-an-ip-address"></a>Uygulamamın dağıttıktan sonra kendisiyle ilişkili bir ağ kaynağına bir IP adresi yok

Hangi IP adresini hemen kullanılabilir olmaz bilinen bir sorun yoktur. İlişkili IP adresi görmek için birkaç dakika içinde ağ kaynak durumunu denetleyin.

### <a name="my-application-fails-to-access-the-right-networkvolume-resource"></a>Uygulamamın doğru ağ/birim kaynağına erişemiyor

Uygulama modelinizde, ilişkili kaynak erişebilmesi için ağları ve birimler için tam kaynak Kimliğini kullanın. Hızlı Başlangıç örnek bir örnek burada verilmiştir:

```json
"networkRefs": [
    {
    "name":  "[resourceId('Microsoft.ServiceFabric/networks', 'SbzVotingNetwork')]" 
    }
]
```

### <a name="when-i-scale-out-all-of-my-containers-are-affected-including-running-ones"></a>Çıkış ölçeklediğinizde tüm my kapsayıcılar, olanları çalıştırmak dahil etkilenen

Bu bir hatadır ve bir düzeltme uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

Service Fabric Mesh hakkında daha fazla bilgi edinmek için [genel bakış](service-fabric-mesh-overview.md).
