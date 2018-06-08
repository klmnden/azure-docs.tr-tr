---
title: AAD DS kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma
description: Ayarlama ve Azure Active Directory etki alanı Hizmetleri kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma konusunda bilgi edinin
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 8064940e0d0f035010a2521752d6f32f3f9ccd9f
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849845"
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Azure Active Directory etki alanı Hizmetleri kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma

Etki alanına katılmış kümeleri Hdınsight kümelerinde Çoklu kullanıcı erişimi sağlar. Böylece etki alanı kullanıcıları kümeleriyle kimliğini doğrulamak ve büyük veri işlerini çalıştırmak için etki alanı kimlik bilgilerini kullanabilirsiniz Hdınsight kümeleri etki alanına katılmış bir etki alanına bağlanır. 

Bu makalede, Azure Active Directory etki alanı Hizmetleri'ni kullanan bir etki alanına katılmış Hdınsight kümesi yapılandırma hakkında bilgi edinin.

## <a name="create-azure-adds"></a>Azure EKLER oluşturma

Bir etki alanına katılmış Hdınsight küme oluşturmadan önce Azure AD etki alanı Hizmetleri (AAD-DS) etkinleştirme bir önkoşuldur. AAD DS örneğini etkinleştirmek için bkz: [AAD DS Azure portalını kullanarak etkinleştirmek nasıl](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Yalnızca Kiracı Yöneticiler AAD DS örnek oluşturmak için gerekli ayrıcalıklara sahip. Hdınsight için varsayılan depolama alanı olarak Azure Data Lake Storage (ADLS) kullanırsanız, ardından ADLS için varsayılan Azure AD Kiracı etki alanı Hdınsight kümesi için aynı olduğundan emin olun. Kerberos ve temel kimlik doğrulama Sincde Hadoop dayanır, çok faktörlü kimlik doğrulaması kümeye erişimi olan kullanıcılar için devre dışı bırakılması gerekir.

AAD DS örneği sağlandıktan sonra doğru izinlere sahip bir hizmet hesabı (hangi AAD DS'ye senkronize edilir) aad'de oluşturmanız gerekir. Bu hizmet hesabı zaten varsa, kendi parolalarını sıfırlayabilir ve AAD DS'ye eşitlenir kadar beklemeniz gerekir (Bu sıfırlama kerberos parola karması oluşturulmasında neden olur ve AAD DS'ye eşitleme 30 dakika sürebilir). Bu hizmet hesabı aşağıdaki ayrıcalıklara sahip olmalıdır:

- Makine etki alanına ve küme oluşturma sırasında belirttiğiniz OU içinde makine sorumluları yerleştirin.
- Küme oluşturma sırasında belirttiğiniz OU içinde hizmet asıl adı oluşturun.

> [!NOTE]
> Apache Zeppelin yönetici hizmet hesabı kimlik doğrulaması için etki alanı adını kullandığından, hizmet hesabı, UPN soneki Apache düzgün bir şekilde Zeppelin için etki alanı adıyla aynı olmalıdır.

OU'lar ve bunları yönetme hakkında daha fazla bilgi için bkz: [AAD DS üzerinde bir OU oluşturmanız](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

Güvenli LDAP AAD DS yönetilen etki alanı için gereklidir. Güvenli LDAP etkinleştirmek için bkz: [güvenli LDAP (LDAPS) için bir AAD DS yapılandırmak nasıl yönetilen etki alanı](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Bir etki alanına katılmış Hdınsight kümesi oluşturma

Sonraki adım, AAD DS ve önceki bölümde oluşturduğunuz hizmet hesabı kullanarak Hdınsight kümesi oluşturmaktır.

AAD DS ve Hdınsight kümesi içinde aynı Azure sanal network(VNet) yerleştirmek daha kolaydır. Farklı sanal ağlar içinde koymak seçtiğiniz durumunda, böylece HDI VM'ler bir görüş VM'ler katılmak için etki alanı denetleyicisi için bu sanal ağlar eş gerekir. Daha fazla bilgi için bkz: [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md).

Bir etki alanına katılmış Hdınsight kümesi oluşturduğunuzda, aşağıdaki parametreleri sağlamanız gerekir:

- **Etki alanı adı**: AAD-DS ile ilişkili etki alanı adı. Örneğin, contoso.onmicrosoft.com
- **Etki alanı kullanıcı adı**: önceki bölümde oluşturduğunuz yönetilen etki alanı hizmet hesabı. Örneğin, hdiadmin@contoso.onmicrosoft.com. Bu etki alanı kullanıcısı bu Hdınsight küme yöneticisinin olacaktır.
- **Etki alanı parolası**: hizmet hesabının parolası.
- **Kuruluş birimi**: Hdınsight kümesi ile kullanmak istediğiniz kuruluş biriminin ayırt edici adı. Örneğin: OU HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Bu OU mevcut değilse, Hdınsight kümesi hizmet hesabının ayrıcalıklarını kullanarak OU oluşturmaya çalışır. (Örneğin, hizmet hesabı AAD DS Administrators grubunda ise, bir OU oluşturmak için doğru izinlere sahip, aksi takdirde gereksinim duyabileceğiniz OU ilk oluşturma ve bu OU'yu hizmet hesabı tam denetime ilk verin - görmek [bir OU oluşturmak bir AAD-DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)).

    > [!IMPORTANT]
    > Tüm DC'leri sepearated sonra OU virgülle eklemek önemlidir (örneğin OU = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com).

- **LDAPS URL**: Örneğin, ldaps://contoso.onmicrosoft.com:636

    > [!IMPORTANT]
    > Lütfen ldaps dahil tam URL'yi girin: / / ve bağlantı noktası numarası (: 636)

- **Access kullanıcı grubuna**: kümeye eşitlemek istediğinizden, kullanıcılar güvenlik grupları. Örneğin, HiveUsers. Birden çok kullanıcı gruplarını belirtmek istiyorsanız, bunları virgül ile ayırın ','.
 
Aşağıdaki ekran görüntüsü, Azure portalında yapılandırmaları gösterilmiştir:

![Azure Hdınsight etki alanına katılmış Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Sonraki adımlar
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
* Etki alanına katılmış Hdınsight kümelerine bağlanmak için SSH kullanmak için bkz: [Linux, Unix veya OS X Hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

