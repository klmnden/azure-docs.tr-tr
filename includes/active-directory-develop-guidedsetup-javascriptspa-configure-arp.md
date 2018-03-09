## <a name="add-the-applications-registration-information-to-your-app"></a>Uygulamanın kayıt bilgileri ekleme

Bu adımda, uygulama kayıt bilgilerinizi yeniden yönlendirme URL'sini yapılandırın ve ardından uygulama kimliği JavaScript SPA uygulamanıza eklemeniz gerekir.

### <a name="configure-redirect-url"></a>Yeniden yönlendirme URL'sini yapılandırın

Yapılandırma `Redirect URL` alan index.html sayfanızı web sunucunuzda için URL ile ve ardından *güncelleştirme*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio yönergeleri almak için yeniden yönlendirme URL'si
> Yeniden yönlendirme URL'sini elde etmek için:
> 1.    İçinde *Çözüm Gezgini*, projeyi seçin ve ardından bakmak `Properties` penceresi (Özellikler penceresi görmüyorsanız, basın `F4`)
> 2.    Değerinden kopyalama `URL` panoya:<br/> ![Proje Özellikleri](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Değeri olarak Yapıştır bir `Redirect URL` bu sayfanın üst kısmında'ye tıklayın `Update`

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Python için ayar yeniden yönlendirme URL'si
> Python için web sunucusu bağlantı noktası komut satırı aracılığıyla ayarlayabilirsiniz. Bu Destekli Kurulum, başvuru ancak kullanımında herhangi diğer bağlantı noktası kullanılabilir kullanmak boş 8080 bağlantı noktası kullanır. Herhangi bir durumda, uygulama kayıt bilgileri yeniden yönlendirme URL'sini ayarlamak için aşağıdaki yönergeleri kullanın:<br/>
> Ayarlama `http://localhost:8080/` olarak bir `Redirect URL` bu sayfa veya kullanım üst kısmında `http://localhost:[port]/` özel bir TCP bağlantı noktası kullanıyorsanız, (burada *[bağlantı noktası]* özel TCP bağlantı noktası numarası), 'Update' i tıklatın

### <a name="configure-your-javascript-spa-application"></a>JavaScript SPA uygulamanızı yapılandırın

1.  Adlı bir dosya oluşturun `msalconfig.js` uygulama kayıt bilgilerini içeren. Visual Studio kullanıyorsanız, proje (Proje kök klasöründe), sağ tıklatın ve seçin seçin: `Add`  >  `New Item`  >  `JavaScript File`. Adlandırın `msalconfig.js`
2.  Aşağıdaki kodu ekleyin, `msalconfig.js` dosyası:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
