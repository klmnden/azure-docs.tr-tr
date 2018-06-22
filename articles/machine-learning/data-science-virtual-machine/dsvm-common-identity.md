---
title: Veri bilimi sanal makine için - Azure ortak bir kimlik ayarlama | Microsoft Docs
description: Bir kurumsal ekibin DSVM ortamlarda ortak bir kimlik ayarlayın.
keywords: derin öğrenme, AI, veri bilimi araçları, veri bilimi sanal makine, Jeo-uzamsal analytics, takım veri bilimi işlemi
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: d6235f3a425481a13e627d683bb4c3943b473b40
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/21/2018
ms.locfileid: "36309830"
---
# <a name="set-up-a-common-identity-on-the-data-science-virtual-machine"></a>Ortak bir kimliği üzerindeki veri bilimi sanal makine ayarlama

Bir Azure sanal veri bilimi sanal makine (DSVM) dahil olmak üzere makinede (VM), VM sağlarken yerel kullanıcı hesapları oluşturun. Kullanıcılar daha sonra bu kimlik bilgilerini kullanarak VM kimlik doğrulaması. Erişmesi gereken birden çok VM varsa, kimlik bilgileri yönetirken bu yaklaşım hızlı sıkıcı alabilirsiniz. Genel kullanıcı hesapları ve Yönetimi standartlara dayalı kimlik sağlayıcısı üzerinden birden çok DSVMs dahil olmak üzere birden çok kaynağına erişmek için tek bir kimlik bilgileri kümesi kullanılacak sağlar. 

Active Directory popüler kimlik sağlayıcısı ve Azure üzerinde bir hizmet ve şirket içi desteklenir. Azure Active Directory (Azure AD) kullanabilir veya şirket içi bir tek başına DSVM veya kümede bir Azure sanal makine ölçek kümesi DSVMs kullanıcıların kimliklerini doğrulamak için Active Directory. Bunun için bir Active Directory etki alanına DSVM örnekleri birleştirerek. 

Kimlikleri yönetmek için Active Directory zaten varsa, ortak kimlik sağlayıcınız olarak kullanabilirsiniz. Active Directory yoksa, yönetilen bir Active Directory örneğini Azure üzerinde adı verilen bir hizmeti çalıştırabilirsiniz [Azure Active Directory etki alanı Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/) (Azure AD DS). 

Belgelerine [Azure AD](https://docs.microsoft.com/azure/active-directory/) sağlar ayrıntılı [yönetim yönergeleri](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution#synchronized-identity), varsa Azure AD şirket içi dizininize bağlanmak dahil olmak üzere. 

Bu makalede, Azure AD DS kullanarak Azure üzerinde tam olarak yönetilen bir Active Directory etki alanı hizmeti ayarlamak için adımları açıklar. Ardından, ortak bir kullanıcı hesabı ve kimlik bilgilerini kullanarak bir havuzu DSVMs (ve diğer Azure kaynaklarına) erişmelerini sağlamak için yönetilen Active Directory etki alanına, DSVMs katılmasını sağlayabilir. 

## <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Azure üzerinde tam olarak yönetilen bir Active Directory etki alanı ayarlama

Azure AD DS kimliklerinizi Azure üzerinde tam olarak yönetilen bir hizmet sağlayarak yönetmenizi kolaylaştırır. Bu Active Directory etki alanı kullanıcıları ve grupları yönetin. Bir Azure barındırılan Active Directory etki alanı ve kullanıcı hesapları dizininizde ayarlamak için adımları şunlardır:

1. Azure Portalı'nda, kullanıcının Active Directory'ye ekleyin: 

   a. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) dizini için genel yönetici olan bir hesapla.
    
   b. Seçin **Azure Active Directory** ve ardından **kullanıcılar ve gruplar**.
    
   c. Üzerinde **kullanıcılar ve gruplar**seçin **tüm kullanıcılar**ve ardından **yeni kullanıcı**.
   
      **Kullanıcı** bölmesini açar.
      
      !["Kullanıcı" bölmesi](./media/add-user.png)
    
   d. Kullanıcı için Ayrıntılar gibi girin **adı** ve **kullanıcı adı**. Kullanıcı adı etki alanı adı kısmını ilk varsayılan etki alanı adı "[etki alanı adı].onmicrosoft.com" veya doğrulanmış, olmalıdır Federasyon olmayan [özel etki alanı adı](../../active-directory/add-custom-domain.md) "contoso.com" gibi
    
   e. Kopyalama veya aksi takdirde bu işlem tamamlandıktan sonra kullanıcıya sağlayabilir böylece oluşturulmuş kullanıcı parolası not edin.
    
   f. İsteğe bağlı olarak, açın ve bilgileri doldurun **profil**, **grupları**, veya **dizin rolünü** kullanıcı için. 
    
   g. Üzerinde **kullanıcı**seçin **oluşturma**.
    
   h. Böylece kullanıcı oturum açarak yeni kullanıcı için oluşturulmuş bir parola güvenli bir şekilde dağıtın.

2. Azure AD DS örneği oluşturun. Makalesindeki yönergeleri izleyin [etkinleştirmek Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) (1-5 görevleri). Böylece Azure AD DS'de parola eşitlenen Active Directory'de mevcut kullanıcı parolaları güncelleştirmek önemlidir. Ayrıca görev makaleyi 4 açıklandığı gibi Azure AD DS'ye DNS eklemek önemlidir. 

3. Ayrı bir DSVM alt görev önceki adımı 2 oluşturulan sanal ağ oluşturun.
4. Bir veya daha fazla veri bilimi VM örnekleri DSVM alt ağda oluşturun. 
5. İzleyin [yönergeleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) DSVM Active Directory'ye eklemek için. 
6. Çalışma alanınızı herhangi bir makinede takma etkinleştirmek için ev veya dizüstü bilgisayar dizininize barındırmak için bir Azure dosya paylaşımına bağlayın. (Sıkı dosya düzeyinde izinler gerekiyorsa, bir veya daha fazla Vm'lerinde çalışan NFS gerekir.)

   a. [Bir Azure dosya paylaşımı oluşturmak](../../storage/files/storage-how-to-create-file-share.md).
    
   b. Linux DSVM bağlayın. Seçtiğinizde, **Bağlan** depolama hesabınızda Linux DSVM Kabuğu görünür Bash'te çalıştırılacak komut Azure portalında Azure dosyaları paylaşmak için düğmesine tıklayın. Komut şöyle görünür:
   
   ```
   sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
   ```
7. Azure dosya paylaşımınıza /data/workspace, örneğin bağlandığını varsayar. Artık her kullanıcılarınızın paylaşımda dizinler oluşturun: /data/workspace/user1, /data/workspace/user2 ve benzeri. Oluşturma bir `notebooks` her kullanıcının çalışma alanındaki dizin. 
8. Simgesel bağlantıları oluşturma `notebooks` içinde `$HOME/userx/notebooks/remote`.   

Şimdi, Active Directory örneğinizi Azure üzerinde barındırılan kullanıcıların sağlayın. Active Directory kimlik bilgilerini kullanarak, kullanıcıların Azure AD DS'ye alanına katılmış herhangi DSVM (SSH veya JupyterHub) oturum açabildiğinizden. Kullanıcı çalışma alanında bir Azure dosya paylaşımında olduğundan JupyterHub kullanırken kullanıcıların kendi Not defterlerinin ve diğer iş herhangi DSVM gelen erişimi. 

Otomatik ölçeklendirme için bir sanal makine ölçek kümesi bu şekilde ve takılı paylaşılan disk ile etki alanına katılmış tüm VM'lerin bir havuz oluşturmak için kullanabilirsiniz. Kullanıcılar, sanal makine ölçek kümesindeki herhangi bir kullanılabilir makine oturum açın ve bunların not defterlerini kaydedildiği paylaşılan disk erişimi. 

## <a name="next-steps"></a>Sonraki adımlar

* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde saklayın](dsvm-secure-access-keys.md)



