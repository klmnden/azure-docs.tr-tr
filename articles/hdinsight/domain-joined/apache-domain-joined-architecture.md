---
title: "Etki alanına katılmış Azure Hdınsight mimarisi | Microsoft Docs"
description: "Etki alanına katılmış HDInsight planlama hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: bhanupr
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
ms.date: 12/14/2017
ms.author: bprakash
ms.openlocfilehash: 5285199d22528ed6b9fa3b7dbc85e382e7b28569
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2018
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>HDInsight'ta Azure etki alanına katılmış Hadoop kümeleri planlama

Standart Hdınsight kümesi bir tek kullanıcı kümedir. Büyük veri iş yükleri oluşturan küçük uygulama ekiplerine sahip çoğu şirket için uygundur. Hadoop elde edilen popülerliği gibi pek çok kurum kümeleri BT ekipleri tarafından yönetilir ve birden çok uygulama paylaşımı kümeleri ekipleri model doğru taşıma başlatıldı. Bu nedenle çok kullanıcılı kümeler gibi işlevler, en çok talep gören Azure HDInsight işlevlerinden biri.

Kendi çok kullanıcılı kimlik doğrulama ve yetkilendirme oluşturmak yerine, Hdınsight en popüler kimlik sağlayıcısı--Active Directory (AD) kullanır. AD güçlü güvenlik işlevindeki hdınsight'ta çok kullanıcılı kimlik doğrulamasını yönetmek için kullanılabilir. Hdınsight AD ile tümleştirme, AD kimlik bilgilerinizi kullanarak kümeleriyle iletişim kurabilir. Hdınsight'ta VM'ler için AD etki alanına katılan ve Hdınsight bir AD kullanıcısının yerel bir Hadoop kullanıcıya Hdınsight üzerinde çalışan tüm hizmetleri nasıl eşlendiğini (Ambari, sunucu, bırakabilmenizi, Spark thrift Hive sunucu ve diğerleri) kimliği doğrulanmış kullanıcı için sorunsuz bir şekilde çalışır. Yöneticiler, daha sonra hdınsight'ta kaynaklar için rol tabanlı erişim denetimi sağlamak için Apache bırakabilmenizi kullanarak güçlü yetkilendirme ilkeleri oluşturabilirsiniz.


## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight’ı Active Directory ile tümleştirin

Hdınsight Active Directory ile tümleştirme, AD etki alanı için etki alanına katılan Hdınsight küme düğümleri. Kerberos güvenlik küme üzerinde Hadoop bileşenleri için yapılandırılır. Her Hadoop bileşenleri için bir hizmet sorumlusu üzerinde Active Directory oluşturulur. Asıl karşılık gelen bir makine etki alanına katılan her makine için de oluşturulur. Bu hizmet asıl adı ve makine ilkeleri, Active Directory daraltabilir. Sonuç olarak, bir kuruluş birimi (OU) Active Directory'de bu ilkeleri yerleştirildiği, sağlamak için gereklidir. 

Özetlemek için bir ortam ayarlamanız:

- Bir Active Directory etki alanı denetleyicisi ile yapılandırılmış LDAPS.
- Active Directory etki alanı denetleyicinizi Hdınsight'ın sanal ağ bağlantısını.
- Bir kuruluş Active Directory'de oluşturulan birimi.
- İçin izinlere sahip bir hizmet hesabı:

    - Hizmet sorumluları OU'da oluşturun.
    - Makine etki alanına katın ve OU'da makine ilkeleri oluşturun.

Aşağıdaki ekran görüntüsünde contoso.com oluşturulan bir OU gösterir. Bazı hizmet asıl adı ve makine sorumluları ekran görüntüsünde gösterilir.

![Ou etki alanına katılmış Hdınsight kümeleri](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="two-ways-of-bringing-your-own-active-directory-domain-controllers"></a>Kendi Active Directory etki alanı denetleyicileri getiren iki yolu

Etki alanına katılmış Hdınsight kümeleri oluşturmak için Active Directory etki alanı denetleyicileri getirebilirsiniz iki yolu vardır. 

- **Azure Active Directory etki alanı Hizmetleri**: Bu hizmet, Windows Server Active Directory ile tamamen uyumlu olan bir yönetilen Active Directory etki alanı sağlar. Microsoft, geçen yönetme, düzeltme eki uygulama ve AD etki alanı izleme dikkat edin. Etki alanı denetleyicilerinin bakımını yapmak hakkında endişelenmeden kümenizi dağıtabilirsiniz. Kullanıcılar, gruplar ve parolalar, Azure Active Directory'den şirket kimlik bilgilerini kullanarak kümeye oturum açmalarını etkinleştirme eşitlenir. Daha fazla bilgi için bkz: [Azure Active Directory etki alanı Hizmetleri kullanarak yapılandırma etki alanına katılmış Hdınsight kümelerini](./apache-domain-joined-configure-using-azure-adds.md).

- **Azure Iaas Vm'leri üzerinde Active Directory**: Bu seçenekte, dağıtma ve Azure Iaas Vm'leri kendi Windows Server Active Directory etki alanında yönetme. Daha fazla bilgi için bkz: [yapılandırma etki alanına katılmış korumalı alan ortamıdır](./apache-domain-joined-configure.md).

## <a name="next-steps"></a>Sonraki adımlar
* Etki alanına katılmış bir HDInsight kümesi yapılandırmak için bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](apache-domain-joined-configure.md).
* Etki alanına katılmış HDInsight kümelerini yönetmek için bkz. [Etki alanına katılmış HDInsight kümelerini yönetme](apache-domain-joined-manage.md).
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](apache-domain-joined-run-hive.md).
* Etki alanına katılmış Hdınsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için bkz: [Hdınsight ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md).
