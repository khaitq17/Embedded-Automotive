# CAN
CAN (Controller Area Network) là giao thức giao tiếp nối tiếp hỗ trợ mạnh cho những hệ thống điều khiển thời gian thực phân bố (distributed realtime control system).

CAN đặc biệt được ứng dụng nhiều trong ngành công nghiệp ô tô. 

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/5e0c0725-c7ba-4604-9310-c6c803367ee8)

Mỗi khối được điều khiển bởi 1 vi điều khiển riêng biệt, và thông qua CAN Bus để kết nối, giao tiếp với nhau. Và thời gian phản hồi của mỗi khối VĐK là cực kì nhanh.
## Mạng CAN
CAN có đường dây dẫn gồm 2 dây CAN_H (CAN High) và CAN_L (CAN Low). Các thiết bị được nối chung trên 2 dây này và được gọi là 1 Node trong mạng.

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/68f12553-8b0b-4dc6-a9ae-c60abc5c7ff9)

## CAN Node
Mạng CAN được tạo thành bởi 1 nhóm các Node.

1 Node bao gồm:
- Một thiết bị hỗ trợ xử lý điện áp trên Bus là CAN Transceiver.
- Một MCU có hỗ trợ CAN Controller (giao thức CAN). MCU ngoài việc nhận và xử lý data còn thực hiện chức năng của Node.  

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/d8958672-8ccc-4b34-b280-b34147b0286e)

## CAN Transceiver
Bộ Transceiver hỗ trợ cho 1 Node truyền dữ liệu lên Bus đồng thời nhận lại tín hiệu vừa truyền đi.

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/a7b8a6cc-987e-4dd8-a7e9-f77bccd868b6)

## Trạng thái "dominant" và "recessive"
Can Bus định nghĩa 2 trạng thái điện áp là "dominant" (mức 0) và "recessive" (mức 1). Hai trạng thái này được xử lý bởi Transceiver của Node. Có 2 loại CAN tương ứng với các giá trị điện áp khác nhau.

- CAN low speed

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/2c3c7083-c0a9-4804-872f-3bab731012a5)
|  | Trạng thái recessive | Trạng thái dominant|
|:--:|:--:|:--:|
| CAN_H | 1,75 V | 4 V |
| CAN_L | 3,25 V | 1 V |

- CAN high speed

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/63dc0e5f-5d54-4dcb-883b-8404f71ed1d8)
|  | Trạng thái recessive | Trạng thái dominant|
|:--:|:--:|:--:|
| CAN_H | 2,5 V | 3,5 V |
| CAN_L | 2,5 V | 1,5 V |

## Tính chất
Vì tính chất vi sai trên đường truyền tín hiệu của CAN Bus, tín hiệu nhiễu sẽ ảnh hưởng đến giá trị điện áp ở trên cả CAN H và CAN L.

CAN Bus thường được **xoắn 2 dây vào nhau**. Khi đó tín hiệu nhiễu sẽ đều trên cả 2 dây CAN_H và CAN_L, lúc này nhiễu sẽ bị triệt tiêu.

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/66ab8052-853f-4bb5-866c-7ccd06be6c5d)

## Nguyên tắc hoạt động
- Dữ liệu truyền từ 1 Node bất ký trên CAN Bus không truyền địa chỉ của Node đó, cũng không truyền địa chỉ của Node muốn giao tiếp.
- Thay vào đó, dữ liệu truyền đi được gắn nhãn bởi 1 số nhận dạng (ID) duy nhất trên toàn mạng.

  Ví dụ: Có 8 bit ID: 0000 0000. Bit 1 liên quan đến động cơ, bit 2 liên quan đến phanh ABS, bit 3 liên quan đến bánh xe,... Khi set bit tương ứng lên 1 thì các Node liên quan sẽ xử lý thông điệp được truyền đi.
- Tất cả các Node khác đều nhận được thông điệp, mỗi Node tự kiểm tra mã ID để xem thông điệp có liên quan tới Node đó hay không. Nếu có liên quan quan, nó sẽ xử lý còn không thì bỏ qua.

## Tranh chấp trên Bus
Giao thức CAN cho phép các Node khác nhau đưa dữ liệu cùng lúc. 

Data Frame và Remote Frame làm việc theo cơ chế phân xử quyền ưu tiên của tín hiệu. Vì thế cấu trúc của chúng có vùng phân xử quyền ưu tiên.

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/de6a4bf9-bda3-4a99-9b1a-65df15455988)

Khi phân xử, bit ID có giá trị 0 sẽ được ưu tiên hơn. 

## CAN Frame

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/1311974e-42c1-4cb1-812b-ebe494496778)

CAN định nghĩa 4 loại Frame:
- **Data Frame**: Dùng khi Node muốn truyền dữ liệu tới các Node khác.
- **Remote Frame**: Dùng để yêu cầu truyền data frame.
- **Error Frame** và **Overload Frame**: Dùng trong việc xử lý lỗi.

## Data Frame
Có 2 dạng là **Standard Frame** và **Extended Frame**.

Cấu trúc của 1 data frame:

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/89c30093-165e-4f6c-8569-34a28697412e)


### Standard Frame

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/3e34403a-df6b-4cda-9f24-11501ba23266)

- **Start Of Frame Field – SOF** (Trường bắt đầu): Độ dài 1 bit đầu của frame, giá trị Dominant báo hiệu bắt đầu của 1 frame.
- **Arbitration Field** (Trường xác định quyền ưu tiên): Có độ dài 12 bit, bao gồm 11 bit ID và 1 bit RTR.
    - **Bit RTR** (Remote Transmission Request): Dùng để phân biệt khung là **Data Frame** hay **Remote Frame** (0 - Data frame, 1 - Remote frame).
- **Control Field** (Trường điều khiển): Khung chuẩn gồm IDE, r0 và DLC (Data Length Code).
    - **Bit IDE** (Identifier Extension): Bit phân biệt giữa loại khung chuẩn và khung mở rộng: IDE = 0 - khung chuẩn.
    - **Bit r0** (bit dự trữ): Phải được truyền là Recessive Bit bởi bộ truyền nhưng bộ nhận không quan tâm đến giá trị bit này. 
    - **Bit DLC** (Data Length Code): Có độ dài 4 bit quy định số byte của trường dữ liệu của Data Frame, Chỉ được mang giá trị từ 0 đến 8.
- **Data Field** (Trường dữ liệu): Chứa data, có độ dài từ 0 đến 8 byte tùy vào giá trị của DLC ở trường điều khiển.
- **Cyclic Redundancy Check Field – CRC** (Trường kiểm tra): Gồm 16 bit và được chia làm 2 phần:
    - **CRC Sequence**: Gồm 15 bit CRC tuần tự, tính toán mã hóa dữ liệu. Khi nhận sẽ kiểm tra xem mã hóa đúng hay sai.
    - **CRC Delimiter**: Là một Recessive Bit làm nhiệm vụ phân cách trường CRC với trường phía sau.
- **Acknowledge Field – ACK** (Trường xác nhận): Có độ dài 2 bit và bao gồm 2 phần:
    - **ACK Slot**: Có độ dài 1 bit, Node truyền dữ liệu sẽ truyền bit này là Recessive. Khi một hoặc nhiều Node nhận chính xác giá trị thông điệp (không có lỗi và đã so sánh CRC Sequence trùng khớp) thì nó sẽ báo lại cho bộ truyền bằng cách truyền Dominant Bit ngay vị trí ACK Slot (tương tự việc kéo SDA trong I2C).
    - **ACK Delimiter**: Có độ dài 1 bit, nó luôn là một Recessive Bit ⇒ ACK Slot luôn được đặt giữa hai Recessive Bit là CRC Delimiter và ACK Delimiter.
- **End Of Frame Field – EOF** (Trường kết thúc): Thông báo kết thúc một Data Frame hay Remote Frame. Trường này gồm 7 Recessive Bit.

### Extended Frame

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/44e167be-b619-4060-a749-8d3a65424ace)

- **Start Of Frame Field – SOF** (Trường bắt đầu): Độ dài 1 bit đầu của frame, giá trị Dominant báo hiệu bắt đầu của 1 frame.
- **Arbitration Field** (Trường xác định quyền ưu tiên): Có độ dài 32 bit, bao gồm 29 bit ID, 1 bit SRR, 1 bit IDE và 1 bit RTR.
    - **Bit RTR** (Remote Transmission Request): Dùng để phân biệt khung là Data Frame hay Remote Frame (0- Data frame, 1- Remote frame).
    - **Bit SRR** (Substitute Remote Request): Chỉ có ở khung mở rộng, có giá trị là 1.
    - **Bit IDE** (Identifier Extension): Bit phân biệt giữa loại khung chuẩn và khung mở rộng: IDE = 1 - khung mở rộng. 
- **Control Field** (Trường điều khiển): Khung mở rộng gồm r1, r0 và DLC (Data Length Code).
    - **Bit r0, r1** (hai bit dự trữ): Phải được truyền là Recessive Bit bởi bộ truyền nhưng bộ nhận không quan tâm đến giá trị 2 bit này. 
    - **Bit DLC** (Data Length Code): Có độ dài 4 bit quy định số byte của trường dữ liệu của Data Frame, Chỉ được mang giá trị từ 0 đến 8.
- **Data Field** (Trường dữ liệu): Chứa data, có độ dài từ 0 đến 8 byte tùy vào giá trị của DLC ở trường điều khiển.
- **Cyclic Redundancy Check Field – CRC** (Trường kiểm tra): Gồm 16 bit và được chia làm 2 phần:
    - **CRC Sequence**: Gồm 15 bit CRC tuần tự, tính toán mã hóa dữ liệu. Khi nhận sẽ kiểm tra xem mã hóa đúng hay sai.
    - **CRC Delimiter**: Là một Recessive Bit làm nhiệm vụ phân cách trường CRC với trường phía sau.
- **Acknowledge Field – ACK** (Trường xác nhận): Có độ dài 2 bit và bao gồm 2 phần:
    - **ACK Slot**: Có độ dài 1 bit, Node truyền dữ liệu sẽ truyền bit này là Recessive. Khi một hoặc nhiều Node nhận chính xác giá trị thông điệp (không có lỗi và đã so sánh CRC Sequence trùng khớp) thì nó sẽ báo lại cho bộ truyền bằng cách truyền Dominant Bit ngay vị trí ACK Slot (tương tự việc kéo SDA trong I2C).
    - **ACK Delimiter**: Có độ dài 1 bit, nó luôn là một Recessive Bit ⇒ ACK Slot luôn được đặt giữa hai Recessive Bit là CRC Delimiter và ACK Delimiter.
- **End Of Frame Field – EOF** (Trường kết thúc): Thông báo kết thúc một Data Frame hay Remote Frame. Trường này gồm 7 Recessive Bit.

## Remote Frame
Cấu trúc 1 Remote Frame tương tự Data frame, tuy nhiên Remote Frame có Bit RTR = 1 và không có trường data (Bit DLC = 0).

## Error Frame
Gồm 2 phần:
- Cờ lỗi: Có 2 loại
    - **Error Flag**: 6 bits
    - **Superposition of Error Flags**: 6-12 bits
- **Error Delimiter**: 8 bits 1 liên tiếp

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/e4a45569-6997-48b9-b452-e39a7dbdd8ee)

## Overload Frame
Dùng khi Frame bị tràn bộ đệm. Khi Node nhận quá nhiều dữ liệu khổng thể xử lý kịp, nó sẽ gửi đi Overload Frame.

## Sơ đồ đấu nối

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/cc24c5f1-9ac0-4e1a-a556-78707cb9aad5)

## Cấu hình CAN
- Tương tự các ngoại vi, CAN được cấp xung từ APB1.

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/c689e217-875c-4e88-a78c-8a5d397d0b82)

- Cấu hình cho 2 chân TX và RX của bộ CAN.
    - RX (PA11): Mode In_Floating
    - TX (PA12): Mode AF_PP

Các tham số cho CAN được cấu hình trong struct **CAN_InitTypeDef** bao gồm:
- **CAN_TTCM**: Chế độ giao tiếp được kích hoạt theo thời gian ấn định khoảng thời gian khi truyền message.
- **CAN_ABOM**: Quản lý ngắt bus tự động. Nếu trong quá trình truyền xảy ra lỗi, bus sẽ được ngắt. Bit này quy định việc CAN có quay về trạng thái bình thường hay không.
- **CAN_AWUM**: Chế độ đánh thức tự động. Nếu CAN hoạt động ở SleepMode, Bit này quy định việc đánh thức CAN theo cách thủ công hay tự động.
- **CAN_NART**: Không  tự động truyền lại. CAN sẽ thử lại để truyền tin nhắn nếu các lần thử trước đó không thành công. Nếu set bit này thì sẽ không truyền lại. Nên set bit khi sử dụng chung với **CAN_TTCM**, nếu không thì nên để = 0;
- **CAN_RFLM**: Chế độ khóa nhận FIFO. Chế độ khóa bộ đệm khi đầy.
- **CAN_TXFP**: Ưu tiên truyền FIFO. Đặt bit này =0, ưu tiên truyền các gói có ID thấp hơn. Nếu đặt lên 1, CAN sẽ ưu tiên các gói theo thứ tự trong bộ đệm.
- **CAN_Mode**: Chế độ CAN:
    - **CAN_Mode_Normal**: Gửi message thông thường.
    - **CAN_Mode_LoopBack**: Các message gửi đi sẽ được lưu vào bộ nhớ đệm.
    - **CAN_Mode_Silent**: Chế độ chỉ nhận.
    - **CAN_Mode_Silent_LoopBack**: Kết hợp giữa 2 mode trên.
- **CAN_Prescaler**: Cài đặt giá trị chia để tạo time quatum. 
    - `fCan` = `sysclk` / `CAN_Prescaler`
    - `1tq` = 1 / `fCan`
- **CAN_SJW**: Thời gian trễ phần cứng, tính theo tq.
- **CAN_BS1**: Thời gian đồng bộ đầu frame truyền, tính theo tq.
- **CAN_BS2**: Thời gian đồng bộ cuối frame truyền, tính theo tq.

Tốc độ truyền CAN = 1/(`SJW`+`BS1`+`BS2`) 

## Cấu hình CAN Mask & Filter
CAN hỗ trợ bộ lọc ID, giúp các Node có thể lọc ra ID từ các message trên Bus để quyết định sẽ nhận massge nào. Các tham số cho bộ lọc được cấu hình trong **CAN_FilterInitTypeDef**:
- **CAN_FilterNumber**: Chọn bộ lọc để dùng, từ 0-13.
- **CAN_FilterMode**: Chế độ bộ lọc: 
    - **IdMask**: Sử dụng mặt nạ bit để lọc ID.
    - **IdList**: Không sử dụng mặt nạ bit.
- **CAN_FilterScale**: Kích thước của bộ lọc, 32 hoặc 16 bit.
- **CAN_FilterMaskIdHigh** và **CAN_FilterMaskIdLow**: Giá trị cài đặt cho Mask, 32 bits.
- **CAN_FilterIdHigh** và **CAN_FilterIdLow**:  Giá trị cài đặt cho bộ lọc, 32bits.
- **CAN_FilterFIFOAssignment**: Chọn bộ đệm cần áp dụng bộ lọc.
- **CAN_FilterActivation**: Kích hoạt bộ lọc ID.

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/9396c7b8-ec3e-4b42-96ff-0e5b69c54539)

|Mask Bit n|Filter Bit n|Message Identifier Bit|Accept or Reject Bit n|
|:--:|:--:|:--:|:--:|
|0|x|x|Accept|
|1|0|0|Accept|
|1|0|1|Reject|
|1|1|0|Reject|
|1|1|1|Accept|

## Giá trị bộ lọc & Giá trị ID trong massage
Thanh ghi chứa giá trị ID của gói tin

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/0e78e5df-bfac-4696-9990-9a4b9ca9ce62)

Để áp dụng được Mask và ID cho gói tin với ID là stdID, cần setup 11 bit cao của Mask cùng như của Filter.

## Truyền - Nhận CAN
Để xác định được 1 gói tin, cần có **ID**, các bit **RTR**, **IDE**, **DLC** và tối đa 8 byte data như bài trước đã đề cập. Các thành phần này được tổ chức trong **CanRxMsg**.

Hàm truyền: **uint8_t CAN_Transmit(CAN_TypeDef CANx, CanTxMsg TxMessage)**:
- **CANx**: Bộ CAN cần dùng.
- **TxMessage**: Struct **CanRxMsg** cần truyền.

```
    CanTxMsg TxMessage;

    TxMessage.StdId = 0x123; // 11bit ID voi che do std
    TxMessage.ExtId = 0x00;
    TxMessage.RTR = CAN_RTR_DATA;
    TxMessage.IDE = CAN_ID_STD;
    TxMessage.DLC = len;
	
    for (int i = 0; i < len; i++)
    {
        TxMessage.Data[i] = data[i];
    }

    CAN_Transmit(CAN1, &TxMessage);
```

Gói tin nhận được sẽ được lưu dưới dạng **CanRxMsg** struct. Gồm các thành phần tương tự **CanTxMsg** của gói tin nhận được.

Hàm **CAN_MessagePending(CAN_TypeDef CANx, uint8_t FIFONumber)**: Trả về số lượng gói tin đang đợi trong FIFO của bộ CAN. Dùng hàm này để kiểm tra xem bộ CAN có đang truyền nhận hay không, nếu FIFO trống thì có thể nhận.

Hàm **CAN_Receive(CAN_TypeDef CANx, uint8_t FIFONumber, CanRxMsg RxMessage)**: Nhận về 1 gói tin từ bộ **CANx**, lưu vào **RxMessage**.

```
    CanRxMsg RxMessage;
    while (CAN_MessagePending(CAN1, CAN_FIFO0) <1 );
    CAN_Receive(CAN1, CAN_FIFO0, &RxMessage);
	ID = RxMessage.StdId;
    for (int i = 0; i < RxMessage.DLC; i++)
    {
        TestArray[i] = RxMessage.Data[i];
    }
```

# AUTOSAR
AUTOSAR có 2 tiêu chuẩn: **Classic AUTOSAR** và **Adaptive AUTOSAR**
- **Classic AUTOSAR**: Những tính năng đơn giản (điều hòa, điều khiển động cơ, gạt nước mưa,...), chủ yếu dùng C
- **Adaptive AUTOSAR**: Những tính năng phức tạp hơn (radar, hỗ trợ người lái,...), chủ yếu dùng C++
## Cấu trúc AUTOSAR 

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/006984a4-d7db-4a1f-80e0-525601145141)

- **Microcontroller** (Vi điều khiển): Lập trình thanh ghi
- **Microcontroller abstraction layer**: Sử dụng các device GPIO, SPI, I2C, UART, CAN,...
- **ECU abstraction layer**: Code những tính năng cụ thể hơn.
- **Service layer**: Cung cấp các dịch vụ khác nhau cho các application sử dụng.
- **Runtime Environment (RTE)**: Đây là một trong những layer quan trọng nhất của AUTOSAR, Application Layer sử dụng layer này trong khi giao tiếp với các layer bên dưới bằng các ports.
- **Application Layer**: Chứa code ứng dụng, bao gồm các chương trình điều khiển và các dịch vụ ứng dụng khác nhau.
- **Complex Device Driver (CDD)**: Chứa những tính răng riêng biệt của từng hãng xe

Ví dụ:
![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/6cc53afe-56a1-45df-aff2-b236f1bf979a)



























