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
ms.sourcegitcommit: b5cd321f3cd4c415f5e97ca550b7ab1679324921
ms.openlocfilehash: 94ef8147f1f73b293e818fc508096789373df7fc


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Hdınsight'ta Azure Etki Alanına Katılmış Hadoop kümeleri planlama

Geleneksel Hadoop tek kullanıcılı bir kümedir. Büyük veri iş yüklerini oluşturan küçük uygulama ekiplerine sahip çoğu şirket için uygundur. Hadoop’un popülerliği arttıkça çoğu kurum kümelerin BT ekipleri tarafından yönetildiği ve birden çok uygulama ekibinin ortak kümelerde çalıştığı bir modele geçiş yapıyor. Bu nedenle, çok kullanıcılı kümeler HDInsight’ta en çok talep alan işlevlerden biri.

HDInsight kendi çok kullanıcılı kimlik doğrulaması ve yetkilendirmesini oluşturmak yerine en popüler kimlik sağlayıcısını kullanır: Active Directory (AD). Active Directory’deki güçlü güvenlik grupları işlevi, HDInsight’ta çoklu kullanıcı yetkilendirmesini yönetmek için kullanılabilir. HDInsight’ı Active Directory ile tümleştirdiğinizde, Active Directory kullanıcıları Active Directory kimlik bilgilerini kullanarak kümelerle iletişim kurabilir. Active Directory kullanıcısı HDInsight tarafından yerel bir Hadoop kullanıcısıyla eşlenir ve bu sayede HDInsight’ta çalışan tüm hizmetler (Ambari, Hive sunucusu, Ranger, Spark Thrift sunucusu, vb.) kimliği doğrulanan kullanıcı için sorunsuz bir şekilde çalışır.

## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight’ı Active Directory ile tümleştirin

HDInsight’ı Active Directory ile tümleştirdiğinizde HDInsight küme düğümleri Active Directory etki alanına katılır. HDInsight, kümede çalışan Hadoop hizmetleri için hizmet sorumluları oluşturur ve bunları Active Directory’deki belirli bir Kuruluş Birimi’ne (OU) yerleştirir. HDInsight, etki alanına katılmış düğümlerin IP adresleri için Active Directory etki alanında ters DNS eşlemeleri de oluşturur.

Bu kurulumu elde etmek için izleyebileceğiniz birkaç mimari vardır. Sizin için en uygun mimarinin hangisi olduğuna sizin karar vermeniz gerekir.

**1. Azure IAAS üzerinde çalışan AD ile tümleştirilen HDInsight**

HDInsight’ı Active Directory ile tümleştirmek için kullanabileceğiniz en basit mimari budur. Active Directory etki alanı denetleyicisi Azure’daki bir (veya birden çok) sanal makinede (VM) çalışır. Genellikle bu sanal makineler bir sanal ağ içerisindedir. HDInsight kümesi için başka bir sanal ağ kurarsınız. HDInsight’ın Active Directory’yi doğrudan görebilmesi için [Sanal Ağlar Arası Eşleme](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md) kullanarak bu sanal ağları eşlemeniz gerekir.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> Bu mimaride, Azure Data Lake Store’u HDInsight kümesi ile kullanamazsınız.
 

Active Directory için önkoşullar:

* HDInsight kümesi sanal makinelerini ve küme tarafından kullanılan hizmet sorumlularını yerleştirmeniz gereken bir [Kuruluş Birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır.
* Active Directory ile iletişim kurulabilmesi için [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulmalıdır. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDInsight Alt Ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24).
* HDInsight kümesini oluşturmak için kullanılan bir hizmet hesabı ya da kullanıcı hesabı gerekir. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri.
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Active Directory etki alanına katma izinleri.

**2. Yalnızca bulutta çalışan bir Azure AD ile tümleştirilmiş HDInsight**

Yalnızca bulutta çalışan bir Azure Active Directory (Azure AD) istiyorsanız HDInsight’ın Azure Active Directory’nizle tümleştirilebilmesi için bir etki alanı denetleyicisi yapılandırmanız gerekir. Bu sonucu [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS) kullanarak elde edebilirsiniz. Azure AD DS bulutta etki alanı denetleyici makinelerini oluşturur ve size bunların IP adreslerini sağlar. Yüksek kullanılabilirlik için iki etki alanı denetleyicisi oluşturur.

Şu anda Azure AD DS yalnızca klasik sanal ağlarda bulunmaktadır. Yalnızca klasik Azure portalı kullanılarak erişilebilir. Azure portalında yer alan HDInsight sanal ağının sanal ağlar arası eşleme kullanılarak klasik sanal ağla eşlenmesi gerekir.

> [!NOTE]
> Klasik bir sanal ağ ile bir Azure Resource Manager sanal ağı arasında eşleme gerçekleştirilebilmesi için her iki sanal ağ da aynı bölgede ve aynı Azure aboneliği altında olmalıdır.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Active Directory için önkoşullar:

* HDInsight kümesi sanal makinelerini ve küme tarafından kullanılan hizmet sorumlularını yerleştirmeniz gereken bir [Kuruluş Birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır. 
* AD DS’yi yapılandırırken [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulumu gerçekleştirmeniz gerekir. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDI Alt Ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24). 
* [Parola karmalarının](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) Azure AD’den AD DS'ye eşitlenmesi gerekir.
* HDInsight kümesini oluşturmak için kullanılan bir hizmet hesabı ya da kullanıcı hesabı gerekir. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri.
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Active Directory etki alanına katma izinleri.

**3. VPN aracılığıyla şirket içi bir AD ile tümleştirilmiş HDInsight**

Bu mimari 1 numaralı mimariye benzer. Aralarındaki tek fark Active Directory’nin şirket içinde olması ve HDInsight ile Active Directory arasındaki iletişimin [Azure’dan şirket içi ağa VPN bağlantısı](../expressroute/expressroute-introduction.md) aracılığıyla kurulmasıdır.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_3.png)

> [!NOTE]
> Bu mimaride, Azure Data Lake Store’u HDInsight kümesi ile kullanamazsınız.

Active Directory için önkoşullar:

* HDInsight kümesi sanal makinelerini ve küme tarafından kullanılan hizmet sorumlularını yerleştirmeniz gereken bir [Kuruluş Birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır.
* Active Directory ile iletişim kurulabilmesi için [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulmalıdır. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDI Alt Ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24).
* HDInsight kümesini oluşturmak için kullanılan bir hizmet hesabı ya da kullanıcı hesabı gerekir. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri.
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Active Directory etki alanına katma izinleri.

**4. Bir Azure AD’ye eşitlenmiş şirket içi bir AD ile tümleştirilmiş HDInsight**

Bu mimari 2 numaralı mimariye benzer. Aralarındaki tek fark, şirket içi Active Directory’nin Azure Active Directory’ye eşitlenmesidir. HDInsight’ın Azure Active Directory’nizle tümleştirilebilmesi için bulutta bir etki alanı denetleyicisi yapılandırmanız gerekir. Bu sonucu [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (AD DS) kullanarak elde edebilirsiniz. AD DS bulutta etki alanı denetleyici makinelerini oluşturur ve size bunların IP adreslerini sağlar. Yüksek kullanılabilirlik için iki etki alanı denetleyicisi oluşturur.

Şu anda Azure AD DS yalnızca klasik sanal ağlarda bulunmaktadır. Yalnızca klasik Azure portalı kullanılarak erişilebilir. Azure portalında yer alan HDInsight sanal ağının sanal ağlar arası eşleme kullanılarak klasik sanal ağla eşlenmesi gerekir.

> [!NOTE]
> Klasik bir sanal ağ ile bir Azure Resource Manager sanal ağı arasında eşleme gerçekleştirilebilmesi için her iki sanal ağ da aynı bölgede ve aynı Azure aboneliği altında olmalıdır.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Active Directory için önkoşullar:

* HDInsight kümesi sanal makinelerini ve küme tarafından kullanılan hizmet sorumlularını yerleştirmeniz gereken bir [Kuruluş birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır. 
* AD DS’yi yapılandırırken [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulumu gerçekleştirmeniz gerekir. LDAPS kurulumu için kullanılan sertifika gerçek bir sertifika (otomatik olarak imzalanan sertifika değil) olmalıdır.
* HDI Alt Ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24). 
* [Parola karmalarının](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) Azure AD’den AD DS'ye eşitlenmesi gerekir.
* HDInsight kümesini oluşturmak için kullanılan bir hizmet hesabı ya da kullanıcı hesabı gerekir. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri.
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Active Directory etki alanına katma izinleri.

**5. Varsayılan olmayan bir Azure AD ile tümleştirilmiş HDInsight (yalnızca test ve geliştirme için kullanılması önerilir)**

Bu mimari 2 numaralı mimariye benzer. Çoğu şirket için Active Directory’ye yönetici erişimi yalnızca belirli kişilerle sınırlıdır. Bu nedenle, bir kavram kanıtı uygulamak ya da yalnızca etki alanına katılmış bir küme oluşturmayı denemek istediğinizde yöneticinin Active Directory’de önkoşullar yapılandırmasını beklemek yerine abonelikte yeni bir Azure Active Directory oluşturmak daha avantajlı olabilir. Oluşturduğunuz AD bir Azure AD olduğundan, AD DS’yi yapılandırmak için bu Azure AD’ye yönelik tüm izinlere sahip olursunuz.

AD DS bulutta etki alanı denetleyici makinelerini oluşturur ve size bunların IP adreslerini sağlar. Yüksek kullanılabilirlik için iki etki alanı denetleyicisi oluşturur.

AD DS şu anda yalnızca klasik sanal ağlarda olduğundan, Klasik portala erişebilmeniz ve AD DS’yi yapılandırmak için klasik bir sanal ağ oluşturmanız gerekir. Azure portalında yer alan HDInsight sanal ağının sanal ağlar arası eşleme kullanılarak klasik sanal ağla eşlenmesi gerekir.

> [!NOTE]
> Klasik sanal ağlar ile Azure Resource Manager sanal ağları arasında eşleme gerçekleştirilebilmesi için her iki sanal ağ da aynı bölgede ve aynı Azure aboneliği altında olmalıdır.

![HDInsight küme topolojisini etki alanına katma](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Active Directory için önkoşullar:

* HDInsight kümesi sanal makinelerini ve küme tarafından kullanılan hizmet sorumlularını yerleştirmeniz gereken bir [Kuruluş birimi](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) oluşturulmalıdır. 
* AD DS’yi yapılandırırken [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) kurulumu gerçekleştirmeniz gerekir. LDAPS’yi yapılandırmak için [otomatik olarak imzalanan sertifika](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) oluşturabilirsiniz. Ancak, otomatik olarak imzalanan bir sertifika kullanabilmeniz için <a href="mailto:hdipreview@microsoft.com">hdipreview@microsoft.com</a> adresinden bir özel durum isteğinde bulunmanız gerekir.
* HDI Alt Ağının IP adresi aralığı için etki alanında ters DNS bölgeleri oluşturulmalıdır (örneğin, bir önceki resimde 10.2.0.0/24). 
* [Parola karmalarının](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) Azure AD’den AD DS'ye eşitlenmesi gerekir.
* HDInsight kümesini oluşturmak için kullanılan bir hizmet hesabı ya da kullanıcı hesabı gerekir. Bu hesap aşağıdaki izinlere sahip olmalıdır:

    - Kuruluş birimi içerisinde hizmet sorumlusu nesneleri ve makine nesneleri oluşturma izinleri.
    - Ters DNS proxy kuralları oluşturma izinleri
    - Makineleri Active Directory etki alanına katma izinleri.

## <a name="next-steps"></a>Sonraki adımlar
* Etki alanına katılmış HDInsight kümesini yapılandırmak için bkz. [Etki alanına katılmış HDInsight kümelerini yapılandırma](hdinsight-domain-joined-configure.md).
* Etki alanına katılmış HDInsight kümelerini yönetmek için bkz. [Etki alanına katılmış HDInsight kümelerini yönetme](hdinsight-domain-joined-manage.md).
* Hive ilkelerini yapılandırmak ve Hive sorgularını çalıştırmak için bkz. [Etki alanına katılmış HDInsight kümeleri için Hive ilkelerini yapılandırma](hdinsight-domain-joined-run-hive.md).
* Etki alanına katılmış HDInsight kümelerinde SSH kullanarak Hive sorguları çalıştırmak için bkz. [Linux, Unix ya da OS X’te HDInsight’ta Linux tabanlı Hadoop ile SSH’yi kullanma](hdinsight-hadoop-linux-use-ssh-unix.md).




<!--HONumber=Feb17_HO1-->


