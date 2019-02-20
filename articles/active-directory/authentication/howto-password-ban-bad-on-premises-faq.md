---
title: Şirket içi Azure AD parola koruması ile ilgili SSS
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
ms.openlocfilehash: 8b29e4f6e6e877a3d03ded9edd9e282e0960e177
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/19/2019
ms.locfileid: "56414645"
---
# <a name="preview-azure-ad-password-protection-on-premises---frequently-asked-questions"></a>Önizleme: Azure AD parola koruması hakkında sık sorulan sorular şirket içinde

|     |
| --- |
| Azure AD parola koruması, Azure Active Directory genel Önizleme özelliğidir. Önizlemeler hakkında daha fazla bilgi için bkz: [ek kullanım koşulları Microsoft Azure önizlemeleri için](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="general-questions"></a>Genel sorular

**S: Ne zaman Azure AD parola koruması genel kullanılabilirlik (GA) ulaşır?**

Genel kullanım için S1 CY2019 (Mart 2019 sonu önce) planlanmaktadır. Geri bildirim özelliğini - bugüne yayımlamış herkese teşekkür ederiz, değer veriyoruz!

**S: Hangi Kılavuzu kullanıcıların güvenli bir parola seçin nasıl verilmesi gerekir?**

Bu konu Microsoft'un geçerli yönergeler aşağıdaki bağlantıda bulunabilir:

[Microsoft parolası Kılavuzu](https://www.microsoft.com/en-us/research/publication/password-guidance)

**S: Şirket içi Azure AD parola koruması genel olmayan bulutlarda desteklenir mi?**

Hayır - şirket içi Azure AD parola koruması yalnızca genel bulut ortamında desteklenir. Tarih için genel olmayan bulut kullanılabilirlik duyurdu.

**S: Nasıl ben Azure AD parola koruması avantajları bir şirket içi kullanıcılar alt kümesine uygulayabilir miyim?**

Desteklenmiyor. Dağıtılan ve etkinleştirilen sonra Azure AD parola koruması olmayan ayrım - tüm kullanıcılar eşit güvenlik avantajlardan yararlanabilir.

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

Kesin alan kullanımının doğru şekilde sayısı ve Microsoft global yasaklı parola listesi yasaklı parola belirteçlerinde uzunluğu gibi etkenlere bağlıdır ve Kiracı başına özel parola listesine yanı sıra, şifreleme yükü yasaklanmış belirtilemiyor. Bu listeler, içeriğini, gelecekte büyümeye olasıdır. Göz önünde bir makul geçerli özellik en az beş (5) alanın megabayt olarak miktarını etki alanı sysvol paylaşımına gerektiğini udur.

**S: Neden yüklemek veya DC Aracısı yazılımını yükseltmek için bir yeniden başlatma gerekiyor?**

Bu gereksinim, Windows davranışı core tarafından neden olur.

**S: Belirli bir proxy sunucuyu kullanmak için bir DC aracısını yapılandırmak için herhangi bir yolu var mı?**

Hayır. Proxy sunucusu durum bilgisiz olduğundan, hangi belirli bir proxy sunucusu kullanılan önemli olduğunu unutmayın.

**S: Azure AD parola koruması Proxy Hizmeti yan yana Azure AD Connect gibi başka hizmetler ile bir makinede dağıtmak mudur?**

Evet. Azure AD parola koruması Proxy Hizmeti ve Azure AD Connect hiçbir zaman doğrudan birbiriyle çelişen.

**S: Ben öğesine dağıtılması nedeniyle bu özellik, my etki alanı denetleyicilerinde isabet performans endişe olması gerekiyor mu?**

Azure AD parola koruması DC Aracısı hizmeti, mümkün olduğunca etkili olacak şekilde tasarlanmıştır ve dağıtımda zaten yeteri kadar kaynak var bir var olan Active Directory etki alanı denetleyicisi performansını önemli ölçüde etkilememesi gerekir.

Küçük bir kısmını herhangi belirli bir etki alanı denetleyicisinde genel iş yükü çoğu Active Directory dağıtımları parolasını değiştirme işlemleri olduğunu dikkate almak yararlı olacaktır. Örneğin, 10000 kullanıcı hesapları içeren bir Active Directory etki alanı ve 30 gün olarak ayarlanmış bir MaxPasswordAge ilke düşünün. Ortalama olarak, bu etki alanı 10000 30 görürsünüz ~ 333 parola değiştirme işlemleri = her gün işlemleri tek bir etki alanı denetleyicisi için küçük bir sayıdır. Olası bir kötü durum senaryosunda, örneği gönderme varsayalım bu ~ parola değişiklikleri tek bir DC üzerinde yapılan tek bir saatten fazla 333 (Pazartesi sabahı çalışmak için çok sayıda çalışan tüm gelen örneğin, bu senaryo ortaya çıkabilir); Bu durumda bile ~333/60 dakikalarda hala bekliyoruz yeniden önemli bir yük olmayan dakika başına altı parola değişikliklerini =.

Geçerli etki alanı denetleyicileriniz (örneğin CPU, disk alanı, disk g/ç açısından kullanıma maxed), sınırlı performans düzeylerinde zaten çalışmakta olan, ile Bununla birlikte, ek etki alanı denetleyicileri dağıtma ve/veya kullanılabilir disk genişletmek için iyi bir fikir olurdu alanı, Azure AD parola koruması dağıtmadan önce. Ayrıca bkz. Yukarıdaki sysvol disk alanı kullanımı hakkında soru üzerinde.

**S: Azure AD parola koruması yalnızca birkaç DCs etki alanım içinde test istiyorsunuz. Bu kullanıcı parolası değiştirme işlemleri bu belirli DC'leri kullanmaya zorlamak mümkün mü?**

Hayır. Bir kullanıcı parolasını değiştirdiğinde, Active Directory sitesi ve alt ağ atamaları, ortama özgü ağ yapılandırması, vb. gibi faktörlere göre Windows istemci işletim sistemi tarafından kullanılan etki alanı denetleyicisi seçilmedi. Azure AD parola koruması yok etkileyen veya bu faktörlerin denetlemek ve bu nedenle belirtilen kullanıcı parola değişikliği için seçilen hangi etki alanı denetleyicisi olumlu olamaz.

Azure AD parola koruması tüm belirli bir Active Directory sitesindeki etki alanı denetleyicileri dağıtmak için bu kullanıcıyla Bununla birlikte, bu hedefin benzetimi yakın gelen bir yaklaşım olacaktır. Bu yaklaşım, siteye atanan Windows istemcileri için makul bulunabilen kapsamı sağlar ve bu nedenle de kullanıcılar için bu istemcileri günlüğe kaydetme ve kullanıcıların parolalarını değiştirme.

**S: Yalnızca birincil etki alanı denetleyicisi (PDC üzerindeki) Azure AD parola koruması DC aracı hizmetini yüklerseniz, diğer etki alanındaki tüm etki alanı denetleyicileri de korunur?**

Hayır. Verilen PDC olmayan etki alanı denetleyicisine bir kullanıcının parolasını değiştirildiğinde, düz metin parolası hiç (Bu yaygın bir yanlış perception olur) PDC gönderilir. Bunun yerine, belirli bir DC üzerinde yeni bir parola kabul edildikten sonra Bu DC parola çeşitli kimlik doğrulama protokolü belirli karma değerlerini oluşturmak için bu parolayı kullanır ve sonra bu karma dizine devam ederse. Bu güncelleştirilmiş karmaları ardından PDC'ye - kopyalandığı ancak bu işlem Azure AD parola koruması dahil değil. Bazı durumlarda kullanıcı parolalarını yeniden ağ topolojisi ve Active Directory site tasarımı gibi çeşitli faktörlere bağlı olarak, doğrudan PDC'de değiştirilebilir. (Önceki soruya bakın.)

Özetlersek: Azure AD parola koruması DC aracısı hizmetinin PDC'de dağıtımı % 100 güvenlik kapsamı özelliğinin ulaşmak için gereklidir, ancak kendisi tarafından etki alanındaki başka bir DC'ler için Azure AD parola koruması güvenlik avantajları sağlamaz.

**S: System Center Operations Manager (SCOM) Yönetim Paketi için Azure AD parola koruması kullanılabilir mi?**

Hayır.

## <a name="additional-content"></a>Ek içeriği

Aşağıdaki bağlantılar temel Azure AD parola koruması belgeleri parçası değildir ancak yararlı bir özellik hakkında ek bilgi kaynağı olabilir.

[Kimlik koruması Kılavuzu – Bölüm 15'e-posta: Microsoft Azure AD parola koruması uygulayan (şirket içi çok!)](https://blogs.technet.microsoft.com/cloudready/2018/10/14/email-phishing-protection-guide-part-15-implement-the-microsoft-azure-ad-password-protection-service-for-on-premises-too/)

[Azure AD parola koruması ve akıllı kilitleme genel Önizleme aşamasında kullanıma sunuldu!](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-AD-Password-Protection-and-Smart-Lockout-are-now-in-Public/ba-p/245423#M529)

## <a name="next-steps"></a>Sonraki adımlar

Burada bulamadığınız bir şirket içi Azure AD parola koruması sorunuz varsa, bir geri bildirim öğesi altında - teşekkür gönderin!

[Azure AD parola korumasını dağıtma](howto-password-ban-bad-on-premises-deploy.md)
