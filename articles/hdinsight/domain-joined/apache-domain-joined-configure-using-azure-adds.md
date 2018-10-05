---
title: Azure AD DS kullanarak bir HDInsight kümesi Kurumsal güvenlik paketi ile yapılandırma
description: Ayarlama ve Azure Active Directory Domain Services'ı kullanarak bir HDInsight Kurumsal güvenlik paketi küme yapılandırma hakkında bilgi edinin.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 10/3/2018
ms.openlocfilehash: 84ee24b9002237d0993a30190944dbd6dd190ac8
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/04/2018
ms.locfileid: "48784960"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services'ı kullanarak bir HDInsight kümesi Kurumsal güvenlik paketi ile yapılandırma

Kurumsal güvenlik paketi (ESP) kümeleri, Azure HDInsight kümelerinde birden çok kullanıcı erişim sağlar. Böylece etki alanı kullanıcıları kümeleri ile kimlik doğrulaması ve büyük veri işlerini çalıştırmak için etki alanı kimlik bilgilerini kullanabilirsiniz ESP HDInsight kümeleriyle bir etki alanına bağlanır. 

Bu makalede, Azure Active Directory etki alanı Hizmetleri (Azure AD DS) kullanarak bir HDInsight kümesi ile ESP yapılandırma konusunda bilgi edinin.

>[!NOTE]
>ESP HDI 3.6 için Hadoop Spark ve etkileşimli olarak GA ' dir. ESP HBase ve Kafka küme türleri için Önizleme aşamasındadır.

## <a name="enable-azure-ad-ds"></a>Azure'ı etkinleştirme AD DS

Bir HDInsight kümesi ile ESP oluşturabilmeniz için önce AzureAD DS etkinleştirme önkoşuldur. Daha fazla bilgi için [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

Azure AD DS'yi etkinleştirildiğinde, tüm kullanıcılar ve nesneler Azure Active Directory'den varsayılan olarak Azure AD DS'ye eşitleme başlatın. Eşitleme işlemi uzunluğu, Azure AD'de nesneleri sayısına bağlıdır. Eşitleme yüz binlerce nesne için birkaç gün sürebilir. 

Müşteriler, HDInsight kümeleri erişmesi gereken grupları eşitlenecek seçebilir. Bu seçenek, yalnızca belirli gruplara eşitlemenin adlandırılır *eşitleme kapsamı*. Bkz: [yapılandırma kapsamlı eşitleme Azure AD'den yönetilen etki alanınıza](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-scoped-synchronization) yönergeler için.

Azure AD DS'yi etkinleştirdikten sonra yerel bir etki alanı adı hizmeti (DNS) sunucusu AD sanal makinelerde (VM) çalışır. Azure AD DS sanal ağ (Bu özel DNS sunucuları kullanılacak sanal) yapılandırın. Doğru IP adreslerini bulmak için seçin **özellikleri** altında **Yönet** kategorisi ve IP adreslerini göz listelenen altındaki **sanal ağdaki IP adresi**.

![Yerel DNS sunucuları için IP adresleri bulun](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns.png)

Azure AD DS sanal ağı seçerek bu özel IP'ler kullanılacak DNS sunucularının yapılandırmasını değiştirmek **DNS sunucuları** altında **ayarları** kategorisi. Radyo düğmesinin yanındaki ardından **özel**, ilk IP adresini metin kutusuna girin ve tıklayın **Kaydet**. Aynı adımları kullanarak ek IP adreslerini ekleyin.

![VNET DNS yapılandırması güncelleştiriliyor](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

> [!NOTE]
> Kiracı yöneticileri yalnızca bir Azure AD DS'yi örneği oluşturmak için ayrıcalıklara sahip. Multi-Factor authentication, kümeye erişen kullanıcılar için devre dışı bırakılması gerekir.

Güvenli LDAP etkinleştirilirken sertifika konu adında bir etki alanı adı veya konu alternatif adı yerleştirin. Örneğin, etki alanı adınızı ise *contoso.com*, bu tam adı, sertifika konu adı veya konu diğer adı var olduğundan emin olun. Daha fazla bilgi için [güvenli LDAP yapılandırma için bir Azure AD-DS yönetilen etki alanı](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="check-azure-ad-ds-health-status"></a>Azure AD DS'yi sistem durumunu denetleyin

Azure Active Directory etki alanı Hizmetleri'niz sistem durumunu görüntülemek için **sistem durumu** altında **Yönet** kategorisi. (Çalışan) yeşil ve eşitleme tamamlandığında, Azure AD-DS durumu olduğundan emin olun.

![Azure Active Directory Domain Services durumu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="add-managed-identity"></a>Yönetilen kimlik Ekle

Zaten yoksa, kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Bkz: [Create, liste, delete veya Azure portalını kullanarak bir kullanıcı tarafından atanan yönetilen kimlik rol atama](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal) yönergeler için. 

Yönetilen kimlik etki alanı Hizmetleri işlemleri basitleştirmek için kullanılır. Bu kimlik, okuma, oluşturma, değiştirme ve etki alanı Hizmetleri OU'ları ve hizmet ilkeleri oluşturma gibi HDInsight Kurumsal güvenlik paketi için gerekli bir operations silme erişebilir.

Azure AD DS'yi etkinleştirdikten sonra kullanıcı tarafından atanan bir yönetilen kimlik oluşturun ve atayın **HDInsight etki alanı Hizmetleri katkıda bulunan** Azure AD DS erişim denetimine rol.

![Azure Active Directory etki alanı Hizmetleri erişim denetimi](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Yönetilen bir kimlik için atama **HDInsight etki alanı Hizmetleri katkıda bulunan** rol kimliğini Azure AD DS etki belirli etki alanı Hizmetleri işlemleri gerçekleştirmek için uygun erişimi olduğunu sağlar. Daha fazla bilgi için [Azure kaynakları için yönetilen kimlikleri nedir](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-hdinsight-cluster-with-esp"></a>ESP ile bir HDInsight kümesi oluşturma

Sonraki adım, Azure AD DS kullanarak etkin ESP ile HDInsight kümesi oluşturmaktır.

Hem Azure AD DS'yi örneği hem de HDInsight küme aynı Azure sanal ağında yerleştirmek daha kolaydır. Farklı sanal ağlarda bulunan olmaları durumunda, HDInsight VM'ler için etki alanı denetleyicisi tarafından görülebilir ve etki alanına eklenebilir böylece bu sanal ağları eşleyebilme gerekir. Daha fazla bilgi için [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md). Eşlemesi düzgün şekilde yapıldıysa test etmek için bir VM HDInsight VNET/alt ağına katılın ve etki alanı adı ping veya çalıştırmak **Ldp.exe'yi** Azure AD DS etki alanını erişmek için.

Sanal ağlar eşlendikten sonra HDInsight VNET özel bir DNS sunucusu kullanın ve Azure AD DS'yi özel IP'ler girişi DNS sunucusu adreslerini yapılandırın. Her iki Vnet'in aynı DNS sunucularını kullandığınızda, özel etki alanı adınızı doğru IP çözer ve HDInsight ' erişilebilir. Etki alanı adınızı "contoso.com" daha sonra Bu adımdan sonra ping "contoso.com" Azure AD DS IP sağa çözümlenmelidir örneğin. Bir VM daha sonra bu etki alanına katılmasını sağlayabilirsiniz.

![Eşlenen sanal ağ için özel DNS sunucuları yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

Bir HDInsight kümesi oluşturduğunuzda, Kurumsal güvenlik paketi kullanarak özel sekmedeki etkinleştirebilirsiniz.

![Azure HDInsight güvenlik ve ağ özellikleri](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

ESP etkinleştirdiğinizde, Azure AD DS ile ilgili sık karşılaşılan hatalı yapılandırmalarını otomatik olarak algılandı ve doğrulanır.

![Azure HDInsight, Kurumsal güvenlik paketi etki alanı doğrulama](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Erkenden algılanması, küme oluşturmadan önce hataları düzeltin izin vererek zamandan tasarruf sağlar.

![Azure HDInsight Kurumsal güvenlik paketi, etki alanı doğrulaması başarısız oldu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

ESP ile HDInsight kümesi oluşturduğunuzda, aşağıdaki parametreleri belirtmeniz gerekir:

- **Kümenin yönetici kullanıcı**: kümenizin bir yönetici, eşitlenmiş Azure AD-DS'den seçin. Bu hesap zaten eşitlenmiş ve Azure AD-DS kullanılabilir olması gerekir.

- **Erişim grupları küme**: kümeye eşitlemek istediğinizden, kullanıcıları Azure AD DS'de kullanılabilir güvenlik grupları. Örneğin, HiveUsers grup. Daha fazla bilgi için [bir grup oluşturun ve Azure Active Directory'de üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **LDAPS URL'si**: ldaps://contoso.com:636 örneğidir.

Aşağıdaki ekran görüntüsünde, başarılı bir yapılandırma Azure portalında gösterir:

![Azure HDInsight ESP Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

Oluşturduğunuz yönetilen kimlik olarak kullanıcı tarafından atanan yönetilen kimlik açılan listeden yeni bir küme oluştururken seçilebilir.

![Azure HDInsight ESP Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).


## <a name="next-steps"></a>Sonraki adımlar
* Hive ilkelerini yapılandırma ve Hive sorguları çalıştırmak için bkz: [Hive ilkelerini için HDInsight kümeleri ile ESP](apache-domain-joined-run-hive.md).
* ESP ile HDInsight kümelerine bağlanmak için SSH kullanarak için bkz: [HDInsight Linux, Unix ya da OS X üzerinde Linux tabanlı Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
