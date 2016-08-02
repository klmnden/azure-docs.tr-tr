<properties
    pageTitle="Azure Anahtar Kasası nedir? | Microsoft Azure"
    description="Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Azure Anahtar Kasası'nı kullanarak müşteriler, anahtarları ve gizli anahtarları (kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları, .PFX dosyaları ve parolalar gibi), donanım güvenlik modülleri tarafından korunan anahtarları kullanarak şifreleyebilir."
    services="key-vault"
    documentationCenter=""
    authors="cabailey"
    manager="mbaldwin"
    tags="azure-resource-manager"/>

<tags
    ms.service="key-vault"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="05/06/2016"
    ms.author="cabailey"/>



# Azure Anahtar Kasası nedir?

Azure Anahtar Kasası çoğu bölgede kullanılabilir. Daha fazla bilgi için bkz. [Anahtar Kasası fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/key-vault/).

## Giriş

Azure Anahtar Kasası, bulut uygulamaları ve hizmetleri tarafından kullanılan şifreleme anahtarlarının ve gizli anahtarların korunmasına yardımcı olur. Anahtar Kasası'nı kullanarak anahtarları ve gizli anahtarları (kimlik doğrulaması anahtarları, depolama hesabı anahtarları, veri şifreleme anahtarları, .PFX dosyaları ve parolalar gibi), donanım güvenlik modülleri tarafından korunan anahtarları kullanarak şifreleyebilirsiniz. Ek güvenlik için HSM'lerde anahtarları içeri aktarabilir veya oluşturabilirsiniz. Bunu yapmayı seçerseniz Microsoft anahtarlarınızı FIPS 140-2 Düzey 2 doğrulamasına sahip HSM'lerde (donanım ve bellenim) işler.  

Anahtar Kasası anahtar yönetimi işlemini kolaylaştırır ve verilerinize erişen ve bunları şifreleyen anahtarları denetiminizde tutmanıza olanak sağlar. Geliştiriciler, geliştirme ve test için dakikalar içinde anahtar oluşturabilir ve ardından bunları üretim anahtarlarına sorunsuz bir şekilde geçirebilir. Güvenlik yöneticileri gerektiğinde anahtarlara izin verebilir (ve iptal edebilir).

Geliştiricilerin ve güvenlik yöneticilerinin ihtiyaçlarını karşılamaya Anahtar Kasası'nın nasıl yardımcı olabileceğini daha iyi anlamak için aşağıdaki tabloyu kullanın.





| Rol        | Sorun bildirimi           | Azure Anahtar Kasası tarafından çözüldü  |
| ------------- |-------------|-----|
| Bir Azure uygulaması geliştiricisi      | "İmzalama ve şifreleme için anahtarları kullanan Azure'a yönelik bir uygulama yazmak istiyorum ancak bunların benim uygulamamın dışında olmasını ve böylelikle çözümün coğrafi olarak dağıtılan bir uygulamaya uygun olmasını istiyorum. <br/><br/>Ayrıca kod yazmama gerek kalmadan bu anahtarların ve gizli anahtarların korunmasını ve bunları en uygun performansla uygulamam üzerinden kolayca kullanmayı istiyorum." | √ Anahtarlar bir kasada depolanır ve gerektiğinde URI tarafından çağrılır.<br/><br/> √ Anahtarlar, endüstri standardında algoritmalar, anahtar uzunlukları ve donanım güvenliği modülleri (HSM'ler) kullanılarak Azure tarafından korunur.<br/><br/> √ Anahtarlar, uygulamalarla aynı Azure veri merkezlerinde bulunan HSM'lerde işlenir; böylece anahtarların şirket içi gibi ayrı bir konumda bulunduğu durumlardan daha fazla güvenilirlik ve daha düşük gecikme süresi sağlanır.|
| Hizmet olarak Yazılım (SaaS) geliştiricisi      |"Müşterilerimin kiracı anahtarları ve gizli anahtarları için sorumluluk veya olası bir yükümlülük almak istemiyorum. <br/><br/>Müşterilerin kendi anahtarlarına sahip olmalarını ve bunları yönetmelerini istiyorum; böylece en iyi yaptığım şeye, yani çekirdek yazılım özelliklerini sağlamaya odaklanabilirim." | √ Müşteriler kendi anahtarları Azure'a aktarabilir ve bunları yönetebilir. Bir SaaS uygulamasının müşterilerin anahtarlarını kullanarak şifreleme işlemleri gerçekleştirmesi gerektiğinde, Anahtar Kasası uygulama adına bunu yapar. Uygulama, müşterilerin anahtarlarını görmez.|
| Güvenlik başkanı (CSO) | "Uygulamalarımızın güvenli anahtar yönetimi için FIPS 140-2 Düzey 2 HSM'lerle uyumlu olduğunu bilmek istiyorum. <br/><br/>Kuruluşumun anahtar yaşam döngüsü denetimine sahip olduğundan ve anahtar kullanımını izleyebildiğinden emin olmak istiyorum. <br/><br/>Ayrıca, birden çok Azure hizmeti ve kaynağı kullanıyor olsak da, anahtarları Azure'da tek bir konumdan yönetmek istiyorum."     |√ HSM'ler, FIPS 140-2 Düzey 2 doğrulanmasına sahiptir.<br/><br/>√ Anahtar Kasası, Microsoft anahtarlarınızı görmeyecek ve ayıklamayacak şekilde tasarlanmıştır.<br/><br/>√ Gerçek zamanlıya yakın anahtar kullanımı günlüğü.<br/><br/>√ Kasa, Azure'da kaç kasanız olduğuna, bunların hangi bölgeleri desteklediğine ve hangi uygulamaların bunları kullandığına bakmaksızın tek bir arabirim sağlar. |


Bir Azure aboneliği olan herhangi biri, anahtar kasalarını oluşturabilir ve kullanabilir. Anahtar Kasası geliştiricilere ve güvenlik yöneticilerine avantaj sağlasa da, bir kuruluş için diğer Azure hizmetlerini yöneten bir kuruluş yöneticisi tarafından uygulanabilir ve yönetilebilir. Örneğin, bu yönetici bir Azure aboneliğiyle oturum açabilir, anahtarların depolanacağı kuruluş için bir kasa oluşturur ve ardından şunlar gibi işlemsel görevlerden sorumlu olur:

+ Bir anahtar veya gizli anahtarı oluşturma veya içeri aktarma
+ Bir anahtar veya gizli anahtarı iptal etme veya silme
+ Kullanıcıların veya uygulamaların, anahtar ve gizli anahtarları yönetmeleri veya kullanmaları için anahtar kasasına erişmelerini yetkilendirme
+ Anahtar kullanımını (örneğin, imzalama veya şifreleme) yapılandırma
+ Anahtar kullanımını izleme

Ardından, bu yönetici uygulamalarından çağırmaları için geliştiricilere URI'ler sağlar ve güvenlik yöneticisine anahtar kullanımı günlüğü bilgilerini sunar. 

   ![Azure Anahtar Kasası'na Genel Bakış][1]

Geliştiriciler ayrıca anahtarları doğrudan API'lerini kullanarak yönetebilir. Daha fazla bilgi için bkz. [Anahtar Kasası geliştirici kılavuzu](key-vault-developers-guide.md).

## Sonraki Adımlar

Bir yöneticiye yönelik başlama öğreticisi için bkz. [Azure Anahtar Kasası ile çalışmaya başlama](key-vault-get-started.md).

Anahtar Kasası'na yönelik kullanım günlüğü hakkında daha fazla bilgi için bkz. [Azure Anahtar Kasası Günlüğü](key-vault-logging.md).

Azure Anahtar Kasası ile anahtarları ve gizli anahtarları kullanma hakkında daha fazla bilgi için bkz. [Anahtarlar ve Gizli Anahtarlar Hakkında](https://msdn.microsoft.com/library/azure/dn903623.aspx).


<!--Image references-->
[1]: ./media/key-vault-whatis/AzureKeyVault_overview.png



<!--HONumber=Jun16_HO2-->


