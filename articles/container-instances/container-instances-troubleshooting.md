---
title: Azure Container Instances sorunlarını giderme
description: Azure Container Instances ile ilgili sorunları giderme hakkında bilgi edinin
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/15/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: bf783c988c0163fe562669a8331c332dbf8d535e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "61067350"
---
# <a name="troubleshoot-common-issues-in-azure-container-instances"></a>Azure Container ınstances'da yaygın sorunlarını giderme

Bu makalede, Azure Container Instances'a kapsayıcıları dağıtma veya yönetmek için yaygın sorunlarını giderme gösterilmektedir.

## <a name="naming-conventions"></a>Adlandırma kuralları

Kapsayıcı kuruluma tanımlarken belirli parametreleri için adlandırma kısıtlamaları bağlılığı gerektirir. Aşağıda bir kapsayıcı için belirli gereksinimler Grup Özellikleri tablodur. Azure adlandırma kuralları hakkında daha fazla bilgi için bkz. [adlandırma kuralları] [ azure-name-restrictions] Azure mimari Merkezi'ne.

| Kapsam | Uzunluk | Büyük/Küçük Harf Kullanımı | Geçerli karakterler | Önerilen düzen | Örnek |
| --- | --- | --- | --- | --- | --- |
| Kapsayıcı grubu adı | 1-64 |Büyük/Küçük harfe duyarsız |İlk veya son karakter alfasayısal ve kısa çizgi herhangi bir yere hariç |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Kapsayıcı adı | 1-64 |Büyük/Küçük harfe duyarsız |İlk veya son karakter alfasayısal ve kısa çizgi herhangi bir yere hariç |`<name>-<role>-CG<number>` |`web-batch-CG1` |
| Kapsayıcı bağlantı noktaları | 1 ile 65535 arasında |Tamsayı |1 ile 65535 arasında bir tamsayı |`<port-number>` |`443` |
| DNS ad etiketi | 5-63 |Büyük/Küçük harfe duyarsız |İlk veya son karakter alfasayısal ve kısa çizgi herhangi bir yere hariç |`<name>` |`frontend-site1` |
| Ortam değişkeni | 1-63 |Büyük/Küçük harfe duyarsız |İlk veya son karakter alfasayısal ve alt çizgi (_) herhangi bir yere hariç |`<name>` |`MY_VARIABLE` |
| Birim adı | 5-63 |Büyük/Küçük harfe duyarsız |Küçük harf ve sayı ve kısa çizgi herhangi bir yere ilk veya son karakter hariç. Art arda iki kısa çizgi içeremez. |`<name>` |`batch-output-volume` |

## <a name="os-version-of-image-not-supported"></a>Desteklenmeyen işletim sistemi sürümü

Azure Container Instances desteklemeyen, görüntü belirtirseniz bir `OsVersionNotSupported` hata döndürülür. Ardından, nerede benzer bir hatadır `{0}` dağıtmaya çalıştığınız görüntünün adı:

```json
{
  "error": {
    "code": "OsVersionNotSupported",
    "message": "The OS version of image '{0}' is not supported."
  }
}
```

Bu hata, bir yarı yıllık kanal (SAC) üzerinde tabanlı dağıtımı Windows görüntüleri bıraktığınızda en sık karşılaşılır. Örneğin, Windows sürüm 1709 ve 1803 SAC sürümlerdir ve dağıtım sırasında bu hata oluşturur.

Azure Container Instances, şu anda yalnızca temel Windows görüntüleri destekler **Windows Server 2016 uzun süreli bakım kanalı (LTSC)** bırakın. Windows kapsayıcıları dağıtırken bu sorunu gidermek için her zaman Windows Server 2016 LTSC tabanlı görüntüler dağıtın. Windows Server 2019 (LTSC) dayalı görüntüleri desteklenmez.

Windows LTSC ve SAC sürümleri hakkında daha fazla bilgi için bkz. [Windows Server yarı yıllık kanal genel bakış][windows-sac-overview].

## <a name="unable-to-pull-image"></a>Çekme görüntüsü oluşturulamıyor

Azure Container Instances, görüntü çekmek başlangıçta kuramazsa, bir süre için yeniden dener. Görüntü çekme işlemi başarısız olmaya devam eder, ACI dağıtım sonunda başarısız olur ve görebileceğiniz bir `Failed to pull image` hata.

Bu sorunu çözmek için kapsayıcı örneğini silmek ve dağıtımınız yeniden deneyin. Görüntüyü kayıt defterinde var olduğundan ve görüntü adını doğru girdiğinizden emin olun.

Görüntü çekilen, olayları aşağıdaki gibi çıktısında gösterilen [az container show][az-container-show]:

```bash
"events": [
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "Pulling",
    "type": "Normal"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:19+00:00",
    "lastTimestamp": "2017-12-21T22:57:00+00:00",
    "message": "Failed to pull image \"mcr.microsoft.com/azuredocs/aci-hellowrld\": rpc error: code 2 desc Error: image t/aci-hellowrld:latest not found",
    "name": "Failed",
    "type": "Warning"
  },
  {
    "count": 3,
    "firstTimestamp": "2017-12-21T22:56:20+00:00",
    "lastTimestamp": "2017-12-21T22:57:16+00:00",
    "message": "Back-off pulling image \"mcr.microsoft.com/azuredocs/aci-hellowrld\"",
    "name": "BackOff",
    "type": "Normal"
  }
],
```

## <a name="container-continually-exits-and-restarts-no-long-running-process"></a>Kapsayıcı sürekli olarak çıkar ve yeniden başlatılır (uzun süren işlem)

Kapsayıcı grupları için varsayılan değer bir [yeniden başlatma ilkesi](container-instances-restart-policy.md) , **her zaman**, bunlar tamamlanana kadar çalıştırdıktan sonra kapsayıcı grubundaki kapsayıcı her zaman yeniden başlatın. Bu değişiklik yapmanız gerekebilir **OnFailure** veya **hiçbir zaman** görev-tabanlı kapsayıcıları çalıştırmak istiyorsanız. Belirtirseniz **OnFailure** ve hala bakın sürekli yeniden başlatıldıktan sonra uygulama veya betik kapsayıcınızda yürütülen bir sorun olabilir.

Kapsayıcı grupları olmadan uzun süre çalışan işlemleri çalıştırırken, yinelenen çıkar ve Ubuntu gibi veya Alpine görüntülerle yeniden görebilirsiniz. Aracılığıyla bağlanan [EXEC](container-instances-exec.md) kapsayıcı canlı tutma hiçbir işlem olarak çalışmıyor. Bu sorunu çözmek için kapsayıcıyı tutmak için kapsayıcı grubu dağıtımınız ile aşağıdaki gibi bir başlangıç komutu içerir.

```azurecli-interactive
## Deploying a Linux container
az container create -g MyResourceGroup --name myapp --image ubuntu --command-line "tail -f /dev/null"
```

```azurecli-interactive 
## Deploying a Windows container
az container create -g myResourceGroup --name mywindowsapp --os-type Windows --image mcr.microsoft.com/windows/servercore:ltsc2016
 --command-line "ping -t localhost"
```

Kapsayıcı örnekleri API ve Azure portaldaki bir `restartCount` özelliği. Bir kapsayıcı için yeniden başlatma sayısını denetlemek için kullanabileceğiniz [az container show] [ az-container-show] Azure CLI komutu. Aşağıdaki örnekte, (olduğu için kısaltma kesildi) çıktı gördüğünüz `restartCount` çıktının sonunda özelliği.

```json
...
 "events": [
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:06+00:00",
     "lastTimestamp": "2017-11-13T21:20:06+00:00",
     "message": "Pulling: pulling image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Pulled: Successfully pulled image \"myregistry.azurecr.io/aci-tutorial-app:v1\"",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Created: Created container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   },
   {
     "count": 1,
     "firstTimestamp": "2017-11-13T21:20:14+00:00",
     "lastTimestamp": "2017-11-13T21:20:14+00:00",
     "message": "Started: Started container with id bf25a6ac73a925687cafcec792c9e3723b0776f683d8d1402b20cc9fb5f66a10",
     "type": "Normal"
   }
 ],
 "previousState": null,
 "restartCount": 0
...
}
```

> [!NOTE]
> Linux dağıtımları için çoğu kapsayıcı görüntülerini bash gibi bir kabuk varsayılan komut olarak ayarlayın. Kendi kendine bir kabuk uzun süre çalışan bir hizmeti olmadığından, bu kapsayıcılar hemen çıkmak ve varsayılan yapılandırıldığında bir yeniden başlatma döngüyü kalan **her zaman** yeniden başlatma ilkesi.

## <a name="container-takes-a-long-time-to-start"></a>Kapsayıcı başlatmak uzun sürüyor

Azure Container ınstances'da kapsayıcı başlangıç zamanını katkıda iki temel unsurlar şunlardır:

* [Görüntü boyutu](#image-size)
* [Görüntü konumu](#image-location)

Windows görüntüleri olan [ek hususlar](#cached-windows-images).

### <a name="image-size"></a>Görüntü boyutu

Kapsayıcınızı başlatma, ancak sonuçta uzun sürüyorsa başarılı, kapsayıcı görüntünüzü boyutta bakarak başlayın. Azure Container Instances'a kapsayıcı görüntünüzü isteğe bağlı olarak çeker. çünkü gördüğünüz başlangıç zamanını boyutuna doğrudan ilgilidir.

Kapsayıcı görüntünüzü boyutunu kullanarak görüntüleyebileceğiniz `docker images` Docker CLI komutunu:

```console
$ docker images
REPOSITORY                                    TAG       IMAGE ID        CREATED          SIZE
mcr.microsoft.com/azuredocs/aci-helloworld    latest    7367f3256b41    15 months ago    67.6MB
```

Resim boyutları küçük tutmak için anahtar son görüntünüzü çalışma zamanında gerekli değil herhangi bir şey içermediğinden emin olmaktır. Yapmanın bir yolu bu olan [çok aşamalı yapılar][docker-multi-stage-builds]. Çok aşamalı son görüntü yalnızca uygulamanız için gereken yapıtları içerir ve hiçbir ek içerik sağlamak kolay, oluşturma zamanında gerekli yap oluşturur.

### <a name="image-location"></a>Görüntü konumu

Başka bir yolu, kapsayıcının başlangıç zamanında görüntü çekme etkisini azaltmak, kapsayıcı görüntüsü barındırmaktır [Azure Container Registry](/azure/container-registry/) container ınstances'a dağıtmayı planladığınız burada aynı bölgede. Bu, kapsayıcı görüntüsü seyahat gereken ağ yolunu karşıdan yükleme süresini önemli ölçüde kısaltmak kısaltır.

### <a name="cached-windows-images"></a>Önbelleğe alınan Windows görüntüleri

Azure Container Instances, ortak Windows ve Linux görüntüleri temel alan görüntüler için kapsayıcı başlatma süresini hızlandırmak için bir önbelleğe alma mekanizması kullanır. Önbelleğe alınan görüntüleri ve etiketleri ayrıntılı listesi için kullanmak [önbelleğe alınmış görüntüleri listeleme] [ list-cached-images] API.

Windows kapsayıcı başlangıç süreye sağlamak için aşağıdakilerden birini kullanın: **en son üç** aşağıdaki sürümleri **iki görüntü** temel görüntü olarak:

* [Windows Sunucu Çekirdeği 2016] [ docker-hub-windows-core] (yalnızca LTSC)
* [Windows Server 2016 Nano Server][docker-hub-windows-nano]

### <a name="windows-containers-slow-network-readiness"></a>Windows kapsayıcıları yavaş bir ağ hazırlığı

İlk oluşturma, Windows kapsayıcıları, 30 saniyeye kadar (veya nadir durumlarda daha uzun) gelen veya giden bağlantı olabilir. Kapsayıcı uygulamanızı Internet bağlantısı gerektiriyorsa, gecikme ekleyin ve yeniden deneme mantığı, Internet bağlantısı kurmak 30 saniye izin vermek için. İlk Kurulumdan sonra kapsayıcı ağ iletişimi uygun şekilde devam edecektir.

## <a name="resource-not-available-error"></a>Kaynak kullanılamıyor hatası

Azure'da yük bölgesel kaynak değişen nedeniyle, bir kapsayıcı örneği dağıtmak çalışırken şu hatayı alabilirsiniz:

`The requested resource with 'x' CPU and 'y.z' GB memory is not available in the location 'example region' at this moment. Please retry with a different resource request or in another location.`

Bu hata, kapsayıcınız için belirtilen kaynaklara o anda ayrılamıyor dağıtmaya çalıştığınız bölgede ağır yük nedeniyle gösterir. Sorununuzu çözmenize yardımcı olması için bir veya daha fazla aşağıdaki risk azaltma adımlarını kullanın.

* Kapsayıcı dağıtım ayarlarınızı kalan içinde tanımlanan parametreleri doğrulayın [Azure Container Instances için bölge kullanılabilirliği](container-instances-region-availability.md)
* Kapsayıcı için daha düşük CPU ve bellek ayarlarını belirtin
* Farklı bir Azure bölgesine dağıtın
* Daha sonraki bir zamanda dağıtmak

## <a name="cannot-connect-to-underlying-docker-api-or-run-privileged-containers"></a>Temel alınan Docker API'ye bağlanmak veya ayrıcalıklı kapsayıcıları çalıştırmak olamaz

Azure Container Instances'a kapsayıcı grupları barındıran altyapının doğrudan erişim kullanıma sunmuyor. Bu Docker kapsayıcının konakta çalışan ve çalışan kapsayıcılar ayrıcalıklı API erişimi içerir. Docker etkileşimi gerektiriyorsa, kontrol [REST başvuru belgeleri](https://aka.ms/aci/rest) ne ACI API'sini destekleyen görmek için. Varsa eksik bir şeyler üzerinde talebinizi [ACI geri bildirim forumları](https://aka.ms/aci/feedback).

## <a name="ips-may-not-be-accessible-due-to-mismatched-ports"></a>IP'ler eşleşmeyen bağlantı noktaları nedeniyle erişilebilir olmayabilir

Bu düzeltme yol haritasında ancak azure Container Instances şu anda normal docker yapılandırmasıyla gibi bağlantı noktası eşleme desteklemez. IP'ler olmayan erişilebilir olmalıdır düşündüğünüzde bulursanız, kapsayıcı grubu içinde ortaya çıkarttığınız aynı bağlantı noktalarını dinlemek için kapsayıcı görüntünüzü yapılandırdığınızdan emin olun `ports` özelliği.

## <a name="next-steps"></a>Sonraki adımlar

Bilgi nasıl [kapsayıcı günlüklerini ve olayları almak](container-instances-get-logs.md) kapsayıcılarınızı hata ayıklama yardımcı olmak için.

<!-- LINKS - External -->
[azure-name-restrictions]: https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions#naming-rules-and-restrictions
[windows-sac-overview]: https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview
[docker-multi-stage-builds]: https://docs.docker.com/engine/userguide/eng-image/multistage-build/
[docker-hub-windows-core]: https://hub.docker.com/_/microsoft-windows-servercore
[docker-hub-windows-nano]: https://hub.docker.com/_/microsoft-windows-nanoserver

<!-- LINKS - Internal -->
[az-container-show]: /cli/azure/container#az-container-show
[list-cached-images]: /rest/api/container-instances/listcachedimages
