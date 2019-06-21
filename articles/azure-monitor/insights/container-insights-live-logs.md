---
title: Kapsayıcılar için Azure İzleyici görünümü günlükleri gerçek zamanlı olarak | Microsoft Docs
description: Bu makalede, kapsayıcılar için Azure İzleyici ile kubectl kullanmadan (stdout/stderr) kapsayıcı günlüklerini ve olayları gerçek zamanlı bir görünümünü açıklar.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: magoedte
ms.openlocfilehash: 7fd9248fd38054b7f0e1fad2888d8b0d4cf2e60c
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67274232"
---
# <a name="how-to-view-logs-and-events-in-real-time-preview"></a>Gerçek zamanlı (Önizleme) günlüklerini ve olayları görüntüleme
Kapsayıcılar için Azure İzleyici şu anda kubectl komutlarını çalıştırmak zorunda kalmadan Azure Kubernetes Service (AKS) kapsayıcı günlüklerini (stdout/stderr) ve olayları dinamik bir görünüm sağlar Önizleme aşamasında olan bir özellik içerir. İki seçenekten birini seçtiğinizde, yeni bir bölme performans veri tablosu görünür **düğümleri**, **denetleyicileri**, ve **kapsayıcıları** görünümü. Bu, dinamik günlüğe kaydetme ve daha fazla gerçek zamanlı sorunları gidermeye yardımcı olması için kapsayıcı altyapısı tarafından oluşturulan olayları gösterir.

>[!NOTE]
>**Katkıda bulunan** erişim küme kaynağı için bu özelliğin çalışması için gereklidir.
>

Canlı günlükler günlükleri erişimi denetlemek için üç farklı yöntemi destekler:

1. AKS etkin Kubernetes RBAC yetkilendirme olmadan
2. AKS ile Kubernetes RBAC yetkilendirme etkin
3. Azure Active Directory (AD ile) SAML tabanlı çoklu oturum etkin AKS

## <a name="kubernetes-cluster-without-rbac-enabled"></a>Kubernetes kümesi olmadan etkin RBAC
 
Kubernetes RBAC yetkilendirme ile yapılandırılmamışsa veya Azure AD çoklu oturum açma ile tümleşik bir Kubernetes kümesi varsa, bu adımları izlemeniz gerekmez. Kubernetes yetkilendirme kube-API kullandığından, salt okunur izinler gerekli değildir.

## <a name="kubernetes-rbac-authorization"></a>Kubernetes RBAC yetkilendirme
Kubernetes RBAC yetkilendirme etkinleştirilirse, küme rolü bağlama uygulamak gerekir. Aşağıdaki örnek adımlarda bu yaml yapılandırma şablondan küme rolünü yapılandırmak nasıl ekleyebileceğiniz gösterilmektedir. 

1. Kopyalayın ve yapıştırın yaml dosyası ve LogReaderRBAC.yaml kaydedin.  

    ```
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRole 
    metadata: 
       name: containerHealth-log-reader 
    rules: 
       - apiGroups: [""] 
         resources: ["pods/log", "events"] 
         verbs: ["get", "list"]  
    --- 
    apiVersion: rbac.authorization.k8s.io/v1 
    kind: ClusterRoleBinding 
    metadata: 
       name: containerHealth-read-logs-global 
    roleRef: 
        kind: ClusterRole 
        name: containerHealth-log-reader 
        apiGroup: rbac.authorization.k8s.io 
    subjects: 
       - kind: User 
         name: clusterUser 
         apiGroup: rbac.authorization.k8s.io
    ```

2. İlk kez yapılandırıyorsanız, küme kural bağlama aşağıdaki komutu çalıştırarak oluşturduğunuz: `kubectl create -f LogReaderRBAC.yaml`. Canlı günlükler, yapılandırmasını güncelleştirmek için canlı olay günlükleri, kullanıma sunduk önce önizlemek için destek daha önce etkin değilse aşağıdaki komutu çalıştırın: `kubectl apply -f LogReaderRBAC.yml`.

## <a name="configure-aks-with-azure-active-directory"></a>AKS ile Azure Active Directory'yi yapılandırma

AKS, Azure Active Directory (AD) kullanıcı kimlik doğrulaması için kullanmak üzere yapılandırılabilir. İlk kez yapılandırıyorsanız, bkz. [Azure Active Directory Tümleştirme ile Azure Kubernetes hizmeti](../../aks/azure-ad-integration.md). Oluşturma adımları sırasında [istemci uygulaması](../../aks/azure-ad-integration.md#create-the-client-application), aşağıdakileri belirtin:

- **Yeniden yönlendirme URI'si (isteğe bağlı)** : Bu bir **Web** uygulama türü ve temel URL değeri olmalıdır `https://afd.hosting.portal.azure.net/monitoring/Content/iframe/infrainsights.app/web/base-libs/auth/auth.html`.
- Öğesinden, uygulamayı kaydettikten sonra **genel bakış** seçin sayfasında **kimlik doğrulaması** sol bölmesinden. Üzerinde **kimlik doğrulaması** sayfasındaki **Gelişmiş ayarlar** örtük olarak verme **erişim belirteçlerini** ve **kimlik belirteçlerini** ve kaydedin, değiştirir.

>[!NOTE]
>Kimlik doğrulama işlemini yapılandırmayı Azure Active Directory ile çoklu oturum açma için yalnızca yeni bir AKS kümesi ilk dağıtım sırasında gerçekleştirilebilir. Çoklu oturum zaten dağıtılmış bir AKS kümesi için üzerinde yapılandıramazsınız.
  
>[!IMPORTANT]
>Azure AD güncelleştirilmiş URI'sini kullanarak kullanıcı kimlik doğrulaması için yapılandırdıysanız, güncelleştirilmiş kimlik doğrulaması belirteci indirilir ve uygulanan emin olmak için tarayıcınızın önbelleğini temizleyin.   

## <a name="view-live-logs-and-events"></a>Görünüm Canlı günlükler ve olaylar

Kapsayıcı altyapısı tarafından oluşturulan gerçek zamanlı günlük olayları görüntüleyebileceğiniz **düğümleri**, **denetleyicileri**, ve **kapsayıcıları** görünümü. Özellikler bölmesinde seçtiğiniz **görüntüleme (Önizleme) canlı veriler** seçeneği ve bir bölmesinde görüntüleyebileceğiniz günlük ve olay sürekli bir akış içinde performans veri tablosu aşağıda sunulur.

![Düğüm özellikleri bölmesi görünümü Canlı günlükler seçeneği](./media/container-insights-live-logs/node-properties-live-logs-01.png)  

Günlük ve olay iletileri kaynak türüne görünümünde seçilen temel sınırlıdır.

| Görünüm | Kaynak türü | Günlük veya olay | Sunulan veriler |
|------|---------------|--------------|----------------|
| Düğümler | Düğüm | Olay | Bir düğümü seçildiğinde olayları filtrelenmez ve küme çapında Kubernetes olayları göster. Bölmesi başlığı, kümenin adını gösterir. |
| Düğümler | Pod | Olay | Olaylar, bir pod seçildiğinde, ad alanına filtrelenir. Pod ad Bölmesi Başlığı gösterir. | 
| Denetleyiciler | Pod | Olay | Olaylar, bir pod seçildiğinde, ad alanına filtrelenir. Pod ad Bölmesi Başlığı gösterir. |
| Denetleyiciler | Denetleyici | Olay | Bir denetleyici seçildiğinde olayları kendi ad alanı için filtrelenir. Ad alanı denetleyicisinin Bölmesi Başlığı gösterir. |
| Denetleyicileri/düğüm/kapsayıcılar | Kapsayıcı | Günlükler | Pod kapsayıcı adı ile gruplandırılmış Bölmesi Başlığı gösterir. |

AKS kümesi ile SSO AAD kullanılarak yapılandırıldıysa, ilk kez kullanıldığında, tarayıcı oturumu sırasında kimlik doğrulaması istenir. Hesabınızı ve Azure ile kimlik doğrulamasını tamamlamak seçin.  

Başarıyla kimlik doğrulandıktan sonra dinamik günlük bölmesini Orta bölmenin alt kısmında görünür. Getirme durum göstergesi sağda bölmesinin olan yeşil onay işareti, gösteriliyorsa veri alabildiği anlamına gelir.
    
  ![Canlı günlükler alınan veriler](./media/container-insights-live-logs/live-logs-pane-01.png)  

Arama çubuğunda, metin günlüğü ya da olay ve arama çubuğunda en sağdaki vurgulamak için bir anahtar sözcük göre filtre uygulayabilirsiniz, sonuç filtreyle eşleşen gösterir.

  ![Canlı günlükler bölmesi filtresi örneği](./media/container-insights-live-logs/live-logs-pane-filter-example-01.png)

Olayları görüntülerken kullanarak sonuçları ayrıca sınırlayabilirsiniz **filtre** zehirli arama çubuğunun sağında bulunamadı. Seçtiğiniz hangi kaynaklara bağlı olarak, bir pod, ad veya seçtiğiniz gelen kümeye zehirli listeler.  

Otomatik kaydırma askıya alma ve okunan yeni veriler el ile kaydırma izin bölmesi davranışını denetlemek için tıklayın **kaydırma** seçeneği. Otomatik kaydırma yeniden etkinleştirmek için tıklamanız yeterlidir **kaydırma** yeniden seçeneği. Tıklayarak günlük veya olay verilerinin alınması duraklatabilirsiniz **duraklatmak** seçenek ve devam etmeye hazır olduğunuzda tıklamanız yeterlidir **Play**.  

![Canlı günlükler bölmesi duraklatma Canlı görünümü](./media/container-insights-live-logs/live-logs-pane-pause-01.png)

Geçmiş kapsayıcı günlüklerini seçerek görmek için Azure İzleyici günlüklerine gidin **kapsayıcı günlüklerini görüntüleme** aşağı açılan listeden **analytics'te görüntüle**.

## <a name="next-steps"></a>Sonraki adımlar
- Azure İzleyici ve diğer yönleri AKS kümenizi izlemek öğrenme devam etmek için bkz: [görünümü Azure Kubernetes hizmeti sistem durumu](container-insights-analyze.md).
- Görünüm [sorgu örnekleri oturum](container-insights-log-search.md#search-logs-to-analyze-data) önceden tanımlanmış sorgular ve değerlendirme veya uyarı, görselleştirme veya kümelerinizi çözümleme için özelleştirmek için örnekler görmek için.
