# CAN
CAN (Controller Area Network) là giao thức giao tiếp nối tiếp hỗ trợ mạnh cho những hệ thống điều khiển thời gian thực phân bố (distributed realtime control system).

CAN đặc biệt được ứng dụng nhiều trong ngành công nghiệp ô tô.
![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/5e0c0725-c7ba-4604-9310-c6c803367ee8)

Mỗi khối được điều khiển bởi 1 vi điều khiển riêng biệt, và thông qua CAN Bus để kết nối, giao tiếp với nhau.
## Mạng CAN
CAN có đường dây dẫn gồm 2 dây CAN_H và CAN_L. Các thiết bị được nối chung trên 2 dây này và được gọi là 1 Node trong mạng.

![image](https://github.com/khaitq17/Embedded-Automotive/assets/159031971/68f12553-8b0b-4dc6-a9ae-c60abc5c7ff9)

## CAN Node
Mạng CAN được tạo thành bởi 1 nhóm các Node.

1 Node bao gồm:
- Một thiết bị hỗ trợ xử lý điện áp trên Bus - CAN Transceiver
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
Vì tính chất vi sai trên đường truyền tín hiệu của CAN Bus, tín hiệu nhiễu sẽ ảnh hưởng đến giá trị điện áp.

CAN Bus thường được xoắn 2 dây vào nhau. Khi đó tín hiệu nhiễu sẽ đều trên cả 2 dây CAN_H và CAN_L, lúc này nhiễu sẽ bị triệt tiêu.

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



























