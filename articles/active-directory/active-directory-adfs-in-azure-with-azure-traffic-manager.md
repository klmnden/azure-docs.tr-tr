---
title: Azure Traffic Manager ile Azure’da yüksek kullanılabilirliğe sahip çapraz coğrafi AD FS dağıtımı | Microsoft Belgeleri
description: Bu belgede AD FS’yi yüksek kullanılabilirlik için Azure’a dağıtma hakkında bilgi edineceksiniz.
keywords: Azure traffic manager ile Ad fs, Azure Traffic Manager ile adfs, coğrafi, çoklu veri merkezi, coğrafi veri merkezleri, çoklu coğrafi veri merkezleri, azure’da AD FS dağıtma, azure adfs dağıtma, azure adfs, azure ad fs, adfs dağıtma, ad fs dağıtma, azure’da adfs, azure’da adfs dağıtma, azure’da AD FS dağıtma, adfs azure, AD FS’ye giriş, Azure, Azure’da AD FS, iaas, ADFS, adfs’yi azure’a taşıma
services: active-directory
documentationcenter: ''
author: anandyadavmsft
manager: mtillman
editor: ''
ms.assetid: a14bc870-9fad-45ed-acd5-a90ccd432e54
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2016
ms.author: anandy;billmath
ms.openlocfilehash: 2ed0b551faba68c0956be89277348eeee60d759c
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46298226"
---
# <a name="high-availability-cross-geographic-ad-fs-deployment-in-azure-with-azure-traffic-manager"></a>Azure Traffic Manager ile Azure’da yüksek kullanılabilirliğe sahip çapraz coğrafi AD FS dağıtımı
[Azure’da AD FS dağıtımı](hybrid/how-to-connect-fed-azure-adfs.md), kuruluşunuz için basit bir AD FS altyapısını Azure’da nasıl dağıtabileceğinize ilişkin adım adım yönergeler sağlar. Bu makale Azure’da [Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md) kullanılarak AD FS’nin çapraz coğrafi dağıtımını oluşturmaya ilişkin sıradaki adımları sunmaktadır. Azure Traffic Manager, altyapının farklı ihtiyaçlarına uygun olarak kullanılabilen çeşitli yönlendirme yöntemlerinden yararlanarak kuruluşunuz için coğrafi olarak yayılmış yüksek kullanılabilirliğe sahip ve yüksek performanslı bir AD FS altyapısı oluşturmaya yardımcı olur.

Çapraz coğrafi yüksek kullanılabilirliğe sahip AD FS altyapısı şunları sağlar:

* **Tek hata noktasının ortadan kaldırılması:** Azure Traffic Manager’ın yük devretme özellikleriyle dünyanın bir yerindeki veri merkezlerinden birisi arızalandığında bile yüksek kullanılabilirliğe sahip bir AD FS altyapısı elde edebilirsiniz
* **Daha iyi performans:** Kullanıcıların daha hızlı kimlik doğrulaması yapmasına yardımcı olabilecek yüksek performanslı bir AD FS altyapısı sağlamak üzere bu makalede önerilen dağıtımı kullanabilirsiniz. 

## <a name="design-principles"></a>Tasarım ilkeleri
![Genel tasarım](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/blockdiagram.png)

Temel tasarım ilkeleri Azure’da AD FS dağıtımı adlı makaledeki Tasarım ilkelerinde listelenenlerle aynıdır. Yukarıdaki diyagramda başka bir coğrafi bölgeye temel dağıtımın basit genişletilmesi gösterilmektedir. Dağıtımınızı yeni bir coğrafi bölgeye genişletirken göz önünde bulundurmanız gereken bazı noktalar aşağıda verilmiştir

* **Sanal ağ:** Ek AD FS altyapısı dağıtmak istediğiniz coğrafi bölgede yeni bir sanal ağ oluşturmanız gerekir. Yukarıdaki diyagramda her bir coğrafi bölgedeki iki sanal ağ olarak Geo1 VNET ve Geo2 VNET görülmektedir.
* **Yeni coğrafi sanal ağdaki etki alanı denetleyicileri ve AD FS sunucuları:** Etki alanı denetleyicilerinin, yeni bölgedeki AD FS sunucularının bir kimlik doğrulama işlemini tamamlamak üzere başka bir uzak ağ ile iletişim kurmamasını sağlayacak şekilde dağıtılması ve böyle performansın artırılması önerilir.
* **Depolama hesapları:** Depolama hesapları bir bölge ile ilişkilidir. Makineleri yeni bir coğrafi bölgeye dağıtacağınız için bölgede kullanılacak yeni depolama hesapları oluşturmanız gerekir.  
* **Ağ Güvenlik Grupları:** Bir bölgede oluşturulan Ağ Güvenlik Grupları başka bir coğrafi bölgede depolama hesabı olarak kullanılamaz. Bu nedenle, yeni coğrafi bölgede INT ve DMZ alt ağı için birinci coğrafi bölgedekilere benzer yeni ağ güvenlik grupları oluşturmanız gerekir.
* **Genel IP adresleri için DNS Etiketleri:** Azure Traffic Manager, uç noktalara YALNIZCA DNS etiketleri üzerinden başvurabilir. Bu nedenle, Dış Yük Dengeleyicinin genel IP adresleri için DNS etiketleri oluşturmanız gereklidir.
* **Azure Traffic Manager:** Microsoft Azure Traffic Manager dünyanın dört bir yanındaki farklı veri merkezlerinde çalışan hizmet uç noktalarınıza kullanıcı trafiğinin dağıtılmasını denetler. Azure Traffic Manager, DNS düzeyinde çalışır. Son kullanıcı trafiğini küresel ölçekte dağıtılmış uç noktalarına yönlendirmek için DNS yanıtları kullanır. İstemciler daha sonra bu uç noktalara doğrudan bağlanır. Performans, Ağırlıklı ve Öncelik şeklindeki farklı yönlendirme seçenekleriyle kuruluşunuzun gereksinimlerine en uygun yönlendirme seçeneğini kolayca belirleyebilirsiniz. 
* **İki bölge arasında sanal ağdan sanal ağa bağlantı:** Sanal ağların kendileri arasında bağlantı kurmanız gerekmez. Her sanal ağın etki alanı denetleyicilerine erişiminin olması ve içinde AD FS ve WAP sunucusunun olması nedeniyle farklı bölgelerdeki sanal ağlar arasında herhangi bir bağlantı olmadan çalışabilir. 

## <a name="steps-to-integrate-azure-traffic-manager"></a>Azure Traffic Manager’ı tümleştirme adımları
### <a name="deploy-ad-fs-in-the-new-geographical-region"></a>Yeni coğrafi bölgede AD FS dağıtımı
Aynı topolojiyi yeni coğrafi bölgeye dağıtmak için [Azure’da AD FS dağıtımı](hybrid/how-to-connect-fed-azure-adfs.md) içindeki adım ve yönergeleri izleyin.

### <a name="dns-labels-for-public-ip-addresses-of-the-internet-facing-public-load-balancers"></a>İnternet'e Yönelik (ortak) Yük Dengeleyicilerin genel IP adresleri için DNS etiketleri
Yukarıda belirtildiği gibi Azure Traffic Manager uç nokta olarak yalnızca DNS etiketlerine başvurabilir ve bu nedenle Dış Yük Dengeleyicilerin genel IP adresleri için DNS etiketleri oluşturulması önemlidir. Aşağıdaki ekran görüntüsü, ortak IP adresi için DNS etiketinizin nasıl yapılandırılacağını göstermektedir. 

![DNS Etiketi](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfabstsdnslabel.png)

### <a name="deploying-azure-traffic-manager"></a>Azure Traffic Manager’ı dağıtma
Bir traffic manager profili oluşturmak için aşağıdaki adımları izleyin. Daha fazla bilgi için [Azure Traffic Manager profilini yönetme](../traffic-manager/traffic-manager-manage-profiles.md) bölümüne de bakabilirsiniz.

1. **Traffic Manager profili oluşturun:** Traffic manager profilinize benzersiz bir ad verin. Bu profil adı, DNS adının bir parçasıdır ve Traffic Manager etki alanı ad etiketinin ön eki olarak görev yapar. Ad / ön ek, traffic manager için bir DNS etiketi oluşturmak üzere .trafficmanager.net ifadesine eklenir. Aşağıdaki ekran görüntüsünde mysts olarak ayarlanan traffic manager DNS ön ekini gösterilmektedir ve elde edilen DNS etiketi mysts.trafficmanager.net şeklinde olacaktır. 
   
    ![Traffic Manager profili oluşturma](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/trafficmanager01.png)
2. **Trafik yönlendirme yöntemi:** Traffic manager’da üç yönlendirme seçeneği vardır:
   
   * Öncelik 
   * Performans
   * Ağırlıklı
     
     Yüksek oranda duyarlı bir AD FS altyapısı elde etmek için **Performans** önerilen seçenektir. Ancak, dağıtım ihtiyaçlarınıza en uygun yönlendirme yöntemini seçebilirsiniz. AD FS işlevselliği, belirlenen yönlendirme seçeneğinden etkilenmez. Daha fazla bilgi için bkz. [Traffic Manager trafik yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md). Yukarıdaki örnek ekran görüntüsünde **Performans** yönteminin seçildiğini görebilirsiniz.
3. **Uç nokta yapılandırma:** Trafik yöneticisi sayfasında uç noktalara tıklayın ve Ekle’yi seçin. Bu işlem aşağıdaki ekran görüntüsüne benzer bir Uç nokta ekle sayfası açar
   
   ![Uç noktaları yapılandırma](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/eastfsendpoint.png)
   
   Farklı girdiler için aşağıdaki yönergeleri izleyin:
   
   **Tür:** Azure uç noktasını bir Azure genel IP adresini işaret edecek şekilde seçin.
   
   **Ad:** Uç nokta ile ilişkilendirmek istediğiniz bir ad oluşturun. Bu ad DNS adı değildir ve DNS kayıtlarıyla bir ilgisi yoktur.
   
   **Hedef kaynak türü:** Bu özelliğin değeri olarak Genel IP adresi seçin. 
   
   **Hedef kaynak:** Bu özellik, aboneliğiniz altında kullanılabilen farklı DNS etiketleri arasından seçim yapma imkanı sunar. Yapılandırmakta olduğunuz uç noktaya karşılık gelen DNS etiketini seçin.
   
   Azure Traffic Manager’ın trafiği yönlendirmesini istediğiniz her coğrafi bölge için uç nokta ekleyin.
   Daha fazla bilgi ve traffic manager’da uç nokta ekleme / yapılandırma ile ilgili ayrıntılı adımlar için [Uç nokta ekleme, devre dışı bırakma, etkinleştirme veya silme](../traffic-manager/traffic-manager-endpoints.md)
4. **Araştırmayı yapılandırın:** Traffic manager sayfasında Yapılandırma’ya tıklayın. Yapılandırma sayfasında izleyici ayarlarını HTTP bağlantı noktası 80’de araştırma ve göreli yol /adfs/araştırma için değiştirmeniz gerekir
   
    ![Araştırmayı yapılandırma](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/mystsconfig.png) 
   
   > [!NOTE]
   > **Yapılandırma tamamlandıktan sonra uç noktalarının durumunun ÇEVRİMİÇİ olarak ayarlandığından emin olun**. Tüm uç noktalar ‘düşürülmüş’ durumdaysa Azure Traffic Manager, tanının yanlış ve tüm uç noktaların ulaşılabilir olduğunu varsayarak trafiği yönlendirmeye çalışır.
   > 
   > 
5. **Azure Traffic Manager için DNS kayıtlarını değiştirme:** Federasyon hizmetiniz Azure Traffic Manager DNS adında bir CNAME olmalıdır. Federasyon hizmetine erişmeye çalışan her kişinin gerçekte Azure Traffic Manager’a ulaşması için genel DNS kayıtlarında bir CNAME oluşturun.
   
    Örneğin, fs.fabidentity.com federasyon hizmetinin Traffic Manager’a işaret etmesi için DNS kaynak kaydınızı aşağıdaki gibi olacak şekilde güncelleştirmeniz gerekir:
   
    <code>fs.fabidentity.com IN CNAME mysts.trafficmanager.net</code>

## <a name="test-the-routing-and-ad-fs-sign-in"></a>Yönlendirme AD FS oturum açmayı sınama
### <a name="routing-test"></a>Yönlendirme testi
Federasyon hizmeti DNS adına her bir bölgedeki bir makineden ping gönderilmesi son derece basit bir yönlendirme testidir. Seçilen yönlendirme yöntemine bağlı olarak gerçekte ping gönderilen uç nokta ping ekranında yansıtılır. Örneğin, performans yönlendirmeyi seçtiyseniz istemcinin bölgesine en yakın uç noktaya ulaşılır. Biri Güneydoğu Asya bölgesi ve diğeri Batı ABD olmak üzere iki farklı bölge istemci makinesinden gönderilen iki ping’in ekran görüntüsü aşağıda verilmiştir. 

![Yönlendirme testi](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/pingtest.png)

### <a name="ad-fs-sign-in-test"></a>AD FS oturum açma testi
AD FS’yi test etmenin en kolay yolu IdpInitiatedSignon.aspx sayfasının kullanılmasıdır. Bunu yapabilmek için AD FS özelliklerinde IdpInitiatedSignOn seçeneğinin etkinleştirilmesi gerekir. AD FS kurulumunuzu doğrulamak için aşağıdaki adımları izleyin

1. AD FS sunucusunda PowerShell ile aşağıdaki cmdlet’i çalıştırarak etkinleştirin. 
   Set-AdfsProperties -EnableIdPInitiatedSignonPage $true
2. Herhangi bir dış makineden https://<yourfederationservicedns>/adfs/ls/IdpInitiatedSignon.aspx sayfasına erişin
3. AD FS sayfasını aşağıdaki gibi görmeniz gerekir:
   
    ![ADFS testi - kimlik doğrulama sınaması](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest1.png)
   
    oturum açma başarılı olduğunda aşağıdaki gibi bir başarı iletisi gösterilir:
   
    ![ADFS testi - kimlik doğrulama başarılı](./media/active-directory-adfs-in-azure-with-azure-traffic-manager/adfstest2.png)

## <a name="related-links"></a>İlgili bağlantılar
* [Azure’da temel AD FS dağıtımı](hybrid/how-to-connect-fed-azure-adfs.md)
* [Microsoft Azure Traffic Manager](../traffic-manager/traffic-manager-overview.md)
* [Traffic Manager trafik yönlendirme yöntemleri](../traffic-manager/traffic-manager-routing-methods.md)

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Traffic Manager profilini yönetme](../traffic-manager/traffic-manager-manage-profiles.md)
* [Uç noktaları ekleme, devre dışı bırakma, etkinleştirme veya silme](../traffic-manager/traffic-manager-endpoints.md) 

