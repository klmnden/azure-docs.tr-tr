---
title: Etki alanına katılmış Azure Hdınsight mimarisi | Microsoft Docs
description: Etki alanına katılmış HDInsight planlama hakkında bilgi edinin.
services: hdinsight
documentationcenter: ''
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 8503534031dc5774e64c58edd3e158162a5a6aee
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110465"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>HDInsight'ta Azure etki alanına katılmış Hadoop kümeleri planlama

Standart Hdınsight kümesi bir tek kullanıcı kümedir. Büyük veri iş yükleri oluşturan küçük uygulama ekiplerine sahip çoğu şirket için uygundur. Her kullanıcı farklı bir kümeye ayrılmış kendisi isteğe bağlı olan ve artık gerekli olmadığında yok. Bununla birlikte, çoğu kurum kümeleri BT ekipleri tarafından yönetilir ve birden çok uygulama paylaşımı kümeleri ekipleri model doğru taşıma başlatıldı. Bu nedenle, küme için çok kullanıcılı erişim Azure Hdınsight'ta bu büyük kuruluşlar için gereklidir.

Hdınsight en popüler kimlik sağlayıcısı--Active Directory (AD) yönetilen bir şekilde kullanır. Hdınsight ile tümleştirme tarafından [Azure Active Directory etki alanı Hizmetleri (AAD-DS)](../../active-directory-domain-services/active-directory-ds-overview.md), etki alanı kimlik bilgilerinizi kullanarak kümeleri erişebilirsiniz. Sağlanan etki alanınıza, etki alanına katılan hdınsight'ta VM'ler Hdınsight üzerinde çalışan tüm hizmetleri (Ambari, sunucu, bırakabilmenizi, Spark thrift Hive sunucu ve diğerleri) sorunsuz bir şekilde kimliği doğrulanmış kullanıcı için iş. Yöneticiler, daha sonra kümedeki kaynaklar için rol tabanlı erişim denetimi sağlamak için Apache bırakabilmenizi kullanarak güçlü yetkilendirme ilkeleri oluşturabilirsiniz.


## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight’ı Active Directory ile tümleştirin

Açık kaynak Hadoop üzerinde Kerberos kimlik doğrulaması ve güvenlik sağlamak için kullanır. Bu nedenle, etki alanına katılmış AAD DS tarafından yönetilen bir etki alanına Hdınsight küme düğümleri. Kerberos güvenlik küme üzerinde Hadoop bileşenleri için yapılandırılır. Her Hadoop bileşenleri için bir hizmet sorumlusu otomatik olarak oluşturulur. Asıl karşılık gelen bir makine etki alanına katılan her makine için de oluşturulur. Bu hizmet ve makine ilkelerini depolamak için bir kuruluş birimi (OU) etki alanı denetleyicisi (AAD-DS) içinde bu ilkeleri yerleştirildiği sağlamak için gereklidir. 

Özetlemek için bir ortam ayarlamanız gerekir:

- (AAD-DS tarafından yönetilen) bir Active Directory etki alanı
- LDAP (LDAPS) AAD DS'de etkinleştirilmiş güvenliğini sağlayın.
- Hdınsight'ın bağlantısını uygun ağ VNET-AAD-DS VNET durumda seçtiğiniz ayrı sanal ağlar için. Bir VM HDI VNET içinde bir görüş AAD DS olmalıdır VNET eşlemesi kullanarak. HDI ve AAD DS aynı VNET'i dağıttıysanız, bağlantı otomatik olarak sağlanır ve başka eylem gerekmiyor.
- Bir kuruluş birimi (OU) [AAD-DS üzerinde oluşturulmuş](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)
- İçin izinlere sahip bir hizmet hesabı:
    - Hizmet sorumluları OU'da oluşturun.
    - Makine etki alanına ve OU'da makine ilkeleri oluşturun.

Aşağıdaki ekran görüntüsünde contoso.com oluşturulan bir OU gösterir. Bazı hizmet asıl adı ve makine sorumluları ekran görüntüsünde gösterilir.

![Ou etki alanına katılmış Hdınsight kümeleri](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="different-domain-controllers-setup"></a>Farklı bir etki alanı denetleyicileri Kurulumu:
Hdınsight şu anda yalnızca AAD DS kümenin küme için Kerberise konuşur ana etki alanı denetleyicisi olarak destekler. AAD DS HDI erişimi etkinleştirmek için müşteri adayları sürece ancak, karmaşık diğer AD kurulumları da mümkündür.

- **[Azure Active Directory etki alanı Hizmetleri (AAD-DS)](../../active-directory-domain-services/active-directory-ds-overview.md)**: Bu hizmet, Windows Server Active Directory ile tamamen uyumlu olan bir yönetilen etki alanı sağlar. Microsoft yönetmek, düzeltme eki uygulama ve etki alanında bir yüksek oranda Available(HA) Kurulum izleme ilgilenir. Etki alanı denetleyicilerinin bakımını yapmak hakkında endişelenmeden kümenizi dağıtabilirsiniz. Kullanıcılar, gruplar ve parolalar, kullanıcılardan şirket kimlik bilgilerini kullanarak kümeye oturum açmak için Azure Active Directory(AAD) [tek yönlü eşitlemeyi aad'den AAD DS], etkinleştirme eşitlenir. Daha fazla bilgi için bkz: [AAD DS kullanarak etki alanına katılmış Hdınsight yapılandırmak için kümeleri nasıl](./apache-domain-joined-configure-using-azure-adds.md).
- **AD veya Iaas Vm'leri üzerinde AD şirket içi**: şirket içi varsa, AD veya daha karmaşık diğer AD kurulumları etki alanınız için AD Connect ve ardından etkinleştir AAD-AD DS'de Kiracı kullanarak AAD bu kimlikleri eşitleme. Kerberos Parola karmaları kullandığından gerekecek [AAD DS üzerinde parola karma eşitlemesini etkinleştirmek](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Federasyon ile AD Federasyon Hizmetleri (ADFS) kullanıyorsanız, ADFS altyapınızı başarısız olursa, isteğe bağlı olarak parola karması eşitlemesi yedek olarak ayarlayabilirsiniz. Daha fazla bilgi için bkz: [AAD Connect eşitleme ile parola karma eşitlemesini etkinleştirmek](../../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). Iaas Vm'leri AAD ve AAD DS olmadan tek başına, şirket içi AD veya AD kullanarak HDI kümesi etki alanına katılmak için desteklenen bir yapılandırma değildir.

## <a name="next-steps"></a>Sonraki adımlar
* [Etki alanına katılmış Hdınsight kümeleri yapılandırma](apache-domain-joined-configure-using-azure-adds.md).
* [Etki alanına katılmış Hdınsight kümeleri Hive ilkeleri yapılandırma](apache-domain-joined-run-hive.md).
* [Etki alanına katılmış Hdınsight kümelerini yönetme](apache-domain-joined-manage.md). 
