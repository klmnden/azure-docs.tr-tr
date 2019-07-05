---
title: Bir sanal makine içeriğini denetim işlemini anlama
description: Azure İlkesi Rego ve açık bir ilke aracısı kümelerinde Azure Kubernetes hizmeti yönetmek için nasıl kullandığını öğrenin.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.openlocfilehash: fdb392533e28df1d50e90c842d0117385afb254b
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67453911"
---
# <a name="understand-azure-policy-for-azure-kubernetes-service"></a>Azure ilkesi için Azure Kubernetes hizmeti anlama

Azure İlkesi ile tümleştirilir [Azure Kubernetes hizmeti](../../../aks/intro-kubernetes.md) ölçekli enforcements ve güvenlik önlemleri kümeleriniz üzerinde merkezi ve tutarlı bir şekilde uygulamak için (AKS).
Kullanımını genişleterek [ağ geçidi](https://github.com/open-policy-agent/gatekeeper), bir _giriş denetleyicisine Web kancası_ için [açık İlke Aracısı](https://www.openpolicyagent.org/) (d), Azure İlkesi mümkün kılar yönetebilir ve uyumluluk raporu Azure kaynaklarını ve AKS küme tek bir yerden durumu.

> [!NOTE]
> AKS için Azure İlkesi sınırlı Önizleme aşamasındadır ve yalnızca yerleşik ilke tanımları destekler.

## <a name="overview"></a>Genel Bakış

Etkinleştirme ve Azure İlkesi ile AKS kümenizi AKS için kullanma hakkında bilgi için aşağıdaki eylemleri gerçekleştirin:

- [Önizleme özellikleri katılımı](#opt-in-for-preview)
- [Azure İlkesi eklentisini yükleyin](#installation-steps)
- [AKS için bir ilke tanımı atama](#built-in-policies)
- [Doğrulama işleminin tamamlanmasını bekleyin](#validation-and-reporting-frequency)

## <a name="opt-in-for-preview"></a>Önizleme katılımı

Azure İlkesi eklentisi yükleme veya herhangi bir hizmet özelliklerini etkinleştirme önce aboneliğinizi etkinleştirmeniz gerekir **Microsoft.ContainerService** kaynak sağlayıcısı ve **Microsoft.policyınsights**kaynak sağlayıcısı, ardından önizlemeye katılmak için onaylanması gerekiyor. Önizlemeye katılmak için Azure portalında veya Azure CLI ile aşağıdaki adımları izleyin:

- Azure portalı:

  1. Kayıt **Microsoft.ContainerService** ve **Microsoft.policyınsights** kaynak sağlayıcıları. Adımlar için bkz: [kaynak sağlayıcıları ve türleri](../../../azure-resource-manager/resource-manager-supported-services.md#azure-portal).

  1. Azure portalında **Tüm hizmetler**’e tıkladıktan sonra **İlke**'yi arayıp seçerek Azure İlkesi hizmetini başlatın.

     ![Tüm hizmetler ilkesinde arayın](../media/rego-for-aks/search-policy.png)

  1. Seçin **katılın Önizleme** Azure İlkesi sayfasının sol tarafındaki.

     ![AKS önizlemesini için ilke katılın](../media/rego-for-aks/join-aks-preview.png)

  1. Satır Önizleme eklenmesini istediğiniz aboneliği seçin.

  1. Seçin **katılımı** abonelikleri listesinin üstünde düğme.

- Azure CLI:

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  # Provider register: Register the Azure Kubernetes Services provider
  az provider register --namespace Microsoft.ContainerService

  # Provider register: Register the Azure Policy provider
  az provider register --namespace Microsoft.PolicyInsights

  # Feature register: enables installing the add-on
  az feature register --namespace Microsoft.ContainerService --name AKS-AzurePolicyAutoApprove

  # Feature register: enables the add-on to call the Azure Policy resource provider
  az feature register --namespace Microsoft.PolicyInsights --name AKS-DataplaneAutoApprove
  ```

## <a name="azure-policy-add-on"></a>Azure İlkesi eklentisi

_Azure İlkesi eklenti_ için Kubernetes Azure İlkesi hizmetini ağ geçidi giriş denetleyicisine bağlanır. İçine yüklü eklenti _azure İlkesi_ ad alanı, aşağıdaki işlevleri enacts:

- Azure İlkesi ile AKS kümesi atamaları denetler
- İndirir ve ilke ayrıntıları dahil olmak üzere, önbelleğe _rego_ ilke tanımı olarak **configmaps**
- Bir tam tarama uyumluluk denetimi AKS kümesinde çalıştırılır
- Azure İlkesi, Denetim raporları ve uyumluluk ayrıntıları yedekleme

### <a name="installing-the-add-on"></a>Eklenti yükleme

#### <a name="prerequisites"></a>Önkoşullar

Önizleme uzantısı, AKS kümenizin eklenti yüklemeden önce yüklenmelidir. Bu adım, Azure CLI ile gerçekleştirilir:

1. Azure CLI Sürüm 2.0.62 gerekir veya daha sonra yüklü ve yapılandırılmış. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

1. AKS kümesi sürümü olmalıdır _1.10_ veya üzeri. AKS küme sürümünüzün doğrulamak için aşağıdaki betiği kullanın:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Look for the value in kubernetesVersion
   az aks list
   ```

1. Sürümünü yüklemek _0.4.0_ AKS için Azure CLI Önizleme uzantısı `aks-preview`:

   ```azurecli-interactive
   # Log in first with az login if you're not using Cloud Shell

   # Install/update the preview extension
   az extension add --name aks-preview

   # Validate the version of the preview extension
   az extension show --name aks-preview --query [version]
   ```

   > [!NOTE]
   > Daha önce yüklediyseniz _aks önizlemesini_ uzantısı, tüm güncelleştirmelerin kullanarak `az extension update --name aks-preview` komutu.

#### <a name="installation-steps"></a>Yükleme adımları

Önkoşulları tamamladıktan sonra yönetmek istediğiniz AKS kümesinde Azure İlkesi eklentiyi yükleyin.

- Azure portal

  1. Azure portalında AKS hizmeti tıklanması **tüm hizmetleri**, ardından arama ve seçme **Kubernetes Hizmetleri**.

  1. AKS kümelerinizi birini seçin.

  1. Seçin **ilkeleri (Önizleme)** Kubernetes hizmeti sayfanın sol tarafındaki.

     ![AKS kümesi ilkelerden](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Ana sayfada seçin **etkinleştirme eklenti** düğmesi.

     ![Azure İlkesi AKS eklenti için etkinleştirme](../media/rego-for-aks/enable-policy-add-on.png)

     > [!NOTE]
     > Varsa **etkinleştirme eklenti** düğmesi gri, abonelik henüz Önizleme eklenmedi. Bkz: [katılımı Önizleme için](#opt-in-for-preview) için gerekli adımlar.

- Azure CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks enable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

### <a name="validation-and-reporting-frequency"></a>Doğrulama ve raporlama sıklığı

Eklenti Azure İlkesi ile oturum ilke atamaları değişiklikleri her 5 dakikada olup olmadığını denetler. Bu yenileme döngüsü sırasında eklenti tüm kaldırır _configmaps_ içinde _azure İlkesi_ ad alanı yeniden oluşturur _configmaps_ ağ geçidi kullanmak için.

> [!NOTE]
> Sırasında bir _yönetici küme_ izniniz olmayabilir _azure İlkesi_ ad alanı, onu değil önerilen veya ad alanına değişiklik yapmak için desteklenir. Yenileme dönemi boyunca herhangi bir el ile yapılan değişiklikler kaybolur.

5 dakikada küme için bir tam tarama eklenti çağırır. Tam tarama ve herhangi bir gerçek zamanlı değerlendirmeleri ayrıntılarını denenen değişiklikleri ağ geçidi tarafından kümeye topladıktan sonra eklenti sonuçları geri Azure İlkesi'na kabul edilmeye yönelik raporları [uyumluluk ayrıntıları](../how-to/get-compliance-data.md) istediğiniz herhangi bir Azure İlkesi atama. Yalnızca etkin ilke atamaları için sonuçları denetim döngüsü sırasında döndürülür.

## <a name="policy-language"></a>İlke dili

AKS yönetmek için Azure İlkesi dil yapısı, mevcut ilkeleri izler. Etkisi _EnforceRegoPolicy_ AKS kümelerinizi yönetmek için kullanılır ve alan _ayrıntıları_ d ve ağ geçidi ile çalışmak için belirli özellikleri. Ayrıntılar ve örnekler için bkz. [EnforceRegoPolicy](effects.md#enforceregopolicy) efekt.

Bir parçası olarak _details.policy_ Azure İlkesi ilke tanımının özelliğinde eklentiye rego İlkesi URI'si geçirir. Rego doğrulamak veya Kubernetes kümesi için bir istek bulunmamalıdır d ve ağ geçidi destekleyen bir dildir. Kubernetes yönetimi için mevcut bir standardı destekleyerek, Azure İlkesi, birleşik bulut uyumluluk raporlama deneyimi için bunları Azure İlkesi ile eşleştirin ve varolan kuralları yeniden mümkün kılar. Daha fazla bilgi için [Rego nedir?](https://www.openpolicyagent.org/docs/how-do-i-write-policies.html#what-is-rego).

## <a name="built-in-policies"></a>Yerleşik ilkeler

Azure portalını kullanarak AKS yönetmek için yerleşik ilkeleri bulmak için aşağıdaki adımları izleyin:

1. Azure portalında Azure İlkesi hizmetini başlatın. Seçin **tüm hizmetleri** sol bölmesinde arayın ve seçin **ilke**.

1. Azure İlkesi sayfasının sol bölmesinde seçin **tanımları**.

1. Kategori açılan liste kutusundan kullanmak **Tümünü Seç** filtreyi temizleyin ve ardından **Kubernetes hizmeti**.

1. İlke tanımı'nı seçin ve ardından **atama** düğmesi.

> [!NOTE]
> Azure İlkesi AKS tanımı için atarken **kapsam** AKS küme kaynağı eklemeniz gerekir.

Alternatif olarak, kullanın [bir ilke atama - Portal](../assign-policy-portal.md) bulmak ve bir AKS ilkesi atamak için hızlı başlangıç. Bir Kubernetes ilke tanımı 'denetim vm'leri' örnek yerine arayın.

## <a name="logging"></a>Günlüğe kaydetme

### <a name="azure-policy-add-on-logs"></a>Azure İlkesi eklenti günlükleri

Bir Kubernetes denetleyicisi/kapsayıcı olarak Azure İlkesi eklenti günlükleri AKS kümesinde tutar. Günlükleri sunulan **Insights** AKS kümesi sayfası. Daha fazla bilgi için [anlamak AKS, kapsayıcılar için Azure İzleyici ile performans küme](../../../azure-monitor/insights/container-insights-analyze.md).

### <a name="gatekeeper-logs"></a>Ağ geçidi günlükleri

Yeni kaynak istekleri için ağ geçidi günlüklerini etkinleştirmek için adımları [etkinleştirin ve Kubernetes AKS ana düğüm günlüklerini gözden geçirme](../../../aks/view-master-logs.md).
Yeni kaynak isteklerini reddedilen olayları görüntülemek için örnek bir sorgu aşağıda verilmiştir:

```kusto
| where Category == "kube-audit"
| where log_s contains "admission webhook"
| limit 100
```

Ağ geçidi kapsayıcı günlüklerini görüntülemek için adımları izleyin. [etkinleştirmek ve Kubernetes AKS ana düğüm günlüklerini gözden geçirin](../../../aks/view-master-logs.md) ve _kube-apiserver_ seçeneğini **Tanılamaayarları** bölmesi.

## <a name="remove-the-add-on"></a>Eklentiyi Kaldır

AKS kümenizi Azure İlkesi eklentiyi kaldırmak için Azure portal veya Azure CLI'yı kullanın:

- Azure portal

  1. Azure portalında AKS hizmeti tıklanması **tüm hizmetleri**, ardından arama ve seçme **Kubernetes Hizmetleri**.

  1. AKS kümenizi Azure İlkesi eklentiyi devre dışı bırakmak istediğiniz yeri seçin.

  1. Seçin **ilkeleri (Önizleme)** Kubernetes hizmeti sayfanın sol tarafındaki.

     ![AKS kümesi ilkelerden](../media/rego-for-aks/policies-preview-from-aks-cluster.png)

  1. Ana sayfada seçin **eklentiyi devre dışı bırak** düğmesi.

     ![Azure İlkesi AKS eklenti için devre dışı bırak](../media/rego-for-aks/disable-policy-add-on.png)

- Azure CLI

  ```azurecli-interactive
  # Log in first with az login if you're not using Cloud Shell

  az aks disable-addons --addons azure-policy --name MyAKSCluster --resource-group MyResourceGroup
  ```

## <a name="next-steps"></a>Sonraki adımlar

- Gözden geçirme örneklere [Azure ilkesi örnekleri](../samples/index.md).
- [İlke tanım yapısını](definition-structure.md) gözden geçirin.
- [İlkenin etkilerini anlama](effects.md) konusunu gözden geçirin.
- Anlamak için nasıl [programlı olarak ilkeler oluşturma](../how-to/programmatically-create.md).
- Bilgi edinmek için nasıl [uyumluluk verilerini alma](../how-to/getting-compliance-data.md).
- Bilgi edinmek için nasıl [uyumlu olmayan kaynakları düzeltme](../how-to/remediate-resources.md).
- Bir yönetim grubu olan gözden geçirme [kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../management-groups/index.md).