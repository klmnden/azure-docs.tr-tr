---
title: "Azure Traffic Manager&quot;da uç noktaları yönetme | Microsoft Belgeleri"
description: "Bu makale, Azure Traffic Manager&quot;da uç noktalar ekleme, kaldırma, etkinleştirme ve devre dışı bırakma konularında size yardımcı olacaktır."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 52f6d4f3e68e5eb120ee499827cc8549b8e547fd

---

# <a name="add-disable-enable-or-delete-endpoints"></a>Uç noktaları ekleme, devre dışı bırakma, etkinleştirme veya silme

Azure App Service'teki Web Apps özelliği, web sitesi modundan bağımsız olarak bir veri merkezi içindeki web siteleri için yük devretme ve hepsini bir kez deneme trafik yönlendirme işlevini zaten sağlamaktadır. Azure Traffic Manager, farklı veri merkezlerinde bulunan web siteleri ve bulut hizmetleri için yük devretme ve hepsini bir kez deneme trafik yönlendirmesini belirtmenize olanak tanır. Bu işlevin sağlanması için gereken ilk adım, bulut hizmeti veya web sitesi uç noktasını Traffic Manager'a eklemektir.

> [!NOTE]
> Bu makalede, klasik portalın nasıl kullanılacağı açıklanmıştır. Klasik Azure portalı, yalnızca bulut hizmetlerinin ve Web uygulamalarının oluşturulmasını ve uç noktalar olarak atanmasını destekler. Yeni [Azure portal](https://portal.azure.com) tercih edilen arabirimdir.

Aynı zamanda bir Traffic Manager profilinin parçası olan tekil uç noktalarını da devre dışı bırakabilirsiniz. Devre dışı bırakılan bir uç nokta profilin parçası olmaya devam eder ancak profil, uç nokta profile dahil edilmemiş gibi davranır. Bu eylem, bakım modunda veya yeniden dağıtılmakta olan bir uç noktanın geçici olarak kaldırılmasında yararlı olur. Uç nokta yeniden çalışır duruma geldiği zaman etkinleştirilebilir.

> [!NOTE]
> Bir uç noktanın devre dışı bırakılması Azure'daki dağıtım durumu ile hiçbir şekilde bağlantılı değildir. Sağlıklı bir uç nokta Traffic Manager'da devre dışı bırakıldığında bile çalışır durumda kalır ve trafik alabilir. Ek olarak, bir uç noktanın bir profilde devre dışı bırakılması başka bir profildeki durumunu etkilemez.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Bir bulut hizmeti veya web sitesi uç noktası ekleme

1. Klasik Azure portalındaki Traffic Manager bölmesinde, değiştirmek istediğiniz uç nokta ayarlarını içeren Traffic Manager profilini bulun. Ayarlar sayfasını açmak için profil adının sağında yer alan oka tıklayın.
2. Yapılandırmanızın zaten parçası olan uç noktaları görüntülemek için sayfanın üst kısmındaki **Uç Noktalar**'a tıklayın.
3. **Hizmet Uç Noktaları Ekleme** sayfasına erişmek için sayfanın alt kısmındaki **Ekle**'ye tıklayın. Varsayılan olarak, sayfada **Hizmet Uç Noktaları** altındaki bulut hizmetleri listelenir.
4. Bulut hizmetlerini bu profil için uç noktalar olarak eklemek üzere listeden bulut hizmetlerini seçin. Bulut hizmeti adı temizlendiği zaman uç noktalar listesinden kaldırılır.
5. Web siteleri için **Hizmet Türü** açılan listesine tıklayın ve ardından **Web uygulaması** seçeneğini belirleyin.
6. Bu profil için uç noktalar olarak eklemek üzere listeden web sitelerini seçin. Web sitesi adı temizlendiği zaman uç noktalar listesinden kaldırılır. Her Azure veri merkezi (bölge olarak da bilinir) için tek bir web sitesi seçebilirsiniz. İlk web sitesini seçtiğinizde, aynı veri merkezindeki diğer web siteleri seçilemez. Ayrıca, yalnızca Standart web sitelerinin listelendiğini unutmayın.
7. Bu profil için uç noktaları seçtikten sonra, sağ alt bölümdeki onay işaretine tıklayarak değişikliklerinizi kaydedin.

> [!NOTE]
> *Yük devretme* trafik yönlendirme yöntemini kullanarak bir uç noktayı profile eklediğinizde veya profilden kaldırdığınızda, yük devretme öncelik listesi istediğiniz şekilde sıralanmayabilir. Yapılandırma sayfasında, Yük Devretme Önceliği Listesinin sırasını ayarlayabilirsiniz. Daha fazla bilgi için bkz. [Yük devretme trafik yönlendirmesini yapılandırma](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Bir uç noktayı devre dışı bırakma

1. Klasik Azure portalındaki Traffic Manager bölmesinde, değiştirmek istediğiniz uç nokta ayarlarını içeren Traffic Manager profilini bulun. Ayarlar sayfasını açmak için profil adının sağında yer alan oka tıklayın.
2. Yapılandırmanıza dahil edilen uç noktaları görüntülemek için sayfanın üst kısmındaki **Uç Noktalar**'a tıklayın.
3. Devre dışı bırakmak istediğiniz uç noktaya tıklayın ve ardından sayfanın alt kısmındaki **Devre Dışı Bırak**'a tıklayın.
4. İstemciler, Yaşam Süresi (TTL) boyunca uç noktaya trafik göndermeye devam eder. Traffic Manager profilinin Yapılandırma sayfasında TTL'yi değiştirebilirsiniz.

## <a name="to-enable-an-endpoint"></a>Bir uç noktayı etkinleştirme

1. Klasik Azure portalındaki Traffic Manager bölmesinde, değiştirmek istediğiniz uç nokta ayarlarını içeren Traffic Manager profilini bulun. Ayarlar sayfasını açmak için profil adının sağında yer alan oka tıklayın.
2. Yapılandırmanıza dahil edilen uç noktaları görüntülemek için sayfanın üst kısmındaki **Uç Noktalar**'a tıklayın.
3. Etkinleştirmek istediğiniz uç noktaya tıklayın ve ardından sayfanın alt kısmındaki **Etkinleştir**'e tıklayın.
4. İstemciler, profil tarafından belirlenen etkin uç noktaya yönlendirilir.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Bir bulut hizmetini veya web sitesi uç noktasını silme

1. Klasik Azure portalındaki Traffic Manager bölmesinde, değiştirmek istediğiniz uç nokta ayarlarını içeren Traffic Manager profilini bulun. Ayarlar sayfasını açmak için profil adının sağında yer alan oka tıklayın.
2. Yapılandırmanızın zaten parçası olan uç noktaları görüntülemek için sayfanın üst kısmındaki **Uç Noktalar**'a tıklayın.
3. Uç Noktalar sayfasında, profilden silmek istediğiniz uç noktanın adına tıklayın.
4. Sayfanın alt kısmındaki **Sil**'e tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Traffic Manager profillerini yönetme](traffic-manager-manage-profiles.md)
* [Yönlendirme yöntemlerini yapılandırma](traffic-manager-configure-routing-method.md)
* [Düzeyi düşürülmüş Traffic Manager durumu için sorun giderme](traffic-manager-troubleshooting-degraded.md)
* [Traffic Manager için performans konuları](traffic-manager-performance-considerations.md)
* [Traffic Manager üzerindeki işlemler (REST API Başvurusu)](http://go.microsoft.com/fwlink/p/?LinkID=313584)




<!--HONumber=Nov16_HO5-->


