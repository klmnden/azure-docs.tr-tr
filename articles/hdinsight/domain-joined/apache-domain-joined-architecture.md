---
title: Etki alanına katılmış Azure HDInsight mimarisi
description: Etki alanına katılmış HDInsight planlama hakkında bilgi edinin.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/30/2018
ms.openlocfilehash: efdc9cfbbe9a78571e0a56437e512d0cbbc18b3e
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/19/2018
ms.locfileid: "46297289"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>HDInsight'ta Azure etki alanına katılmış Hadoop kümeleri planlama

Standart Azure HDInsight küme tek kullanıcılı bir kümedir. Büyük veri iş yüklerini oluşturan küçük uygulama ekiplerine sahip çoğu şirket için uygundur. Her kullanıcının isteğe bağlı olarak ayrılmış bir küme oluşturabilir ve artık gerekli olmadığında, yok. 

Çoğu kurum kümelerin BT ekipleri tarafından yönetildiği bir model doğru taşıdık ve birden çok uygulama ekibinin ortak kümelerde takımlar. Bu büyük kuruluşlar, her bir Azure HDInsight kümesinde çok kullanıcılı erişiminin olması gerekir.

HDInsight, yönetilen bir şekilde Active Directory--bir popüler kimlik sağlayıcısını kullanır. HDInsight ile tümleştirerek [Azure Active Directory etki alanı Hizmetleri (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), kümeleri, etki alanı kimlik bilgilerinizi kullanarak erişebilir. 

Sağlanan, etki alanına katılmış HDInsight sanal makinelerin (VM'ler) var. Bunu, HDInsight üzerinde çalışan tüm hizmetler (Ambari, Hive sunucusu, Ranger, Spark thrift sunucusu ve diğerleri) kimliği doğrulanan kullanıcı için sorunsuz bir şekilde çalışır. Yöneticiler, daha sonra kümedeki kaynaklar için rol tabanlı erişim denetimi sağlamak için Apache Ranger'ı kullanarak güçlü yetkilendirme ilkeleri oluşturabilirsiniz.


## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight’ı Active Directory ile tümleştirin

Açık kaynaklı Hadoop üzerinde Kerberos kimlik doğrulaması ve güvenlik için kullanır. Bu nedenle, HDInsight küme düğümleri Azure AD DS tarafından yönetilen bir etki alanına katılır. Kerberos güvenlik küme üzerindeki Hadoop bileşenleri için yapılandırılır. 

Her bir Hadoop bileşeni için bir hizmet sorumlusunu otomatik olarak oluşturulur. Asıl karşılık gelen bir makine etki alanına katılan her makine için de oluşturulur. Bu hizmet depolamak ve ilkeleri makine için bu ilkeleri yerleştirildiği etki alanı denetleyicisi (Azure AD DS) içinde bir kuruluş birimine (OU) sağlamalısınız. 

Özetlemek gerekirse, bir ortam ile ayarlamanız gerekir:

- Bir Active Directory (Azure AD DS tarafından yönetilen) etki alanı.
- Azure AD DS'de etkinleştirilmiş LDAP (LDAPS) güvenli hale getirin.
- Ayrı sanal ağlar tercih ederseniz Azure AD DS sanal ağ için uygun ağ bağlantısı HDInsight sanal ağdan. HDInsight'ın sanal ağ içindeki bir VM görebilmesi için sanal ağ eşlemesi üzerinden Azure AD DS olması gerekir. Aynı sanal ağda HDInsight ve Azure AD DS dağıtılmışsa, bağlantı otomatik olarak sağlanır ve başka bir eylem gerekmez.
- Bir OU [Azure AD DS'de oluşturulan](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).
- İzinleri olan bir hizmet hesabı:
    - Hizmet sorumluları oluşturma.
    - Makine etki alanına ve makine sorumluları oluşturma.

Aşağıdaki ekran görüntüsünde, contoso.com oluşturulan bir OU gösterir. Ayrıca bazı hizmet sorumluları ve makine sorumluları gösterir.

![Etki alanına katılmış HDInsight kümeleri için kuruluş birimi](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

## <a name="set-up-different-domain-controllers"></a>Farklı bir etki alanı denetleyicileri kurun
HDInsight, küme Kerberos iletişimi için kullandığı ana etki alanı denetleyicisi şu anda yalnızca Azure AD DS destekler. Ancak böyle bir kurulum HDInsight erişim için Azure AD DS'yi etkinleştirmek için müşteri adayları sürece diğer karmaşık Active Directory ayarları mümkündür.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) Windows Server Active Directory ile tamamen uyumlu olan yönetilen bir etki alanı sağlar. Microsoft yönetme, düzeltme eki uygulama ve etki alanında bir yüksek oranda kullanılabilir (HA) Kurulum izleme üstlenir. Etki alanı denetleyicilerinin bakımını hakkında endişelenmeden, kümeye dağıtabilirsiniz. 

Azure Active Directory'den (Azure AD) kullanıcıları, grupları ve parolaları eşitlenir. Azure AD DS için Azure AD Örneğinize tek yönlü eşitleme kümeye şirket kimlik bilgilerini kullanarak oturum açmalarını sağlar. 

Daha fazla bilgi için [yapılandırma etki alanına katılmış HDInsight kümeleri Azure AD DS kullanarak](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>Şirket içi Active Directory veya Iaas Vm'leri üzerinde Active Directory

Etki alanınız için bir şirket içi Active Directory örneğine veya daha karmaşık Active Directory ayarları varsa, Azure AD Connect kullanarak kimliklerle Azure AD'ye eşitleyebilirsiniz. Ardından, Azure AD DS, Active Directory kiracısı üzerinde etkinleştirebilirsiniz. 

Kerberos Parola karmalarının üzerinde kullandığından, gerekecektir [Azure AD DS parola karması eşitlemeyi etkinleştir](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Active Directory Federasyon Hizmetleri (AD FS) ile Federasyon kullanıyorsanız, AD FS altyapınızı başarısız olursa, isteğe bağlı olarak parola karması eşitlemeyi yedek olarak ayarlayabilirsiniz. Daha fazla bilgi için [Azure AD Connect eşitlemesi ile parola karma eşitlemesini etkinleştirme](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

Kullanarak Active Directory veya tek başına, Iaas vm'lerinde Active Directory etki alanına katılmış HDInsight kümeleri için desteklenen bir yapılandırma değil Azure AD ve Azure AD DS, olmadan şirket içi.

## <a name="next-steps"></a>Sonraki adımlar
* [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure-using-azure-adds.md)
* [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md)
* [Etki alanına katılmış HDInsight kümelerini yönetme](apache-domain-joined-manage.md) 
