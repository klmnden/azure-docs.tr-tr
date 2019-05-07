---
title: Azure Active Directory Connect SSS - | Microsoft Docs
description: Bu makalede, Azure AD Connect ile ilgili sık sorulan sorular yanıtlanmaktadır.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/03/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2caca430de5ad666f4f4341e0723bc3173d6d91a
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65137796"
---
# <a name="azure-active-directory-connect-faq"></a>Azure Active Directory Connect SSS

## <a name="general-installation"></a>Genel yükleme

**S: Güvenlik saldırı yüzeyini azaltmak için Azure AD Connect sunucuma nasıl sağlamlaştırmak?**

Microsoft, bu kritik bir bileşen BT ortamınızın için güvenlik saldırı yüzeyini azaltmak için Azure AD Connect sunucunuzu sağlamlaştırma önerir.  Aşağıdaki aşağıdaki önerileri, kuruluşunuz için güvenlik risklerini azaltır.

* Azure AD Connect bir etki alanına katılmış sunucuya dağıtma ve etki alanı yöneticileri veya diğer sıkı bir şekilde denetlenen güvenlik gruplarına yönetici erişimi kısıtlama

Daha fazla bilgi için bkz: 

* [Yöneticiler grubunun güvenliğini sağlama](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-g--securing-administrators-groups-in-active-directory)

* [Yerleşik yönetici hesaplarını güvenli hale getirme](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/appendix-d--securing-built-in-administrator-accounts-in-active-directory)

* [Güvenlik geliştirme ve saldırı yüzeylerini azaltarak sustainment](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access#2-reduce-attack-surfaces )

* [Active Directory saldırı yüzeyini azaltma](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/reducing-the-active-directory-attack-surface)

**S: Azure Active Directory (Azure AD) genel yönetici etkin iki öğeli kimlik doğrulamayı (2FA) varsa, yükleme çalışacak mı?**  
Şubat 2016 yapıları itibarıyla, bu senaryo desteklenmiyor.

**S: Azure AD Connect katılımsız yüklemek için bir yol var mı?**  
Azure AD Connect yüklemesi, Yükleme Sihirbazı'nı kullandığınızda desteklenir. Bir katılımsız, sessiz yüklemesi desteklenmez.

**S: Burada bir etki alanına erişilemiyor bir ormana sahibim. Azure AD Connect'i nasıl yüklerim?**  
Şubat 2016 yapıları itibarıyla, bu senaryo desteklenmiyor.

**S: Azure Active Directory etki alanı Hizmetleri (Azure AD DS) sistem durumu aracısı, Sunucu Çekirdeği'nde çalışır mı?**  
Evet. Aracıyı yükledikten sonra aşağıdaki PowerShell cmdlet'ini kullanarak kayıt işlemini tamamlayabilirsiniz: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**S: Azure AD Connect için iki etki alanlarının Azure AD'de eşitleme destekliyor mu?**  
Evet, bu senaryo desteklenir. Başvurmak [birden çok etki alanı](how-to-connect-install-multiple-domains.md).
 
**S: Azure AD Connect aynı Active Directory etki alanı için birden fazla bağlayıcıyı olabilir mi?**  
Hayır, birden çok bağlayıcı aynı AD etki alanı için desteklenmiyor. 

**S: Uzak bir SQL Server örneği için Azure AD Connect'in veritabanı yerel veritabanından taşıyabilirim?**   
Evet, aşağıdaki adımlar bunu nasıl genel rehberlik sağlar. Şu anda daha ayrıntılı bir belge üzerinde çalışıyoruz.
1. LocalDB ADSync veritabanını yedekleyin.
Bunu yapmanın en kolay yolu, Azure AD Connect ile aynı makinede yüklü SQL Server Management Studio kullanmaktır. Bağlanma *(LocalDb). \ADSync*ve ardından ad eşitleme veritabanını yedekleyin.

2. Ad eşitleme veritabanını uzak SQL Server örneğine geri yükleyin.

3. Varolan karşı Azure AD Connect'i yükleme [uzak bir SQL veritabanı](how-to-connect-install-existing-database.md).
   Makalede, yerel bir SQL veritabanına geçirme gösterilmektedir. İşlem 5. adımında, uzak bir SQL veritabanına geçiş yapıyorsanız, eşitleme Windows hizmeti olarak çalıştırılır mevcut bir hizmet hesabı da girmelisiniz. Bu eşitleme altyapısı hizmet hesabı aşağıda açıklanmıştır:
   
      **Mevcut bir hizmet hesabını kullanma**: Azure AD Connect, varsayılan olarak, kullanılacak sanal hizmet hesabı Eşitleme Hizmetleri kullanır. Uzak SQL Server örneğini kullanmak veya kimlik doğrulaması gerektiren bir ara sunucu kullanıyorsanız, yönetilen hizmet hesabı veya bir hizmet hesabı etki alanınızı kullanmak ve parolayı biliyor. Bu gibi durumlarda kullanılacak olan hesabı girin. Oturum açma kimlik bilgileri hizmet hesabı için oluşturulan yüklemesini çalıştıran kullanıcılar SQL sistem yöneticileri olduğundan emin olun. Daha fazla bilgi için [Azure AD Connect hesapları ve izinleri](reference-connect-accounts-permissions.md#adsync-service-account). 
   
      En son sürümle, veritabanını sağlama, artık SQL yöneticisi tarafından bant dışında gerçekleştirilebilir ve ardından veritabanı sahibi haklarıyla Azure AD Connect yöneticisi tarafından yüklenebilir. Daha fazla bilgi için [SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme](how-to-connect-install-sql-delegation.md).

Örneği basit tutmak için Azure AD Connect'i yükleyen kullanıcılar SQL sistem yöneticileri olmasını öneririz. Kullanım SQL yönetici temsilcisi Bununla birlikte, son yapılar ile açıklandığı gibi artık [SQL yönetici temsilcisi izinlerini kullanarak Azure AD Connect'i yükleme](how-to-connect-install-sql-delegation.md).

**S: Bazı alanından en iyi uygulamalar nelerdir?**  

Bazı en iyi yöntemler, mühendislik, destek sunan bir bilgilendirme belge verilmiştir ve Danışmanlarımız yıllar içinde geliştirdik.  Bu hızlı bir şekilde başvurulabilen bir madde işareti listede sunulur.  Bu liste kapsamlı olarak çalışır, ancak bu listede henüz yapmış olabilirsiniz değil ek iyi olabilir.

- Yerel kalmalıdır sonra tam SQL kullanıyorsanız uzaktan karşılaştırması
    - Daha az atlama
    - Daha kolay sorun giderme
    - Daha az karmaşıklık
    - SQL kaynakları atamak ve Azure AD Connect ve işletim sistemi için ek yükü izin vermeniz gerekir
- Bu tamamen mümkünse proxy atlama, emin olmanız gerekir daha sonra proxy atlama ise zaman aşımı değeri 5 dakikadan fazla olur.
- Ara sunucu gerekliyse sonra proxy machine.config dosyasına eklemeniz gerekir
- Yerel SQL işleri ve Bakım ve özellikle yeniden dizin oluşturma - Azure AD Connect'i nasıl etkiler unutmayın
- Harici olarak çözümlemek için DNS daha emin olun.
- Emin [sunucu belirtimlerini](how-to-connect-install-prerequisites.md#hardware-requirements-for-azure-ad-connect) öneri fiziksel veya sanal sunucular kullanmanıza olan
- Bir sanal sunucu için gereken kaynakları ayrılmış kullanıyorsanız emin olun.
- SQL Server için en iyi karşılayan disk yapılandırması ve disk olduğundan emin olun
- Yükleme ve Azure AD Connect Health, izleme için yapılandırma
- Azure AD Connect ile oluşturulan silme eşiği kullanın.
- Tüm değişiklikleri ve eklenebilir yeni öznitelikler için hazırlıklı olmak için dikkatle gözden geçirme sürüm güncelleştirmeleri
- Her şeyi yedekleme
    - Yedekleme anahtarları
    - Yedekleme eşitleme kuralları
    - Yedekleme sunucusu yapılandırması
    - SQL veritabanını yedekle
- SQL VSS Yazıcı (3. taraf anlık görüntüleri içeren sanal sunucular ortak) olmadan SQL yedekleyen 3 taraf yedekleme aracı olduğundan emin olun
- Bunlar karmaşıklığı eklerken kullanılan özel eşitleme kurallarını miktarını sınırlamak
- Katman 0 sunucuları gibi davranma Azure AD Connect sunucuları
- Bulut eşitleme kuralları harika anlama etkisi ve doğru iş sürücüleri olmadan değiştirilmesini leery olabilir
- Güvenlik Duvarı bağlantı noktaları ve doğru URL'nin Azure AD Connect ve Azure AD Connect Health desteği için açık olduğundan emin olun
- Sorun giderme ve sahte nesneler önlemek için bulut filtrelenmiş bir öznitelik yararlanın
- Hazırlama sunucusu ile sunucu arasında tutarlılık için Azure AD Connect yapılandırma Belgeleyici'yi kullandığınızdan emin olun.
- Hazırlama sunucuları ayrı veri merkezleri (fiziksel konumlara olmalıdır
- Hazırlama sunucuları yüksek oranda kullanılabilirlik çözümü olması beklenmez, ancak birden çok hazırlık sunucusu olabilir.
- Bir "Lag" hazırlama sunucuları ile tanışın hata durumunda bazı olası kapalı kalma süresini azaltmak
- Test ve hazırlık sunucu üzerindeki tüm yükseltmelerde ilk doğrulama
- Her zaman için hazırlama serverLeverage tam içeri aktarmalar ve tam eşitlemeler iş etkisini azaltmak hazırlık sunucusu geçmeden önce dışarı aktarmaları doğrula
- Azure AD Connect sunucuları arasında sürüm tutarlılığı mümkün olduğunca tutun 

**S: Çalışma grubu makinesinde Azure AD Bağlayıcısı hesabı oluşturmak Azure AD Connect izin ver?**
Hayır.  Azure AD Bağlayıcısı hesabı otomatik olarak oluşturmak Azure AD Connect izin vermek üzere makine etki alanına katılmış olması gerekir.  

## <a name="network"></a>Ağ
**S: Bir güvenlik duvarı, ağ aygıtı veya bağlantıları Ağımdaki açık kalabileceği süreyi sınırlar sorun sahibim. Azure AD Connect kullandığımda ne my istemci-tarafı zaman aşımı eşiğini olmalıdır?**  
Tüm ağ yazılımı, fiziksel cihazlar veya başka bir şey bağlantıları açık kalabileceği en uzun süreyi sınırlayan bir eşiği en az beş dakika (300 saniye), Azure AD Connect istemcisinin yüklü olduğu sunucu arasında bağlantı kurmak için kullanmanız gerekir ve Azure Active Directory. Bu öneri, tüm daha önce yayımlanmış Microsoft Identity eşitleme araçları için de geçerlidir.

**S: Tek etiketli etki alanları (SLD'ler) destekleniyor?**  
Bu ağ yapılandırması karşı öneririz ancak ([makaleye bakın](https://support.microsoft.com/help/2269810/microsoft-support-for-single-label-domains)), bir tek etiketli etki alanı ile Azure AD Connect eşitleme kullanarak desteklenir, tek düzey etki alanı için ağ yapılandırması çalıştığından sürece doğru.

**S: Orman, desteklenen kopuk AD etki alanlarıyla misiniz?**  
Hayır, Azure AD Connect, kopuk ad alanlarını içeren şirket içi ormana desteklemez.

**S: "Desteklenen NetBIOS adları noktalı"?**  
Hayır, Azure AD Connect, şirket içi orman veya etki alanlarını burada NetBIOS adını içeriyorsa bir nokta (.) desteklemez.

**S: Saf IPv6 ortamı desteklenir?**  
Hayır, Azure AD Connect, saf bir IPv6 ortamına desteklemez.

**Soru: çok ormanlı bir ortamda sahibim ve iki orman arasında ağ NAT (ağ adresi çevirisi) kullanıyor. Azure AD Connect, desteklenen bu iki orman arasında kullanıyor mu?**</br>
 Hayır, Azure AD Connect kullanarak NAT üzerinde desteklenmiyor. 

## <a name="federation"></a>Federasyon
**S: Bana Office 365 Sertifikamı yenilemek için soran bir e-posta alıyorum varsa ne yapmalıyım?**  
Sertifika yenileme hakkında yönergeler için bkz. [sertifikalarını yenilemesi](how-to-connect-fed-o365-certs.md).

**S: "Otomatik olarak bağlı olan taraf Office 365 bağlı olan taraf için ayarlanmış güncelleştir" sahibim. İmzalama sertifikasını otomatik olarak belirtecimi geldiğinde, herhangi bir eylemde bulunmanız gerekir mi?**  
Makalede açıklanan yönergeleri kullanın [sertifikalarını yenilemesi](how-to-connect-fed-o365-certs.md).

## <a name="environment"></a>Ortam
**S: Azure AD Connect yüklendikten sonra sunucuyu yeniden adlandırmak için destekleniyor mu?**  
Hayır. Eşitleme Altyapısı SQL veritabanı örneğine bağlanamıyor oluşturur ve sunucu adını değiştirme ve hizmet başlatılamıyor.

**S: Sonraki nesil şifreleme (NGC) eşitleme kuralları FIPS etkin bir makinede destekleniyor mu?**  
Hayır.  Desteklenmeyen bir durumdur.

## <a name="identity-data"></a>Kimlik verilerini
**S: Neden Azure AD userPrincipalName (UPN) öznitelik, şirket içi UPN eşleşmeyen?**  
Bilgi için şu makalelere bakın:

* [Office 365, Azure veya Intune'daki kullanıcı adları, şirket içi UPN veya alternatif oturum açma kimliği eşleşmiyor.](https://support.microsoft.com/kb/2523192)
* [Farklı bir Federasyon etki alanı kullanmak için bir kullanıcı hesabının UPN'sini değiştirdikten sonra değişiklikler tarafından Azure Active Directory Eşitleme Aracı eşitlenmiş değil](https://support.microsoft.com/kb/2669550)

Azure AD eşitleme altyapısı UPN güncelleştirmek açıklandığı izin vermek için de yapılandırabilirsiniz [Azure AD Connect eşitleme hizmeti özelliklerini](how-to-connect-syncservice-features.md).

**S: Geçici bir şirket içi Azure AD grup veya kişi nesnesini içeren mevcut bir Azure AD grubu eşleştirme veya nesne destekleniyor mu?**  
Evet, bu geçici bir eşleşme proxyAddress üzerinde temel alır. Geçici eşleştirme posta-etkin olmayan gruplar için desteklenmez.

**S: El ile var olan bir Azure AD grubuna İmmutableıd özniteliği ayarlayın ya da sabit, şirket içi Azure AD grubu için eşleştirme veya nesne başvurun nesnesine başvurun destekleniyor mu?**  
Hayır, İmmutableıd özniteliği sabit, eşleştirme için varolan bir Azure AD grup veya kişi nesnesini üzerinde ayarı el ile şu anda desteklenmiyor.

## <a name="custom-configuration"></a>Özel yapılandırma
**S: Azure AD Connect için PowerShell cmdlet'leri burada belirtilmiştir?**  
Bu sitede belgelenen cmdlet'leri hariç olmak üzere Azure AD Connect içinde bulunan diğer PowerShell cmdlet'leri, müşteri kullanım için desteklenmez.

**S: Eşitleme hizmeti yapılandırma sunucuları arasında taşıyor Yöneticisi'nde bulunan "Sunucu dışarı aktarma/sunucu Al" seçeneğini kullanabilir miyim?**  
Hayır. Bu seçenek, tüm yapılandırma ayarlarını almaz ve onu kullanılmamalıdır. Bunun yerine, ikinci sunucusunda temel yapılandırma oluşturmak için Sihirbazı'nı kullanın ve sunucular arasında herhangi bir özel kural taşımak için PowerShell komut dosyaları oluşturmak için eşitleme kuralı Düzenleyicisi'ni kullanın. Daha fazla bilgi için [Swing geçişi](how-to-upgrade-previous-version.md#swing-migration).

**S: Azure oturum açma sayfası için parolaları önbelleğe alınabilir ve bir parola input öğesi içerdiği için bu önbelleğe alma engellenebilir *otomatik tamamlama = "false"* özniteliği?**  
Şu anda HTML özniteliklerini değiştirme **parola** alanı, otomatik tamamlama etiketi de dahil olmak üzere desteklenmez. Herhangi bir öznitelik eklemenize olanak sağlayan özel bir JavaScript olanak sağlayan bir özellik üzerinde çalışıyoruz **parola** alan.

**S: Azure oturum açma sayfasında, daha önce başarıyla oturum açmış kullanıcıların kullanıcı adlarını görüntüler. Bu davranışı devre dışı bırakılabilir mi?**  
Şu anda HTML özniteliklerini değiştirme **parola** otomatik tamamlama etiketi de dahil olmak üzere, giriş alanı desteklenmez. Herhangi bir öznitelik eklemenize olanak sağlayan özel bir JavaScript olanak sağlayan bir özellik üzerinde çalışıyoruz **parola** alan.

**S: Eşzamanlı oturum önlemek için bir yol var mı?**  
Hayır.

## <a name="auto-upgrade"></a>Otomatik yükseltme

**S: Avantajları nelerdir ve yükseltme kullanarak sonuçlarıyla otomatik?**  
Biz, tüm müşterilerin kendi Azure AD Connect yüklemesi için otomatik yükseltmeyi etkinleştir bildiren. Her zaman Azure AD Connect içinde bulunan güvenlik açıkları için güvenlik güncelleştirmeleri de dahil olmak üzere en son düzeltme eklerini alır avantajdır. Yükseltme işlemi sorunsuzdur ve otomatik olarak yeni bir sürümü var. hemen sonra gerçekleşir. Binlerce Azure AD Connect müşteriler otomatik yükseltmeyi her yeni sürümle birlikte kullanın.

Otomatik yükseltme işlemi her zaman ilk yüklemenin otomatik yükseltme için uygun olup olmadığını belirler. Uygun durumda, yükseltme gerçekleştirilen ve test. İşlem, kuralları ve belirli bir çevresel etmenler özel değişiklikler aranıyor de içerir. Testleri yükseltme başarısız olduğunu gösterirse, önceki sürümü otomatik olarak geri yüklenir.

Ortamın boyutuna bağlı olarak, işlem birkaç saat sürebilir. Yükseltme işlemi devam ederken Windows Server Active Directory ve Azure AD arasında hiçbir eşitleme gerçekleşir.

**S: Me, my otomatik yükseltme artık çalışır ve yeni bir sürümünü yüklemeniz gerekir bildiren bir e-posta aldım. Bunu yapmak neden ihtiyacım var?**  
Geçen yıl, belirli koşullar altında sunucunuzda otomatik yükseltme özelliğini devre dışı bırakmış olabilir, Azure AD Connect sürümünü çıkardık. Azure AD Connect sürümü 1.1.750.0 sorunu düzelttik. Bu sorundan etkilendiğini, onarmak için bir PowerShell Betiği çalıştırılarak veya el ile Azure AD Connect'in en son sürüme yükseltme azaltabilirsiniz. 

PowerShell betiğini çalıştırmak için [betiği indirin](https://aka.ms/repairaadconnect) ve uygulamayı Azure AD Connect sunucunuzda yönetici bir PowerShell penceresi. Betik çalıştırma hakkında bilgi edinmek için [bu kısa videoyu izleyin](https://aka.ms/repairaadcau).

El ile yükseltmek için indirin ve AADConnect.msi dosyanın en son sürümünü çalıştırması gerekir.
 
-  Geçerli sürümünüzü 1.1.750.0 eskiyse [indirin ve en son sürüme yükseltme](https://www.microsoft.com/download/details.aspx?id=47594).
- Azure AD Connect sürümünüzü 1.1.750.0 veya daha sonra başka bir eylem gerekli ise. Otomatik yükseltme düzeltme içeren bir sürüm zaten kullanmakta olduğunuz. 

**S: Otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmek için bana bildiren bir e-posta aldım. Sürüm 1.1.654.0 kullanıyorum. Yükseltmem gerekiyor mu?**  
Evet, 1.1.750.0 sürümüne yükseltin veya daha sonra otomatik yükseltmeyi yeniden etkinleştirmek için gerekir. [Karşıdan yükle ve en son sürüme yükseltme](https://www.microsoft.com/download/details.aspx?id=47594).

**S: Otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmek için bana bildiren bir e-posta aldım. Otomatik yükseltmeyi etkinleştirmek için PowerShell kullandığımı olursa hala en son sürümünü yüklemek ihtiyacım var?**  
Evet, yine 1.1.750.0 sürüme yükseltin veya üzeri gerekir. PowerShell ile otomatik yükseltme hizmetini etkinleştirme 1.1.750.0 öncesi sürümlerinde bulunan Otomatik yükseltme sorunu hafifletmeye değil.

**S: Yeni bir sürüme yükseltmek istiyorum ancak Azure AD Connect yüklemiş emin değilim ve kullanıcı adı ve parola sahibiz değil. Bunu yapmamız gerekir?**
Kullanıcı adı ve Azure AD Connect'e yükseltmenin başlangıçta kullanılan parolayı bilmeniz gerekmez. Genel yönetici rolüne sahip herhangi bir Azure AD hesabını kullanın.

**S: Hangi Azure AD Connect sürümünün kullandığımı nasıl öğrenebilirim?**  
Hangi sürümün Azure AD Connect sunucunuzda yüklendiğini doğrulamak için Denetim Masası'na gidin ve Microsoft Azure AD Connect'in yüklü sürümü seçerek görüneceğini **programlar** > **programlar ve Özellikler** , burada gösterildiği gibi:

![Denetim Masası'nda Azure AD Connect sürümü](./media/reference-connect-faq/faq1.png)

**S: Azure AD Connect'in en son sürüme nasıl yükseltebilirim?**  
En son sürüme yükseltme öğrenmek için bkz: [Azure AD Connect: Önceki bir sürümden en son sürüme yükseltme](how-to-upgrade-previous-version.md) makalesine bakın. 

**S: Biz, Azure AD Connect'in en son sürüme geçen yıl zaten yükseltildi. Yeniden yükseltmeye gerekiyor mu?**  
Azure AD Connect takım sık sık güncelleştirme hizmetine yapar. Hata düzeltmeleri ve güvenlik güncelleştirmeleri yeni özellikleri de yararlanmak için sunucunuzun en son sürümüyle güncel tutmak önemlidir. Otomatik yükseltme etkinleştirirseniz, Yazılım sürümünüzün otomatik olarak güncelleştirilir. Azure AD Connect sürüm yayımlama geçmişi bulmak için bkz: [Azure AD Connect: Sürüm yayınlama geçmişi](reference-connect-version-history.md) makalesine bakın.

**S: Ne kadar yükseltmeyi gerçekleştirmek için sürer ve kullanıcılar üzerindeki etkisi nedir?**  
Yükseltme için gereken süre, Kiracı boyutuna bağlıdır. Büyük kuruluşlarda, akşam veya hafta sonu yükseltmeyi gerçekleştirmek en iyi yöntem olabilir. Yükseltme sırasında hiçbir eşitleme etkinliği gerçekleşir.

**S: Ben, ben Azure AD Connect, ancak Office portalı hala bahsetmeleri DirSync yükseltilmiş inanıyoruz. Neden bu mi?**  
Office ekibi, geçerli bir ürün adı yansıtacak şekilde Office portalı güncelleştirmelerini almak için çalışmaktadır. Kullanmakta olduğunuz hangi eşitleme aracı yansıtmaz.

**S: Otomatik yükseltme durum bildiren, "Askıya alındı." Neden şu askıya alındı? Onu etkinleştirmeniz gerekir?**  
Bir hata, belirli koşullar altında otomatik yükseltme durumu "Askıya alındı." olarak ayarlanmış bırakır önceki bir sürümünde kullanıma sunulmuştur El ile etkinleştirme, teknik olarak mümkündür ama karmaşık birkaç adım gerektirir. Yapabileceğiniz en iyi şey, Azure AD Connect'in en son sürümünü yüklemek olabilir.

**S: Şirketim katı değişiklik Yönetimi gereksinimleri vardır ve ne zaman bunu gönderildiğinde denetlemek istiyorum. Otomatik yükseltme ne zaman başlatılır denetleyebilir miyim?**  
Hayır, var. böyle bir özellik Bugün Özellik gelecekteki sürümlerde sunulması değerlendirilir.

**S: Otomatik yükseltme başarısız olursa, bir e-posta alırım? Başarılı olduğunu nasıl bilebilirim?**  
Yükseltmenin sonucu bildirilmez. Özellik gelecekteki sürümlerde sunulması değerlendirilir.

**S: Otomatik yükseltme konusunu göndermek planlama yaparken zaman çizelgesi için yayımlama?**  
Otomatik yükseltme daha yeni bir sürüme sürüm sürecinin ilk adımıdır. Yeni bir sürüm olduğunda, yükseltme otomatik olarak gönderilir. Daha yeni sürümleri, Azure AD Connect, önceden duyurulmuş [Azure AD yol haritası](../fundamentals/whats-new.md).

**S: Otomatik yükseltme, ayrıca Azure AD Connect Health yükseltme mu?**  
Evet, Otomatik yükseltme, Azure AD Connect Health da yükseltir.

**S: Aynı zamanda otomatik hazırlama modunda Azure AD Connect sunucuları yükseltme?**  
Evet, otomatik hazırlama modunda olan bir Azure AD Connect sunucusu yükseltme.

**S: Otomatik yükseltme başarısız olursa ve Azure AD Connect sunucumu başlamıyor ne yapmalıyım?**  
Nadiren de olsa, yükseltme gerçekleştirildikten sonra Azure AD Connect hizmetini başlamıyor. Bu durumlarda, genellikle sunucunun yeniden başlatılması sorunu giderir. Azure AD Connect hizmetini hala başlatılmazsa, bir destek bileti açın. Daha fazla bilgi için [Office 365 desteğe başvurmak için bir hizmet isteği oluşturduğunuzda](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**S: Ben Azure AD Connect'in yeni bir sürüme yükseltme yaptığınızda riskler nelerdir emin değilim. Bana bana yükseltmede size yardımcı olacak çağırabilir miyim?**  
Yardıma ihtiyacınız varsa Azure AD Connect'in yeni bir sürüme yükseltme sırasında bir destek bileti açın [Office 365 desteğe başvurmak için bir hizmet isteği oluşturduğunuzda](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Sorun giderme
**S: Azure AD Connect ile ilgili Yardım nasıl alabilirim?**

[Microsoft Bilgi Bankası (KB) arama](https://www.microsoft.com/en-us/search/result.aspx?q=azure+active+directory+connect)

* KB ortak onarım sorunları için Azure AD Connect desteği hakkında teknik çözümler için arama yapın.

[Azure Active Directory forumları](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Teknik sorular ve yanıtlar için arama yapın veya giderek kendi sorularınızı sorun [Azure AD'ye topluluk](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)
