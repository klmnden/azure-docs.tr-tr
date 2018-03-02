Etki alanı adınızı kayıtlarını dağıtıldıktan sonra özel etki alanı adınızı web uygulamanızı Azure App Service'te erişmek için kullanılabilir doğrulamak için tarayıcınızı kullanmanız mümkün olması gerekir.

> [!NOTE]
> CNAME DNS sistemi üzerinden yayılması biraz zaman alabilir. Bir hizmeti gibi kullanabilir <a href="http://www.digwebinterface.com/">http://www.digwebinterface.com/</a> CNAME kullanılabilir olduğunu doğrulayın.
> 
> 

Trafik Yöneticisi uç noktası olarak web uygulamanızı eklemediyseniz, ad çözümlemesi özel etki alanı adı yollar için trafik Yöneticisi olarak çalışmadan önce bunu yapmanız gerekir. Trafik Yöneticisi daha sonra web uygulamanızın yönlendirir. İçinde bulunan bilgileri kullanın [ekleme veya silme uç noktaların](../articles/traffic-manager/traffic-manager-endpoints.md) web uygulamanızı bir uç nokta Traffic Manager profilinize olarak eklemek için.

> [!NOTE]
> Web uygulamanızı bir uç nokta eklerken listelenmemişse için yapılandırıldığından emin olun **standart** uygulama hizmeti planı modu. Kullanmalısınız **standart** Traffic Manager ile çalışmak için web uygulamanız için modu.
> 
> 

1. Tarayıcınızda açın [Azure Portal](https://portal.azure.com).
2. İçinde **Web Apps** sekmesini seçin, web uygulamanızın adını tıklatın, **ayarları**ve ardından **özel etki alanları**
   
    ![](./media/custom-dns-web-site/dncmntask-cname-6.png)
3. İçinde **özel etki alanları** dikey penceresinde tıklatın **ana bilgisayar adını eklemek**.
4. Kullanım **ana bilgisayar adı** metin kutuları bu web uygulaması ile ilişkilendirmek için trafik yöneticisi etki alanı adı girin.
   
    ![](./media/custom-dns-web-site/dncmntask-cname-8.png)
5. Tıklatın **doğrulama** etki alanı adı yapılandırmasını kaydetmek için.
6. Tıklatarak bağlı **doğrulama** Azure etki alanı doğrulama iş akışı kazandırın. Bu etki alanı sahipliğini yanı sıra ana bilgisayar adı kullanılabilirliğini ve rapor başarı veya hata düzeltme konusunda tavsiyeler ayrıntılı hata kontrol eder.    
7. Doğrulama başarılı bağlı **ana bilgisayar adını eklemek** düğmesi etkin olacak ve, Ata ana bilgisayar adı için kullanamazsınız. Özel etki alanı adınızı bir tarayıcıda şimdi gidin. Özel etki alanı adınızı kullanarak, uygulama çalışan görmelisiniz. 
   
   Yapılandırma tamamlandıktan sonra özel etki alanı adı listelenir **etki alanı adları** , web uygulamanızın bölümü.

Bu noktada, tarayıcınızda trafik yöneticisi etki alanı adı girin ve başarılı bir şekilde, web uygulamanıza sürdüğünü olduğunu görmek mümkün olması gerekir.

