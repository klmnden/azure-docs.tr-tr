---
title: "CDN uç noktanız için özel bir etki alanı ekleme | Microsoft Docs"
description: "Azure CDN içeriğini özel bir etki alanına Eşle öğrenin."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: 289f8d9e-8839-4e21-b248-bef320f9dbfc
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/09/2017
ms.author: mazha
<<<<<<< HEAD
ms.openlocfilehash: fd36b94c64ad31064dbb2e0badceaee5e5bc400f
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: HT
=======
ms.openlocfilehash: ec53b91b8aba4e38a8f7cb4b010d6be2a62150d5
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
>>>>>>> 8b6419510fe31cdc0641e66eef10ecaf568f09a3
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2017
---
# <a name="add-a-custom-domain-to-your-cdn-endpoint"></a>CDN uç noktanız için özel bir etki alanı Ekle
Bir profili oluşturduktan sonra genellikle de bir veya daha fazla CDN uç noktası (bir etki alanının alt azureedge.net) HTTP ve HTTPS kullanarak içeriğinizi teslim etmek için oluşturursunuz. Varsayılan olarak, bu uç noktaya tüm, URL'ler, örneğin, yer `http(s)://contoso.azureedge.net/photo.png`). Size kolaylık sağlamak için Azure CDN özel etki alanı ilişkilendirme seçeneğiniz sağlar (örneğin, `www.contoso.com`), uç noktası ile. Bu seçenek ile içeriğinizi teslim etmek yerine uç noktanız için özel bir etki alanı kullanın. Bu seçenek, örneğin, kendi etki alanı adınızı amacıyla markalama müşterileriniz için görünür olmasını istiyorsanız yararlıdır.

Özel bir etki alanı zaten yoksa, öncelikle bir etki alanı sağlayıcısı ile satın almalısınız. Özel bir etki alanı aldıktan sonra aşağıdaki adımları izleyin:
1. [Etki alanı sağlayıcınıza DNS kayıtlarını erişim](#step-1-access-dns-records-by-using-your-domain-provider)
2. [CNAME DNS kayıtları oluşturma](#step-2-create-the-cname-dns-records)
    - Seçenek 1: Doğrudan CDN uç noktası için özel etki alanınızı eşlenmesini
    - Seçenek 2: kullanarak etki alanınız CDN uç noktası için eşleme **cdnverify** alt etki alanı 
3. [CNAME kaydı eşleme Azure içinde etkinleştir](#step-3-enable-the-cname-record-mapping-in-azure)
4. [Özel alt etki alanı CDN uç noktanız başvurduğunu doğrulayın](#step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint)
5. [(Bağımlı adım) Harita CDN uç noktası için kalıcı özel etki alanı](#step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint)

## <a name="step-1-access-dns-records-by-using-your-domain-provider"></a>1. adım: Etki alanı sağlayıcınıza kullanarak erişim DNS kayıtları

Ana bilgisayara Azure kullanıyorsanız, [DNS etki alanı](https://docs.microsoft.com/azure/dns/dns-overview), etki alanı sağlayıcısının DNS Azure DNS'ye temsilci gerekir. Daha fazla bilgi için bkz. [Bir etki alanını Azure DNS'ye devretme](https://docs.microsoft.com/azure/dns/dns-delegate-domain-azure-dns).

DNS etki alanınız işlemek için etki alanı sağlayıcınıza kullanıyorsanız, aksi takdirde, etki alanı sağlayıcınıza Web sitesine oturum açın. Sağlayıcının belgelerine danışmanlık veya etiketli web sitesi alanları için arama DNS kayıtlarını yönetme sayfasını bulun **etki alanı adı**, **DNS**, veya **adı sunucu yönetimi**. Genellikle, hesap bilgilerinizi görüntülemek ve bir bağlantı gibi arayan DNS kayıtları sayfasında bulabilirsiniz **My etki alanları**. Bazı sağlayıcılar farklı kayıt türleri eklemek için farklı bağlantıları vardır.

> [!NOTE]
> GoDaddy gibi bazı sağlayıcılarda, DNS kayıtlarında yapılan değişiklikler ayrı bir **Değişiklikleri Kaydet** bağlantısı seçilene kadar geçerlilik kazanmaz. 


## <a name="step-2-create-the-cname-dns-records"></a>2. adım: CNAME DNS kayıtları oluşturma

Bir Azure CDN uç noktası ile özel bir etki alanı kullanmadan önce ilk etki alanı sağlayıcınıza kurallı ad (CNAME) kaydı oluşturmanız gerekir. Bir CNAME kaydı, kayıt etki alanı adı sistemi (DNS) "kurallı" veya doğru etki alanı adı için bir diğer ad etki alanı adı belirterek hedef etki alanı için bir kaynak etki alanı eşlemeleri türüdür. Azure CDN için kaynak etki alanı, özel etki alanı (ve alt etki alanı) ve hedef etki alanını CDN uç noktanız olduğunda. Özel etki alanı portal ya da API uç noktasına eklediğinizde, azure CDN CNAME DNS kaydı doğrular. 

Belirli bir etki alanı ve alt etki alanı, bir CNAME kaydı gibi eşler `www.contoso.com` veya `cdn.contoso.com`; bir kök etki alanı için bir CNAME kaydı gibi eşlemek mümkün değildir `contoso.com`. Bir alt etki alanı yalnızca bir CDN uç noktasıyla ilişkili olabilir. Tüm trafiği belirtilen uç noktası için bir alt etki alanı için bir CNAME kaydı yönlendirir. Örneğin, ilişkilendirirseniz `www.contoso.com` , CDN uç noktasıyla, onu bir depolama hesabı uç noktası veya bir bulut Hizmeti uç noktası gibi başka bir Azure uç noktası ile ilişkilendiremezsiniz. Ancak, aynı etki alanından farklı bir alt etki alanları için farklı hizmet uç noktaları kullanabilirsiniz. Bu gibi durumlarda, farklı bir alt etki alanları da aynı CDN uç noktası eşleyebilirsiniz.

Özel etki alanınız CDN uç noktaya eşlemek için aşağıdaki seçeneklerden birini kullanın:

- Seçenek 1: Özel bir etki alanının CDN uç noktası için doğrudan eşleme. Hiçbir üretim trafik özel etki alanı üzerinde çalışıyorsa, özel bir etki alanı için bir CDN uç noktası doğrudan eşleyebilirsiniz. Azure Portalı'nda etki alanının kaydettirirken CDN uç noktanız için özel etki alanınızı eşleme işlemi kapalı kalma süresi etki alanı için kısa bir süre neden olabilir. CNAME eşleme girdisi şu biçimde olmalıdır: 
 
  | AD             | TÜR  | DEĞER                  |
  |------------------|-------|------------------------|
  | `www.contoso.com` | `CNAME` | `contoso.azureedge.net` |


- Seçenek 2: kullanarak etki alanınız CDN uç noktası için eşleme **cdnverify** alt etki alanı. Kesilemez üretim trafiği özel etki alanı üzerinde çalışıyorsa, CDN uç noktanız için geçici bir CNAME eşlemesi oluşturabilirsiniz. Bu seçenek ile Azure kullanma **cdnverify** alt etki alanı kullanıcıları etki alanınızın DNS eşlemesi sırasında kesinti olmadan erişebilmesi için bir ara kayıt adım sağlamak üzere kurulur.

   1. Yeni bir CNAME kaydı oluşturun ve içeren bir alt etki alanı diğer adı sağlayın **cdnverify** alt etki alanı. Örneğin, `cdnverify.www` veya `cdnverify.cdn`. 
   2. Şu biçimde, CDN uç noktası ana bilgisayar adı girin: `cdnverify.<EndpointName>.azureedge.net`. CNAME eşleme girdisi şu biçimde olmalıdır: 

   | AD                       | TÜR  | DEĞER                            |
   |----------------------------|-------|----------------------------------|
   | `cdnverify.www.contoso.com` | `CNAME` | `cdnverify.contoso.azureedge.net` | 


## <a name="step-3-enable-the-cname-record-mapping-in-azure"></a>3. adım: Azure CNAME kaydı eşlemesindeki etkinleştir

Önceki yordamlardan birini kullanarak özel etki alanınızı kaydettikten sonra Azure CDN özel etki alanı özelliği etkinleştirebilirsiniz. 

1. Oturum [Azure portal](https://portal.azure.com/) ve özel bir etki alanına eşlemek istediğiniz uç noktası ile CDN profili konumuna göz atın.  
2. İçinde **CDN profili** dikey penceresinde istediğiniz alt etki alanı ilişkilendirmek CDN uç noktası seçin.
3. Uç nokta dikey pencerenin üst sol tıklatın **özel etki alanı**. 

   ![Özel etki alanı düğmesi](./media/cdn-map-content-to-custom-domain/cdn-custom-domain-button.png)

4. İçinde **özel ana bilgisayar adı** metin kutusunda, alt etki alanı dahil olmak üzere, özel etki alanı adı girin. Örneğin, `www.contoso.com` veya `cdn.contoso.com`.

   ![Özel etki alanı iletişim ekleyin](./media/cdn-map-content-to-custom-domain/cdn-add-custom-domain-dialog.png)

5. **Ekle**'ye tıklayın.

   Azure, girdiğiniz alan adı için CNAME kaydının bulunduğunu doğrular. CNAME doğruysa, özel alan adınız doğrulanır. Ad sunucuları yaymak CNAME kaydı için biraz zaman alabilir. Etki alanınızı hemen doğrulanmaz, CNAME kaydı doğru olduğundan emin olun, sonra birkaç dakika bekleyin ve yeniden deneyin. İçin **verizon'dan Azure CDN** (standart ve Premium) uç noktaları, bu özel etki alanı ayarlarının tüm CDN uç düğümlerine yayılması 90 dakika kadar sürebilir.  


## <a name="step-4-verify-that-the-custom-subdomain-references-your-cdn-endpoint"></a>4. adım: özel alt etki alanı CDN uç noktanız başvurduğunu doğrulayın

Özel etki alanınızı kaydını tamamladıktan sonra özel alt etki alanı CDN uç noktanız başvurduğunu doğrulayın.
 
1. Uç noktada önbelleğe alınan bir ortak içeriğe olduğundan emin olun. Örneğin, CDN uç noktanız bir depolama hesabı ile ilişkili ise, CDN ortak blob kapsayıcıları içeriği önbelleğe alır. Özel etki alanı test etmek için kapsayıcı genel erişime izin verecek şekilde ayarlanır ve en az bir blob içerdiğini doğrulayın.

2. Tarayıcınızda, özel etki alanı kullanarak blob adresine gidin. Örneğin, özel etki alanınızı ise `cdn.contoso.com`, önbelleğe alınan blob URL'si aşağıdaki URL'ye benzer olmalıdır: `http://cdn.contoso.com/mypubliccontainer/acachedblob.jpg`.


## <a name="step-5-dependent-step-map-the-permanent-custom-domain-to-the-cdn-endpoint"></a>Adım 5 (bağımlı adım): kalıcı özel etki alanı için CDN uç noktası eşleme

Bu adım adım 2 bağımlı olduğundan, seçenek 2: kullanarak etki alanınız CDN uç noktası için eşleme **cdnverify** alt etki alanı. Geçici kullanıyorsanız **cdnverify** alt etki alanı ve çalışır, ardından kalıcı özel etki alanınız CDN uç noktası için eşleyebilirsiniz doğrulamadınız.

1. Etki alanı sağlayıcınızın web sitesinde kalıcı özel etki alanınız CDN uç noktaya eşlemek için bir CNAME DNS kaydı oluşturun. CNAME eşleme girdisi şu biçimde olmalıdır: 
 
   | AD             | TÜR  | DEĞER                  |
   |------------------|-------|------------------------|
   | `www.contoso.com` | `CNAME` | `contoso.azureedge.net` |
2. CNAME kaydı ile silme **cdnverify** daha önce oluşturduğunuz alt etki alanı.

## <a name="see-also"></a>Ayrıca Bkz.
[Azure içerik teslim ağı (CDN) etkinleştirme](cdn-create-new-endpoint.md)  
[Etki alanınızı Azure DNS'ye temsilci seçme](../dns/dns-domain-delegation.md)
