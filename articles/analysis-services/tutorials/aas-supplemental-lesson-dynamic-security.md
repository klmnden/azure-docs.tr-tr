---
title: "Azure Analysis Services öğreticisi ek ders: Dinamik güvenlik | Microsoft Docs"
description: "Azure Analysis Services öğreticisinde satır filtrelerini kullanarak dinamik güvenliğin nasıl kullanılacağını açıklar."
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 4e97a558ae1a2601b5275a73164b483351f03857
ms.contentlocale: tr-tr
ms.lasthandoff: 07/26/2017

---
# <a name="supplemental-lesson---dynamic-security"></a>Ek ders - Dinamik güvenlik

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Bu ek derste dinamik güvenlik uygulayan bir ek rol oluşturacaksınız. Dinamik güvenlik, oturum açmış olan kullanıcının kullanıcı adı veya oturum açma kimliğine bağlı olarak satır düzeyinde güvenlik sağlar. 
  
Dinamik güvenliği uygulamak için modelinize ona bağlanabilecek ve model nesneleriyle verilere göz atabilecek kullanıcıların kullanıcı adlarını içeren bir tablo eklersiniz. Bu öğreticiyi kullanarak oluşturduğunuz model, Adventure Works kapsamındadır ancak bu dersi tamamlamak için kendi etki alanınızdaki kullanıcıları içeren bir tablo eklemeniz gerekir. Eklediğiniz kullanıcı adlarının parolalarına ihtiyaç yoktur. Kendi etki alanınızdaki birkaç örnek kullanıcıyla bir EmployeeSecurity tablosu oluşturmak için Yapıştır özelliğini kullanarak bir Excel çalışma sayfasında yer alan çalışan verilerini yapıştırmanız gerekir. Gerçek hayatta kullanıcı adlarını içeren tablo genelde gerçek bir DimEmployee tablosu gibi gerçek bir veri kaynağından alınan bir tablo olacaktır.  
  
Dinamik güvenliği uygulamak için iki DAX işlevini kullanırsınız: [USERNAME İşlevi (DAX)](http://msdn.microsoft.com/22dddc4b-1648-4c89-8c93-f1151162b93f) ve [LOOKUPVALUE İşlevi (DAX)](http://msdn.microsoft.com/73a51c4d-131c-4c33-a139-b1342d10caab). Bu işlevler bir satır filtresi formülüne uygulanır ve yeni bir rolde tanımlanır. Formül, LOOKUPVALUE işlevini kullanarak EmployeeSecurity tablosundan bir değer belirtir. Formül ardından bu değeri bu role ait olan oturum açmış kullanıcının kullanıcı adını belirten USERNAME işlevine iletir. Bunun ardından kullanıcı yalnızca rolün satır filtreleri tarafından belirtilen verilere erişebilir. Bu senaryoda satış elemanlarının yalnızca üyesi oldukları satış bölgelerine ait İnternet satış verilerine bakabilmelerini sağlıyorsunuz.  
  
Bu görevler bu Adventure Works tablosal model senaryosuna özgü ancak gerçek dünyadaki senaryolar için geçerli olmayabilir. Her görevde görevin amacını belirten ek bilgiler yer alır.  
  
Bu dersin tahmini tamamlanma süresi: **30 dakika**  
  
## <a name="prerequisites"></a>Ön koşullar  
Bu ek ders konusu, sırayla tamamlanması gereken bir tablosal modelleme öğreticisinin bir parçasıdır. Bu ek dersteki görevleri gerçekleştirmeden önce önceki tüm dersleri tamamlamış olmanız gerekir.  
  
## <a name="add-the-dimsalesterritory-table-to-the-aw-internet-sales-tabular-model-project"></a>DimSalesTerritory tablosunu AW İnternet Satışları Tablosal Model Projesi içine ekleme  
Bu Adventure Works senaryosuna dinamik güvenliği uygulamak için modelinize iki tablo eklemeniz gerekir. Ekleyeceğiniz ilk tablo, aynı AdventureWorksDW veritabanından DimSalesTerritory (Satış Bölgesi olarak) tablosudur. Ardından SalesTerritory tablosuna oturum açmış olan kullanıcının göz atabileceği verileri tanımlayan bir satır filtresi uygularsınız.  
  
#### <a name="to-add-the-dimsalesterritory-table"></a>DimSalesTerritory tablosunu eklemek için  
  
1.  Tablosal Model Gezgini'nde > **Veri Kaynakları**, bağlantınıza sağ tıklayın ve ardından **Yeni Tabloları İçeri Aktar**'a tıklayın.  

    Açılan Kimliğe Bürünme Kimlik Bilgileri iletişim kutusunda Ders 2: Veri Ekleme sırasında kullandığınız kimliğe bürünme kimlik bilgilerini yazın.
  
2.  Gezgin'de **DimSalesTerritory** tablosunu seçin ve **Tamam**'a tıklayın.    
  
3.  Sorgu Düzenleyicisi'nde **DimSalesTerritory** sorgusuna tıklayın ve **SalesTerritoryAlternateKey** sütununu kaldırın.  
  
7.  **İçeri Aktar**’a tıklayın.  
  
    Yeni tablo model çalışma alanına eklenir. Kaynak DimSalesTerritory tablosundaki nesneler ve veriler AW İnternet Satışları Tablosal Model içine aktarılır.  
  
9. Tablo başarıyla içeri aktarıldıktan sonra **Kapat**'a tıklayın.  

## <a name="add-a-table-with-user-name-data"></a>Kullanıcı adı verilerinin bulunduğu bir tablo ekleme  
AdventureWorksDW örnek veritabanındaki DimEmployee tablosunda AdventureWorks etki alanından kullanıcılar yer almaktadır. Bu kullanıcı adları kendi ortamınızda mevcut değildir. Modelinizde kuruluşunuzdan birkaç kişiyi (en az üç) içeren bir tablo oluşturmanız gerekir. Ardından bu kullanıcıları yeni role üye olarak ekleyebilirsiniz. Örnek kullanıcı adlarının parolalarına ihtiyaç yoktur ancak kendi etki alanınızdan gerçek Windows kullanıcı adları kullanmanız gerekir.  
  
#### <a name="to-add-an-employeesecurity-table"></a>Bir EmployeeSecurity tablosu eklemek için  
  
1.  Microsoft Excel'i açıp bir çalışma sayfası oluşturun.  
  
2.  Üst bilgi satırı dahil olmak üzere aşağıdaki tabloyu kopyalayıp çalışma sayfasına yapıştırın.  

    ```
      |EmployeeId|SalesTerritoryId|FirstName|LastName|LoginId|  
      |---------------|----------------------|--------------|-------------|------------|  
      |1|2|<user first name>|<user last name>|\<domain\username>|  
      |1|3|<user first name>|<user last name>|\<domain\username>|  
      |2|4|<user first name>|<user last name>|\<domain\username>|  
      |3|5|<user first name>|<user last name>|\<domain\username>|  
    ```

3.  Ad, soyadı ve etki alanı\kullanıcı adı bölümlerine kuruluşunuzdan üç kişinin adını ve oturum açma kimliğini yazın. EmployeeId 1 için ilk iki satıra aynı kullanıcı adını yazarak birden fazla satış bölgesine ait olduğunu belirtin. EmployeeId ve SalesTerritoryId alanlarını değiştirmeyin.  
  
4.  Çalışma sayfasını **SampleEmployee** adıyla kaydedin.  
  
5.  Çalışma sayfasında üst bilgiler dahil olmak üzere çalışan verisi içeren tüm hücreleri seçin, seçtiğiniz verilere sağ tıklayın ve **Kopyala**'ya tıklayın.  
  
6.  SSDT içinde **Düzen** menüsüne ve ardından **Yapıştır**'a tıklayın.  
  
    Yapıştır gri renkliyse model tasarımcısı penceresinde herhangi bir sütuna tıkladıktan sonra tekrar deneyin.  
  
7.  **Yapıştırma Önizlemesi** iletişim kutusunun **Tablo Adı** bölümüne **EmployeeSecurity** yazın.  
  
8.  **Yapıştırılacak veriler** bölümünde üst bilgiler dahil olmak üzere SampleEmployee çalışma sayfasındaki tüm verilerin yer aldığından emin olun.  
  
9. **İlk satırı sütun başlığı olarak kullan** seçeneğinin işaretli olduğundan emin olun ve **Tamam**'a tıklayın.  
  
    SampleEmployee çalışma sayfasından kopyalanan çalışan verileriyle EmployeeSecurity adlı yeni bir tablo oluşturulur.  
  
## <a name="create-relationships-between-factinternetsales-dimgeography-and-dimsalesterritory-table"></a>FactInternetSales, DimGeography ve DimSalesTerritory tabloları arasında ilişki oluşturma  
FactInternetSales, DimGeography ve DimSalesTerritory tablolarında SalesTerritoryId adlı ortak bir sütun vardır. DimSalesTerritory tablosundaki SalesTerritoryId sütunu her satış bölgesi için farklı kimliğe sahip değerler içerir.  
  
#### <a name="to-create-relationships-between-the-factinternetsales-dimgeography-and-the-dimsalesterritory-table"></a>FactInternetSales, DimGeography ve DimSalesTerritory tabloları arasında ilişki oluşturmak için  
  
1.  Diyagram Görünümünde **DimGeography** tablosunda **SalesTerritoryId** sütununa tıklayıp basılı tutun ve imleci **DimSalesTerritory** tablosundaki **SalesTerritoryId** sütununa sürükleyip bırakın.  
  
2.  **FactInternetSales** tablosunda **SalesTerritoryId** sütununa tıklayıp basılı tutun ve imleci **DimSalesTerritory** tablosundaki **SalesTerritoryId** sütununa sürükleyip bırakın.  
  
    Bu ilişki için Active özelliğinin False değerine sahip olduğuna, yani devre dışı olduğuna dikkat edin. FactInternetSales tablosu zaten başka bir etkin ilişkiye sahiptir.  
  
## <a name="hide-the-employeesecurity-table-from-client-applications"></a>EmployeeSecurity tablosunu istemci uygulamalarından gizleme  
Bu görevde EmployeeSecurity tablosunu gizleyerek istemci uygulamasının alan listesinde görünmesini engelleyeceksiniz. Gizleme işleminin, tabloyu güvenli hale getirmeyeceğini unutmayın. Nasıl yapacağını bilen kullanıcılar EmployeeSecurity tablosundaki verileri sorgulayabilir. EmployeeSecurity tablosundaki verilerin güvenliğini sağlamak ve kullanıcıların verileri sorgulamasını engellemek için sonraki görevde bir filtre uygulayacaksınız.  
  
#### <a name="to-hide-the-employeesecurity-table-from-client-applications"></a>EmployeeSecurity tablosunu istemci uygulamalarından gizlemek için  
  
-   Model tasarımcısında, Diyagram Görünümünde, **Employee** tablo üst bilgisine sağ tıklayın ve **İstemci Araçlarından Gizle**'ye tıklayın.  
  
## <a name="create-a-sales-employees-by-territory-user-role"></a>Sales Employees by Territory kullanıcı rolü oluşturma  
Bu görevde bir kullanıcı rolü oluşturacaksınız. Bu rol, DimSalesTerritory tablosunun hangi satırlarının kullanıcılar tarafından görüneceğini belirleyen bir satır filtresi içerir. Ardından bu filtre bir-çok ilişki yönünde DimSalesTerritory ile ilişkili diğer tüm tablolara uygulanır. Ayrıca EmployeeSecurity tablosunun tamamının role üye olan kullanıcılar tarafından sorgulanabilir olmasını engelleyen bir filtre uygulayacaksınız.  
  
> [!NOTE]  
> Bu derste oluşturacağınız Sales Employees by Territory rolü, üyelerin yalnızca ait oldukları satış bölgesindeki satış verilerine göz atmasını (veya sorgulamasını) sağlayacak. Sales Employees by Territory rolüne aynı zamanda [Ders 11: Rol Oluşturma](../tutorials/aas-lesson-11-create-roles.md) sırasında oluşturduğunuz bir role üye olan bir kullanıcı eklerseniz izinler birleştirilir. Birden fazla rolün üyesi olan kullanıcılar her bir rol için tanımlanan izinlerin ve satır filtrelerinin birleşimine sahip olur. Başka bir deyişle kullanıcı rol birleşimi ile verilen en geniş izinlere sahip olur.  
  
#### <a name="to-create-a-sales-employees-by-territory-user-role"></a>Sales Employees by Territory kullanıcı rolü oluşturmak için  
  
1.  SSDT'de **Model** menüsüne ve ardından **Roller**'e tıklayın.  
  
2.  **Rol Yöneticisi**'nde **Yeni**'ye tıklayın.  
  
    Listeye Yok iznine sahip yeni bir rol eklenir.  
  
3.  Yeni role tıklayın ve ardından **Ad** sütununda rolü **Sales Employees by Territory** olarak adlandırın.  
  
4.  **İzinler** sütununda açılan listeye tıklayın ve ardından **Okuma** iznini seçin.  
  
5.  **Üyeler** sekmesine ve ardından **Ekle**'ye tıklayın.  
  
6.  **Kullanıcı veya Grup Seç** iletişim kutusunda **Seçilecek nesne adını girin** bölümüne EmployeeSecurity tablosunu oluştururken kullandığınız ilk örnek kullanıcı adını girin. **Adları Denetle**'ye tıklayarak kullanıcı adının geçerli olup olmadığını doğrulayın ve ardından **Tamam**'a tıklayın.  
  
    Bu adımı tekrarlayın ve EmployeeSecurity tablosunu oluştururken kullandığınız diğer örnek kullanıcı adlarını ekleyin.  
  
7.  **Satır Filtreleri** sekmesine tıklayın.  
  
8.  **EmployeeSecurity** tablosunun **DAX Filtresi** sütununa şu formülü yazın:  
  
    ```
      =FALSE()  
    ```
  
    Bu formül tüm sütunların false Boolean değerini vermesi gerektiğini belirtir. EmployeeSecurity tablosunun hiçbir sütunu Sales Employees by Territory kullanıcı rolünün üyeleri tarafından sorgulanamaz.  
  
9. **DimSalesTerritory** tablosu için şu formülü yazın:  

    ```  
    ='Sales Territory'[Sales Territory Id]=LOOKUPVALUE('Employee Security'[Sales Territory Id], 
      'Employee Security'[Login Id], USERNAME(), 
      'Employee Security'[Sales Territory Id], 
      'Sales Territory'[Sales Territory Id]) 
    ```
  
    Bu formülde LOOKUPVALUE işlevi DimEmployeeSecurity[SalesTerritoryId] sütunu için tüm değerleri döndürür ve burada EmployeeSecurity[LoginId], oturum açmış olan Windows kullanıcı adıyla aynıdır ve EmployeeSecurity[SalesTerritoryId], DimSalesTerritory[SalesTerritoryId] ile aynıdır.  
  
    LOOKUPVALUE tarafından döndürülen satış bölgesi kimlikleri, DimSalesTerritory tablosunda gösterilen satırları sınırlamak için kullanılır. Satır için SalesTerritoryID değerinin LOOKUPVALUE işlevi tarafından döndürüldüğü kimlik kümesinde yer alan satırlar görüntülenir.  
  
10. Rol Yöneticisi'nde **Tamam**'a tıklayın.  
  
## <a name="test-the-sales-employees-by-territory-user-role"></a>Sales Employees by Territory kullanıcı rolünü test etme  
Bu görevde SSDT'nin Excel'de Çözümleme özelliğini kullanarak Sales Employees by Territory kullanıcı rolünün çalışıp çalışmadığını test edeceksiniz. EmployeeSecurity tablosuna ve bu role eklediğiniz kullanıcı adlarından birini belirteceksiniz. Ardından bu kullanıcı adı Excel ile model arasında oluşturulan bağlantıda geçerli kullanıcı olarak kullanılacak.  
  
#### <a name="to-test-the-sales-employees-by-territory-user-role"></a>Sales Employees by Territory kullanıcı rolünü test etmek için  
  
1.  SSDT'de **Model** menüsüne ve ardından **Excel'de Çözümleme**'ye tıklayın.  
  
2.  **Excel'de Çözümleme** iletişim kutusunun **Modele bağlanmak için kullanılacak kullanıcı adını veya rolü belirtin** bölümünde **Başka Windows Kullanıcısı**'nı seçin ve **Göz at**'a tıklayın.  
  
3.  **Kullanıcı veya Grup Seç** iletişim kutusunun **Seçilecek nesne adını girin** bölümüne EmployeeSecurity tablosuna eklediğiniz kullanıcı adlarından birini girin ve **Adları Denetle**'ye tıklayın.  
  
4.  **Tamam**'a tıklayarak **Kullanıcı veya Grup Seç** iletişim kutusunu kapatın ve tekrar **Tamam**'a tıklayarak **Excel'de Çözümleme** iletişim kutusunu kapatın.  
  
    Excel yeni bir çalışma kitabı ile açılır. Bir PivotTable otomatik olarak oluşturulur. PivotTable Alanları listesi, yeni modelinizde bulunan veri alanlarının çoğunu içerir.  
  
    EmployeeSecurity tablosunun PivotTable Alanları listesinde görünmediğine dikkat edin. Bu tabloyu önceki görevlerden birinde istemci araçlarından gizlemiştiniz.  
  
5.  **Alanlar** listesinin **∑ Internet Sales** (ölçüler) bölümünde **InternetTotalSales** ölçüsünü seçin. Ölçü **Değerler** alanlarına girilir.  
  
6.  **SalesTerritoryId** sütununu **DimSalesTerritory** tablosundan seçin. Sütun **Satır Etiketleri** alanına girilir.  
  
    İnternet satış rakamlarının yalnızca kullandığınız kullanıcı adının ait olduğu bölge için gösterildiğine dikkat edin. Satır Etiketi olarak DimGeography tablosunun City etiketi gibi başka bir sütun seçerseniz yalnızca etkili kullanıcının ait olduğu satış bölgesindeki şehirler gösterilir.  
  
    Bu kullanıcı ait olduğunun haricindeki İnternet satış verilerine göz atamaz veya bu verileri sorgulayamaz. Bu kısıtlamanın nedeni DimSalesTerritory tablosu için Sales Employees by Territory kullanıcı rolünde tanımlanmış olan satır filtresinin diğer satış bölgeleriyle ilgili verileri korumasıdır.  
  
## <a name="see-also"></a>Ayrıca Bkz.  
[USERNAME İşlevi (DAX)](https://msdn.microsoft.com/library/hh230954.aspx)  
[LOOKUPVALUE İşlevi (DAX)](https://msdn.microsoft.com/library/gg492170.aspx)  
[CUSTOMDATA İşlevi (DAX)](https://msdn.microsoft.com/library/hh213140.aspx)  
