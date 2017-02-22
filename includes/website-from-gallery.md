Azure Marketi, Microsoft, üçüncü taraf şirketler ve açık kaynak yazılım girişimleri tarafından geliştirilen çok çeşitli popüler web uygulamalarını kullanıma sunar. Azure Marketinden oluşturulan web uygulamaları, [Azure Preview Portal](http://go.microsoft.com/fwlink/?LinkId=529715)’a bağlanmak için kullanılan tarayıcı dışında bir yazılımın yüklenmesini gerektirmez. 

Bu öğreticide şunları öğreneceksiniz:

* Azure Market üzerinden yeni bir web uygulaması oluşturma.
* Azure Preview Portal üzerinden web uygulamasını dağıtma.

Varsayılan bir şablon kullanan WordPress blogu oluşturacaksınız. Aşağıdaki şekilde, tamamlanmış uygulama gösterilmiştir:

![Wordpress blogu][13]

> [!NOTE]
> Azure hesabı için kaydolmadan önce Azure App Service’i kullanmaya başlamak isterseniz, App Service’te hemen kısa süreli bir başlangıç web uygulaması oluşturabileceğiniz [App Service’i Deneyin](https://azure.microsoft.com/try/app-service/) sayfasına gidin. Kredi kartı ve taahhüt gerekmez.
> 
> 

## <a name="create-a-web-app-in-the-portal"></a>Portalda web uygulaması oluşturma
1. Azure Preview Portal’da oturum açın.
2. Azure Market’i açmak için **Market** simgesine:
   
    ![Market simgesi][marketplace]
   
    Veya panonun sağ üst köşesindeki **Yeni** simgesine tıklayın ve listenin altındaki **Market** öğesini seçin.
   
    ![Yeni Oluştur][5]
3. **Web + Mobil**’i seçin. **WordPress**’i arayın ve ardından **WordPress** simgesine tıklayın.
   
    ![Listede WordPress][7]
4. WordPress uygulamasının açıklamasını okuduktan sonra **Oluştur**’u seçin.
5. **Web uygulaması**’na tıklayın ve web uygulamanızı yapılandırmak için gerekli değerleri belirtin.
   
    ![uygulamanızı yapılandırma][8]
6. **Veritabanı**’na tıklayın ve MySQL veritabanınızı yapılandırmak için gerekli değerleri belirtin. 
   
    ![veritabanı yapılandırma][database]
7. Yeni kaynak grubu için bir ad belirtin.
   
    ![Kaynak grubu ayarlama][groupname]
8. Gerekirse, **ABONELİK** öğesine tıklayın ve kullanılacak aboneliği belirtin. 
9. Web uygulamasını tanımlamayı bitirdiğinizde **Oluştur**’a tıklayın ve yeni web uygulamasının oluşturulmasını bekleyin.
   
   Uygulama oluşturulduğunda, web uygulamasını ve veritabanını içeren kaynak grubunu görürsünüz.
   
   ![grup görüntüleme][resourcegroup]

## <a name="launch-and-manage-your-wordpress-web-app"></a>WordPress web uygulamanızı başlatma ve yönetme
1. Uygulamanızın ayrıntılarını görmek için yeni web uygulamanıza tıklayın.
   
    ![panoyu başlatma][10]
2. **Temel Parçalar** sayfasında **Gözat**’a veya **Url** altındaki bağlantıya tıklayarak web uygulamasının karşılama sayfasını açın.
   
    ![site URL][browse]
3. WordPress’i yüklemediyseniz WordPress için gereken uygun yapılandırma bilgilerini girin ve **WordPress Yükle**’ye tıklayarak yapılandırmayı tamamlayıp web uygulamasının oturum açma sayfasını açın.
4. **Oturum Aç**’a tıklayıp kimlik bilgilerinizi girin.  
5. Aşağıdaki web uygulamasına benzeyen yeni bir WordPress web uygulamasına sahip olursunuz.    
   
    ![WordPress siteniz][13]

[5]: ./media/website-from-gallery/start-marketplace.png
[6]: ./media/website-from-gallery/wordpressgallery-02.png
[7]: ./media/website-from-gallery/search-web-app.png
[8]: ./media/website-from-gallery/set-web-app.png
[9]: ./media/website-from-gallery/wordpressgallery-05.png
[10]: ./media/website-from-gallery/select-web.png
[13]: ./media/website-from-gallery/wordpressgallery-09.png
[webapps]: ./media/website-from-gallery/selectwebapps.png
[database]: ./media/website-from-gallery/set-db.png
[resourcegroup]: ./media/website-from-gallery/show-rg.png
[browse]: ./media/website-from-gallery/browse-web.png
[marketplace]: ./media/website-from-gallery/marketplace-icon.png
[groupname]: ./media/website-from-gallery/set-rg.png


<!--HONumber=Jan17_HO3-->


