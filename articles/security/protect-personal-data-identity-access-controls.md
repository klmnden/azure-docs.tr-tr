---
title: Kişisel veriler Azure kimlik ve erişim denetimleri ile koruma | Microsoft Docs
description: Azure kimlik ve erişim denetimleri kişisel verileri korumak ve genel veri koruma düzenleme (GDPR) ile uymak yardımcı olabilecek Yardım
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/06/2018
ms.author: barclayn
ms.custom: ''
ms.openlocfilehash: c0e7f2060f81812cd69ed1af0246287757985243
ms.sourcegitcommit: fa493b66552af11260db48d89e3ddfcdcb5e3152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2018
---
# <a name="azure-active-directory-and-multi-factor-authentication-protect-personal-data-with-identity-and-access-controls"></a>Azure Active Directory ve çok faktörlü kimlik doğrulaması: kimlik ve erişim denetimleri ile kişisel verileri koruma

Bu makale, bilgi ve Azure Active Directory ve çok faktörlü kimlik doğrulama güvenlik özellikleri ve Hizmetleri kullanarak kişisel verilerinizi korumak için kullanabileceğiniz yordamları sağlar. Bu makalede yer alan bilgileri, genel veri koruma düzenleme (GDPR) ile uyumlu olacak şekilde çalışmalarını yararlı olabilir.

## <a name="scenario"></a>Senaryo

Amerika Birleşik Devletleri'nde yönetim büyük seyahat şirket, masraflarını Akdeniz, Adriatic ve Baltık seas yanı sıra İngiliz Adaları arasında içinde sunmaya işlemlerini genişletmektedir. Bu çalışmalarını desteklemek için daha küçük birkaç seyahat satırlarını İtalya, Almanya, Danimarka ve İngiltere göre aldı 

Şirket Microsoft Azure bulutta şirket verilerini depolamak için kullanır. Bu ad, adres, telefon numaraları gibi kişisel olarak tanımlanabilir bilgileri ve genel müşteri tabanı, kredi kartı bilgilerini içerir. Ayrıca tüm konumlarda adresleri, telefon numaralarını, vergi kimlik numaraları ve şirket çalışanlar hakkında diğer bilgiler gibi geleneksel İnsan Kaynakları bilgileri içerir. Seyahat satır de geçerli ve geçmiş müşterilerle ilişkilerini izlemek için kişisel bilgi içeren büyük bir veritabanı ödül ve bağlılık programı üyelerinin tutar.

Şirket çalışanları şirketin şubelere ağ erişimi ve seyahat tüm dünyada bulunan aracıları bazı şirket kaynaklarına erişimi vardır.

## <a name="problem-statement"></a>Sorun bildirimi

Şirket müşterilerin ve çalışanların kişisel verilerin gizliliği tehlikeye atılmış kimlik erişebilmek için kullanmak isteyen saldırganlardan korumanız gerekir. Bunlar ayrıca yasal kullanıcılar tarafından kişisel verilere erişimi yalnızca işlerini yapmak için ihtiyacınız olan olanlar için sınırlı olduğundan emin olmalısınız.

## <a name="company-goal"></a>Şirket hedefi

Şirketin hedefi, kişisel verilere erişimi kesinlikle denetlenir sağlamaktır. Kişisel verilere erişimi olan kullanıcıların kimliklerini güçlü kimlik doğrulamasıyla korunması önemlidir. Bir ilke [en az ayrıcalık] (https://en.wikipedia.org/wiki/Principle_of_least_privilege) böylece yetkili kullanıcıları yalnızca ihtiyaç duydukları erişim ve artık düzeyi zorlanmış olmalıdır.

## <a name="solutions"></a>Çözümler

Microsoft Azure kişisel verileri içeren kaynaklara erişiminin şirketler denetlemeye yardımcı olmak için kimlik ve erişim yönetim araçları sağlar.

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) (AAD) kimlikleri yönetir ve sair şirket içi ve diğer bulut kaynakları, veri ve uygulamaları için Azure erişimi kontrol eder. [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access) kişisel verileri gibi belirli bilgilere erişimi olan kişiler sayısını en aza indirmek için Azure yöneticilerin yardımcı olur. Bunları bulmak, kısıtlama ve ayrıcalıklı kimliklerinizi ve bunların kaynaklara erişimi izlemek ve geçici, atamak için uygun kullanıcılara yönetici hakları Just-In-Time (JIT) sağlar. Ayrıca, kullanıcılar AAD yönetici ayrıcalıklarına sahip bir anlayış sağlar.

AAD PIM kullanarak söz konusu etkinlikleri içerir:

- Privileged Identity Management dizininiz için etkinleştirme

- Bir bakışta önemli bilgileri görmek için Privileged Identity Management yönetim panosunu kullanma

- Ekleyerek veya kaldırarak her rol için kalıcı veya uygun administrators (Yöneticiler) ayrıcalıklı kimlikleri yönetme

- Rol etkinleştirme ayarlarını yapılandırma

- Rol etkinleştirme

- Rol etkinliği gözden geçirme

#### <a name="how-do-i-enable-aad-pim"></a>AAD PIM nasıl etkinleştirebilirim?

PIM dizininiz için kullanmaya başlamak için aşağıdakileri yapın:

1. Azure portalına dizininizin genel yönetici olarak oturum açın.

2. Kuruluşunuz birden fazla dizine sahipse Azure portalının sağ üst köşesinde kullanıcı adınızı seçin. Azure AD Privileged Identity Management burada kullanacağı dizini seçin.

3. Seçin **daha fazla hizmet** ve **filtre** için Azure AD Privileged Identity Management aramak için metin kutusu.

4. **Panoya sabitle**'yi işaretleyin ve ardından **Oluştur**’a tıklayın. Privileged Identity Management uygulaması açılır.

Azure AD Privileged Identity Management ayarlandığında, uygulamayı her açtığınızda gezinti dikey penceresini görürsünüz.

![](media/protect-personal-data-identity-access-controls/azure-pim.png)

Daha fazla bilgi ve AAD PIM ile çalışmaya başlama hakkında yönergeler için bkz: [Başlat kullanarak Azure AD Privileged Identity Management.](https://docs.microsoft.com/active-directory/active-directory-privileged-identity-management-getting-started)

### <a name="azure-role-based-access-control"></a>Azure rol tabanlı erişim denetimi

[Azure rol tabanlı erişim denetimi](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) (RBAC) Azure kaynaklarına erişimi kullanıcının atanmış rol tabanlı olarak erişim verilmesi etkinleştirerek Azure yönetmesine yardımcı olur. Ekip içinde görevlerini kurabilmeleri ve kullanıcıları, grupları ve işlerini yapmak için gereksinim duydukları uygulamaları sadece erişim miktarını verebilirsiniz.

Kullanıcılara rol tabanlı erişim Azure portalı, Azure Komut Satırı araçları ve Azure Management API’leri kullanılarak verilebilir.

Azure RBAC temel kavramları hakkında daha fazla bilgi için bkz: [Azure Portal'da rol tabanlı erişim denetimi kullanmaya başlayın.](https://docs.microsoft.com/azure/role-based-access-control/overview)

#### <a name="how-do-i-manage-azure-rbac-with-powershell"></a>PowerShell ile Azure RBAC nasıl yönetebilirim?

Aşağıdaki yönetim görevlerini de dahil olmak üzere Azure RBAC yönetmek için PowerShell cmdlet'lerini kullanabilirsiniz:

- Liste rolleri

- Kimlerin erişebileceğini bakın

- Erişim verme

- Erişimi kaldırma

- Özel bir rol oluşturun

- Bir kaynak sağlayıcısı için Eylemler Al

- Özel bir rol değiştirme

- Bir özel rolü silmeyi

- Liste özel roller

PowerShell ile Azure RBAC yönetme hakkında daha fazla yönerge için bkz: [yönetmek rol tabanlı erişim Azure PowerShell ile](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-powershell).

### <a name="azure-multi-factor-authentication"></a>Azure Multi-Factor Authentication

[Azure çok faktörlü kimlik doğrulaması](https://docs.microsoft.com/azure/multi-factor-authentication/) (MFA), veri ve uygulamaları, erişim için basit bir oturum açma işlemini kullanıcı talebine buluştururken korunmasına yardımcı olan iki aşamalı doğrulama çözümü. Bir dizi doğrulama yöntemi (ör. telefon çağrısı, metin mesajı veya mobil uygulama doğrulaması) aracılığıyla güçlü kimlik doğrulaması sunar.

Azure bulutta MFA dağıtmak için önce onu etkinleştirmeniz ve kullanıcılar için iki aşamalı doğrulamayı açmak gerekir.

#### <a name="how-do-i-enable-azure-to-use-mfa"></a>Azure MFA kullanmaya nasıl etkinleştirebilirim?

Kullanıcılarınızın Azure çok faktörlü kimlik doğrulamasını içeren lisansına sahipseniz, yalnızca Azure MFA üzerinde yapılandırmanız gereken bir kullanıcı veya grup için ayrı ayrı başına. 

![MFA etkin kullanıcılar](media/protect-personal-data-identity-access-controls/enable-mfa.png)

Lisansları şu anda yoksa senaryonuz için en uygun dağıtım türünü belirleme işlemi gitmesi gerekir. Başlıklı makalenin bakarak başlatabilirsiniz [Azure çok faktörlü Autehntication çözüm seçtiğiniz](../active-directory/authentication/concept-mfa-whichversion.md). Bir multi-Factor Authentication sunucusu oluşturmak gerektiğine karar verirseniz. Aşağıdaki adımları izleyerek başlatabilirsiniz:

1. Seçin **Active Directory** (yönetici olarak oturum açmış) Azure portalında.

2. Seçin **MFA sunucusu**

3. Zaman aşımı değeri belirtin. 

    ![](media/protect-personal-data-identity-access-controls/mfa-server-settings.png)

4. **Kaydet**’e tıklayın

Bu pencerede MFA sunucusu yükleme seçeneği de vardır. Boyut ve makaleyi gözden geçirerek dağıtımınızı planlama hakkında ek ayrıntılar elde edebilirsiniz [Azure multi-Factor Authentication Sunucusu'nu kullanmaya başlama](../active-directory/authentication/howto-mfaserver-deploy.md)

Çok faktörlü kimlik doğrulama sağlayıcısının nasıl yönetileceği hakkında daha fazla yönerge için bkz: [Azure multi-Factor Auth sağlayıcısı ile çalışmaya başlama.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-get-started-auth-provider)

#### <a name="how-do-i-turn-on-two-step-verification-for-users"></a>Kullanıcılar için iki aşamalı doğrulamayı nasıl kapatırım?

Tüm oturum açma işlemleri için iki aşamalı doğrulama zorunlu kılabilir veya yalnızca belirli koşullarda, iki aşamalı doğrulama gerektirecek şekilde koşullu erişim ilkeleri oluşturabilirsiniz.

Azure MFA kullanıcı durumları değiştirerek etkinleştirme, iki aşamalı doğrulama gerektirme geleneksel bir yaklaşımdır. Etkinleştirmeniz tüm kullanıcılar oturum her zaman iki aşamalı doğrulamayı gerçekleştirmek için aynı gereksinime sahip olacaktır. Bir kullanıcının kullanıcı etkileyebilecek herhangi bir koşullu erişim ilkeleri geçersiz kılar.

Koşullu erişim ilkesi ile Azure MFA'yı etkinleştirerek, iki aşamalı doğrulama gerektirme daha esnek bir yaklaşımdır. Tek tek kullanıcılar yanı sıra grupları uygulamak koşullu erişim ilkeleri oluşturabilirsiniz. Daha fazla kısıtlama düşük riskli grupları daha yüksek riskli grupları verilebilir veya iki aşamalı doğrulamayı yalnızca yüksek riskli bulut uygulamaları için gereklidir ve düşük riskli için olanları atlandı. Ancak, koşullu erişim Azure Active Directory, ücretli bir özelliğidir.

Kullanıcı durumunu değiştirerek MFA etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında yönetici olarak oturum açın.
2. Git **Azure Active Directory \> kullanıcılar ve gruplar \> tüm kullanıcılar**.
3. Seçin **çok faktörlü kimlik doğrulaması**.
4. Azure MFA için etkinleştirmek istediğiniz kullanıcıyı bulun. Üst kısımdaki görünümü değiştirmeniz gerekebilir.
5. Kullanıcı adının yanındaki kutuyu işaretleyin.
6. Sağdaki hızlı adımlar altında seçin **etkinleştirmek**.

   ![](media/protect-personal-data-identity-access-controls/mfa-bulk.png)

7. Açılır pencere Seçiminizi onaylayın.  Kendisi için MFA etkinleştirilmiş kullanıcılar oturum açtığında kaydetmek için istenir.

Azure MFA ile koşullu erişim ilkesini etkinleştirmek için aşağıdakileri yapın:

1. Azure portalında yönetici olarak oturum açın.

2. Git **Azure Active Directory \> koşullu erişim**.

3. Seçin **yeni ilke**.

4. Altında **atamaları**seçin **kullanıcılar ve gruplar**. Kullanım **Ekle** ve **hariç** sekmeleri hangi kullanıcıların ve grupların ilke tarafından yönetilen belirtin.

5. Altında **atamaları**seçin **bulut uygulamaları.** Tercih **tüm bulut uygulamaları dahil**.
6.  Altında **erişim denetimleri**seçin **Grant**. Seçin **çok faktörlü kimlik doğrulaması gerektiren**.
7.  Kapatma **ilkesini etkinleştir** için **üzerinde** ve ardından **kaydetmek**.

Sahtekarlık Uyarıları ayarlayın, bir kerelik geçiş oluşturmak, özel sesli mesajları kullanın, önbelleğe alınmasını yapılandırmak, güvenilen IP'leri belirtin, uygulama parolaları oluşturmak için Azure MFA ayarlarının nasıl yapılandırılacağı hakkında daha fazla bilgi için kullanıcıların güven aygıtlara ve select hatırlamaya MFA etkinleştirin doğrulama yöntemlerini görmek [Azure çok faktörlü kimlik doğrulama ayarlarını yapılandırın.](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-whats-next)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure AD'de ayrıcalıklı erişimi güvenli hale getirme](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/active-directory-securing-privileged-access)

- [Azure Multi-Factor Authentication hakkında sık sorulan sorular](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-faq)

- [Rol tabanlı erişim denetimi sorunlarını giderme](https://docs.microsoft.com/azure/role-based-access-control/troubleshooting)

- [Azure Active Directory kimlik koruması](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)
