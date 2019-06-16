---
title: Azure'da SQL Server için güvenlik konuları | Microsoft Docs
description: Bu konu, bir Azure sanal makinesinde çalışan SQL Server güvenliğini sağlamak için genel rehberlik sağlar.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: d710c296-e490-43e7-8ca9-8932586b71da
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/23/2018
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: d5d10562a70b7d37908bc272bf555fd967831009
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076986"
---
# <a name="security-considerations-for-sql-server-in-azure-virtual-machines"></a>Azure Sanal Makineler'de SQL Server için Güvenlikle İlgili Dikkat Edilmesi Gerekenler

Bu konu, bir Azure sanal makinesinde (VM) SQL Server örneğine güvenli erişim sağlanmasına yardımcı genel güvenlik yönergeleri içerir.

Azure, birçok sektör düzenlemelerinin ve bir sanal makinede çalışan SQL Server ile uyumlu bir çözüm oluşturmanıza olanak veren standartları ile uyumludur. Azure yasal Uyumluluk hakkında daha fazla bilgi için bkz. [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/).

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

## <a name="control-access-to-the-sql-vm"></a>SQL VM erişimi denetleme

SQL Server sanal makinenizi oluşturduğunuzda, kimlerin erişebileceğini makine ve SQL Server için dikkatli bir şekilde denetlemek nasıl göz önünde bulundurun. Genel olarak, aşağıdakileri yapmanız gerekir:

- Erişimi yalnızca uygulamaları ve ihtiyacınız olan istemciler için SQL Server kısıtlayın.
- Kullanıcı hesapları ve parolaları yönetmek için en iyi güvenlik uygulamalarını izleyin.

Aşağıdaki bölümlerde, düşünme şu noktaları aracılığıyla hakkında öneriler sağlar.

## <a name="secure-connections"></a>Güvenli bağlantılar

Bir galeri görüntüsü ile bir SQL Server sanal makinesi oluşturduğunuzda **SQL Server bağlantısı** seçeneği, size, tercih ettiğiniz **yerel (VM)** , **özel (sanal ağ içinde)** , veya **genel (Internet)** .

![SQL Server bağlantısı](./media/virtual-machines-windows-sql-security/sql-vm-connectivity-option.png)

En iyi güvenlik için en kısıtlayıcı seçenek senaryonuz için seçin. Bir uygulama çalıştırıyorsanız Örneğin, aynı VM üzerinde SQL Server ardından erişen **yerel** en güvenli seçenektir. Ardından SQL Server'a erişim gerektiren bir Azure uygulama çalıştırıyorsanız **özel** yalnızca içinde belirtilen SQL Server iletişimi korur [Azure sanal ağı](../../../virtual-network/virtual-networks-overview.md). Gerektiriyorsa **genel** (internet) erişmek için SQL Server VM, sonra saldırı yüzey alanını azaltmak için bu konudaki diğer en iyi uygulamaları takip ettiğinizden emin olun.

Portalındaki seçili seçenekler gelen güvenlik kuralları VM'ye ait kullanın [ağ güvenlik grubu](../../../virtual-network/security-overview.md) izin verdiğini veya reddettiğini ağ trafiğinin sanal makinenize (NSG). Değiştirdiğinizde ya da SQL Server bağlantı noktası (varsayılan 1433) trafiğine izin vermek için yeni gelen NSG kuralları oluşturun. Bu bağlantı noktası üzerinden iletişim kurmasına izin verilen belirli IP adresleri de belirtebilirsiniz.

![Ağ güvenlik grubu kuralları](./media/virtual-machines-windows-sql-security/sql-vm-network-security-group-rules.png)

Ağ trafiği kısıtlamak için NSG kurallarının yanı sıra, sanal makinede Windows Güvenlik Duvarı'nı kullanabilirsiniz.

Uç noktaları ile klasik dağıtım modelini kullanıyorsanız, bunları kullanmıyorsanız sanal makinedeki tüm uç noktaları kaldırın. ACL'leri kullanarak uç noktaları ile ilgili yönergeler için bkz: [bir uç nokta ACL'sini Yönet](/previous-versions/azure/virtual-machines/windows/classic/setup-endpoints#manage-the-acl-on-an-endpoint). Bu Kaynak Yöneticisi'ni kullanan VM'ler için gerekli değildir.

Son olarak, şifrelenmiş bağlantılar, Azure sanal makinesinde SQL Server veritabanı altyapısı örneği için etkinleştirmeyi düşünün. SQL server örneği, imzalanmış bir sertifikayla yapılandırın. Daha fazla bilgi için [etkinleştirme şifrelenmiş veritabanı altyapısı bağlantılarını](https://docs.microsoft.com/sql/database-engine/configure-windows/enable-encrypted-connections-to-the-database-engine) ve [bağlantı dizesi söz dizimi](https://msdn.microsoft.com/library/ms254500.aspx).

## <a name="use-a-non-default-port"></a>Varsayılan olmayan bağlantı noktasını kullanın

Varsayılan olarak, SQL Server 1433 iyi bilinen bir bağlantı noktasında dinler. Daha fazla güvenlik için SQL Server'ın 1401 gibi varsayılan olmayan bir bağlantı noktası dinleyecek şekilde yapılandırın. Azure portalında bir SQL Server galeri görüntüsü sağlarsanız, bu bağlantı noktası belirtebilirsiniz **SQL Server ayarları** dikey penceresi.

[!INCLUDE [windows-virtual-machines-sql-use-new-management-blade](../../../../includes/windows-virtual-machines-sql-new-resource.md)]

Sağladıktan sonra bu yapılandırma için iki seçeneğiniz vardır:

- Resource Manager Vm'leri için seçtiğiniz **güvenlik** gelen [SQL sanal makineleri kaynak](virtual-machines-windows-sql-manage-portal.md#access-sql-virtual-machine-resource). Bu bağlantı noktasını değiştirmek için bir seçenek sağlar.

  ![TCP bağlantı noktası portalında değiştirin](./media/virtual-machines-windows-sql-security/sql-vm-change-tcp-port.png)

- Klasik VM'ler için veya portal ile sağlanmamış olan bir SQL Server Vm'leri için VM için uzaktan bağlanarak bağlantı noktasını el ile yapılandırabilirsiniz. Yapılandırma adımları için bkz. [bir belirli TCP bağlantı noktasını dinlemek üzere yapılandırmak](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-server-to-listen-on-a-specific-tcp-port). El ile bu tekniği kullanırsanız, ayrıca, TCP bağlantı noktasında gelen trafiğe izin vermek için bir Windows Güvenlik duvarı kuralı eklemeniz gerekir.

> [!IMPORTANT]
> Genel internet bağlantıları için SQL Server bağlantı noktası açıksa varsayılan olmayan bağlantı noktası belirterek iyi bir fikirdir.

SQL Server varsayılan olmayan bağlantı noktasında dinliyor, bağlandığınızda, bağlantı noktası belirtmeniz gerekir. Örneğin, burada sunucu IP adresi 13.55.255.255 ve SQL Server bağlantı noktasını 1401 dinlediğini bir senaryo düşünün. SQL Server'a bağlanmak için şunu belirtmeniz gerekir `13.55.255.255,1401` bağlantı dizesindeki.

## <a name="manage-accounts"></a>Hesapları yönetme

Hesap adlarını ve parolalarını kolayca tahmin saldırganlar istemezsiniz. Yardımcı olması için aşağıdaki ipuçlarını kullanın:

- Yok adlı bir benzersiz yerel yönetici hesabı oluşturmanız **yönetici**.

- Tüm hesaplarınız için karmaşık güçlü parolalar kullanın. Güçlü bir parola oluşturmakla ilgili daha fazla bilgi için bkz. [güçlü bir parola oluşturmak](https://support.microsoft.com/instantanswers/9bd5223b-efbe-aa95-b15a-2fb37bef637d/create-a-strong-password) makalesi.

- Varsayılan olarak, Azure, SQL Server sanal makinesi kurulumu sırasında Windows kimlik doğrulaması seçer. Bu nedenle, **SA** parola Kurulumu tarafından atanır ve oturum açma devre dışı bırakıldı. Öneririz **SA** oturum açma kullanılan veya etkin. Bir SQL oturum açma görüntülenmesi gerekiyorsa, aşağıdaki stratejilerden birini kullanın:

  - SQL hesabı sahip benzersiz bir adla oluşturun **sysadmin** üyelik. Portaldan etkinleştirerek bunu yapabilirsiniz **SQL kimlik doğrulaması** sağlama sırasında.

    > [!TIP] 
    > Sağlama işlemi sırasında SQL kimlik doğrulamasını etkinleştirmezseniz, kimlik doğrulama modu için el ile değiştirmeniz gerekir **SQL Server ve Windows kimlik doğrulama modu**. Daha fazla bilgi için [sunucu kimlik doğrulaması modunu değiştirme](https://docs.microsoft.com/sql/database-engine/configure-windows/change-server-authentication-mode).

  - Kullanmanız gerekirse **SA** oturum açma, oturum açma sağladıktan sonra etkinleştirmek ve yeni güçlü bir parola atayın.

## <a name="follow-on-premises-best-practices"></a>Şirket içi en iyi uygulamaları izleyin

Bu konuda açıklanan yöntemler yanı sıra, gözden geçirin ve uygunsa geleneksel şirket içi güvenlik uygulamalarını uygulayın öneririz. Daha fazla bilgi için [bir SQL Server yüklemesi için güvenlik konuları](https://docs.microsoft.com/sql/sql-server/install/security-considerations-for-a-sql-server-installation)

## <a name="next-steps"></a>Sonraki Adımlar

Ayrıca en iyi performansı geçici olarak ilgilendiğiniz olup [Azure sanal Makineler'de SQL Server için en iyi performans](virtual-machines-windows-sql-performance.md).

Azure Vm'lerinde SQL Server çalıştırma ile ilgili diğer konular için bkz [SQL Server Azure sanal makinelerine genel bakış](virtual-machines-windows-sql-server-iaas-overview.md). SQL Server sanal makineleri hakkında sorularınız olursa [Sık Sorulan Sorular](virtual-machines-windows-sql-server-iaas-faq.md) bölümüne bakın.

