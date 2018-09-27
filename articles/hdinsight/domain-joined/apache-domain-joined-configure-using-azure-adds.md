---
title: Azure AD DS kullanarak bir HDInsight kümesi Kurumsal güvenlik paketi ile yapılandırma
description: Ayarlama ve Azure Active Directory Domain Services'ı kullanarak bir HDInsight Kurumsal güvenlik paketi küme yapılandırma hakkında bilgi edinin.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 2e4aab68dba02eb5df16aa316f867697680b8977
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/26/2018
ms.locfileid: "47181694"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services'ı kullanarak bir HDInsight kümesi Kurumsal güvenlik paketi ile yapılandırma

Kurumsal güvenlik paketi (ESP) kümeleri, Azure HDInsight kümelerinde birden çok kullanıcı erişim sağlar. Böylece etki alanı kullanıcıları kümeleri ile kimlik doğrulaması ve büyük veri işlerini çalıştırmak için etki alanı kimlik bilgilerini kullanabilirsiniz ESP HDInsight kümeleriyle bir etki alanına bağlanır. 

Bu makalede, Azure Active Directory etki alanı Hizmetleri (Azure AD DS) kullanarak bir HDInsight kümesi ile ESP yapılandırma konusunda bilgi edinin.

>[!NOTE]
>ESP HDI 3.6 için Hadoop Spark ve etkileşimli olarak GA ' dir. ESP HBase ve Kafka küme türleri için Önizleme aşamasındadır.

## <a name="enable-azure-ad-ds"></a>Azure'ı etkinleştirme AD DS

Bir HDInsight kümesi ile ESP oluşturabilmeniz için önce Azure AD DS'yi etkinleştirme önkoşuldur. Daha fazla bilgi için [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Kiracı yöneticileri yalnızca bir Azure AD DS'yi örneği oluşturmak için ayrıcalıklara sahip. Multi-Factor authentication, kümeye erişen kullanıcılar için devre dışı bırakılması gerekir.

Güvenli LDAP etkinleştirilirken sertifika konu adında bir etki alanı adı veya konu alternatif adı yerleştirin. Örneğin, etki alanı adınızı ise *contoso.com*, bu tam adı, sertifika konu adı veya konu diğer adı var olduğundan emin olun. Daha fazla bilgi için [güvenli LDAP yapılandırma için bir Azure AD-DS yönetilen etki alanı](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="check-aad-ds-health-status"></a>AAD-DS sistem durumunu denetleyin

Azure Active Directory etki alanı Hizmetleri'niz sistem durumunu görüntülemek için **sistem durumu** altında **Yönet** kategorisi. (Çalışan) yeşil ve eşitleme tamamlandığında AAD DS durumu olduğundan emin olun.

![Azure Active Directory Domain Services durumu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="add-managed-identity"></a>Yönetilen kimlik Ekle

Azure AD DS'yi etkinleştirdikten sonra kullanıcı tarafından atanan bir yönetilen kimlik oluşturun ve atayın **HDInsight etki alanı Hizmetleri katkıda bulunan** Azure AD DS erişim denetimine rol.

![Azure Active Directory etki alanı Hizmetleri erişim denetimi](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Yönetilen bir kimlik için atama **HDInsight etki alanı Hizmetleri katkıda bulunan** rol kimliği AAD DS etki alanındaki belirli bir etki alanı Hizmetleri işlemlerin gerçekleştirilmesi için uygun erişimi olduğunu sağlar. Daha fazla bilgi için [Azure kaynakları için yönetilen kimlikleri nedir](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-hdinsight-cluster-with-esp"></a>ESP ile bir HDInsight kümesi oluşturma

Sonraki adım, Azure AD DS kullanarak etkin ESP ile HDInsight kümesi oluşturmaktır.

Hem Azure AD DS'yi örneği hem de HDInsight küme aynı Azure sanal ağında yerleştirmek daha kolaydır. Farklı sanal ağlarda bulunan koymak seçerseniz, böylece HDInsight Vm'leri görebilmesi Vm'leri katılmak için etki alanı denetleyicisi için bu sanal ağları eşleyebilme gerekir. Daha fazla bilgi için [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md). Eşlemesi düzgün şekilde yapıldıysa test etmek için bir VM HDInsight VNET/alt ağına katılın ve etki alanı adı ping veya çalıştırmak **Ldp.exe'yi** erişim AAD DS etki alanı.

Bir HDInsight kümesi oluşturduğunuzda, Kurumsal güvenlik paketi kullanarak özel sekmedeki etkinleştirme seçeneğiniz vardır. 

![Azure HDInsight güvenlik ve ağ özellikleri](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

ESP etkinleştirdiğinizde, Azure AD DS ile ilgili sık karşılaşılan hatalı yapılandırmalarını otomatik olarak algılandı ve doğrulanır.

![Azure HDInsight, Kurumsal güvenlik paketi etki alanı doğrulama](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Erkenden algılanması, küme oluşturmadan önce hataları düzeltin izin vererek zamandan tasarruf sağlar.

![Azure HDInsight Kurumsal güvenlik paketi, etki alanı doğrulaması başarısız oldu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

ESP ile HDInsight kümesi oluşturduğunuzda, aşağıdaki parametreleri belirtmeniz gerekir:

- **Kümenin yönetici kullanıcı**: kümenizin bir yönetici, eşitlenmiş Azure AD-DS'den seçin. Bu hesap zaten eşitlenmiş ve AAD DS'de kullanılabilir olması gerekir.

- **Erişim grupları küme**: kümeye eşitlemek istediğinizden, kullanıcıları Azure AD DS'de kullanılabilir güvenlik grupları. Örneğin, HiveUsers grup. Daha fazla bilgi için [bir grup oluşturun ve Azure Active Directory'de üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **LDAPS URL'si**: ldaps://contoso.com:636 örneğidir.

Aşağıdaki ekran görüntüsünde, Azure portalında başarılı yapılandırmaları gösterir:

![Azure HDInsight ESP Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

Oluşturduğunuz yönetilen kimlik olarak kullanıcı tarafından atanan yönetilen kimlik açılan listeden yeni bir küme oluştururken seçilebilir.

![Azure HDInsight ESP Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).


## <a name="next-steps"></a>Sonraki adımlar
* Hive ilkelerini yapılandırma ve Hive sorguları çalıştırmak için bkz: [Hive ilkelerini için HDInsight kümeleri ile ESP](apache-domain-joined-run-hive.md).
* ESP ile HDInsight kümelerine bağlanmak için SSH kullanarak için bkz: [HDInsight Linux, Unix ya da OS X üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).