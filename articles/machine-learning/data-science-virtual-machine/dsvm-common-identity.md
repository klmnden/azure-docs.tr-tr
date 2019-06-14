---
title: Ortak bir kimlik ayarlamak için veri bilimi sanal makinesi - Azure | Microsoft Docs
description: Birden çok veri bilimi sanal makinelerde kullanılabilir ortak kullanıcı hesapları oluşturma hakkında bilgi edinin. Azure Active Directory veya bir şirket içi Active Directory, veri bilimi sanal makinesi için kullanıcıların kimliğini doğrulamak için kullanabilirsiniz.
keywords: derin öğrenme yapay ZEKA, veri bilimi araçları, veri bilimi sanal makinesi, Jeo-uzamsal analiz, team data science Process'i
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2018
ms.author: gokuma
ms.openlocfilehash: 0146ee6ee37c2eb9e98d831b54df2218d7de5b62
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "60502386"
---
# <a name="set-up-a-common-identity-on-the-data-science-virtual-machine"></a>Üzerinde veri bilimi sanal makinesi ortak bir kimlik ayarlayın

Azure sanal veri bilimi sanal makinesi (DSVM) dahil olmak üzere makinesinde (VM), VM sağlarken yerel kullanıcı hesapları oluşturun. Kullanıcılar daha sonra bu kimlik bilgilerini kullanarak VM'ye kimlik doğrulaması. Erişmesi gereken birden fazla VM varsa, kimlik bilgilerini yönetme gibi bu yaklaşım hızlı bir şekilde hantal alabilirsiniz. Genel kullanıcı hesapları ve Yönetimi standartlara dayalı kimlik sağlayıcısı üzerinden azure'da birden çok Dsvm'leri dahil olmak üzere birden çok kaynaklara erişmek için tek bir kimlik bilgileri kümesi kullanmak sağlar. 

Active Directory, popüler kimlik sağlayıcısı ve Azure üzerinde bir hizmet ve şirket içi desteklenir. Azure Active Directory (Azure AD) kullanabilir veya tek başına DSVM veya kümede bir Azure sanal makine ölçek kümesi Dsvm'leri kullanıcıların kimliklerini doğrulamak için Active Directory şirket. Bunun için DSVM örnekleri bir Active Directory etki alanına katılarak. 

Kimliklerini yönetmek için Active Directory zaten varsa, ortak bir kimlik sağlayıcısı olarak kullanabilirsiniz. Active Directory yoksa, adlı bir hizmet aracılığıyla yönetilen bir Active Directory örneğini Azure üzerinde çalıştırabilirsiniz [Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/) (Azure AD DS). 

Belgelerine [Azure AD'ye](https://docs.microsoft.com/azure/active-directory/) sağlayan ayrıntılı [yönetim yönergeleri](https://docs.microsoft.com/azure/active-directory/choose-hybrid-identity-solution), varsa şirket içi dizininizi Azure AD'ye bağlanma dahil olmak üzere. 

Bu makalede Azure AD DS kullanarak azure'da tam olarak yönetilen bir Active Directory etki alanı hizmeti ayarlamak için gereken adımlar açıklanmaktadır. Ardından, ortak bir kullanıcı hesabı ve kimlik bilgilerini kullanarak Dsvm'leri (ve diğer Azure kaynakları) oluşan bir havuz erişmelerini sağlamak için yönetilen Active Directory etki alanında, Dsvm'leri katılmasını sağlayabilirsiniz. 

## <a name="set-up-a-fully-managed-active-directory-domain-on-azure"></a>Azure üzerinde tam olarak yönetilen bir Active Directory etki alanı ayarlama

Azure AD DS Azure'da tam olarak yönetilen bir hizmet sağlayarak kimliklerinizi yönetmek basit hale getirir. Bu Active Directory etki alanı kullanıcıları ve grupları yönetin. Dizininizdeki bir Azure'da barındırılan Active Directory etki alanı ve kullanıcı hesaplarını ayarlama adımları şunlardır:

1. Azure portalında, kullanıcının Active Directory'ye ekleyin: 

   a. Dizin için genel yönetici olan bir hesapla [Azure Active Directory yönetim merkezinde](https://aad.portal.azure.com) oturum açın.
    
   b. **Azure Active Directory**'yi ve ardından **Kullanıcılar ve gruplar**'ı seçin.
    
   c. **Kullanıcılar ve gruplar** sayfasında **Tüm kullanıcılar**'ı ve ardından **Yeni kullanıcı**'yı seçin.
   
      **Kullanıcı** bölmesi açılır.
      
      !["Kullanıcı" bölmesi](./media/add-user.png)
    
   d. **Ad** ve **Kullanıcı adı** gibi kullanıcı ayrıntılarını girin. İlk varsayılan etki alanı adı "[etki alanı adı].onmicrosoft.com" veya doğrulanmış, kullanıcı adı etki alanı adı kısmı olmalıdır Federasyon olmayan [özel etki alanı adı](../../active-directory/add-custom-domain.md) "contoso.com" gibi
    
   e. Bu işlem tamamlandıktan sonra kullanıcıyla paylaşabilmek için oluşturulan kullanıcı parolasını kopyalayın veya bir yere not edin.
    
   f. İsteğe bağlı olarak kullanıcının **Profil**, **Gruplar** veya **Dizin rolü** bilgilerini doldurun. 
    
   g. **Kullanıcı** sayfasında **Oluştur**'u seçin.
    
   h. Kullanıcının oturum açabilmesi için oluşturulan parolayı güvenli bir şekilde iletin.

1. Azure AD DS örneği oluşturun. Makaledeki yönergeleri [etkinleştirme Azure Active Directory etki alanı Azure portalını kullanarak Hizmetleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started) (1-5 görevleri). Azure AD DS'de parola eşitlendiğinde, mevcut Active Directory kullanıcı parolalarını güncelleştirmek önemlidir. Ayrıca görev makaleyi 4 açıklandığı Azure AD DS, DNS eklemek önemlidir. 

1. Önceki adımı 2 görevde oluşturduğunuz sanal ağı ayrı bir DSVM alt ağ oluşturun.
1. Bir veya daha fazla veri bilimi sanal makinesi örneğini DSVM alt ağında oluşturun. 
1. İzleyin [yönergeleri](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-join-ubuntu-linux-vm ) DSVM Active Directory'ye ekleme. 
1. Herhangi bir makinede çalışma alanınızı bağlamak etkinleştirmek üzere, giriş veya not defteri dizinde barındırmak için Azure dosyaları paylaşımına bağlayın. (Sıkı dosya düzeyinde izinlerin gerekiyorsa, bir veya daha fazla Vm'lerde çalışan NFS gerekir.)

   a. [Bir Azure dosya paylaşımı oluşturma](../../storage/files/storage-how-to-create-file-share.md).
    
   b. Bu, üzerinde bir Linux DSVM'sini bağlayın. Seçtiğinizde, **Connect** Linux DSVM'sini Kabuk görünür Bash'te çalıştırmak için komutu Azure portalında depolama hesabınızda Azure dosyaları paylaşım için düğme. Komut şöyle görünür:
   
   ```
   sudo mount -t cifs //[STORAGEACCT].file.core.windows.net/workspace [Your mount point] -o vers=3.0,username=[STORAGEACCT],password=[Access Key or SAS],dir_mode=0777,file_mode=0777,sec=ntlmssp
   ```
1. Azure dosya paylaşımınızdaki /data/workspace, örneğin bağlandığını varsayar. Artık kullanıcılarınızı paylaşımındaki her dizini oluşturmak: /data/workspace/user1 /data/workspace/user2 ve benzeri. Oluşturma bir `notebooks` her bir kullanıcının çalışma dizini. 
1. İçin simgesel bağlantılar oluştur `notebooks` içinde `$HOME/userx/notebooks/remote`.   

Artık, kullanıcıların, Azure'da barındırılan, Active Directory örneğine sahip. Active Directory kimlik bilgilerini kullanarak, kullanıcılar, Azure AD DS'ye katılan tüm DSVM (SSH veya JupyterHub) için oturum açabilir. JupyterHub kullanırken kullanıcılar kullanıcı çalışma bir Azure dosya paylaşımında olduğundan, kullanıcıların not defterlerini ve diğer iş herhangi DSVM erişimini sahip olursunuz. 

Otomatik ölçeklendirme için sanal makine ölçek kümesi bağlı paylaşılan disk ile ve bu şekilde etki alanına katılmış tüm VM'lerin bir havuz oluşturmak için kullanabilirsiniz. Kullanıcılar, sanal makine ölçek kümesindeki tüm kullanılabilir makine oturum açın ve kullanıcıların not defterlerini kaydedildiği paylaşılan disk erişimi. 

## <a name="next-steps"></a>Sonraki adımlar

* [Bulut kaynaklarına erişmek için kimlik bilgilerini güvenli bir şekilde depolayın](dsvm-secure-access-keys.md)



