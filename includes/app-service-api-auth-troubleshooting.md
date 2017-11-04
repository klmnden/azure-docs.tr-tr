Çoğu zaman kimlik doğrulama hataları, tutarsız veya hatalı yapılandırma ayarları neden. Denetlenecek öğeler için belirli bazı öneriler şunlardır.

* Kaçırılması kaydetmedi emin olun **kaydetmek** her yerden düğmesine tıklayın. Bu genellikle yapmak kolaydır ve portal sayfasında doğru değerlere aramanız, ancak bunlar gerçekten Azure ortamı ya da Azure AD uygulaması kaydedilmemiş sonucudur.
* Yapılandırılan ayarları için **uygulama ayarları** Azure portal dikey ayarları girildiğinde seçili doğru API uygulaması veya web uygulaması olduğundan emin olun.  Ayrıca ayarları olarak girildiğinden emin olun **uygulama ayarları** ve **bağlantı dizeleri**, iki bölüme biçimi benzer şekilde.
* JavaScript ön uç için kimlik doğrulaması için yeniden doğrulamak için bildirimini karşıdan `oauth2AllowImplicitFlow` başarıyla değiştirildi `true`.
* HTTPS URL'leri yapılandırılmış her yerde kullanılan doğrulayın:
  
  * Proje kodda
  * CORS içinde
  * Her bir API uygulaması ve web uygulaması için Azure ortamı uygulaması ayarları içinde
  * Azure AD uygulama ayarları.
    
    Portaldan bir API uygulamasının URL'si kopyalarsanız, genellikle olduğunu unutmayın `http://` ve el ile değiştirmek zorunda `https://`.
* Kod değişiklikleri başarıyla dağıtıldı emin olun. Örneğin, bir birden çok proje çözümü bir projenin kod değiştirmek ve değişiklik dağıtmayı planladığınız durumlarda yanlışlıkla diğer birini mümkündür.
* Tarayıcınızda HTTP URL'lerini HTTPS URL'lere gitme emin olun. Varsayılan olarak, Visual Studio oluşturur HTTP URL'lerini profilleriyle yayımlama ve proje dağıttıktan sonra ne tarayıcısında açar.
* JavaScript ön uç için kimlik doğrulaması için CORS JavaScript kodu çağırır API uygulaması üzerinde doğru şekilde yapılandırıldığından emin olun. CORS ile ilgili sorun olup olmadığı hakkında emin değilseniz, deneyin "*" izin verilen kaynağı URL'si olarak. 
* JavaScript ön uç için daha fazla hata bilgilerini almak için tarayıcınızın geliştirici araçları konsolunu sekmesini açın ve ağ üzerindeki HTTP isteklerini inceleyin. Ancak, konsol hata iletileri yanıltıcı olabilir. CORS hata belirten bir ileti alırsanız, kimlik doğrulaması gerçek sorunu olabilir. Geçici olarak geçici olarak devre dışı kimlik doğrulaması ile uygulamayı çalıştırarak bu durum geçerli olup olmadığını kontrol edebilirsiniz.
* .NET API uygulaması için elde kadar bilgi hata iletilerinde mümkün olduğunca ayarlayarak emin olun [kapalı customErrors moduna](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* .NET API uygulaması için Başlat bir [uzaktan hata ayıklama oturumunun](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)ve bir taşıyıcı belirtecini almak üzere ADAL kullanan kodu ya da talep beklenen hizmet asıl kimliği karşı denetler kod geçirilen değişkenlerin değerleri inceleyin Bu şekilde beklenmeyen durumları bulmak olası böylece kodunuzu yapılandırma değerleri birçok farklı kaynaktan, seçim yapabilirsiniz unutmayın. Örneğin, yanlış yazdınız, `ida:ClientId` olarak `ida:ClientID` Azure uygulama hizmeti ortamı ayarlarını yapılandırırken kodu alabilirsiniz `ida:ClientId` Azure uygulama hizmeti ayarı yoksayılıyor Web.config dosyasından aradığı değeri. 
* Normal bir Internet Explorer penceresinde şeyler işe yaramazsa, bir varolan oturum açma engel olabilen; InPrivate ve Chrome ya da Firefox deneyin.

