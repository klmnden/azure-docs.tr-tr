---
title: "Azure Blob storage uç noktanız için özel etki alanı adı yapılandırma | Microsoft Docs"
description: "Bir Azure depolama hesabındaki Blob storage uç kendi kurallı ad (CNAME) eşleştirmek için Azure portalını kullanın."
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: aaafd8c5-eacb-49dc-8c8b-3f7011ad5e92
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: tamram
ms.openlocfilehash: cbc8654bf1755826afa2cf83e5476e88903e0854
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-custom-domain-name-for-your-blob-storage-endpoint"></a>Blob depolama uç noktası için özel etki alanı adınızı yapılandırma

Azure depolama hesabınızdaki blob verilerine erişmek için özel bir etki alanı yapılandırabilirsiniz. Blob storage için varsayılan uç noktası `<storage-account-name>.blob.core.windows.net`. Özel etki alanı ve alt etki alanı gibi eşlerseniz **www.contoso.com** için depolama alanınızı blob uç nokta, kullanıcıların can sonra hesap erişim o etki alanını kullanan depolama hesabınızda blob verileri.

> [!IMPORTANT]
> Azure depolama henüz yerel olarak HTTPS ile özel etki alanlarını desteklemiyor. Şu anda yapabilecekleriniz [BLOB'lar özel etki alanları ile HTTPS üzerinden erişmek için Azure CDN kullanan](storage-https-custom-domain-cdn.md).
>

Adlı bir depolama hesabında bulunan blob veriler için birkaç örnek URL'leri aşağıdaki tabloda gösterilmektedir **mystorageaccount**. Özel etki alanı kayıtlı depolama hesabı için **www.contoso.com**:

| Kaynak Türü | Varsayılan URL | Özel etki alanı URL'si |
| --- | --- | --- |
| Depolama hesabı | http://mystorageaccount.BLOB.Core.Windows.NET | http://www.contoso.com |
| Blob |http://mystorageaccount.BLOB.Core.Windows.NET/MyContainer/myblob | http://www.contoso.com/MyContainer/myblob |
| Kök kapsayıcı | http://mystorageaccount.BLOB.Core.Windows.NET/myblob veya http://mystorageaccount.blob.core.windows.net/$ kök/myblob| http://www.contoso.com/myblob ya da http://www.contoso.com/$ kök/myblob |

## <a name="direct-vs-intermediary-domain-mapping"></a>Doğrudan ara etki alanı eşleme karşılaştırması

Özel etki alanınızı depolama hesabınız için blob uç noktası için iki yolu vardır: eşleme ve kullanarak CNAME doğrudan *asverify* Ara alt etki alanı.

### <a name="direct-cname-mapping"></a>Doğrudan CNAME eşlemesi

İlk ve en basit, doğrudan blob uç noktası için özel etki alanı ve alt etki alanı eşleyen bir kurallı ad (CNAME) kaydı oluşturmak için yöntemidir. Bir CNAME kaydı hedef etki alanı için bir kaynak etki alanı eşleyen bir etki alanı adı sistemi (DNS) özelliğidir. Bu durumda, kaynak etki alanı kendi özel etki alanı ve alt etki alanı, örneğin olduğunda *www.contoso.com*. Blob Hizmeti uç noktanızı, örneğin hedef etki alanıdır *mystorageaccount.blob.core.windows.net*.

Doğrudan yöntemi içinde ele [özel bir etki alanı kayıt](#register-a-custom-domain).

### <a name="intermediary-mapping-with-asverify"></a>Ara eşlemesi ile *asverify*

İkinci yöntem de CNAME kayıtları kullanır, ancak ilk kesinti süresini önlemek için Azure tarafından tanınan özel bir alt etki alanı kullanır: **asverify**.

İçinde kaydettirirken özel etki alanınızı blob uç noktasına eşleme işlemi kapalı kalma süresi etki alanı için kısa bir süre sonuçlanabilir [Azure portal](https://portal.azure.com). Özel etki alanınızı şu anda bir uygulama sıfır kapalı kalma süresi gerektiren bir hizmet düzeyi sözleşmesi (SLA) ile destekleme sonra Azure kullanabilirsiniz *asverify* alt etki alanı Ara kayıt adımı olarak. Bu ara adım kullanıcıların etki alanınızın DNS eşlemesi gerçekleştirilirken erişebilir sağlar.

Ara yöntemi içinde ele [kullanarak bir özel etki alanı kayıt *asverify* alt etki alanı](#register-a-custom-domain-using-the-asverify-subdomain).

## <a name="register-a-custom-domain"></a>Özel bir etki alanı kaydetme
Özel etki alanınızı kullanıcılarınıza kısaca kullanılabilir durumda etki alanı hakkında hiçbir kaygılarınız varsa veya özel etki alanınızı uygulama şu anda barındırmayan kaydetmek için bu yordamı kullanın.

Özel etki alanınızı şu anda kapalı kalma süresi olamaz uygulamanın destekleyen, özetlenen yordamı izleyin. [kullanarak bir özel etki alanı kayıt *asverify* alt etki alanı](#register-a-custom-domain-using-the-asverify-subdomain).

Özel etki alanı yapılandırmak için DNS'de yeni bir CNAME kaydı oluşturmanız gerekir. CNAME kaydı bir etki alanı adı için bir diğer ad belirtir. Bu durumda, depolama hesabınız için Blob storage uç için özel etki alanınızı adresini eşler.

Genellikle, etki alanı kayıt şirketinizin sitesinde etki alanınızın DNS ayarlarını yönetebilirsiniz. Her kayıt için bir CNAME kaydı belirtmenin benzer ancak biraz farklı bir yöntem olsa da, aynı kavramdır. CNAME kaydı oluşturmadan önce etki alanı kayıt paketi yükseltmeniz gerekebilir şekilde bazı temel etki alanı kayıt paketler DNS yapılandırmasını sağlamaz.

1. Depolama hesabınıza gidin [Azure portal](https://portal.azure.com).
1. Altında **BLOB hizmeti** menü dikey penceresinde, seçin **özel etki alanı** açmak için *özel etki alanı* dikey.
1. Etki alanı kayıt şirketinizin Web sitesine oturum açın ve DNS Yönetme sayfasına gidin. Bunu, **Etki Alanı Adı**, **DNS** veya **Ad Sunucusu Yönetimi** gibi bir bölümde bulabilirsiniz.
1. CNAME'leri yönetme ile ilgili bölümü bulun. Bir Gelişmiş Ayarları sayfasına gidin ve sözcüklerini arayın gerekebilir **CNAME**, **diğer**, veya **alt etki alanları**.
1. Yeni bir CNAME kaydı oluşturun ve bir alt etki alanı diğer adı gibi sağlamak **www** veya **fotoğraf**. Ardından, Blob Hizmeti uç noktası biçimi olan bir ana bilgisayar adı sağlayın **mystorageaccount.blob.core.windows.net** (burada *mystorageaccount* depolama hesabınızın adıdır). Öğesinin #1 kullanmak için ana bilgisayar adı görünür *özel etki alanı* dikey penceresinde [Azure portal](https://portal.azure.com).
1. Metin kutusuna *özel etki alanı* dikey penceresinde [Azure portal](https://portal.azure.com), alt etki alanı dahil olmak üzere, özel etki alanı adını girin. Örneğin, etki alanınız varsa **contoso.com** ve alt etki alanı diğer adı **www**, girin **www.contoso.com**. Alt etki alanı ise **fotoğraf**, girin **photos.contoso.com**. Alt etki alanı olan *gerekli*.
1. Seçin **kaydetmek** üzerinde *özel etki alanı* özel etki alanınızı kaydetmek için dikey. Kayıt başarılı olursa, depolama hesabı başarıyla güncelleştirildi portal bir bildirim görürsünüz.

Yeni CNAME kaydı DNS dağıtıldıktan sonra kullanıcılarınızın özel etki alanınızı kullanarak uygun izinlere sahip oldukları sürece blob verilerini görüntüleyebilir.

## <a name="register-a-custom-domain-using-the-asverify-subdomain"></a>Kullanarak bir özel etki alanı kayıt *asverify* alt etki alanı
Özel kaydetmek için bu yordamı kullanın etki alanı özel etki alanınızı şu anda bir uygulama gerektiren bir SLA ile destekleniyorsa olması kapalı kalma süresi. Gelen işaret eden bir CNAME oluşturarak `asverify.<subdomain>.<customdomain>` için `asverify.<storageaccount>.blob.core.windows.net`, etki alanınızı Azure ile önceden kaydedebilirsiniz. Ardından gelen işaret ikinci bir CNAME oluşturun `<subdomain>.<customdomain>` için `<storageaccount>.blob.core.windows.net`, bu noktada trafik özel etki alanınız için blob uç noktasına yönlendirilirsiniz.

**Asverify** alt etki alanı olan Azure tarafından tanınan özel bir alt etki alanı. Eklenmesini tarafından `asverify` kendi alt etki alanı, etki alanı için DNS kaydı değiştirmeden özel etki alanınızı tanımak için Azure izin verir. Etki alanı için DNS kaydı değiştirdiğinizde, blob uç noktası kapalı kalma süresi olmadan eşleşecektir.

1. Depolama hesabınıza gidin [Azure portal](https://portal.azure.com).
1. Altında **BLOB hizmeti** menü dikey penceresinde, seçin **özel etki alanı** açmak için *özel etki alanı* dikey.
1. DNS sağlayıcınızın Web sitesine oturum açın ve DNS Yönetme sayfasına gidin. Bunu, **Etki Alanı Adı**, **DNS** veya **Ad Sunucusu Yönetimi** gibi bir bölümde bulabilirsiniz.
1. CNAME'leri yönetme ile ilgili bölümü bulun. Bir Gelişmiş Ayarları sayfasına gidin ve sözcüklerini arayın gerekebilir **CNAME**, **diğer**, veya **alt etki alanları**.
1. Yeni bir CNAME kaydı oluşturun ve içeren bir alt etki alanı diğer adı sağlayın *asverify* alt etki alanı. Örneğin, **asverify.www** veya **asverify.photos**. Ardından, Blob Hizmeti uç noktası biçimi olan bir ana bilgisayar adı sağlayın **asverify.mystorageaccount.blob.core.windows.net** (burada **mystorageaccount** depolama hesabınızın adıdır). Öğesinin #2 kullanmak için ana bilgisayar adı görünür *özel etki alanı* dikey penceresinde [Azure portal](https://portal.azure.com).
1. Metin kutusuna *özel etki alanı* dikey penceresinde [Azure portal](https://portal.azure.com), alt etki alanı dahil olmak üzere, özel etki alanı adını girin. İçermeyen *asverify*. Örneğin, etki alanınız varsa **contoso.com** ve alt etki alanı diğer adı **www**, girin **www.contoso.com**. Alt etki alanı ise **fotoğraf**, girin **photos.contoso.com**. Alt etki alanı gereklidir.
1. Seçin **dolaylı CNAME doğrulaması kullan** onay kutusu.
1. Seçin **kaydetmek** üzerinde *özel etki alanı* özel etki alanınızı kaydetmek için dikey. Kayıt başarılı olursa, depolama hesabı başarıyla güncelleştirildi belirten bir portal bildirim görürsünüz. Bu noktada, özel etki alanınızı Azure tarafından doğrulandı, ancak etki alanınız için trafiği henüz depolama hesabınıza yönlendirilmez.
1. DNS sağlayıcınızın Web sitesine dönüp, alt etki alanı için Blob Hizmeti uç noktanızı eşlemeleri başka bir CNAME kaydı oluşturun. Örneğin, alt etki alanı belirtin **www** veya **fotoğraf** (olmadan *asverify*) ve ana bilgisayar adı olarak **mystorageaccount.blob.core.windows.net** (burada **mystorageaccount** depolama hesabınızın adıdır). Bu adımda, özel etki alanınızı kayıt tamamlanır.
1. Son olarak, oluşturduğunuz içeren CNAME kaydı silebilirsiniz **asverify** şekliyle alt etki alanı bir ara adım olarak yalnızca gerekli.

Yeni CNAME kaydı DNS dağıtıldıktan sonra kullanıcılarınızın özel etki alanınızı kullanarak uygun izinlere sahip oldukları sürece blob verilerini görüntüleyebilir.

## <a name="test-your-custom-domain"></a>Özel etki alanınızı test

Özel etki alanınız için Blob Hizmeti uç noktanızı gerçekten eşlendi onaylamak için açık bir kapsayıcıdaki depolama hesabınızda blob oluşturun. Ardından, bir web tarayıcısında blob erişmek için aşağıdaki biçimde bir URI kullanın:

`http://<subdomain.customdomain>/<mycontainer>/<myblob>`

Örneğin, bir web formunda erişim sağlamak için aşağıdaki URI kullanabileceğiniz **myforms** kapsayıcısında **photos.contoso.com** özel alt etki alanı:

`http://photos.contoso.com/myforms/applicationform.htm`

## <a name="deregister-a-custom-domain"></a>Özel bir etki alanı kaydını

Blob storage uç noktanız için özel bir etki alanı kaydını silmek için aşağıdaki yordamlardan birini kullanın.

### <a name="azure-portal"></a>Azure portalına

Özel etki alanı ayarını kaldırmak için Azure Portalı'nda aşağıdakileri yapın:

1. Depolama hesabınıza gidin [Azure portal](https://portal.azure.com).
1. Altında **BLOB hizmeti** menü dikey penceresinde, seçin **özel etki alanı** açmak için *özel etki alanı* dikey.
1. İçeriği özel etki alanı adınızı içeren metin kutusunun işaretini kaldırın.
1. **Kaydet** düğmesini seçin.

Özel etki alanı başarıyla kaldırıldı, depolama hesabı başarıyla güncelleştirildi belirten bir portal bildirim görürsünüz.

### <a name="azure-cli-20"></a>Azure CLI 2.0

Kullanım [az depolama hesabı güncelleştirme](https://docs.microsoft.com/cli/azure/storage/account#az_storage_account_update) CLI komut ve boş bir dize belirtin (`""`) için `--custom-domain` özel etki alanı kaydını kaldırmak için bağımsız değişken değeri.

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
* [Özel bir etki alanı Azure içerik teslim ağı (CDN) uç noktasına eşleme](../../cdn/cdn-map-content-to-custom-domain.md)
* [BLOB'lar özel etki alanları ile HTTPS üzerinden erişmek için Azure CDN'yi kullanma](storage-https-custom-domain-cdn.md)
