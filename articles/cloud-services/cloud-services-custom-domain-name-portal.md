---
title: "Bulut Hizmetleri'nde bir özel etki alanı adı yapılandırma | Microsoft Docs"
description: "Azure uygulamanızı veya verilerin internet'te özel bir etki alanı için DNS ayarlarını yapılandırarak öğrenin.  Bu örnekler Azure Portalı'nı kullanın."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: f5d244fc747b923989407afd50927cda2b8d4a0f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a>Bir Azure bulut hizmeti için bir özel etki alanı adı yapılandırma
> [!div class="op_single_selector"]
> * [Azure portal](cloud-services-custom-domain-name-portal.md)
> * [Klasik Azure Portalı](cloud-services-custom-domain-name.md)
> 
> 

Azure bulut hizmeti oluşturduğunuzda, bir alt etki alanı için atar **cloudapp.net**. Örneğin, bulut hizmetinizin "contoso" ise, kullanıcılarınız uygulamanızda http://contoso.cloudapp.net gibi bir URL erişebilir olacaktır. Azure de bir sanal IP adresi atar.

Ancak, aynı zamanda uygulamanız kendi etki alanı adınızı gibi getirebilir **contoso.com**. Bu makalede, ayırmak veya Bulut hizmeti web rolleri için bir özel etki alanı adı yapılandırma açıklanmaktadır.

CNAME ve A kayıtlarını nelerdir zaten biliyor musunuz? [Açıklama geçmiş atlama](#add-a-cname-record-for-your-custom-domain).

> [!NOTE]
> Bu görevde yordamlar Azure bulut Hizmetleri için geçerlidir. Uygulama hizmetleri için bkz: [bu](../app-service/app-service-web-tutorial-custom-domain.md). Depolama hesapları için bkz: [bu](../storage/blobs/storage-custom-domain-name.md).
> 
> 

<p/>

> [!TIP]
> Daha hızlı--yapmalarını yeni Azure kullanmak [izlenecek destekli](http://support.microsoft.com/kb/2990804)!  Özel etki alanı adı ve güvenliğini sağlama iletişim (SSL) ilişkilendirme Azure Cloud Services veya Azure Web siteleri bir ek kolaylaştırır.
> 
> 

## <a name="understand-cname-and-a-records"></a>CNAME ve A kayıtları anlama
CNAME (veya diğer ad kayıtları) ve her iki A kayıtlarını belirli bir sunucuyla bir etki alanı adı ilişkilendirmek (veya bu durumda, hizmet) ancak bunlar farklı şekilde çalışır izin verir. Aynı zamanda bir kayıtları kullanılacak karar vermeden önce dikkate almanız gereken Azure bulut Hizmetleri ile kullanırken ayrıca bazı belirli noktalar vardır.

### <a name="cname-or-alias-record"></a>CNAME veya diğer ad kaydı
Bir CNAME kaydı eşleyen bir *belirli* etki alanı gibi **contoso.com** veya **www.contoso.com**, kurallı etki alanı adı. Bu durumda, kurallı etki alanı adıdır **[Uygulamam] .cloudapp .net** Azure etki alanı adını barındırılan uygulama. Oluşturduktan sonra CNAME için diğer ad oluşturur **[Uygulamam] .cloudapp .net**. CNAME girişi IP adresine çözümleyecek, **[Uygulamam] .cloudapp .net** bulut hizmeti IP adresi değişirse, herhangi bir eylemde bulunmanız gerekmez şekilde otomatik olarak, hizmet.

> [!NOTE]
> Bazı etki alanı kayıt yalnızca www.contoso.com ve contoso.com gibi kök adları değil gibi bir CNAME kaydı kullanırken alt etki alanlarını eşleme izin verir. CNAME kayıtları hakkında daha fazla bilgi için şirketiniz tarafından sağlanan belgelere bakın [CNAME kaydı Wikipedia girişinde](http://en.wikipedia.org/wiki/CNAME_record), veya [IETF etki alanı adları - uygulama ve belirtim](http://tools.ietf.org/html/rfc1035) belge.
> 
> 

### <a name="a-record"></a>Bir kayıt
Bir *A* kaydı bir etki alanı gibi eşler **contoso.com** veya **www.contoso.com**, *veya bir joker karakter etki alanı* gibi  **\*. contoso.com**, bir IP adresi için. Azure bulut hizmeti söz konusu olduğunda, sanal IP hizmetinin. Bir A kaydı bir CNAME kaydı üzerinden ana avantajı gibi bir joker karakter kullanan bir giriş olduğunu nedenle \* **. contoso.com**, hangi işlemek birden çok alt etki alanı için istekleri gibi **mail.contoso.com**, **login.contoso.com**, veya **www.contso.com**.

> [!NOTE]
> Bir A kaydı bir statik IP adresine eşlenmiş olduğundan, bu değişiklikleri otomatik olarak bulut hizmetinizin IP adresi çözümlenemiyor. Bulut hizmeti tarafından kullanılan IP adresi boş bir yuvaya (üretim veya hazırlama.) dağıtmak ilk kez ayrılır Dağıtım yuvası için silerseniz, IP adresi Azure tarafından yayımlanan ve gelecekteki tüm dağıtımlar yuvaya verilen yeni bir IP adresi.
> 
> Uygun şekilde, bir belirtilen dağıtım yuvası (üretim veya hazırlama) IP adresini hazırlama ve üretim dağıtımları veya varolan bir dağıtım yerinde yükseltme gerçekleştirme arasında takası zaman kalıcıdır. Bu eylemler gerçekleştirme hakkında daha fazla bilgi için bkz: [bulut hizmetlerini yönetmek nasıl](cloud-services-how-to-manage.md).
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a>Özel etki alanınız için bir CNAME kaydı ekleyin
Bir CNAME kaydı oluşturmak için yeni bir giriş DNS tabloda özel etki alanınız için şirketiniz tarafından sağlanan araçları kullanarak eklemeniz gerekir. Her kayıt için bir CNAME kaydı belirtmenin benzer ancak biraz farklı bir yöntem olsa da, kavramlar aynıdır.

1. Bulmak için aşağıdaki yöntemlerden birini kullanın **. cloudapp.net** bulut Hizmetinize atanmış etki alanı adı.
   
   * Oturum açma [Azure portal], bulut hizmetinizi seçin, bakmak **Essentials** bölümünde ve ardından bulmak **Site URL'si** girişi.
     
       ![Hızlı Bakış bölümüne site URL'si gösterme][csurl]
     
       **VEYA**
   * Yükleme ve yapılandırma [Azure Powershell](/powershell/azure/overview)ve ardından aşağıdaki komutu kullanın:
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     Her iki yöntemi tarafından döndürülen URL bir CNAME kaydı oluştururken gerekir olarak kullanılan etki alanı adı kaydedin.
2. DNS kaydedicinizin Web sitesinde oturum açın ve DNS Yönetme sayfasına gidin. Bağlantıları veya olarak etiketli sitesinin alanları arayın **etki alanı adı**, **DNS**, veya **adı sunucu yönetimi**.
3. Şimdi burada seçin veya CNAME'ın girin bulun. Aşağı açılır bir kayıt türü seçin veya Gelişmiş Ayarları sayfasına gitmek zorunda kalabilirsiniz. Sözcükleri göz önünde bulundurmanız gerekenler **CNAME**, **diğer**, veya **alt etki alanları**.
4. Ayrıca etki alanı ya da alt etki alanı diğer adı için CNAME, gibi sağlamanız gereken **www** için diğer ad oluşturmak istiyorsanız **www.customdomain.com**. Kök etki alanı için diğer ad oluşturmak istiyorsanız, bunu olarak listelenmeyebilir '**@**' kayıt şirketinizin DNS araçlarında simgesi.
5. Ardından, uygulamanızın bir kurallı ana bilgisayar adı sağlamalısınız **cloudapp.net** bu durumda etki alanı.

Örneğin, aşağıdaki CNAME kaydı gelen tüm trafiği iletir **www.contoso.com** için **contoso.cloudapp.net**, dağıtılan uygulamanız özel etki alanı adı:

| Diğer ad/ana bilgisayar adı/alt etki alanı | Kurallı etki alanı |
| --- | --- |
| www |contoso.cloudapp.NET |

> [!NOTE]
> Bir ziyaretçi, **www.contoso.com** iletmeyi işlemdir son kullanıcıya görünmez şekilde doğru ana bilgisayar (contoso.cloudapp.net) hiçbir zaman görürsünüz.
> 
> Yukarıdaki örnekte yalnızca trafiğinin uygular **www** alt etki alanı. Sahip CNAME kayıtlarına joker karakterler kullanılamaz olduğundan, her etki alanı/alt etki alanı için bir CNAME oluşturmanız gerekir. Alt etki alanları, gelen trafiği gibi doğrudan isteyip istemediğinizi *. contoso.com cloudapp.net adresinizi yapılandırabilirsiniz bir **yeniden yönlendirme URL'si** veya **İleri URL** DNS ayarlarınız giriş veya bir A kaydını oluşturun.
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a>Özel etki alanınız için bir A kaydı ekleme
Bir A kaydı oluşturmak için öncelikle sanal IP adresine bulut hizmetinizin bulmanız gerekir. Ardından yeni bir giriş özel etki alanınız için DNS tablosundaki kayıt şirketiniz tarafından sağlanan araçları kullanarak ekleyin. Her kayıt için bir A kaydı belirtmenin benzer ancak biraz farklı bir yöntem olsa da, kavramlar aynıdır.

1. Bulut hizmetinizin IP adresini almak için aşağıdaki yöntemlerden birini kullanın.
   
   * Oturum açma [Azure portal], bulut hizmetinizi seçin, bakmak **Essentials** bölümünde ve ardından bulmak **ortak IP adresleri** girişi.
     
       ![Hızlı Bakış bölümüne VIP gösterme][vip]
     
       **VEYA**
   * Yükleme ve yapılandırma [Azure Powershell](/powershell/azure/overview)ve ardından aşağıdaki komutu kullanın:
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     Bir A kaydı oluştururken gerekir olarak IP adresini kaydedin.
2. DNS kaydedicinizin Web sitesinde oturum açın ve DNS Yönetme sayfasına gidin. Bağlantıları veya olarak etiketli sitesinin alanları arayın **etki alanı adı**, **DNS**, veya **adı sunucu yönetimi**.
3. Şimdi burada seçin veya bir kaydın girin bulun. Aşağı açılır bir kayıt türü seçin veya Gelişmiş Ayarları sayfasına gitmek zorunda kalabilirsiniz.
4. Etki alanı ya da bu A kaydı kullanacağı alt etki alanı girin veya seçin. Örneğin, seçin **www** için diğer ad oluşturmak istiyorsanız **www.customdomain.com**. Tüm alt etki alanları için bir joker karakter girişi oluşturmak istiyorsanız, girin ' ***'. Bu gibi tüm alt etki alanlarını kapsar **mail.customdomain.com**, **login.customdomain.com**, ve **www.customdomain.com**.
   
    Kök etki alanı için bir A kaydı oluşturmak istiyorsanız, bunu olarak listelenmeyebilir '**@**' kayıt şirketinizin DNS araçlarında simgesi.
5. IP adresi bulut hizmetinizin sağlanan alana girin. Bu, bulut hizmeti dağıtımınızın IP adresiyle bir kayıttaki kullanılan etki alanı girdisi ilişkilendirir.

Örneğin, bir kayıt iletir gelen tüm trafiği aşağıdaki **contoso.com** için **137.135.70.239**, dağıtılan uygulamanız IP adresi:

| Ana bilgisayar adı/alt etki alanı | IP adresi |
| --- | --- |
| @ |137.135.70.239 |

Bu örnekte, kök etki alanı için bir A kaydı oluşturmayı gösterir. Tüm alt etki alanları kapsayacak şekilde bir joker karakter girişi oluşturmak isterseniz, girersiniz ' ***' alt etki alanı olarak.

> [!WARNING]
> Azure'daki IP adresleri, varsayılan olarak dinamik. Kullanmak istersiniz bir [ayrılmış IP adresi](../virtual-network/virtual-networks-reserved-public-ip.md) IP adresiniz değişmeyen emin olmak için.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Cloud Services nasıl yönetilir?](cloud-services-how-to-manage.md)
* [CDN İçeriğini Özel Etki Alanı ile Eşleme](../cdn/cdn-map-content-to-custom-domain.md)
* [Genel yapılandırma bulut hizmetinizin](cloud-services-how-to-configure-portal.md).
* Bilgi edinmek için nasıl [bir bulut hizmetinin dağıtılacağı](cloud-services-how-to-create-deploy-portal.md).
* Yapılandırma [ssl sertifikalarını](cloud-services-configure-ssl-certificate-portal.md).

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure portal]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
