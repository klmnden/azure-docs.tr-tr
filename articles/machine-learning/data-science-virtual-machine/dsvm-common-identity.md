---
title: Veri bilimi sanal makine için - Azure ortak kimlik Kurulumu | Microsoft Docs
description: Kurumsal ekibin DSVM ortamlarda ortak kimlik ayarlayın.
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
ms.openlocfilehash: 70c6b8cd147cefaa3128bc1e6a414a6fa61dba6d
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2018
ms.locfileid: "34830903"
---
# <a name="setup-common-identity-on-the-data-science-vm"></a>Veri bilimi VM üzerinde ortak kimlik Kurulumu

Varsayılan olarak, bu kimlik bilgileri ile VM için kullanıcıların kimliğini doğrulamak ve veri bilimi VM (DSVM) yerel kullanıcı dahil olmak üzere Azure VM'de VM sağlarken hesapları oluşturulur. Erişmesi gereken birden çok VM varsa, bu yaklaşım hızlı kimlik bilgilerini yönetmek sıkıcı alabilirsiniz. Ortak kullanıcı hesapları ve kullanarak Yönetimi standartlara dayalı kimlik sağlayıcısı, birden çok DSVMs dahil olmak üzere birden çok kaynağına erişmek için tek bir kimlik bilgileri kümesi kullanmanıza olanak sağlar. 

Active Directory (AD) bir popüler kimlik sağlayıcısı ve desteklenen hem de şirket içi yanı sıra bir hizmet olarak Azure. Tek başına bir veri bilimi VM (DSVM) veya kümede bir Azure sanal makine ölçek kümesi DSVM kullanıcıların kimliklerini doğrulamak için AD veya Azure AD yararlanabilirsiniz. Bu, DSVM örnekleri bir AD etki alanına katılma tarafından gerçekleştirilir. Kimlikleri yönetmek için Active Directory zaten varsa, ortak kimlik sağlayıcınız olarak kullanabilirsiniz. Bir AD olmayan olasılığına adı verilen bir hizmeti yönetilen AD Azure üzerinde çalıştırabilirsiniz [Azure Active Directory etki alanı Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/)(Azure AD DS). 

Belgelerine [Azure Active directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/) sağlar ayrıntılı [yönergeleri](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution#synchronized-identity) varsa Azure AD şirket içi dizininize bağlanmak dahil olmak üzere yönetilen active directory. 

Bu makalenin devamında, Azure AD DS kullanarak Azure üzerinde tam olarak yönetilen bir AD etki alanı hizmeti ayarlayabilir ve, DSVMs ortak bir kullanıcı hesabı ve kimlik bilgilerini kullanarak DSVMs (ve diğer Azure kaynaklarına) havuzu erişmelerini sağlamak için yönetilen AD etki alanına katmak için gereken adımları açıklar. 

##  <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Azure üzerinde tam olarak yönetilen bir Active Directory etki alanı ayarlama

Azure AD DS kimliklerinizi Azure üzerinde tam olarak yönetilen bir hizmet sağlayarak yönetmenizi kolaylaştırır. Bu Active directory etki alanı kullanıcıları ve grupları yönetilir.  Adımları Azure AD etki alanı barındırılan ve dizininizde kullanıcı hesaplarının:

1. Portal üzerinde Active directory kullanıcıları ekleme 

    a. Oturum [Azure Active Directory Yönetim Merkezi](https://aad.portal.azure.com) dizini için genel yönetici olan bir hesapla.
    
    b. Seçin **Azure Active Directory** ve ardından **kullanıcılar ve gruplar**.
    
    c. Üzerinde **kullanıcılar ve gruplar**seçin **tüm kullanıcılar**ve ardından **yeni kullanıcı**.
   
   ![Ekle komutu seçme](./media/add-user.png)
    
    d. Kullanıcı için Ayrıntılar gibi girin **adı** ve **kullanıcı adı**. Kullanıcı adı etki alanı adı kısmını ya da gereken ilk varsayılan etki alanı adı "[etki alanı adı].onmicrosoft.com" veya doğrulanmış, Federasyon olmayan olması [özel etki alanı adı](../../active-directory/add-custom-domain.md) "contoso.com" gibi
    
    e. Kopyalama veya aksi takdirde bu işlem tamamlandıktan sonra kullanıcıya sağlayabilir böylece oluşturulmuş kullanıcı parolası not edin.
    
    f. İsteğe bağlı olarak, açın ve bilgileri doldurun **profil**, **grupları**, veya **dizin rolünü** kullanıcı için. 
    
    g. Üzerinde **kullanıcı**seçin **oluşturma**.
    
    h. Böylece kullanıcı oturum açarak yeni kullanıcı için oluşturulmuş bir parola güvenli bir şekilde dağıtın.

2.  Azure AD etki alanı hizmetleri oluşturma

    Bir Azure EKLER oluşturmak için makalesindeki yönergeleri izleyin. "[etkinleştirmek Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started)" (görev 5 görev 1). Active Directory'de mevcut kullanıcı parolalarını Azure AD DS'de parola synched şekilde güncelleştirilir önemlidir. Azure AD yukarıdaki makalenin görev # 4'te listelendiği gibi DS DNS eklemek önemlidir. 

3.  Ayrı bir DSVM alt önceki adımın görev # 2'de oluşturulan sanal ağ oluşturun
4.  DSVM alt ağdaki bir veya daha fazla veri bilimi VM örneği oluştur 
5.  İzleyin [yönergeleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) DSVM için AD eklemek için. 
6.  Sonraki çalışma alanınızı herhangi bir makinede takma etkinleştirmek için ev veya dizüstü bilgisayar dizininize barındırmak için paylaşılan bir Azure dosyaları bağlayın. (Sıkı dosya düzeyinde izinler gerekiyorsa, bir veya daha fazla Vm'lerinde çalışan NFS gerekir)

    a. [Bir Azure dosya paylaşımı oluşturma](../../storage/files/storage-how-to-create-file-share.md)
    
    b. Linux DSVM bağlayın. Azure Portal'da depolama hesabındaki Azure dosyaları için "Bağlan" düğmesine tıklayın, bash kabuğunda Linux DSVM üzerinde çalıştırmak için komutu gösterilir. Komut şöyle görünür:
```
sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
```
7.  Azure dosyalarınızda /data/workspace takılı söyleyin. Artık her kullanıcılarınızın paylaşımda dizinler oluşturun. /Data/Workspace/user1, /data/workspace/user2 ve benzeri. Oluşturma bir ```notebooks``` her kullanıcının çalışma alanındaki dizin. 
8. Simgesel bağlantıları oluşturma ```notebooks``` içinde ```$HOME/userx/notebooks/remote```.   

Şimdi, Azure ve AD kimlik bilgilerini kullanarak Azure AD DS'ye alanına katılmış herhangi DSVM (SSH, Jupyterhub) oturum açmak için barındırılan active Directory'de kullanıcıların sağlayın. Kullanıcı çalışma paylaşılan Azure dosyalarda olduğundan, kullanıcının kendi Not defterlerinin ve diğer iş herhangi DSVM gelen Jupyterhub kullanırken erişebilir. 

Otomatik ölçeklendirme için sanal makine ölçek kümesi bu şekilde ve takılı paylaşılan disk ile etki alanına katılmış tüm VM'lerin bir havuz oluşturmak için kullanabilirsiniz. Kullanıcılar, sanal makine ölçek kümesindeki herhangi bir kullanılabilir makine oturum açın ve bunların not defterlerini kaydedildiği paylaşılan disk erişimi. 

## <a name="next-steps"></a>Sonraki adımlar

* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde saklayın](dsvm-secure-access-keys.md)



