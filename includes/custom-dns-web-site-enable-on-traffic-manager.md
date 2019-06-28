---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/09/2018
ms.author: cephalin
ms.openlocfilehash: 67b9c0ba2566206b0e70db51844b21e5d5d3c261
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2019
ms.locfileid: "67188948"
---
Etki alanı adınız için kayıtları yayıldıktan sonra özel etki alanı adınızı Azure App Service'te web uygulamanıza erişmek için kullanılabilir olduğunu doğrulamak için tarayıcınızı kullanmayı başlatabilmeniz gerekir.

> [!NOTE]
> Bu, CNAME DNS sistem üzerinden yayılması biraz zaman alabilir. Bir hizmet gibi kullanabileceğiniz <a href="https://www.digwebinterface.com/"> https://www.digwebinterface.com/ </a> CNAME kullanılabilir olduğunu doğrulayın.
> 
> 

Bir Traffic Manager uç noktası olarak web uygulamanızın eklemediyseniz, ad çözümlemesi için Traffic Manager özel etki alanı adı yollar olarak çalışmadan önce bunu yapmanız gerekir. Traffic Manager, ardından web uygulamanıza yönlendirir. Yer alan bilgileri kullanın [Ekle veya Sil uç noktaları](../articles/traffic-manager/traffic-manager-endpoints.md) web uygulamanızı bir uç nokta Traffic Manager profilinize olarak eklenecek.

> [!NOTE]
> Web uygulamanızı bir uç nokta eklerken listelenmemişse için yapılandırıldığından emin olun **standart** App Service planı modu. Kullanmalısınız **standart** Traffic Manager ile çalışmak için web uygulamanız için modu.
> 
> 

1. Tarayıcınızda açın [Azure portalı](https://portal.azure.com).
2. İçinde **Web Apps** sekmesini seçin, web uygulaması adını tıklatın, **ayarları**ve ardından **özel etki alanları**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. İçinde **özel etki alanları** dikey penceresinde tıklayın **konak adı Ekle**.
4. Kullanım **Hostname** bu web uygulaması ile ilişkilendirmek için Traffic Manager etki alanı adını girmek için metin kutuları.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Tıklayın **doğrulama** etki alanı adı yapılandırmasını kaydetmek için.
6. Üzerine tıklayarak **doğrulama** Azure etki alanı doğrulama iş akışı devre dışı yaslanıp. Bu etki alanı sahipliğini yanı sıra ana bilgisayar adı kullanılabilirliği ve rapor başarı veya hata düzeltme konusunda tavsiyeler ile ayrıntılı hata denetler.    
7. Doğrulama başarılı bağlı **konak adı Ekle** düğmesi etkin olacak ve ata ana bilgisayar adı için mümkün olacaktır. Bir tarayıcı özel etki alanı adınızı artık gidin. Özel etki alanı adınızı kullanarak uygulama çalışan görmelisiniz. 
   
   Yapılandırma tamamlandıktan sonra özel etki alanı adı, listelenir **etki alanı adları** web uygulamanızın bölümü.

Bu noktada, tarayıcınızda Traffic Manager etki alanı adı girin ve başarıyla, web uygulamanıza sürdüğünü görmek mümkün olması gerekir.

