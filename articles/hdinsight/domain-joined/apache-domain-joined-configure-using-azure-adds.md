---
title: Azure Active Directory Domain Services - Azure HDInsight'ı kullanarak kurumsal güvenlik paketi yapılandırması
description: Ayarlama ve Azure Active Directory Domain Services'ı kullanarak bir HDInsight Kurumsal güvenlik paketi küme yapılandırma hakkında bilgi edinin.
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.custom: seodec18
ms.date: 04/23/2019
ms.openlocfilehash: e1bc99cdc089050fbfa931bbbc7b9a6a316a3a75
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/27/2019
ms.locfileid: "66240185"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Azure Active Directory Domain Services'i kullanarak bir HDInsight kümesi ile Kurumsal Güvenlik Paketi yapılandırma

Kurumsal güvenlik paketi (ESP) kümeleri, Azure HDInsight kümelerinde birden çok kullanıcı erişim sağlar. Böylece etki alanı kullanıcıları kümeleri ile kimlik doğrulaması ve büyük veri işlerini çalıştırmak için etki alanı kimlik bilgilerini kullanabilirsiniz ESP HDInsight kümeleriyle bir etki alanına bağlanır.

Bu makalede, Azure Active Directory etki alanı Hizmetleri (Azure AD DS) kullanarak bir HDInsight kümesi ile ESP yapılandırma konusunda bilgi edinin.

> [!NOTE]  
> ESP, HDInsight 3.6 ve 4.0 küme türleri için genel olarak kullanıma sunulmuştur: Apache Spark, etkileşimli, Apache Hadoop ve HBase. ESP Apache Kafka kümesi türü için Önizleme aşamasındadır.

## <a name="enable-azure-ad-ds"></a>Azure'ı etkinleştirme AD DS

> [!NOTE]  
> Kiracı yöneticileri yalnızca Azure AD DS'yi etkinleştirme için ayrıcalıklara sahip. Küme depolama, Azure Data Lake Storage (ADLS) olup olmadığını Gen1 ya da 2. nesil, devre dışı bırakmanız multi-Factor Authentication (MFA) yalnızca temel Kerberos kimlik doğrulamaları kullanarak kümeye erişmek için gereken kullanıcılar. Kullanabileceğiniz [güvenilen IP'ler](../../active-directory/authentication/howto-mfa-mfasettings.md#trusted-ips) veya [koşullu erişim](../../active-directory/conditional-access/overview.md) belirli kullanıcılar için mfa'yı devre dışı bırakmak için yalnızca zaman HDInsight küme sanal ağ IP aralığı eriştikleri. Koşullu kullanıyorsanız erişim lütfen emin olun, AD hizmet uç noktasında HDInsight VNET üzerinde etkin.
>
> Küme depolama alanı Azure Blob Storage (WASB) ise, mfa'yı devre dışı bırakmayın.

Bir HDInsight kümesi ile ESP oluşturabilmeniz için önce AzureAD DS etkinleştirme önkoşuldur. Daha fazla bilgi için [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/create-instance.md). 

Azure AD DS'yi etkinleştirildiğinde, tüm kullanıcılar ve nesneler Azure Active Directory (AAD dan), varsayılan olarak Azure AD DS'ye eşitleme başlatın. Eşitleme işlemi uzunluğu, Azure AD'de nesneleri sayısına bağlıdır. Eşitleme yüz binlerce nesne için birkaç gün sürebilir. 

HDInsight kümeleri erişmesi gereken grupları eşitlemeyi seçebilirsiniz. Bu seçenek, yalnızca belirli gruplara eşitlemenin adlandırılır *eşitleme kapsamı*. Bkz: [yapılandırma kapsamlı eşitleme Azure AD'den yönetilen etki alanınıza](../../active-directory-domain-services/scoped-synchronization.md) yönergeler için.

Güvenli LDAP etkinleştirilirken sertifika konu adında bir etki alanı adı ve konu alternatif adı yerleştirin. Örneğin, etki alanı adınızı ise *contoso100.onmicrosoft.com*, bu tam adı, sertifika konu adı ve konu diğer adı var olduğundan emin olun. Daha fazla bilgi için [güvenli LDAP yapılandırma için bir Azure AD-DS yönetilen etki alanı](../../active-directory-domain-services/configure-ldaps.md). Aşağıda, otomatik olarak imzalanan sertifika oluşturmanın bir örnektir ve etki alanı adına sahip (*contoso100.onmicrosoft.com*) konu adı hem de DnsName (konu alternatif adı):

```powershell
$lifetime=Get-Date
New-SelfSignedCertificate -Subject contoso100.onmicrosoft.com `
  -NotAfter $lifetime.AddDays(365) -KeyUsage DigitalSignature, KeyEncipherment `
  -Type SSLServerAuthentication -DnsName *.contoso100.onmicrosoft.com, contoso100.onmicrosoft.com
```

## <a name="check-azure-ad-ds-health-status"></a>Azure AD DS'yi sistem durumunu denetleyin
Azure Active Directory etki alanı Hizmetleri'niz sistem durumunu görüntülemek için **sistem durumu** altında **Yönet** kategorisi. (Çalışan) yeşil ve eşitleme tamamlandığında, Azure AD-DS durumu olduğundan emin olun.

![Azure Active Directory Domain Services durumu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="create-and-authorize-a-managed-identity"></a>Oluşturma ve yönetilen bir kimlik yetkilendirme

A **yönetilen kullanıcı tarafından atanan kimliği** basitleştirmek ve güvenli etki alanı Hizmetleri işlemler için kullanılır. HDInsight etki alanı Hizmetleri katkıda bulunan rolü için yönetilen kimlik olarak atadığınızda, okuma, oluşturma, değiştirme ve silme etki alanı Hizmetleri. OU'lar ve hizmet sorumluları oluşturma gibi belirli etki alanı hizmetleri işlemlerini HDInsight Kurumsal güvenlik paketi için gereklidir. Yönetilen kimlik herhangi bir abonelikte oluşturulabilir. Kimlikleri genel olarak yönetilen hakkında daha fazla bilgi için bkz: [kimliklerini Azure kaynakları için yönetilen](../../active-directory/managed-identities-azure-resources/overview.md). Azure HDInsight yönetilen kimlikleri çalışması hakkında daha fazla bilgi için bkz. [yönetilen Azure HDInsight kimliklerini](../hdinsight-managed-identities.md).

ESP kümeleri için zaten yoksa, kullanıcı tarafından atanan bir yönetilen kimlik oluşturun. Bkz: [Create, liste, delete veya Azure portalını kullanarak bir kullanıcı tarafından atanan yönetilen kimlik rol atama](../../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md) yönergeler için. Ardından, Ata **HDInsight etki alanı Hizmetleri katkıda bulunan** yönetilen kimliğe (DS AAD yönetici ayrıcalıkları bu rol ataması yapmak için gerekli) Azure AD DS erişim denetimine rol.

![Azure Active Directory etki alanı Hizmetleri erişim denetimi](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Atama **HDInsight etki alanı Hizmetleri katkıda bulunan** rolü, bu kimlik, (adına) erişim OU oluşturma, silme OU'ları, vb. AAD DS etki alanındaki gibi etki alanı Hizmetleri işlemleri gerçekleştirmek için uygun olduğunu sağlar.

Yönetilen kimlik oluşturup doğru rolü verilmiş olan bu yönetilen kimlik kullanabilir DS AAD yönetici ayarlayabilirsiniz. Kullanıcılar için yönetilen kimlik ayarlamak için yönetici yönetilen kimlik portalda seçin ve tıklayın **erişim denetimi (IAM)** altında **genel bakış**. Ardından, sağ tarafta atayın **yönetilen kimlik işleci** kullanıcıları veya grupları ESP HDInsight kümeleri oluşturmak istediğiniz rol. Örneğin, AAD DS yönetici bu role atayabilirsiniz **MarketingTeam** grubunun **sjmsi** aşağıdaki görüntüde gösterildiği gibi yönetilen kimliği. Bu, kuruluşunuzdaki doğru kişilere erişim ESP kümelerini oluşturmak amacıyla bu yönetilen bir kimlik kullanacak şekilde sahip olmanızı sağlar.

![HDInsight yönetilen kimlik işleci rol ataması](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-managed-identity-operator-role-assignment.png)

## <a name="networking-considerations"></a>Ağ konusunda dikkat edilmesi gerekenler

> [!NOTE]  
> Azure AD DS, bir Azure Resource Manager (ARM) tabanlı sanal ağda dağıtılmalıdır. Klasik sanal ağlar için Azure AD DS desteklenmez. Lütfen [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](../../active-directory-domain-services/active-directory-ds-getting-started-network.md) daha fazla ayrıntı için.

Azure AD DS'yi etkinleştirdikten sonra yerel bir etki alanı adı hizmeti (DNS) sunucusu AD sanal makinelerde (VM) çalışır. Azure AD DS sanal ağ (Bu özel DNS sunucuları kullanılacak sanal) yapılandırın. Doğru IP adreslerini bulmak için seçin **özellikleri** altında **Yönet** kategorisi ve IP adreslerini göz listelenen altındaki **sanal ağdaki IP adresi**.

![Yerel DNS sunucuları için IP adresleri bulun](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns.png)

Azure AD DS sanal ağı seçerek bu özel IP'ler kullanılacak DNS sunucularının yapılandırmasını değiştirmek **DNS sunucuları** altında **ayarları** kategorisi. Radyo düğmesinin yanındaki ardından **özel**, ilk IP adresini metin kutusuna girin ve tıklayın **Kaydet**. Aynı adımları kullanarak ek IP adreslerini ekleyin.

![VNET DNS yapılandırması güncelleştiriliyor](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

Hem Azure AD DS'yi örneği hem de HDInsight küme aynı Azure sanal ağında yerleştirmek daha kolaydır. Farklı sanal ağ kullanmayı planlıyorsanız, bu sanal ağları eş ve etki alanı denetleyicisi HDI Vm'lere görünür olması gerekir. Daha fazla bilgi için [sanal ağ eşlemesi](../../virtual-network/virtual-network-peering-overview.md). 

Sanal ağlar eşlendikten sonra HDInsight VNET özel bir DNS sunucusu kullanın ve Azure AD DS'yi özel IP'ler girişi DNS sunucusu adreslerini yapılandırın. Her iki Vnet'in aynı DNS sunucularını kullandığınızda, özel etki alanı adınızı doğru IP çözer ve HDInsight ' erişilebilir. Etki alanı adınızı "contoso.com" daha sonra Bu adımdan sonra ping "contoso.com" Azure AD DS IP sağa çözümlenmelidir örneğin. 

![Eşlenen sanal ağ için özel DNS sunucuları yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

Ağ güvenlik grupları (NSG) kuralları, HDInsight alt ağda kullanıyorsanız, izin vermeniz gerekir [IP'ler gerekli](../hdinsight-extend-hadoop-virtual-network.md) hem gelen hem de giden trafiği için. 

**Sınanacak** ağınızın doğru şekilde ayarlanıp ayarlanmadığını windows VM HDInsight VNET/alt ağına katılın ve etki alanı adı (çözmek için bir IP) ping ve ardından çalıştırın **Ldp.exe'yi** Azure AD DS etki alanını erişmek için. Ardından **bu windows VM onaylamak için etki alanına** istemci ve sunucu tüm gerekli RPC çağrıları başarılı. Ayrıca **nslookup** (örneğin, dış Hive meta veri deposu veya Ranger DB) kullanıyor olabileceğiniz herhangi bir dış DB veya depolama hesabınız için ağ erişimi onaylamak için.
Emin olmanız gerekir tüm [gerekli bağlantı noktaları](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd772723(v=ws.10)#communication-to-domain-controllers) AAD DS bir NSG tarafından sağlanıyorsa AAD DS alt ağ güvenlik grubu kurallarını güvenilir listededir. Bu windows etki alanına katılma VM başarılı olur sonraki adıma geçin ve ESP kümeleri oluşturma.

## <a name="create-a-hdinsight-cluster-with-esp"></a>ESP ile bir HDInsight kümesi oluşturma

Önceki adımları doğru şekilde ayarladıktan sonra sonraki adıma HDInsight kümesi ile ESP etkin oluşturmaktır. Bir HDInsight kümesi oluşturduğunuzda, Kurumsal güvenlik paketi içinde etkinleştirebilirsiniz **özel** sekmesi. Dağıtım için bir Azure Resource Manager şablonu kullanmak isterseniz, portal deneyimi kez kullanın ve sonra yeniden kullanmak için son "Özet" sayfası üzerinde önceden doldurulmuş şablonunu indirin.

> [!NOTE]  
> ESP küme adlarının ilk altı karakterinin ortamınızda benzersiz olmalıdır. Örneğin, farklı sanal ağlarda birden çok ESP kümeniz varsa, ilk altı karakter kümesi adları benzersiz garantiler bir adlandırma convension seçmeniz gerekir.

![Azure HDInsight, Kurumsal güvenlik paketi etki alanı doğrulama](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

ESP etkinleştirdiğinizde, Azure AD DS ile ilgili sık karşılaşılan hatalı yapılandırmalarını otomatik olarak algılandı ve doğrulanır. Bu hataları düzelttikten sonra sonraki adıma geçebilirsiniz: 

![Azure HDInsight Kurumsal güvenlik paketi, etki alanı doğrulaması başarısız oldu](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

ESP ile HDInsight kümesi oluşturduğunuzda, aşağıdaki parametreleri belirtmeniz gerekir:

- **Kümenin yönetici kullanıcı**: Kümenizin bir yönetici, eşitlenmiş Azure AD-DS'den seçin. Bu etki alanı hesabı eşitlendi ve Azure AD-DS kullanılabilir olması gerekir.

- **Erişim grupları küme**: Kullanıcılar, eşitleme ve Küme erişimi istediğiniz güvenlik grupları, Azure AD DS'de kullanılabilir olmalıdır. Örneğin, HiveUsers grup. Daha fazla bilgi için [bir grup oluşturun ve Azure Active Directory'de üye ekleme](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **LDAPS URL'Sİ**: Ldaps://contoso.com:636 buna bir örnektir.

Aşağıdaki ekran görüntüsünde, başarılı bir yapılandırma Azure portalında gösterir:

![Azure HDInsight ESP Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

Oluşturduğunuz yönetilen kimlik olarak kullanıcı tarafından atanan yönetilen kimlik açılan listeden yeni bir küme oluştururken seçilebilir.

![Azure HDInsight ESP Active Directory etki alanı Hizmetleri Yapılandırma](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).

## <a name="next-steps"></a>Sonraki adımlar

* Hive ilkelerini yapılandırma ve Hive sorguları çalıştırmak için bkz: [HDInsight için Apache Hive ilkelerini kümeleri ile ESP](apache-domain-joined-run-hive.md).
* ESP ile HDInsight kümelerine bağlanmak için SSH kullanarak için bkz: [HDInsight Linux, Unix ya da OS X üzerinde Linux tabanlı Apache Hadoop ile SSH kullanma](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).
