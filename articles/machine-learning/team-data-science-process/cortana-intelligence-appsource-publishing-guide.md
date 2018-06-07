---
title: Cortana Intelligence AppSource yayımlama Kılavuzu | Microsoft Docs
description: Microsoft Partner AppSource için Cortana Intelligence çözümünüzü yayımlamak için izlemeniz gereken adımlar şunlardır.
services: machine-learning
documentationcenter: ''
author: AnupamMicrosoft
manager: jhubbard
editor: cgronlun
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: anupams
ms.openlocfilehash: 3817d58cd61fb349d7815984420d0deb1ae0edd9
ms.sourcegitcommit: 6cf20e87414dedd0d4f0ae644696151e728633b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34808137"
---
# <a name="cortana-intelligence-appsource-publishing-guide"></a>Cortana Intelligence AppSource yayımlama Kılavuzu

## <a name="overview"></a>Genel Bakış
AppSource bulmak ve sorunsuz bir şekilde işletme çözümleri/iş ortakları tarafından oluşturulmuş ve Microsoft tarafından değerlendirilen uygulamalar denemek için iş karar alıcılar (BDMs) tek hedefi değil. Gözcü [bu videoyu](https://youtu.be/hpq_Y9LuIB8) AppSource nasıl çalıştığını öğrenin. 

Microsoft Partner varsa üzerinde AppSource yayımlama gerçekten yararlı olabilir:
- Bir akıllı çözüm/uygulama kullanılarak oluşturulan [Cortana Intelligence Suite](https://azure.microsoft.com/suites/cortana-intelligence-suite/?cdn=disable).
- Çözüm ya da uygulama belirli iş sorunu giderir.
- Modülleri veya müşterilerinize oldukça hızlı bir şekilde tahmin edilebilir bir şekilde yeniden kullanabilirsiniz fikri mülkiyet yerleşiktir.

Bir göz atalım [Cortana Intelligence çözümleri](https://appsource.microsoft.com/en-us/marketplace/apps?product=cortana-intelligence&page=1) , zaten yayımlanır AppSource üzerinde. 

Bu makalede AppSource için yayımlanan bir iş ortağının Cortana Intelligence çözüm almak için adımlarında yol gösterir

## <a name="getting-started"></a>Başlarken
1. İçinde [Partner topluluğunun avantajları Kılavuzu](https://www.microsoftpartnerserverandcloud.com/_layouts/download.aspx?SourceUrl=Hosted%20Documents/Partner%20Community%20Benefits%20Guide%20-%20Cloud%20and%20Enterprise.pdf) (PDF), sayfa 9 Advanced Analytics iş ortağı olarak listelenen için bkz.
1. Tamamlamak [uygulamanızı gönderme](https://appsource.microsoft.com/en-us/partners/list-an-app) formu.

    Soru için *"uygulamanız için yerleşik ürünleri seçin*", denetleme **diğer** düzenleme "Cortana Intelligence" onay kutusu ve liste denetimi.
1. Cortana Intelligence uygulama AppSource sayede sağlayabilmek için önce Cortana Intelligence'nın iş ortağı çözümü teknolojisi ekibi tarafından sertifikalı gerekir. İşlem sırasında anket formu doldurarak uygulamanızı Lütfen paylaşım ayrıntılarını başlatıldığından emin almak için [Cortana Intelligence çözüm değerlendirme AppSource yayımlama](https://aka.ms/cisappsrceval). Bu site parola korumalıysa ve site parola alma hakkında yönergeler içerir.

## <a name="solution-evaluation-criteria"></a>Çözüm değerlendirme ölçütleri
Uygulama gereksinimlerini karşılamak için ölçüt listesi aşağıdadır
1. Adres belirli iş gereksinimlerine göre uygulama servis talebi sorun, küçük yapılandırmaları ile tekrarlanabilir bir şekilde önceden tanımlanmış değer önermeleri kısa bir süre içinde implementable için belirli bir işlev alanı içinde kullanın.
1. Çözüm, aşağıdaki bileşenleri en az birini kullanmalıdır:

    - HDInsight
    - Machine Learning
    - Data Lake Analytics
    - Stream Analytics
    - Bilişsel Hizmetler
    - Robot Altyapısı
    - Analysis Services
    - Microsoft R Server tek başına bekleme
    - R hizmetlerinde SQL 2016 ya da Hdınsight Premium
1. En az $1000 DPOR/CSP kullanarak müşteri ayda bir çözüm oluşturmak.
1. Çözüm (AAD Federasyon SSO) Azure Active Directory Federasyon tek oturum açma kullanıcı kimlik doğrulaması ve kaynak erişim denetimleri için etkin onay kullanmalısınız. Değerlendirme ekibine çözümünüzün AAD Federasyon SSO uygulamanızı AppSource sayede çalıştırılmadan önce etkin olduğunu göstermek gerekir.

     AAD Federasyon SSO etkin olması ne anlama geldiğini görmek için 02:35 konumlandırmak arama [AppSource deneme sürümü deneyimi için size yol](https://aka.ms/trialexperienceforwebapps) video. Uygulamanız ile etkin değilse SSO AAD Federasyon henüz, ilgili bazı belgelerde İşte
   1. [Tek tıklamayla oturum açma](https://identity.microsoft.com/Landing?ru=https://identity.microsoft.com/).
   1. [Uygulamaları Azure Active Directory ile tümleştirme](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#adding-an-application).
     
1. Power BI çözümünüzde kullanın: isteğe bağlıdır, ancak daha yüksek sayıda müşteri adayları oluşturmak için kanıtlanmış önemle önerilir.

## <a name="devcenter-account-setup"></a>DevCenter hesabı kurulumu
Bu, şirketinizin Microsoft ile bir yayımcı olmasını kaydetme işlemidir. Kayıtlı bir yayımcı ile DevCenter hesabınız zaten varsa, DevCenter hesabınızla ilişkili e-posta kimliği paylaşır. Uygulamanızı yayımlama için onaylandıktan sonra tam belgelerine erişim karşılaşırsınız [yayımcı profil bulut Portal yönetme](https://cloudpartner.azure.com/#documentation/manage-publisher-profile)

Aşağıda, yoksa, bir DevCenter hesabı kurmak için temel adımlar verilmiştir.
1. Bir Microsoft hesabı oluşturmak [burada](https://signup.live.com/signup.aspx).
1. Microsoft DevCenter gidin [kayıt sayfası](http://go.microsoft.com/fwlink/?LinkId=615100) "kaydolun"'a tıklayın.
1. Ödeme için istendiğinde, biz size sağlanan kodunu kullanın. Bir kod yoksa, kişi [ appsourcecissupport@microsoft.com ](mailto:appsourcecissupport@microsoft.com?subject=Request%20for%20promotion-code%20for%20DevCenter%20account%20setup).
1. Seçin [ülke/bölge](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees) yaşadığınız veya işletmenizin bulunduğu. **Bu daha sonra değiştirmesi mümkün olmayacaktır.**
1. Seçin, [Geliştirici hesap türü](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees) için AppSource, select **şirket**. Bir şirket hesabı için bu gözden geçirdiğinizden emin olun [yönergeleri](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account).
1. Geliştirici hesabınız için kullanmak istediğiniz kişi bilgilerini girin.
Yönergeler Kurulumu DevCenter hesabı konusunda ayrıntılı adım adım yönergeler için bkz: [burada](https://docs.microsoft.com/azure/marketplace-publishing/marketplace-publishing-accounts-creation-registration).

## <a name="provide-info-for-microsoft-sellers"></a>Microsoft satıcıları için bilgilerini belirtin
AppSource anahtar değer önermeleri iş ortakları için iş ortağı uygulamalarını potansiyel müşterilerle önünde konumlandırma Microsoft Sellers ile işbirliği yapabilir için biridir.

Doldurup [Microsoft Sellers için iş ortağı çözümü bilgisi](https://aka.ms/aapartnerappinfo) ve göndermeden [ appsourcecissupport@microsoft.com ](mailto:appsourcecissupport@microsoft.com?subject=Request%20publisher%20account%20creation%20for%20%3cPartner%20Name%3e%20and%20whitelist%20owner/contributer%20AAD/MSA%20email%20IDs). Bu bir Cortana Intelligence uygulaması yayımlama için onay almanız için gerekli adımdır.

## <a name="build-a-compelling-customer-walkthrough-on-appsource"></a>İlgi çekici bir müşteri kılavuz üzerinde AppSource derleme
İlk olarak, bir göz atalım [Neal Analytics stok iyileştirme](https://appsource.microsoft.com/en-us/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview&tag=CISHome) AppSource üzerinde. AppSource her uygulama giriş için bir deneme sürümü deneyimi pdf belgelerini girişi dışında'nın üzerine başlık, özeti (en fazla 100 karakter), açıklama (max 1300 karakter), görüntüler, videoları (isteğe bağlı) sahiptir. İş ortakları tüm bunlar bir ilgi çekici müşteri deneyimi oluşturmak için bu hizmetlerden yararlanır.

İş ortakları, uçtan uca satış orchestration olarak AppSource üzerinde koyarlar içeriğin düşünmelisiniz. Müşteriler, başlık ve değer teklifinde anlamak ve görüntüler ve çözüm hakkındadır anlamak için videolar aracılığıyla gitmek için açıklama okuyun. Örnek olay incelemeleri müşterilerinizin nasıl Mevcut müşterileri görmek olası değer alınıyor. 

Tüm bu müşteri olmalısınız düşünüyor, ilgi ve isteyen daha fazla bilgi edinmek için. Bu aralık tabanlı deste sunu iyi teknik satış temsilcisi yeni müşteriler size yol olarak düşünün. Önerilen açıklama metni değere göre alt bölümlere ayırmak için propositions, her bir alt başlık ile vurgulanmış biçimidir. 

Ziyaretçilerin genellikle bakışta "Özet sunan" alan ve uygulama adreslerinin gist almak için alt başlıklar üzerinden ve neden bunlar düşünmelisiniz yalnızca hızlı bir bakış uygulamada. Bu nedenle, kullanıcının almak önemlidir dikkat edin bunları nedeni ayrıntıları almak için okumaya devam edin.

Bu iş ortaklarının yapmış olmanız bakın.
- [Neal Analytics stok en iyi duruma getirme](https://appsource.microsoft.com/en-us/product/web-apps/neal_analytics.8066ad01-1e61-40cd-bd33-9b86c65fa73a?tab=Overview)
- [Plexure perakende kişiselleştirme](https://appsource.microsoft.com/en-us/product/web-apps/plexure.c82dc2fc-817b-487e-ae83-1658c1bc8ff2?tab=Overview)
- [AvePoint Citizen Hizmetleri](https://appsource.microsoft.com/en-us/product/web-apps/avepoint.7738ac97-fd40-4ed3-aaab-327c3e0fe0b3?tab=Overview)

Son satış nasıl değer teklifinde teslim gösteren uygulama/çözüm gösterimini göstermek için işlemidir. Tam olarak bir müşteri deneme sürümü deneyimi AppSource üzerinde için tasarlanmıştır olmasıdır. İyi bir tanıtım aşağıdakileri yapın:
- Değer teklifinde çözümle etkileşime müşteri kesin içinde Kişiler listesini ve kısa uygulamada yeniden özetler
- Bir olay anlatın ve belirli sorunlarla ilgilenen bir örnek müşteri hakkında bağlam ayarlar
- Nasıl durum genellikle devolve ve müşterinin (önce) VS çözüm kullanımda olduğunda ne olacağını etkisi açıklanmaktadır. Bu Power BI panoları vb. kullanarak gösterilebilir.
- Nasıl çözüm herhangi belirli Machine Learning algoritmaları vb. kullanarak durum kolaylaştırır özetler.

İçinde AppSource eklenen içerik kaliteli olmalıdır ve aşağıdaki işlemleri etkinleştirmek için yeterli Dikiş:
- Bir iş ortağının teknik satış insanların kendi satış düzenlemesi kullanıyor olması gerekir. Satış ekipleriniz kullanmadan da aynı şeyi bölümlemeye MS satış terimleri bekleyebilirsiniz iyi bir oturum olur. Bu, müşteri tutarlı olarak aynı Öykü takım mates ve daha yüksek ups satın alma anlaşma yapılabilmesi bütçe ve onaylar almak için geçiş yapabilmek için iletişim noktası olanak tanır.
- Organik bir şekilde sitesinden bir müşteri, tüm kendilerini ve hissi tarafından geri sonraki adımlara devam taşıma için iş ortağı iletişimi yanıt verecek şekilde heyecan içerik aracılığıyla gidebilirsiniz.

İşte bu nedenle iş ortakları düşündüğünüz koyarlar uçtan uca satış orchestration olarak AppSource üzerinde içerik. Öykü satır ve deneme sürümü deneyimi gösterilecek özellikleri bağlı olarak, Teklif türü karar.

## <a name="publish-your-app-on-the-publishing-portal"></a>Uygulamanızı yayımlama portalında yayımlayın
Biz, uygulamanız için yukarıdaki adımları hesaplanan sonra yayımlama portalına erişim alırsınız ve görebilirsiniz [Cortana Intelligence teklif bulut iş ortağı Portalı aracılığıyla yayımlamak nasıl](https://cloudpartner.azure.com/#documentation/cloud-partner-portal-publish-cortana-intelligence-app) sonraki adımlar hakkında ayrıntılı yönergeler için.

Alanlar hakkında bilgi gerekiyorsa, e-posta <appsourcecissupport@microsoft.com>.
## <a name="my-app-is-published-on-appsource---now-what"></a>Uygulamam AppSource üzerinde - yayımlanan şimdi ne?
İlk olarak, uygulamanızı alma Tebrikler yayımladı.
Hedef kitle nasıl etkileyeceğini uygulamanıza AppSource yoğun yayımlama alma döndürür düzeyine bağlıdır. Bkz: [büyüme-korsan Cortana Intelligence uygulamanıza AppSource](http://aka.ms/aagrowthhackguide) nasıl, döndürür en üst düzeye hakkında daha fazla ayrıntı için.

Herhangi bir sorunuz veya önerileri varsa, lütfen bizimle ulaşmak <appsourcecissupport@microsoft.com>.

