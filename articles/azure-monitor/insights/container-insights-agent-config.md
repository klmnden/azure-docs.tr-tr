---
title: Azure İzleyicisi'ni yapılandırmak için kapsayıcı aracısı veri toplama | Microsoft Docs
description: Bu makalede Azure İzleyici stdout/stderr denetlemek kapsayıcı aracısı için yapılandırabilirsiniz ve ortam değişkenlerini toplamayla nasıl açıklanmaktadır.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/22/2019
ms.author: magoedte
ms.openlocfilehash: 1e7506e311c38d87371dd1b65440b6fb41a7ce78
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2019
ms.locfileid: "67341655"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Kapsayıcılar için Azure İzleyici aracı verileri toplamayı yapılandır

Kapsayıcılar için Azure İzleyici kapsayıcı iş yükleri için yönetilen Kubernetes kümelerini kapsayıcı Aracısı'ndan Azure Kubernetes Service (AKS) barındırılan dağıtılan stdout, stderr ve ortam değişkenlerini toplar. Bu deneyim denetlemek için Kubernetes ConfigMaps özel oluşturarak aracısını veri toplama ayarları yapılandırabilirsiniz. Bu makalede ConfigMap oluşturma ve gereksinimlerinize göre veri toplamasını yapılandırmadan gösterilmektedir.

## <a name="configure-your-cluster-with-custom-data-collection-settings"></a>Özel veri toplama ayarları ile kümenizi yapılandırma

Kolayca sıfırdan oluşturmak zorunda kalmadan özelleştirmelerinizle düzenlemenize olanak sağlayan bir şablon ConfigMap dosyası sağlanır. Başlamadan önce Kubernetes belgeleri hakkında gözden geçirmeniz gereken [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) ve oluşturma, yapılandırma ve dağıtma ConfigMaps nasıl ile kendinizi alıştırın. Bu, stderr ve stdout ad alanı başına veya tüm küme ve kümedeki tüm pod'ların / düğümler arasında çalışan herhangi bir kapsayıcı için ortam değişkenlerini filtrelemek olanak tanır.

>[!IMPORTANT]
>Microsoft bu özellik tarafından desteklenen en düşük aracı sürümü, / oms:ciprod06142019 veya üzeri. 

### <a name="overview-of-configurable-data-collection-settings"></a>Yapılandırılabilir veri toplama ayarları genel bakış

Veri toplamayı denetlemek için yapılandırılabilir ayarları şunlardır:

|Anahtar |Veri türü |Value |Açıklama |
|----|----------|------|------------|
|`schema-version` |Dize (büyük/küçük harfe duyarlı) |v1 |Bu, bu ConfigMap ayrıştırılırken aracı tarafından kullanılan şema sürümüdür. Şu anda desteklenen şema sürümü v1 ' dir. Bu değer değiştirme desteklenmez ve ConfigMap değerlendirildiğinde reddedilir.|
|`config-version` |String | | Bu yapılandırma dosyasının sürümü kaynak denetim sistemi/deponuzda izlemek için özelliğini destekler. İzin verilen karakter sayısı üst sınırı 10 olan ve diğer tüm karakterler kesildi. |
|`[log_collection_settings.stdout] enabled =` |Boolean | TRUE veya false | STDOUT kapsayıcı günlük toplama etkinse bu denetler. Ayarlandığında `true` ve ad alanı için stdout günlük toplama hariç tutulan (`log_collection_settings.stdout.exclude_namespaces` ayara bakın), stdout günlükleri tüm pod'ların / küme içindeki düğümler arasında tüm kapsayıcılardan toplanacak. İçinde ConfigMaps belirtilmezse, varsayılan değer: `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|String | Virgülle ayrılmış bir dizi |Kubernetes ad alanları için hangi stdout günlükleri toplanmayacak dizisi. Bu ayar etkilidir yalnızca `log_collection_settings.stdout.enabled` ayarlanır `true`. İçinde ConfigMap belirtilmezse, varsayılan değer: `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Boolean | TRUE veya false |Stderr kapsayıcı günlük toplama etkinse bu denetler. Ayarlandığında `true` ve ad alanı için stdout günlük toplama hariç tutulan (`log_collection_settings.stderr.exclude_namespaces` ayarı), stderr günlüklerini tüm pod'ların / küme içindeki düğümler arasında tüm kapsayıcılardan toplanacak. İçinde ConfigMaps belirtilmezse, varsayılan değer: `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |String |Virgülle ayrılmış bir dizi |Kubernetes ad alanları için hangi stderr günlüklerini toplanmayacak dizisi. Bu ayar etkilidir yalnızca `log_collection_settings.stdout.enabled` ayarlanır `true`. İçinde ConfigMap belirtilmezse, varsayılan değer: `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Boolean | TRUE veya false | Bu ortam değişkeni toplama etkinse denetler. Ayarlandığında `false`, hiçbir ortam değişkenleri tüm pod'ların / küme içindeki düğümler arasında çalışan herhangi bir kapsayıcı için toplanır. İçinde ConfigMap belirtilmezse, varsayılan değer: `enabled = true`. |

### <a name="configure-and-deploy-configmaps"></a>Yapılandırma ve ConfigMaps dağıtma

Yapılandırma ve kümenize ConfigMap yapılandırma dosyanızı dağıtmak için aşağıdaki adımları gerçekleştirin.

1. [İndirme](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) şablon ConfigMap yaml dosyası ve kapsayıcı-azm-ms-agentconfig.yaml kaydedin.  
1. Özelleştirmeleriniz ile ConfigMap yaml dosyasını düzenleyin. 

    - Belirli ad alanları için stdout günlük toplama dışarıda bırakmak için aşağıdaki örneği kullanarak anahtar/değer yapılandırma: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    - Belirli bir kapsayıcı için ortam değişkeni koleksiyonunu devre dışı bırakmak için anahtar/değer kümesi `[log_collection_settings.env_var] enabled = true` değişken koleksiyonu etkinleştirmek ve adımları [burada](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) belirli kapsayıcısı için yapılandırmayı tamamlamak için.
    - Stderr günlük toplama küme çapında devre dışı bırakmak için aşağıdaki örneği kullanarak anahtar/değer yapılandırma: `[log_collection_settings.stderr] enabled = false`.

1. Kubectl aşağıdaki komutu çalıştırarak ConfigMap oluşturun: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Örnek: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Yapılandırma değişikliğinin etkili almadan önce tamamlanması birkaç dakika sürebilir ve tüm omsagent pod'ların kümesinde başlayacak. Yeniden başlatma tüm omsagent pod'ları için sıralı bir yeniden başlatma, tümü aynı anda yeniden başlatın. Yeniden başlatma tamamlandıktan sonra bir ileti aşağıdakine benzer ve sonucu içeren görüntülenir: `configmap "container-azm-ms-agentconfig" created`.

Yapılandırma başarıyla uygulandı doğrulamak için bir aracı pod günlüklerini gözden geçirmek için aşağıdaki komutu kullanın: `kubectl logs omsagent-fdf58 -n=kube-system`. Osmagent pod'ları yapılandırma hatalarından varsa, çıkış aşağıdakine benzer hatalar gösterir:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Hatalar, yeniden başlatın ve varsayılan yapılandırmayı kullanmak için neden dosya ayrıştırması omsagent engelliyor. ConfigMap içinde hataları düzelttikten sonra yaml dosyası kaydedip komutunu çalıştırarak güncelleştirilmiş ConfigMaps uygulayabileceğiniz: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Uygulama ConfigMap güncelleştirildi

Kümeniz için zaten bir ConfigMap dağıttıktan ve daha yeni yapılandırmayla güncelleştirmek istediğiniz, yalnızca daha önce kullandınız ve sonra önceden olduğu gibi aynı komutu kullanarak geçerli ConfigMap dosyayı düzenleyebilirsiniz `kubectl apply -f <configmap_yaml_file.yaml`.

Yapılandırma değişikliğinin etkili almadan önce tamamlanması birkaç dakika sürebilir ve tüm omsagent pod'ların kümesinde başlayacak. Yeniden başlatma tüm omsagent pod'ları için sıralı bir yeniden başlatma, tümü aynı anda yeniden başlatın. Yeniden başlatma tamamlandıktan sonra bir ileti aşağıdakine benzer ve sonucu içeren görüntülenir: `configmap "container-azm-ms-agentconfig" updated`.

## <a name="verifying-schema-version"></a>Şema sürümü doğrulanıyor

Desteklenen yapılandırma şeması sürümleri omsagent pod üzerindeki pod ek açıklama (şema sürümleri) olarak kullanılabilir. Bunları şu kubectl komutla görebilirsiniz: `kubectl describe pod omsagent-fdf58 -n=kube-system`

Çıktı ek açıklama şema sürümleriyle şuna benzer şekilde gösterilir:

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="next-steps"></a>Sonraki adımlar

- Azure İzleyici ve diğer yönleri AKS kümenizi izlemek öğrenme devam etmek için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).
- Görünüm [sorgu örnekleri oturum](container-insights-log-search.md#search-logs-to-analyze-data) önceden tanımlanmış sorgular ve değerlendirme veya uyarı, görselleştirme veya kümelerinizi çözümleme için özelleştirmek için örnekler görmek için.