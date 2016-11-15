---
title: "Azure Traffic Manager profillerini yönetme| Microsoft Belgeleri"
description: "Bu makale, bir Azure Traffic Manager profili oluşturma, devre dışı bırakma, etkinleştirme, silme ve geçmişini görüntüleme konularında size yardımcı olur."
services: traffic-manager
documentationcenter: 
author: sdwheeler
manager: carmonm
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: sewhee
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 7c322fef4fb8588d6d3cc5c6c30fdcb486834155

---

# <a name="manage-an-azure-traffic-manager-profile"></a>Bir Azure Traffic Manager profilini yönetme

Traffic Manager profilleri, trafiğin bulut hizmetlerinize veya web sitesi uç noktalarına dağıtımını denetlemek için trafik yönlendirme yöntemlerini kullanır. Bu makalede, bu profillerin nasıl oluşturulacağı ve yönetileceği açıklanmıştır.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Hızlı Oluştur'u kullanarak bir Traffic Manager profili oluşturma

Klasik Azure portalında Hızlı Oluştur'u kullanarak bir Traffic Manager profilini hızlı bir şekilde oluşturabilirsiniz. Hızlı Oluştur, profilleri temel yapılandırma ayarlarıyla oluşturmanızı sağlar. Ancak uç noktalar kümesi (bulut hizmetleri ve web siteleri) ve yük devretme trafik yönlendirme yönteminin yük devretme sırası gibi seçenekler veya izleme seçenekleri için Hızlı Oluştur'u kullanamazsınız. Profilinizi oluşturduktan sonra, bu ayarları klasik Azure portalında yapılandırabilirsiniz. Traffic Manager her profil için en fazla 200 uç noktayı destekler. Bununla birlikte, çoğu kullanım senaryosu yalnızca birkaç uç nokta gerektirir.

### <a name="to-create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

1. **Bulut hizmetlerinizi ve web sitelerinizi üretim ortamınıza dağıtın.** Bulut hizmetleri hakkında daha fazla bilgi için bkz. [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074). Web siteleri hakkında daha fazla bilgi için bkz. [Web siteleri](http://go.microsoft.com/fwlink/p/?LinkId=393327).
2. **Klasik Azure portalında oturum açın.** Portalın sol alt kısmındaki **Yeni**'ye tıklayın, **Ağ Hizmetleri > Traffic Manager**'a tıklayın ve ardından **Hızlı Oluştur**'a tıklayarak profilinizi yapılandırmaya başlayın.
3. **DNS ön ekini yapılandırın.** Trafik yöneticisi profilinize benzersiz bir DNS ön ek adı verin. Bir Traffic Manager etki alanı adı için yalnızca ön eki belirtebilirsiniz.
4. **Aboneliği seçin.** Uygun Azure aboneliğini seçin. Her profil tek bir abonelik ile ilişkilidir. Yalnızca bir aboneliğiniz varsa bu seçenek görünmez.
5. **Trafik yönlendirme yöntemini seçin.** **Trafik yönlendirme İlkesinde** trafik yönlendirme yöntemini seçin . Trafik yönlendirme yöntemleri hakkında daha fazla bilgi için bkz. [Traffic Manager trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md).
6. **Profili oluşturmak için “Oluştur”a tıklayın**. Profil yapılandırması tamamlandığı zaman, profilinizi klasik Azure portalındaki Traffic Manager bölmesinde bulabilirsiniz.
7. **Klasik Azure portalında uç noktaları, izlemeyi ve ek ayarları yapılandırın.** Hızlı Oluştur’un kullanılması, yalnızca temel ayarları yapılandırır. Uç noktaların listesi ve uç nokta yük devretme sırası gibi ek ayarların yapılandırılması gerekir.

## <a name="disable-enable-or-delete-a-profile"></a>Bir profili devre dışı bırakma, etkinleştirme veya silme

Mevcut bir profili devre dışı bırakarak Traffic Manager’ın kullanıcı istekleri için yapılandırılan uç noktalara başvurmamasını sağlayabilirsiniz. Bir Traffic Manager profilini devre dışı bıraktığınızda, profil ve profilde yer alan bilgiler değişmeden kalır ve Traffic Manager arabiriminden düzenlenebilir.  Referanslar, profili yeniden etkinleştirdiğinizde devam eder. Klasik Azure portalında bir Traffic Manager profili oluşturduğunuzda, profil otomatik olarak etkinleştirilir. Bir profilin artık gerekli olmadığına karar verirseniz profili silebilirsiniz.

### <a name="to-disable-a-profile"></a>Bir profili devre dışı bırakma

1. Özel etki alanı adı kullanıyorsanız İnternet DNS sunucunuzdaki CNAME kaydını, artık Traffic Manager profilinizi göstermeyecek şekilde değiştirin.
2. Traffic Manager profil ayarları aracılığıyla trafiğin uç noktalara yönlendirilmesi durdurulur.
3. Devre dışı bırakmak istediğiniz profili seçin. Traffic Manager sayfasında, profil adının yanındaki sütuna tıklayarak profili vurgulayın. Profilin adına veya adın yanındaki oka tıkladığınızda, profilin ayarlar sayfası açılır.
4. Profil seçtikten sonra sayfanın alt kısmındaki **Devre Dışı Bırak**'a tıklayın.

### <a name="to-enable-a-profile"></a>Bir profili etkinleştirme

1. Devre dışı bırakmak istediğiniz profili seçin. Traffic Manager sayfasında, profil adının yanındaki sütuna tıklayarak profili vurgulayın. Profilin adına veya adın yanındaki oka tıkladığınızda, profilin ayarlar sayfası açılır.
2. Profili seçtikten sonra sayfanın alt kısmındaki **Etkinleştir**'e tıklayın.
3. Özel etki alanı adı kullanıyorsanız İnternet DNS sunucunuzda Traffic Manager profilinizin etki alanı adını gösterecek bir CNAME kaynak kaydı oluşturun.
4. Trafik yeniden uç noktalara yönlendirilir.

### <a name="to-delete-a-profile"></a>Bir profili silme

1. İnternet DNS sunucunuzdaki DNS kaynak kaydının Traffic Manager profilinize ait etki alanı adına işaret eden bir CNAME kaynak kaydını artık kullanmadığından emin olun.
2. Devre dışı bırakmak istediğiniz profili seçin. Traffic Manager sayfasında, profil adının yanındaki sütuna tıklayarak profili vurgulayın. Profilin adına veya adın yanındaki oka tıkladığınızda, profilin ayarlar sayfası açılır.
3. Profil seçtikten sonra sayfanın alt kısmındaki **Sil**'e tıklayın.

## <a name="view-traffic-manager-profile-change-history"></a>Traffic Manager profilinin değişiklik geçmişini görüntüleme

Klasik Azure portalındaki Yönetim Hizmetleri'nde, Traffic Manager profilinizin değişiklik geçmişini görüntüleyebilirsiniz.

### <a name="to-view-your-traffic-manager-change-history"></a>Traffic Manager değişiklik geçmişinizi görüntüleme

1. Klasik Azure portalının sol bölmesinde **Yönetim Hizmetleri**'ne tıklayın.
2. Yönetim Hizmetleri sayfasında **İşlem Günlükleri**'ne tıklayın.
3. İşlem Günlükleri sayfasında, Traffic Manager profilinizin değişiklik geçmişini görüntülemek için filtreleyebilirsiniz. Filtreleme seçeneklerinizi belirledikten sonra, sonuçları görüntülemek için onay işaretine tıklayın.

   * Tüm profillerinize ilişkin değişiklikleri görüntülemek için aboneliğinizi ve zaman aralığını seçin ve ardından **Tür** kısayol menüsünden **Traffic Manager**'ı seçin.
   * Profil adına göre filtrelemek için profilin adını **Hizmet Adı** alanına yazın veya kısayol menüsünden seçin.
   * Tek tek her değişikliğe ait ayrıntıları görüntülemek için görüntülemek istediğiniz değişikliği içeren satırı seçin ve ardından sayfanın alt kısmındaki **Ayrıntılar**'a tıklayın. **İşlem Ayrıntıları** penceresinde, işlemin bir parçası olarak oluşturulan veya güncelleştirilen API nesnesinin XML gösterimini görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir uç nokta ekleme](traffic-manager-endpoints.md)
* [Yük devretme yönlendirme yöntemini yapılandırma](traffic-manager-configure-failover-routing-method.md)
* [Hepsini bir kez deneme yönlendirme yöntemini yapılandırma](traffic-manager-configure-round-robin-routing-method.md)
* [Performans yönlendirme yöntemini yapılandırma](traffic-manager-configure-performance-routing-method.md)
* [Bir şirketin İnternet etki alanını Traffic Manager etki alanı adına yönlendirme](traffic-manager-point-internet-domain.md)
* [Düzeyi düşürülmüş Traffic Manager durumu için sorun giderme](traffic-manager-troubleshooting-degraded.md)



<!--HONumber=Nov16_HO2-->


