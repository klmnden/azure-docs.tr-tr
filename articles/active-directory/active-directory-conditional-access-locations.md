---
title: Azure Active Directory koşullu erişim konumu koşullar | Microsoft Docs
description: Bir kullanıcının ağ konumuna dayalı bulut uygulamalarınıza erişimi denetlemek için konum koşul kullanmayı öğrenin.
services: active-directory
keywords: uygulamaları, Azure AD ile koşullu erişim, koşullu erişim ilkeleri, şirket kaynaklarına güvenli erişim için koşullu erişim
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 8c1d978f-e80b-420e-853a-8bbddc4bcdad
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/17/2018
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: 842fe8c194f1c88c7dabb073e0fa7b7806d92d44
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="location-conditions-in-azure-active-directory-conditional-access"></a>Azure Active Directory koşullu erişim konumu koşullar 

İle [Azure Active Directory (Azure AD) koşullu erişim](active-directory-conditional-access-azure-portal.md), nasıl yetkili kullanıcılar denetleyebilir, bulut uygulamalarınızı erişebilirsiniz. Koşullu erişim ilkesinin konumu koşulunu, kullanıcılarınızın ağ konumlarına erişim denetimleri ayarlarını bağlamanın sağlar.

Bu makalede konumu koşulu yapılandırmak gereken bilgiler sağlar. 

## <a name="locations"></a>Konumlar

Azure AD çoklu oturum açma cihazları, uygulamaları etkinleştirir ve hizmetlere her yerden genel internet'te. Konum koşuluyla, bir kullanıcının ağ konumuna dayalı bulut uygulamalarınıza erişimi denetleyebilirsiniz. Konum koşul için ortak kullanım durumları verilmiştir:

- Şirket ağı devre dışı olduklarında hizmet erişen kullanıcılar için çok faktörlü kimlik doğrulama gerektirme  

- Bir hizmeti belirli ülke veya bölgelerden erişen kullanıcılar için erişimi engelleme. 

Güvenilen IP'leri bir ağ konumu ya da temsil ettiğini adlandırılmış konum veya çok faktörlü kimlik doğrulaması için bir etiket konumdur.


## <a name="named-locations"></a>Adlandırılan yerler 

Adlandırılmış konumlarla IP adres aralıkları, ülke ve bölgelerden oluşan mantıksal gruplar oluşturabilirsiniz. 

Adlandırılmış konumlarda erişebilirsiniz **Yönet** koşullu erişim sayfasının bölümünde.

![Konumlar](./media/active-directory-conditional-access-locations/02.png)

 


Adlandırılmış bir konumu aşağıdaki bileşenlere sahiptir:

![Konumlar](./media/active-directory-conditional-access-locations/42.png)

- **Ad** -adlandırılmış konum görünen adı.

- **IP aralıklarını** -bir veya daha fazla IP adresi aralıkları CIDR biçiminde.

- **Güvenilen konum olarak işaretle** -bir bayrak güvenilir bir konum belirtmek için bir adlandırılmış konum ayarlayabilirsiniz. Genellikle, güvenilir konumlar BT departmanınız tarafından denetlenen ağ alanlarıdır. Koşullu erişim ek olarak, güvenilen adlandırılmış konumlara ayrıca Azure kimlik koruması ve Azure AD güvenlik raporlar tarafından azaltmak için kullanılan [hatalı pozitif sonuç](active-directory-reporting-risk-events.md#impossible-travel-to-atypical-locations-1).

- **Ülke / Bölge** -bu seçenek, bir veya daha fazla ülke veya bölge adlandırılmış bir konumu tanımlamak için seçmenize olanak sağlar. 

- **Bilinmeyen alanlar şunlardır** -bazı IP adreslerini belirli bir ülkeye eşlenmedi. Bu seçenek, bu IP adreslerine adlandırılmış konumda dahil edileceğini seçmenize olanak sağlar. Adlandırılmış konumu kullanarak ilkeyi bilinmeyen konumlara uyguladığınızda onay olması olabilir.

Adlandırılmış konumu yapılandırabilirsiniz Azure AD'de ilgili nesne boyutuyla sınırlıdır. Aşağıdakileri yapılandırabilirsiniz:

- Bir konumu 1200 IP aralıkları ile adlı.

- En fazla 90 adlandırılmış konumlarla her birine atanan bir IP aralığı.




## <a name="trusted-ips"></a>Güvenilen IP'ler

Kuruluşunuzun yerel intranet olarak temsil eden IP adres aralıklarını da yapılandırabilirsiniz [çok faktörlü kimlik doğrulama hizmeti ayarlarını](https://account.activedirectory.windowsazure.com/usermanagement/mfasettings.aspx). Bu özellik, en çok 50 IP adres aralıklarını yapılandırmanızı sağlar. IP adres aralıklarını CIDR biçimindedir. Daha fazla bilgi için bkz: [güvenilen IP'leri](authentication/howto-mfa-mfasettings.md#trusted-ips).  

Güvenilen yapılandırılmış IP'leri varsa, bunlar olarak görünmesini **MFA güvenilen IP'leri** konumu koşulu için konumları listesinde.   

### <a name="skipping-multi-factor-authentication"></a>Çok faktörlü kimlik doğrulaması atlanıyor

Çok faktörlü kimlik doğrulaması ile ilgili hizmet ayarları sayfasında, Kurumsal intranet kullanıcılarının seçerek tanımlayabilirsiniz **my intranet üzerindeki Federasyon kullanıcıları gelen istekleri Atla çok faktörlü kimlik doğrulamasını**. Bu ayar gösterir şirket içi AD FS tarafından verilen, talep ağ güvenilir ve gerekir şirket ağındaki olarak kullanıcıyı tanımlamak için kullanılır. Daha fazla bilgi için bkz: [koşullu erişim kullanarak güvenilen IP'leri özelliğini etkinleştirmek](authentication/howto-mfa-mfasettings.md#enable-the-trusted-ips-feature-by-using-conditional-access).

Bu seçenek denetledikten sonra adlandırılmış konumu da dahil olmak üzere **MFA güvenilen IP'leri** hiçbir ilkede seçilen bu ile uygulanır.

Oturum süreleri uzun ömürlü, mobil ve Masaüstü uygulamaları için koşullu erişim düzenli olarak yeniden değerlendirilir. Varsayılan bir kez bir saattir. İç şirket ağı talebi ve verilen ilk kimlik doğrulaması aynı anda yalnızca Azure AD güvenilen IP aralıkları listesi olmayabilir. Bu durumda, kullanıcı hala şirket ağında olup olmadığını belirlemek daha zor olabilir:

1. Kullanıcının IP adresi güvenilen IP aralıkları birinde olup olmadığını denetleyin.

2. Kullanıcının IP adresi ilk üç sekizlisinin ilk kimlik doğrulaması IP adresinin ilk 3 sekizli eşleşip eşleşmediğini denetleyin. Bu zaman olduğundan IP adresi ilk kimlik doğrulaması ile karşılaştırılır iç şirket ağı talep ilk verildiği ve kullanıcı konumu doğrulandı.

Her iki adım başarısız olursa, bir kullanıcı artık güvenilir bir IP olarak değerlendirilir.



## <a name="location-condition-configuration"></a>Konum koşul yapılandırma

Konum koşul yapılandırdığınızda ayırt etmek için seçeneğiniz vardır:

- Herhangi bir konum 
- Tüm güvenilen konumlar
- Seçili konumlar

![Konumlar](./media/active-directory-conditional-access-locations/01.png)

### <a name="any-location"></a>Herhangi bir konum

Varsayılan olarak, seçme **herhangi bir yere** Internet üzerindeki herhangi bir adresi Bunun anlamı tüm IP adresleri için uygulanacak bir ilke neden olur. Bu ayar adlandırılmış konumu olarak yapılandırılmış IP adresleri için sınırlı değildir. Seçtiğinizde, **herhangi bir yere**, belirli konumlara hala bir ilkesinden hariç tutabilirsiniz. Örneğin, şirket ağı dışındaki tüm konumlara kapsamını belirlemek için güvenilen konumları dışındaki tüm konumlara yönelik bir ilke uygulayabilirsiniz.

### <a name="all-trusted-locations"></a>Tüm güvenilen konumlar

Bu seçenek için geçerlidir:

- Güvenilen konumu olarak işaretlenmiş tüm konumları
- **MFA güvenilen IP'ler** (yapılandırıldıysa)


### <a name="selected-locations"></a>Seçili konumlar

Bu seçenek ile bir veya daha fazla adlandırılmış konumlar seçebilirsiniz. Uygulamak için bu ayarı ile bir ilke için bir kullanıcı seçili konumlardan herhangi birinden bağlanması gerekir. Tıkladığınızda **seçin** adlandırılmış ağlar listesini gösterir adlandırılmış ağ seçim denetim açar. Ağ konumu işaretlenmiş liste ayrıca gösterir olarak güvenilir. Belirtilen konum adlı **MFA güvenilen IP'ler** çok faktörlü kimlik doğrulama hizmeti ayar sayfasında yapılandırılmış IP ayarları eklemek için kullanılır.

## <a name="what-you-should-know"></a>Bilmeniz gerekenler

### <a name="when-is-a-location-evaluated"></a>Bir konum ne zaman değerlendirileceğini?

Koşullu erişim ilkeleri değerlendirilir zaman: 

- Bir kullanıcı bir web uygulaması, mobil veya masaüstü uygulaması başlangıçta açar. 

- Modern kimlik doğrulaması kullanan bir mobil veya masaüstü uygulaması, yeni bir erişim belirteci almak için bir yenileme belirteci kullanır. Varsayılan olarak bu kez bir saattir. 

Bu için mobile anlamına gelir ve Masaüstü uygulamaları, modern kimlik doğrulaması kullanan bir değişiklik konumda ağ konumunu değiştirme bir saat içinde algılandı. Modern kimlik doğrulaması kullanmayan mobil ve Masaüstü uygulamaları için belirteç her istekte ilkesi uygulanır. İstek sıklığını uygulama göre değişebilir. Benzer şekilde, web uygulamaları için ilke ilk oturum açma sırasında uygulanır ve bu web uygulaması, oturum ömrü için uygundur. Uygulamalar arasında oturum süreleri farklılıkları nedeniyle, ilke değerlendirmesi arasındaki zaman da değişir. Uygulamanın yeni bir oturum açma belirteci, istekleri her zaman ilkesi uygulanır.


Varsayılan olarak, Azure AD saatlik olarak başka bir belirteç verir. Bir saat içinde şirket ağı devre dışı taşıdıktan sonra modern kimlik doğrulaması kullanan uygulamalar için ilke zorlanır.


### <a name="user-ip-address"></a>Kullanıcının IP adresi

İlke hesaplanmasında kullanılan IP adresi, kullanıcının ortak IP adresidir. Cihazları için özel bir ağda, bu kullanıcının aygıtına intranet üzerindeki istemci IP'si değil, genel internet'e bağlanmak için ağ tarafından kullanılan adresi değildir. 

### <a name="bulk-uploading-and-downloading-of-named-locations"></a>Toplu yükleme ve adlandırılmış konumları indirme

Oluşturduğunuzda ya da güncelleştirmek için toplu güncelleştirmeleri konumları adlı karşıya yükleme veya kullanabilirsiniz IP aralıklarını içeren bir CSV dosyası yükle. Karşıya yükleme listesinde IP aralıkları bu dosya ile değiştirir. Her satır dosyanın CIDR biçiminde bir IP adres aralığı içerir. 


### <a name="cloud-proxies-and-vpns"></a>Bulut proxy ve VPN'ler 

Barındırılan bulut proxy ya da VPN çözümü kullandığınızda, bir ilke değerlendirme proxy IP adresi olsa da Azure AD IP adresini kullanır. Güvenilen bir kaynaktan geldiğinden hiçbir doğrulama olduğundan genel IP adresi kullanılmayan kullanıcıları içeren X-Forwarded-For (XFF) üstbilgisi böylece sunmak faking bir IP adresi için bir yöntem. 

Yerine, bir etki alanına katılmış aygıt kullanılabilir istemek için kullanılan bir ilke veya iç bulut proxy olduğunda AD FS corpnet talep.



### <a name="api-support-and-powershell"></a>PowerShell ve API desteği 

API ve PowerShell henüz desteklenmiyor adlandırılmış konumları için veya koşullu erişim ilkeleri için.





## <a name="next-steps"></a>Sonraki adımlar

- Koşullu erişim ilkesini yapılandırma hakkında bilmek istiyorsanız [Azure Active Directory'de koşullu erişimi kullanmaya başlama](active-directory-conditional-access-azure-portal-get-started.md).

- Ortamınız için koşullu erişim ilkelerini yapılandırma için hazır olup olmadığını görmek [Azure Active Directory'de koşullu erişim için en iyi uygulamaları](active-directory-conditional-access-best-practices.md). 
