---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/09/2018
ms.author: cephalin
ms.openlocfilehash: f42a97cdd74d360bc047ef561cbe626d526f9e4a
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66136333"
---
Etki alanı adı sistemi (DNS), internet'te şeyler bulmak için kullanılır. Örneğin, tarayıcınızda bir adres girin veya bir web sayfası bağlantısına tıklayın, etki alanı bir IP adresine çevirmek için DNS kullanır. IP adresi, bir açık adres gibi tür olmakla birlikte çok İnsan kolay değil. Örneğin, bir DNS adı gibi çok kolay olan **contoso.com** 192.168.1.88 veya 2001:0:4137:1f67:24a2:3888:9cce:fea3 gibi bir IP adresi hatırlamak yerine.

DNS sistem dayanır *kayıtları*. Kayıtları belirli bir ilişkilendirme *adı*, gibi **contoso.com**, bir IP adresi veya başka bir DNS adı. Bir uygulama, bir web tarayıcısı gibi bir ad DNS arar, kaydı bulur ve her adresi olarak işaret kullanır. Tarayıcı, işaret değeri bir IP adresi varsa, bu değeri kullanır. Başka bir DNS adını işaret ediyorsa, uygulama, çözümü yeniden bulunduğuyla ilgilidir. Sonuç olarak, tüm ad çözümlemesi, bir IP adresi sona erecek.

Bir Azure Web sitesi oluşturduğunuzda, bir DNS adı otomatik olarak siteye atanmış. Bu ad biçimi alır  **&lt;yoursitename&gt;. azurewebsites.net**. Web sitenizi bir Azure Traffic Manager uç noktası olarak eklediğinizde, Web sitenizi ardından aracılığıyla erişilebilir  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** etki alanı.

> [!NOTE]
> Web sitenizi bir Traffic Manager uç noktası olarak yapılandırıldığında, kullanacağınız **. trafficmanager.net** adresi DNS kayıtlarını oluştururken.
> 
> Traffic Manager ile CNAME kayıtlarına yalnızca kullanabilirsiniz
> 
> 

Ayrıca birden çok kayıt, her biri kendi işlevleri ve sınırlamalar da vardır, ancak için Traffic Manager uç noktaları olarak yapılandırılmış. Web siteleri için biz yalnızca yaklaşık bir dikkatli olun; *CNAME* kaydeder.

### <a name="cname-or-alias-record"></a>CNAME ya da diğer ad kaydı
Eşleyen bir CNAME kaydı bir *belirli* DNS adı, örneğin **mail.contoso.com** veya **www\.contoso.com**, başka bir (kurallı) etki alanı adı. Azure Traffic Manager'ı kullanarak Web. siteleri söz konusu olduğunda kurallı bir etki alanı adıdır  **&lt;Uygulamam >. trafficmanager.net** Traffic Manager profilinizin etki alanı adı. Oluşturulduktan sonra CNAME için bir diğer ad oluşturur  **&lt;Uygulamam >. trafficmanager.net** etki alanı adı. CNAME girişi IP adresine çözümlenmesi,  **&lt;Uygulamam >. trafficmanager.net** etki alanı adı otomatik olarak, böylece Web sitesinin IP adresi değişirse, herhangi bir eylemde bulunmanız gerekmez.

Trafik Traffic Manager, geldikten sonra trafik Yük Dengeleme için yapılandırılan yöntemini kullanarak Web sitenizi yönlendirir. Bu Web sitenizi ziyaret edenler için tamamen saydamdır. Yalnızca özel etki alanı adı tarayıcısında görürler.

> [!NOTE]
> Bazı etki alanı kayıt yalnızca bir CNAME kaydı kullanarak alt etki alanları eşleyin izin **www\.contoso.com**ve değil kök adları gibi **contoso.com**. CNAME kayıtları hakkında daha fazla bilgi için bkz: şirketiniz tarafından sağlanan belgelere <a href="https://en.wikipedia.org/wiki/CNAME_record">CNAME kaydı Wikipedia girdiye</a>, veya <a href="https://tools.ietf.org/html/rfc1035">IETF etki alanı adları - uygulama ve belirtimi</a> belge.

