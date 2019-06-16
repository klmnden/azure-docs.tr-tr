---
title: Azure depolama hesabınız için bir özel etki alanı adı yapılandırma | Microsoft Docs
description: Azure portalında bir Azure depolama hesabında Blob Depolama veya web uç kendi kurallı ad (CNAME) eşlemek için kullanın.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: normesta
ms.reviewer: seguler
ms.subservice: blobs
ms.openlocfilehash: 4f6776a5f15cf391f3a65aceb6e9e783d87a2078
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65148925"
---
# <a name="configure-a-custom-domain-name-for-your-azure-storage-account"></a>Azure depolama hesabınız için bir özel etki alanı adı yapılandırma

Azure depolama hesabınızdaki blob verilerine erişmek için özel bir etki alanı yapılandırabilirsiniz. Azure Blob Depolama için varsayılan uç nokta  *\<depolama hesabı adı >. blob.core.windows.net*. Oluşturulan web uç noktası bir parçası olarak da kullanabilirsiniz [statik Web siteleri özelliği](storage-blob-static-website.md). Özel etki alanı ve alt etki alanıyla gibi eşlerseniz *www\.contoso.com*, blob veya web uç noktası için depolama hesabınız için kullanıcılarınız bu etki, depolama hesabınızdaki blob verilerine erişmek için kullanabilirsiniz.

> [!IMPORTANT]
> Azure depolama henüz yerel olarak HTTPS ile özel etki alanlarını desteklemiyor. Şu anda yapabilecekleriniz [HTTPS üzerinden özel etki alanları kullanarak bloblara erişmek için kullanım Azure CDN](storage-https-custom-domain-cdn.md).
> 
> 
> [!NOTE]
> Depolama hesapları, hesap başına yalnızca bir özel etki alanı adı şu anda destekler. Hem web hem de blob Hizmeti uç noktaları için özel etki alanı eşlenemiyor.
> 
> [!NOTE]
> Eşleme için alt etki alanları yalnızca çalışır (örneğin, www\.contoso.com). Kök etki alanına (örneğin, contoso.com), web uç noktası kullanılabilir olmasını istediğiniz sonra sahip olduğunuz [Azure CDN özel etki alanları ile kullanma](storage-https-custom-domain-cdn.md)

Adlı bir depolama hesabında bulunan blob veriler için birkaç örnek URL'ler aşağıdaki tabloda gösterilmektedir *mystorageaccount*. Depolama hesabı için kayıtlı özel alt etki alanı *www\.contoso.com*:

| Kaynak türü | Varsayılan URL | Özel etki alanı URL'si |
| --- | --- | --- |
| Depolama hesabı | http://mystorageaccount.blob.core.windows.net | http://www.contoso.com |
| Blob |http://mystorageaccount.blob.core.windows.net/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Kök kapsayıcı | http://mystorageaccount.blob.core.windows.net/myblob veya http://mystorageaccount.blob.core.windows.net/ $root/myblob| http://www.contoso.com/myblob veya http://www.contoso.com/ $root/myblob |
| Web |  http://mystorageaccount. [zone].web.core.windows.net/$web/[indexdoc] veya http://mystorageaccount. [ Zone].Web.Core.Windows.NET/[indexdoc] veya http://mystorageaccount. [ Zone].Web.Core.Windows.NET/$Web veya http://mystorageaccount. [ Zone].Web.Core.Windows.NET/ | http://www.contoso.com/ $web veya http://www.contoso.com/ veya http://www.contoso.com/ $web / [indexdoc] veya http://www.contoso.com/ [indexdoc] |

> [!NOTE]  
> Aşağıdaki bölümlerde gösterildiği gibi tüm örnekler blob Hizmeti uç noktası için web hizmeti uç noktası için de geçerlidir.

## <a name="direct-vs-intermediary-cname-mapping"></a>Doğrudan Ara CNAME eşlemesi karşılaştırması

İle bir alt etki alanı ön eki özel etki alanınıza işaret edebilir (örneğin, www\.contoso.com) için iki yoldan biriyle, depolama hesabınızdaki blob uç nokta: 
* Doğrudan CNAME eşlemesi kullanın.
* Kullanım *asverify* Ara alt etki alanı.

### <a name="direct-cname-mapping"></a>Doğrudan CNAME eşlemesi

İlk ve en basit, yöntem, doğrudan blob uç noktası için özel etki alanı ve alt etki alanı eşleyen bir kurallı ad (CNAME) kaydı oluşturmaktır. CNAME kaydı, bir kaynak etki alanını hedef etki alanına eşleyen bir etki alanı adı sistemi (DNS) özelliği ' dir. Kendi özel etki alanı ve alt etki alanı Bizim örneğimizde kaynak etki alanıdır (*www\.contoso.com*, örneğin). Blob Hizmeti uç noktanızı hedef etki alanıdır (*mystorageaccount.blob.core.windows.net*, örneğin).

Doğrudan yöntem "özel etki alanı Kaydet" bölümünde ele alınmıştır.

### <a name="intermediary-mapping-with-asverify"></a>Aracı eşlemesi ile *asverify*

İkinci yöntem de CNAME kayıtları kullanır. Kapalı kalma süresini önlemek için ancak bunu önce özel bir alt etki alanı kullanan *asverify* Azure tarafından tanınır.

Bir blob uç noktasına özel etki alanınızı eşleme neden olabilecek kısa bir süre kapalı kalma süresi etki alanında kaydederken [Azure portalında](https://portal.azure.com). Etki alanı şu anda bir uygulamayı sıfır kapalı kalma süresi gerektiren bir hizmet düzeyi sözleşmesi (SLA) ile destekliyorsa, Azure kullanan *asverify* alt etki alanı kayıt ara adım olarak. Bu adım, DNS eşlemesi gerçekleşirken kullanıcıların etki alanınızı erişebilmesini sağlar.

Aracı yöntemi kapsamında özel bir etki alanı kullanarak kaydetmek *asverify* alt etki alanı.

## <a name="register-a-custom-domain"></a>Özel bir etki alanına kaydetme
Etki alanı, aşağıdaki deyimleri uygularsanız, bu bölümdeki yordamı kullanarak kaydedin:
* Etki alanı kullanıcılarınıza kullanılamaz durumda unconcerned olursunuz.
* Özel etki alanınız şu anda bir uygulama barındırma değil. 

Azure DNS, Azure Blob Depolama için özel DNS adını yapılandırmak için kullanabilirsiniz. Daha fazla bilgi için bkz. [Bir Azure hizmeti için özel etki alanı ayarları sağlamak üzere Azure DNS'yi kullanma](https://docs.microsoft.com/azure/dns/dns-custom-domain#blob-storage).

Özel etki alanınız şu anda kapalı kalma süresi olamaz uygulamanın destekliyorsa, kayıt yordamda özel bir etki alanı kullanarak *asverify* alt etki alanı.

Özel etki alanı yapılandırmak için DNS'de yeni bir CNAME kaydı oluşturun. CNAME kaydı, bir etki alanı adı için bir diğer ad belirtir. Bizim örneğimizde, depolama hesabının Blob Depolama uç noktasına özel etki alanınızı adresiyle eşleşir.

Genellikle, etki alanı kayıt şirketinizin Web sitesindeki etki alanının DNS ayarlarını yönetebilirsiniz. Her kayıt şirketi için bir CNAME kaydı belirtme benzer ancak biraz farklı bir yöntem olsa da, aynı kavramdır. DNS yapılandırması bazı temel etki alanı kayıt paketleri sunmayan olduğundan CNAME kaydını oluşturmadan önce etki alanı kayıt paketini yükseltme gerekebilir.

1. İçinde [Azure portalında](https://portal.azure.com)depolama hesabınıza gidin.

1. Menü bölmede altında **Blob hizmeti**seçin **özel etki alanı**.  
   **Özel etki alanı** bölmesi açılır.

1. Etki alanı kayıt şirketinizin Web sitesine oturum açın ve ardından DNS Yönetme sayfasına gidin.  
   Sayfa adında bir bölümde bulabilirsiniz **etki alanı adı**, **DNS**, veya **ad sunucusu yönetimi**.

1. CNAME'leri yönetme ile ilgili bölümü bulun.  
   Bir Gelişmiş Ayarları sayfasına gidin ve Ara gerekebilir **CNAME**, **diğer**, veya **alt etki alanlarını**.

1. Yeni bir CNAME kaydı oluşturun, bir alt etki alanı diğer adı gibi girin **www** veya **fotoğraf** (alt etki alanı gereklidir, kök etki alanlarında desteklenmez) ve ardından bir konak adı belirtin.  
   Blob Hizmeti uç noktanızı ana bilgisayar adıdır. Şu biçimdedir  *\<mystorageaccount >. blob.core.windows.net*burada *mystorageaccount* depolama hesabınızın adıdır. Öğenin #1 kullanılacak ana bilgisayar adı görünür **özel etki alanı** bölmesinde [Azure portalında](https://portal.azure.com). 

1. İçinde **özel etki alanı** bölmesinde, metin kutusuna alt etki alanı da dahil olmak üzere, özel etki alanı adını girin.  
   Örneğin, etki alanınız varsa *contoso.com* ve alt etki alanı diğer adınızı *www*, girin **www\.contoso.com**. Varsa, alt etki alanı *fotoğraf*, girin **photos.contoso.com**.

1. Özel etki alanınızı kaydetmek için işaretleyin **Kaydet**.  
   Kayıt başarılı olursa, portal depolama hesabınıza başarıyla güncelleştirildiğini bildirir.

Kullanıcılarınıza uygun izinlere sahip değilse, yeni CNAME kaydınız DNS üzerinden dağıtıldıktan sonra özel etki alanınızı kullanarak blob verilerini görüntüleyebilir.

## <a name="register-a-custom-domain-by-using-the-asverify-subdomain"></a>Özel bir etki alanını kullanarak kayıt *asverify* alt etki alanı
Özel etki alanınız şu anda hiç kapalı kalma süresi gerektiren var olan bir SLA ile bir uygulama destekliyorsa, bu bölümdeki yordamı kullanarak özel etki alanınızı kaydedin. Gelen işaret eden bir CNAME oluşturarak *asverify.\< alt etki alanı >. \<customdomain >* için *asverify.\< Depolama hesabı >. blob.core.windows.net*, etki alanınızı Azure ile önceden kaydedebilirsiniz. Ardından gelen işaret eden ikinci bir CNAME oluşturun  *\<alt etki alanı >.\< customdomain >* için  *\<storageaccount >. blob.core.windows.net*, ve ardından özel etki alanınızı trafiği blob uç noktanıza yönlendirilir.

*Asverify* alt etki alanı olan Azure tarafından tanınan özel bir alt etki alanı. Eklenmesini tarafından *asverify* kendi alt etki alanı, özel etki alanınızın etki alanı için DNS kaydı değiştirmek zorunda kalmadan tanımak için Azure izin verir. Etki alanı için DNS kaydı değiştirdiğinizde, blob uç noktası kapalı kalma süresi ile eşleştirilir.

1. İçinde [Azure portalında](https://portal.azure.com)depolama hesabınıza gidin.

1. Menü bölmede altında **Blob hizmeti**seçin **özel etki alanı**.  
   **Özel etki alanı** bölmesi açılır.

1. DNS sağlayıcınızın Web sitesinde oturum açın ve ardından DNS Yönetme sayfasına gidin.  
   Sayfa adında bir bölümde bulabilirsiniz **etki alanı adı**, **DNS**, veya **ad sunucusu yönetimi**.

1. CNAME'leri yönetme ile ilgili bölümü bulun.  
   Bir Gelişmiş Ayarları sayfasına gidin ve Ara gerekebilir **CNAME**, **diğer**, veya **alt etki alanlarını**.

1. Yeni bir CNAME kaydı oluşturun, içeren bir alt etki alanı diğer adı sağlayın *asverify* alt etki alanıyla gibi **asverify.www** veya **asverify.photos**ve ardından bir konak adı belirtin.  
   Blob Hizmeti uç noktanızı ana bilgisayar adıdır. Şu biçimdedir *asverify.\< mystorageaccount >. blob.core.windows.net*burada *mystorageaccount* depolama hesabınızın adıdır. Öğenin #2 kullanılacak ana bilgisayar adı görünür *özel etki alanı* bölmesinde [Azure portalında](https://portal.azure.com).

1. İçinde **özel etki alanı** bölmesinde, metin kutusuna alt etki alanı da dahil olmak üzere, özel etki alanı adını girin.  
   Dahil etmezseniz *asverify*. Örneğin, etki alanınız varsa *contoso.com* ve alt etki alanı diğer adınızı *www*, girin **www\.contoso.com**. Varsa, alt etki alanı *fotoğraf*, girin **photos.contoso.com**.

1. Seçin **dolaylı CNAME doğrulaması kullan** onay kutusu.

1. Özel etki alanınızı kaydetmek için işaretleyin **Kaydet**.  
   Kayıt başarılı olursa, portal depolama hesabınıza başarıyla güncelleştirildiğini bildirir. Özel etki alanınızı Azure tarafından doğrulandı, ancak trafik etki alanınıza henüz depolama hesabınıza yönlendirilmemesidir.

1. DNS sağlayıcınızın Web sitesine dönün ve ardından, alt etki alanı, blob Hizmeti uç noktası için eşleşen başka bir CNAME kaydı oluşturun.  
   Örneğin, alt etki alanı belirtin *www* veya *fotoğraf* (olmadan *asverify*) ve ana bilgisayar adı olarak belirtmeniz  *\<mystorageaccount >. blob.core.windows.net*burada *mystorageaccount* depolama hesabınızın adıdır. Bu adım ile özel etki alanınızın kayıt tamamlanmıştır.

1. Son olarak, içerir yeni oluşturulan bir CNAME kaydı silebilirsiniz *asverify* yalnızca bir ara adım olarak gerekli olan alt etki alanı.

Kullanıcılarınıza uygun izinlere sahip değilse, yeni CNAME kaydınız DNS üzerinden dağıtıldıktan sonra özel etki alanınızı kullanarak blob verilerini görüntüleyebilir.

## <a name="test-your-custom-domain"></a>Özel etki alanınızı test

Özel etki alanınızı blob Hizmeti uç noktanızla eşlendi onaylamak için depolama hesabınızda genel bir kapsayıcıda bir blob oluşturun. Ardından, aşağıdaki biçimde bir URI'yı kullanarak bir web tarayıcısında bloba erişmek: `http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Örneğin, bir web formunda erişmek için *myforms* kapsayıcısında *photos.contoso.com* özel alt etki alanı, aşağıdaki URI kullanabilirsiniz: `http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Özel bir etki alanı kaydını kaldırma

Blob Depolama uç noktanız için özel bir etki alanı kaydını silmek için aşağıdaki prosedürlerden birini kullanın.

### <a name="azure-portal"></a>Azure portal

Özel etki alanı ayarını kaldırmak için aşağıdakileri yapın:

1. İçinde [Azure portalında](https://portal.azure.com)depolama hesabınıza gidin.

1. Menü bölmede altında **Blob hizmeti**seçin **özel etki alanı**.  
   **Özel etki alanı** bölmesi açılır.

1. İçeriği özel etki alanı adınızı içeren metin kutusunun işaretini kaldırın.

1. **Kaydet** düğmesini seçin.

Özel etki alanı başarıyla kaldırıldıktan sonra depolama hesabınızı başarıyla güncelleştirildiğini portal bir bildirim görür.

### <a name="azure-cli"></a>Azure CLI

Özel etki alanı kaydını kaldırmak için kullanın [az depolama hesabını güncelleştirme](https://docs.microsoft.com/cli/azure/storage/account) CLI komutunu ve ardından boş bir dize belirtin (`""`) için `--custom-domain` bağımsız değişken değeri.

* Komut biçimi:

  ```azurecli
  az storage account update \
      --name <storage-account-name> \
      --resource-group <resource-group-name> \
      --custom-domain ""
  ```

* Komut örneği:

  ```azurecli
  az storage account update \
      --name mystorageaccount \
      --resource-group myresourcegroup \
      --custom-domain ""
  ```

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Özel etki alanı kaydını kaldırmak için kullanın [kümesi AzStorageAccount](/powershell/module/az.storage/set-azstorageaccount) PowerShell cmdlet'ini ve ardından boş bir dize belirtin (`""`) için `-CustomDomainName` bağımsız değişken değeri.

* Komut biçimi:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Komut örneği:

  ```powershell
  Set-AzStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Azure Content Delivery Network (CDN) uç noktasına özel etki alanı eşleme](../../cdn/cdn-map-content-to-custom-domain.md)
* [HTTPS üzerinden özel etki alanları kullanarak bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md)
* [Azure Blob Depolama (Önizleme) statik Web sitesi barındırma](storage-blob-static-website.md)
