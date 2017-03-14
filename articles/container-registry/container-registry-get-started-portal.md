---
title: "Özel Docker kayıt defteri oluşturma - Azure portalı | Microsoft Docs"
description: "Azure portalı ile özel Docker kapsayıcısı kayıt defterleri oluşturmaya ve yönetmeye başlayın"
services: container-registry
documentationcenter: 
author: stevelas
manager: balans
editor: dlepow
tags: 
keywords: 
ms.assetid: 53a3b3cb-ab4b-4560-bc00-366e2759f1a1
ms.service: container-registry
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/14/2016
ms.author: stevelas
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: d9dad6cff80c1f6ac206e7fa3184ce037900fc6b
ms.openlocfilehash: 33944d34ce695e1729b761a7f762e24a6dce70a2
ms.lasthandoff: 03/06/2017

---

# <a name="create-a-private-docker-container-registry-using-the-azure-portal"></a>Azure portalı kullanarak özel bir Docker kapsayıcı kayıt defteri oluşturma
Kapsayıcı kayıt defteri oluşturmak ve kayıt defteri ayarlarını yönetmek için Azure portalını kullanın. Ayrıca, [Azure CLI 2.0 komutlarını](container-registry-get-started-azure-cli.md) kullanarak veya Kapsayıcı Kayıt Defteri [REST API](https://go.microsoft.com/fwlink/p/?linkid=834376) ile programlı olarak da kapsayıcı kayıt defterleri oluşturup yönetebilirsiniz.

Arka plan ve kavramlar için, bkz: [genel bakış](container-registry-intro.md)


> [!NOTE]
> Container Kayıt Defteri şu an önizleme aşamasındadır.


## <a name="create-a-container-registry"></a>Kapsayıcı kayıt defteri oluşturma
1. [Portalda](https://portal.azure.com) **+ Yeni**’ye tıklayın.
2. Markette **kapsayıcı kayıt defteri** ifadesini aratın.
3. Yayımcısı **Microsoft** olan **Container Kayıt Defteri (önizleme)** uygulamasını seçin. 
    ![Azure Market’te Container Kayıt Defteri hizmeti](./media/container-registry-get-started-portal/container-registry-marketplace.png)
4. **Oluştur**’a tıklayın. **Container Kayıt Defteri** dikey penceresi görünür.

    ![Container kayıt defteri ayarları](./media/container-registry-get-started-portal/container-registry-settings.png)
5. **Container Kayıt Defteri** dikey penceresinde aşağıdaki bilgileri girin. İşiniz bittiğinde **Oluştur**’a tıklayın.
   
    a. **Kayıt defteri adı**: Oluşturduğunuz kayıt defterinize özel olan, genel olarak benzersiz bir üst düzey etki alanı adı. Bu örnekte kayıt defterinin adı *myRegistry01*, ancak bunu kendi bulduğunuz bir adla değiştirin. Ad yalnızca harf ve sayı içerebilir.
   
    b. **Kaynak grubu**: Mevcut bir [kaynak grubu](../azure-resource-manager/resource-group-overview.md#resource-groups) seçin ya da yeni bir grup için ad yazın. 
   
    c. **Konum**: Hizmetin [kullanılabilir](https://azure.microsoft.com/regions/services/) olduğu bir Azure veri merkezi bölgesi seçin, örneğin **Orta Güney ABD**. 
   
    d. **Yönetici kullanıcı**: İsterseniz bir yönetici kullanıcıya kayıt defterine erişim izni verin. Kayıt defterini oluşturduktan sonra bu ayarı değiştirebilirsiniz.
   
    > [!IMPORTANT]
    > Kapsayıcı kayıt defterleri, bir yönetici kullanıcı hesabı aracılığıyla erişim sağlamaya ek olarak Azure Active Directory hizmet sorumluları tarafından desteklenen kimlik doğrulamasını destekler. Daha fazla bilgi ve göz önünde bulundurulması gereken diğer noktalar için bkz. [Kapsayıcı kayıt defteri ile kimlik doğrulama](container-registry-authentication.md).
    >
    
    e. **Depolama hesabı**: Varsayılan ayarı kullanarak bir [depolama hesabı](../storage/storage-introduction.md) oluşturun veya aynı konumdaki mevcut bir depolama hesabını seçin. Premium Depolama şu anda desteklenmemektedir.


## <a name="manage-registry-settings"></a>Kayıt defteri ayarlarını yönetme
Kayıt defterini oluşturduktan sonra portaldaki **Container Kayıt Defterleri** dikey penceresinden başlayarak kayıt defteri ayarlarını bulun. Örneğin, kayıt defterinizde oturum açmak için ayarlara ihtiyacınız olabilir ya da yönetici kullanıcıyı etkinleştirmek veya devre dışı bırakmak isteyebilirsiniz.

1. **Container Kayıt Defterleri** dikey penceresinde kayıt defterinizin adına tıklayın.
   
    ![Container kayıt defteri dikey penceresi](./media/container-registry-get-started-portal/container-registry-blade.png)
2. Erişim ayarlarını yönetmek için **Erişim tuşu**’na tıklayın.
   
    ![Container kayıt defteri erişimi](./media/container-registry-get-started-portal/container-registry-access.png)
3. Aşağıdaki ayarlara dikkat edin:
   
   * **Oturum açma sunucusu** - Kayıt defterinde oturum açmak için kullandığınız tam ad. Bu örnekte bu değer `myregistry01-contoso.azurecr.io`’dur.
   * **Yönetici kullanıcı** - Kayıt defterindeki yönetici kullanıcı hesabını etkinleştirmek veya devre dışı bırakmak için düğmeyi değiştirin.
   * **Kullanıcı adı** ve **Parola** - Kayıt defterinde oturum açmak için kullanabileceğiniz yönetici kullanıcı hesabının (etkinse) kimlik bilgileri. İsteğe bağlı olarak parolayı yeniden oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
* [Docker CLI’yı kullanarak ilk görüntünüzü itme](container-registry-get-started-docker-cli.md)




