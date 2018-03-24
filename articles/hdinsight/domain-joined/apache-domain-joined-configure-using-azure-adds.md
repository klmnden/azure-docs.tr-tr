---
title: Azure Active Directory etki alanı Hizmetleri - Azure kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma | Microsoft Docs
description: Ayarlama ve Azure Active Directory etki alanı Hizmetleri kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma konusunda bilgi edinin
services: hdinsight
documentationcenter: ''
author: bprakash
manager: jhubbard
editor: cgronlun
tags: ''
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/20/2018
ms.author: bhanupr
ms.openlocfilehash: ae7ccaf3d167176a1fc6015e84b0eb023da945d5
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/23/2018
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Azure Active Directory etki alanı Hizmetleri kullanarak etki alanına katılmış Hdınsight kümelerini yapılandırma

Etki alanına katılmış kümeleri çok kullanıcılı Kurumsal Hdınsight'ta güvenlik özellikleri sağlar. Böylece etki alanı kullanıcıları kümeleriyle kimliğini doğrulamak ve büyük veri işlerini çalıştırmak için etki alanı kimlik bilgilerini kullanabilirsiniz active directory etki alanları için etki alanına katılmış Hdınsight kümelerine bağlanır. 

Bu makalede, Azure Active Directory etki alanı Hizmetleri'ni kullanan bir etki alanına katılmış Hdınsight kümesi yapılandırma hakkında bilgi edinin.

> [!NOTE]
> Azure Iaas Vm'leri üzerinde Active Directory artık desteklenmiyor.

## <a name="create-azure-adds"></a>Azure EKLER oluşturma

Hdınsight kümesi oluşturmadan önce bir Azure AD DS oluşturmanız gerekir. Bir Azure EKLER oluşturmak için bkz: [etkinleştirmek Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Yalnızca Kiracı yöneticileri, etki alanı hizmetleri oluşturmak için gerekli ayrıcalıklara sahiptir. Hdınsight için varsayılan depolama alanı olarak Azure Data Lake Storage (ADLS) kullanırsanız, ardından ADLS için varsayılan Azure AD Kiracı etki alanı Hdınsight kümesi için aynı olduğundan emin olun. Bunun için Azure Data Lake Store ile çalışmak için ayarladığınız çok faktörlü kimlik doğrulaması kümeye erişimi olan kullanıcılar için devre dışı bırakılması gerekir.

Etki alanı hizmeti sağlandıktan sonra bir hizmet hesabı oluşturmanız gerekir **Azure AD DC Yöneticiler** Hdınsight kümesi oluşturmak için Grup. Hizmet hesabı üzerinde Azure AD genel yönetici olması gerekir.

Güvenli LDAP Azure AD etki alanı Hizmetleri yönetilen etki alanı için etkinleştirmeniz gerekir. Güvenli LDAP etkinleştirmek için bkz: [yapılandırma güvenli LDAP (LDAPS) bir Azure AD etki alanı Hizmetleri yönetilen etki alanı](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Bir etki alanına katılmış Hdınsight kümesi oluşturma

Sonraki adım, AAD DS ve önceki bölümde oluşturduğunuz hizmet hesabı kullanarak Hdınsight kümesi oluşturmaktır.

Azure AD etki alanı hizmeti ve Hdınsight kümesi içinde aynı Azure sanal network(VNet) yerleştirmek daha kolaydır. Farklı sanal ağlar içinde olduğu durumlarda her iki sanal ağlar eş gerekir. Daha fazla bilgi için bkz: [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md).

Bir etki alanına katılmış Hdınsight kümesi oluşturduğunuzda, aşağıdaki parametreleri sağlamanız gerekir:

- **Etki alanı adı**: Azure AD DS ile ilişkili etki alanı adı. Örneğin, contoso.onmicrosoft.com
- **Etki alanı kullanıcı adı**: önceki bölümde oluşturduğunuz Azure AD DC Yöneticiler grubunda hizmet hesabı. Örneğin, hdiadmin@contoso.onmicrosoft.com. Bu etki alanı kullanıcısı, bu etki alanına katılmış Hdınsight kümesinin yöneticisidir.
- **Etki alanı parolası**: hizmet hesabının parolası.
- **Kuruluş birimi**: Hdınsight kümesi ile kullanmak istediğiniz kuruluş biriminin ayırt edici adı. Örneğin: OU HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Bu OU mevcut değilse, Hdınsight kümesi bu OU oluşturmaya çalışır. 
- **LDAPS URL**: Örneğin, ldaps://contoso.onmicrosoft.com:636
- **Access kullanıcı grubuna**: kümeye eşitlemek istediğinizden, kullanıcılar güvenlik grupları. Örneğin, HiveUsers. Birden çok kullanıcı gruplarını belirtmek istiyorsanız, bunları virgül ile ayırın ','.
 
> [!NOTE]
> Apache Zeppelin yönetici hizmet hesabı kimlik doğrulaması için etki alanı adını kullandığından, hizmet hesabı, UPN soneki Apache düzgün bir şekilde Zeppelin için etki alanı adıyla aynı olmalıdır.
 
Aşağıdaki ekran görüntüsü, Azure portalında yapılandırmaları gösterilmiştir:

![Azure Hdınsight etki alanına katılmış Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Sonraki adımlar
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
* Etki alanına katılmış Hdınsight kümelerine bağlanmak için SSH kullanmak için bkz: [Linux, Unix veya OS X Hdınsight'ta Linux tabanlı Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

