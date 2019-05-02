---
title: Şirket içinde Azure AD parola koruması SSS - Azure Active Directory
description: Şirket içi Azure AD parola koruması ile ilgili SSS
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3bd117b79c2d103225e8f1f29b63eb6ae341031d
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917654"
---
# <a name="azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Azure AD parola koruması hakkında sık sorulan sorular şirket içinde

## <a name="general-questions"></a>Genel sorular

**S: Hangi Kılavuzu kullanıcıların güvenli bir parola seçin nasıl verilmesi gerekir?**

Bu konu Microsoft'un geçerli yönergeler aşağıdaki bağlantıda bulunabilir:

[Microsoft parolası Kılavuzu](https://www.microsoft.com/en-us/research/publication/password-guidance)

**S: Şirket içi Azure AD parola koruması genel olmayan bulutlarda desteklenir mi?**

Hayır - şirket içi Azure AD parola koruması yalnızca genel bulut ortamında desteklenir. Tarih için genel olmayan bulut kullanılabilirlik duyurdu.

**S: Nasıl ben Azure AD parola koruması avantajları bir şirket içi kullanıcılar alt kümesine uygulayabilir miyim?**

Desteklenmiyor. Dağıtılan ve etkinleştirilen sonra Azure AD parola koruması ayrım yoktur - tüm kullanıcılar eşit güvenlik avantajlardan yararlanabilir.

**S: Parola değiştirme ve bir parola ayarlama (veya Sıfırla) arasındaki fark nedir?**

Parola kullanıcı yeni bir parola eski parola bilgisine sahip oldukları kanıtlama sonra seçtiğinde farklıdır. Örneğin, bir kullanıcı Windows günlüğe kaydeder ve sonra yeni bir parola seçmesi istenir ne olur budur.

Yönetici bir hesap parolası ile yeni bir parola, örneğin Active Directory Kullanıcıları ve Bilgisayarları yönetim aracını kullanarak değiştirir (bazen bir parola sıfırlama olarak adlandırılır) bir parola ayarlama andır. Bu işlem bir yüksek düzeydeki ayrıcalıkla (genellikle etki alanı yöneticisi) gerektirir ve genellikle işlemi yapan kişi eski parola bilgilere sahip değildir. Yardım Masası senaryoları genellikle bunu örneği için bir kullanıcı parolasını unutmuş Yardım. Ayrıca, yeni bir kullanıcı hesabı için bir parola ile ilk kez oluşturulduğunda olayları parola ayarlama görürsünüz.

Parola doğrulama ilkesinin bir parola değişikliği veya yapıldığını bağımsız olarak aynı şekilde davranır. Azure AD parola koruması DC Aracı hizmeti bir parola değişikliği olmadığını bildirmek için farklı olayları günlüğe kaydetmez veya set işlemi yapılmadı.  Bkz: [izleme ve günlüğe kaydetme Azure AD parola koruması](https://docs.microsoft.com/azure/active-directory/authentication/howto-password-ban-bad-on-premises-monitor).

**S: Azure AD parola koruması parola-filtre tabanlı diğer ürünleri ile yan yana yüklenecek destekleniyor mu?**

Evet. Birden çok kayıtlı parola filtresi DLL'leri çekirdek Windows özelliği için destek ve Azure AD parola koruması özgüdür. Bir parola kabul edilmeden önce tüm kayıtlı parola filtresi DLL'leri kabul etmesi gerekir.

**S: Nasıl dağıtmak ve Azure'ı kullanmadan my Active Directory ortamında Azure AD parola koruması yapılandırma?**

Desteklenmiyor. Azure AD parola koruması bir şirket içi Active Directory ortamına Genişletilmekte olan destekleyen bir Azure özelliğidir.

**S: Active Directory düzeyinde bir ilke içeriğini nasıl değişiklik yapabilirsiniz?**

Desteklenmiyor. İlke, yalnızca Azure AD Yönetim Portalı kullanılarak yönetilebilir. Ayrıca önceki soruya bakın.

**S: DFSR, sysvol çoğaltma için neden gereklidir?**

FRS (DFSR için öncül teknolojisi) birçok bilinen sorunlar ve Windows Server Active Directory daha yeni sürümlerinde tamamen desteklenmez. Azure AD parola koruması sıfır test etki alanlarında FRS yapılandırılmış gerçekleştirilir.

Daha fazla bilgi için lütfen aşağıdaki makalelere bakın:

[Durum taşıma sysvol çoğaltması DFSR için](https://blogs.technet.microsoft.com/askds/2010/04/22/the-case-for-migrating-sysvol-to-dfsr)

[Sonu Nigh için FRS](https://blogs.technet.microsoft.com/filecab/2014/06/25/the-end-is-nigh-for-frs)

**S: Etki alanı sysvol paylaşımı özellik ne kadar disk alanı gerektiriyor?**

Sayısı ve Microsoft global Engellenenler listesi ve Kiracı başına özel liste yanı sıra şifreleme yükü yasaklı belirteçlerin uzunluğu gibi etkenlere bağlı olduğundan, kesin alan kullanımının değişir. Bu listeler, içeriğini, gelecekte büyümeye olasıdır. İle unutmayın, makul bir olasılığın özellik en az beş (5) alanın megabayt olarak miktarını etki alanı sysvol paylaşımına gerektiğini olmasıdır.

**S: Neden yüklemek veya DC Aracısı yazılımını yükseltmek için bir yeniden başlatma gerekiyor?**

Bu gereksinim, Windows davranışı core tarafından neden olur.

**S: Belirli bir proxy sunucuyu kullanmak için bir DC aracısını yapılandırmak için herhangi bir yolu var mı?**

Hayır. Proxy sunucusu durum bilgisiz olduğundan, hangi belirli bir proxy sunucusu kullanılan önemli değildir.

**S: Diğer hizmetler gibi Azure AD Connect ile yan yana Azure AD parola koruması Proxy Hizmeti dağıtmak mudur?**

Evet. Azure AD parola koruması Proxy Hizmeti ve Azure AD Connect hiçbir zaman doğrudan birbiriyle çelişen.

**S: Hangi sırayla proxy'leri ve DC aracıları yüklü ve kayıtlı?**

Bir Proxy Aracısı yüklemesi, DC aracı yüklemesi, orman kayıt ve kayıt Proxy sıralama desteklenir.

**S: Ben öğesine dağıtılması nedeniyle bu özellik, my etki alanı denetleyicilerinde isabet performans endişe olması gerekiyor mu?**

Azure AD parola koruması DC Aracı hizmeti sağlıklı bir Active Directory dağıtımda mevcut etki alanı denetleyicisi performansını önemli ölçüde etkisi olmaması gerekir.

Çoğu Active Directory dağıtımları parola değişikliği küçük bir kısmını herhangi belirli bir etki alanı denetleyicisinde genel iş yükü işlemlerdir. Örneğin, 10000 kullanıcı hesapları içeren bir Active Directory etki alanı ve 30 gün olarak ayarlanmış bir MaxPasswordAge ilke düşünün. Ortalama olarak, bu etki alanı 10000 30 görürsünüz ~ 333 parola değiştirme işlemleri = her gün işlemleri tek bir etki alanı denetleyicisi için küçük bir sayıdır. Olası bir kötü durum senaryosunda göz önünde bulundurun: Bu ~ parola değişiklikleri tek bir DC üzerinde yapılan tek bir saatten fazla 333 varsayalım. Örneğin, çok sayıda çalışan tüm Pazartesi sabahı çalışmaya geldiğinde bu senaryo ortaya çıkabilir. Hatta durumda hala ~333/60 dakikalarda bekliyoruz, yeniden önemli bir yük olmayan dakika başına altı parola değişikliklerini =.

Geçerli etki alanı denetleyicilerinizin zaten performans sınırlı düzeyde (örneğin, CPU, disk alanı, disk g/ç vb. göre kullanıma maxed) çalıştırıyorsanız, ancak bu ek etki alanı denetleyicilerini ekleyin veya önce kullanılabilir disk alanı genişletmek için önerilir Bu özellik dağıtılıyor. Ayrıca bkz. Yukarıdaki sysvol disk alanı kullanımı hakkında soru üzerinde.

**S: Azure AD parola koruması yalnızca birkaç DCs etki alanım içinde test istiyorsunuz. Bu kullanıcı parolası değiştirme işlemleri bu belirli DC'leri kullanmaya zorlamak mümkün mü?**

Hayır. Windows istemci işletim sistemi, bir kullanıcı parolasını değiştirene hangi etki alanı denetleyicisi kullanıldığında denetler. Etki alanı denetleyicisi, Active Directory sitesi ve alt ağ atamaları, ortama özgü ağ yapılandırması, vb. gibi faktörlere göre seçilir. Azure AD parola koruması faktörlerin denetlemez ve hangi etki alanı denetleyicisine bir kullanıcının parolasını değiştirmek için seçili etkiler.

Azure AD parola koruması tüm belirli bir Active Directory sitesindeki etki alanı denetleyicileri dağıtma kısmen bu hedefe ulaşmak için bir yol olabilir. Bu yaklaşım, o siteye atanmış olan Windows istemcileri için makul kapsamı sağlar ve bu nedenle de kullanıcılar için bu istemcileri günlüğe kaydetme ve kullanıcıların parolalarını değiştirme.

**S: Yalnızca birincil etki alanı denetleyicisi (PDC üzerindeki) Azure AD parola koruması DC aracı hizmetini yüklerseniz, diğer etki alanındaki tüm etki alanı denetleyicileri de korunur?**

Hayır. Verilen PDC olmayan etki alanı denetleyicisine bir kullanıcının parolasını değiştirildiğinde, düz metin parolası hiç (Bu yaygın bir yanlış perception olur) PDC gönderilir. Belirli bir DC üzerinde yeni bir parola kabul edildikten sonra Bu DC parola çeşitli kimlik doğrulama protokolü belirli karma değerlerini oluşturmak için bu parolayı kullanır ve sonra bu karma dizine devam ederse. Düz metin parolayı kalıcı olmaz. Güncelleştirilmiş karmalar, ardından PDC'ye çoğaltılır. Bazı durumlarda kullanıcı parolalarını yeniden ağ topolojisi ve Active Directory site tasarımı gibi çeşitli faktörlere bağlı olarak, doğrudan PDC'de değiştirilebilir. (Önceki soruya bakın.)

Özet olarak, etki alanları arasında % 100 güvenlik kapsamı özelliğinin ulaşmak için PDC üzerindeki Azure AD parola koruması DC aracı hizmetinin dağıtılması gerekir. PDC'nin özelliği dağıtmayı yalnızca Azure AD parola koruması güvenlik avantajları etki alanındaki başka bir DC'ler için sağlamaz.

**S: System Center Operations Manager Yönetim Paketi için Azure AD parola koruması kullanılabilir mi?**

Hayır.

**S: Neden Denetim modunda olacak şekilde bir ilke yapılandırmış olduğunuz olsa da Azure hala zayıf parolalarda reddediyor?**

Denetleme modunda, yalnızca şirket içi Active Directory ortamında desteklenir. Parolaları değerlendirirken örtük olarak her zaman "Uygula" modunda azure'dur.

## <a name="additional-content"></a>Ek içeriği

Aşağıdaki bağlantılar temel Azure AD parola koruması belgeleri parçası değildir ancak yararlı bir özellik hakkında ek bilgi kaynağı olabilir.

[Azure AD parola koruması genel kullanıma sunulmuştur!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-is-now-generally-available/ba-p/377487)

[Kimlik koruması Kılavuzu – Bölüm 15'e-posta: Microsoft Azure AD parola koruması uygulayan (şirket içi çok!)](https://blogs.technet.microsoft.com/cloudready/2018/10/14/email-phishing-protection-guide-part-15-implement-the-microsoft-azure-ad-password-protection-service-for-on-premises-too/)

[Azure AD parola koruması ve akıllı kilitleme genel Önizleme aşamasında kullanıma sunuldu!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-and-Smart-Lockout-are-now-in-Public/ba-p/245423#M529)

## <a name="microsoft-premierunified-support-training-available"></a>Microsoft Premier\Unified destek eğitimi

Azure AD parola koruması hakkında daha fazla öğrenme ve ortamınıza dağıtmak istiyorsanız, proaktif bir Microsoft hizmetinin kullanılabilir Premier veya birleştirilmiş bir destek sözleşmesi ile bu müşterilere yararlanabilirsiniz. Hizmet, Azure Active Directory çağrılır: Parola koruması. Daha fazla bilgi için teknik hesap yöneticinize başvurun.

## <a name="next-steps"></a>Sonraki adımlar

Burada bulamadığınız bir şirket içi Azure AD parola koruması sorunuz varsa, bir geri bildirim öğesi altında - teşekkür gönderin!

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
