## Service Bus kuyrukları nelerdir?

Service Bus kuyrukları **aracılı mesajlaşma** iletişim modelini destekler. Kuyruklar kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir kuyruk aracılığıyla iletileri değiş tokuş eder (aracı). İleti üreticisi (gönderen) iletiyi kuyruğa aktarır ve ardından işleme devam eder. Zaman uyumsuz olarak, ileti tüketicisi (alıcı) iletiyi kuyruktan alır ve bunu işler. Üreticinin işleme devam etmesi ve daha fazla ileti göndermesi için tüketiciden yanıt beklemesi gerekmez. Kuyruklar, bir veya birden çok rakip tüketiciye **İlk Giren İlk Çıkar (FIFO)** yöntemine göre ileti teslimi sunar. Bu da, genellikle iletilerin kuyruğa eklendiği bir düzende alıcılar tarafından alınıp işleneceği ve her iletinin tek bir ileti tüketicisi tarafından alınıp işleneceği anlamına gelir.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Service Bus kuyrukları çok sayıda çeşitli senaryolar için kullanılabilen genel amaçlı bir teknolojidir:

-   Çok katmanlı bir Azure uygulamasında web ve çalışan rolleri arasındaki iletişim.
-   Karma bir çözümde şirket içi uygulamalar ve Azure barındırmalı uygulamalar arasındaki iletişim.
-   Farklı kuruluşlarda veya bir kuruluşun farklı departmanlarında şirket içi çalışan dağıtılmış bir uygulamanın bileşenleri arasındaki iletişim.

Kuyrukların kullanılması uygulamalarınızı daha kolay ölçeklendirmenizi ve mimarinizi daha dayanıklı hale getirmenizi sağlar.

## Hizmet ad alanı oluşturma

Azure'da Service Bus kuyruklarını kullanmaya başlamak için öncelikle bir hizmet ad alanı oluşturmanız gerekir. Ad alanı, uygulamanızda bulunan Service Bus kaynaklarını adreslemek için içeriğin kapsamını belirleyen bir kapsayıcı sunar.

Ad alanı oluşturmak için:

1.  [Klasik Azure portalı][]’nda oturum açın.

2.  Portalın sol gezinti bölmesinde **Service Bus** hizmetine tıklayın.

3.  Portalın alt bölmesinde **Oluştur**'a tıklayın.
    ![](./media/howto-service-bus-queues/sb-queues-03.png)

4.  **Yeni bir ad alanı ekle** iletişim kutusunda ad alanı adını girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.   
    ![](./media/howto-service-bus-queues/sb-queues-04.png)

5.  Ad alanındaki adın kullanılabilirliğinden emin olduktan sonra, ad alanınızın barındırılması gereken ülkeyi veya bölgeyi seçin (işlem kaynaklarınızın dağıtıldığı aynı ülkeyi/bölgeyi kullandığınızdan emin olun).

     > [AZURE.IMPORTANT] Uygulamanızı dağıtmak için seçmeyi planladığınız **aynı bölgeyi** seçin. Bu en iyi performansı verir.

6.  İletişim kutusundaki diğer alanları varsayılan değerleriyle bırakın (**Mesajlaşma** ve **Standart Katman**), ardından Tamam onay işaretine tıklayın. Artık sistem ad alanınızı oluşturur ve kullanıma açar. Sistem, hesabınıza yönelik kaynakları sağlarken birkaç dakika bekleyebilirsiniz.

    ![](./media/howto-service-bus-queues/getting-started-multi-tier-27.png)

Oluşturduğunuz ad alanı bir dakika içinde etkinleşir ve portalda görüntülenir. Devam etmeden önce ad alanı durumunun **Etkin** olmasını bekleyin.

## Ad alanı için varsayılan yönetim kimlik bilgilerini alma

Yeni ad alanında kuyruk oluşturma gibi yönetim işlemlerini gerçekleştirmek için ad alanına yönelik yönetici kimlik bilgilerini edinmeniz gerekir. Bu kimlik bilgilerini [Klasik Azure portalı][]’ndan elde edebilirsiniz.

###Yönetim kimlik bilgilerini portaldan almak için

1.  Kullanılabilir ad alanlarının listesini görüntülemek için sol gezinti bölmesinde **Service Bus** düğümüne tıklayın:   
    ![](./media/howto-service-bus-queues/sb-queues-13.png)

2.  Görüntülenen listede az önce oluşturduğunuz ad alanını seçin:   
    ![](./media/howto-service-bus-queues/sb-queues-09.png)

3.  **Bağlantı Bilgileri**'ne tıklayın.   
    ![](./media/howto-service-bus-queues/sb-queues-06.png)

4.  **Erişim bağlantısı bilgileri** bölmesinde, SAS anahtarını ve anahtar adını içeren bağlantı dizesini bulun.   

    ![](./media/howto-service-bus-queues/multi-web-45.png)
    
5.  Anahtarı not edin veya panoya kopyalayın.

  [Klasik Azure portalı]: http://manage.windowsazure.com




<!--HONumber=Jun16_HO2-->


