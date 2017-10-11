Kuruluşlar daha kullanıyorsanız [(SaaS) hizmet olarak yazılım](https://azure.microsoft.com/overview/what-is-saas/) üretkenlik uygulamaları için bulut teknolojisi ve araçları olma daha kullanıma hazır. SaaS uygulamaları sayısı arttıkça, yöneticilerin, hesaplarını yönetme ve erişim haklarını ve farklı parolalarını kullanıcılar için hatırlaması zor hale gelir. Bu uygulamalar ayrı ayrı yönetmek ek iş oluşturur ve daha az güvenlidir.

* Parolalar yazma ya da birçok hesaplarında parolalarla bunları anımsaması daha az güvenli yöntemlerini kullanmayı birçok parolaları izlemek zorunda çalışanlar eğilimi gösterir.
* Yeni bir çalışan geldiğinde veya bir bırakır, tüm hesaplarına ayrı ayrı sağlanan XML'deki sağlanan veya gerekir.
* Ayrıca, çalışanların oluşturulmak olmadan çalışmaları için SaaS uygulamalarını kullanarak başlayabilir BT, bunlar oluşturmakta kendi hesaplarını, BT yöneticilerinin onaylamadınız ve izleme olmayan sistemlerde anlamına gelir.  

Bir bu zorluklar tamamı için çoklu oturum açma (SSO) çözümüdür. Birden fazla uygulama yönetin ve kullanıcılara tutarlı bir oturum açma deneyimi ile sağlamak için kolay yoludur. Azure Active Directory (Azure AD) güçlü bir SSO çözüm sağlar ve öğreticileri için hızla yeni bir uygulama ayarlama ve kullanıcıların sağlamaya başlayın admins ile birçok kullanılabilir önceden tümleştirilmiş uygulama sahiptir.

## <a name="how-does-azure-active-directory-integrate-apps"></a>Azure Active Directory uygulamaları nasıl tümleştiriliyor?
Azure AD, uygulamaların ve sağlanan hesapları tümleştirmenize olanak sağlar. Bu da iki yaklaşım yapılabilir.

* Uygulama Galerisi uygulamada önceden tümleştirilmiş ise, uygulamaları ayarlamak ve SSO izin vermek için ayarları yapılandırmak için bu portalı aracılığıyla gidebilirsiniz. Herhangi bir galeri uygulama için başlama tarafından çoklu oturum açmayı etkinleştirmek için uygulama galerisinde ve Azure portalında sunulan basit adım adım yönergeleri izleyin.
* Uygulama galerisinde değilse, özel bir uygulama olarak Azure AD hala çoğu uygulamalar ayarlayabilirsiniz. Bu yapılandırma için biraz daha teknik uzmanlığı gerektirir. SAML 2.0 federe bir uygulama olarak destekleyen herhangi bir uygulama veya bir HTML tabanlı oturum açma sayfası bir parola SSO uygulama olarak olduğu herhangi bir uygulama ekleyebilirsiniz.

Kullanıcıların kendi hesapları tarafından yönetilmeyen SaaS uygulamaları için oluşturmuş olduğu durumda BT [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) aracı, bir çözüm sağlar. Bu araç, hangi uygulamaların kuruluş ve bunların her birini kullanan kişilerin sayısı boyunca kullanılan belirlemek için web trafiği izler. BT kullanıcıların tercih ve SSO için Azure AD ile tümleştirmek karar hangi uygulamaların öğrenmek için bu bilgileri kullanabilirsiniz.  

Bir uygulamayı Azure AD ile tümleştirdiğinizde, kullanıcıların kurulan uygulama kimlikleri kendi ilgili eşleyebilirsiniz Azure AD kimlik.  

