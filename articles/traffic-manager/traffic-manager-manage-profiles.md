<properties
   pageTitle="Azure Traffic Manager profillerini yönetme| Microsoft Azure"
   description="Bu makale bir Azure Traffic Manager profilini oluşturma, devre dışı bırakma, etkinleştirme, silme ve geçmişini görüntüleme konularında size yardımcı olacaktır."
   services="traffic-manager"
   documentationCenter=""
   authors="joaoma"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="joaoma" />

# Bir Azure Traffic Manager profilini yönetme

Traffic Manager tarafından izlenecek bulut hizmetlerini veya web sitesi uç noktalarını, aynı zamanda bu uç noktalarına bağlantıları dağıtırken hangi trafik yönlendirme yöntemini kullanmak istediğinizi belirtmek için bir Traffic Manager profilini kullanırsınız.

## Hızlı Oluştur'u kullanarak bir Traffic Manager profili oluşturma

Klasik Azure portalında Hızlı Oluştur'u kullanarak bir Traffic Manager profilini hızlı bir şekilde oluşturabilirsiniz. Hızlı Oluştur, profilleri temel yapılandırma ayarlarıyla oluşturmanızı sağlar. Ancak uç noktalar kümesi (bulut hizmetleri ve web siteleri) ve yük devretme trafik yönlendirme yönteminin yük devretme sırası gibi seçenekler veya izleme seçenekleri için Hızlı Oluştur'u kullanamazsınız. Profilinizi oluşturduktan sonra, bu ayarları klasik Azure portalında yapılandırabilirsiniz. Traffic Manager her profil için en fazla 200 uç noktayı destekler. Bununla birlikte, çoğu kullanım senaryosu çok az sayıda uç noktayı gerektirir. 

### Yeni bir Traffic Manager profili oluşturma

1. **Bulut hizmetlerinizi ve web sitelerinizi üretim ortamınıza dağıtın.** Bulut hizmetleri hakkında daha fazla bilgi için bkz. [Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=314074). Bulut hizmetleri hakkında bilgi için bkz. [En iyi uygulamalar](https://msdn.microsoft.com/library/azure/5229dd1c-5a91-4869-8522-bed8597d9cf5#bkmk_TrafficManagerBestPracticesProfile). Web siteleri hakkında daha fazla bilgi için bkz. [Web siteleri](http://go.microsoft.com/fwlink/p/?LinkId=393327).

2. **Klasik Azure portalında oturum açın.** Yeni bir Traffic Manager profili oluşturmak için portalın sol alt kısmındaki **Yeni**'ye tıklayın, **Ağ Hizmetleri > Traffic Manager**'a tıklayın ve ardından **Hızlı Oluştur**'a tıklayarak profilinizi yapılandırmaya başlayın.
3. **DNS ön ekini yapılandırın.** Trafik yöneticisi profilinize benzersiz bir DNS ön ek adı verin. Bir Traffic Manager etki alanı adı için yalnızca ön eki belirtebilirsiniz.
4. **Aboneliği seçin.** Uygun Azure aboneliğini seçin. Her profil tek bir abonelik ile ilişkilidir. Yalnızca bir aboneliğiniz varsa bu seçenek görünmez.
5. **Trafik yönlendirme yöntemini seçin.** **Trafik yönlendirme İlkesinde** trafik yönlendirme yöntemini seçin . Trafik yönlendirme yöntemleri hakkında daha fazla bilgi için bkz. [Traffic Manager trafik yönlendirme yöntemleri hakkında](traffic-manager-routing-methods.md).
6. **Yeni profilinizi oluşturmak için "Oluştur" seçeneğine tıklayın**. Profil yapılandırması tamamlandığı zaman, profilinizi klasik Azure portalındaki Traffic Manager bölmesinde bulabilirsiniz.
7. **Klasik Azure portalında uç noktaları, izlemeyi ve ek ayarları yapılandırın.** Hızlı Oluştur'u kullanarak yalnızca temel ayarları yapılandırabilirsiniz, bu nedenle istediğiniz yapılandırmayı tamamlamak için uç noktalar listesi ve uç nokta yük devretme sırası gibi ek ayarları yapılandırmanız gereklidir. 


## Bir profili devre dışı bırakma, etkinleştirme veya silme

Var olan bir Traffic Manager profilini devre dışı bırakarak Traffic Manager'ın kullanıcı istekleri için yapılandırılmış uç noktalarına başvurmamasını sağlayabilirsiniz. Bir Traffic Manager profilini devre dışı bıraktığınızda, profilin kendisi ve profilde yer alan bilgiler değişmeden kalır ve Traffic Manager arabiriminden düzenlenebilir. Profili yeniden etkinleştirmek istediğinizde bunu klasik Azure portalında kolaylıkla yapabilirsiniz, bu durumda başvurular devam eder. Klasik Azure portalında bir Traffic Manager profili oluşturduğunuzda, profil otomatik olarak etkinleştirilir. Bir profilin artık gerekli olmadığına karar verirseniz profili silebilirsiniz.

### Bir profili devre dışı bırakma

1. İnternet DNS sunucunuzdaki DNS kaynak kaydını değiştirerek İnternet üzerindeki belirli bir konumun başka bir adına veya IP adresine yönlendiren uygun kayıt türünü ve işaretçiyi kullanın. Diğer bir deyişle, İnternet DNS sunucunuzdaki DNS kaynak kaydını değiştirerek Traffic Manager profilinize ait etki alanı adına yönlendiren bir CNAME kaynak kaydını artık kullanmamasını sağlayın.
2. Traffic Manager profili ayarları yoluyla trafiğin uç noktalara yönlendirilmesi durdurulur.
3. Devre dışı bırakmak istediğiniz profili seçin. Profili seçmek için Traffic Manager sayfasında profil adının yanındaki sütuna tıklayarak profili vurgulayın. Profilin adına veya adın yanındaki oka tıklamayın, bu işlemler sizi profilin ayarlar sayfasına götürür.
4. Profil seçtikten sonra sayfanın alt kısmındaki **Devre Dışı Bırak**'a tıklayın.

### Bir profili etkinleştirme

1. Etkinleştirmek istediğiniz profili seçin. Profili seçmek için Traffic Manager sayfasında profil adının yanındaki sütuna tıklayarak profili vurgulayın. Profilin adına veya adın yanındaki oka tıklamayın, bu işlemler sizi profilin ayarlar sayfasına götürür.
2. Profili seçtikten sonra sayfanın alt kısmındaki **Etkinleştir**'e tıklayın.
3. İnternet DNS sunucunuzdaki DNS kaynak kaydını değiştirerek, şirketinizin etki alanı adını Traffic Manager profilinizin etki alanı adına eşleyecek CNAME kaynak türünü kullanın. Daha fazla bilgi için bkz. [Bir şirketin İnternet etki alanını Traffic Manager etki alanına yönlendirme](traffic-manager-point-internet-domain.md).
4. Trafik yeniden uç noktalara yönlendirilmeye başlar.

### Bir profili silme

1. İnternet DNS sunucunuzdaki DNS kaynak kaydının Traffic Manager profilinize ait etki alanı adına işaret eden bir CNAME kaynak kaydını artık kullanmadığından emin olun.
2. Silmek istediğiniz profili seçin. Profili seçmek için Traffic Manager sayfasında profilin yanındaki sütuna tıklayarak profili vurgulayın. Profilin adına veya adın yanındaki oka tıklamayın, bu işlemler sizi profilin ayarlar sayfasına götürür.
4. Profil seçtikten sonra sayfanın alt kısmındaki **Sil**'e tıklayın.

## Traffic Manager profilinin değişiklik geçmişini görüntüleme

Klasik Azure portalındaki Yönetim Hizmetleri'nde, Traffic Manager profilinizin değişiklik geçmişini görüntüleyebilirsiniz.

### Traffic Manager değişiklik geçmişinizi görüntüleme

1. Klasik Azure portalının sol bölmesinde **Yönetim Hizmetleri**'ne tıklayın.
2. Yönetim Hizmetleri sayfasında **İşlem Günlükleri**'ne tıklayın.
3. İşlem Günlükleri sayfasında, Traffic Manager profilinizin değişiklik geçmişini görüntülemek için filtreleyebilirsiniz. Filtreleme seçeneklerinizi belirledikten sonra, sonuçları görüntülemek için onay işaretine tıklayın.
   - Tüm profillerinize ilişkin profil değişikliklerini görüntülemek için aboneliğinizi ve zaman aralığını seçin ve ardından **Tür** kısayol menüsünden **Traffic Manager**'ı seçin.
   - Profil adına göre filtrelemek için profilin adını **Hizmet Adı** alanına yazın veya kısayol menüsünden seçin.
   - Tek tek her değişikliğe ait ayrıntıları görüntülemek için görüntülemek istediğiniz değişikliği içeren satırı seçin ve ardından sayfanın alt kısmındaki **Ayrıntılar**'a tıklayın. **İşlem Ayrıntıları** penceresinde, işlemin bir parçası olarak oluşturulan veya güncelleştirilen API nesnesinin XML gösterimini görüntüleyebilir ve XML kodunu panoya kopyalayabilirsiniz.


## Sonraki adımlar

[Bir uç nokta ekleme](traffic-manager-endpoints.md)

[Yük devretme yönlendirme yöntemini yapılandırma](traffic-manager-configure-failover-routing-method.md)

[Hepsini bir kez deneme yönlendirme yöntemini yapılandırma](traffic-manager-configure-round-robin-routing-method.md)

[Performans yönlendirme yöntemini yapılandırma](traffic-manager-configure-performance-routing-method.md)

[Traffic Manager düşürülmüş durumu için sorun giderme](traffic-manager-troubleshooting-degraded.md)


<!--HONumber=Jun16_HO2-->


