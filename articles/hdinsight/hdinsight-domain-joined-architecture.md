---
title: "Etki alanına katılmış Azure HDInsight mimarisi | Microsoft Docs"
description: "Etki alanına katılmış HDInsight planlama hakkında bilgi edinin."
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 02/03/2017
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: 4ba44128ec19d3937643ac934ca3e787cb9819a3
ms.openlocfilehash: 690ba97d2b0634548a0ec424d441ad34d70667b9
ms.lasthandoff: 02/27/2017


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>HDInsight'ta Azure etki alanına katılmış Hadoop kümeleri planlama

Geleneksel Hadoop tek kullanıcılı bir kümedir. Büyük veri iş yükleri oluşturan küçük uygulama ekiplerine sahip çoğu şirket için uygundur. Hadoop'un popülerliği arttıkça çoğu kurum, kümelerin BT ekipleri tarafından yönetildiği ve birden çok uygulama ekibinin ortak kümelerde çalıştığı bir modele geçiş yapıyor. Bu nedenle çok kullanıcılı kümeler gibi işlevler, en çok talep gören Azure HDInsight işlevlerinden biri.

HDInsight kendi çok kullanıcılı kimlik doğrulaması ve yetkilendirmesini oluşturmak yerine en popüler kimlik sağlayıcısını kullanır: Azure Active Directory (Azure AD). Azure AD'deki güçlü güvenlik işlevleri, HDInsight'ta çoklu kullanıcı yetkilendirmesini yönetmek için kullanılabilir. HDInsight'ı Azure AD ile tümleştirdiğinizde Azure AD kimlik bilgilerinizi kullanarak kümelerinizle iletişim kurabilirsiniz. Bir Azure AD kullanıcısı HDInsight tarafından yerel bir Hadoop kullanıcısıyla eşlenir ve bu sayede HDInsight'ta çalışan tüm hizmetler (Ambari, Hive sunucusu, Ranger, Spark Thrift sunucusu ve diğerleri) kimliği doğrulanan kullanıcı için sorunsuz bir şekilde çalışır.

## <a name="integrate-hdinsight-with-azure-ad"></a>HDInsight'ı Azure AD ile tümleştirme

HDInsight'ı Azure AD ile tümleştirdiğinizde HDInsight küme düğümleri Azure AD etki alanına katılır. HDInsight, kümede çalışan Hadoop hizmetleri için hizmet sorumluları oluşturur ve bunları Azure AD'deki belirli bir kuruluş birimine (OU) yerleştirir. HDInsight, etki alanına katılmış düğümlerin IP adresleri için Azure AD etki alanında ters DNS eşlemeleri de oluşturur.

Bu kuruluma birden fazla mimari kullanarak ulaşabilirsiniz. Aşağıdaki mimarilerin arasından seçim yapabilirsiniz.

**Azure IaaS üzerinde çalışan Azure AD ile tümleştirilen HDInsight**

HDInsight'ı Azure AD ile tümleştirmek için kullanabileceğiniz en basit mimari budur. Azure AD etki alanı denetleyicisi Azure'daki bir (veya birden çok) sanal makinede (VM) çalışır. Genellikle bu VM'ler bir sanal ağ içerisindedir. HDInsight kümesi için başka bir sanal ağ kurarsınız. HDInsight'ın Azure AD'yi doğrudan görebilmesi için [Sanal Ağlar Arası Eşleme](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md) kullanarak bu sanal ağları eşlemeniz gerekir.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> Bu mimaride, Azure Data Lake Store'u HDInsight kümesi ile kullanamazsınız.


Azure AD önkoşulları:

* HDInsight kümesi VM'lerini ve küme tarafından kullanılan hizmet sorumlularını yerleştireceğiniz bir [kuruluş birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır.
* Azure AD ile iletişim kurmak için [Basit Dizin Erişimi Protokolleri](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAP) kurulmalıdır. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDInsight alt ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24).
* Hizmet hesabı veya kullanıcı hesabı gereklidir. Bu hesabı HDInsight kümesini oluşturmak için kullanın. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Active Directory etki alanına katma izinleri

**Yalnızca bulutta çalışan Azure AD ile tümleştirilmiş HDInsight**

Yalnızca bulutta çalışan Azure AD için bir etki alanı denetleyiciyi yapılandırarak HDInsight ile Azure AD'nin tümleştirilmesini sağlayın. Bu sonucu [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS) kullanarak elde edebilirsiniz. Azure AD DS bulutta etki alanı denetleyici makinelerini oluşturur ve bunların IP adreslerini sağlar. Yüksek kullanılabilirlik için iki etki alanı denetleyicisi oluşturur.

Şu anda Azure AD DS yalnızca klasik sanal ağlarda mevcuttur. Yalnızca klasik Azure portalı kullanılarak erişilebilir. Azure portalında yer alan HDInsight sanal ağının sanal ağlar arası eşleme kullanılarak klasik sanal ağla eşlenmesi gerekir.

> [!NOTE]
> Klasik bir sanal ağ ile bir Azure Resource Manager sanal ağı arasında eşleme gerçekleştirilebilmesi için her iki sanal ağ da aynı bölgede ve aynı Azure aboneliği altında olmalıdır.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Azure AD önkoşulları:

* HDInsight kümesi VM'lerini ve küme tarafından kullanılan hizmet sorumlularını yerleştireceğiniz bir [kuruluş birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır.
* Azure AD DS'yi yapılandırırken [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulumu gerçekleştirmeniz gerekir. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDInsight alt ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24).
* [Parola karmalarının](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) Azure AD'den Azure AD DS'ye eşitlenmesi gerekir.
* Hizmet hesabı veya kullanıcı hesabı gereklidir. Bu hesabı HDInsight kümesini oluşturmak için kullanın. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Azure AD etki alanına katma izinleri

**VPN aracılığıyla şirket içi bir Active Directory ile tümleştirilmiş HDInsight**

Bu mimari Azure IaaS üzerinde çalışan Azure AD ile tümleştirilen HDInsight ile benzerdir. Aralarındaki tek fark Azure AD'nin şirket içinde olması ve HDInsight ile Azure AD arasındaki iletişimin [Azure'dan şirket içi ağa VPN bağlantısı](../expressroute/expressroute-introduction.md) aracılığıyla kurulmasıdır.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_3.png)

> [!NOTE]
> Bu mimaride, Azure Data Lake Store'u HDInsight kümesi ile kullanamazsınız.

Azure AD önkoşulları:

* HDInsight kümesi VM'lerini ve küme tarafından kullanılan hizmet sorumlularını yerleştireceğiniz bir [kuruluş birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır.
* Azure AD ile iletişim kurmak için [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulumu yapılmalıdır. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDInsight alt ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24).
* Hizmet hesabı veya kullanıcı hesabı gereklidir. Bu hesabı HDInsight kümesini oluşturmak için kullanın. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Azure AD etki alanına katma izinleri

**Bir Azure AD'ye eşitlenmiş şirket içi Active Directory ile tümleştirilmiş HDInsight**

Bu mimari yalnızca bulutta çalışan Azure AD ile tümleştirilmiş HDInsight ile benzerdir. Aralarındaki tek fark, şirket içi Active Directory'nin Azure AD'ye eşitlenmesidir. Buluttaki bir etki alanı denetleyiciyi yapılandırarak HDInsight ile Azure AD'nin tümleştirilmesini sağlayın. Bu sonucu [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) kullanarak elde edebilirsiniz. Azure AD DS bulutta etki alanı denetleyici makinelerini oluşturur ve bunların IP adreslerini sağlar. Yüksek kullanılabilirlik için iki etki alanı denetleyicisi oluşturur.

Şu anda Azure AD DS yalnızca klasik sanal ağlarda mevcuttur. Yalnızca klasik Azure portalı kullanılarak erişilebilir. Azure portalında yer alan HDInsight sanal ağının sanal ağlar arası eşleme kullanılarak klasik sanal ağla eşlenmesi gerekir.

> [!NOTE]
> Klasik bir sanal ağ ile bir Azure Resource Manager sanal ağı arasında eşleme gerçekleştirilebilmesi için her iki sanal ağ da aynı bölgede ve aynı Azure aboneliği altında olmalıdır.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Azure AD önkoşulları:

* HDInsight kümesi VM'lerini ve küme tarafından kullanılan hizmet sorumlularını yerleştireceğiniz bir [kuruluş birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır.
* Azure AD DS'yi yapılandırırken [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulumu gerçekleştirmeniz gerekir. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDInsight alt ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24).
* [Parola karmalarının](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) Azure AD'den Azure AD DS'ye eşitlenmesi gerekir.
* Hizmet hesabı veya kullanıcı hesabı gereklidir. Bu hesabı HDInsight kümesini oluşturmak için kullanın. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Active Directory etki alanına katma izinleri

**Varsayılan olmayan bir Azure AD ile tümleştirilmiş HDInsight (yalnızca test ve geliştirme için kullanılması önerilir)**

Bu mimari yalnızca bulutta çalışan Azure AD ile tümleştirilmiş HDInsight ile benzerdir. Çoğu şirket için Azure AD'ye yönetici erişimi belirli kişilerle sınırlıdır. Bu nedenle, bir kavram kanıtı uygulamak ya da yalnızca etki alanına katılmış bir küme oluşturmayı denemek istediğinizde yöneticinin Azure AD'de önkoşullar yapılandırmasını beklemek yerine abonelikte yeni bir Azure AD örneği oluşturmak daha avantajlı olabilir. Oluşturduğunuz bir Azure AD örneği olduğundan, Azure AD DS'yi yapılandırmak için Azure AD'ye yönelik tüm izinlere sahip olursunuz.

Azure AD DS bulutta etki alanı denetleyici makinelerini oluşturur ve bunların IP adreslerini sağlar. Yüksek kullanılabilirlik için iki etki alanı denetleyicisi oluşturur.

Azure AD DS yalnızca klasik sanal ağlarda mevcuttur. Bu nedenle klasik Azure portalına erişim sahibi olmanız ve Azure AD DS yapılandırması için bir klasik sanal ağ oluşturmanız gerekir. Azure portalında yer alan HDInsight sanal ağının sanal ağlar arası eşleme kullanılarak klasik sanal ağla eşlenmesi gerekir.

> [!NOTE]
> Klasik sanal ağlar ile Azure Resource Manager sanal ağları arasında eşleme gerçekleştirilebilmesi için her iki sanal ağ da aynı bölgede ve aynı Azure aboneliği altında olmalıdır.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Azure AD önkoşulları:

* HDInsight kümesi VM'lerini ve küme tarafından kullanılan hizmet sorumlularını yerleştireceğiniz bir [kuruluş birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır.
* Azure AD DS'yi yapılandırırken [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulumu gerçekleştirmeniz gerekir. LDAPS’yi yapılandırmak için [otomatik olarak imzalanan sertifika](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) oluşturabilirsiniz. Ancak, otomatik olarak imzalanan bir sertifika kullanabilmeniz için <a href="mailto:hdipreview@microsoft.com">hdipreview@microsoft.com</a> adresinden bir özel durum isteğinde bulunmanız gerekir.
* HDInsight alt ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24).
* [Parola karmalarının](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) Azure AD'den Azure AD DS'ye eşitlenmesi gerekir.
* Hizmet hesabı veya kullanıcı hesabı gereklidir. Bu hesabı HDInsight kümesini oluşturmak için kullanın. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Azure Active Directory etki alanına katma izinleri

## <a name="next-steps"></a>Sonraki adımlar
* Etki alanına katılmış bir HDInsight kümesi yapılandırmak için bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](hdinsight-domain-joined-configure.md).
* Etki alanına katılmış HDInsight kümelerini yönetmek için bkz. [Etki alanına katılmış HDInsight kümelerini yönetme](hdinsight-domain-joined-manage.md).
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](hdinsight-domain-joined-run-hive.md).
* Etki alanına katılmış HDInsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için bkz. [Linux, Unix ya da OS X'te HDInsight'ta Linux tabanlı Hadoop ile SSH'yi kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).

