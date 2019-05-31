---
title: 'Azure Active Directory etki alanı Hizmetleri: Dağıtım senaryoları | Microsoft Docs'
description: Azure AD Domain Services için dağıtım senaryoları
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: daveba
editor: curtand
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/21/2017
ms.author: ergreenl
ms.openlocfilehash: 0659586512b36c51c5058271fa5e1bdb46efbc3b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66246832"
---
# <a name="deployment-scenarios-and-use-cases"></a>Dağıtım senaryoları ve kullanım örnekleri
Bu bölümde, bazı senaryolar ve Azure Active Directory (AD) etki alanı Hizmetleri'nden yararlanan kullanım örnekleri bakacağız.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Azure sanal makinelerini güvenli, kullanımı kolay yönetim
Azure sanal makinelerinizin kolaylaştırılmış bir şekilde yönetmek için Azure Active Directory Domain Services'ı kullanabilirsiniz. Azure sanal makineleri, böylece oturum açmak için Kurumsal AD kimlik bilgilerinizi kullanmanıza olanak yönetilen etki alanına katılabilir. Bu yaklaşım, Azure sanal makinelerinizin tümünde yerel yönetici hesapları bakımını yapma gibi kimlik bilgisi yönetimi sorunlarıyla uğraşmak zorunda kalmamanızı sağlar.

Yönetilen etki alanına Server sanal makineleri de yönetilebilir ve Grup İlkesi kullanılarak güvenli. Gerekli güvenlik temellerini Azure sanal makineleriniz için geçerlidir ve bunları Kurumsal güvenlik yönergelerine uygun olarak kilitleme. Örneğin, bu sanal makinelere başlatılabilen uygulamalar türlerini kısıtlamak için Grup İlkesi yönetim özelliklerini kullanabilirsiniz.

![Azure sanal makinelerini kolaylaştırılmış Yönetim](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Contoso yaşam uç sunucuları ve diğer altyapısını ulaşır gibi birçok uygulama şu anda şirket içi bulutta barındırılan taşınıyor. Kendi geçerli BT standart kurumsal uygulamaları barındıran sunucu etki alanına katılmış ve Grup İlkesi kullanılarak yönetilen olmasını zorunlu kılar. Contoso'nun BT yöneticisi tercih ettiği etki alanı katılma sanal makineleri, Azure'a Dağıtılmış Yönetim kolaylaştırmak için. Sonuç olarak, Yöneticiler ve kullanıcılar şirket kimlik bilgilerini kullanarak da oturum açabilirsiniz. Aynı zamanda, Grup İlkesi kullanarak gerekli güvenlik temelleri ile uyum sağlamak için makine yapılandırılabilir. Contoso, dağıtma, izleme ve Azure sanal makineleri korumak için Azure etki alanı denetleyicilerini Yönet etmek zorunda kalmamayı tercih edebilir. Bu nedenle, Azure AD etki alanı Hizmetleri, bu kullanım örneği için çok uygun olabilir.

**Dağıtım notları**

Bu dağıtım senaryosu için aşağıdaki önemli noktalara dikkat edin:

* Yönetilen etki alanlarını Azure AD Domain Services tarafından sağlanan varsayılan olarak tek bir düz OU (kuruluş birimi) yapısı sağlar. Etki alanına katılmış tüm makinelerde tek bir düz OU'da yer alır. Ancak özel OU'ları oluşturmak tercih edebilirsiniz.
* Azure AD etki alanı Hizmetleri, bir yerleşik GPO'yu her kullanıcıları ve bilgisayarları biçiminde basit Grup İlkesi destekler kapsayıcıları. Özel GPO'ları oluşturmak ve bunları özel OU'lara hedefleyin.
* Azure AD etki alanı Hizmetleri temel AD bilgisayar nesnesi şemaya destekler. Bilgisayar nesnesinin şema genişletemezsiniz.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Lift-and-shift ile taşıma LDAP bağlama kimlik doğrulaması için Azure altyapı hizmetleri kullanan bir şirket içi uygulama
![LDAP bağlama](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso bir ISV çok sayıda yıl önce satın alınmış bir şirket içi uygulama sahiptir. Uygulama şu anda ISV tarafından Bakım modunda ve uygulamada değişiklik isteyen Contoso için fazla vakit pahalıdır. Bu uygulama bir web formu kullanarak kullanıcı kimlik bilgilerini toplayan ve ardından bir LDAP bağlaması Kurumsal Active Directory gerçekleştirerek kullanıcılarının kimliğini doğrulayan bir web tabanlı bir ön ucu vardır. Contoso, Azure altyapı hizmetleri için bu uygulamayı geçirmek istiyor. Uygulamanın herhangi bir değişikliğe gerek olmadan, olduğu gibi çalıştığını istenen bir durumdur. Ayrıca, kullanıcılar, mevcut Kurumsal kimlik bilgilerini kullanarak kimlik doğrulaması için olmalıdır ve kullanıcılar farklı işlemler yapmak üzere yeniden eğitme zorunda kalmadan. Diğer bir deyişle, son kullanıcıların uygulamayı nerede çalıştığını oblivious olmalıdır ve geçiş için saydam olmalıdır.

**Dağıtım notları**

Bu dağıtım senaryosu için aşağıdaki önemli noktalara dikkat edin:

* Uygulamanın dizine değiştirmek/yazma gerekmeyen emin olun. Yönetilen etki alanlarını Azure AD Domain Services tarafından sağlanan LDAP yazma erişimi desteklenmiyor.
* Yönetilen etki alanına karşı doğrudan parolaları değiştiremezsiniz. Son kullanıcılara parolalarını değiştirebilir ya da Azure AD Self Servis parola değiştirme mekanizmasını kullanma veya şirket içi dizin karşı. Bu değişiklikleri otomatik olarak eşitlenen ve yönetilen etki alanında kullanılabilir.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Azure altyapı hizmetleri dizine erişmek için LDAP kullanan bir şirket içi uygulamayı lift-and-shift ile taşıma okuyun
Contoso neredeyse on yıl önce geliştirilmiş bir şirket içi iş kolu (LOB) uygulaması vardır. Bu uygulamayı kullanan dizin ve Windows Server AD ile çalışacak şekilde tasarlanmıştır. Uygulama, kullanıcılar hakkında bilgileri/özniteliklerini Active Directory'den okumak için LDAP (Basit Dizin Erişimi Protokolü) kullanır. Uygulama özniteliklerini değiştirin veya aksi halde dizine yazma desteklemez. Contoso, Azure altyapı hizmetleri için bu uygulamayı geçirmek ve şu anda bu uygulamayı barındıran eskime şirket içi donanım devre dışı bırakmak ister misiniz? Modern dizini REST tabanlı Azure AD Graph API gibi API'leri kullanmak için uygulamayı yeniden yazılması olamaz. Bu nedenle, kod değiştirme veya uygulama yeniden yazma bulutta çalıştırmak için uygulama geçirilebilir gerçekleştirilmesine lift-and-shift ile taşıma seçeneği istenir.

**Dağıtım notları**

Bu dağıtım senaryosu için aşağıdaki önemli noktalara dikkat edin:

* Uygulamanın dizine değiştirmek/yazma gerekmeyen emin olun. Yönetilen etki alanlarını Azure AD Domain Services tarafından sağlanan LDAP yazma erişimi desteklenmiyor.
* Uygulama özel genişletilmiş bir Active Directory şemasını gerekmeyen emin olun. Şema uzantıları, Azure AD Etki Alanı Hizmetleri'nde desteklenmez.

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Azure altyapı hizmetleri için bir şirket içi hizmet veya yordam uygulamasını geçirme
Bazı uygulamalar birden çok katman, katmanlardan birine veritabanı katmanı gibi bir arka uç katmanı için kimliği doğrulanmış aramalar gerçekleştirmek için gereken yere oluşur. Active Directory hizmet hesapları, bu kullanım durumları için yaygın olarak kullanılır. Lift-and-shift ile taşıma için söz konusu uygulamalar, Azure altyapı hizmetleri ve bu uygulamaların kimlik gereksinimleri için Azure AD etki alanı Hizmetleri kullanın. Şirket içi dizininizden Azure AD'ye eşitlenen aynı hizmet hesabını kullanmayı da tercih edebilirsiniz. Alternatif olarak, ilk özel bir OU oluşturun ve ardından söz konusu uygulamalar dağıtmak için OU içinde ayrı bir hizmet hesabı oluşturun.

![WIA kullanarak hizmet hesabı](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso bir arka uç FTP sunucusuna bir web ön ucu ve bir SQL server içeren bir özel olarak geliştirilmiş yazılımlar kasa uygulama vardır. Windows tümleşik kimlik doğrulaması hizmet hesapları, web ön ucu FTP sunucusuna kimlik doğrulaması için kullanılır. Web ön ucu bir hizmet hesabı çalışacak şekilde ayarlayın. Arka uç sunucusu için web ön uç hizmet hesabından erişim yetkisi vermek için yapılandırılır. Contoso, Azure altyapı hizmetleri bu uygulamaya taşımak için bulutta bir etki alanı denetleyicisi sanal makinesini dağıtmak almamayı tercih eder. Contoso'nun BT yöneticisi, web ön ucu, SQL server ve Azure sanal makinelerine FTP sunucusunun barındırma sunucuları dağıtabilirsiniz. Bu makineler sonra bir Azure AD Domain Services yönetilen etki alanına katılmış. Ardından, bunların aynı hizmet hesabını kendi şirket içi dizininde uygulamanın kimlik doğrulama amaçlarıyla kullanabilirsiniz. Bu hizmet hesabı, Azure AD Domain Services yönetilen etki alanına eşitlenir ve kullanıma hazırdır.

**Dağıtım notları**

Bu dağıtım senaryosu için aşağıdaki önemli noktalara dikkat edin:

* Uygulama kimlik doğrulaması için kullanıcı adı/parola kullandığından emin olun. Sertifika/akıllı kart tabanlı kimlik doğrulaması, Azure AD Domain Services tarafından desteklenmiyor.
* Yönetilen etki alanına karşı doğrudan parolaları değiştiremezsiniz. Son kullanıcılara parolalarını değiştirebilir ya da Azure AD Self Servis parola değiştirme mekanizmasını kullanma veya şirket içi dizin karşı. Bu değişiklikleri otomatik olarak eşitlenen ve yönetilen etki alanında kullanılabilir.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Azure dağıtımları Windows Server Uzak Masaüstü Hizmetleri
Azure AD Domain Services, Azure'da dağıtılan, Uzak Masaüstü sunucularının yönetilen AD etki alanı hizmetleri sağlamak için kullanabilirsiniz.

Bu dağıtım senaryosu hakkında daha fazla bilgi için bkz. nasıl [Azure AD Domain Services, RDS dağıtımı ile tümleştirmeniz](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).


## <a name="domain-joined-hdinsight-clusters-preview"></a>Etki alanına katılmış HDInsight kümeleri (Önizleme)
Apache Ranger etkin bir Azure AD Domain Services yönetilen etki alanına katılmış bir Azure HDInsight kümesi ayarlayabilirsiniz. Oluşturma ile Apache Ranger Hive ilkelerini uygulamak ve kullanıcıların (örneğin, veri uzmanları) için Hive ODBC tabanlı araçlar, örneğin Excel, Tableau vb. kullanarak bağlanmasına izin vermek. Microsoft kısa süre içinde etki alanına katılmış HDInsight için Storm, HBase ve Spark gibi diğer iş yükleri ekleme çalışmaktadır.

Bu dağıtım senaryosu hakkında daha fazla bilgi için bkz. nasıl [etki alanına katılmış HDInsight kümelerini yapılandırma](../hdinsight/domain-joined/apache-domain-joined-configure.md)
