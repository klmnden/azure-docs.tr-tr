
İndirdiğiniz mobil hizmet projesi yeni mobil hizmetinizi hemen kendi yerel bilgisayarınızda veya sanal makinenizde çalıştırmanızı sağlar. Böylece, Azure’da yayımlamadan önce bile hizmet kodunuzun hatalarını ayıklamak kolaylaşır.

Bu bölümde, yeni uygulamanızı yerel olarak çalışan mobil hizmete karşı sınayacaksınız.

1. Sıkıştırılmış proje dosyalarını kaydettiğiniz konuma göz atın, bilgisayarınızdaki dosyaları genişletin ve Visual Studio’da çözüm dosyasını açın.

2. Proje oluşturup mobil hizmeti yerel olarak başlatmak için Visual Studio'daki Çözüm Gezgini'nde hizmet projenize sağ tıklayın, **Başlangıç Projesi Olarak Ayarla**’ya tıklayın, ardından **F5** tuşuna basın.

    ![](./media/mobile-services-dotnet-backend-test-local-service-dotnet/mobile-service-startup.png)

    Mobil hizmet sorunsuz başlatıldıktan sonra bir web sayfası görüntülenir.

3. Mağaza uygulamasını sınamak amacıyla, projeyi yeniden oluşturup uygulamayı başlatmak için istemci uygulaması projenize sağ tıklayın, **Başlangıç Projesi Olarak Ayarla**’ya tıklayın ve **F5** tuşuna basın.

    Bu işlem, yerel mobil hizmet örneğine bağlanan uygulamayı başlatır.   

4. Uygulamada **TodoItem Ekle** bölümüne _Öğreticiyi tamamla_ gibi anlamlı bir metin yazın ve ardından**Kaydet**’e tıklayın.

    Bu, yerel mobil hizmete bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve veriler uygulamada ikinci sütunda görüntülenir.

<!--HONumber=Sep16_HO3-->


