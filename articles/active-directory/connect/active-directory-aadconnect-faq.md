---
title: Azure Active Directory Connect ile ilgili SSS - | Microsoft Docs
description: Bu makalede, Azure AD Connect hakkında sık sorulan sorular yanıtlanmaktadır.
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
ms.date: 06/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 7edabc99da5e1466e848336c647a33213c9edd8b
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37133109"
---
# <a name="azure-active-directory-connect-faq"></a>Azure Active Directory Connect SSS

## <a name="general-installation"></a>Genel yükleme
**S: Azure Active Directory (Azure AD) genel yönetici etkin iki faktörlü kimlik doğrulamasını (2FA) varsa yükleme çalışacak mı?**  
Şubat 2016 derlemeleri sürümünden itibaren bu senaryo desteklenmiyor.

**S: Azure AD Connect Katılımsız yüklemenin bir yolu var mı?**  
Azure AD Connect yüklemesi, Yükleme Sihirbazı'nı kullandığınızda desteklenir. Sessiz, katılımsız bir yükleme desteklenmez.

**S: olduğu bir etki alanına erişilemiyor bir ormana sahibim. Azure AD Connect yükleme nasıl yaparım?**  
Şubat 2016 derlemeleri sürümünden itibaren bu senaryo desteklenmiyor.

**S: Azure Active Directory etki alanı (Azure AD DS) Sistem Durumu Aracısı sunucu Çekirdeğinde çalışma Hizmetleri mu?**  
Evet. Aracı yükledikten sonra aşağıdaki PowerShell cmdlet'ini kullanarak kayıt işlemini tamamlamak: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**S: Azure AD Connect eşitleme üzerinde Azure AD desteği için iki etki alanlarından mu?**  
Evet, bu senaryo desteklenmiyor. Başvurmak [birden çok etki alanı](active-directory-aadconnect-multiple-domains.md).
 
**S: Azure AD Connect aynı Active Directory etki alanı için birden çok bağlayıcı sahip olabilir?**  
Hayır, birden çok bağlayıcı aynı AD etki alanı için desteklenmez. 

**S: ı Azure AD Connect veritabanını yerel veritabanından uzak bir SQL Server örneğine taşıyabilirsiniz?**   
Evet, aşağıdaki adımlar bunu nasıl genel rehberlik sağlar. Şu anda daha ayrıntılı bir belge üzerinde çalışıyoruz.
1. Yerel veritabanı ADSync veritabanını yedekleyin.
Bunu yapmanın en kolay yolu, Azure AD Connect ile aynı makinede yüklü SQL Server Management Studio kullanmaktır. Bağlanmak *(localdb)\.\ADSync*ve ardından ADSync veritabanını yedekleyin.

2. ADSync veritabanı, uzak SQL Server örneğine geri yükleyin.

3. Varolan karşı Azure AD Connect'i yüklemek [uzak SQL veritabanı](active-directory-aadconnect-existing-database.md).
   Makale, yerel bir SQL veritabanı kullanmaya geçiş gösterilmiştir. 5. adımda işlemin uzak bir SQL veritabanı için kullanılacak geçiriyorsanız, Windows eşitleme hizmeti çalışacağı mevcut bir hizmet hesabını da girmelisiniz. Bu eşitleme altyapısı hizmet hesabı burada açıklanmıştır:
   
      **Varolan hizmet hesabını kullan**: kullanmak için varsayılan olarak, Azure AD Connect Eşitleme hizmetlerini sanal hizmet hesabı kullanır. Uzak SQL Server örneğini kullanmak veya kimlik doğrulama gerektiren bir proxy kullanıyorsanız, bir yönetilen hizmet hesabı veya bir hizmet hesabı etki alanınızı kullanmak ve parolayı biliyor. Bu gibi durumlarda kullanılacak olan hesabı girin. Hizmet hesabı için oturum açma kimlik bilgileri oluşturulan yüklemesini çalıştıran kullanıcılar sistem yöneticileri SQL olduğundan emin olun. Daha fazla bilgi için bkz: [Azure AD Connect hesapları ve izinleri](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). 
   
      En son sürümle, veritabanını sağlama, artık SQL yöneticisi tarafından bant dışında gerçekleştirilebilir ve ardından veritabanı sahibi haklarıyla Azure AD Connect yöneticisi tarafından yüklenebilir. Daha fazla bilgi için bkz: [temsilci SQL yönetici izinleri kullanarak Azure AD Connect'i yükleme](active-directory-aadconnect-sql-delegation.md).

Örneği basit tutmak için Azure AD Connect'i yükleyen kullanıcılar sistem yöneticileri SQL olmasını öneririz. Kullanım SQL yönetici temsilcileri artık ancak ile son yapıları açıklandığı gibi yapabilecekleriniz [temsilci SQL yönetici izinleri kullanarak Azure AD Connect'i yükleme](active-directory-aadconnect-sql-delegation.md).

## <a name="network"></a>Ağ
**S: bir güvenlik duvarı, ağ aygıtı veya bağlantıları Ağımdaki açık kalabileceği süreyi sınırlar şey sahibim. Azure AD Connect kullandığınızda ne my istemci-tarafı zaman aşımı eşiği olmalıdır?**  
Tüm ağ yazılımı, fiziksel aygıtların ya da başka bir şey bağlantıları açık kalabileceği en uzun süreyi sınırlayan bir eşik en az beş dakika (300 saniye olarak) Azure AD Connect istemcisinin yüklü olduğu sunucu arasındaki bağlantıyı için kullanmanız gerekir ve Azure Active Directory. Bu öneri için önceden yayımlanan tüm Microsoft Identity eşitleme araçları da geçerlidir.

**S: desteklenen tek etiketli etki alanları (SLD'ler) misiniz?**  
Hayır, Azure AD Connect, şirket içi ormanları veya SLD'ler kullanan etki alanları desteklemez.

**S: desteklenen ayrık AD etki alanları ile ormanları misiniz?**  
Hayır, Azure AD Connect, ayrık ad alanları içeren şirket içi ormanlardaki desteklemez.

**S: "desteklenen NetBIOS adları noktalı"?**  
Hayır, Azure AD Connect, şirket içi ormanları veya etki alanları, NetBIOS adını içerdiği bir nokta (.) desteklemez.

**S: desteklenen saf IPv6 ortam mi?**  
Hayır, Azure AD Connect saf bir IPv6 ortamına desteklemez.

## <a name="federation"></a>Federasyon
**S: bana my Office 365 sertifikayı yenilemek için soran bir e-posta almaya devam ederseniz ne yapmalıyım?**  
Sertifika yenileme hakkında yönergeler için bkz [sertifikaları Yenile](active-directory-aadconnect-o365-certs.md).

**S: "Otomatik olarak bağlı olan taraf Office 365 bağlı olan taraf için ayarlanmış güncelleştir" sahibim. My belirteç imzalama sertifikası otomatik olarak geldiğinde herhangi bir eylemde bulunmanız gerekir mi?**  
Makalede açıklanan yönergeleri kullanmanız [sertifikaları Yenile](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Ortam
**S: Azure AD Connect yüklendikten sonra sunucuyu yeniden adlandırmak için destekleniyor mu?**  
Hayır. Sunucu adının değiştirilmesi bir SQL veritabanı örneğine bağlanamıyor eşitleme altyapısı işler ve hizmet başlatılamıyor.

## <a name="identity-data"></a>Kimlik verileri
**Neden Azure AD userPrincipalName (UPN) özniteliğinde şirket içi UPN eşleşmiyor?**  
Bilgi için bu makalelere bakın:

* [Office 365, Azure veya Intune kullanıcı adları şirket içi UPN veya alternatif oturum açma kimliği eşleşmiyor](https://support.microsoft.com/en-us/kb/2523192)
* [Farklı bir Federasyon etki alanını kullanmak için bir kullanıcı hesabının UPN değiştirdikten sonra değişiklikleri Azure Active Directory eşitleme aracı tarafından eşitlenen değil](https://support.microsoft.com/en-us/kb/2669550)

UPN güncelleştirmek eşitleme altyapısı açıklandığı gibi izin vermek için Azure AD de yapılandırabilirsiniz [Azure AD Connect eşitleme hizmeti özelliklerini](active-directory-aadconnectsyncservice-features.md).

**S: geçici bir şirket içi Azure AD grup veya ilgili kişi nesnesi içeren mevcut bir Azure AD grubu eşleştirmesi veya nesne iletişim kurmak için destekleniyor mu?**  
Evet, bu yazılım eşleşme proxyAddress üzerinde temel alır. Yazılım eşleşen posta-etkin olmayan grupları için desteklenmiyor.

**S: el ile İmmutableıd özniteliği var olan bir Azure AD grubuna ayarlayın veya sabit onu bir şirket içi Azure AD grubuna eşleştirme veya nesne iletişim kurmak için nesne iletişim kurmak için destekleniyor mu?**  
Hayır, sabit, eşleştirme için varolan bir Azure AD grup veya ilgili kişi nesnesi üzerinde İmmutableıd özniteliği el ile ayarlama şu anda desteklenmiyor.

## <a name="custom-configuration"></a>Özel yapılandırma
**S: Burada Azure AD Connect için PowerShell cmdlet'leri belgelenen?**  
Bu sitede belgelenen cmdlet'leri hariç olmak üzere, Azure AD Connect içinde bulunan diğer PowerShell cmdlet'leri müşteri kullanım için desteklenmez.

**S: eşitleme hizmeti yapılandırma sunucuları arasında taşımak için Yöneticisi'nde bulunan "Sunucu dışa aktarma/sunucu içe aktarma" seçeneğini kullanmak?**  
Hayır. Bu seçenek, tüm yapılandırma ayarlarını almaz ve onu kullanılmamalıdır. Bunun yerine, temel yapılandırma ikinci sunucusunda oluşturmak için Sihirbazı'nı kullanın ve herhangi bir özel kural sunucular arasında taşımak için PowerShell komut dosyaları oluşturmak için eşitleme kuralı Düzenleyicisi'ni kullanın. Daha fazla bilgi için bkz: [çarpma geçiş](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**S: Azure oturum açma sayfası için parolaları önbelleğe ve bir parola input öğesi ile içerdiğinden bu önbelleğe alma engellenebilir *otomatik tamamlama = "false"* özniteliği?**  
Şu anda, HTML özniteliklerini değiştirme **parola** otomatik tamamlama etiketi de dahil olmak üzere alan desteklenmiyor. Herhangi bir öznitelik eklemenize olanak sağlayan özel bir JavaScript izin veren bir özellik üzerinde çalışıyoruz **parola** alan.

**S: Azure oturum açma sayfasında, daha önce başarıyla oturum açtıktan kullanıcıların kullanıcı adlarını görüntüler. Bu davranış kapalı?**  
Şu anda, HTML özniteliklerini değiştirme **parola** otomatik tamamlama etiketi de dahil olmak üzere giriş alanını desteklenmiyor. Herhangi bir öznitelik eklemenize olanak sağlayan özel bir JavaScript izin veren bir özellik üzerinde çalışıyoruz **parola** alan.

**S: eşzamanlı oturum önlemek için bir yol var mı?**  
Hayır.

## <a name="auto-upgrade"></a>Otomatik yükseltme

**S: avantajları nelerdir ve yükseltme sonuçlarını kullanarak otomatik?**  
Biz, tüm müşterilerin kendi Azure AD Connect yüklemesi için otomatik yükseltmeyi etkinleştirmek için bildiren. Her zaman Azure AD Connect içinde bulunan güvenlik açıkları için güvenlik güncelleştirmeleri de dahil olmak üzere en son düzeltme eklerini aldığınız avantajdır. Yükseltme işlemi sorunsuz ve yeni bir sürümü hemen otomatik olarak gerçekleşir. Azure AD Connect müşteriler binlerce otomatik yükseltme her yeni sürümde kullanın.

Otomatik yükseltme işlemi her zaman ilk yüklemenin otomatik yükseltme için uygun olup olmadığını belirler. Uygunsa, yükseltme gerçekleştirilen ve test. İşlem kuralları ve belirli çevresel etmenler özel değişiklikler arayan de içerir. Testleri bir yükseltme başarısız olduğunu gösterirse, önceki sürümü otomatik olarak geri yüklenir.

Ortam boyutuna bağlı olarak, işlem birkaç saat sürebilir. Yükseltme işlemi devam ederken, Windows Server Active Directory ve Azure AD arasındaki eşitleme yok olur.

**S: me, my otomatik yükseltme artık çalışır ve yeni bir sürümünü yüklemeniz gerekir bildiren bir e-posta aldım. Bunu yapmak neden gerekiyor mu?**  
Geçen yıl belirli koşullar altında sunucunuzda otomatik yükseltme özelliği devre dışı bırakmış olabilir, Azure AD Connect'in sürümünü yayımladı. Azure AD Connect sürüm 1.1.750.0 biz sorunu düzeltildi. Bu sorundan etkilenmiş, etkisini onarmak için bir PowerShell betiğini çalıştırarak veya el ile Azure AD Connect'in en son sürümüne yükseltme. 

PowerShell betiğini çalıştırmak için [komut dosyasını karşıdan](https://aka.ms/repairaadconnect) çalıştırmak Azure AD Connect sunucunuzda yönetici bir PowerShell penceresi. Komut dosyasını çalıştırmak öğrenmek için [bu kısa video görüntülemek](https://aka.ms/repairaadcau).

El ile yükseltmek için indirin ve AADConnect.msi dosyasının en son sürümünü çalıştırın.
 
-  Geçerli sürümünüzü 1.1.750.0 eski ise [karşıdan yükle ve en son sürüme yükseltme](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Azure AD Connect sürümünüzü 1.1.750.0 ya daha sonra başka bir eylem gerekli ise. Otomatik yükseltme düzeltme içeren sürüm zaten kullanmakta olduğunuz. 

**S: bana otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmek için bildiren bir e-posta aldım. Sürüm 1.1.654.0 kullanıyorum. Yükseltme gerekiyor mu?**  
Evet, 1.1.750.0 sürümüne yükseltmek için veya daha sonra otomatik yükseltmeyi yeniden etkinleştirmek için gerekir. [Karşıdan yükle ve en son sürüme yükseltme](https://www.microsoft.com/en-us/download/details.aspx?id=47594).

**S: bana otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmek için bildiren bir e-posta aldım. I PowerShell otomatik yükseltmeyi etkinleştirmek için kullandıysanız, hala en son sürümünü yüklemek yapmalıyım?**  
Evet, yine 1.1.750.0 sürümüne yükseltme veya üstü gerekir. PowerShell ile otomatik yükseltme hizmetini etkinleştirme 1.1.750.0 önceki sürümler bulunan Otomatik yükseltme sorunu etkisini azaltmaz.

**S: daha yeni bir sürüme yükseltmek istiyor ancak Azure AD Connect yüklemiş emin değilim ve biz kullanıcı adı ve parolaya sahip değil. Bu ihtiyacımız var?**
Kullanıcı adı ve başlangıçta Azure AD Connect yükseltmek için kullanılan parolayı bilmeniz gerek yoktur. Genel yönetici rolüne sahip herhangi bir Azure AD hesabı kullanın.

**S: Azure AD Connect hangi sürümünün kullanıyorum nasıl bulabilirim?**  
Azure AD Connect hangi sürümünün sunucunuzda yüklendiğini doğrulamak için Denetim Masası'na gidin ve Microsoft Azure AD Connect yüklü sürüm seçerek arayın **programları** > **programlar ve Özellikler** , aşağıda gösterildiği gibi:

![Denetim Masası'nda Azure AD Connect sürümü](media/active-directory-aadconnect-faq/faq1.png)

**S: nasıl Azure AD Connect'in en son sürüme yükseltme?**  
En son sürüme yükseltmek öğrenmek için bkz: [Azure AD Connect: önceki bir sürümünden yükseltme en son](active-directory-aadconnect-upgrade-previous-version.md). 

**S: biz zaten Azure AD Connect'in en son sürüme geçen yıl yükseltildi. Yükseltme işlemini yeniden gerekiyor mu?**  
Azure AD Connect takım hizmete sık güncelleştirmeler sağlar. Hata düzeltmeleri ve güvenlik güncelleştirmelerinin yanı sıra yeni özelliklerden yararlanmak için sunucunuzu en son sürümüyle güncel tutmak önemlidir. Otomatik yükseltme etkinleştirirseniz, yazılım sürümünüz otomatik olarak güncelleştirilir. Azure AD Connect sürüm yayımlama geçmişi bulmak için bkz: [Azure AD Connect: sürüm yayımlama geçmişi](active-directory-aadconnect-version-history.md).

**S: ne kadar yükseltmeyi gerçekleştirmek için sürer ve kullanıcılar üzerindeki etkiyi nedir?**  
Yükseltme için gereken süre, Kiracı boyutuna bağlıdır. Büyük kuruluşlar için Akşam veya hafta sonu yükseltmeyi gerçekleştirmek en iyi yöntem olabilir. Yükseltme sırasında eşitleme etkinliği gerçekleşir.

**S: Azure AD Connect, ancak Office portalı için yükseltilmiş ı düşünüyorsanız hala DirSync tanımlamıştır. Neden bu mi?**  
Geçerli ürün yansıtacak şekilde Office portalı güncelleştirmelerini almak için Office ekibi çalışıyor. Kullanmakta olduğunuz hangi eşitleme aracı yansıtmaz.

**S: Otomatik yükseltme Durumum diyor, "Askıya alındı." Neden askıya alınmış? Onu etkinleştirmeniz gerekir?**  
Bir hata belirli koşullar altında otomatik yükseltme durumu "Askıya alındı olarak." olarak ayarlanmış bırakır, önceki bir sürümünde tanıtılan El ile etkinleştirme teknik olarak mümkün olmakla birlikte birkaç karmaşık adımlar gerektirir. Yapabileceğiniz en iyi Azure AD Connect'in en son sürümünü yüklemek şeydir.

**S: Şirketim katı değişiklik Yönetimi gereksinimleri vardır ve ne zaman, gönderilen denetlemek istediğiniz. Otomatik yükseltme ne zaman başlatılır denetleyebilir miyim?**  
Hayır, var. böyle bir özellik Bugün Bu özellik sonraki bir sürüm için değerlendirilir.

**S: Otomatik yükseltme başarısız olursa bir e-posta alıyorum? Başarılı olup olmadığını nasıl anlarım?**  
Yükseltme sonucunu bildirilmez. Bu özellik sonraki bir sürüm için değerlendirilir.

**S: otomatik yükseltmeler göndermek planlama yaparken için bir zaman çizelgesi yayımlama?**  
Otomatik yükseltme daha yeni bir sürüm yayın sürecinin ilk adımdır. Yükseltme, yeni bir sürüm olduğunda otomatik olarak atılır. Azure AD Connect daha yeni sürümleri, önceden duyurulmuş [Azure AD yol haritası](../fundamentals/whats-new.md).

**S: Otomatik yükseltme ayrıca Azure AD Connect Health yükseltme mu?**  
Evet, Otomatik yükseltme da Azure AD Connect Health yükseltir.

**S:, aynı zamanda otomatik hazırlama modunda Azure AD Connect sunucuları yükseltme?**  
Evet, siz otomatik hazırlama modu olan bir Azure AD Connect sunucusu yükseltme.

**S: Otomatik yükseltme başarısız olur ve Azure AD Connect sunucum başlamıyor, ne yapmalıyım?**  
Nadir durumlarda yükseltme yaptıktan sonra Azure AD Connect hizmet başlamaz. Bu durumlarda, genellikle sunucunun yeniden başlatılmasını sorunu giderir. Azure AD Connect hizmetinin hala başlamazsa, destek bileti açın. Daha fazla bilgi için bkz: [Office 365 destek hizmetlerine bir hizmet isteği oluştur](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**S: Azure AD Connect daha yeni bir sürüme yükselttiğinizde riskleri nelerdir emin değilim. Bana bana yükseltmeye yardımcı olmak için çağırabilir miyim?**  
Yardıma ihtiyacınız varsa Azure AD Connect daha yeni bir sürüme yükseltme, adresindeki destek bilet [Office 365 destek hizmetlerine bir hizmet isteği oluştur](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Sorun giderme
**S: Azure AD Connect ile ilgili Yardım nasıl alabilirim?**

[Microsoft Bilgi Bankası'nda (KB) arama](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* KB sık karşılaşılan onarım sorunlara Azure AD Connect desteği hakkında teknik çözümler için arama yapın.

[Azure Active Directory forumları](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Teknik sorular ve yanıtlar için arama veya giderek kendi sorular sormak [Azure AD topluluğu](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)
