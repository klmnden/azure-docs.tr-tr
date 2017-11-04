---
title: "Etki alanına katılmış Azure Hdınsight mimarisi | Microsoft Docs"
description: "Etki alanına katılmış HDInsight planlama hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/21/2017
ms.author: saurinsh
ms.openlocfilehash: 9deb5e4dbd925c4fdc523d8d21afbcfbf85947b8
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>HDInsight'ta Azure etki alanına katılmış Hadoop kümeleri planlama

Geleneksel Hadoop tek kullanıcılı bir kümedir. Büyük veri iş yükleri oluşturan küçük uygulama ekiplerine sahip çoğu şirket için uygundur. Hadoop'un popülerliği arttıkça çoğu kurum, kümelerin BT ekipleri tarafından yönetildiği ve birden çok uygulama ekibinin ortak kümelerde çalıştığı bir modele geçiş yapıyor. Bu nedenle çok kullanıcılı kümeler gibi işlevler, en çok talep gören Azure HDInsight işlevlerinden biri.

Kendi çok kullanıcılı kimlik doğrulama ve yetkilendirme oluşturmak yerine, Hdınsight en popüler kimlik sağlayıcısı--Active Directory (AD) kullanır. AD güçlü güvenlik işlevindeki hdınsight'ta çok kullanıcılı yetkilendirmeyi yönetmek için kullanılabilir. Hdınsight AD ile tümleştirme, AD kimlik bilgilerinizi kullanarak kümeleriyle iletişim kurabilir. Hdınsight Hdınsight üzerinde çalışan tüm hizmetleri yerel bir Hadoop kullanıcı için bir AD kullanıcısının eşler (Ambari, sunucu, bırakabilmenizi, Spark thrift Hive sunucu ve diğerleri) kimliği doğrulanmış kullanıcı için sorunsuz bir şekilde çalışır.

## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight’ı Active Directory ile tümleştirin

Hdınsight Active Directory ile tümleştirildiğinde, AD etki alanı için etki alanına katılan Hdınsight küme düğümleri. Hdınsight küme üzerinde çalışan Hadoop Hizmetleri için hizmet asıl adı oluşturur ve bunları etki alanında belirtilen kuruluş birimi (OU) içinde yerleştirir. Hdınsight geriye doğru DNS eşlemelerini etki alanına katılan düğümlerin IP adresleri etki alanında da oluşturur.

Active Directory için iki dağıtım seçenekleri şunlardır:
* **[Azure Active Directory etki alanı Hizmetleri](../../active-directory-domain-services/active-directory-ds-overview.md):** bu hizmet, Windows Server Active Directory ile tamamen uyumlu olan yönetilen Active Directory etki alanı sağlar. Microsoft, geçen yönetme, düzeltme eki uygulama ve AD etki alanı izleme dikkat edin. Etki alanı denetleyicilerinin bakımını yapmak hakkında endişelenmeden kümenizi dağıtabilirsiniz. Kullanıcılar, gruplar ve parolalar, Azure Active Directory'den şirket kimlik bilgilerini kullanarak kümeye oturum açmalarını etkinleştirme eşitlenir.

* **Azure Iaas Vm'leri üzerinde Windows Server Active Directory etki alanı:** Bu seçenekte, dağıtma ve Azure Iaas Vm'leri kendi Windows Server Active Directory etki alanında yönetme. 

Bu kuruluma birden fazla mimari kullanarak ulaşabilirsiniz. Aşağıdaki mimarilerin arasından seçim yapabilirsiniz.


### <a name="hdinsight-integrated-with-an-azure-ad-domain-services-managed-ad-domain"></a>Bir Azure AD etki alanı Hizmetleri ile tümleşik Hdınsight AD etki alanı yönetilen
Dağıtabilmeniz için bir [Azure Active Directory etki alanı Hizmetleri](../../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS) yönetilen etki alanı. Azure AD DS yönetilen bir AD etki alanı yönetilen, güncelleştirilen ve Microsoft tarafından izlenen Azure sağlar. Yüksek kullanılabilirlik için iki etki alanı denetleyicisi oluşturur ve DNS hizmetleri içerir. Bu gibi durumlarda, Hdınsight kümesi sonra bu yönetilen etki alanı ile tümleştirebilirsiniz. Bu dağıtım seçeneği ile yönetme, düzeltme eki uygulama, güncelleştirme ve etki alanı denetleyicisinin izlenmesini hakkında endişelenmeniz gerekmez.

![HDInsight küme topolojisini etki alanına katma](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Azure AD etki alanı Hizmetleri ile tümleştirme için Önkoşullar:

* [Azure AD etki alanı hizmetleri sağlama yönetilen etki alanı](../../active-directory-domain-services/active-directory-ds-getting-started.md).
* Oluşturma bir [kuruluş birimi](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md), içinde Hdınsight kümesi VM'ler yerleştirin ve küme tarafından kullanılan hizmet asıl adı.
* Kurulum [LDAPS](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md), Azure AD DS yapılandırdığınızda. LDAPS ayarlamak için kullanılan sertifikanın bir genel sertifika yetkilisi (otomatik imzalı bir sertifika değil) tarafından verilmiş olması gerekir.
* Geriye doğru DNS bölgeleri Hdınsight alt ağ (örneğin, önceki resimde 10.2.0.0/24) IP adres aralığı yönetilen etki alanını oluşturun.
* Yapılandırma [NTLM ve Kerberos kimlik doğrulaması için gerekli parola karma eşitlemesi](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) Azure AD DS yönetilen etki alanı için Azure AD'den.
* Hizmet hesabı veya kullanıcı hesabı gereklidir. Bu hesabı HDInsight kümesini oluşturmak için kullanın. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Azure AD etki alanına katma izinleri


### <a name="hdinsight-integrated-with-windows-server-ad-running-on-azure-iaas"></a>Hdınsight ile Windows Server AD tümleşik Azure Iaas'da çalışan

Bir (veya birden çok) sanal makinelerle (VM'ler) Windows Server Active Directory etki alanı Hizmetleri rolünü dağıtmak ve bunları etki alanı denetleyicileri olarak yükseltin. Hdınsight kümesi olarak aynı sanal ağda resource manager dağıtım modelini kullanarak bu etki alanı denetleyicisi VM'ler dağıtılabilir. Etki alanı denetleyicileri farklı bir sanal ağ dağıttıysanız kullanarak bu sanal ağlar arası ihtiyacınız [VNet-VNet eşlemesi](../../virtual-network/virtual-network-create-peering.md). 

[Daha fazla bilgi - Windows Server Active Directory Azure vm'lerinde dağıtma](../../active-directory/virtual-networks-windows-server-active-directory-virtual-machines.md)

![HDInsight küme topolojisini etki alanına katma](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> Bu mimaride, Azure Data Lake Store'u HDInsight kümesi ile kullanamazsınız.


Azure vm'lerinde Windows Server Active Directory ile tümleştirme için Önkoşullar:

* HDInsight kümesi VM'lerini ve küme tarafından kullanılan hizmet sorumlularını yerleştireceğiniz bir [kuruluş birimi](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır.
* [Hafif Dizin Erişim protokolleri](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) ayarlanmalıdır AD ile iletişim kurmak için. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDInsight alt ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24).
* Hizmet hesabı veya kullanıcı hesabı gereklidir. Bu hesabı HDInsight kümesini oluşturmak için kullanın. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Active Directory etki alanına katma izinleri


## <a name="next-steps"></a>Sonraki adımlar
* Etki alanına katılmış bir HDInsight kümesi yapılandırmak için bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md).
* Etki alanına katılmış HDInsight kümelerini yönetmek için bkz. [Etki alanına katılmış HDInsight kümelerini yönetme](apache-domain-joined-manage.md).
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
* Etki alanına katılmış Hdınsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).
