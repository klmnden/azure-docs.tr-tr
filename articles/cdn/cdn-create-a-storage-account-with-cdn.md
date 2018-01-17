---
title: "Azure storage hesabı Azure CDN ile tümleştirme | Microsoft Docs"
description: "Azure Storage bloblarından önbelleğe alarak yüksek bant genişliği içerik ulaştırmak için Azure içerik teslim ağı (CDN) kullanmayı öğrenin."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/04/2018
ms.author: mazha
ms.openlocfilehash: 022071f7825cb9184bd4c815c09e1c202a0a6f91
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2018
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Azure storage hesabı Azure CDN ile tümleştirme
Azure depolama biriminden önbellek içeriği Azure içerik teslim ağı (CDN) etkinleştirebilirsiniz. Azure CDN geliştiriciler yüksek bant genişliği içeriği teslimi için genel bir çözüm sunar. BLOB'ları ve işlem örnekleri ABD, Avrupa, Asya, Avustralya ve Güney Amerika fiziksel düğümlerde, statik içeriği önbelleğe alabilir.

## <a name="step-1-create-a-storage-account"></a>1. adım: depolama hesabı oluşturma
Bir Azure aboneliğine yönelik yeni bir depolama hesabı oluşturmak için aşağıdaki yordamı kullanın. Bir depolama hesabı Azure Storage hizmetlerine erişmenizi sağlar. Depolama hesabı Azure Storage Hizmeti bileşenlerinin her biri erişmek için ad alanı en yüksek düzeyde temsil eder: Azure Blob, kuyruk ve tablo depolama. Daha fazla bilgi için bkz: [Microsoft Azure Storage'a giriş](../storage/common/storage-introduction.md).

Bir depolama hesabı oluşturmak için Hizmet Yöneticisi veya ilişkili abonelik için bir Abonelikteki olması gerekir.

> [!NOTE]
> Azure portal ve PowerShell dahil olmak üzere bir depolama hesabı oluşturmak için çeşitli yöntemler kullanabilirsiniz. Bu öğretici Azure Portalı'nı kullanmayı gösterir.   
> 

**Bir Azure aboneliği için bir depolama hesabı oluşturmak için**

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. Sol üst köşedeki seçin **kaynak oluşturma**. İçinde **yeni** bölmesinde, **depolama**ve ardından **depolama hesabı - blob, dosya, tablo, kuyruk**.
    
    **Depolama hesabı oluşturma** bölmesinde görünür.   

    ![Depolama hesabı bölmesi oluşturma](./media/cdn-create-a-storage-account-with-cdn/cdn-create-new-storage-account.png)

3. İçinde **adı** kutusunda, bir alt etki alanı adı girin. Bu giriş, 3-24 küçük harfler ve sayılar içerebilir.
   
    Bu değer blob, kuyruk veya tablo kaynaklar abonelik için gidermek için kullanılan URI içindeki konak adına dönüşür. Blob depolamada kapsayıcı kaynak adresi için aşağıdaki biçimde bir URI kullanın:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*

    Burada  *&lt;StorageAccountLabel&gt;*  girdiğiniz değer başvurduğu **adı** kutusu.
   
    > [!IMPORTANT]    
    > URL etiketi depolama hesabının URI alt etki alanı oluşturur ve azure'da barındırılan tüm hizmetler arasında benzersiz olması gerekir.
   
    Bu değer ayrıca portalında veya bu hesap program aracılığıyla erişirken depolama hesabı adı olarak kullanılır.
    
4. Varsayılan değerleri kullanmak **dağıtım modeli**, **tür hesap**, **performans**, ve **çoğaltma**. 
    
5. İçin **abonelik**, depolama hesabıyla kullanılacak aboneliği seçin.
    
6. İçin **kaynak grubu**bir kaynak grubu oluşturun veya seçin. Kaynak grupları hakkında daha fazla bilgi için bkz: [Azure Resource Manager'a genel bakış](../azure-resource-manager/resource-group-overview.md#resource-groups).
    
7. İçin **konumu**, depolama hesabınız için bir konum seçin.
    
8. **Oluştur**’u seçin. Depolama hesabı oluşturma işleminin tamamlanması birkaç dakika sürebilir.

## <a name="step-2-enable-cdn-for-the-storage-account"></a>2. adım: Depolama hesabı için CDN etkinleştirme

Depolama hesabınız için depolama hesabınızdan doğrudan CDN etkinleştirebilirsiniz. 

1. Panodan bir depolama hesabı seçin ve ardından **Azure CDN** sol bölmeden. Varsa **Azure CDN** düğmesi hemen görünmüyorsa, CDN'de girebilirsiniz **arama** kutusunu sol bölmenin.
    
    **Azure içerik teslim ağı** bölmesinde görünür.

    ![CDN uç noktası oluşturma](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png)
    
2. Yeni bir uç noktası, gerekli bilgileri girerek oluşturabilirsiniz:
    - **CDN profili**: yeni bir CDN profili oluşturun veya varolan bir CDN profili kullanın.
    - **Fiyatlandırma katmanı**: bir fiyatlandırma katmanı yalnızca bir CDN profili oluşturuyorsanız seçin.
    - **CDN uç nokta adı**: bir CDN uç nokta adı girin.

    > [!TIP]
    > Varsayılan olarak, yeni bir CDN uç noktası kaynak sunucu olarak depolama hesabınızın ana bilgisayar adını kullanır.

3. **Oluştur**’u seçin. Uç nokta oluşturulduktan sonra uç nokta listesinde görüntülenir.

    ![Depolama yeni CDN uç noktası](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png)

> [!NOTE]
> CDN uç noktanız, en iyi duruma getirme türü gibi gelişmiş yapılandırma ayarlarını belirtmek istiyorsanız bunun yerine kullanabileceğiniz [Azure CDN uzantısı](cdn-create-new-endpoint.md#create-a-new-cdn-endpoint) bir CDN uç noktası veya bir CDN profili oluşturmak için.

## <a name="step-3-enable-additional-cdn-features"></a>3. adım: ek CDN özellikleri etkinleştirme

Depolama hesabından **Azure CDN** bölmesinde, CDN uç noktası CDN yapılandırma bölmesini açmak için listeden seçin. Sıkıştırma, sorgu dizesi ve coğrafi filtreleme gibi teslimat için ek CDN özellikleri etkinleştirebilirsiniz. Ayrıca, özel etki alanı eşleme CDN uç noktanızı eklemeyi ve özel etki alanı HTTPS etkinleştirin.
    
![Depolama CDN uç noktası yapılandırması](./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png)

## <a name="step-4-access-cdn-content"></a>4. adım: Erişim CDN içerik
CDN önbelleğe alınmış içeriğe erişmek için CDN Portal'da sağlanan URL'yi kullanın. Önbelleğe alınan bir blob adresi aşağıdaki biçime sahiptir:

http://<*EndpointName*\>.azureedge.net/<*myPublicContainer*\>/<*BlobName*\>

> [!NOTE]
> Bir depolama hesabı için CDN erişim etkinleştirdikten sonra genel olarak kullanılabilir tüm nesneler CDN uç önbelleğe alma için uygundur. CDN şu anda önbelleğe alınmış nesneyi değiştirirseniz, önbelleğe alınmış içeriği için yaşam süresi süresi dolduktan sonra CDN içeriğini yeniler kadar yeni içerik CDN kullanılabilir olmayacaktır.

## <a name="step-5-remove-content-from-the-cdn"></a>5. adım: içerik CDN Kaldır
Artık Azure CDN nesneyi önbelleğe almak istiyorsanız, aşağıdaki adımlardan birini gerçekleştirebilirsiniz:

* Ortak yerine kapsayıcı özel yapma. Daha fazla bilgi için bkz: [kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../storage/blobs/storage-manage-access-to-resources.md).
* Devre dışı bırakmak veya Azure portalını kullanarak CDN uç noktası silin.
* Barındırılan hizmet artık nesne için isteklere yanıt verecek şekilde değiştirin.

Zaten Azure CDN'de önbelleğe alınmış bir nesne, nesne yaşam süresi boyunca süresi doluncaya kadar veya uç nokta temizlenir kadar önbelleğe alınmış kalır. Yaşam süresi süresi sona erdiğinde, Azure CDN CDN uç noktası hala geçerli olduğundan ve nesne hala anonim olarak erişilebilir durumda olup olmadığını denetler. Değilse, nesne artık önbelleğe alınır.

## <a name="additional-resources"></a>Ek kaynaklar
* [CDN uç noktanıza özel etki alanı ekleme](cdn-map-content-to-custom-domain.md)
* [Bir Azure CDN özel etki alanı üzerinde HTTPS yapılandırma](cdn-custom-ssl.md)

