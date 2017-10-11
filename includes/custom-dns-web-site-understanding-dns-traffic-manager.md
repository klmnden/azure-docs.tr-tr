Etki alanı adı sistemi (DNS), internet'te öğeleri bulmak için kullanılır. Tarayıcınızda bir adres girin veya bir web sayfası bir bağlantıya tıklayın, örneğin, bu DNS etki alanı bir IP adresine çevirmek için kullanır. IP adresi tür sokak adresi gibi ancak çok İnsan kolay değil. Örneğin, bir DNS adı gibi unutmayın çok daha kolay **contoso.com** 192.168.1.88 veya 2001:0:4137:1f67:24a2:3888:9cce:fea3 gibi bir IP adresi anımsaması küçüktür.

DNS sistem dayanır *kayıtları*. Kayıtları ilişkilendirmek belirli bir *adı*, gibi **contoso.com**, bir IP adresi ya da başka bir DNS adına sahip. Bir uygulama bir web tarayıcısı gibi bir ad DNS arar, kaydı bulur ve her adresi olarak işaret kullanır. İşaret değeri bir IP adresi varsa, tarayıcı bu değeri kullanır. Başka bir DNS adına işaret ediyorsa, uygulama çözümleme yeniden yapmanız gerekir. Sonuç olarak, tüm ad çözümlemesi bir IP adresi sona erer.

Bir Azure Web sitesi oluşturduğunuzda, bir DNS adı otomatik olarak siteye atanmış. Bu ad biçimini alır  **&lt;yoursitename&gt;. azurewebsites.net**. Web sitenizi bir Azure trafik Yöneticisi uç noktası olarak eklediğinizde, Web sitenizi üzerinden erişilebilir ise  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** etki alanı.

> [!NOTE]
> Web sitenizi bir trafik Yöneticisi uç noktası olarak yapılandırıldığında, kullanacağınız **. trafficmanager.net** adresi DNS kayıtlarını oluştururken.
> 
> Trafik Yöneticisi ile CNAME kayıtlarına yalnızca kullanabilirsiniz
> 
> 

Ayrıca kayıtları, her biri kendi işlevleri ve sınırlamaları, birden çok tür vardır, ancak için trafik Yöneticisi uç noktalar olarak yapılandırılmış Web sitelerinde, biz yalnızca yaklaşık bir dikkat edin; *CNAME* kaydeder.

### <a name="cname-or-alias-record"></a>CNAME veya diğer ad kaydı
Bir CNAME kaydı eşleyen bir *belirli* gibi DNS adı **mail.contoso.com** veya **www.contoso.com**, başka bir (kurallı) etki alanı adı. Azure trafik Yöneticisi'ni kullanarak Web söz konusu olduğunda kurallı etki alanı adıdır  **&lt;Uygulamam >. trafficmanager.net** Traffic Manager profilinizin etki alanı adı. Oluşturduktan sonra CNAME için diğer ad oluşturur  **&lt;Uygulamam >. trafficmanager.net** etki alanı adı. CNAME girişi IP adresine çözümleyecek,  **&lt;Uygulamam >. trafficmanager.net** etki alanı adı otomatik olarak, böylece Web sitesinin IP adresi değişirse, herhangi bir eylemde bulunmanız gerekmez.

Trafik Yöneticisi trafiği geldikten sonra ardından trafiği yük dengeleyici yöntemi için yapılandırılmış Web sitenizi, yönlendirir. Bu Web sitenizin ziyaretçileri için tamamen saydamdır. Özel etki alanı adı tarayıcısında yalnızca görürler.

> [!NOTE]
> Bazı etki alanı kayıt yalnızca bir CNAME kaydı gibi kullanırken alt etki alanları eşlemeye izin **www.contoso.com**ve değil kök adları gibi **contoso.com**. CNAME kayıtları hakkında daha fazla bilgi için şirketiniz tarafından sağlanan belgelere bakın <a href="http://en.wikipedia.org/wiki/CNAME_record">CNAME kaydı Wikipedia girişinde</a>, veya <a href="http://tools.ietf.org/html/rfc1035">IETF etki alanı adları - uygulama ve belirtim</a> belge.
> 
> 

