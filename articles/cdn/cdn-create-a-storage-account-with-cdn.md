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
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: dbdf263d9d7fdfbe4fbc47db9ba9f30637e8c3ad
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2018
---
# <a name="integrate-an-azure-storage-account-with-azure-cdn"></a>Azure storage hesabı Azure CDN ile tümleştirme
CDN önbellek içeriği Azure depolama hesabınızdan etkinleştirilebilir. Geliştiriciler BLOB'ları ve işlem örnekleri ABD, Avrupa, Asya, Avustralya ve Güney Amerika fiziksel düğümlerde, statik içeriği önbelleğe alarak yüksek bant genişliği içeriği teslimi için genel bir çözüm sunar.

## <a name="step-1-create-a-storage-account"></a>1. adım: depolama hesabı oluşturma
Bir Azure aboneliği için yeni bir depolama hesabı oluşturmak için aşağıdaki yordamı kullanın. Bir depolama hesabı için Azure storage hizmetlerine erişmenizi sağlar. Depolama hesabı Azure depolama hizmeti bileşenlerinin her biri erişmek için ad alanı en yüksek düzeyde temsil eder: Blob Hizmetleri, kuyruk Hizmetleri ve Tablo Hizmetleri. Daha fazla bilgi için bkz [Microsoft Azure Storage'a giriş](../storage/common/storage-introduction.md).

Bir depolama hesabı oluşturmak için Hizmet Yöneticisi veya ilişkili abonelik için bir ortak yönetici olması gerekir.

> [!NOTE]
> Azure Portal ve Powershell dahil olmak üzere bir depolama hesabı oluşturmak için kullanabileceğiniz çeşitli yöntemler vardır.  Bu öğretici için size Azure portalını kullanacağız.  
> 
> 

**Bir Azure aboneliği için bir depolama hesabı oluşturmak için**

1. [Azure Portal](https://portal.azure.com)’da oturum açın.
2. Sol üst köşedeki seçin **yeni**. İçinde **yeni** iletişim kutusunda **veri + depolama**, ardından **depolama hesabı**.
    
    **Depolama hesabı oluşturma** dikey penceresi görünür.   

    ![Depolama Hesabı Oluştur][create-new-storage-account]  

3. İçinde **adı** alanında, bir alt etki alanı adı yazın. Bu giriş, 3-24 küçük harfler ve sayılar içerebilir.
   
    Bu değer aboneliği için Blob, kuyruk veya tablo kaynak adres için kullanılan URI içindeki konak adına dönüşür. Blob hizmetinde kapsayıcı kaynak adres için aşağıdaki biçimde bir URI kullanırsınız. burada  *&lt;StorageAccountLabel&gt;*  , yazdığınız değer başvurduğu **birURLgirin**:
   
    http://*&lt;StorageAcountLabel&gt;*.blob.core.windows.net/*&lt;mycontainer&gt;*
   
    **Önemli:** URL depolama hesabının URI alt etki alanı forms etiket ve azure'da barındırılan tüm hizmetler arasında benzersiz olması gerekir.
   
    Bu değer, aynı zamanda bu hesabı program aracılığıyla erişirken Portalı'nda veya bu depolama hesabı adı olarak kullanılır.
4. Varsayılan değerleri bırakın **dağıtım modeli**, **tür hesap**, **performans**, ve **çoğaltma**. 
5. Seçin **abonelik** depolama hesabı ile kullanılacak.
6. Bir **Kaynak Grubu** seçin veya oluşturun.  Kaynak Grupları hakkında daha fazla bilgi için bkz. [Azure Resource Manager’a genel bakış](../azure-resource-manager/resource-group-overview.md#resource-groups).
7. Depolama hesabınız için bir konum seçin.
8. **Oluştur**’a tıklayın. Depolama hesabı oluşturma işleminin tamamlanması birkaç dakika sürebilir.

## <a name="step-2-enable-cdn-for-the-storage-account"></a>2. adım: Depolama hesabı için CDN etkinleştirme

En yeni tümleştirmesi ile artık CDN depolama hesabınız için depolama portal uzantınızı ayrılmadan etkinleştirebilirsiniz. 

1. Depolama hesabını seçin, "CDN" ya da aşağı sol gezinti menüsünde arayın, ardından "Azure CDN"'i tıklatın.
    
    **Azure CDN** dikey penceresi görünür.

    ![CDN etkinleştir gezinme][cdn-enable-navigation]
    
2. Gerekli bilgileri girerek yeni bir uç noktası oluşturun
    - **CDN profili**: yeni oluşturun veya varolan bir profili kullanın.
    - **Fiyatlandırma katmanı**: yeni bir CDN profili oluşturursanız, bir fiyatlandırma katmanı seçmek yeterlidir.
    - **CDN uç nokta adı**: tercih ettiğiniz her bir uç nokta adı girin.

    > [!TIP]
    > Oluşturulan CDN uç noktası ana bilgisayar depolama hesabınızın adını varsayılan olarak kaynak kullanır.

    ! [cdn yeni uç nokta oluşturma] [cdn yeni-endpoint-oluşturma]

3. Oluşturulduktan sonra yeni uç nokta yukarıdaki uç nokta listesinde görünecektir.

    ![CDN depolama yeni uç noktası][cdn-storage-new-endpoint]

> [!NOTE]
> CDN etkinleştirmek için Azure CDN uzantısı de gidebilirsiniz. [Öğretici](#Tutorial-cdn-create-profile).
> 
> 

[!INCLUDE [cdn-create-profile](../../includes/cdn-create-profile.md)]  

## <a name="step-3-enable-additional-cdn-features"></a>3. adım: ek CDN özellikleri etkinleştirme

Depolama hesabı "Azure CDN" dikey penceresinden CDN yapılandırma dikey penceresini açmak için listeden CDN uç noktası'ı tıklatın. Sıkıştırma ve sorgu dizesi gibi teslimat için ek CDN özellikleri etkinleştirebilirsiniz coğrafi filtreleme. Ayrıca, özel etki alanı eşleme CDN uç noktanızı eklemeyi ve özel etki alanı HTTPS etkinleştirin.
    
![CDN depolama cdn yapılandırması][cdn-storage-cdn-configuration]

## <a name="step-4-access-cdn-content"></a>4. adım: Erişim CDN içerik
CDN önbelleğe alınmış içeriğe erişmek için CDN Portal'da sağlanan URL'yi kullanın. Önbelleğe alınan bir blob adresi aşağıdakine benzer olacaktır:

http://<*EndpointName*\>.azureedge.net/ <*myPublicContainer*\>/<*BlobName*\>

> [!NOTE]
> Bir depolama hesabı için CDN erişimi etkinleştirdiğinizde, genel olarak kullanılabilir tüm nesneler CDN uç önbelleğe alma için uygundur. CDN şu anda önbelleğe alınmış nesneyi değiştirirseniz, yaşam süresi önbelleğe alınan içerik süresi sona erdiğinde, CDN içeriğini yeniler kadar yeni içerik CDN kullanılabilir olmayacaktır.
> 
> 

## <a name="step-5-remove-content-from-the-cdn"></a>5. adım: içerik CDN Kaldır
Artık bir nesne içinde Azure içerik teslim ağı (CDN) önbelleğe almak istiyorsanız, aşağıdaki adımlardan birini gerçekleştirebilirsiniz:

* Kapsayıcı yapabileceğiniz ortak yerine özel. Daha fazla bilgi için bkz.: [Kapsayıcılar ve bloblar için anonim okuma erişimini yönetme](../storage/blobs/storage-manage-access-to-resources.md).
* Yönetim Portalı'nı kullanarak CDN uç noktasını silmek ya da devre dışı bırakın.
* Barındırılan hizmet artık nesne için isteklere yanıt verecek şekilde değiştirebilirsiniz.

Bir nesne zaten CDN önbelleğe nesne yaşam süresi boyunca sona erene kadar veya uç nokta temizlenir kadar önbelleğe alınmış olarak kalır. Yaşam süresi süresi sona erdiğinde, CDN CDN uç noktası hala geçerli olup olmadığını ve hala anonim olarak erişilebilir nesne görmek için kontrol eder. Değilse, ardından nesne artık önbelleğe alınır.

## <a name="additional-resources"></a>Ek kaynaklar
* [CDN İçeriğini Özel Etki Alanı ile Eşleme](cdn-map-content-to-custom-domain.md)
* [Özel etki alanınız için HTTPS'yi etkinleştir](cdn-custom-ssl.md)

[create-new-storage-account]: ./media/cdn-create-a-storage-account-with-cdn/CDN_CreateNewStorageAcct.png
[cdn-enable-navigation]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-creation.png
[cdn-storage-new-endpoint]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-new-endpoint-list.png
[cdn-storage-cdn-configuration]: ./media/cdn-create-a-storage-account-with-cdn/cdn-storage-endpoint-configuration.png 
