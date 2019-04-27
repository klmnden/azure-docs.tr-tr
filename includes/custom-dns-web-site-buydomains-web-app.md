---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/09/2018
ms.author: cephalin
ms.openlocfilehash: ce949caa2b80c08f1015ee21c00197d6a95103c2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60531097"
---
Bir etki alanı istiyorsanız, şirket etki alanları satın alabileceğiniz [Azure Yönetim Portalı](https://portal.azure.com) doğrudan. Etki alanı adları satın alın ve web uygulamanıza atamak için aşağıdaki adımları kullanın.

1. Tarayıcınızda açın [Azure Yönetim Portalı](https://portal.azure.com).
2. İçinde **Web Apps** sekmesini seçin, web uygulaması adını tıklatın, **ayarları**ve ardından **özel etki alanları ve SSL**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. İçinde **özel etki alanları ve SSL** dikey penceresinde tıklayın **etki alanları satın**.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-buydomains-1.png)
4. İçinde **etki alanları satın** dikey penceresinde satın almak istediğiniz etki alanı adını girmek için metin kutusunu kullanın. Önerilen kullanılabilir etki alanları gösterilecek metin kutusunda yalnızca sizin olabilir. Satın almak istediğiniz hangi etki alanını seçin.
   
   ![](./media/custom-dns-web-site/dncmntask-cname-buydomains-2.png)
5. Tıklayın **irtibat bilgileri** ve etki alanı bilgilerini formu doldurun.
   
   ![](./media/custom-dns-web-site/dncmntask-cname-buydomains-3.png)
6. Tıklayın **seçin** üzerinde **etki alanları satın** dikey, ardından görürsünüz satın alma bilgileri üzerinde **satın alma onayı** dikey penceresi. Yasal koşulları kabul edin ve'ı tıklatırsanız **satın**siparişinizi gönderilir ve satın alma işlemi izleyebilirsiniz **bildirim**.
   
   ![](./media/custom-dns-web-site/dncmntask-cname-buydomains-4.png)
   
   ![](./media/custom-dns-web-site/dncmntask-cname-buydomains-5.png)
7. Bir etki alanı başarıyla sıralı, etki alanını yönetin ve web uygulamanıza atayın. Tıklayın **"..."** etki alanının sağ tarafındaki. Daha sonra **satın alma iptal** veya **Yönet etki alanı**. Tıklayın **Yönet etki alanı**, biz bağlayabilirsiniz sonra **alt etki alanı** web uygulamamız için **Yönet etki alanı** dikey penceresi.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-buydomains-6.png)
   
    Yapılandırma tamamlandıktan sonra özel etki alanı adı, listelenir **konak adı bağlamaları** web uygulamanızın bölümü.

Bu noktada, tarayıcınızda özel etki alanı adı girin ve başarıyla, web uygulamanıza sürdüğünü görmek mümkün olması gerekir.

