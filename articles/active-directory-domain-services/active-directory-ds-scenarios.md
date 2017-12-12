---
title: "Azure Active Directory etki alanı Hizmetleri: Dağıtım senaryoları | Microsoft Docs"
description: "Azure AD etki alanı Hizmetleri için dağıtım senaryoları"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: c5216ec9-4c4f-4b7e-830b-9d70cf176b20
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/21/2017
ms.author: maheshu
ms.openlocfilehash: 11844fb8fabada9d863fe4adf0839ae6fa2ed101
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/11/2017
---
# <a name="deployment-scenarios-and-use-cases"></a>Dağıtım senaryoları ve kullanım örnekleri
Bu bölümde, birkaç senaryo ve Azure Active Directory (AD) etki alanı Hizmetleri'nden yararlanan kullanım örnekleri ele.

## <a name="secure-easy-administration-of-azure-virtual-machines"></a>Güvenli, kolay yönetim Azure sanal makineler
Azure Active Directory etki alanı Hizmetleri, Azure sanal makinelerinizi kolay bir şekilde yönetmek için kullanabilirsiniz. Azure sanal makineler, böylece oturum açmak için Kurumsal AD kimlik bilgilerinizi kullanmanıza olanak sağlayarak yönetilen etki alanı katılabilir. Bu yaklaşım, yerel yönetici hesapları her Azure sanal makinelerinizin bakımı gibi kimlik bilgisi yönetim Taahhüde önlemeye yardımcı olur.

Yönetilen etki alanına katılan bir sunucu sanal makineler de yönetilebilir ve Grup İlkesi kullanılarak güvenlik altına. Gerekli güvenlik temelleri, Azure sanal makineleri için geçerli ve bunları Kurumsal güvenlik yönergelerine uygun olarak kilitleme. Örneğin, bu sanal makinelere başlatılabilir uygulama türlerini sınırlamak için Grup İlkesi yönetim özelliklerini kullanabilirsiniz.

![Azure sanal makinelerin kolaylaştırılmış Yönetim](./media/active-directory-domain-services-scenarios/streamlined-vm-administration.png)

Yaşam uç sunucuları ve diğer altyapıya ulaştığında gibi birçok uygulama şu anda şirket içi bulutta barındırılan Contoso taşımaktır. Geçerli kendi BT standart şirket uygulamaları barındıran sunucu etki alanına katılmış ve Grup İlkesi kullanılarak yönetilen olmalıdır olması zorunlu tutulmuştur. Contoso'nun BT yöneticisi tercih eder, Azure'da dağıtılan etki alanı katılma sanal makineleri için yönetim kolaylaştırmak için. Sonuç olarak, Yöneticiler ve kullanıcılar şirket kimlik bilgilerini kullanarak oturum açabilir. Aynı anda makineler Grup İlkesi kullanarak gerekli güvenlik temelleri ile uyumlu olacak şekilde yapılandırılabilir. Contoso dağıtmak, izlemek ve Azure sanal makineler güvenli hale getirmek için Azure etki alanı denetleyicileri yönetmek zorunda kalmazsınız tercih edebilir. Bu nedenle, Azure AD etki alanı Hizmetleri, bu kullanım örneği için harika bir sığdırma olur.

**Dağıtım notları**

Bu dağıtım senaryosu için aşağıdaki önemli noktaları göz önünde bulundurun:

* Azure AD etki alanı Hizmetleri tarafından sağlanan yönetilen etki alanları varsayılan olarak tek bir düz OU (kuruluş birimi) yapısı sağlar. Etki alanına katılmış tüm makinelerde tek bir düz OU'da bulunur. Ancak, özel OU'ları oluşturmak seçebilirsiniz.
* Azure AD etki alanı Hizmetleri, bir yerleşik GPO her kullanıcılar ve bilgisayarlar için formunda basit Grup İlkesi destekler kapsayıcıları. Özel GPO'ları oluşturmak ve bunları özel OU'lar hedefleyebilirsiniz.
* Azure AD etki alanı Hizmetleri, temel AD bilgisayar nesnesi şemaya destekler. Bilgisayar nesnesi şemasının genişletemezsiniz.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-bind-authentication-to-azure-infrastructure-services"></a>Yükseltme-ve-shift Azure altyapı hizmetleri için LDAP bağlama kimlik doğrulaması kullanan bir şirket içi uygulama
![LDAP bağlama](./media/active-directory-domain-services-scenarios/ldap-bind.png)

Contoso birçok yıl önce bir ISV satın alınmış bir şirket içi uygulama sahiptir. Uygulama şu anda bakım modunda ISV tarafından ve uygulamasında yapılacak değişiklikler isteyen Contoso için şekilde basımı karşılamayacak kadar pahalıdır. Bu uygulama bir web formu kullanarak kullanıcı kimlik bilgilerini toplar ve daha sonra kurumsal Active Directory'ye LDAP bağı gerçekleştirerek kullanıcıların kimliğini doğrulayan bir web tabanlı bir ön uç sahiptir. Contoso Azure altyapı hizmetleri bu uygulamaya geçirmek istediğiniz. Uygulama, herhangi bir değişiklik gerektirmeden olduğu gibi çalıştığını iyi bir şeydir. Ayrıca, kullanıcıların varolan şirket kimlik bilgilerini kullanarak kimlik doğrulaması için gereken ve kullanıcıların farklı şeyler yeniden eğitme gerek kalmadan. Diğer bir deyişle, son kullanıcıların uygulamanın çalıştığı oblivious olmalıdır ve geçiş için saydam olmalıdır.

**Dağıtım notları**

Bu dağıtım senaryosu için aşağıdaki önemli noktaları göz önünde bulundurun:

* Uygulamanın dizine değiştirin/yazma gerekmez emin olun. Azure AD etki alanı Hizmetleri tarafından sağlanan yönetilen etki alanları için LDAP yazma erişimi desteklenmiyor.
* Doğrudan yönetilen etki alanına göre parolaları değiştiremezsiniz. Son kullanıcıların parolalarını değiştirebilir ya da Azure AD Self Servis parola değişikliği mekanizmasını kullanarak veya şirket içi dizin karşı. Bu değişiklikler yönetilen etki alanında otomatik olarak eşitlenmesi ve kullanılabilir.

## <a name="lift-and-shift-an-on-premises-application-that-uses-ldap-read-to-access-the-directory-to-azure-infrastructure-services"></a>Yükseltme ve shift, Azure altyapı hizmetleri dizinine erişmek için LDAP kullanan bir şirket içi uygulama okuyun.
Contoso hemen önce bir on geliştirilmiştir bir şirket içi iş kolu (LOB) uygulama sahiptir. Bu uygulama dizini kullanan ve Windows Server AD ile çalışacak şekilde tasarlanmıştır. Uygulama, kullanıcılar hakkında bilgi/özniteliklerini Active Directory'den okumak için LDAP (Basit Dizin Erişimi Protokolü) kullanır. Uygulama, öznitelikleri değiştirmeyin veya aksi halde dizine yazma. Contoso Azure altyapı hizmetleri bu uygulamaya geçirmek ve şu anda bu uygulamayı barındıran eskime şirket içi donanım devre dışı bırakmak istiyor. Modern dizini API REST tabanlı Azure AD grafik API'si gibi kullanmak için uygulamayı yeniden yazılması olamaz. Bu nedenle, uygulama kodunda değişiklik veya uygulamayı yeniden yazma işlemi bulutta çalışacak geçirilemez aslına yükseltme shift seçeneği istenen.

**Dağıtım notları**

Bu dağıtım senaryosu için aşağıdaki önemli noktaları göz önünde bulundurun:

* Uygulamanın dizine değiştirin/yazma gerekmez emin olun. Azure AD etki alanı Hizmetleri tarafından sağlanan yönetilen etki alanları için LDAP yazma erişimi desteklenmiyor.
* Uygulama özel genişletilmiş Active Directory Şeması'nı gerekmez emin olun. Şema uzantıları, Azure AD Etki Alanı Hizmetleri'nde desteklenmez.

## <a name="migrate-an-on-premises-service-or-daemon-application-to-azure-infrastructure-services"></a>Şirket içi hizmet veya arka plan programı uygulamaya Azure altyapı hizmetleri geçirme
Birden çok katmanları, burada bir katmanı veritabanı katmanı gibi bir arka uç katmanı kimliği doğrulanmış çağrıları gerçekleştirmek için gereken bazı uygulamalar oluşur. Active Directory hizmet hesapları, bu kullanım durumları için yaygın olarak kullanılır. Yükseltme ve üst karakter olabilir ve bu uygulamaların kimlik ihtiyaçları için Azure AD etki alanı Hizmetleri Azure altyapı hizmetleri bu tür uygulamalar. Şirket içi dizininizden Azure AD'ye eşitlenen aynı hizmet hesabını kullanmayı seçebilirsiniz. Alternatif olarak, önce özel bir OU oluşturmanız ve ardından bu tür uygulamalar dağıtmak için bu OU içinde ayrı bir hizmet hesabı oluşturun.

![WIA kullanarak hizmeti hesabı](./media/active-directory-domain-services-scenarios/wia-service-account.png)

Contoso web ön uç, bir SQL server ve arka uç FTP sunucusu içeren bir özel olarak geliştirilmiş yazılımlar kasası uygulama vardır. Hizmet hesapları Windows tümleşik kimlik doğrulaması, FTP sunucusu için web ön uç kimlik doğrulaması için kullanılır. Web ön uç hizmet hesabı çalışacak şekilde ayarlanır. Arka uç sunucusu için web ön uç hizmet hesabından erişim yetkisi vermek için yapılandırılır. Contoso Azure altyapı hizmetleri bu uygulamayı taşımak için bulutta bir etki alanı denetleyicisi sanal makine dağıtmak zorunda kalmazsınız tercih eder. Contoso'nun BT yöneticisi, web ön uç, SQL server ve Azure sanal makineleri FTP sunucusuna barındırma sunucuları dağıtabilirsiniz. Bu makineler sonra bir Azure AD etki alanı Hizmetleri yönetilen etki alanına eklenir. Ardından, bunlar aynı hizmet hesabını kendi şirket içi dizin uygulamanın kimlik doğrulama amacıyla kullanabilirsiniz. Bu hizmet hesabı, Azure AD etki alanı Hizmetleri yönetilen etki alanı eşitlenir ve kullanıma hazırdır.

**Dağıtım notları**

Bu dağıtım senaryosu için aşağıdaki önemli noktaları göz önünde bulundurun:

* Uygulama kimlik doğrulaması için kullanıcı adı/parola kullandığından emin olun. Sertifika/akıllı kart tabanlı kimlik doğrulaması Azure AD etki alanı Hizmetleri tarafından desteklenmiyor.
* Doğrudan yönetilen etki alanına göre parolaları değiştiremezsiniz. Son kullanıcıların parolalarını değiştirebilir ya da Azure AD Self Servis parola değişikliği mekanizmasını kullanarak veya şirket içi dizin karşı. Bu değişiklikler yönetilen etki alanında otomatik olarak eşitlenmesi ve kullanılabilir.

## <a name="windows-server-remote-desktop-services-deployments-in-azure"></a>Azure dağıtımları Windows Server Uzak Masaüstü Hizmetleri
Azure AD etki alanı Hizmetleri, Azure üzerinde dağıtılan Uzak Masaüstü sunucularınızı yönetilen AD etki alanı hizmetleri sağlamak için kullanabilirsiniz.

Bu dağıtım senaryosu hakkında daha fazla bilgi için bkz: nasıl yapılır [RDS dağıtımı ile Azure AD etki alanı Hizmetleri Tümleştirme](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-azure-adds).


## <a name="domain-joined-hdinsight-clusters-preview"></a>Etki alanına katılmış Hdınsight kümeleri (Önizleme)
Apache bırakabilmenizi etkin bir Azure AD etki alanı Hizmetleri yönetilen etki alanına katılmış Azure Hdınsight kümesi ayarlayabilirsiniz. Oluşturun ve Apache bırakabilmenizi aracılığıyla Hive ilkeleri uygulanır ve kullanıcıların (örneğin, veri bilimcilerine) için Hive ODBC tabanlı araçlar, örneğin Excel, Tableau vb. kullanarak bağlanmasına izin vermek. Microsoft, HBase, Spark ve Storm, gibi diğer iş yüklerinin en kısa sürede etki alanına katılmış Hdınsight için ekleme çalışmaktadır.

Bu dağıtım senaryosu hakkında daha fazla bilgi için bkz: nasıl yapılır [etki alanına katılmış Hdınsight kümeleri yapılandırma](../hdinsight/domain-joined/apache-domain-joined-configure.md)
