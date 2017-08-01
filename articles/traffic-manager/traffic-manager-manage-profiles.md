---
title: "Azure Traffic Manager profillerini yönetme| Microsoft Belgeleri"
description: "Bu makale, bir Azure Traffic Manager profili oluşturma, devre dışı bırakma, etkinleştirme ve silme konularında size yardımcı olur."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: kumud
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: a5164282264124835692bc72a4ab61891aa7af9d
ms.contentlocale: tr-tr
ms.lasthandoff: 06/01/2017

---

# <a name="manage-an-azure-traffic-manager-profile"></a>Bir Azure Traffic Manager profilini yönetme

Traffic Manager profilleri, trafiğin bulut hizmetlerinize veya web sitesi uç noktalarına dağıtımını denetlemek için trafik yönlendirme yöntemlerini kullanır. Bu makalede, bu profillerin nasıl oluşturulacağı ve yönetileceği açıklanmıştır.

## <a name="create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

Azure portalını kullanarak bir Traffic Manager profili oluşturabilirsiniz. Profilinizi oluşturduktan sonra, Azure portalında uç noktaları, izleme ayarlarını ve diğer ayarları yapılandırabilirsiniz. Traffic Manager her profil için en fazla 200 uç noktayı destekler. Bununla birlikte, çoğu kullanım senaryosu yalnızca birkaç uç nokta gerektirir.

### <a name="to-create-a-traffic-manager-profile"></a>Traffic Manager profili oluşturma

1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın. Henüz bir hesabınız yoksa, [bir aylık ücretsiz denemeye](https://azure.microsoft.com/free/) kaydolabilirsiniz. 
2. **Hub** menüsünde **Yeni** > **Ağ** > **Tümünü gör**’e tıklayın, **Traffic Manager** profiline tıklayarak **Traffic Manager profili oluştur** dikey penceresini açın, ardından **Oluştur**’a tıklayın.
3. **Traffic Manager profili oluştur** dikey penceresini aşağıdaki gibi doldurun:
    1. **Ad** alanında profiliniz için bir ad belirtin. Bu adın trafficmanager.net bölgesinde benzersiz olması ve Traffic Manager profilinize erişmek için kullanılan <name>, trafficmanager.net DNS adı ile sonuçlanması gerekir.
    2. **Yönlendirme yöntemi** alanında **Öncelik** yönlendirme yöntemini seçin.
    3. **Abonelik** alanında bu profili hangi abonelik altında oluşturacağınızı seçin
    4. **Kaynak Grubu** alanında bu profilin yerleştirileceği yeni bir kaynak grubu oluşturun.
    5. **Kaynak grubu konumu** alanında kaynak grubunun konumunu seçin. Bu ayar, kaynak grubunun konumunu ifade eder ve genel olarak dağıtılacak Traffic Manager profilini etkilemez.
    6. **Oluştur**’a tıklayın.
    7. Traffic Manager profilinizin genel dağıtımı, tamamlandığında ilgili kaynak grubunda kaynaklardan biri olarak listelenir.

## <a name="disable-enable-or-delete-a-profile"></a>Bir profili devre dışı bırakma, etkinleştirme veya silme

Mevcut bir profili devre dışı bırakarak Traffic Manager’ın kullanıcı istekleri için yapılandırılan uç noktalara başvurmamasını sağlayabilirsiniz. Bir Traffic Manager profilini devre dışı bıraktığınızda, profil ve profilde yer alan bilgiler değişmeden kalır ve Traffic Manager arabiriminden düzenlenebilir.  Referanslar, profili yeniden etkinleştirdiğinizde devam eder. Azure portalında bir Traffic Manager profili oluşturduğunuzda profil otomatik olarak etkinleştirilir. Bir profilin artık gerekli olmadığına karar verirseniz profili silebilirsiniz.

### <a name="to-disable-a-profile"></a>Bir profili devre dışı bırakma

1. Özel etki alanı adı kullanıyorsanız İnternet DNS sunucunuzdaki CNAME kaydını, artık Traffic Manager profilinizi göstermeyecek şekilde değiştirin.
2. Traffic Manager profil ayarları aracılığıyla trafiğin uç noktalara yönlendirilmesi durdurulur.
3. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın.
2. Portalın arama çubuğunda, değiştirmek istediğiniz **Traffic Manager profili** adını arayın ve ardından gösterilen sonuçlardaki Traffic Manager profiline tıklayın.
3. **Traffic Manager profili** dikey penceresinde **Genel Bakış**’a, Genel Bakış dikey penceresinde **Devre Dışı Bırak**’a tıklayın ve ardından Traffic Manager profilini devre dışı bırakmak istediğinizi onaylayın.

### <a name="to-enable-a-profile"></a>Bir profili etkinleştirme

1. Bir tarayıcıdan [Azure portalında](http://portal.azure.com) oturum açın.
2. Portalın arama çubuğunda, değiştirmek istediğiniz **Traffic Manager profili** adını arayın ve ardından gösterilen sonuçlardaki Traffic Manager profiline tıklayın.
3. **Traffic Manager profili** dikey penceresinde **Genel Bakış**’a ve sonra Genel Bakış dikey penceresinde **Etkinleştir**’e tıklayın.
5. Özel etki alanı adı kullanıyorsanız İnternet DNS sunucunuzda Traffic Manager profilinizin etki alanı adını gösterecek bir CNAME kaynak kaydı oluşturun.
6. Trafik yeniden uç noktalara yönlendirilir.

### <a name="to-delete-a-profile"></a>Bir profili silme

1. İnternet DNS sunucunuzdaki DNS kaynak kaydının Traffic Manager profilinize ait etki alanı adına işaret eden bir CNAME kaynak kaydını artık kullanmadığından emin olun.
2. Portalın arama çubuğunda, değiştirmek istediğiniz **Traffic Manager profili** adını arayın ve ardından gösterilen sonuçlardaki Traffic Manager profiline tıklayın.
3. **Traffic Manager profili** dikey penceresinde **Genel Bakış**’a, Genel Bakış dikey penceresinde **Sil**’e tıklayın ve ardından Traffic Manager profilini silmek istediğinizi onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir uç nokta ekleme](traffic-manager-endpoints.md)
* [Öncelikli yönlendirme yöntemini yapılandırma](traffic-manager-configure-priority-routing-method.md)
* [Coğrafi yönlendirme yöntemini yapılandırma](traffic-manager-configure-geographic-routing-method.md) 
* [Ağırlıklı yönlendirme yöntemini yapılandırma](traffic-manager-configure-weighted-routing-method.md)
* [Performans yönlendirme yöntemini yapılandırma](traffic-manager-configure-performance-routing-method.md)

