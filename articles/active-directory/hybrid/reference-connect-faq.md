---
title: Azure Active Directory Connect SSS - | Microsoft Docs
description: Bu makalede, Azure AD Connect ile ilgili sık sorulan sorular yanıtlanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/02/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 50ec49c22c64780c8f887b12eef1dd0e75c379ed
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2018
ms.locfileid: "51010613"
---
# <a name="azure-active-directory-connect-faq"></a>Azure Active Directory Connect SSS

## <a name="general-installation"></a>Genel yükleme
**Denetimlerim Azure Active Directory (Azure AD) genel yönetici etkin iki öğeli kimlik doğrulamayı (2FA) varsa yükleme çalışır mı?**  
Şubat 2016 yapıları itibarıyla, bu senaryo desteklenmiyor.

**S: Azure AD Connect katılımsız yüklemek için bir yol var mı?**  
Azure AD Connect yüklemesi, Yükleme Sihirbazı'nı kullandığınızda desteklenir. Bir katılımsız, sessiz yüklemesi desteklenmez.

**S:, burada bir etki alanına erişilemiyor bir ormana sahibim. Azure AD Connect'i nasıl yüklerim?**  
Şubat 2016 yapıları itibarıyla, bu senaryo desteklenmiyor.

**S: Azure Active Directory etki alanı (Azure AD DS) health Aracısı sunucu Çekirdeğinde çalışma Hizmetleri mu?**  
Evet. Aracıyı yükledikten sonra aşağıdaki PowerShell cmdlet'ini kullanarak kayıt işlemini tamamlayabilirsiniz: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**S: Azure AD Connect eşitleme Azure AD'de desteği için iki etki alanlarından mu?**  
Evet, bu senaryo desteklenir. Başvurmak [birden çok etki alanı](how-to-connect-install-multiple-domains.md).
 
**Azure AD Connect aynı Active Directory etki alanı için birden çok takılabilecek miyim?**  
Hayır, birden çok bağlayıcı aynı AD etki alanı için desteklenmiyor. 

**Azure AD Connect veritabanını yerel veritabanı uzak bir SQL Server örneğine taşırım miyim?**   
Evet, aşağıdaki adımlar bunu nasıl genel rehberlik sağlar. Şu anda daha ayrıntılı bir belge üzerinde çalışıyoruz.
1. LocalDB ADSync veritabanını yedekleyin.
Bunu yapmanın en kolay yolu, Azure AD Connect ile aynı makinede yüklü SQL Server Management Studio kullanmaktır. Bağlanma *(LocalDb). \ADSync*ve ardından ad eşitleme veritabanını yedekleyin.

2. Ad eşitleme veritabanını uzak SQL Server örneğine geri yükleyin.

3. Varolan karşı Azure AD Connect'i yükleme [uzak bir SQL veritabanı](how-to-connect-install-existing-database.md).
   Makalede, yerel bir SQL veritabanına geçirme gösterilmektedir. İşlem 5. adımında, uzak bir SQL veritabanına geçiş yapıyorsanız, eşitleme Windows hizmeti olarak çalıştırılır mevcut bir hizmet hesabı da girmelisiniz. Bu eşitleme altyapısı hizmet hesabı aşağıda açıklanmıştır:
   
      **Mevcut bir hizmet hesabını kullanma**: kullanılacak varsayılan olarak, Azure AD Connect Eşitleme Hizmetleri için bir sanal hizmet hesabı kullanır. Uzak SQL Server örneğini kullanmak veya kimlik doğrulaması gerektiren bir ara sunucu kullanıyorsanız, yönetilen hizmet hesabı veya bir hizmet hesabı etki alanınızı kullanmak ve parolayı biliyor. Bu gibi durumlarda kullanılacak olan hesabı girin. Oturum açma kimlik bilgileri hizmet hesabı için oluşturulan yüklemesini çalıştıran kullanıcılar SQL sistem yöneticileri olduğundan emin olun. Daha fazla bilgi için [Azure AD Connect hesapları ve izinleri](reference-connect-accounts-permissions.md#adsync-service-account). 
   
      En son sürümle, veritabanını sağlama, artık SQL yöneticisi tarafından bant dışında gerçekleştirilebilir ve ardından veritabanı sahibi haklarıyla Azure AD Connect yöneticisi tarafından yüklenebilir. Daha fazla bilgi için [SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme](how-to-connect-install-sql-delegation.md).

Örneği basit tutmak için Azure AD Connect'i yükleyen kullanıcılar SQL sistem yöneticileri olmasını öneririz. Kullanım SQL yönetici temsilcisi Bununla birlikte, son yapılar ile açıklandığı gibi artık [SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme](how-to-connect-install-sql-delegation.md).

## <a name="network"></a>Ağ
**S: bir güvenlik duvarı, ağ aygıtı veya bağlantıları Ağımdaki açık kalabileceği süreyi sınırlar sorun vardır. Azure AD Connect kullandığımda ne my istemci-tarafı zaman aşımı eşiğini olmalıdır?**  
Tüm ağ yazılımı, fiziksel cihazlar veya başka bir şey bağlantıları açık kalabileceği en uzun süreyi sınırlayan bir eşiği en az beş dakika (300 saniye), Azure AD Connect istemcisinin yüklü olduğu sunucu arasında bağlantı kurmak için kullanmanız gerekir ve Azure Active Directory. Bu öneri, tüm daha önce yayımlanmış Microsoft Identity eşitleme araçları için de geçerlidir.

**S: desteklenen tek etiketli etki alanları (SLD'ler)?**  
Bu ağ yapılandırması karşı öneririz ancak ([makaleye bakın](https://support.microsoft.com/help/2269810/microsoft-support-for-single-label-domains)), bir tek etiketli etki alanı ile Azure AD Connect eşitleme kullanarak desteklenir, tek düzey etki alanı için ağ yapılandırması çalıştığından sürece doğru.

**S: desteklenen kopuk AD etki alanları ile ormanları?**  
Hayır, Azure AD Connect, kopuk ad alanlarını içeren şirket içi ormana desteklemez.

**S: "desteklenen NetBIOS adları noktalı"?**  
Hayır, Azure AD Connect, şirket içi orman veya etki alanlarını burada NetBIOS adını içeriyorsa bir nokta (.) desteklemez.

**S: desteklenen saf IPv6 ortamı var mı?**  
Hayır, Azure AD Connect, saf bir IPv6 ortamına desteklemez.

**Soru: çok ormanlı bir ortamda sahibim ve iki orman arasında ağ NAT (ağ adresi çevirisi) kullanıyor. Azure AD Connect, desteklenen bu iki orman arasında kullanıyor mu?**</br>
 Hayır, Azure AD Connect kullanarak NAT üzerinde desteklenmiyor. 

## <a name="federation"></a>Federasyon
**S: Office 365 Sertifikamı yenilemek için bana isteyen bir e-posta alıyorum, ne yapmalıyım?**  
Sertifika yenileme hakkında yönergeler için bkz. [sertifikalarını yenilemesi](how-to-connect-fed-o365-certs.md).

**S: "otomatik olarak bağlı olan taraf Office 365 bağlı olan taraf için kümesi güncelleştirmesi" vardır. İmzalama sertifikasını otomatik olarak belirtecimi geldiğinde, herhangi bir eylemde bulunmanız gerekir mi?**  
Makalede açıklanan yönergeleri kullanın [sertifikalarını yenilemesi](how-to-connect-fed-o365-certs.md).

## <a name="environment"></a>Ortam
**S: Azure AD Connect yüklendikten sonra sunucuyu yeniden adlandırmak için desteklenen var mı?**  
Hayır. Eşitleme Altyapısı SQL veritabanı örneğine bağlanamıyor oluşturur ve sunucu adını değiştirme ve hizmet başlatılamıyor.

## <a name="identity-data"></a>Kimlik verilerini
**S: neden Azure AD userPrincipalName (UPN) öznitelik, şirket içi UPN eşleşmeyen?**  
Bilgi için şu makalelere bakın:

* [Office 365, Azure veya Intune'daki kullanıcı adları, şirket içi UPN veya alternatif oturum açma kimliği eşleşmiyor.](https://support.microsoft.com/kb/2523192)
* [Farklı bir Federasyon etki alanı kullanmak için bir kullanıcı hesabının UPN'sini değiştirdikten sonra değişiklikler tarafından Azure Active Directory Eşitleme Aracı eşitlenmiş değil](https://support.microsoft.com/kb/2669550)

Azure AD eşitleme altyapısı UPN güncelleştirmek açıklandığı izin vermek için de yapılandırabilirsiniz [Azure AD Connect eşitleme hizmeti özelliklerini](how-to-connect-syncservice-features.md).

**S: geçici bir şirket içi Azure AD grup veya kişi nesnesini içeren mevcut bir Azure AD grubu eşleştirme veya nesne desteklenen var mı?**  
Evet, bu geçici bir eşleşme proxyAddress üzerinde temel alır. Geçici eşleştirme posta-etkin olmayan gruplar için desteklenmez.

**S: el ile var olan bir Azure AD grubuna İmmutableıd özniteliği ayarlayın ya da sabit, şirket içi Azure AD grubu için eşleştirme veya nesne başvurun nesnesine başvurun desteklenen var mı?**  
Hayır, İmmutableıd özniteliği sabit, eşleştirme için varolan bir Azure AD grup veya kişi nesnesini üzerinde ayarı el ile şu anda desteklenmiyor.

## <a name="custom-configuration"></a>Özel yapılandırma
**S: nereden Azure AD Connect için PowerShell cmdlet'leri belgelenen?**  
Bu sitede belgelenen cmdlet'leri hariç olmak üzere Azure AD Connect içinde bulunan diğer PowerShell cmdlet'leri, müşteri kullanım için desteklenmez.

**Eşitleme hizmeti yapılandırma sunucuları arasında taşıyor Yöneticisi'nde bulunan "Sunucu dışarı aktarma/sunucu Al" seçeneğini kullanabilirim miyim?**  
Hayır. Bu seçenek, tüm yapılandırma ayarlarını almaz ve onu kullanılmamalıdır. Bunun yerine, ikinci sunucusunda temel yapılandırma oluşturmak için Sihirbazı'nı kullanın ve sunucular arasında herhangi bir özel kural taşımak için PowerShell komut dosyaları oluşturmak için eşitleme kuralı Düzenleyicisi'ni kullanın. Daha fazla bilgi için [Swing geçişi](how-to-upgrade-previous-version.md#swing-migration).

**Azure oturum açma sayfasındaki parolalar önbelleğe oluşturabilir ve bir parola input öğesi içerdiği için bu önbelleğe alma engellenebilir *otomatik tamamlama = "false"* özniteliği?**  
Şu anda HTML özniteliklerini değiştirme **parola** alanı, otomatik tamamlama etiketi de dahil olmak üzere desteklenmez. Herhangi bir öznitelik eklemenize olanak sağlayan özel bir JavaScript olanak sağlayan bir özellik üzerinde çalışıyoruz **parola** alan.

**S: Azure oturum açma sayfasında, daha önce başarıyla oturum açmış kullanıcıların kullanıcı adlarını görüntüler. Bu davranışı devre dışı bırakılabilir mi?**  
Şu anda HTML özniteliklerini değiştirme **parola** otomatik tamamlama etiketi de dahil olmak üzere, giriş alanı desteklenmez. Herhangi bir öznitelik eklemenize olanak sağlayan özel bir JavaScript olanak sağlayan bir özellik üzerinde çalışıyoruz **parola** alan.

**S: eşzamanlı oturum önlemek için bir yol var mı?**  
Hayır.

## <a name="auto-upgrade"></a>Otomatik yükseltme

**S: avantajları nelerdir ve yükseltme kullanarak sonuçlarıyla otomatik?**  
Biz, tüm müşterilerin kendi Azure AD Connect yüklemesi için otomatik yükseltmeyi etkinleştir bildiren. Her zaman Azure AD Connect içinde bulunan güvenlik açıkları için güvenlik güncelleştirmeleri de dahil olmak üzere en son düzeltme eklerini alır avantajdır. Yükseltme işlemi sorunsuzdur ve otomatik olarak yeni bir sürümü var. hemen sonra gerçekleşir. Binlerce Azure AD Connect müşteriler otomatik yükseltmeyi her yeni sürümle birlikte kullanın.

Otomatik yükseltme işlemi her zaman ilk yüklemenin otomatik yükseltme için uygun olup olmadığını belirler. Uygun durumda, yükseltme gerçekleştirilen ve test. İşlem, kuralları ve belirli bir çevresel etmenler özel değişiklikler aranıyor de içerir. Testleri yükseltme başarısız olduğunu gösterirse, önceki sürümü otomatik olarak geri yüklenir.

Ortamın boyutuna bağlı olarak, işlem birkaç saat sürebilir. Yükseltme işlemi devam ederken Windows Server Active Directory ve Azure AD arasında hiçbir eşitleme gerçekleşir.

**S: me, my otomatik yükseltme artık çalışır ve yeni bir sürümünü yüklemeniz gerekir bildiren bir e-posta aldım. Bunu yapmak neden ihtiyacım var?**  
Geçen yıl, belirli koşullar altında sunucunuzda otomatik yükseltme özelliğini devre dışı bırakmış olabilir, Azure AD Connect sürümünü çıkardık. Azure AD Connect sürümü 1.1.750.0 sorunu düzelttik. Bu sorundan etkilendiğini, onarmak için bir PowerShell Betiği çalıştırılarak veya el ile Azure AD Connect'in en son sürüme yükseltme azaltabilirsiniz. 

PowerShell betiğini çalıştırmak için [betiği indirin](https://aka.ms/repairaadconnect) ve uygulamayı Azure AD Connect sunucunuzda yönetici bir PowerShell penceresi. Betik çalıştırma hakkında bilgi edinmek için [bu kısa videoyu izleyin](https://aka.ms/repairaadcau).

El ile yükseltmek için indirin ve AADConnect.msi dosyanın en son sürümünü çalıştırması gerekir.
 
-  Geçerli sürümünüzü 1.1.750.0 eskiyse [indirin ve en son sürüme yükseltme](https://www.microsoft.com/download/details.aspx?id=47594).
- Azure AD Connect sürümünüzü 1.1.750.0 veya daha sonra başka bir eylem gerekli ise. Otomatik yükseltme düzeltme içeren bir sürüm zaten kullanmakta olduğunuz. 

**S: otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmek için bana bildiren bir e-posta aldım. Sürüm 1.1.654.0 kullanıyorum. Yükseltmem gerekiyor mu?**  
Evet, 1.1.750.0 sürümüne yükseltin veya daha sonra otomatik yükseltmeyi yeniden etkinleştirmek için gerekir. [Karşıdan yükle ve en son sürüme yükseltme](https://www.microsoft.com/download/details.aspx?id=47594).

**S: otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmek için bana bildiren bir e-posta aldım. Otomatik yükseltmeyi etkinleştirmek için PowerShell kullandığımı olursa hala en son sürümünü yüklemek ihtiyacım var?**  
Evet, yine 1.1.750.0 sürüme yükseltin veya üzeri gerekir. PowerShell ile otomatik yükseltme hizmetini etkinleştirme 1.1.750.0 öncesi sürümlerinde bulunan Otomatik yükseltme sorunu hafifletmeye değil.

**S: yeni bir sürüme yükseltmek istiyor ancak Azure AD Connect yüklemiş emin değilim ve kullanıcı adı ve parola sahibiz değil. Bunu yapmamız gerekir?**
Kullanıcı adı ve Azure AD Connect'e yükseltmenin başlangıçta kullanılan parolayı bilmeniz gerekmez. Genel yönetici rolüne sahip herhangi bir Azure AD hesabını kullanın.

**S: hangi Azure AD Connect sürümü kullanıyorum nasıl bulabilirim?**  
Hangi sürümün Azure AD Connect sunucunuzda yüklendiğini doğrulamak için Denetim Masası'na gidin ve Microsoft Azure AD Connect'in yüklü sürümü seçerek görüneceğini **programlar** > **programlar ve Özellikler** , burada gösterildiği gibi:

![Denetim Masası'nda Azure AD Connect sürümü](./media/reference-connect-faq/faq1.png)

**S: Azure AD Connect'in en son sürüme nasıl yükseltebilirim?**  
En son sürüme yükseltme öğrenmek için bkz. [Azure AD Connect: önceki bir sürümden yükseltme en son](how-to-upgrade-previous-version.md). 

**S: size Azure AD Connect'in en son sürüme geçen yıl zaten yükseltildi. Yeniden yükseltmeye gerekiyor mu?**  
Azure AD Connect takım sık sık güncelleştirme hizmetine yapar. Hata düzeltmeleri ve güvenlik güncelleştirmeleri yeni özellikleri de yararlanmak için sunucunuzun en son sürümüyle güncel tutmak önemlidir. Otomatik yükseltme etkinleştirirseniz, Yazılım sürümünüzün otomatik olarak güncelleştirilir. Azure AD Connect sürüm yayımlama geçmişi bulmak için bkz: [Azure AD Connect: sürüm yayımlama geçmişi](reference-connect-version-history.md).

**S: ne kadar yükseltmeyi gerçekleştirmek için sürer ve kullanıcılar üzerindeki etkisi nedir?**  
Yükseltme için gereken süre, Kiracı boyutuna bağlıdır. Büyük kuruluşlarda, akşam veya hafta sonu yükseltmeyi gerçekleştirmek en iyi yöntem olabilir. Yükseltme sırasında hiçbir eşitleme etkinliği gerçekleşir.

**S: ben Azure AD Connect, ancak Office portalı sürümüne yükselttiğimde düşünüyorsanız hala DirSync değinmektedir. Neden bu mi?**  
Office ekibi, geçerli bir ürün adı yansıtacak şekilde Office portalı güncelleştirmelerini almak için çalışmaktadır. Kullanmakta olduğunuz hangi eşitleme aracı yansıtmaz.

**S: Alanım otomatik yükseltme durumunu yazan, "Askıya alındı." Neden şu askıya alındı? Onu etkinleştirmeniz gerekir?**  
Bir hata, belirli koşullar altında otomatik yükseltme durumu "Askıya alındı." olarak ayarlanmış bırakır önceki bir sürümünde kullanıma sunulmuştur El ile etkinleştirme, teknik olarak mümkündür ama karmaşık birkaç adım gerektirir. Yapabileceğiniz en iyi şey, Azure AD Connect'in en son sürümünü yüklemek olabilir.

**S: Şirketim katı değişiklik Yönetimi gereksinimleri vardır ve ne zaman düğmeye denetlemek istiyorum. Otomatik yükseltme ne zaman başlatılır denetleyebilir miyim?**  
Hayır, var. böyle bir özellik Bugün Özellik gelecekteki sürümlerde sunulması değerlendirilir.

**S: Otomatik yükseltme başarısız olursa bir e-posta alırım? Başarılı olduğunu nasıl bilebilirim?**  
Yükseltmenin sonucu bildirilmez. Özellik gelecekteki sürümlerde sunulması değerlendirilir.

**S: Otomatik yükseltme konusunu göndermek planlama yaparken zaman çizelgesi için yayımlama?**  
Otomatik yükseltme daha yeni bir sürüme sürüm sürecinin ilk adımıdır. Yeni bir sürüm olduğunda, yükseltme otomatik olarak gönderilir. Daha yeni sürümleri, Azure AD Connect, önceden duyurulmuş [Azure AD yol haritası](../fundamentals/whats-new.md).

**S: Otomatik yükseltme, ayrıca Azure AD Connect Health yükseltme mu?**  
Evet, Otomatik yükseltme, Azure AD Connect Health da yükseltir.

**S:, aynı zamanda otomatik hazırlama modunda Azure AD Connect sunucuları yükseltme?**  
Evet, otomatik hazırlama modunda olan bir Azure AD Connect sunucusu yükseltme.

**S: Otomatik yükseltme başarısız olur ve Azure AD Connect sunucumu başlamıyor varsa ne yapmalıyım?**  
Nadiren de olsa, yükseltme gerçekleştirildikten sonra Azure AD Connect hizmetini başlamıyor. Bu durumlarda, genellikle sunucunun yeniden başlatılması sorunu giderir. Azure AD Connect hizmetini hala başlatılmazsa, bir destek bileti açın. Daha fazla bilgi için [Office 365 desteğe başvurmak için bir hizmet isteği oluşturduğunuzda](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**S: Ben Azure AD Connect'in yeni bir sürüme yükseltme yaptığınızda riskler nelerdir emin değilim. Bana bana yükseltmede size yardımcı olacak çağırabilir miyim?**  
Yardıma ihtiyacınız varsa Azure AD Connect'in yeni bir sürüme yükseltme sırasında bir destek bileti açın [Office 365 desteğe başvurmak için bir hizmet isteği oluşturduğunuzda](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Sorun giderme
**S: Azure AD Connect ile ilgili Yardım alabilirim?**

[Microsoft Bilgi Bankası (KB) arama](https://www.microsoft.com/en-us/search/result.aspx?q=azure+active+directory+connect)

* KB ortak onarım sorunları için Azure AD Connect desteği hakkında teknik çözümler için arama yapın.

[Azure Active Directory forumları](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Teknik sorular ve yanıtlar için arama yapın veya giderek kendi sorularınızı sorun [Azure AD'ye topluluk](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)
