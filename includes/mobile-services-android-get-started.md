Bu öğreticinin son aşaması yeni uygulamanızı oluşturmak ve çalıştırmaktır.

### Projeyi Android Studio uygulamasına yükleme ve Gradle’ı eşitleme

1. Sıkıştırılmış proje dosyalarını kaydettiğiniz konuma göz atın ve bilgisayarınızdaki dosyaları Android Studio projeleri dizininde genişletin.

2. Android Studio’yu açın. Bir proje üzerinde çalışıyorsanız ve bu görüntüleniyorsa projeyi kapatın (Dosya = > Projeyi Kapat).

3. **Var Olan Android Studio projesini aç**’ı seçin, proje konumuna göz atın ve ardından **Tamam**’a tıklayın. Böylece proje yüklenecek ve Gradle ile eşitlenmesi başlayacaktır.

    ![](./media/mobile-services-android-get-started/android-studio-import-project.png)

4. Gradle eşitleme işleminin tamamlanmasını bekleyin. "Hedef bulunamadı" hatasını görürseniz, bunun nedeni, Android Studio’da kullanılan sürümün örnektekiyle eşleşmemesidir. Bu sorunu gidermenin için en kolay yolu hata iletisindeki **Eksik platformları yükle ve projeyi eşitle** bağlantısına tıklamaktır. Ek sürüm hatası iletileri alabilirsiniz; hiç hata almayana kadar bu işlemi tekrarlamanız yeterlidir.
    - "En son ve en iyi" Android sürümüyle çalıştırmak istiyorsanız bu sorunu gidermenin başka bir yolu vardır. **SDK Yöneticisi** simgesine tıklayarak ve hangi sürümün listelendiğine bakarak bulabildiğiniz makinenizde zaten yüklü olan sürümle eşleştirmek için *app* dizinindeki *build.gradle* dosyasında **targetSdkVersion** girişini güncelleştirebilirsiniz. Bundan sonra **Projeyi Gradle Dosyalarıyla Eşitle**’ye basın. Derleme Araçları sürümüyle ilgili bir hata iletisini alabilirsiniz; bunu da aynı şekilde düzeltebilirsiniz.

### Uygulamayı çalıştırma

Öykünücüyü veya gerçek bir cihazı kullanarak uygulamayı çalıştırabilirsiniz.

1. Cihazdan çalıştırmak için bunu bir USB kablosuyla bilgisayarınıza bağlayın. [Geliştirme için cihazı kurmalısınız](https://developer.android.com/training/basics/firstapp/running-app.html). Bir Windows makinesinde geliştiriyorsanız USB sürücüsünü de indirip yüklemelisiniz.

2. Android öykünücüsünü kullanarak projeyi çalıştırmak için en az bir Android Sanal Cihazı (AVD) tanımlamanız gerekir. Bu cihazları oluşturmak ve yönetmek için AVD Yöneticisi simgesine tıklayın.

3. **Çalıştır** menüsünde **Çalıştır** seçeneğine tıklayarak projeyi başlatın. görüntülenen iletişim kutusundan da bir cihaz veya öykünücü seçin.

4. Uygulama görüntülendiğinde, _Öğreticiyi tamamla_ gibi anlamlı bir metin yazın ve ardından **Ekle** seçeneğine tıklayın.

    ![](./media/mobile-services-android-get-started/mobile-quickstart-startup-android.png)

    Bu, Azure üzerinde barındırılan yeni mobil hizmete bir POST isteği gönderir. İstekten alınan veriler TodoItem tablosuna eklenir. Tabloda depolanan öğeler mobil hizmet tarafından döndürülür ve veriler listede görüntülenir.

    > [AZURE.NOTE] Sorgulamak ve ToDoActivity.java dosyasında bulunan verileri eklemek için, mobil hizmetinize erişen kodu gözden geçirebilirsiniz.

8. **Klasik Azure Portalı**’na geri dönün, **Veri** sekmesine ve sonra TodoItems tablosuna tıklayın.

    ![](./media/mobile-services-android-get-started/mobile-data-tab1.png)

    Bu, uygulama tarafından tabloya eklenen verilere göz atmanızı sağlar.

    ![](./media/mobile-services-android-get-started/mobile-data-browse.png)


<!--HONumber=Sep16_HO3-->


