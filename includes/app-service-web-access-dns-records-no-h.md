---
author: cephalin
ms.service: app-service-web
ms.topic: include
ms.date: 11/03/2016
ms.author: cephalin
ms.openlocfilehash: d001d76bca5b9a0837349b6e05b3b0a271ea3a73
ms.sourcegitcommit: 67625c53d466c7b04993e995a0d5f87acf7da121
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2019
ms.locfileid: "65920690"
---
> [!NOTE]
> Azure Web Apps’inize yönelik bir özel DNS adını yapılandırmak için Azure DNS’yi kullanabilirsiniz. Daha fazla bilgi için bkz. [Bir Azure hizmeti için özel etki alanı ayarları sağlamak üzere Azure DNS'yi kullanma](../articles/dns/dns-custom-domain.md#app-service-web-apps).
>
>

Etki alanı sağlayıcınızın web sitesinde oturum açın.

DNS kayıtlarını yönetme sayfasını bulun. Her etki alanı sağlayıcısının kendi DNS kayıtları arabirimi vardır, bu nedenle sağlayıcının belgelerine başvurun. Sitede **Domain Name**, **DNS** veya **Name Server Management** etiketli alanları bulun. 

Çoğunlukla, hesap bilgilerinizi görüntüleyip ardından **My domains** (Etki alanlarım) gibi bir bağlantı arayarak DNS kayıtları sayfasını bulabilirsiniz. O sayfaya gidin ve **Zone file**, **DNS Records** veya **Advanced configuration** gibi bir adı olan bağlantıyı arayın.

DNS kayıtları sayfasının bir örneğini aşağıdaki ekran görüntüsünde görebilirsiniz:

![Örnek DNS kayıtları sayfası](./media/app-service-web-access-dns-records-no-h/example-record-ui.png)

Örnek ekran görüntüsünde, kayıt oluşturmak için **Add** (Ekle) öğesini seçersiniz. Bazı sağlayıcıların farklı kayıt türlerini eklemek için farklı bağlantıları vardır. Yeniden belirtelim; sağlayıcının belgelerine başvurun.

> [!NOTE]
> GoDaddy gibi bazı sağlayıcılarda, DNS kayıtlarında yapılan değişiklikler ayrı bir **Değişiklikleri Kaydet** bağlantısı seçilene kadar geçerlilik kazanmaz. 
