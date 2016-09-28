## Event Hub'ı oluşturma

1. [Azure Portal][]’da oturum açın ve ekranın sol üst köşesindeki **Yeni**’ye tıklayın.

2. **Veri + Analiz**’e, ardından **Event Hubs**’a tıklayın.

    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)

3. **Ad alanı oluştur** dikey penceresine bir ad alanı adı girin. Adın kullanılabilirliği sistem tarafından hemen denetlenir.

    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)

4. Ad alanı adının kullanılabilir durumda olduğundan emin olduktan sonra fiyatlandırma katmanını (Temel veya Standart) seçin. Ayrıca, bir Azure aboneliği, kaynak grubu ve kaynağın oluşturulacağı konumu seçin. 

2. Ad alanını oluşturmak için **Oluştur**’a tıklayın.

6. Event Hubs ad alanı listesinde yeni oluşturulan ad alanına tıklayın.      

    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)

7. Ad alanı dikey penceresinde **Event Hubs**’a tıklayın.

    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)

8. Dikey pencerenin en üstündeki **Olay Hub’ı Ekle** seçeneğine tıklayın.

    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)

3. Olay Hub'ı için bir ad yazın, ardından **Oluştur**’a tıklayın.

    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)

4. Event Hubs listesinde yeni oluşturulan Olay Hub’ı adına tıklayın. 

    ![](./media/event-hubs-create-event-hub/create-event-hub6.png)

5. Ad alanı dikey penceresine (ilgili Olay Hub’ı dikey penceresine değil) geri dönerek **Paylaşılan erişim ilkeleri** ve ardından **RootManageSharedAccessKey** öğesine tıklayın.

    ![](./media/event-hubs-create-event-hub/create-event-hub7.png)

5. **RootManageSharedAccessKey** bağlantı dizesini panoya kopyalamak için kopyala düğmesine tıklayın. Daha sonra öğreticide kullanmak üzere bu bağlantı dizesini kaydedin.

    ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Olay Hub'ınız artık oluşturuldu; şimdi olayları alıp göndermek için gereken bağlantı dizelerine sahipsiniz.

[Azure Portal]: https://portal.azure.com/

<!--HONumber=Sep16_HO3-->


