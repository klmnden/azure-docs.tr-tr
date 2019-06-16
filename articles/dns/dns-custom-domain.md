---
title: Azure DNS'yi kullanarak Azure kaynaklarınızı tümleştirme
description: Boyunca Azure DNS, DNS, Azure kaynaklarınızı sağlamak için kullanmayı öğrenin.
services: dns
author: vhorne
ms.service: dns
ms.topic: article
ms.date: 1/18/2019
ms.author: victorh
ms.openlocfilehash: 5c098c6c22b079d586c0bd808df9af4a737c17a8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62096257"
---
# <a name="use-azure-dns-to-provide-custom-domain-settings-for-an-azure-service"></a>Bir Azure hizmeti için özel etki alanı ayarları sağlamak için Azure DNS kullanma

Azure DNS özel etki alanları desteği ya da tam etki alanı adı (FQDN) sahip tüm Azure kaynaklarınızın için özel bir etki alanı için DNS sağlar. Bir Azure web uygulamanız ve ya da erişmesini istediğiniz örnektir contoso.com veya www kullanarak\.bir FQDN olarak contoso.com. Bu makalede, Azure service ile Azure DNS özel etki alanlarını kullanmak için nasıl yapılandıracağınız anlatılmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Azure DNS için özel etki alanınızı kullanmak için etki alanınızı Azure DNS'e devretmeniz gerekir. Ziyaret [bir etki alanını Azure DNS'ye devretme](./dns-delegate-domain-azure-dns.md) ad sunucularınızın temsilcisi için yapılandırma hakkında yönergeler için. Etki alanınızda, Azure DNS bölgesini temsilci sonra gerekli DNS kayıtlarını yapılandırabilirsiniz.

Gösterim veya özel etki alanı için yapılandırabileceğiniz [Azure işlev uygulamaları](#azure-function-app), [genel IP adresleri](#public-ip-address), [App Service (Web uygulamaları)](#app-service-web-apps), [Blob Depolama](#blob-storage), ve [Azure CDN](#azure-cdn).

## <a name="azure-function-app"></a>Azure işlev uygulaması

Azure işlev uygulamaları için özel bir etki alanı yapılandırmak için işlev uygulaması yapılandırmasına yanı sıra bir CNAME kaydı oluşturulur.
 
Gidin **işlev uygulaması** ve işlev uygulamanızı seçin. Tıklayın **Platform özellikleri** altında **ağ** tıklayın **özel etki alanları**.

![işlev uygulaması dikey](./media/dns-custom-domain/functionapp.png)

Geçerli URL'sini not alın **özel etki alanları** dikey penceresinde, bu adresi diğer ad olarak oluşturulan DNS kaydı için kullanılır.

![özel etki alanı dikey penceresi](./media/dns-custom-domain/functionshostname.png)

DNS bölgenizi gelin ve tıklayın **+ kayıt kümesi**. Aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** dikey de **Tamam** oluşturun.

|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | myfunctionapp        | Bu etki alanı ad etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | CNAME        | Bir diğer ad kullanımı bir CNAME kaydı kullanıyor.        |
|TTL     | 1        | 1 saat boyunca 1 kullanılır        |
|TTL birimi     | Saat        | Saatleri zaman ölçümü kullanılır         |
|Alias     | adatumfunction.azurewebsites.net        | DNS adı varsayılan olarak işlev uygulaması için sağlanan adatumfunction.azurewebsites.net DNS adı olduğu Bu örnekte, diğer oluşturuyorsunuz.        |

İşlev uygulamanıza geri gidin, tıklayın **Platform özellikleri**, altında **ağ** tıklayın **özel etki alanları**, altında **özel ana bilgisayar adları** tıklayın **+ konak adı Ekle**.

Üzerinde **konak adı Ekle** dikey penceresinde, CNAME kaydı girin **hostname** metni alanına ve tıklayın **doğrulama**. Kaydı bulunamazsa, **konak adı Ekle** düğmesi görünür. Tıklayın **konak adı Ekle** diğer ad eklemek için.

![işlev uygulamaları konak adı dikey penceresi ekleme](./media/dns-custom-domain/functionaddhostname.png)

## <a name="public-ip-address"></a>Genel IP adresi

Özel bir etki alanı için bir genel IP kullanan hizmetler Application Gateway, yük dengeleyici, bulut hizmeti, Resource Manager Vm'lerinde gibi kaynak adres ve klasik VM'ler, bir A kaydı kullanılan yapılandırmak için.

Gidin **ağ** > **genel IP adresi**, genel IP kaynağı seçin ve tıklayın **yapılandırma**. Gösterilen IP adresiyle bildirmek.

![genel IP dikey penceresi](./media/dns-custom-domain/publicip.png)

DNS bölgenizi gelin ve tıklayın **+ kayıt kümesi**. Aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** dikey de **Tamam** oluşturun.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | mywebserver        | Bu etki alanı ad etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | A        | Kaynak IP adresi olduğu gibi bir A kaydı kullanın.        |
|TTL     | 1        | 1 saat boyunca 1 kullanılır        |
|TTL birimi     | Saat        | Saatleri zaman ölçümü kullanılır         |
|IP Adresi     | `<your ip address>`       | Genel IP adresi.|

![bir A kaydı oluşturma](./media/dns-custom-domain/arecord.png)

A kaydını oluşturulduktan sonra Çalıştır `nslookup` kayıt çözümler doğrulamak için.

![genel IP dns araması](./media/dns-custom-domain/publicipnslookup.png)

## <a name="app-service-web-apps"></a>App Service (Web uygulamaları)

Aşağıdaki adımları bir app service web uygulaması için özel bir etki alanı yapılandırma yoluyla uygulayın.

Gidin **App Service** ve bir özel etki alanı adı yapılandırma ve tıklayın kaynağı seçin **özel etki alanları**.

Geçerli URL'sini not alın **özel etki alanları** dikey penceresinde, bu adresi diğer ad olarak oluşturulan DNS kaydı için kullanılır.

![özel etki alanları dikey penceresi](./media/dns-custom-domain/url.png)

DNS bölgenizi gelin ve tıklayın **+ kayıt kümesi**. Aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** dikey de **Tamam** oluşturun.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | mywebserver        | Bu etki alanı ad etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | CNAME        | Bir diğer ad kullanımı bir CNAME kaydı kullanıyor. Kaynak IP adresi kullandıysanız, bir A kaydı kullanılabilir.        |
|TTL     | 1        | 1 saat boyunca 1 kullanılır        |
|TTL birimi     | Saat        | Saatleri zaman ölçümü kullanılır         |
|Alias     | webserver.azurewebsites.net        | DNS adı varsayılan olarak web uygulaması için sağlanan webserver.azurewebsites.net DNS adı olduğu Bu örnekte, diğer oluşturuyorsunuz.        |


![bir CNAME kaydı oluşturun](./media/dns-custom-domain/createcnamerecord.png)

Özel etki alanı adı için yapılandırılmış app Service'e geri gidin. Tıklayın **özel etki alanları**, ardından **ana bilgisayar adları**. Oluşturduğunuz CNAME kaydı eklemek için tıklatın **+ konak adı Ekle**.

![Şekil 1](./media/dns-custom-domain/figure1.png)

İşlem tamamlandıktan sonra Çalıştır **nslookup** ad çözümlemesini doğrulamak için çalışmaktadır.

![Şekil 1](./media/dns-custom-domain/finalnslookup.png)

App Service özel etki alanı eşleme hakkında daha fazla bilgi edinmek için [mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json).

Özel bir etki alanı satın almak için ihtiyacınız varsa bkz [Azure Web Apps için bir özel etki alanı adı satın alma](../app-service/manage-custom-dns-buy-domain.md) App Service etki alanları hakkında daha fazla bilgi edinmek için.

## <a name="blob-storage"></a>Blob depolama

Aşağıdaki adımları asverify yöntemi kullanarak blob depolama hesabı için bir CNAME kaydı nasıl yapılandıracağınız uygulayın. Bu yöntem, kapalı kalma süresi sağlar.

Gidin **depolama** > **depolama hesapları**, depolama hesabınızı seçin ve tıklayın **özel etki alanı**. 2\. adım altında FQDN bildirmek, bu değer, ilk CNAME kaydı oluşturmak için kullanılır

![BLOB Depolama özel etki alanı](./media/dns-custom-domain/blobcustomdomain.png)

DNS bölgenizi gelin ve tıklayın **+ kayıt kümesi**. Aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** dikey de **Tamam** oluşturun.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | asverify.mystorageaccount        | Bu etki alanı ad etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | CNAME        | Bir diğer ad kullanımı bir CNAME kaydı kullanıyor.        |
|TTL     | 1        | 1 saat boyunca 1 kullanılır        |
|TTL birimi     | Saat        | Saatleri zaman ölçümü kullanılır         |
|Alias     | asverify.adatumfunctiona9ed.blob.core.windows.net        | DNS adı Bu örnekte, varsayılan depolama hesabı tarafından sağlanan asverify.adatumfunctiona9ed.blob.core.windows.net DNS adı olduğu için diğer ad oluşturuyorsunuz.        |

Tıklayarak, depolama hesabınıza gidin **depolama** > **depolama hesapları**, depolama hesabınızı seçin ve tıklayın **özel etki alanı**. Metin kutusundaki onay asverify öneki olmadan oluşturduğunuz diğer ad türü ** dolaylı CNAME doğrulaması kullan öğesini tıklatıp **Kaydet**. Bu adım tamamlandıktan sonra DNS bölgenizi dönün ve asverify öneki olmadan bir CNAME kaydı oluşturun.  Ondan sonra ön eki cdnverify CNAME kaydını silmek güvenlidir.

![BLOB Depolama özel etki alanı](./media/dns-custom-domain/indirectvalidate.png)

Çalıştırarak DNS çözümlemesini doğrulayın. `nslookup`

Ziyaret bir blob depolama uç noktasına özel etki alanı eşleme hakkında daha fazla bilgi edinmek için [Blob Depolama uç noktanız için bir özel etki alanı adı yapılandırma](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json)

## <a name="azure-cdn"></a>Azure CDN

Aşağıdaki adımları cdnverify yöntemini kullanarak bir CDN uç noktası için bir CNAME kaydı nasıl yapılandıracağınız uygulayın. Bu yöntem, kapalı kalma süresi sağlar.

Gidin **ağ** > **CDN profilleri**, CDN profilinizi seçin.

İle çalışma ve tıklayın uç noktayı seçin **+ özel etki alanı**. Not **uç noktası ana bilgisayar** CNAME kaydı için kaydı bu değeri olduğu gibi.

![CDN özel etki alanı](./media/dns-custom-domain/endpointcustomdomain.png)

DNS bölgenizi gelin ve tıklayın **+ kayıt kümesi**. Aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** dikey de **Tamam** oluşturun.

|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | cdnverify.mycdnendpoint        | Bu etki alanı ad etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | CNAME        | Bir diğer ad kullanımı bir CNAME kaydı kullanıyor.        |
|TTL     | 1        | 1 saat boyunca 1 kullanılır        |
|TTL birimi     | Saat        | Saatleri zaman ölçümü kullanılır         |
|Alias     | cdnverify.adatumcdnendpoint.azureedge.net        | DNS adı Bu örnekte, varsayılan depolama hesabı tarafından sağlanan cdnverify.adatumcdnendpoint.azureedge.net DNS adı olduğu için diğer ad oluşturuyorsunuz.        |

Tıklayarak, CDN uç noktasına gidin **ağ** > **CDN profilleri**, CDN profilinizi seçin. Tıklayın **+ özel etki alanı** cdnverify öneki olmadan CNAME kaydı diğer adınızı girip __iade **Ekle**.

Bu adım tamamlandıktan sonra DNS bölgenizi dönün ve cdnverify öneki olmadan bir CNAME kaydı oluşturun.  Ondan sonra ön eki cdnverify CNAME kaydını silmek güvenlidir. CDN ve Ara kayıt adımı olmadan özel bir etki alanı yapılandırma hakkında daha fazla bilgi için ziyaret [harita Azure CDN içeriğini özel bir etki alanıyla](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [Azure'da barındırılan hizmetleri için ters DNS yapılandırma](dns-reverse-dns-for-azure-services.md).
