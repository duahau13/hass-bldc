# Điều khiển BLDC PWM motor bằng esp8266
Mục đích sử dụng vi điều khiển esp8266 tạo xung PMW điều tốc các loại motor không chổi than dùng xung PWM, đặc biệt các model chuyên chạy quạt của Nidec có 3 dây (VCC, GND, PWM) rất dễ mua trên các trang bán hàng online như Shopee, Lazada,...

Cuối cùng tích hợp vào Home Assistant và Apple Homekit để điều khiển bằng giọng nói qua Siri.

![516b8f9af2a894ad63a6f6362e2707eb](https://user-images.githubusercontent.com/56484469/130795755-a8e43cad-ace3-4660-a304-ab39391690a2.jpg)

## Phần mềm
Code viết bằng ngôn ngữ YAML dựa trên [Esphome](https://esphome.io/), một add-on của [Home Assistant](https://home-assistant.io/) nên yêu cầu phải có sẵn Home Assistant và Esphome để compile thành firmware. Nếu không có sẵn Home Assistant, có thể download file .bin rồi flash mạch D1 mini để dùng bình thường (điều khiển bằng nút nhấn vật lý).

## Vật tư cần thiết
* Motor BLDC PWM (chạy 12-24V)
* D1 mini
* Rotary encoder
* IC ổn áp L7805CV
* Tụ 35V 470mF
* Trở 100K, 22K và 3,3K (xem phần voltage divider)
* Terminal 2P
* Công tắc
* Header 3P, 5P kèm dây ra
* Bảng mạch trống
* Dây điện

## Sơ đồ đấu nối
![BLDC_bb3](https://user-images.githubusercontent.com/56484469/130901478-66d3aef1-01fe-4480-96f7-ca965baab95d.png)

Một số lưu ý:
* Nguồn cấp vào 12-24V mắc song song tụ 35V 470mF hỗ trợ motor khởi động
* Nguồn vào IC L7805CV hạ áp xuống 5V cho D1 mini
* Rotary encoder lấy chung nguồn 5V từ L7805

Đấu dây Encoder vào D1 mini như sau:

D1 mini | Rotary encoder
------------ | -------------
5V | +
G | GND
D4 | SW
D3 | DT
D2 | CLK

## Mạch hoàn thành

## Viết code

## Voltage divider
esp8266 có chân ADC dùng để đọc các giá trị analog, nên có thể dùng đo điện áp đầu vào. Tuy nhiên, ADC chỉ nhận điện áp tối đa 3,3V, do đó cần phải hạ điện áp đầu vào để không làm hỏng MCU, bằng một mạch đơn giản gọi là Voltage divider.

![Impedance_voltage_divider](https://user-images.githubusercontent.com/56484469/131849726-19682202-782d-483e-80d8-ba4b74d56a25.png)

``` Vout = Vin*R2/(R1+R2) ```

* Vin: điện áp đầu vào (điện áp cần đo)
* R1: điện trở (Ω)
* R2: điện trở (Ω)
* Vout: điện áp đầu ra (vào chân ADC)

Pin Lifepo4 4S có điện áp sạc đầy là 14,6V, điện áp xả cạn là 10,6V => điện áp đầu Vin vào cần theo dõi.

R1 là điện trở 100KΩ, R2 là điện trở 25,3KΩ (nối tiếp điện trở 22KΩ và 3,3KΩ).

Theo công thức tính được điện áp đầu ra Vout = 2,948V

Như vậy hệ số điều chỉnh: Δ1 = 14,6/2,948 = 4,95

*Tuy nhiên, Δ1 là hệ số trên lý thuyết, trong thực tế sẽ xê dịch một chút do chất lượng linh kiện, sai số thiết bị đo,... nên cần tinh chỉnh lại khi lắp ráp.*

D1 mini có sẵn voltage divider cho ADC, khi cấp điện áp 0-3,3V sẽ được điều chỉnh xuống còn 0-1V => hệ số điều chỉnh Δ2 = 3,3

Cuối cùng, hệ số điều chỉnh: Δ= Δ1 * Δ2 = 4,95 * 3,3 = 16,35

Vout = Vin/16,35 => 14,6/16,35 = 0,89 => đây chính là điện áp cuối cùng mà MCU đo được qua chân ADC

## Nguồn tham khảo
1. [Esphome Speed Fan](https://esphome.io/components/fan/speed.html)
2. [Esphome Rotary Encoder](https://esphome.io/components/sensor/rotary_encoder.html)
3. [ESP8266 battery level meter](https://ezcontents.org/esp8266-battery-level-meter)
4. [Voltage divider: calculator and application](https://www.mischianti.org/2019/06/15/voltage-divider-calculator-and-application/)
