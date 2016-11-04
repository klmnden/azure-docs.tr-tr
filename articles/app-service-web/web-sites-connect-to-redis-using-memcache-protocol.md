---
title: Azure App Service’te Memcache protokolü aracılığıyla bir web uygulamasını Redis Önbelleği’ne bağlama | Microsoft Docs
description: Azure App service’te Memcache protokolünü kullanarak bir web uygulamasını Redis Önbelleği’ne bağlama
services: app-service\web
documentationcenter: php
author: SyntaxC4
manager: wpickett
editor: riande

ms.service: app-service-web
ms.devlang: php
ms.topic: get-started-article
ms.tgt_pltfrm: windows
ms.workload: na
ms.date: 02/29/2016
ms.author: cfowler

---
# Azure App Service’te Memcache protokolü aracılığıyla bir web uygulamasını Redis Önbelleği’ne bağlama
Bu makalede, [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714)’te [Memcache][13] protokolünü kullanarak bir WordPress web uygulamasını [Azure Redis Önbelleği][12]’ne nasıl bağlayacağınızı öğreneceksiniz.  Bellek içi önbelleğe alma işlemi için Memcached sunucusu kullanan mevcut bir web uygulamanız varsa, bu web uygulamasını Azure App Service’e taşıyabilir ve uygulama kodlarınızda çok az değişiklikle veya hiç değişiklik yapmadan Microsoft Azure’daki birinci taraf önbelleğe alma çözümünü kullanabilirsiniz. Ayrıca, Memcache uzmanlığınızdan yararlanarak bir yandan Azure Redis Önbelleği’ni kullanarak Azure App Service’te bellek içi önbelleğe alma işlemi için son derece ölçeklenebilir, dağıtılmış uygulamalar oluşturabilir, diğer yandan NET, PHP, Node.js, Java ve Python gibi popüler uygulama çerçevelerini kullanabilirsiniz.  

App Service Web Apps, Azure Redis Önbelleği’ne gelen önbelleğe alma çağrıları için bir Memcache ara sunucusu olarak görev yapan yerel bir Memcached sunucusu olan Web Apps Memcache dolgusu ile bu uygulama senaryosuna olanak sağlar. Bu, Memcache protokolünü kullanarak iletişim kuran tüm uygulamaların Redis Önbelleği ile verileri önbelleğe almasını sağlar. Bu Memcache dolgusu protokol düzeyinde çalışır. Bu nedenle, Memcache protokolünü kullanarak iletişim kurdukları sürece tüm uygulamalar veya uygulama çerçeveleri tarafından kullanılabilir.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## Önkoşullar
Web Apps Memcache dolgusu, Memcache protokolünü kullanarak iletişim kurmaları şartıyla tüm uygulamalar ile kullanılabilir. Söz konusu örnekte, başvuru uygulaması Azure Marketi’nden sağlanabilecek bir Ölçeklenebilir WordPress sitesidir.

Aşağıdaki makalelerde açıklanan adımları izleyin:

* [Azure Redis Önbelleği Hizmeti’nin bir örneğini sağlama][0]
* [Azure’da Ölçeklenebilir WordPress sitesi dağıtma][1]

Ölçeklenebilir WordPress sitesini dağıttıktan ve Redis Önbelleği örneği sağladıktan sonra, Azure App Service Web Apps’de Memcache dolgusunu etkinleştirme işlemiyle devam edebilirsiniz.

## Web Apps Memcache dolgusunu etkinleştirme
Memcache dolgusunu yapılandırmak için üç uygulama ayarı oluşturmanız gerekir. Bu işlemi, [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), [klasik portal][3], [Azure PowerShell Cmdlet’leri][5] veya [Azure Komut Satırı Arabirimi][5] gibi birçok farklı yöntemle gerçekleştirebilirsiniz. Bu gönderinin amacı doğrultusunda, uygulama ayarlarını belirlemek üzere [Azure Portal][4]’ı kullanacağım. Aşağıdaki değerler, Redis Önbelleği örneğinizin **Ayarlar** dikey penceresinden alınabilir.

![Azure Redis Önbelleği Ayarları Dikey Penceresi](./media/web-sites-connect-to-redis-using-memcache-protocol/1-azure-redis-cache-settings.png)

### REDIS_HOST uygulama ayarı ekleme
Oluşturmanız gereken ilk uygulama ayarı **REDIS\_HOST** uygulama ayarıdır. Bu ayar, dolgunun önbellek bilgilerini ilettiği hedefi belirler. REDIS_HOST uygulama ayarı için gereken değer Redis Önbelleği örneğinizin **Özellikler** dikey penceresinden alınabilir.

![Azure Redis Önbelleği Ana Bilgisayar Adı](./media/web-sites-connect-to-redis-using-memcache-protocol/2-azure-redis-cache-hostname.png)

Uygulama ayarının anahtarını **REDIS\_HOST** olarak belirleyin ve uygulama ayarının değerini Redis Önbelleği örneğinin **ana bilgisayar adı** olarak belirleyin.

![Web Uygulaması Uygulama Ayarı REDIS_HOST](./media/web-sites-connect-to-redis-using-memcache-protocol/3-azure-website-appsettings-redis-host.png)

### REDIS_KEY uygulama ayarı ekleme
Oluşturmanız gereken ikinci uygulama ayarı **REDIS\_KEY** uygulama ayarıdır. Bu ayar, Redis Önbelleği örneğine güvenli erişim sağlamak için gereken kimlik doğrulama belirtecini sağlar. REDIS_KEY uygulama ayarı için gereken değeri Redis Önbelleği örneğinin **Erişim tuşları** dikey penceresinden alabilirsiniz.

![Azure Redis Önbelleği Birincil Anahtarı](./media/web-sites-connect-to-redis-using-memcache-protocol/4-azure-redis-cache-primarykey.png)

Uygulama ayarının anahtarını **REDIS\_KEY** olarak belirleyin ve uygulama ayarının değerini Redis Önbelleği örneğinin **Birincil Anahtar** olarak belirleyin.

![Azure Web Sitesi Uygulama Ayarı REDIS_KEY](./media/web-sites-connect-to-redis-using-memcache-protocol/5-azure-website-appsettings-redis-primarykey.png)

### MEMCACHESHIM_REDIS_ENABLE uygulama ayarı ekleme
Azure Redis Önbelleği’ne bağlanmak için ve önbellek çağrılarını iletmek için REDIS_HOST ve REDIS_KEY uygulama ayarını kullanan son uygulama ayarı, Web Apps’de Memcache Dolgusunu etkinleştirmek için kullanılır. Uygulama ayarının anahtarını **MEMCACHESHIM\_REDIS\_ENABLE** ve değeri **true** olarak ayarlayın.

![Web Uygulaması Uygulama Ayarı MEMCACHESHIM_REDIS_ENABLE](./media/web-sites-connect-to-redis-using-memcache-protocol/6-azure-website-appsettings-enable-shim.png)

Bu üç (3) uygulama ayarını ekledikten sonra **Kaydet**’e tıklayın.

## PHP için Memcache uzantısını etkinleştirme
Uygulamanın Memcache protokolü ile iletişim kurması için WordPress sitenizin dil çerçevesi olan PHP’ye Memcache uzantısını yüklemeniz gerekir.

### php_memcache Uzantısını İndirme
[PECL][6]’ye göz atın. Önbelleğe alma kategorisi altında [memcache][7] seçeneğine tıklayın. İndirilenler sütununun altında DLL bağlantısına tıklayın.

![PHP PECL Web Sitesi](./media/web-sites-connect-to-redis-using-memcache-protocol/7-php-pecl-website.png)

Web Apps’de etkin PHP sürümü için İş Parçacığı Güvenli Olmayan (NTS) x86 bağlantısını indirin. (Varsayılan PHP 5.4 sürümüdür)

![PHP PECL Web Sitesi Memcache Paketi](./media/web-sites-connect-to-redis-using-memcache-protocol/8-php-pecl-memcache-package.png)

### php_memcache uzantısını etkinleştirme
Dosyayı indirdikten sonra **php\_memcache.dll** sıkıştırmasını **d:\\home\\site\\wwwroot\\bin\\ext\\** dizininde açın. php_memcache.dll web uygulamasına yüklendikten sonra, PHP Çalışma Zamanı için uzantıyı etkinleştirmeniz gerekir. Memcache uzantısını Azure Portal’da etkinleştirmek için web uygulamasının **Uygulama Ayarları** dikey penceresini açın ve **PHP\_EXTENSIONS** anahtarı ve **bin\\ext\\php_memcache.dll** değeri ile yeni uygulama ayarını ekleyin.

> [!NOTE]
> Web uygulamasının birden fazla PHP uzantısı yüklemesi gerekirse, PHP_EXTENSIONS değeri, ilgili DLL dosyaları yollarının virgülle ayrılmış bir listesi olmalıdır.
> 
> 

![Web Uygulaması Uygulama Ayarı PHP_EXTENSIONS](./media/web-sites-connect-to-redis-using-memcache-protocol/9-azure-website-appsettings-php-extensions.png)

İşlemi tamamladıktan sonra **Kaydet**’e tıklayın.

## Memcache WordPress eklentisini yükleme
> [!NOTE]
> Ayrıca, WordPress.org adresinden [Memcached Nesne Önbelleği Eklentisi](https://wordpress.org/plugins/memcached/)’ni indirebilirsiniz.
> 
> 

WordPress eklentileri sayfasında **Yeni Ekle**’ye tıklayın.

![WordPress Eklenti Sayfası](./media/web-sites-connect-to-redis-using-memcache-protocol/10-wordpress-plugin.png)

Arama kutusuna **memcached** yazın ve **Enter**’a basın.

![WordPress Yeni Eklenti Ekleme](./media/web-sites-connect-to-redis-using-memcache-protocol/11-wordpress-add-new-plugin.png)

Listede **Memcached Nesne Önbelleği**’ni bulun ve **Şimdi Yükle**’ye tıklayın.

![WordPress Memcache Eklentisini Yükleme](./media/web-sites-connect-to-redis-using-memcache-protocol/12-wordpress-install-memcache-plugin.png)

### Memcache WordPress eklentisini etkinleştirme
> [!NOTE]
> Visual Studio Team Services’ı yüklemek için bu blog’daki [Web Apps’de Site Uzantısı’nı etkinleştirme][8] bölümünde bulunan yönergeleri izleyin.
> 
> 

`wp-config.php` dosyasında, aşağıdaki kodu dosyanın sonlarında yer alan düzenlemeyi durdur yorumu üzerine ekleyin.

```php
$memcached_servers = array(
    'default' => array('localhost:' . getenv("MEMCACHESHIM_PORT"))
);
```

Bu kod yapıştırıldıktan sonra, monaco belgeyi otomatik olarak kaydeder.

Sonraki adım, nesne önbelleği eklentisini etkinleştirmektir. Memcache Nesne Önbelleği işlevini etkinleştirmek için **wp-content/plugins/memcached** klasöründeki **object-cache.php** dosyasını **wp-content** klasörüne sürükleyip bırakabilirsiniz.

![memcache object-cache.php eklentisini bulma](./media/web-sites-connect-to-redis-using-memcache-protocol/13-locate-memcache-object-cache-plugin.png)

**object-cache.php** dosyası **wp-content** klasöründe yer aldığından, Memcached Nesne Önbelleği şimdi etkinleştirilir.

![memcache object-cache.php eklentisini etkinleştirme](./media/web-sites-connect-to-redis-using-memcache-protocol/14-enable-memcache-object-cache-plugin.png)

## Memcache Nesne Önbelleği uzantısının çalıştığını doğrulama
Web Apps Memcache dolgusunu etkinleştirmek üzere uygulanması gereken tüm adımlar tamamlandı. Yapılması gereken tek şey verilerin Redis Önbelleği örneğinizi doldurduğunu doğrulamaktır.

### Azure Redis Önbelleği’nde SSL olmayan bağlantı noktası desteğini etkinleştirme
> [!NOTE]
> Bu makale yazıldığı sırada, Redis CLI SSL bağlantısını desteklemiyordu. Bu nedenle, aşağıdaki adımlar gereklidir.
> 
> 

Azure Portal'da, bu web uygulaması için oluşturduğunuz Redis Önbelleği örneğine gidin. Önbelleğin dikey penceresi açıldıktan sonra, **Ayarlar** simgesine tıklayın.

![Azure Redis Önbelleği Ayarları Düğmesi](./media/web-sites-connect-to-redis-using-memcache-protocol/15-azure-redis-cache-settings-button.png)

Listeden **Erişim Bağlantı Noktaları**’nı seçin.

![Azure Redis Önbelleği Erişim Bağlantı Noktası](./media/web-sites-connect-to-redis-using-memcache-protocol/16-azure-redis-cache-access-port.png)

**Yalnızca SSL aracılığıyla erişime izin ver** seçeneği için **Hayır**’a tıklayın.

![Azure Redis Önbelleği Erişim Bağlantı Noktası Yalnızca SSL](./media/web-sites-connect-to-redis-using-memcache-protocol/17-azure-redis-cache-access-port-ssl-only.png)

SSL OLMAYAN bağlantı noktasının şimdi ayarlandığını görürsünüz. **Kaydet** düğmesine tıklayın.

![Azure Redis Önbelleği Redis Erişim Portalı SSL Olmayan](./media/web-sites-connect-to-redis-using-memcache-protocol/18-azure-redis-cache-access-port-non-ssl.png)

### redis-cli arabiriminde Azure Redis Önbelleği’ne bağlanma
> [!NOTE]
> Bu adım için redis’in geliştirme makinenizde yerel olarak yüklendiği varsayılır. [Bu yönergeleri kullanarak Redis’i yerel olarak yükleme][9].
> 
> 

İstediğiniz komut satırı konsolunu açın ve aşağıdaki komutu yazın:

```shell
redis-cli –h <hostname-for-redis-cache> –a <primary-key-for-redis-cache> –p 6379
```

**&lt;hostname-for-redis-cache&gt;** değerini gerçek xxxxx.redis.cache.windows.net ana bilgisayar adıyla ve **&lt;primary-key-for-redis-cache&gt;** değerini önbelleğin erişim tuşuyla değiştirin ve **Enter**’a basın. CLI ve Redis Önbelleği örneği arasında bağlantı kurulduktan sonra, herhangi bir redis komutu yürütün. Aşağıdaki ekran görüntüsünde, anahtarların listelenmesini tercih ettim.

![Terminal’de Redis CLI arabiriminden Azure Redis Önbelleği’ne bağlanma](./media/web-sites-connect-to-redis-using-memcache-protocol/19-redis-cli-terminal.png)

Anahtarların listelenmesi çağrısı bir değer döndürmelidir. Döndürmezse, web uygulamasına gidin ve yeniden deneyin.

## Sonuç
Tebrikler! WordPress uygulamasında artık işlemenin artırılmasına yardımcı olmak üzere merkezi bir bellek içi önbellek bulunur. Web Apps Memcache Dolgusu’nun programlama dili veya uygulama çerçevesinden bağımsız olarak Memcache istemcisi ile kullanılabileceğini unutmayın. Web Apps Memcache dolgusu hakkında geri bildirimlerinizi veya sorularınızı [MSDN Forumları][10]’na veya [Stackoverflow][11]’na gönderin.

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’de hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](http://go.microsoft.com/fwlink/?LinkId=523751) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

## Yapılan değişiklikler
* Web Sitelerinden App Service’e kadar değiştirme kılavuzu için bkz. [Azure App Service ve mevcut Azure Hizmetlerine etkileri](http://go.microsoft.com/fwlink/?LinkId=529714)

[0]: ../redis-cache/cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache
[1]: http://bit.ly/1t0KxBQ
[2]: http://manage.windowsazure.com
[3]: http://portal.azure.com
[4]: ../powershell-install-configure.md
[5]: /downloads
[6]: http://pecl.php.net
[7]: http://pecl.php.net/package/memcache
[8]: http://blog.syntaxc4.net/post/2015/02/05/how-to-enable-a-site-extension-in-azure-websites.aspx
[9]: http://redis.io/download#installation
[10]: https://social.msdn.microsoft.com/Forums/home?forum=windowsazurewebsitespreview
[11]: http://stackoverflow.com/questions/tagged/azure-web-sites
[12]: /services/cache/
[13]: http://memcached.org



<!--HONumber=Sep16_HO3-->


