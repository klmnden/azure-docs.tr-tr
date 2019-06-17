---
title: Bulut Hizmetleri'nde bir özel etki alanı adı yapılandırma | Microsoft Docs
description: DNS ayarlarını yapılandırarak Azure uygulamanızı veya internet'te özel bir etki alanı için verileri kullanıma sunma konusunda bilgi edinin.  Bu örnekler, Azure portalını kullanın.
services: cloud-services
documentationcenter: .net
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: jeconnoc
ms.openlocfilehash: e210882cb773718f68e9178cbbce6874c2729744
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67063609"
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Azure bulut hizmeti için bir özel etki alanı adı yapılandırma
Azure bulut hizmeti oluşturma zaman, bir alt etki alanı için atar **cloudapp.net**. Bulut hizmetinizin "contoso" ise, örneğin, kullanıcılarınız gibi bir URL uygulamanıza erişmek mümkün olacaktır `http://contoso.cloudapp.net`. Azure Ayrıca, sanal bir IP adresi atar.

Ancak, ayrıca uygulamanızı kendi etki alanı adına gibi getirebilir **contoso.com**. Bu makalede, ayırabilir veya Bulut hizmeti web rolleri için bir özel etki alanı adı yapılandırma açıklanmaktadır.

CNAME ve A kayıtları ne olduğunu zaten biliyor musunuz? [Açıklama son atlama](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> Azure Cloud Services için bu görevi yordamları uygulayın. Uygulama hizmetleri için bkz: [mevcut bir özel DNS adını Azure Web Apps ile eşleme](../app-service/app-service-web-tutorial-custom-domain.md). Depolama hesapları için bkz: [bir Azure Blob Depolama uç noktanız için özel etki alanı adı yapılandırma](../storage/blobs/storage-custom-domain-name.md).
> 
> 

<p/>

> [!TIP]
> Daha hızlı bir şekilde--başlayın yeni Azure kullanın [adım adım destekli çözüm](https://support.microsoft.com/kb/2990804)!  Özel etki alanı adı (SSL) ve güvenliğini sağlama iletişim ilişkilendirme Azure Cloud Services veya Azure Web siteleri bir ek kolaylaştırır.
> 
> 

## <a name="understand-cname-and-a-records"></a>CNAME ve A kayıtları anlama
CNAME (veya diğer ad kayıtlarını) ve hem A kayıtlarının bir etki alanı adı belirli bir sunucu ile ilişkilendirme (veya bu durumda, hizmet) ancak bunlar farklı şekilde çalışır olanak sağlar. Aynı zamanda bir kayıt hangi kullanmaya karar vermeden önce göz önünde bulundurmanız gereken Azure bulut Hizmetleri ile kullanırken ayrıca bazı belirli durumlar vardır.

### <a name="cname-or-alias-record"></a>CNAME ya da diğer ad kaydı
Eşleyen bir CNAME kaydı bir *belirli* etki alanı gibi **contoso.com** veya **www\.contoso.com**, kurallı bir etki alanı adı. Bu durumda, kurallı bir etki alanı adıdır **[Uygulamam] .cloudapp .net** barındırılan uygulama Azure etki alanı adı. Oluşturulduktan sonra CNAME için bir diğer ad oluşturur **[Uygulamam] .cloudapp .net**. CNAME girişi IP adresine çözümlenmesi, **[Uygulamam] .cloudapp .net** bulut hizmetinin IP adresi değişirse, herhangi bir eylemde bulunmanız gerekmez şekilde otomatik olarak hizmet.

> [!NOTE]
> Bazı etki alanı kayıt yalnızca www gibi bir CNAME kaydı kullanarak alt etki alanlarını eşleme izin\.contoso.com ve contoso.com gibi kök adı değil. CNAME kayıtları hakkında daha fazla bilgi için bkz: şirketiniz tarafından sağlanan belgelere [CNAME kaydı Wikipedia girdiye](https://en.wikipedia.org/wiki/CNAME_record), veya [IETF etki alanı adları - uygulama ve belirtimi](https://tools.ietf.org/html/rfc1035) belge.

### <a name="a-record"></a>Bir kaydı
Bir *A* kayıt eşleyen bir etki alanı gibi **contoso.com** veya **www\.contoso.com**, *veya bir joker karakter etki alanını* gibi **\*. contoso.com**, bir IP adresi. Azure bulut hizmeti söz konusu olduğunda, hizmet sanal IP'si. Bir A kaydı bir CNAME kaydı üzerinden ana avantajı gibi bir joker karakter kullanan bir giriş olabilir, bu nedenle \* **. contoso.com**, hangi işleme birden çok alt etki alanları için istekleri gibi  **Mail.contoso.com**, **login.contoso.com**, veya **www\.contso.com**.

> [!NOTE]
> Bir A kaydı bir statik IP adresine eşlenir olduğundan, bu değişiklikleri otomatik olarak bulut hizmetinizin IP adresine çözümlenemiyor. Bulut hizmetiniz tarafından kullanılan IP adresi boş bir yuvaya (üretim veya hazırlama.) dağıttığınız ilk kez ayrılır Dağıtım yuvası için silerseniz, IP adresi, Azure tarafından sunulan ve yeni bir IP adresi tüm gelecekteki dağıtımlar yuvasına verilebilir.
> 
> Rahat, bir dağıtım yuvasını (üretim veya hazırlama) IP adresini hazırlama ve üretim dağıtımları veya mevcut dağıtımının yerinde yükseltme gerçekleştirme takası zaman kalıcı hale getirilir. Bu eylemler gerçekleştirme hakkında daha fazla bilgi için bkz. [cloud services nasıl yönetilir](cloud-services-how-to-manage-portal.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Özel etki alanınız için bir CNAME kaydı ekleyin
CNAME kaydı oluşturmak için yeni bir giriş DNS tablosunda özel etki alanınız için şirketiniz tarafından sağlanan araçları kullanarak eklemeniz gerekir. Her kayıt şirketi için bir CNAME kaydı belirtme benzer ancak biraz farklı bir yöntem olsa da, aynı kavramlardır.

1. Bulmak için aşağıdaki yöntemlerden birini kullanın **. cloudapp.net** , bulut hizmetine atanan etki alanı adı.

   * Oturum açma [Azure portal], bulut hizmetinizi seçin, bakmak **genel bakış** bölümüne ve ardından bulun **Site URL'si** girişi.

       ![site URL'si gösteren Hızlı Bakış bölümü][csurl]

       **VEYA**
   * Yükleme ve yapılandırma [Azure Powershell](/powershell/azure/overview)ve ardından aşağıdaki komutu kullanın:

       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```

     Her iki yöntem tarafından döndürülen URL'nin bir CNAME kaydı oluştururken ihtiyaç olarak kullanılan etki alanı adını kaydedin.
2. DNS kaydedicinizin Web sitesinde oturum açın ve DNS Yönetme sayfasına gidin. Bağlantılar veya alanları olarak etiketlenmiş sitenin arayın **etki alanı adı**, **DNS**, veya **ad sunucusu yönetimi**.
3. Şimdi burada seçin veya girin CNAME'ın bulun. Aşağı açılan bir kayıt türü seçin veya bir Gelişmiş ayarlar sayfasına gitmek zorunda kalabilirsiniz. Sözcüklerini görünmelidir **CNAME**, **diğer**, veya **alt etki alanlarını**.
4. Ayrıca etki alanı veya alt etki alanı diğer adı için CNAME, gibi sağlamalısınız **www** için bir diğer ad oluşturmak istiyorsanız **www\.customdomain.com**. Kök etki alanı için bir diğer ad oluşturmak istiyorsanız, bunu olarak listelenebilir ' **\@** ' sembol kayıt şirketinizin DNS araçlar.
5. Ardından, uygulamanızın bir canonical konak adı sağlamalısınız **cloudapp.net** etki alanında bu durumda.

Örneğin, aşağıdaki CNAME kaydı giden tüm trafiği iletir **www\.contoso.com** için **contoso.cloudapp.net**, dağıtılan uygulamanız özel etki alanı adı:

| Diğer ad/konak adı/alt etki alanı | Kurallı bir etki alanı |
| --- | --- |
| www |contoso.cloudapp.net |

> [!NOTE]
> Bir ziyaretçi **www\.contoso.com** iletme işlemi son kullanıcıya görünmez, bu nedenle hiçbir zaman true ana bilgisayar (contoso.cloudapp.net) görürsünüz.
> 
> Yukarıdaki örnekte trafiği yalnızca uygulandığı **www** alt etki alanı. CNAME kayıtları ile joker karakterler kullanılamaz olduğundan, her etki alanı/alt etki alanı için bir CNAME oluşturmanız gerekir. Gibi alt etki alanlarını, gelen trafiği yönlendirmek istiyorsanız *. contoso.com cloudapp.net adresinizi yapılandırabileceğiniz bir **URL'sini yeniden yönlendirme** veya **İleri URL** girişi DNS ayarlarınızı veya bir A kaydı oluşturun.

## <a name="add-an-a-record-for-your-custom-domain"></a>Özel etki alanınız için bir A kaydı ekleme
Bir A kaydı oluşturmak için sanal IP adresi bulut hizmetinizin bulmalıdır. Ardından yeni bir giriş özel etki alanınız için DNS tablosunda şirketiniz tarafından sağlanan araçları kullanarak ekleyin. Her kayıt şirketi için bir A kaydı belirtme benzer ancak biraz farklı bir yöntem olsa da, aynı kavramlardır.

1. Bulut hizmetinizin IP adresini almak için aşağıdaki yöntemlerden birini kullanın.

   * Oturum açma [Azure portal], bulut hizmetinizi seçin, bakmak **genel bakış** bölümüne ve ardından bulun **genel IP adresleri** girişi.

       ![Hızlı Bakış bölümünde VIP gösteriliyor][vip]

       **VEYA**
   * Yükleme ve yapılandırma [Azure Powershell](/powershell/azure/overview)ve ardından aşağıdaki komutu kullanın:

       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```

     Bir A kaydı oluştururken ihtiyacınız olacağı IP adresini kaydedin.
2. DNS kaydedicinizin Web sitesinde oturum açın ve DNS Yönetme sayfasına gidin. Bağlantılar veya alanları olarak etiketlenmiş sitenin arayın **etki alanı adı**, **DNS**, veya **ad sunucusu yönetimi**.
3. Şimdi burada seçin veya bir kaydın girin bulun. Aşağı açılan bir kayıt türü seçin veya bir Gelişmiş ayarlar sayfasına gitmek zorunda kalabilirsiniz.
4. Etki alanı ya da bu A kaydını kullanacak bir alt etki alanı girin veya seçin. Örneğin, **www** için bir diğer ad oluşturmak istiyorsanız **www\.customdomain.com**. Tüm alt etki alanları için bir joker karakter girişi oluşturmak isterseniz, girin ' ***'. Bu gibi tüm alt etki alanlarını kapsar **mail.customdomain.com**, **login.customdomain.com**, ve **www\.customdomain.com**.

    Kök etki alanı için bir A kaydı oluşturmak istiyorsanız, bunu olarak listelenebilir ' **\@** ' sembol kayıt şirketinizin DNS araçlar.
5. Sağlanan alana, bulut hizmetinin IP adresini girin. Bu, bulut hizmeti dağıtımı IP adresi ile A kaydı kullanılan etki alanı giriş ilişkilendirir.

Örneğin, bir kaydı iletir giden tüm trafiği aşağıdaki **contoso.com** için **137.135.70.239**, dağıtılan uygulamanızın IP adresi:

| Ana bilgisayar adı/alt etki alanı | IP adresi |
| --- | --- |
| \@ |137.135.70.239 |

Bu örnekte, kök etki alanı için bir A kaydı oluşturma gösterilmektedir. Tüm alt etki alanlarını kapsayan bir joker karakter giriş oluşturmak istiyorsanız, şunu yazarsınız: ' ***' alt etki alanı olarak.

> [!WARNING]
> Azure'da IP adresleri, varsayılan olarak dinamiktir. Büyük olasılıkla kullanmak istersiniz bir [ayrılmış IP adresi](../virtual-network/virtual-networks-reserved-public-ip.md) IP adresiniz değişmez emin olmak için.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Cloud Services nasıl yönetilir?](cloud-services-how-to-manage-portal.md)
* [CDN İçeriğini Özel Etki Alanı ile Eşleme](../cdn/cdn-map-content-to-custom-domain.md)
* [Bulut hizmetinizin genel yapılandırma](cloud-services-how-to-configure-portal.md).
* Bilgi edinmek için nasıl [bir bulut hizmeti dağıtma](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma [ssl sertifikaları](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure portal]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
