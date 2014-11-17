# Azure Kimlik

Kimlik yönetimi şirket içinde olduğu kadar genel bulut ortamında da önemlidir. Buna yardımcı olmak için Azure bazı farklı bulut kimlik teknolojilerini destekler. Kapsamı şöyledir:

- Azure Sanal Makineleri ile oluşturulmuş sanal makineleri kullanarak bulutta Windows Server Active Directory'yi (genelde yalnızca AD olarak anılır) çalıştırabilirsiniz. Şirket içi veri merkezinizi buluta genişletmek için Azure'u kullanırken bu yaklaşım akla uygundur.

- Hizmet Olarak Yazılım (SaaS) uygulamalarında çoklu oturum açma olanağını kullanıcılarınıza kazandırmak için Azure Active Directory'yi kullanabilirsiniz. Örneğin, Microsoft'un Office 365'i bu teknolojiyi kullanır ve Azure'da ya da diğer bulut ortamlarında çalışan uygulamalar da aynı teknolojiyi kullanabilir.

- Bulutta ya da şirket içinde çalışan uygulamalar, kullanıcıların Facebook, Google, Microsoft ve diğer kimlik sağlayıcılarındaki kimliklerini kullanarak oturum açmalarına izin vermek için Azure Active Directory Erişim Denetimi'ni kullanabilir.


Bu makalede üç seçenek de açıklanmaktadır.

## İçindekiler Tablosu

- [Sanal Makinelerde Windows Server Active Directory Çalıştırma](#adinvm)

- [Azure Active Directory Kullanma](#ad)

- [Azure Active Directory Erişim Denetimini Kullanma](#ac)


## <a name="adinvm"></a>Sanal Makinelerde Windows Server Active Directory Çalıştırma

Azure sanal makinelerinde Windows Server AD çalıştırmak şirket içinde çalıştırmaya çok benzer. [Şekil 1](#fig1)'de bunun nasıl göründüğünün tipik bir örneği gösterilmektedir.

![Azure Active Directory in Virtual Machine](./media/identity/identity_01_ADinVM.png)


<a name="Fig1"></a>Şekil 1: Windows Server Active Directory, Azure Sanal Ağ kullanımıyla bir kuruluşun şirket içi veri merkezine bağlı Azure sanal makinelerinde çalışabilir.

Burada gösterilen örnekte Windows Server AD, Azure Sanal Makineleri (platformun IaaS teknolojisi) kullanılarak oluşturulmuş sanal makinelerde çalışmaktadır. Bu sanal makineler ve diğer birkaçı, Azure Sanal Ağ kullanılarak şirket içi veri merkezine bağlı sanal ağ (VNET) halinde gruplandırılır. VNET, sanal özel ağ (VPN) bağlantısı aracılığıyla şirket içi ağla etkileşimde bulunan sanal makineler grubunu oluşturur. Bunun yapılması, şirket içi veri merkezinin Azure sanal makinelerini sadece diğer bir alt ağmış gibi görmesini sağlar. Şekilde gösterildiği gibi, bu sanal makinelerin ikisi Windows Server AD etki alanı denetleyicilerini çalıştırmaktadır. VNET'teki diğer iki sanal makine SharePoint gibi uygulamaları çalıştırıyor veya geliştirme ve test gibi başka bir amaçla kullanılıyor olabilir. Şirket içi veri merkezi de iki Windows Server AD etki alanı denetleyicisi çalıştırmaktadır.

Buluttaki etki alanı denetleyicilerini şirket içinde çalışanlara bağlamak için çeşitli seçenekler vardır. Kapsamı şöyledir:

- Bunların tümünü tek bir Active Directory etki alanının parçası yapın.

- Şirket içinde ve bulutta aynı ormanın parçası olan ayrı AD etki alanları oluşturun.

- Bulutta ve şirket içinde ayrı AD ormanları oluşturun ve sonra ormanlar arası güven veya Azure'daki sanal makinelerde de çalışabilen Windows Server Active Directory Federasyon Hizmetleri'ni (AD FS) kullanarak ormanları bağlayın.

Bulut bağlantısı şirket içi ağlardan yavaş olabileceğinden, yapılan tercih ne olursa olsun, yönetici şirket içi kullanıcılardan gelen kimlik doğrulama isteklerinin bulut etki alanı denetleyicilerine yalnızca gerektiğinde gitmesini sağlamalıdır. Bulut ve şirket içi etki alanı denetleyicilerini bağlarken dikkate alınması gereken diğer bir faktör çoğaltmanın oluşturduğu trafiktir. Buluttaki etki alanı denetleyicileri normalde kendi AD sitesindedir ve bu da yöneticiye çoğaltmanın yapılacağı sıklığı zamanlama olanağı sağlar. Azure içeri gönderilen baytlar için ücretlendirme yapmasa da, Azure veri merkezinden dışarı gönderilen trafiği ücretlendirir ve bu da yöneticinin çoğaltma tercihlerini etkileyebilir. Azure kendi Etki Alanı Adı Sistemi (DNS) desteğini sağlamakla birlikte, bu hizmette Active Directory için gerekli özelliklerin eksik (Dinamik DNS ve SRV kayıtları desteği gibi) olduğuna da dikkat çekilmesi önemlidir. Bu nedenle, Windows Server AD'yi Azure'da çalıştırmak için bulutta kendi DNS sunucularınızı kurmanız gerekir.

Azure sanal makinelerinde Windows Server AD çalıştırılması bazı farklı durumlarda anlamlı olabilir. İşte bazı örnekler:

- Azure Sanal Makinelerini kendi veri merkezinizin bir uzantısı olarak kullanıyorsanız, Windows Tümleşik Kimlik Doğrulaması istekleri veya LDAP sorguları gibi şeyleri işlemek için yerel etki alanı denetleyicilerine gerek duyulan bulutta uygulamaları çalıştırabilirsiniz. Örneğin SharePoint, Active Directory ile sık sık etkileşimde bulunur ve bu nedenle, bir şirket içi dizini kullanarak Azure'da SharePoint grubunu çalıştırmak mümkün olurken, bulut ortamında etki alanı denetleyicilerinin ayarlanması performansı önemli ölçüde artıracaktır. (Ancak bunun mutlaka gerekli olmadığının anlaşılması önemlidir; çok sayıda uygulama yalnızca şirket içi etki alanı denetleyicilerinin kullanımıyla bulutta başarılı bir şekilde çalışabilir.)

- Çok uzak bir şubenin kendi etki alanı denetleyicilerini çalıştıracak kaynaklara sahip olmadığını varsayalım. Bugün, şube kullanıcılarının dünyanın diğer tarafındaki etki alanı denetleyicilerine kimlik doğrulatması gerekmektedir ve oturum açılışları yavaştır. Azure üzerinde Active Directory'nin daha yakın bir Microsoft veri merkezinde çalıştırılması, şubede daha fazla sunucuya gerek duyulmaksızın bu işlemi hızlandırabilir.

- Olağanüstü durum kurtarma için Azure'u kullanan bir kuruluş, etki alanı denetleyicisi dahil küçük bir etkin sanal makine kümesini bulutta tutabilir. Daha sonra, gerektiğinde başka yerlerdeki arızalarda yükü devralması için bu siteyi genişletecek şekilde hazırlanabilir.

Başka olasılıklar da bulunmaktadır. Örneğin, buluttaki Windows Server AD'yi bir şirket içi veri merkezine bağlamanıza gerek yoktur. Örneğin, tüm kullanıcıların yalnızca bulut tabanlı kimlikler ile oturum açacağı belirli bir kullanıcı kümesine hizmet sunan bir SharePoint grubu çalıştırmak isterseniz, Azure'da tek başına bir orman oluşturabilirsiniz. Bu teknolojiyi nasıl kullanacağınız amaçlarınıza göre değişir. (Azure ile Windows Server AD'yi kullanma hakkında daha ayrıntılı rehberlik için [buraya bakın](http://msdn.microsoft.com/tr-tr/library/windowsazure/jj156090.aspx).)

## <a name="ad"></a>Azure Active Directory Kullanma

SaaS uygulamaları giderek daha da yaygınlaştıkça bariz bir sorun ortaya çıkmaktadır: Bulut tabanlı bu uygulamalar ne tür bir dizin hizmeti kullanmalıdır? Microsoft'un bu soruya yanıtı Azure Active Directory'dir.

Bulutta bu dizin hizmetini kullanmak için iki ana seçenek vardır:

- Azure Active Directory'de yalnızca SaaS uygulamalarını kullanan kişiler ve kuruluşlar tek dizin hizmeti olarak Azure Active Directory'ye güvenebilir.

- Windows Server Active Directory çalıştıran kuruluşlar şirket içi dizinlerini Azure Active Directory'ye bağlayabilir ve sonra da bunu kullanıcılarına SaaS uygulamalarında çoklu oturum açma olanağı sağlamak için kullanabilirler.


[Şekil 2](#fig2)'de bu iki seçenekten birincisi gösterilmektedir ve burada tek gerekli unsur Azure Active Directory'dir.

![Azure Active Directory in Virtual Machine](./media/identity/identity_02_AD.png)

<a name="fig2"></a>Şekil 2: Azure Active Directory bir kuruluşun kullanıcılarına Office 365 dahil SaaS uygulamalarında çoklu oturum açma olanağı sağlar.

Şekilde gösterildiği gibi Azure AD çok kiracılı bir hizmettir. Yani, birçok farklı kuruluşu aynı anda destekleyebilir ve bunların her birindeki kullanıcılarla ilgili dizin bilgilerini depolar. Bu örnekte, A kuruluşundaki bir kullanıcı bir SaaS uygulamasına erişim sağlamaya çalışmaktadır. Bu uygulama Office 365'in bir parçası olabilir (SharePoint Online gibi) veya başka bir öğe olabilir; Microsoft dışı uygulamalar da bu teknolojiyi kullanabilir. Azure AD, SAML 2.0 protokolünü desteklediğinden bir uygulamada bulunması gereken tek özellik bu endüstri standardını kullanarak etkileşimde bulunma kabiliyetidir. (Aslında, Azure AD kullanan uygulamalar yalnızca Azure veri merkezinde değil herhangi bir veri merkezinde çalışabilir.)

Kullanıcı bir SaaS uygulamasına erişim sağladığında süreç başlar (1. adım). Bu uygulamayı kullanmak için kullanıcının Azure AD tarafından verilmiş bir belirteç sunması gerekir.

Bu belirteç kullanıcıyı tanımlayan bilgiler içerir ve Azure AD tarafından dijital olarak imzalanmıştır. Kullanıcı bu belirteci almak için kullanıcı adını ve parolasını sağlamak suretiyle Azure AD'de kendi kimlik doğrulamasını yapar (2. adım). Sonra da Azure AD kullanıcının gerek duyduğu bu belirteci döndürür (3. adım).

Bu belirteç daha sonra SaaS uygulamasına gönderilir (4. adım) ve uygulama da belirtecin imzasını doğrulayıp içeriğini kullanır (5. adım). Normalde uygulama, belirtecin içerdiği kimlik bilgilerini kullanıcının hangi bilgilere erişmesine izin verildiğini belirlemek için kullanır ve belki başka biçimlerde de kullanabilir.

Uygulama kullanıcı hakkında belirteç içinde yer alandan daha fazla bilgiye gerek duyarsa, Azure AD Grafik API'sini kullanarak bu bilgileri doğrudan Azure AD'den isteyebilir (6. adım). Azure AD'nin başlangıç sürümünde dizin şeması oldukça basittir: Yalnızca kullanıcılar ve gruplar ile bunlar arasındaki ilişkileri içerir. Uygulamalar, kullanıcılar arasındaki bağlantılar hakkında bilgi edinmek için bu bilgileri kullanabilir. Örneğin bir uygulamanın, belirli bir veri öbeğine erişimine izin verilip verilmediğini belirlemek amacıyla bir kullanıcının yöneticisinin kim olduğunu bilmesi gerektiğini varsayalım. Uygulama, Grafik API üzerinden Azure AD'yi sorgulayarak bunu öğrenebilir.

Grafik API sıradan bir RESTful protokolü kullanır ve bu da mobil cihazlar dahil çoğu istemciden kullanılmasını basitleştirir. API ayrıca OData tarafından tanımlanan uzantıları destekler ve istemcilerin verilere daha yararlı yollarla erişmesini sağlamak için sorgu dili gibi öğeleri ekler. (OData hakkında daha fazlası için bkz. [OData Tanıtımı](http://download.microsoft.com/download/E/5/A/E5A59052-EE48-4D64-897B-5F7C608165B8/IntroducingOData.pdf).) Grafik API, kullanıcılar arasındaki ilişkiler hakkında bilgi edinmek için kullanılabildiğinden, uygulamaların belirli bir kuruluş için Azure AD şemasına eklenmiş sosyal grafiği anlamasına izin verir (Grafik API denmesinin nedeni budur). Ayrıca, Grafik API istekleri için Azure AD'de kendi kimlik doğrulamasını yapmak için uygulamalar OAuth 2.0'ı kullanır.

Bir kuruluş Windows Server Active Directory'yi kullanmıyorsa, şirket içinde sunucuları veya etki alanları yoksa ve yalnızca Azure AD'yi kullanan bulut uygulamalarına güveniyorsa, sadece bu bulut dizininin kullanılması firmanın kullanıcılarına tüm uygulamalarda çoklu oturum açma olanağı kazandıracaktır. Bu senaryo her geçen gün daha yaygın hale gelirken, çoğu kuruluş halen Windows Server Active Directory ile oluşturulmuş şirket içi etki alanlarını kullanmaktadır. [Şekil 3](#fig3)'te gösterildiği üzere Azure AD bu konuda da yararlı bir rol oynayabilir.

![Azure Active Directory in Virtual Machine](./media/identity/identity_03_AD.png)
<a id="fig3"></a>Şekil 3: Bir kuruluş, SaaS uygulamalarında çoklu oturum açma olanağını kullanıcılarına sağlamak için Windows Server Active Directory'yi Azure Active Directory ile federasyon haline getirebilir.

Bu senaryoda, B kuruluşundaki bir kullanıcı bir SaaS uygulamasına erişim sağlamayı istemektedir. Kullanıcının bunu yapabilmesi için, önce kuruluşun dizin yöneticilerinin AD FS kullanarak şekilde gösterildiği gibi Azure AD ile bir federasyon ilişkisi kurması gerekir. Bu yöneticiler aynı zamanda kuruluşun şirket içi Windows Server AD'si ile Azure AD arasında veri eşitlemeyi de yapılandırmalıdır. Böylece, kullanıcı ve grup bilgileri şirket içi dizinden Azure AD'ye otomatik olarak kopyalanır. Bunun neleri sağladığına dikkat edin: Gerçekte, kuruluş şirket içi dizinini buluta genişletmektedir. Windows Server AD ile Azure AD'nin bu yolla birleştirilmesi, tek bir varlık ile yönetilebilirken hem şirket içinde hem de bulutta iz bırakmaya devam eden bir dizin hizmetini kuruluşa kazandırır.

Azure AD'yi kullanmak için kullanıcı önce şirket içi Active Directory etki alanında normalde yaptığı gibi oturum açar (1. adım). Kullanıcı SaaS uygulamasına erişim sağlamaya çalıştığında (2. adım), federasyon işlemi Azure AD'nin bu uygulama için kullanıcıya bir belirteç vermesini sağlar (3. adım). (Federasyonun çalışma şekliyle ilgili daha fazla bilgi için bkz. [Windows için Beyana Dayalı Kimlik: Teknolojiler ve Senaryolar](http://www.davidchappell.com/writing/white_papers/Claims-Based_Identity_for_Windows_v3.0--Chappell.docx).) Daha önce olduğu gibi, bu belirteç kullanıcıyı tanımlayan bilgiler içerir ve Azure AD tarafından dijital olarak imzalanmıştır. Bu belirteç daha sonra SaaS uygulamasına gönderilir (4. adım) ve uygulama da belirtecin imzasını doğrulayıp içeriğini kullanır (5. adım). Ve daha önceki senaryoda olduğu gibi, SaaS uygulaması gerektiğinde kullanıcı hakkında daha fazla bilgi edinmek için Grafik API'yi kullanabilir (6. adım).

Bugün için Azure AD şirket içi Windows Server AD'nin tam bir ikamesi değildir. Daha önce belirtildiği gibi, bulut dizini çok daha basit şemaya sahiptir ve grup ilkesi (makineler hakkında bilgileri depolama kabiliyeti) ve LDAP desteği gibi özelliklerden yoksundur. (Aslında, bir Windows makinesi kullanıcıların Azure AD dışında hiçbir şey kullanmadan oturum açmasına izin verecek şekilde yapılandırılamaz; bu desteklenen bir senaryo değildir.) Bunun yerine, Azure AD'nin başlangıç hedefleri arasında ayrı bir oturum kimliği sağlamadan kurumsal kullanıcıların buluta erişimini sağlamak ve şirket içi dizin yöneticilerini, şirket içi dizinlerini kuruluşlarının kullandığı her SaaS uygulamasıyla manuel olarak eşitleme zahmetinden kurtarmak yer alır. Ancak zaman içinde bu bulut dizin hizmetinin daha geniş bir yelpazede senaryolara yanıt vermesini bekleyebilirsiniz.

## <a name="ac"></a>Azure Active Directory Erişim Denetimini Kullanma

Bulut tabanlı kimlik teknolojileri çok çeşitli sorunları çözmek için kullanılabilir. Azure Active Directory örneğin, bir kuruluşun kullanıcılarına birden fazla SaaS uygulamasında çoklu oturum açma olanağı sağlar. Ancak buluttaki kimlik teknolojileri başka yollarla da kullanılabilir.

Örneğin, bir uygulamanın kullanıcılarına birden fazla *kimlik sağlayıcısı (IdP)* tarafından verilen belirteçleri kullanarak oturum açma imkanı tanımak istediğini varsayalım. Günümüzde, Facebook, Google, Microsoft ve diğerlerini de kapsayan birçok farklı kimlik sağlayıcısı bulunmaktadır ve uygulamalar çoğu zaman kullanıcıların bu kimliklerden birini kullanarak oturum açmasına izin vermektedir. Bir uygulama zaten var olan kimliklere güvenebilecekken, kendi kullanıcı ve parola listesini tutmakla uğraşması neden gereksin ki? Var olan kimliklerin kabul edilmesi, hem hatırlamaları gerekecek kullanıcı adı ve parola sayısı bir azalan kullanıcılar, hem de uygulamayı oluşturan ve kendi kullanıcı adı ve parola listelerini tutmaya artık gerek olmayan kişiler açısından hayatı kolaylaştırır.

Ancak her kimlik sağlayıcısı belirli bir türde belirteç vermekle birlikte, bu belirteçler standart değildir ve her bir IdP'nin kendi biçimi vardır. Üstelik, bu belirteçlerdeki bilgiler de standart değildir. Örneğin Facebook, Google ve Microsoft tarafından verilen belirteçleri kabul etmek isteyen bir uygulama, bu farklı biçimlerin her birini işleyecek benzersiz kod yazma zorluğuyla karşı karşıya kalmaktadır.

Ancak bunu yapmak neden gereksin ki? Bunun yerine, kimlik bilgilerinin ortak temsilini içeren tek bir belirteç biçimi oluşturabilen bir aracı neden oluşturulmasın ki? Böylece artık tek bir tür belirteçle işlem yapmaları gerekeceğinden, bu yaklaşım uygulamaları oluşturan geliştiriciler açısından hayatı basitleştirirdi. Azure Active Directory Erişim Denetimi de tam olarak bunu yapmakta ve bulutta muhtelif belirteçlerle çalışmak üzere bir aracı sağlamaktadır. [Şekil 4](#fig4)'te bunun nasıl çalıştığı gösterilmektedir

![Azure Active Directory in Virtual Machine](./media/identity/identity_04_IdentityProviders.png) 
<a id="fig4"></a>Şekil 4: Azure Active Directory Erişim Denetimi, uygulamaların farklı kimlik sağlayıcıları tarafından verilen kimlik belirteçlerini kabul etmesini kolaylaştırır.

Bu süreç, kullanıcının bir tarayıcıdan uygulamaya erişmeyi denemesiyle başlar. Uygulama bu kullanıcıyı kendi tercih ettiği (ve uygulamanın da güvendiği) bir IdP'ye yönlendirir. Kullanıcı, kullanıcı adını ve parolasını girmek gibi bir yöntemle bu IdP'de kendi kimliğini doğrular (1. adım) ve IdP, bu kullanıcı hakkında bilgileri içeren bir belirteç döndürür (2. adım).

Şekilde gösterildiği gibi, Erişim Denetimi bir dizi farklı bulut tabanlı IdP'yi desteklemektedir ve Google, Yahoo, Facebook, Microsoft (eski adıyla Windows Live ID olarak bilinir) ve herhangi bir OpenID sağlayıcı tarafından oluşturulmuş hesaplar da buna dahildir. Ayrıca, Azure Active Directory ve (AD FS ile federasyon aracılığıyla) Windows Server Active Directory kullanılarak oluşturulmuş kimlikleri de destekler. Buradaki amaç, gerek buluttaki gerekse şirket içindeki IdP'ler tarafından verilmiş olsun, günümüzde en yaygın olarak kullanılan kimlikleri kapsama almaktır.

Kullanıcının tarayıcısı tercih ettiği IdP'den bir IdP belirtecine sahip olduğunda bu belirteci Erişim Denetimi'ne gönderir (3. adım). Erişim Denetimi, belirteci doğrulayıp gerçekten bu IdP tarafından verildiğinden emin olur ve sonra bu uygulama için tanımlanmış kurallara göre yeni bir belirteç oluşturur. Azure Active Directory gibi Erişim Denetimi de çok kiracılı bir hizmettir, ancak kiracılar müşteri kuruluşlar değil de uygulamalardır. Şekilde gösterildiği gibi her uygulama kendi ad alanını alabilir ve yetkilendirme ve daha fazlasıyla ilgili çeşitli kurallar tanımlayabilir.

Bu kurallar, her bir uygulamanın yöneticisinin, çeşitli IdP'lerden gelen belirteçlerin bir Erişim Denetimi belirtecine nasıl dönüştürülmesi gerektiğini tanımlamasına izin verir. Örneğin, farklı IdP'ler kullanıcı adlarını temsilen farklı türler kullanıyorsa, Erişim Denetimi kuralları bunların tümünü ortak bir kullanıcı adı türüne dönüştürebilir. Erişim Denetimi daha sonra bu yeni belirteci tarayıcıya geri gönderir (4. adım) ve tarayıcı da bunu uygulamaya gönderir (5. adım). Erişim Denetimi belirtecine sahip olduğunda uygulama, bu belirtecin gerçekten Erişim Denetimi tarafından verilip verilmediğini doğrular ve sonra da içerdiği bilgileri kullanır (6. adım).

Bu işlem biraz karmaşık görünmekle birlikte, aslında uygulamayı oluşturan açısından hayatı önemli ölçüde basitleştirir. Farklı bilgileri içeren muhtelif belirteçler ile işlem yapmak yerine uygulama, birden fazla kimlik sağlayıcısının verdiği kimlikleri kabul edebilirken, yine de bilindik bilgileri içeren tek bir belirteç almaya devam edebilir. Ayrıca, her bir uygulamanın çeşitli IdP'lere güvenecek şekilde yapılandırılmasını gerektirmek yerine bu güven ilişkileri Erişim Denetimi tarafından tutulur ve yalnızca uygulamanın buna güvenmesi gerekir.

Erişim Denetimi ile ilgili hiçbir konunun Windows ile ilişkili olmadığına dikkat çekmekte fayda vardır; yalnızca Google ve Facebook kimliklerini kabul eden bir Linux uygulaması tarafından da aynı şekilde kullanılabilir. Erişim Denetimi Azure Active Directory ailesinin bir parçası olsa bile, bunu önceki bölümde açıklanandan tümüyle farklı bir hizmet olarak düşünebilirsiniz. Her iki teknoloji de kimlikle çalışmakla birlikte oldukça farklı sorunları ele alırlar (Microsoft bu ikisini bir noktada tümleştirmeyi umduğunu belirtmesine karşın).

Kimlikle çalışmak neredeyse her uygulamada önemlidir. Erişim Denetimi'nin amacı geliştiricilerin çeşitli kimlik sağlayıcılarından kimlik kabul eden uygulamalar oluşturmasını kolaylaştırmaktır. Microsoft bu hizmeti buluta koyarak, herhangi bir platformda çalışan her uygulamanın bunu kullanabilmesini sağlamıştır.

##Yazar Hakkında

David Chappell, San Francisco, California'daki Chappell & Associates'in [www.davidchappell.com](http://www.davidchappell.com) müdürüdür. Konuşması, yazması ve danışmanlığı...
