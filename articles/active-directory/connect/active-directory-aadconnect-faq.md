---
title: 'Azure Active Directory Connect: SSS - | Microsoft Docs'
description: Bu sayfa, Azure AD Connect hakkında sık bir sorulan sorular.
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
ms.date: 05/09/2018
ms.author: billmath
ms.openlocfilehash: 46a9bf47b4998c4d5be47f67556fbdb3ba7b71db
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2018
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Azure Active Directory Connect için sık sorulan sorular

## <a name="general-installation"></a>Genel yükleme
**S: Azure AD genel yönetici etkin 2FA varsa yükleme çalışacak mı?**  
Şubat 2016 yapılar ile bu desteklenir.

**S: Azure AD Connect Katılımsız yüklemenin bir yolu var mı?**  
Yalnızca Azure AD Connect Yükleme Sihirbazı'nı kullanarak yüklemek için desteklenir. Katılımsız ve sessiz bir yükleme desteklenmez.

**S: olduğu bir etki alanına erişilemiyor bir ormana sahibim. Azure AD Connect yükleme nasıl yaparım?**  
Şubat 2016 yapılar ile bu desteklenir.

**S: AD DS Sistem Durumu Aracısı Sunucu Çekirdeği yüklemesinde çalışmıyor?**  
Evet. Aracıyı yükledikten sonra aşağıdaki PowerShell cmdlet'ini kullanarak kayıt işlemini tamamlamak: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**S: AADConnect, iki etki alanlarından üzerinde Azure AD eşitleme destekliyor mu?**</br>
Evet, bu desteklenir. Başvurmak [birden çok etki alanı](active-directory-aadconnect-multiple-domains.md)
 
**S: Azure AD'de aynı Active Directory etki alanı için birden çok bağlayıcı bağlanma sahip destekliyoruz?**</br> Hayır, bu olmayan desteklenmiyor 

## <a name="network"></a>Ağ
**S: sahibim bir güvenlik duvarı, ağ aygıtını veya başka bir şey en uzun süre bağlantıları sınırlayan Ağımdaki açık kalabilir. Ne kadar süreyle my istemci tarafı zaman aşımı eşiği Azure AD Connect kullanırken olmalıdır?**  
Tüm ağ yazılımı, fiziksel aygıtların ya da başka bir şey bağlantıları açık kalabileceği en uzun süre sınırlayan bir eşik en az 5 dakika (300 saniye olarak) Azure Active Directory ve Azure AD Connect istemcisinin yüklü olduğu sunucu arasındaki bağlantıyı için kullanmanız gerekir. Bu durum, tüm daha önce yayımlanmış Microsoft Identity eşitleme araçları için de geçerlidir.

**S: desteklenen SLD'ler (tek etiketli etki alanları) misiniz?**  
Hayır, Azure AD Connect, şirket içi ormanlar/SLD'ler kullanarak etki alanları desteklemez.

**S: desteklenen ayrık AD etki alanları ile ormanları misiniz?**  
Hayır, Azure AD Connect, şirket içi ormanları ayrık ad alanları içeren desteklemez.

**S: "noktalı" adlı NetBIOS desteklenir?**  
Hayır, Azure AD Connect, şirket içi ormanlar/etki, NetBIOS adını içerdiği bir süre desteklemiyor "." adında.

**S: desteklenen saf IPv6 ortam mi?**  
Hayır, Azure AD Connect saf IPv6 ortamında desteklemez.

## <a name="federation"></a>Federasyon
**S: t, bana my Office 365 sertifikayı yenilemek için isteyen bir e-posta almaya devam ederseniz ne yapmalıyım**  
İçinde açıklanan yönergeleri kullanmanız [sertifikaları Yenile](active-directory-aadconnect-o365-certs.md) sertifikayı yenilemek nasıl konu.

**S: "Otomatik olarak bağlı olan taraf O365 bağlı olan taraf için ayarlanmış güncelleştir" sahibim. My belirteç imzalama sertifikası otomatik olarak geldiğinde herhangi bir eylemde bulunmanız gerekir mi?**  
Makalede açıklanan yönergeleri kullanmanız [sertifikaları Yenile](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Ortam
**S: Azure AD Connect yüklendikten sonra sunucuyu yeniden adlandırmak için destekleniyor mu?**  
Hayır. Sunucu adının değiştirilmesi SQL veritabanına bağlanabilmek için olmaması eşitleme altyapısı neden olur ve hizmeti başlatmak mümkün olmaz.

## <a name="identity-data"></a>Kimlik verileri
**S: Azure AD UPN (userPrincipalName) özniteliğinde şirket içi UPN - neden eşleşmiyor?**  
Bu makalelere bakın:

* [Office 365, Azure veya Intune kullanıcı adları şirket içi UPN veya alternatif oturum açma kimliği eşleşmiyor](https://support.microsoft.com/en-us/kb/2523192)
* [Farklı bir Federasyon etki alanını kullanmak için bir kullanıcı hesabının UPN değiştirdikten sonra değişiklikleri Azure Active Directory eşitleme aracı tarafından eşitlenen değil](https://support.microsoft.com/en-us/kb/2669550)

UserPrincipalName açıklandığı şekilde güncelleştirmek eşitleme altyapısı izin vermek için Azure AD de yapılandırabilirsiniz [Azure AD Connect eşitleme hizmeti özelliklerini](active-directory-aadconnectsyncservice-features.md).

**S: olan yazılım eşleşen desteklenen şirket içi AD grup/kişisi nesneleri ile var olan Azure AD Grup/kişi nesneleri?**  
Evet, bu proxyAddress üzerinde temel alır.  Yazılım eşleşen posta-etkin olmayan grupları için desteklenmiyor.

**S: olan desteklenen el ile ayarlamak için sabit kendisine eşleştirmek için var olan Azure AD Grup/kişi nesneleri İmmutableıd özniteliği şirket içi AD Grup/kişi nesneleri?**  
Hayır, bu şu anda desteklenmiyor.

## <a name="custom-configuration"></a>Özel yapılandırma
**S: Burada Azure AD Connect için PowerShell cmdlet'leri belgelenen?**  
Bu sitede belgelenen cmdlet'leri hariç olmak üzere, Azure AD Connect içinde bulunan diğer PowerShell cmdlet'leri müşteri kullanım için desteklenmez.

**S: "sunucu dışa aktarma/sunucu alma bulundu" kullanabilirim *Eşitleme Hizmeti Yöneticisi'ni* yapılandırma sunucuları arasında taşımak için?**  
Hayır. Bu seçenek, tüm yapılandırma ayarlarını almaz ve kullanılmamalıdır. Bunun yerine, temel yapılandırma ikinci sunucusunda oluşturmak ve herhangi bir özel kural sunucular arasında taşımak için PowerShell komut dosyaları oluşturmak için eşitleme kuralı Düzenleyicisi'ni kullanmak için Sihirbazı'nı kullanmanız gerekir. Bkz: [çarpma geçiş](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**S: Azure oturum açma sayfası için parolaları önbelleğe ve bir parola input öğesi otomatik tamamlama ile içerdiğinden bu engellenebilir = "false" özniteliği?**</br>
Şu anda parola giriş alanını otomatik tamamlama etiketi de dahil olmak üzere, HTML özniteliklerini değiştirme desteklemiyoruz. Şu anda herhangi bir öznitelik parola alanına eklemenize olanak tanıyan özel Javascript için izin veren bir özellik üzerinde çalışıyoruz. Bu 2017 kullanılabilir sonraki parçası olmalıdır.

**S: Azure oturum açma sayfasında, daha önce başarıyla oturum açtıktan kullanıcılar için kullanıcı adları gösterilir.  Bu davranış kapalı?**</br>
Şu anda oturum açma sayfasının HTML özniteliklerini değiştirme desteklemiyoruz. Şu anda herhangi bir öznitelik parola alanına eklemenize olanak tanıyan özel Javascript için izin veren bir özellik üzerinde çalışıyoruz. Bu 2017 kullanılabilir sonraki parçası olmalıdır.

**S: eşzamanlı oturum önlemek için bir yol var mı?**</br>
Hayır.

## <a name="auto-upgrade"></a>Otomatik yükseltme

**S: avantajları nelerdir ve yükseltme sonuçlarını kullanarak otomatik?**</br>
Tüm müşterilerin kendi Azure AD Connect yüklemesi için otomatik yükseltmeyi etkinleştirmek için hazır olduğunu bildirmek. Bunlar her zaman içinde Azure AD Connect bulduk güvenlik açıkları için güvenlik güncelleştirmeleri de dahil olmak üzere en son düzeltme eklerini alırsınız yararlar şunlardır. Yükseltme işlemi sorunsuz ve yeni bir sürümü hemen otomatik olarak gerçekleşir. Biz otomatik yükseltme aracılığıyla Azure AD Connect müşteriler binlerce her yeni sürümle hizmet.

Otomatik yükseltme işlemi, her zaman ilk yüklemenin otomatik yükseltme için uygun olup olmadığını kurar (Bu kurallar, özel değişiklikler arayan içerir belirli çevresel etmenler vb.) ve bu nedenle, yükseltme gerçekleştirilen test ve varsa. Testleri yükseltmesi başarılı olmadı gösterirse, önceki sürümü otomatik olarak geri.

Ortam büyüklüğüne işlemi birkaç saat sürebilir ve yükseltme gerçekleşirken, Windows Server AD ve Azure AD arasında hiçbir eşitleme gerçekleşir.

**S: me, my otomatik yükseltme artık çalışır ve yeni sürümünü yüklemeniz gerekir bildiren bir e-posta aldım. Bunu yapmak neden gerekiyor mu?**</br>
Geçen yıl belirli koşullar altında sunucunuzda otomatik yükseltme özelliği devre dışı bırakmış olabilir, Azure AD Connect'in sürümünü yayımladı. Azure AD CONNECT'te geçen ay sonunda yayımlanan sürüm 1.1.750.0, bu sorun düzeltilmiştir. El ile sorunu azaltmak için Azure AD Connect'in en son sürümüne yükseltmek için bu sorundan etkilenmiş müşteriler ihtiyacımız var. El ile yükseltmek için indirin ve AADConnect.msi dosyasının en son sürümünü çalıştırın.
 
-  Geçerli sürümünüzü 1.1.750.0 eski ise, en son sürüme yükseltmelisiniz [hangi indirilebilir burada](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Azure AD Connect sürümünüzü 1.1.750.0 ise ya da daha yeni bir düzeltme bu sürümü zaten girdiğinizi gibi otomatik yükseltme sorunu azaltmak için herhangi bir eylemde bulunmanız gerekmez. 

**S: bana otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmek için bildiren bir e-posta aldım. Üzerinde 1.1.654.0 ben, yükseltme gerekiyor mu?** </br>    
Evet, siz 1.1.750 yükseltin ya da daha yeni Otomatik yükseltme yeniden etkinleştirmeniz gerekir. Daha yeni bir sürüme yükseltmek açıklanmaktadır bağlantı İşte

**S: bana otomatik yükseltmeyi yeniden etkinleştirmek için en son sürüme yükseltmek için bildiren bir e-posta aldım. PowerShell otomatik yükseltme etkinleştirmek için kullandığım, ı hala en son sürümünü yüklemem gerekiyor mu?**</br>    
Evet, yine 1.1.750.0 sürümüne yükseltme ya da daha yeni gerekir. PowerShell ile otomatik yükseltme hizmetini etkinleştirme 1.1.750 önceki sürümler bulunan Otomatik yükseltme sorunu azaltmak değil

**S: daha yeni bir sürüme yükseltmek istiyor ancak Azure AD Connect yüklemiş emin değilim ve biz kullanıcı adı ve parolaya sahip değil.  Bu ihtiyacımız var?**</br>
Kullanıcı adı bilmek zorunda değilsiniz ve başlangıçta Azure AD Connect – genel Yönetici rolüne sahip herhangi bir Azure AD hesabı yükseltmek için kullanılan parola kullanılabilir.

**S: üzerinde ben Azure AD Connect hangi sürümünün nasıl bulabilirim?**</br>   
Azure AD Connect hangi sürümünün sunucunuzda yüklendiğini doğrulamak için Denetim Masası'na gidin ve "Programlar > Programlar ve Özellikler" Microsoft Azure AD Connect yüklü olan sürümü aramak:

![sürüm](media/active-directory-aadconnect-faq/faq1.png)

**S: AADConnect en son sürümüne yükseltme nasıl yaparım?**</br>    
Bu [makale](active-directory-aadconnect-upgrade-previous-version.md) daha yeni bir sürüme yükseltmek açıklanmaktadır. 

**S: biz zaten geçen yıl AADConnect en son sürümüne yükseltme, yükseltme işlemini yeniden ihtiyacımız?**</br> Azure AD Connect takımlar hizmete sık güncelleştirmeler yapar ve sunucunuzu hata düzeltmeleri ve güvenlik güncelleştirmelerinin yanı sıra yeni özelliklerden yararlanmak için en son sürümle güncel olması önemlidir. Otomatik yükseltme etkinleştirirseniz, yazılım sürümü otomatik olarak güncelleştirilir. Azure AD Connect sürüm yayımlama geçmişi bulmak için lütfen bu izleyin [bağlantı](active-directory-aadconnect-version-history.md).

**S: ne kadar yükseltme ve kullanıcılar üzerindeki etkiyi nedir gerçekleştirmek için sürer?**</br>    
Yükseltme için gereken süre, Kiracı boyutuna bağlıdır ve büyük kuruluşlarda Akşam veya hafta sonu Bunun en iyi yöntem olabilir. Yükseltme sırasında herhangi bir eşitleme etkinlik yer alır.

**S: AADConnect için yükseltilmiş ancak Office Portalı'nda, hala DirSync değinmektedir inanıyoruz.  Wny bu mi?**</br>    
Office ekibi geçerli ürün adı – yansıtacak şekilde Office portalı güncelleştirmelerini almak için kullanmakta olduğunuz hangi eşitleme aracı yansıtmaz çalışmaktadır.

**S: Otomatik yükseltme Durumum denetlenir ve "askıya alındı" söyler. Neden askıya alınmış? Onu etkinleştirmeniz gerekir?**</br>     
Bir hata belirli koşullar altında otomatik yükseltme durumu "askıya alındı" olarak ayarlanmış bırakabilir, önceki bir sürümde sunulmuştur. El ile etkinleştirme teknik olarak mümkün olmakla birlikte yapabileceğiniz en iyi şey, Azure AD Connect'in en son sürümünü yükleyin böylece birkaç karmaşık adımlar gerektirir

**S: Şirketim katı değişiklik yönetim gereksinimleri vardır ve ne zaman onu itildiği denetlemek istiyorum. Otomatik yükseltme ne zaman başlatılır denetleyebilir miyim?**</br> Hayır, böyle bir özellik Bugün, gelecekteki bir sürümde değerlendirme bir şey budur.

**S: Otomatik yükseltme başarısız olursa bir e-posta alıyorum? Başarılı olup olmadığını nasıl anlarım?**</br>     
Yükseltme, sonuç konusunda bildirilmez bu gelecekteki bir sürümde değerlendirme bir şeydir.

**Otomatik yükseltmeler göndermeyi planlarken ilişkin bir zaman çizgisi yayımlama Q:Do?**</br>    
Yeni bir sürüm olduğunda otomatik yükseltmeler itme şekilde otomatik yükseltme daha yeni sürüm bizim yayın işlemindeki ilk adımdır. Azure AD Connect daha yeni sürümleri, önceden duyurulmuş [Azure AD yol haritası](../../active-directory/whats-new.md).

**S: Otomatik yükseltme AAD Connect Health yükseltme mu?**</br>   Evet, Otomatik yükseltme AAD Connect Health da yükseltir

**S: de Otomatik yükseltme AAD Connect sunucuları hazırlama modunda musunuz?**</br>   
Hayır, otomatik hazırlama modu olan bir Azure AD Connect sunucusu yükseltme değil.

**S: durumunda otomatik yükseltme başarısız olur ve AAD Connect sunucum başlatılmazsa, ne yapmalıyım?**</br>   
Nadir durumlarda, yükseltme işleminden sonra Azure AD Connect hizmet başlamaz. Bu durumda, lütfen genellikle sorunu giderir sunucunun yeniden başlatın. Azure AD Connect hizmetinin hala başlamazsa, Lütfen bir destek bileti açın. Burada bir [bağlantı](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) bunun nasıl yapılacağını açıklar. 

**S: riskleri Azure AD Connect daha yeni bir sürüme yükseltirken nelerdir emin değilim. Bana bana yükseltmeye yardımcı olmak için çağırabilir miyim?**</br>
Azure AD Connect daha yeni bir sürüme yükseltme Yardım, Lütfen bir destek bileti açmanız gerekiyorsa İşte bir [bağlantı](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) bunun nasıl yapılacağını gösterir.

## <a name="troubleshooting"></a>Sorun giderme
**S: Azure AD Connect ile ilgili Yardım nasıl alabilirim?**

[Microsoft Bilgi Bankası'nda (KB) arama](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Ortak onarım sorunları Azure AD Connect desteği hakkında teknik çözümleri için Microsoft Bilgi Bankası (KB) arayın.

[Microsoft Azure Active Directory forumları](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Arama ve teknik sorular ve yanıtlar topluluktan veya kendi tıklayarak soru sorun göz atmak [burada](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure AD için destek alma](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

* Azure portalı üzerinden destek almak için bu bağlantıyı kullanın.

