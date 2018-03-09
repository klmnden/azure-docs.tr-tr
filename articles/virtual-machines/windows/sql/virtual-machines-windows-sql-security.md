---
title: "Azure SQL Server için güvenlik konuları | Microsoft Docs"
description: "Bu konu, bir Azure sanal makinede çalışan SQL Server güvenliğini sağlamaya yönelik genel rehberlik sağlar."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: craigg
editor: 
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/02/2017
ms.author: jroth
ms.openlocfilehash: 609e18cf2bdfdd84c71b67e31b66cd0ca7d47577
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/08/2018
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler

Bu konu, bir Azure sanal makine (VM) SQL Server örnekleri güvenli erişim sağlanmasına yardımcı genel güvenlik yönergeleri içerir.

Azure birkaç sektör düzenlemelerini ve bir sanal makinede çalışan SQL Server ile uyumlu bir çözümü oluşturmak etkinleştirebileceğiniz standartları karşılar. Azure ile Mevzuat uyumluluğu hakkında daha fazla bilgi için bkz: [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-to-the-sql-vm"></a>SQL VM erişimi denetleme

SQL Server sanal makine oluşturduğunuzda, makine ve SQL Server için erişim izni olan dikkatle denetleme göz önünde bulundurun. Genel olarak, aşağıdakileri yapmanız gerekir:

- Erişim, yalnızca uygulamalar ve ihtiyacınız olan istemciler için SQL Server'a kısıtlayın.
- Kullanıcı hesapları ve parolaları yönetmek için en iyi uygulamaları izleyin.

Aşağıdaki bölümlerde bu noktaları üzerinden düşünmeye hakkında öneriler sağlar.

## <a name="secure-connections"></a>Güvenli bağlantılar

Bir galeri görüntüsü ile bir SQL Server sanal makine oluşturduğunuzda **SQL Server bağlantısı** seçeneği seçimi size **yerel (VM)**, **özel (sanal ağ dahilinde)**, veya **genel (Internet)**.

![SQL Server bağlantısı](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

En iyi güvenlik için senaryonuza en kısıtlayıcı seçeneği belirleyin. Bir uygulama çalıştırıyorsanız, örneğin, aynı VM SQL Server'da sonra erişen **yerel** en güvenli seçenektir. Ardından SQL Server'a erişim gerektiren bir Azure uygulama çalıştırıyorsanız, **özel** yalnızca içinde belirtilen SQL Server iletişimi güvenli hale getirdiği [Azure Virtual Network](../../../virtual-network/virtual-networks-overview.md). Gerektiriyorsa **ortak** VM, SQL Server (internest) erişimi daha sonra saldırı alanını azaltmak için bu konudaki diğer en iyi uygulamaları izleyerek emin olun.

Seçili seçenekler portalında sanal makinelerin gelen güvenlik kurallarını kullanın [ağ güvenlik grubu](../../../virtual-network/virtual-networks-nsg.md) veya sanal makinenize ağ trafiği reddetmek için (NSG). Değiştirin veya SQL Server bağlantı noktası (varsayılan 1433) trafiğine izin verecek şekilde, yeni gelen NSG kuralları oluşturun. Bu bağlantı noktası üzerinden iletişim kurmasına izin belirli IP adreslerini de belirtebilirsiniz.

![Ağ güvenlik grubu kuralları](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

Ağ trafiği kısıtlamak için NSG kurallarının yanı sıra, Windows Güvenlik Duvarı sanal makinede de kullanabilirsiniz.

Uç noktaları Klasik dağıtım modeliyle kullanıyorsanız, bunları kullanmıyorsanız sanal makine üzerindeki tüm uç noktaları kaldırın. ACL'ler uç ile kullanma ile ilgili yönergeler için bkz: [bir uç nokta ACL'sini Yönet](../classic/setup-endpoints.md#manage-the-acl-on-an-endpoint). Bu Kaynak Yöneticisi'ni VM'ler için gerekli değildir.

Son olarak, Azure sanal makinesinde SQL Server veritabanı altyapısı örneği için şifreli bağlantıları etkinleştirmeyi düşünün. SQL server örneği ile bir imzalı sertifika yapılandırın. Daha fazla bilgi için bkz: [etkinleştirmek şifrelenmiş veritabanı altyapısı bağlantılarını](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine) ve [bağlantı dizesi sözdizimi](https://msdn.microsoft.com/library/ms254500.aspx).

## <a name="use-a-non-default-port"></a>Varsayılan olmayan bağlantı noktası kullanma

Varsayılan olarak, SQL Server 1433 iyi bilinen bir bağlantı noktasında dinler. Daha yüksek güvenlik için SQL Server'ın 1401 gibi varsayılan olmayan bir bağlantı noktası üzerinde dinleme yapılandırın. Azure portalında bir SQL Server galeri görüntüsü sağlarsanız, bu bağlantı noktasına belirtebilirsiniz **SQL Server ayarları** dikey.

Bu sağladıktan sonra yapılandırmak için iki seçeneğiniz vardır:

- Resource Manager VM'ler için seçtiğiniz **SQL Server yapılandırma** VM genel bakış dikey penceresinden. Bu bağlantı noktasını değiştirmek için bir seçenek sağlar.

  ![TCP bağlantı noktası portalında değiştirin](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- Klasik sanal makineleri veya SQL Server Vm'lerinin'de, portal ile sağlanan değil, VM'ye uzaktan bağlanarak bağlantı noktasını el ile yapılandırabilirsiniz. Yapılandırma adımları için bkz: [belirli bir TCP bağlantı noktası üzerinde dinleme bir sunucuyu yapılandırmak](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port). El ile bu yöntemi kullanırsanız, ayrıca, TCP bağlantı noktasında gelen trafiğe izin vermek için bir Windows Güvenlik duvarı kuralı eklemeniz gerekir.

> [!IMPORTANT]
> SQL Server bağlantı noktası ortak Internet bağlantıları açıksa, varsayılan olmayan bağlantı noktası belirterek iyi bir fikirdir.

SQL Server varsayılan olmayan bağlantı noktasında dinleme eklerken bağladığınızda, bağlantı noktası belirtmeniz gerekir. Örneğin, burada sunucu IP adresi 13.55.255.255 ve SQL Server 1401 bağlantı noktasında dinleme bir senaryo düşünün. SQL Server'a bağlanmak için belirtirsiniz `13.55.255.255,1401` bağlantı dizesinde.

## <a name="manage-accounts"></a>Hesapları yönetme

Hesap adlarını veya parolaları kolayca tahmin etmek, saldırganların istemezsiniz. Yardımcı olması için aşağıdaki ipuçlarını kullanın:

- Yok adlı bir benzersiz yerel yönetici hesabı oluşturma **yönetici**.

- Karmaşık güçlü parolalar, tüm hesapları için kullanın. Güçlü bir parola oluşturma hakkında daha fazla bilgi için bkz: [güçlü bir parola oluşturmak](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password) makalesi.

- Varsayılan olarak, Azure SQL Server sanal makinesine Kurulum sırasında Windows kimlik doğrulaması seçer. Bu nedenle, **SA** oturum açma devre dışı bırakıldı ve bir parola Kurulumu tarafından atanır. Öneririz **SA** oturum açma bırakılamadı kullanılan veya etkinleştirilemedi. Bir SQL oturum açma görüntülenmesi gerekiyorsa, aşağıdaki stratejilerden birini kullanın:

  - SQL hesabı olan benzersiz bir ad oluşturmak **sysadmin** üyeliği. Portaldan etkinleştirerek bunu yapabilirsiniz **SQL kimlik doğrulaması** sağlama sırasında.

    > [!TIP] 
    > Sağlama işlemi sırasında SQL kimlik doğrulamasını etkinleştirmezseniz, kimlik doğrulama modu için el ile değiştirmeniz gerekir **SQL Server ve Windows kimlik doğrulama modu**. Daha fazla bilgi için bkz: [sunucu kimlik doğrulaması modunu değiştirme](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode).

  - Kullanmanız gerekiyorsa **SA** oturum açma, oturum açma sağladıktan sonra etkinleştirin ve yeni güçlü bir parola atayın.

## <a name="follow-on-premises-best-practices"></a>Şirket içi en iyi uygulamaları izleyin

Bu konuda açıklanan yöntemler yanı sıra, gözden geçirin ve uygun olan yerlerde geleneksel şirket içi güvenlik uygulamalarını uygulayın öneririz. Daha fazla bilgi için bkz: [bir SQL Server yüklemesi için güvenlik konuları](https://docs.microsoft.com/sql/sql-server/install/security-considerations-for-a-sql-server-installation)

## <a name="next-steps"></a>Sonraki Adımlar

Ayrıca performans geçici en iyi yöntemler ilgilendiğiniz olup [Azure Virtual Machines'de SQL Server için performans en iyi uygulamaları](virtual-machines-windows-sql-performance.md).

Azure Vm'lerde SQL Server çalıştırma ile ilgili diğer konular için bkz: [Azure sanal makinelere genel bakış SQL Server'da](virtual-machines-windows-sql-server-iaas-overview.md). SQL Server sanal makineler hakkında sorularınız varsa bkz [ilgili sık sorulan sorular](virtual-machines-windows-sql-server-iaas-faq.md).

