---
title: Azure DNS'yi Azure kaynaklarınızı ile tümleştirme | Microsoft Docs
description: Azure kaynaklarınız için DNS sağlamak üzere Azure DNS boyunca kullanmayı öğrenin.
services: dns
documentationcenter: na
author: KumudD
manager: jeconnoc
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 1/19/2018
ms.author: kumud
ms.openlocfilehash: cbc769cd7356b3057fd2aae295071b04d2e40d91
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2018
ms.locfileid: "27994391"
---
# <a name="use-azure-dns-to-provide-custom-domain-settings-for-an-azure-service"></a>Bir Azure hizmetini özel etki alanı ayarları sağlamak için Azure DNS kullanabilir

Azure DNS destek özel etki alanları veya, bir tam etki alanı adı (FQDN) sahip herhangi bir Azure kaynaklarınızı için özel bir etki alanı için DNS sağlar. Azure web uygulaması varsa ve kullanıcılarınızın ya da erişmek istediğiniz örneğidir contoso.com ya da www.contoso.com bir FQDN kullanarak. Bu makalede özel etki alanlarını kullanmak için Azure hizmeti Azure DNS ile yapılandıracağınız anlatılmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Azure DNS için özel etki alanınızı kullanmak için etki alanınızı Azure DNS'ye temsilci. Ziyaret [bir etki alanını Azure DNS'ye devretme](./dns-delegate-domain-azure-dns.md) temsilci seçmek için ad sunucularının nasıl yapılandırılacağı hakkında yönergeler için. Etki alanınızı Azure DNS bölgenizi temsilci sonra gereken DNS kayıtlarını yapılandırabilirsiniz.

Bir gösterim veya özel etki alanı için yapılandırabileceğiniz [Azure işlev uygulamalarının](#azure-function-app), [ortak IP adresleri](#public-ip-address), [uygulama hizmeti (Web uygulamaları)](#app-service-web-apps), [Blob storage](#blob-storage), ve [Azure CDN](#azure-cdn).

## <a name="azure-function-app"></a>Azure işlev uygulaması

Azure işlevi uygulamalar için özel bir etki alanı yapılandırmak için işlev uygulaması yapılandırmasına yanı sıra bir CNAME kaydı oluşturulur.
 
Gidin **diğer** > **işlev uygulaması** ve işlev uygulamanızı seçin. Tıklatın **Platform özellikleri** ve altında **ağ** tıklatın **özel etki alanları**.

![işlev uygulaması dikey](./media/dns-custom-domain/functionapp.png)

Üzerinde geçerli url Not **özel etki alanları** dikey penceresinde, bu adresi diğer ad olarak oluşturulan DNS kaydı için kullanılır.

![özel etki alanı dikey penceresi](./media/dns-custom-domain/functionshostname.png)

DNS bölgenizi gidin ve tıklayın **+ kayıt kümesine**. Üzerinde aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** tıklayın ve dikey **Tamam** oluşturabilirsiniz.

|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | myfunctionapp        | Bu etki alanı adı etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | CNAME        | Kullanım bir CNAME kaydı bir diğer ad kullanıyor.        |
|TTL     | 1        | 1 1 saat boyunca kullanılır        |
|TTL birim     | Saat        | Saatleri zaman ölçümü kullanılır         |
|Diğer ad     | adatumfunction.azurewebsites.net        | DNS adı Bu örnekte, işlev uygulaması için varsayılan olarak sağlanan adatumfunction.azurewebsites.net DNS adı olduğu için diğer ad oluşturuyorsunuz.        |

İşlevi uygulamanıza geri gidin, tıklatın **Platform özellikleri**ve altında **ağ** tıklatın **özel etki alanları**, altında **ana bilgisayar adları**tıklatın **+ ana bilgisayar adını eklemek**.

Üzerinde **ana bilgisayar adını eklemek** dikey penceresinde, CNAME kaydı girin **ana bilgisayar adı** metin alanı ve tıklatın **doğrulama**. Kayıt bulunabilmesi, mümkün olduğunda **ana bilgisayar adını eklemek** düğmesi görünür. Tıklatın **ana bilgisayar adını eklemek** diğer ad eklemek için.

![ana bilgisayar adı dikey işlevi uygulamalar ekleme](./media/dns-custom-domain/functionaddhostname.png)

## <a name="public-ip-address"></a>Genel IP adresi

Uygulama ağ geçidi, yük dengeleyici, bulut hizmeti, Resource Manager sanal makineleri gibi kaynak adresi genel IP kullanan Hizmetleri ve klasik sanal makineleri, bir CNAME kaydı için özel bir etki alanı yapılandırmak için kullanılır.

Gidin **ağ** > **genel IP adresi**, genel IP kaynağı seçin ve tıklatın **yapılandırma**. Gösterilen IP adresi bildirmek.

![genel IP dikey penceresi](./media/dns-custom-domain/publicip.png)

DNS bölgenizi gidin ve tıklayın **+ kayıt kümesine**. Üzerinde aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** tıklayın ve dikey **Tamam** oluşturabilirsiniz.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | mywebserver        | Bu etki alanı adı etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | A        | Bir A kaydı kaynak IP adresi olarak kullanın.        |
|TTL     | 1        | 1 1 saat boyunca kullanılır        |
|TTL birim     | Saat        | Saatleri zaman ölçümü kullanılır         |
|IP Adresi     | <your ip address>       | Genel IP adresi.|

![bir A kaydını oluşturun](./media/dns-custom-domain/arecord.png)

A kaydı oluşturulduktan sonra çalıştırmak `nslookup` kayıt çözümler doğrulanacak.

![genel IP dns araması](./media/dns-custom-domain/publicipnslookup.png)

## <a name="app-service-web-apps"></a>Uygulama Hizmeti (Web uygulamaları)

Aşağıdaki adımları bir app service web uygulaması için özel bir etki alanı nasıl yapılandıracağınız uygulayın.

Gidin **Web ve mobil** > **uygulama hizmeti** ve bir özel etki alanı adı yapılandırma ve tıklatın kaynağı seçin **özel etki alanları**.

Üzerinde geçerli url Not **özel etki alanları** dikey penceresinde, bu adresi diğer ad olarak oluşturulan DNS kaydı için kullanılır.

![özel etki alanlarını dikey penceresi](./media/dns-custom-domain/url.png)

DNS bölgenizi gidin ve tıklayın **+ kayıt kümesine**. Üzerinde aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** tıklayın ve dikey **Tamam** oluşturabilirsiniz.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | mywebserver        | Bu etki alanı adı etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | CNAME        | Kullanım bir CNAME kaydı bir diğer ad kullanıyor. Kaynak IP adresi kullanıldığında, bir A kaydı olarak kullanılır.        |
|TTL     | 1        | 1 1 saat boyunca kullanılır        |
|TTL birim     | Saat        | Saatleri zaman ölçümü kullanılır         |
|Diğer ad     | webserver.azurewebsites.net        | DNS adı Bu örnekte, web uygulaması için varsayılan olarak sağlanan webserver.azurewebsites.net DNS adı olduğu için diğer ad oluşturuyorsunuz.        |


![Bir CNAME kaydı oluşturun](./media/dns-custom-domain/createcnamerecord.png)

Özel etki alanı adı için yapılandırılmış uygulama hizmeti geri gidin. Tıklatın **özel etki alanları**, ardından **ana bilgisayar adları**. Oluşturduğunuz CNAME kaydı eklemek için tıklatın **+ ana bilgisayar adını eklemek**.

![Şekil 1](./media/dns-custom-domain/figure1.png)

İşlem tamamlandıktan sonra çalıştırmak **nslookup** ad çözümlemesini doğrulamak için çalışıyor.

![Şekil 1](./media/dns-custom-domain/finalnslookup.png)

Uygulama hizmeti için özel bir etki alanı eşleştirme hakkında daha fazla bilgi için [Azure Web uygulamaları için var olan bir özel DNS ad eşleme](../app-service/app-service-web-tutorial-custom-domain.md?toc=%dns%2ftoc.json).

Özel bir etki alanı satın almanız, ziyaret [Azure Web uygulamaları için özel etki alanı adı satın](../app-service/custom-dns-web-site-buydomains-web-app.md) uygulama hizmeti etki alanları hakkında daha fazla bilgi için.

## <a name="blob-storage"></a>Blob depolama

Aşağıdaki adımları asverify yöntemini kullanarak bir blob depolama hesabı için bir CNAME kaydı nasıl yapılandıracağınız uygulayın. Bu yöntem kapalı kalma süresi sağlar.

Gidin **depolama** > **depolama hesapları**, depolama hesabınızı seçin ve tıklatın **özel etki alanı**. 2. adım altında FQDN bildirmek, bu değer ilk CNAME kaydı oluşturmak için kullanılır

![BLOB Depolama özel etki alanı](./media/dns-custom-domain/blobcustomdomain.png)

DNS bölgenizi gidin ve tıklayın **+ kayıt kümesine**. Üzerinde aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** tıklayın ve dikey **Tamam** oluşturabilirsiniz.


|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | asverify.mystorageaccount        | Bu etki alanı adı etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | CNAME        | Kullanım bir CNAME kaydı bir diğer ad kullanıyor.        |
|TTL     | 1        | 1 1 saat boyunca kullanılır        |
|TTL birim     | Saat        | Saatleri zaman ölçümü kullanılır         |
|Diğer ad     | asverify.adatumfunctiona9ed.blob.core.windows.net        | DNS adı Bu örnekte, varsayılan depolama hesabı tarafından sağlanan asverify.adatumfunctiona9ed.blob.core.windows.net DNS adı olduğu için diğer ad oluşturuyorsunuz.        |

Tıklayarak, depolama hesabınıza gidin **depolama** > **depolama hesapları**, depolama hesabınızı seçin ve tıklatın **özel etki alanı**. Oluşturulan metin kutusundaki onay asverify öneki olmadan diğer ad türü ** dolaylı CNAME doğrulaması kullan öğesini tıklatıp **kaydetmek**. Bu adım tamamlandıktan sonra DNS bölgenizi dönüp asverify öneki olmadan bir CNAME kaydı oluşturun.  Ondan sonra cdnverify önekiyle CNAME kaydını silmek güvenlidir.

![BLOB Depolama özel etki alanı](./media/dns-custom-domain/indirectvalidate.png)

DNS çözümlemesi çalıştırarak doğrula`nslookup`

Ziyaret bir blob storage uç için özel bir etki alanı eşleştirme hakkında daha fazla bilgi edinmek için [Blob storage uç noktanız için özel etki alanı adı yapılandırma](../storage/blobs/storage-custom-domain-name.md?toc=%dns%2ftoc.json)

## <a name="azure-cdn"></a>Azure CDN

Aşağıdaki adımları cdnverify yöntemini kullanarak bir CDN uç noktası için bir CNAME kaydı nasıl yapılandıracağınız uygulayın. Bu yöntem kapalı kalma süresi sağlar.

Gidin **ağ** > **CDN profili**CDN profilinizi seçin ve tıklatın **uç noktaları** altında **genel**.

İle çalışma ve tıklayın uç nokta seçin **+ özel etki alanı**. Not **uç noktası ana bilgisayar adı** olarak bu değer CNAME kaydı işaret kaydı.

![CDN özel etki alanı](./media/dns-custom-domain/endpointcustomdomain.png)

DNS bölgenizi gidin ve tıklayın **+ kayıt kümesine**. Üzerinde aşağıdaki bilgileri doldurun **kayıt kümesi Ekle** tıklayın ve dikey **Tamam** oluşturabilirsiniz.

|Özellik  |Değer  |Açıklama  |
|---------|---------|---------|
|Ad     | cdnverify.mycdnendpoint        | Bu etki alanı adı etiketi ile birlikte özel etki alanı adı FQDN değerdir.        |
|Tür     | CNAME        | Kullanım bir CNAME kaydı bir diğer ad kullanıyor.        |
|TTL     | 1        | 1 1 saat boyunca kullanılır        |
|TTL birim     | Saat        | Saatleri zaman ölçümü kullanılır         |
|Diğer ad     | cdnverify.adatumcdnendpoint.azureedge.net        | DNS adı Bu örnekte, varsayılan depolama hesabı tarafından sağlanan cdnverify.adatumcdnendpoint.azureedge.net DNS adı olduğu için diğer ad oluşturuyorsunuz.        |

Tıklayarak geri CDN uç noktası için gidin **ağ** > **CDN profili**, CDN profilinizi seçin. Tıklatın **+ özel etki alanı** cdnverify öneki olmadan CNAME kaydı diğer adınızı girin ve tıklatın **Ekle**.

Bu adım tamamlandıktan sonra DNS bölgenizi dönüp cdnverify öneki olmadan bir CNAME kaydı oluşturun.  Ondan sonra cdnverify önekiyle CNAME kaydını silmek güvenlidir. CDN ve özel bir etki alanı Ara kayıt adımı olmadan yapılandırma konusunda daha fazla bilgi için ziyaret [harita Azure CDN içeriğini özel bir etki alanı için](../cdn/cdn-map-content-to-custom-domain.md?toc=%dns%2ftoc.json).

## <a name="next-steps"></a>Sonraki adımlar

Bilgi edinmek için nasıl [geriye doğru DNS Azure içinde barındırılan hizmetler için yapılandırma](dns-reverse-dns-for-azure-services.md).