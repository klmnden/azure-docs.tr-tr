
## <a name="configure-your-aspnet-web-app-with-the-applications-registration-information"></a>ASP.NET Web uygulamanız uygulamanın kayıt bilgileri ile yapılandırma

Bu adımda, projenizin SSL kullanacak şekilde yapılandırın ve uygulamanızın kayıt bilgilerini yapılandırmak için SSL URL'sini kullanın. Bundan sonra uygulama Ekle ' kayıt bilgileri çözümünüze *web.config*.

1.  Çözüm Gezgini'nde, proje seçip bakmak `Properties` (bir özellik penceresinde F4 tuşuna basın görmüyorsanız) penceresi
2.  Değişiklik `SSL Enabled` için`True`
3.  Değerinden kopyalama `SSL URL` yukarıda ve yapıştırın `Redirect URL` alan bu sayfanın üst kısmında ve ardından *güncelleştirme*:<br/><br/>![Proje Özellikleri](media/active-directory-develop-guidedsetup-aspnetwebapp-configure/vsprojectproperties.png)<br />
4.  Aşağıdakileri ekleyin `web.config` bölümünde kökün klasörde bulunan dosya `configuration\appSettings`:

```xml
<add key="ClientId" value="[Enter the application Id here]" />
<add key="RedirectUri" value="[Enter the Redirect URL here]" />
<add key="Tenant" value="common" />
<add key="Authority" value="https://login.microsoftonline.com/{0}/v2.0" /> 
```
