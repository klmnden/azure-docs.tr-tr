---
title: Azure AD DS kullanarak etki alanına katılmış bir HDInsight kümesi yapılandırma
description: Ayarlama ve Azure Active Directory Domain Services'ı kullanarak bir etki alanına katılmış HDInsight kümesi yapılandırma hakkında bilgi edinin
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 07/17/2018
ms.openlocfilehash: 17924b0a00f4605d41492768b0124c583664aca6
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2018
ms.locfileid: "43042150"
---
# <a name="configure-a-domain-joined-hdinsight-cluster-by-using-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services'ı kullanarak bir etki alanına katılmış HDInsight kümesi yapılandırma

Etki alanına katılmış kümeleri, Azure HDInsight kümelerinde çok kullanıcılı erişim sağlar. Böylece etki alanı kullanıcıları, etki alanı kimlik kümeleri ile kimlik doğrulaması ve büyük veri işlerini çalıştırmak için kullanabilirsiniz, etki alanına katılmış HDInsight kümeleri bir etki alanına bağlanır. 

Bu makalede, Azure Active Directory etki alanı Hizmetleri (Azure AD DS) kullanarak bir etki alanına katılmış HDInsight kümesi yapılandırma konusunda bilgi edinin.

## <a name="enable-azure-ad-ds"></a>Azure AD DS'yi etkinleştirme

Bir etki alanına katılmış HDInsight kümesi oluşturmadan önce Azure AD DS'yi etkinleştirme önkoşuldur. Daha fazla bilgi için [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Kiracı yöneticileri yalnızca bir Azure AD DS örneği oluşturmak için ayrıcalıklara sahip. HDInsight için varsayılan depolama alanı olarak Azure Data Lake depolama Gen1 kullanırsanız, varsayılan Azure AD kiracısı için Data Lake depolama Gen1 HDInsight kümesi için etki alanı olarak aynı olduğundan emin olun. Hadoop, Kerberos ve temel kimlik doğrulaması kullandığından, multi-Factor authentication kümeye erişen kullanıcılar için devre dışı bırakılması gerekir.

Azure AD DS örneği sağladıktan sonra doğru izinler ile bir Azure Active Directory'de (Azure AD) hizmet hesabı oluşturun. Bu hizmet hesabı zaten varsa, kendi parolalarını sıfırlayabilir ve Azure AD DS'ye eşitlenene kadar bekleyin. Bu sıfırlama Kerberos Parola Karması oluşturulmasında neden olur ve Azure AD DS'ye eşitlemek için en fazla 30 dakika sürebilir. 

Hizmet hesabı aşağıdaki ayrıcalıklara gerekir:

- Makine etki alanına ve makine sorumluları, küme oluşturma sırasında belirttiğiniz OU'daki yerleştirin.
- Küme oluşturma sırasında belirttiğiniz OU içinde hizmet sorumluları oluşturma.

> [!NOTE]
> Apache Zeppelin yönetici hizmet hesabı, hizmet hesabı kimlik doğrulaması için etki alanı adını kullandığından *gerekir* Apache Zeppelin'ın düzgün çalışması için UPN soneki olarak aynı etki alanı adına sahip.

OU'lar ve bunları yönetme hakkında daha fazla bilgi için bkz: [Azure AD DS yönetilen etki alanında OU oluşturma](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

Güvenli LDAP için bir Azure AD DS yönetilen etki alanı adıdır. Daha fazla bilgi için [bir Azure AD DS için güvenli LDAP yapılandırma yönetilen etki alanı](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Bir etki alanına katılmış HDInsight kümesi oluşturma

Sonraki adım, Azure AD DS ve önceki bölümde oluşturduğunuz hizmet hesabını kullanarak HDInsight kümesi oluşturmaktır.

Hem Azure AD DS örneği ve HDInsight kümesi aynı Azure sanal ağında yerleştirmek daha kolaydır. Farklı sanal ağlarda bulunan koymak seçerseniz, böylece HDInsight Vm'leri görebilmesi Vm'leri katılmak için etki alanı denetleyicisi için bu sanal ağları eşleyebilme gerekir. Daha fazla bilgi için [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md).

Bir etki alanına katılmış HDInsight kümesi oluşturduğunuzda, aşağıdaki parametreleri belirtmeniz gerekir:

- **Etki alanı adı**: Azure AD DS ile ilişkili etki alanı adı. Contoso.onmicrosoft.com buna bir örnektir.

- **Etki alanı kullanıcı adı**: Yönetilen hizmet hesabı Azure EKLER DC, önceki bölümde oluşturduğunuz etki alanı. hdiadmin@contoso.onmicrosoft.com bunun bir örneğidir. Bu etki alanı kullanıcısı bu HDInsight kümesinin yönetici olacaktır.

- **Etki alanı parolası**: hizmet hesabının parolası.

- **Kuruluş birimi**: HDInsight kümesi ile kullanmak istediğiniz kuruluş biriminin ayırt edici adı. OU örneğidir HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com. Bu OU'ya mevcut değilse, HDInsight Küme hizmeti hesabının ayrıcalıkları kullanarak OU oluşturmaya çalışır. Örneğin, Azure AD DS Yöneticileri grubuna hizmet hesabı ise bir OU oluşturmak için doğru izinlere sahip. Aksi takdirde, OU ilk oluşturun ve bu OU'yu hizmet hesabı tam denetime vermek gerekir. Daha fazla bilgi için [Azure AD DS yönetilen etki alanında OU oluşturma](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).

    > [!IMPORTANT]
    > Tüm sonra OU virgülle DC'leri içerir (örneğin OU HDInsightOU, DC = contoso, DC = onmicrosoft, DC = com).

- **LDAPS URL'si**: ldaps://contoso.onmicrosoft.com:636 örneğidir.

    > [!IMPORTANT]
    > Tam URL'yi girin dahil olmak üzere "ldaps: / /" ve bağlantı noktası numarası (: 636).

- **Erişim kullanıcı grubu**: kümeye eşitlemek istediğinizden, kullanıcılar güvenlik grupları. Örneğin, HiveUsers. Birden çok kullanıcı gruplarını belirtmek istiyorsanız, bunları noktalı virgülle ayırın. ';'. Gruplara sağlama önce dizinin mevcut olması gerekir. Daha fazla bilgi için [bir grup oluşturun ve Azure Active Directory'de üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Grup mevcut değilse bir hata oluşur: "Grup HiveUsers bulunamadı Active Directory'de."

Aşağıdaki ekran görüntüsünde, Azure portalında yapılandırmaları gösterilmiştir:

   ![Azure HDInsight etki alanına katılmış Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Sonraki adımlar
* Hive ilkelerini yapılandırma ve Hive sorguları çalıştırmak için bkz: [etki alanına katılmış HDInsight kümeleri için Hive ilkelerini](apache-domain-joined-run-hive.md).
* Etki alanına katılmış HDInsight kümelerine bağlanmak için SSH kullanmak için bkz: [HDInsight Linux, Unix ya da OS X üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

