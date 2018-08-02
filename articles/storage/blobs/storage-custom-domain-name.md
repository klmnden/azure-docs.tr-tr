---
title: Azure depolama hesabınız için bir özel etki alanı adı yapılandırma | Microsoft Docs
description: Kendi kurallı ad (CNAME) bir Azure depolama hesabındaki Blob veya web uç noktasına eşlemek için Azure portalını kullanın.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 06/26/2018
ms.author: tamram
ms.component: blobs
ms.openlocfilehash: 5fd823e9105157f8292d5a9554850b0f4338a392
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2018
ms.locfileid: "39398861"
---
# <a name="configure-a-custom-domain-name-for-your-azure-storage-account"></a>Azure depolama hesabınız için bir özel etki alanı adı yapılandırma

Azure depolama hesabınızdaki blob verilerine erişmek için özel bir etki alanı yapılandırabilirsiniz. Blob Depolama için varsayılan uç nokta `<storage-account-name>.blob.core.windows.net`. Bir parçası olarak oluşturulan web uç noktası ayrıca kullanabileceğiniz [statik Web siteleri özelliği (Önizleme)](storage-blob-static-website.md). Özel etki alanı ve alt etki alanı gibi eşlerseniz **www.contoso.com** blob veya web uç noktası için depolama hesabı, kullanıcılar kullanabilirsiniz sonra erişim, etki alanı kullanarak, depolama hesabınızdaki blob verilerini.

> [!IMPORTANT]
> Azure depolama henüz yerel olarak HTTPS ile özel etki alanlarını desteklemiyor. Şu anda yapabilecekleriniz [HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md).
>

> [!NOTE]  
> Depolama hesapları, hesap başına yalnızca bir özel etki alanı adı şu anda destekler. Hem web hem de blob Hizmeti uç noktaları için bir özel etki alanı adına eşlenemez anlamına gelir.

Adlı bir depolama hesabında bulunan blob veriler için birkaç örnek URL'ler aşağıdaki tabloda gösterilmektedir **mystorageaccount**. Depolama hesabı için özel etki kaydedilen **www.contoso.com**:

| Kaynak Türü | Varsayılan URL | Özel etki alanı URL'si |
| --- | --- | --- | --- |
| Depolama hesabı | http://mystorageaccount.blob.core.windows.net | http://www.contoso.com |
| Blob |http://mystorageaccount.blob.core.windows.net/mycontainer/myblob | http://www.contoso.com/mycontainer/myblob |
| Kök kapsayıcı | http://mystorageaccount.blob.core.windows.net/myblob veya http://mystorageaccount.blob.core.windows.net/$root/myblob| http://www.contoso.com/myblob veya http://www.contoso.com/$root/myblob |
| Web |  http://mystorageaccount. [zone].web.core.windows.net/$web/[indexdoc] veya http://mystorageaccount. [ Zone].Web.Core.Windows.NET/[indexdoc] veya http://mystorageaccount. [ Zone].Web.Core.Windows.NET/$Web veya http://mystorageaccount. [ Zone].Web.Core.Windows.NET/ | http://www.contoso.com/$web veya http://www.contoso.com/ veya http://www.contoso.com/$web / [indexdoc] veya http://www.contoso.com/[indexdoc] |

> [!NOTE]  
> Tüm örnekler Blob Hizmeti uç noktası için web hizmeti uç noktası için de geçerlidir.

## <a name="direct-vs-intermediary-domain-mapping"></a>Doğrudan aracı bir etki alanı eşlemesi karşılaştırması

Depolama hesabınız için blob uç noktasına özel etki alanınıza işaret edecek şekilde iki yolu vardır: eşleme ve kullanarak bir CNAME doğrudan *asverify* Ara alt etki alanı.

### <a name="direct-cname-mapping"></a>Doğrudan CNAME eşlemesi

İlk ve en basit, yöntem, doğrudan blob uç noktası için özel etki alanı ve alt etki alanı eşleyen bir kurallı ad (CNAME) kaydı oluşturmaktır. CNAME kaydı, bir kaynak etki alanını hedef etki alanına eşleyen bir etki alanı adı sistemi (DNS) özelliği ' dir. Bu durumda, kaynak etki alanı kendi özel etki alanı ve alt etki alanı, örneğin olduğunda *www.contoso.com*. Blob Hizmeti uç noktanızı örneğin hedef etki alanıdır *mystorageaccount.blob.core.windows.net*.

Doğrudan yöntem ele alınmıştır [özel bir etki alanı kayıt](#register-a-custom-domain).

### <a name="intermediary-mapping-with-asverify"></a>Aracı eşlemesi ile *asverify*

İkinci yöntem de CNAME kayıtları kullanır, ancak bir özel alt etki alanı kapalı kalma süresini önlemek için Azure tarafından tanınan ilk kullanır: **asverify**.

İçinde kaydettirirken bir blob uç noktasına özel etki alanınızı eşleme işleminin kısa bir süre kapalı kalma süresi etki alanı için sonuçlanabilir [Azure portalında](https://portal.azure.com). Özel etki alanınız şu anda bir uygulamayı sıfır kapalı kalma süresi gerektiren bir hizmet düzeyi sözleşmesi (SLA) ile destekleme sonra Azure kullanabileceğiniz *asverify* alt etki alanı kayıt ara adım olarak. Bu ara adım, kullanıcıların etki alanınızın DNS eşlemesi gerçekleşirken erişebilir olmasını sağlar.

Aracı yöntemi ele alınmıştır [kullanarak bir özel etki alanı kayıt *asverify* alt etki alanı](#register-a-custom-domain-using-the-asverify-subdomain).

## <a name="register-a-custom-domain"></a>Özel bir etki alanına kaydetme
Özel etki alanınızın etki alanı kullanıcılarınıza kısaca kullanılamadığı hakkında hiçbir ilgili endişeleriniz varsa ya da özel etki alanınız şu anda bir uygulama barındırma değil kaydetmek için bu yordamı kullanın. Azure DNS, Azure Blob Depolama için özel DNS adını yapılandırmak için kullanabilirsiniz. Daha fazla bilgi için bkz. [Bir Azure hizmeti için özel etki alanı ayarları sağlamak üzere Azure DNS'yi kullanma](https://docs.microsoft.com/azure/dns/dns-custom-domain#blob-storage).

Özel etki alanınız şu anda kapalı kalma süresi olamaz uygulamanın destekleyen, bölümünde açıklanan yordamı izleyin. [kullanarak bir özel etki alanı kayıt *asverify* alt etki alanı](#register-a-custom-domain-using-the-asverify-subdomain).

Özel etki alanı yapılandırmak için DNS'de yeni bir CNAME kaydı oluşturmanız gerekir. CNAME kaydı, bir etki alanı adı için bir diğer ad belirtir. Bu durumda, depolama hesabınız için Blob Depolama uç noktasına özel etki alanınızı adresiyle eşleşir.

Genellikle, etki alanı kayıt şirketinizin Web sitesindeki etki alanının DNS ayarlarını yönetebilirsiniz. Her kayıt şirketi için bir CNAME kaydı belirtme benzer ancak biraz farklı bir yöntem olsa da, aynı kavramdır. CNAME kaydını oluşturmadan önce etki alanı kayıt paketi yükseltmeniz gerekebilir için DNS yapılandırması bazı temel etki alanı kayıt paketleri sunmaz.

1. Depolama hesabınıza gidin [Azure portalında](https://portal.azure.com).
1. Altında **BLOB hizmeti** menü dikey penceresinde seçin **özel etki alanı** açmak için *özel etki alanı* dikey penceresi.
1. Etki alanı kayıt şirketinizin Web sitesine oturum açın ve DNS Yönetme sayfasına gidin. Bunu, **Etki Alanı Adı**, **DNS** veya **Ad Sunucusu Yönetimi** gibi bir bölümde bulabilirsiniz.
1. CNAME'leri yönetme ile ilgili bölümü bulun. Bir Gelişmiş ayarlar sayfasına gidip sözcüklerini aramak zorunda kalabilirsiniz **CNAME**, **diğer**, veya **alt etki alanlarını**.
1. Yeni bir CNAME kaydı oluşturun ve bir alt etki alanı diğer adı gibi sağlayın **www** veya **fotoğraf**. Ardından, Blob Hizmeti uç noktası biçimi olan bir konak adı sağlayın **mystorageaccount.blob.core.windows.net** (burada *mystorageaccount* depolama hesabınızın adıdır). Öğenin #1 kullanılacak ana bilgisayar adı görünür *özel etki alanı* dikey penceresinde [Azure portalında](https://portal.azure.com).
1. Metin kutusuna *özel etki alanı* dikey penceresinde [Azure portalında](https://portal.azure.com), alt etki alanı da dahil olmak üzere, özel etki alanı adını girin. Örneğin, etki alanınız varsa **contoso.com** ve alt etki alanı diğer adınızı **www**, girin **www.contoso.com**. Varsa, alt etki alanı **fotoğraf**, girin **photos.contoso.com**. Alt etki alanı olan *gerekli*.
1. Seçin **Kaydet** üzerinde *özel etki alanı* özel etki alanınızı kaydetmek için dikey pencere. Kayıt başarılı olursa, depolama hesabınızı başarıyla güncelleştirildiğini portal bir bildirim görür.

Yeni CNAME kaydınız DNS üzerinden dağıtıldıktan sonra kullanıcılarınızın özel etki alanınızı kullanarak uygun izinlere sahip oldukları sürece blob verilerini görüntüleyebilir.

## <a name="register-a-custom-domain-using-the-asverify-subdomain"></a>Kullanarak bir özel etki alanı kayıt *asverify* alt etki alanı
Kendi özel kaydetmek için bu yordamı kullanın. etki alanı özel etki alanınız şu anda bir uygulama gerektiren bir SLA ile destekleniyorsa olması kapalı kalma süresi. Gelen işaret eden bir CNAME oluşturarak `asverify.<subdomain>.<customdomain>` için `asverify.<storageaccount>.blob.core.windows.net`, etki alanınızı Azure ile önceden kaydedebilirsiniz. Ardından gelen işaret eden ikinci bir CNAME oluşturun `<subdomain>.<customdomain>` için `<storageaccount>.blob.core.windows.net`, bu noktada blob uç noktanıza özel etki alanınızı trafiği yönlendirilirsiniz.

**Asverify** alt etki alanı olan Azure tarafından tanınan özel bir alt etki alanı. Eklenmesini tarafından `asverify` kendi alt etki alanı, özel etki alanınızın etki alanı için DNS kaydı değiştirmeden tanımak için Azure izin verir. Etki alanı için DNS kaydı değiştirdiğinizde, blob uç noktası kapalı kalma süresi ile eşleştirilir.

1. Depolama hesabınıza gidin [Azure portalında](https://portal.azure.com).
1. Altında **BLOB hizmeti** menü dikey penceresinde seçin **özel etki alanı** açmak için *özel etki alanı* dikey penceresi.
1. DNS sağlayıcınızın Web sitesinde oturum açın ve DNS Yönetme sayfasına gidin. Bunu, **Etki Alanı Adı**, **DNS** veya **Ad Sunucusu Yönetimi** gibi bir bölümde bulabilirsiniz.
1. CNAME'leri yönetme ile ilgili bölümü bulun. Bir Gelişmiş ayarlar sayfasına gidip sözcüklerini aramak zorunda kalabilirsiniz **CNAME**, **diğer**, veya **alt etki alanlarını**.
1. Yeni bir CNAME kaydı oluşturun ve içeren bir alt etki alanı diğer ad belirtin *asverify* alt etki alanı. Örneğin, **asverify.www** veya **asverify.photos**. Ardından, Blob Hizmeti uç noktası biçimi olan bir konak adı sağlayın **asverify.mystorageaccount.blob.core.windows.net** (burada **mystorageaccount** depolama hesabınızın adıdır). Öğenin #2 kullanılacak ana bilgisayar adı görünür *özel etki alanı* dikey penceresinde [Azure portalında](https://portal.azure.com).
1. Metin kutusuna *özel etki alanı* dikey penceresinde [Azure portalında](https://portal.azure.com), alt etki alanı da dahil olmak üzere, özel etki alanı adını girin. Dahil etmezseniz *asverify*. Örneğin, etki alanınız varsa **contoso.com** ve alt etki alanı diğer adınızı **www**, girin **www.contoso.com**. Varsa, alt etki alanı **fotoğraf**, girin **photos.contoso.com**. Alt etki alanı gereklidir.
1. Seçin **dolaylı CNAME doğrulaması kullan** onay kutusu.
1. Seçin **Kaydet** üzerinde *özel etki alanı* özel etki alanınızı kaydetmek için dikey pencere. Kayıt başarılı olursa, depolama hesabınızı başarıyla güncelleştirildiğini belirten bir portal bildirimi görürsünüz. Bu noktada, özel etki alanınızı Azure tarafından doğrulandı, ancak trafik etki alanınıza henüz depolama hesabınıza yönlendirilmemesidir.
1. DNS sağlayıcınızın Web sitesine dönün ve Blob Hizmeti uç noktanızı, alt etki alanı eşleşen başka bir CNAME kaydı oluşturun. Örneğin, alt etki alanı belirtin **www** veya **fotoğraf** (olmadan *asverify*) ve konak adı olarak **mystorageaccount.blob.core.windows.net**  (burada **mystorageaccount** depolama hesabınızın adıdır). Bu adım ile özel etki alanınızın kayıt tamamlanmıştır.
1. Son olarak, oluşturduğunuz içeren CNAME kaydını silebilirsiniz **asverify** haliyle alt etki alanı ara adım olarak yalnızca gerekli.

Yeni CNAME kaydınız DNS üzerinden dağıtıldıktan sonra kullanıcılarınızın özel etki alanınızı kullanarak uygun izinlere sahip oldukları sürece blob verilerini görüntüleyebilir.

## <a name="test-your-custom-domain"></a>Özel etki alanınızı test

Özel etki alanınızı gerçekten Blob Hizmeti uç noktanızla eşlendi onaylamak için depolama hesabınızda genel bir kapsayıcıda bir blob oluşturun. Ardından, bir web tarayıcısında bloba erişmek için şu biçimde bir URI kullanın:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Örneğin, bir web formunda erişmek için şu URI kullanabilirsiniz **myforms** kapsayıcısında **photos.contoso.com** özel alt etki alanı:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Özel bir etki alanı kaydını kaldırma

Blob Depolama uç noktanız için özel bir etki alanı kaydını silmek için aşağıdaki prosedürlerden birini kullanın.

### <a name="azure-portal"></a>Azure portal

Özel etki alanı ayarını kaldırmak için Azure Portalı'nda aşağıdakileri yapın:

1. Depolama hesabınıza gidin [Azure portalında](https://portal.azure.com).
1. Altında **BLOB hizmeti** menü dikey penceresinde seçin **özel etki alanı** açmak için *özel etki alanı* dikey penceresi.
1. Özel etki alanı adınızı içeren metin kutusunun içeriğini temizleyin.
1. **Kaydet** düğmesini seçin.

Özel etki alanı başarıyla kaldırıldı, depolama hesabınızı başarıyla güncelleştirildiğini belirten bir portal bildirimi görürsünüz.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Kullanım [az depolama hesabını güncelleştirme](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_update) CLI komutunu ve boş bir dize belirtin (`""`) için `--custom-domain` özel etki alanı kaydını kaldırmak için bağımsız değişken değeri.

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

Kullanım [Set-AzureRmStorageAccount](/powershell/module/azurerm.storage/set-azurermstorageaccount) PowerShell cmdlet'ini ve boş bir dize belirtin (`""`) için `-CustomDomainName` özel etki alanı kaydını kaldırmak için bağımsız değişken değeri.

* Komut biçimi:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "<resource-group-name>" `
      -AccountName "<storage-account-name>" `
      -CustomDomainName ""
  ```

* Komut örneği:

  ```powershell
  Set-AzureRmStorageAccount `
      -ResourceGroupName "myresourcegroup" `
      -AccountName "mystorageaccount" `
      -CustomDomainName ""
  ```

## <a name="next-steps"></a>Sonraki adımlar
* [Bir Azure Content Delivery Network (CDN) uç noktasına özel etki alanı eşleme](../../cdn/cdn-map-content-to-custom-domain.md)
* [HTTP'ler üzerinden özel etki alanlarıyla bloblara erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md)
* [Azure Blob Depolama (Önizleme) statik Web sitesi barındırma](storage-blob-static-website.md)
