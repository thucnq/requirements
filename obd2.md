# OBD2 Tracker

Instruction: https://docs.espressif.com/projects/esp-idf/en/latest/

# API

### Các thông tin lưu trữ tại Device khi sản xuất:
- DeviceID/HardwareID: OBD2-ABCDEFABC 
- Model: OBD2-1
- FirmwareVersion: 1.0.0-alpha.1 (semver)
- ManufactureToken/reg_token: Token cho mỗi đợt sản xuất, Server sẽ gán cho 1 số lượng Device có thể reg bằng token này, sau khi reg 1 device thì giảm 1, tới khi Reg hết thì invalidate token
- vin: VIN của xe
- device_token: Token dùng để cập nhập firmware (hoặc auth cho MQTT)
- sim_serial

### Đăng ký thiết bị với Server
- API endpoint của Server là: `API_ENDPOINT`, ví dụ `API_ENDPOINT = https://iotapi.agiletech.vn/v1/api`
```
export API_ENDPOINT=https://iotapi.agiletech.vn/v1/api
curl $API_ENDPOINT/regisgter\
    -X POST\
    -H "Content-type: application/json"\
    -d '{"vin":"VIN_XE","hardware_id":"OBD2-ABCDEFGH","sim_serial":"12349993888","reg_token":"GlaHquq0","model":"OBD2-1"}'
```
 + Nếu hợp lệ, Server trả về status 200 và dữ liệu là `{ "device_token": "vqGmBX" }`
 + Nếu không hợp lệ trả về status khác 200 + message lỗi

### Check OTA 
- API endpoint của OTA server là `$API_ENDPOINT/ota/update`, 
- Kiểm tra OTA:

```
curl $API_ENDPOINT/ota/update?\
    fw_version=1.1.0\
    &hw_version=OBD_V4\
    &hardware_id=OBD2-ABCDEFGH\
    &model=OBD2-1\
    &device_token=vqGmBX
```

 + Nếu thiết bị cần phải cập nhật firmware, server trả về file firmware dạng binary
 + Nếu không cần phải cập nhật firmware, server trả về status 304

### MQTT Broker
- URL: mqtts://user:pass@host:port/client_id
    + Host: Địa chỉ kết nối đến broker
    + Port: Port kết nối
    + user: HardwareID
    + pass: device_token
    + client_id: HardwareID
- Public topic: `/devices/hardware_id/pub`
- Subscribe topic: `/devices/hardware_id/sub`
- Data report over public topic:
    + `{"type":"report", "hardware_id":"hardware_id", "sim_serial": "sim_serial", "vin": "VIN", "obd2": "obd2_data", "location": "lat:long", "time": "utc_timestamp"}`
- Data receive from subscribe topic:
    + `{"type": "cmd": "cmd": "force_ota"}`
