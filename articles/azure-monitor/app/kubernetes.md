---
title: Azure İzleyici - Kubernetes Service için Application Insights mesh Istio | Microsoft Docs
description: Application Insight Kubernetes için gelen ve giden istekleri adlı hizmeti kafes teknolojisini kullanarak Kubernetes kümenizde çalışan pod'ların gelen ve ilgili Application Insights telemetri toplamak izin veren bir izleme çözümüdür Istio.
services: application-insights
author: tokaplan
manager: carmonm
ms.service: application-insights
ms.topic: conceptual
ms.date: 04/25/2019
ms.author: alkaplan
ms.openlocfilehash: f3b278c2678542ec127c1c644cc0267622ca39fa
ms.sourcegitcommit: e7d4881105ef17e6f10e8e11043a31262cfcf3b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2019
ms.locfileid: "64870695"
---
# <a name="application-insights-for-kubernetes-with-service-mesh"></a>Ağ Hizmeti ile Kubernetes için Application Insights

> [!IMPORTANT]
> Kubernetes hizmeti kafes aracılığıyla için Application Insights şu anda genel önizlemede.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure İzleyici hizmeti kafes teknik Kubernetes kümenizde kutusu uygulama herhangi bir barındırılan Kubernetes uygulamasını izleme dışında sağlamak artık yararlanır. Varsayılan gibi Application Insight Özellikler [Uygulama Haritası](../../azure-monitor/app/app-map.md) bağımlılıklarınızı, model [Canlı ölçümleri Stream](../../azure-monitor/app/live-stream.md) gerçek zamanlı izleme, güçlü görselleştirmelerle [varsayılan Pano](../../azure-monitor/app/overview-dashboard.md), [ölçüm Gezgini'nde](../../azure-monitor/platform/metrics-getting-started.md), ve [çalışma kitapları](../../azure-monitor/app/usage-workbooks.md). Bu özellik, Kubernetes iş yüklerini seçili bir Kubernetes ad alanı içindeki tüm kullanıcılar nokta performans sorunlarını ve hata noktalarına yardımcı olur. Azure İzleyici mevcut hizmet kafes yatırımlarınızı Istio gibi teknolojilerle üzerinde büyük harf yaparak, uygulamanızın kodunda herhangi bir değişiklik yapılmadan otomatik izleme eklenmiş uygulama izleme sağlar.

> [!NOTE]
> Bu, Kubernetes üzerinde uygulama izleme yapmak için birçok yolu biridir. Kubernetes kullanarak barındırılan herhangi bir uygulama da işaretleyebilir [Application Insights SDK'sı](../../azure-monitor/azure-monitor-app-hub.md) hizmet kafes gerek kalmadan. Kubernetes ile kullanabileceğiniz bir SDK uygulamada ölçümlü izleme olmadan izlemek için aşağıdaki yöntemi.

## <a name="prerequisites"></a>Önkoşullar

- A [Kubernetes kümesi](https://docs.microsoft.com/azure/aks/concepts-clusters-workloads).
- Konsolunda çalıştırmak için kümeye erişim *kubectl*.
- Bir [Application Insight kaynak](create-new-resource.md)
- Bir hizmet kafes vardır. Kümenize dağıtılan Istio yoksa, şunları öğrenebilirsiniz nasıl [yükleyip Istio Azure Kubernetes Service'teki](https://docs.microsoft.com/azure/aks/istio-install).

## <a name="capabilities"></a>Özellikler

Barındırılan Kubernetes uygulamanız için Application Insights kullanarak kullanmanız mümkün olacaktır:

- [Uygulama Eşlemesi](../../azure-monitor/app/app-map.md)
- [Stream Canlı ölçümleri](../../azure-monitor/app/live-stream.md)
- [Panolar](../../azure-monitor/app/overview-dashboard.md)
- [Ölçüm Gezgini](../../azure-monitor/platform/metrics-getting-started.md)
- [Dağıtılmış izleme](../../azure-monitor/app/distributed-tracing.md)
- [Uçtan uca işlem izleme](../../azure-monitor/learn/tutorial-performance.md#identify-slow-server-operations)

## <a name="installation-steps"></a>Yükleme adımları

Çözümü etkinleştirmek için biz aşağıdaki adımları gerçekleştiriyor:
- (Zaten dağıtıldıysa) uygulamayı dağıtın.
- Uygulama hizmeti kafes parçası olduğundan emin olun.
- Toplanan telemetri gözlemleyin.

### <a name="configure-your-app-to-work-with-a-service-mesh"></a>Bir hizmet kafes ile çalışmak için uygulamanızı yapılandırın

Istio destekleyen iki yöntemle [bir pod ölçümlü izleme yapma](https://istio.io/docs/setup/kubernetes/additional-setup/sidecar-injection/).
Çoğu durumda, uygulamanızla birlikte içeren bir Kubernetes ad alanı işaretlemek kolay *istio ekleme* etiketi:

```console
kubectl label namespace <my-app-namespace> istio-injection=enabled
```

> [!NOTE]
> Hizmet kafes asansörler veri kablo kapalı olduğundan, biz şifrelenmiş trafik müdahale edemez. Küme bırakmamasını trafiği için şifrelenmemiş bir protokol (örneğin, HTTP) kullanın. Şifrelenmelidir dış trafiğini göz önünde bulundurun [SSL sonlandırma ayarı](https://kubernetes.io/docs/concepts/services-networking/ingress/#tls) giriş denetleyicisine.

Hizmet kafes dışında çalışan uygulamalar etkilenmez.

### <a name="deploy-your-application"></a>Uygulamanızı dağıtma

- Uygulamanızı dağıtmak *uygulama ad Alanım* ad alanı. Uygulama zaten dağıtılmışsa ve yukarıda açıklanan otomatik sepet ekleme yöntemi izlediyseniz, pod'ları, sepet Istio eklediği emin olmak için yeniden oluşturmanız gerekecek; sıralı güncelleştirme başlatın veya tek tek pod'ları silin ve yeniden oluşturulması bekleyin.
- Uygulamanızı uyumlu ile olun [Istio gereksinimleri](https://istio.io/docs/setup/kubernetes/prepare/requirements/).

### <a name="deploy-application-insights-for-kubernetes"></a>Kubernetes için Application Insights'ı dağıtma

1. İndirin ve ayıklayın bir [ *Kubernetes için Application Insights* yayın](https://github.com/Microsoft/Application-Insights-Istio-Adapter/releases/).
2. Gidin */src/kubernetes/* yayın klasörün içindeki.
3. Düzen *application-insights-istio-mixer-adapter-deployment.yaml*
    - değeri Düzenle *ISTIO_MIXER_PLUGIN_AI_INSTRUMENTATIONKEY* telemetri içerecek şekilde, Azure portalındaki Application Insights kaynağına ait izleme anahtarını içeren ortam değişkeni.
    - Gerekirse, değerini düzenlemek *ISTIO_MIXER_PLUGIN_WATCHLIST_NAMESPACES* ad alanları için istediğiniz izlemeyi etkinleştirmek, virgülle ayrılmış listesini içeren ortam değişkeni. Tüm ad alanlarını izlemek için boş bırakın.
4. Uygulama *her* YAML dosyası bulundu altında *src/kubernetes/* aşağıdakileri çalıştırarak (hala içinde olmalıdır */src/kubernetes/*):

   ```console
   kubectl apply -f .
   ```

### <a name="verify-application-insights-for-kubernetes-deployment"></a>Kubernetes dağıtımı için Application Insights doğrulayın

- Kubernetes bağdaştırıcısı için Application Insights dağıtılabilir emin olun:

  ```console
  kubectl get pods -n istio-system -l "app=application-insights-istio-mixer-adapter"
  ```
> [!NOTE]
> Bazı durumlarda, ayarlama ince ayar gereklidir. Dahil etmek veya toplanmakta olan tek bir pod için telemetri hariç tutmak için kullanın *appinsights/monitoring.enabled* bu pod etiketi. Bu, tüm ad alanı tabanlı yapılandırma üzerinde önceliğe sahip olacaktır. Ayarlama *appinsights/monitoring.enabled* için *true* pod dahil etmek ve *false* dışlamak istediğiniz.

### <a name="view-application-insights-telemetry"></a>Application Insights telemetrisini görüntüleme

- Uygulama izleme düzgün çalıştığını doğrulamak için bir örnek isteği oluşturun.
- 3-5 dakika içinde Azure Portalı'nda görünmesi telemetri görmeye başlamanız gerekir. Kullanıma mutlaka *Uygulama Haritası* portalında Application Insights kaynağınıza bölümü.

## <a name="troubleshooting"></a>Sorun giderme

Aşağıdaki sorun giderme akış telemetri Azure portalında görünmüyor kullanılacak bekleniyor.

1. Uygulama yük altındayken ve gönderme/düz HTTP istekleri alma emin olun. Telemetri, kablo yükseltilmiş olduğundan, şifrelenmiş trafik desteklenmiyor. Gelen veya giden isteği yoksa olacaktır telemetri yok ya da.
2. Doğru izleme anahtarını sağlandığından emin olmak *ISTIO_MIXER_PLUGIN_AI_INSTRUMENTATIONKEY* ortam değişkeninde *application-insights-istio-mixer-adapter-deployment.yaml*. İzleme anahtarı bulunur *genel bakış* Azure portalında Application Insights kaynağı sekmesi.
3. Doğru Kubernetes ad alanı sağlanır olun *ISTIO_MIXER_PLUGIN_WATCHLIST_NAMESPACES* ortam değişkeninde *application-insights-istio-mixer-adapter-deployment.yaml*. Tüm ad alanlarını izlemek için boş bırakın.
4. Uygulamanızın pod'ları tarafından Istio sepet olarak eklenen olun. Sepet Istio'nın her pod var olduğunu doğrulayın.

   ```console
   kubectl describe pod -n <my-app-namespace> <my-app-pod-name>
   ```
   Adlı bir kapsayıcı olduğundan emin olun *istio proxy* pod'u çalıştıran.

5. Görünüm *Kubernetes için Application Insights* bağdaştırıcının izler.

   ```console
   kubectl get pods -n istio-system -l "app=application-insights-istio-mixer-adapter"
   kubectl logs -n istio-system application-insights-istio-mixer-adapter-<fill in from previous command output>
   ```

   Alınan telemetri öğelerinin sayısını dakikada bir kez güncelleştirilir. Bu dakika dakika içinde - değil geçerseniz telemetri yok Istio tarafından bağdaştırıcısına gönderiliyor.
   Herhangi bir hata günlüğüne bakın.
6. Bunu kurulmuşsa, *Kubernetes için Application Insight* bağdaştırıcısı yok iletilir ve telemetri, neden, veri bağdaştırıcısına göndermeme anlamaya Istio'nın Mixer günlüklerini kontrol edin:

   ```console
   kubectl get pods -n istio-system -l "istio=mixer,app=telemetry"
   kubectl logs -n istio-system istio-telemetry-<fill in from previous command output> -c mixer
   ```
   Özellikle iletişimleri ile ilgili hataları arayın *applicationinsightsadapter* bağdaştırıcısı.

## <a name="faq"></a>SSS

Bu projede ilerlemesi için en son bilgi için ziyaret [Istio Mixer projenin GitHub için Application Insights bağdaştırıcısı](https://github.com/Microsoft/Application-Insights-Istio-Adapter/blob/master/SETUP.md#faq).

## <a name="uninstall"></a>Kaldırma

Ürünü için kaldırma *her* YAML dosyası bulundu altında *src/kubernetes/* çalıştırın:

```console
kubectl delete -f <filename.yaml>
```


## <a name="next-steps"></a>Sonraki adımlar

Azure İzleyici ve kapsayıcıları birlikte nasıl ziyaret çalıştığı hakkında daha fazla bilgi edinmek için [kapsayıcılara genel bakış için Azure İzleyici](../../azure-monitor/insights/container-insights-overview.md)