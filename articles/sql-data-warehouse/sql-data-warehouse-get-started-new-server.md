<properties
   pageTitle="Azure Portal'da SQL Data Warehouse veritabanı oluşturma | Microsoft Azure"
   description="Azure Portal'da Azure SQL Data Warehouse oluşturmayı öğrenin"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/03/2016"
   ms.author="lodipalm;"/>

# Yeni Bir Mantıksal Sunucu Oluşturma

SQL Database ve SQL Data Warehouse'da her veritabanı bir sunucuya, her sunucu da bir coğrafi konuma atanır. Sunucu, mantıksal SQL sunucusu olarak adlandırılır.

> [AZURE.NOTE] <a name="note"></a>Mantıksal SQL sunucusu:
  >
  > + Aynı coğrafi konumda bulunan birden fazla veritabanını yapılandırmak için tutarlı bir yol sunar.
  > + Şirket içi sunucularda olduğu gibi fiziksel bir donanım değildir. Hizmet yazılımının bir parçasıdır. Bu nedenle *mantıksal sunucu* olarak adlandırılır.
  > + Performanslarını etkilemeden birden çok veritabanını içinde barındırabilir.
  > + Adında küçük *s* harfi bulunur. SQL **s**unucusu bir Azure mantıksal sunucusu iken SQL **S**erver bir Microsoft şirket içi veritabanı ürünüdür.

1. **Sunucu** > **Yeni sunucu oluştur** öğesine tıklayın. Sunucu için herhangi bir ücret alınmaz. Zaten kullanmak istediğiniz bir V12 mantıksal SQL sunucunuz varsa var olan sunucunuzu seçip sonraki adıma ilerleyin.

    ![Yeni bir sunucu oluşturma](./media/sql-data-warehouse-get-started-provision/create-server.png)

2. **Yeni sunucu** bilgilerini girin.

    - **Sunucu Adı**: Mantıksal sunucunuz için bir ad girin. Bu ad, her bir coğrafi konum için benzersizdir.
    - **Sunucu Yöneticisi Adı**: Sunucu yöneticisi hesabı için bir kullanıcı adı girin.
    - **Parola**: Sunucu yöneticisi parolasını girin.
    - **Konum**: Sunucunuz için bir coğrafi konum belirleyin. Veri aktarım süresini azaltmak için sunucunuzu bu veritabanının erişeceği diğer veri kaynaklarına coğrafi olarak yakın biçimde konumlandırmak sizin için en iyisi olacaktır.
    - **V12 Sunucusu oluşturma**: EVET seçeneği, SQL Data Warehouse için geçerli olan tek seçenektir.
    - **Azure hizmetlerinin sunucuya erişimine izin ver**: SQL Data Warehouse için bu seçenek her zaman işaretlidir.

    >[AZURE.NOTE] Sunucu adını, sunucu yöneticisi adını ve parolayı bir yerde depoladığınızdan emin olun.  Bu bilgilere daha sonra sunucuda oturum açmak için ihtiyacınız olacak.

3. Mantıksal SQL sunucusu yapılandırma ayarlarını kaydetmek için **TAMAM**'a tıklayın ve SQL Data Warehouse dikey penceresine geri dönün.

    ![Yeni sunucuyu yapılandırma](./media/sql-data-warehouse-get-started-provision/configure-server.png)



<!---HONumber=Jun16_HO2-->


