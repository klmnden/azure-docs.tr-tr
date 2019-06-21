---
title: Microsoft Azure üzerinde güvenli uygulamalar geliştirin
description: Bu makalede, web uygulaması projenize uygulama ve doğrulama aşamaları sırasında dikkate alınması gereken en iyi uygulamalar açıklanmaktadır.
author: TerryLanfear
manager: barbkess
ms.author: terrylan
ms.date: 06/12/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: bcd66d1a8077b4cc87c184f34b43cc5846a83f2f
ms.sourcegitcommit: 22c97298aa0e8bd848ff949f2886c8ad538c1473
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/14/2019
ms.locfileid: "67144431"
---
# <a name="develop-secure-applications-on-azure"></a>Azure'da güvenli uygulamalar geliştirin
Bu makalede güvenlik etkinliklerini ve bulut için uygulamalar geliştirirken dikkate alınması gereken denetimler sunar. Güvenlik sorularını ve Microsoft uygulama ve doğrulama aşamaları sırasında dikkate alınması gereken kavramlar [Security Development Lifecycle (SDL)](https://msdn.microsoft.com/library/windows/desktop/84aed186-1d75-4366-8e61-8d258746bopq.aspx) ele alınmaktadır. Etkinlikleri ve daha güvenli bir uygulama geliştirmek için kullanabileceğiniz Azure Hizmetleri tanımlamanıza yardımcı olmaktır.

Aşağıdaki SDL aşamaları, bu makalede ele alınmaktadır:

- Uygulama
- Doğrulama

## <a name="implementation"></a>Uygulama
Odak noktası, uygulama aşamasından erken önleme için en iyi uygulamaları oluşturmak ve algılama ve güvenlik sorunlarını koddan kaldırma sağlamaktır.
Uygulamanız, kullanılacak düşünmediğiniz yollarla kullanılan varsayılır. Bu, uygulamanızın yanlışlıkla veya kasıtlı kötüye karşı korumanıza yardımcı olur.

### <a name="perform-code-reviews"></a>Kod incelemeleri gerçekleştirin

Kodu iade etmeden önce gerçekleştir [kod incelemesi](https://docs.microsoft.com/azure/devops/learn/devops-at-microsoft/code-reviews-not-primarily-finding-bugs) genel kod kalitesini artırmak ve hata oluşturma riskini azaltmak için. Kullanabileceğiniz [Visual Studio](https://docs.microsoft.com/azure/devops/repos/tfvc/get-code-reviewed-vs?view=vsts) kod inceleme işlemini yönetmek için.

### <a name="perform-static-code-analysis"></a>Statik kod analizini gerçekleştirin

[Statik kod analizi](https://www.owasp.org/index.php/Static_Code_Analysis) (diğer adıyla *kaynak kod analizi*) genellikle bir kod incelemesi bir parçası olarak gerçekleştirilir. Statik kod analizi yaygın olarak başvuran statik kod çözümleme araçları gibi teknikler kullanılarak olası güvenlik açıklarını çalışan olmayan kodu bulmak için çalışan [denetimi taint](https://en.wikipedia.org/wiki/Taint_checking) ve [veri akışını analiz](https://en.wikipedia.org/wiki/Data-flow_analysis).

Azure Marketi tekliflerini [Geliştirici Araçları](https://azuremarketplace.microsoft.com/marketplace/apps/category/developer-tools?page=1&search=code%20review) statik kod analizini gerçekleştirin ve kod incelemeleriyle yardımcı olur.

### <a name="validate-and-sanitize-every-input-for-your-application"></a>Doğrulama ve uygulamanız için her bir giriş temizleyin

Tüm girişler, uygulamanızın en yaygın web uygulaması güvenlik açıklarına karşı korumak için güvenilmeyen olarak kabul eder. Güvenilir olmayan verileri ekleme saldırıları için kullanılan bir araçtır. URL'deki veritabanından veya bir API'den verileri kullanıcıdan giriş parametreleri uygulamanız için giriş içerir ve bir kullanıcı, geçirilen herhangi bir şey olabilecek işlemek. Bir uygulama gereken [doğrulama](https://www.owasp.org/index.php/OWASP_Proactive_Controls_2016#4:_Validate_All_Inputs) (kullanıcıya geri görüntüleme dahil) herhangi bir şekilde veri uygulamanın kullandığı önce veri sözdizimsel olarak ve anlamsal olarak geçerli olduğunu.

Erken yalnızca düzgün şekilde biçimlendirilmiş veri iş akışını girdiğinden emin olmak için veri akış girişi doğrulayın. Veritabanınızda kalıcı yapma veya bir aşağı akış bileşeni içinde bir arıza tetikleme hatalı oluşturulmuş veri istemezsiniz.

Blacklisting ve beyaz listeye ekleme giriş söz dizimi doğrulama gerçekleştirmek için iki genel yaklaşım vardır:

  - Kara liste belirli kullanıcı girişi "bilinen kötü amaçlı olarak" içerik içermediğini denetlemek çalışır.

  - Beyaz listeye ekleme, belirli kullanıcı girişi "bilinen iyi" girişleri kümesi eşleşip eşleşmediğini kontrol dener. Beyaz listeye ekleme karakter tabanlı bir beyaz listeye ekleme uygulamanın kullanıcı girişini yalnızca "bilinen iyi" bir karakter içeriyor ya da giriş, bilinen bir biçimde eşleştiğini nerede denetler biçimidir.
    Örneğin, bu bir kullanıcı adı yalnızca alfasayısal karakterler içerdiğini veya tam olarak iki sayı içerdiği denetimi gerektirebilir.

Beyaz listeye ekleme, güvenli yazılımlar oluşturmak için tercih edilen bir yaklaşımdır.
Potansiyel olarak hatalı giriş tam listesi düşünme mümkün olduğundan kara hataya olur.

Bu iş sunucuda, istemci tarafındaki (veya sunucu ve istemci tarafında) yapın.

### <a name="verify-your-applications-outputs"></a>Uygulamanızın çıkışları doğrulayın

Görsel olarak ya da belge içinde mevcut herhangi bir çıktı her zaman kodlanmış kaçış ve. [Kaçış](https://www.owasp.org/index.php/Injection_Theory#Escaping_.28aka_Output_Encoding.29)olarak da bilinen *çıktı kodlaması*, güvenilir olmayan verileri bir araç için bir ekleme saldırısına olmadığından emin olmak için kullanılır. , Veri doğrulama ile birlikte bir bütün olarak sistemin güvenliğini artırmak için katmanlı savunmaları sağlar.

Yaptığı emin kaçış her şeyin olarak görüntülenen *çıktı.* Kaçış veri yürütülmek üzere tasarlanmamıştır ve bu saldırıları çalışmasını engeller yorumlayıcı sağlar. Bu adlı başka bir yaygın saldırı tekniktir *siteler arası betik* (XSS).

Bir üçüncü taraf bir web çerçevesi kullanıyorsanız, seçeneklerinizi kullanarak Web sitelerinde çıkış kodlaması için doğrulayabilirsiniz [OWASP XSS önleme kağıdı](https://github.com/OWASP/CheatSheetSeries/blob/master/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.md).

### <a name="use-parameterized-queries-when-you-contact-the-database"></a>Veritabanı başvurduğunuzda parametreli sorgular kullanma

Satır içi veritabanı sorgusu "hareket halindeyken" kodunuzda hiçbir zaman oluşturun ve doğrudan veritabanına göndermek. Uygulamanıza eklediğiniz kötü amaçlı kod çalınırsa, silinebilen veya değiştirilecek veritabanınızı olası neden olabilir. Uygulamanız, veritabanınızı barındıran işletim sisteminde kötü amaçlı işletim sistemi komutlarını çalıştırmak için de kullanılabilir.

Bunun yerine, parametreli sorgular veya saklı yordamları kullanın. Parametreli sorgular kullandığınızda, yordamı kodunuzdan güvenli bir şekilde çağırmak ve sorgu deyimi bir parçası olarak değerlendirilir endişelenmeden dize geçirin.

### <a name="remove-standard-server-headers"></a>Standart sunucu üst bilgileri kaldırın

Sunucu, X-desteklenen-tarafından üst bilgileri ister ve X-ASP.NET-Version açığa sunucusu ve temel teknolojileri hakkında bilgi. Uygulama izinden önlemek için bu üst bilgilerini gösterme öneririz.
Bkz: [Azure Web siteleri standart sunucu başlıklarını kaldırma](https://azure.microsoft.com/blog/removing-standard-server-headers-on-windows-azure-web-sites/).

### <a name="segregate-your-production-data"></a>Üretim verilerinizi görevlerini birbirinden ayırın

Üretim verisi veya "gerçek" veriler, geliştirme, test ya da iş amaçlanan değerinden başka bir amaç için kullanılmamalıdır. Bir maskelenmiş ([anonim hale getirilen](https://en.wikipedia.org/wiki/Data_anonymization)) kullanılan tüm geliştirme ve sınama veri kümesi olması gerekir.

Bu, daha az sayıda, saldırı yüzeyini azaltan gerçek veri erişimi anlamına gelir. Ayrıca, daha az çalışanları gizliliği olası bir güvenlik ihlaline ortadan kaldırır, kişisel verileri görmek anlamına gelir.

### <a name="implement-a-strong-password-policy"></a>Güçlü parola ilkesini uygulama

Deneme yanılma ve sözlük tabanlı tahmin karşı korumak için kullanıcılar (örneğin, en az 12 karakter uzunluğu ve alfasayısal ve özel karakterler gerektiren) karmaşık bir parola oluşturduğunuzdan emin olmak için güçlü bir parola ilkesi uygulamalıdır.

Bir kimlik çerçevesi oluşturmak ve parola ilkelerini zorlamak için kullanabilirsiniz. Azure AD B2C yardımcı olur, parola yönetimi ile sağlayarak [yerleşik ilkeleri](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-policies#create-a-password-reset-policy), [Self Servis parola sıfırlama](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-reference-sspr)ve daha fazlası.

Varsayılan hesapları saldırılarına karşı korumak için tüm anahtarları ve parolaları değiştirilebilir ve bunlar oluşturulan veya kaynakları yükledikten sonra değiştirilen olduğunu doğrulayın.

Uygulamayı otomatik olarak parolaları oluşturmak gerekir, oluşturulan parolaların rastgele olduğunu ve yüksek entropi olduğunu emin olun.

### <a name="validate-file-uploads"></a>Dosya yüklemeleri doğrulama

Uygulamanızı izin veriyorsa [dosya yüklemeleri](https://www.owasp.org/index.php/Unrestricted_File_Upload), bu riskli etkinlik için uygulayabileceğiniz önlemleri göz önünde bulundurun. İlk adımda sayıda saldırı Saldırıya uğramış bir sisteme bazı kötü amaçlı kod almaktır. Dosyayı karşıya yükleme kullanarak bunu saldırgan yardımcı olur. OWASP doğrulamak için bir dosya karşıya yüklemekte dosya güvenli olmasını sağlamak için çözümler sunar.

Kötü amaçlı yazılımdan korunma belirlenmesi ve virüslerin, casus yazılımların ve diğer kötü amaçlı yazılım kaldırılmasına yardımcı olur. Yükleyebileceğiniz [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) veya bir Microsoft iş ortağı uç nokta koruma çözümüne ([Trend Micro](https://www.trendmicro.com/azure/), [Symantec](https://www.symantec.com/products), [McAfee](https://www.mcafee.com/us/products.aspx), [Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/windows-defender-antivirus-in-windows-10), ve [System Center Endpoint Protection](https://docs.microsoft.com/sccm/protect/deploy-use/endpoint-protection)).

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) gerçek zamanlı koruma, zamanlanmış tarama, kötü amaçlı yazılım düzeltme, imza güncelleştirmeleri, altyapı güncelleştirmeleri, raporlama örnekleri ve dışlama olay koleksiyonu gibi özellikler içerir. Microsoft Antimalware ve iş ortağı çözümleriyle tümleştirilebilir [Azure Güvenlik Merkezi](https://docs.microsoft.com/azure/security-center/security-center-partner-integration) kolay dağıtım ve yerleşik algılamalar (Uyarılar ve olaylar).

### <a name="dont-cache-sensitive-content"></a>Hassas içerikleri önbelleğe almak yok

Tarayıcıda hassas içeriği önbelleğe yok. Tarayıcılar, önbelleğe alma ve geçmiş bilgilerini depolayabilirsiniz. Önbelleğe alınan dosyaları, Internet Explorer söz konusu olduğunda Temporary Internet Files klasörü gibi bir klasörde depolanır. Ne zaman bu sayfaları tarayıcının yeniden adlandırılır, önbellek sayfalarındaki görüntüler. (Adres, kredi kartı bilgileri, sosyal güvenlik numarası, kullanıcı adı), hassas bilgileri kullanıcıya gösterilirse, bilgileri tarayıcının önbellekte depolanabilir ve alınabilir, tarayıcının önbelleğini inceleme veya yalnızca tarayıcının tuşlarınabasarak **Geri** düğmesi.

## <a name="verification"></a>Doğrulama
Doğrulama aşaması, kod, önceki aşamada oluşturulan güvenlik ve gizlilik ilkesi karşıladığından emin olmak için kapsamlı bir çaba kapsar.

### <a name="find-and-fix-vulnerabilities-in-your-application-dependencies"></a>Güvenlik açıklarını bulup uygulama bağımlılıklarınızı düzeltin

Uygulamanız ve onun bağımlı kitaplıkları bilinen tüm güvenlik açığı bileşenleri tanımlamak için tarama. Bu tarama gerçekleştirmek kullanılabilir olan ürünler içerir [OWASP bağımlılık denetleyin](https://www.owasp.org/index.php/OWASP_Dependency_Check),[Snyk](https://snyk.io/), ve [Black Duck](https://www.blackducksoftware.com/).

Güvenlik Açığı taraması ile desteklenen [Tinfoil Security](https://www.tinfoilsecurity.com/) Azure App Service Web Apps için kullanılabilir. [Tinfoil güvenlik App Service ile tarama](https://azure.microsoft.com/blog/web-vulnerability-scanning-for-azure-app-service-powered-by-tinfoil-security/) geliştiriciler ve Yöneticiler, bulma ve kötü amaçlı bir aktör bunları yararlanabilir önce güvenlik açıklarını adresleme hızlı, tümleşik ve ekonomik bir yöntem sunar.

> [!NOTE]
> Ayrıca [Tinfoil Security Azure AD ile tümleştirme](https://docs.microsoft.com/azure/active-directory/saas-apps/tinfoil-security-tutorial). Tinfoil Security Azure AD ile tümleştirme ile aşağıdaki avantajları sağlar:
>  - Azure AD'de için Tinfoil Security kimlerin erişebildiğini denetleyebilirsiniz.
>  - Kullanıcılarınızı otomatik olarak (çoklu oturum açma) için Tinfoil Security, Azure AD hesaplarını kullanarak oturum açmanız.
>  - Hesaplarınızı tek, merkezi bir konumda, Azure portalında yönetebilir.

### <a name="test-your-application-in-an-operating-state"></a>Bir işletim durumda uygulamanızı test edin

Dinamik uygulama (DAST) güvenlik testi bir uygulama güvenlik açıklarını bulmak için bir işletme durumda test bir işlemdir. Bellek Bozulması, güvenli olmayan sunucu yapılandırması, siteler arası betik, kullanıcı ayrıcalık sorunları, SQL ekleme ve diğer önemli güvenlik sorunları gibi güvenlik açıklarını bulmak için yürütme sırasında DAST Araçlar, programlar analiz edin.

Test (SAST) statik uygulama güvenlikten DAST farklıdır. SAST araçları, kaynak kodu analiz edin veya güvenlik açıkları bulmak için kod çalışmıyorken kod sürümlerini derlenmiş.

DAST, tercihen güvenlik uzmanı yardıma gerçekleştirin (bir [sızma tester](https://docs.microsoft.com/azure/security/azure-security-pen-testing) veya güvenlik açığı denetçisi). Bir güvenlik uzmanı kullanılabilir durumda değilse, DAST kendiniz bir web proxy tarayıcı ve bazı eğitim ile gerçekleştirebilirsiniz. Erkenden kodunuza güvenliğe dayalı bariz sorunları açmadığınızdan emin olmak için DAST tarayıcı takın. Bkz: [OWASP](https://www.owasp.org/index.php/Category:Vulnerability_Scanning_Tools) site için bir web uygulaması güvenlik açığı tarayıcıları listesi.

### <a name="perform-fuzz-testing"></a>Fuzz testi yapma

İçinde [rastgele test](https://cloudblogs.microsoft.com/microsoftsecure/2007/09/20/fuzz-testing-at-microsoft-and-the-triage-process/), program hatalarına kasıtlı olarak bir uygulama için hatalı biçimlendirilmiş ya da rastgele veri sunarak anlamına. Program hata inducing uygulama yayımlanmadan önce olası güvenlik sorunlarını ortaya yardımcı olur.

[Güvenlik riski algılama](https://docs.microsoft.com/security-risk-detection/) olan test etme hizmetinin yazılım güvenlik açısından kritik hataları bulmaya yönelik Microsoft benzersiz fuzz.

### <a name="conduct-attack-surface-review"></a>Saldırı yüzeyi gözden geçirme

Herhangi bir tasarım veya uygulama bir uygulamaya değiştirir veya sistem kabul kod tamamlama yaşamanıza sonra saldırı yüzeyini gözden geçirme. Tehdit modelleri de dahil olmak üzere değişikliklerin bir sonucu olarak oluşturulan tüm yeni saldırı vektörlerinin gözden azaltılabilir emin olun ve yardımcı olur.

Uygulama tarayarak, saldırı yüzeyini resmini oluşturabilirsiniz. Microsoft'un sunduğu adında bir saldırı yüzeyi analizi araç [saldırı yüzeyi Çözümleyicisi](https://www.microsoft.com/download/details.aspx?id=24487). Birçok ticari dinamik test etme ve araçları veya Hizmetleri dahil olmak üzere, tarama güvenlik açığı seçebileceğiniz [OWASP Lleştirilen saldırı Proxy proje](https://www.owasp.org/index.php/OWASP_Zed_Attack_Proxy_Project), [Arachni](http://arachni-scanner.com/), [Skipfish](http://code.google.com/p/skipfish/), ve [w3af](http://w3af.sourceforge.net/). Bu aracı, uygulamanızın gezinme ve web üzerinden erişilebilir olan uygulama bölümlerini eşleyin. Azure Marketi için arama da yapabilirsiniz benzer [Geliştirici Araçları](https://azuremarketplace.microsoft.com/marketplace/apps/category/developer-tools?page=1).

### <a name="perform-security-penetration-testing"></a>Güvenlik sızma testi gerçekleştirme

Uygulamanızın güvenli olmasını sağlamak, diğer tüm işlevlerden test olarak kadar önemlidir. Olun [sızma testi](https://docs.microsoft.com/azure/security/azure-security-pen-testing) derleme ve dağıtım işlemini standart bir parçası. Normal güvenlik testlerini ve güvenlik açığı taraması dağıtılan uygulamaları zamanlayın ve açık bağlantı noktalarını, uç noktaları ve saldırıları izleyin.

### <a name="run-security-verification-tests"></a>Güvenlik doğrulama testleri çalıştırın

[Azure DevOps Seti'ni güvenli](https://azsk.azurewebsites.net/index.html) (AzSK) için birden çok Azure platformu Hizmetleri SVTs içerir. Düzenli aralıklarla Azure aboneliğinizi ve uygulamanızı oluşturan farklı kaynakları güvenli bir durumda olduğundan emin olmak için bu SVTs çalıştırın. Ayrıca, Visual Studio uzantısı SVTs kullanılabilmesini AzSK, sürekli tümleştirme/sürekli dağıtım (CI/CD) uzantıları özelliğini kullanarak bu testleri otomatik hale getirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, güvenlik denetimleri öneririz ve yardımcı olabilecek etkinlikleri tasarım ve güvenli uygulamalar dağıtın.

- [Güvenli uygulamalar tasarlama](secure-design.md)
- [Güvenli uygulamalar dağıtma](secure-deploy.md)
