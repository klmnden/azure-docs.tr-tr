---
title: "Azure API Management Geliştirici Portalı üzerinde sayfa stili özelleştirme | Microsoft Docs"
description: "Azure API Management Geliştirici Portalı öğelerde stilini özelleştirmek için bu hızlı başlangıç adımları izleyin."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: f427663ba1c437785c8c521925d9f733c45cb40d
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2017
---
# <a name="customize-the-style-of-the-developer-portal-pages"></a>Geliştirici portal sayfalarına stil özelleştirme

Azure API Management'ta Geliştirici portalını özelleştirmek için en yaygın üç yolu vardır:
 
* [Statik sayfaları ve sayfa düzeni öğelerini içeriğini düzenleme](api-management-modify-content-layout.md)
* (Bu kılavuzda açıklanan) Geliştirici Portalı üzerinden sayfası öğeleri için kullanılan stillerini güncelleştir
* [Portal tarafından oluşturulan sayfalar için kullanılan şablonları değiştirmek](api-management-developer-portal-templates.md) (örneğin, API belgeleri, ürünler, kullanıcı kimlik doğrulaması)

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Öğeleri sayfalarında stilini özelleştirmek **Geliştirici** portalı
> * Değişikliğiniz görüntüleyin

![Stil özelleştirme](./media/modify-developer-portal-style/developer_portal.png)

## <a name="prerequisites"></a>Ön koşullar

+ Aşağıdaki Hızlı Başlangıç tamamlamak: [bir Azure API Management örneği oluşturma](get-started-create-service-instance.md).
+ Ayrıca, aşağıdaki öğreticiye: [alma ve ilk API'nizi yayımlama](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="customize-the-developer-portal"></a>Geliştirici portalını özelleştirme

1. Seçin **genel bakış**.
2. Tıklatın **Geliştirici Portalı** tepesindeki düğmesini **genel bakış** penceresi. Alternatif olarak, tıklayabilirsiniz **Geliştirici Portalı URL'si** bağlantı.
3. Ekranın sol üst tarafında iki boyama Fırçalar oluşan bir simge bakın. Portal özelleştirmenin menüsünü açmak için bu simgenin üzerine getirin.

    ![Stil özelleştirme](./media/modify-developer-portal-style/modify-developer-portal-style01.png)
4. Seçin **stilleri** stil özelleştirme bölmesini açmak için menüden.

    Kullanarak özelleştirebileceğiniz tüm öğeleri **stilleri** sayfada görünecek
5. "Headings-color" girin **Geliştirici Portalı görünümünü özelleştirmek için değişken değerlerini değiştirin:** alan.

     **@headings-color**  Öğe sayfasında görüntülenir. Bu değişken metnin rengini denetler.

    ![Stil özelleştirme](./media/modify-developer-portal-style/modify-developer-portal-style02.png)
    
6. ' I tıklatın alanına  **@headings-color**  değişkeni. 
    
    Renk Seçici açılan açar.
7. Renk seçiciler açılan yeni bir renk seçin.

    > [!TIP]
    > Gerçek zamanlı Önizleme tüm değişiklikleri için kullanılabilir. Bir İlerleme göstergesi özelleştirme bölmenin en üstünde görünür. Birkaç saniye sonra yeni seçilen renkte üstbilgi metni değiştirir.

8. Seçin **Yayımla** özelleştirme bölmesi menüsünde alt soldan.
9. Seçin **özelleştirmeleri yayımlamak** genel kullanıma açık bir değişiklik için.

## <a name="view-your-change"></a>Değişikliğiniz görüntüleyin

1. Geliştirici portalına gidin.
2. Yaptığınız değişikliği görebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Öğeleri sayfalarında stilini özelleştirmek **Geliştirici** portalı
> * Değişikliğiniz görüntüleyin

> [!div class="nextstepaction"]
> [Şablonları kullanarak Azure API Management Geliştirici portalını özelleştirme](api-management-developer-portal-templates.md)