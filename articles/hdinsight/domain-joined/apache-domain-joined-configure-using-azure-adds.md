---
title: Azure AD DS kullanarak bir HDInsight kümesi Kurumsal güvenlik paketi ile yapılandırma
description: Ayarlama ve Azure Active Directory Domain Services'ı kullanarak bir HDInsight Kurumsal güvenlik paketi küme yapılandırma hakkında bilgi edinin
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a5b377381fd540c2a9f1d85e0cb7edce32c2dae8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968382"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services'ı kullanarak bir HDInsight kümesi Kurumsal güvenlik paketi ile yapılandırma

Kurumsal güvenlik paketi (ESP) kümeleri, Azure HDInsight kümelerinde birden çok kullanıcı erişim sağlar. Böylece etki alanı kullanıcıları kümeleri ile kimlik doğrulaması ve büyük veri işlerini çalıştırmak için etki alanı kimlik bilgilerini kullanabilirsiniz ESP HDInsight kümeleriyle bir etki alanına bağlanır. 

Bu makalede, Azure Active Directory etki alanı Hizmetleri (Azure AD DS) kullanarak bir HDInsight kümesi ile ESP yapılandırma konusunda bilgi edinin.

>[!NOTE]
>ESP HDI 3.6 + Spark, etkileşimli ve Hadoop için kullanıma sunulmuştur. ESP HBase küme türleri için Önizleme aşamasındadır.


## <a name="enable-azure-ad-ds"></a>Azure'ı etkinleştirme AD DS

Bir HDInsight kümesi ile ESP oluşturabilmeniz için önce Azure AD DS'yi etkinleştirme önkoşuldur. Daha fazla bilgi için [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Kiracı yöneticileri yalnızca bir Azure AD DS'yi örneği oluşturmak için ayrıcalıklara sahip. HDInsight için varsayılan depolama alanı olarak Azure Data Lake depolama Gen1 kullanırsanız, varsayılan Azure AD kiracısı için Data Lake depolama Gen1 HDInsight kümesi için etki alanı olarak aynı olduğundan emin olun. Hadoop, Kerberos ve temel kimlik doğrulaması kullandığından, multi-Factor authentication kümeye erişen kullanıcılar için devre dışı bırakılması gerekir.

Güvenli LDAP için bir Azure AD DS'yi yönetilen etki alanı adıdır. LDAPS etkinleştirirken, sertifika konu adında bir etki alanı adı veya konu alternatif adı yerleştirin. Daha fazla bilgi için [güvenli LDAP yapılandırma için bir Azure AD-DS yönetilen etki alanı](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="add-managed-identity"></a>Yönetilen kimlik Ekle

Azure AD DS'yi etkinleştirdikten sonra yönetilen bir kimlik oluşturmak ve atamak için **HDInsight etki alanı Hizmetleri katkıda bulunan** Azure AD DS erişim denetimine rol.

![Azure Active Directory etki alanı Hizmetleri erişim denetimi](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Daha fazla bilgi için [Azure kaynakları için yönetilen kimlikleri nedir](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-hdinsight-cluster-with-esp"></a>ESP ile bir HDInsight kümesi oluşturma

Sonraki adım, Azure AD DS kullanarak etkin ESP ile HDInsight kümesi oluşturmaktır.

Hem Azure AD DS'yi örneği hem de HDInsight küme aynı Azure sanal ağında yerleştirmek daha kolaydır. Farklı sanal ağlarda bulunan koymak seçerseniz, böylece HDInsight Vm'leri görebilmesi Vm'leri katılmak için etki alanı denetleyicisi için bu sanal ağları eşleyebilme gerekir. Daha fazla bilgi için [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md).

Bir HDInsight kümesi oluşturduğunuzda, Azure AD DS ile kümenize bağlanmak Kurumsal güvenlik paketi etkinleştirme seçeneğiniz vardır. 

![Azure HDInsight güvenlik ve ağ özellikleri](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

ESP etkinleştirdiğinizde, Azure AD DS ile ilgili sık karşılaşılan hatalı yapılandırmalarını otomatik olarak algılandı ve doğrulanır.

![Azure HDInsight, Kurumsal güvenlik paketi etki alanı doğrulama](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Erkenden algılanması, küme oluşturmadan önce hataları düzeltin izin vererek zamandan tasarruf sağlar.

![Azure HDInsight Kurumsal güvenlik paketi, etki alanı doğrulaması başarısız oldu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

ESP ile HDInsight kümesi oluşturduğunuzda, aşağıdaki parametreleri belirtmeniz gerekir:

- **Kümenin yönetici kullanıcı**: kümenizin bir yönetici, eşitlenmiş Azure AD-DS'den seçin.

- **Erişim grupları küme**: güvenlik grupları kümeye eşitlemek istediğiniz kullanıcıları eşitlendi ve Azure AD-DS kullanılabilir olmalıdır. Örneğin, HiveUsers. Birden çok kullanıcı gruplarını belirtmek istiyorsanız, bunları noktalı virgülle ayırın. ';'. Gruplara sağlama önce dizinin mevcut olması gerekir. Daha fazla bilgi için [bir grup oluşturun ve Azure Active Directory'de üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Grup mevcut değilse bir hata oluşur: "Grup HiveUsers bulunamadı Active Directory'de."

- **LDAPS URL'si**: ldaps://contoso.onmicrosoft.com:636 örneğidir.

    > [!IMPORTANT]
    > Tam URL'yi girin dahil olmak üzere "ldaps: / /" ve bağlantı noktası numarası (: 636).

Aşağıdaki ekran görüntüsünde, Azure portalında yapılandırmaları gösterilmiştir:

   ![Azure HDInsight ESP Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Sonraki adımlar
* Hive ilkelerini yapılandırma ve Hive sorguları çalıştırmak için bkz: [Hive ilkelerini için HDInsight kümeleri ile ESP](apache-domain-joined-run-hive.md).
* ESP ile HDInsight kümelerine bağlanmak için SSH kullanarak için bkz: [HDInsight Linux, Unix ya da OS X üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

