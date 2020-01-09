# 接口文档说明

## 1. 接口说明
### 1.1 系统配置
#### 1.1.1 设置系统参数

__URL:__

url|method|
:---:|:---:
http://ip:port/seetadevice/v1/platform/system/set| POST 

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
auto_add|int64|否|自动添加设备开关，1：关闭，2：开启，开启自动添加之后设备服务发现之后平台自动添加设备为已知设备，业务系统使用list获取设备
device_status_callback|string|否|设备状态回调地址，用于设备改变状态或出现异常时进行回调,接口见2.12
register_image_callback|string|否|照片注册错误回调接口，当安卓设备照片注册错误进行回调，接口见4.7
log_callback|object|否|设备错误回调地址，用于设备运行出错之后进行回调，<br>如：{"url":"http://jj.com/jj","level":1}<br>接口见2.14
seetacloud_url|string|否|SeetaCloud地址，http://&lt;api_key&gt;:&lt;secret_key&gt;@ip:port/params<br>例如：http://127.0.0.1:9180/seetacloud/cpp/detect<br>https://fsdfs:dfisi@cloud.seetatech.com/api/face/detect<br>当存在api_key和secret_key时则使用开放云进行人脸检测，否侧使用本地服务<br>不填写则平台不进行人脸检测，直接添加图片，填写则通过地址进行人脸检测，尝试特征下发
min_face|int64|否|人脸检测最小人脸宽度
min_clarity|float64|否|最小清晰度,0&lt;值&lt;=1
max_angle|float64|否|最大识别人脸角度,值&gt;0
handshake_key|string|否|组播请求的key
handshake_response|string|否|组播应答key
status_callback_cycle|int64|否|定时上报时长(s)

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.1.2 获取系统参数

__URL:__

url|method|
:---:|:---:
http://ip:port/seetadevice/v1/platform/system/get| GET 

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
param|系统参数|object|平台系统参数，object见下表

__object:__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
auto_add|自动添加设备|int64|1：关闭，2：开启
device_status_callback|设备状态回调地址|string|用于设备改变状态时进行回调
register_image_callback|string|否|照片注册错误回调接口
log_callback|object|否|设备错误回调地址，如：{"url":"http://ll.com/ls","level":1}
seetacloud_url|seeta cloud 地址|string|当前使用的seeta cloud 地址
min_face|最小人脸宽度|int64|人脸检测最小人脸宽度
min_clarity|清晰度阈值|float64|0&lt;值&lt;=1
max_angle|最大识别人脸角度|float64|人脸倾斜角度，值&gt;0
handshake_key|组播请求key|string|组播请求的key
handshake_response|组播应答key|string|组播应答key
status_callback_cycle|定时上报时长|int64|定时上报时长(s)

#### 1.1.3 系统重置
恢复出厂设置（安卓设备和平台删除设备信息、人员信息、任务执行信息）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/system/reset| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
reset_types|[]int64|是 |重置类型，<br>1：设备重置参数（设备参数，流参数和样式，清空设备上报日志），<br>2：清空人员数据（人员信息，人员同步信息，人员照片），<br>需设置到数组中传递，如[1,2]

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

### 1.2、组别

组别管理多个设备。
平台默认group_id为"default"

#### 1.2.1 创建设备组
创建新的设备组别,用于设备分组

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/group/create| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
group_id|string|是|设备组别名称，不可重复

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.2.2 删除设备组
删除创建的设备组别（备注：删除时组别必须为空组,"default"分组不可删除）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/group/delete| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
group_id|string|是|设备组别名称

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.2.3 设置设备默认参数
为组别设置设备的默认参数，对应组添加设备时使用对应设置的默认参数进行赋值，若修改默认参数不对之前设置的设备重新设置，只对修改之后添加的设备套用默认参数

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/group/set_default| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
group_ids|[]string|否|设备组别，不填则为"default"组
device_params|interface{}|否|设备参数，见安卓参数表单

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.2.4 获取组默认参数
获取所有组的默认参数

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/group/get_default| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
group_ids|[]string|否|设备组别，不填则为所有分组
skip|int|否|开始个数
limit|int|否|返回个数

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
defaults|默认参数|[]object|设备正在使用的默认参数，object如下表
total|总数|int|数据库总数

__object:__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
group_id|设备组别|string|组名
device_params|interface{}|否|设备参数，见安卓参数表单

### 1.3、设备

> 流参数概念：

* 流：原为IPC摄像头的RTSP流，这里包含IPC流、设备（门禁机、人证一体机）虚拟流
* 安卓app会调用门禁机、人证一体机的摄像头的画面，系统把这些摄像头画面虚拟为特殊的流，默认流id为default

> 使用设备的步骤：

* 调用”**设备发现**“接口发现设备
* 把设备发现中的设备 注册到”**设备添加**“接口
* 然后再调用”**设备设置**“接口配置设备属性
* 再调用先后调用”流参数增加“、”流参数修改“为设备设置流参数
* 调用”**设备数据重新下发**“，重新下发数据到设备中
* 删除设备调用”**删除接口**“

#### 1.3.1 设备发现
设备发现获取所有未知设备

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/discover| GET


__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
devices|所有连接但未添加设备|[]object|object信息见下表

__devices的object:__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
device_code|设备编码|string|设备对应的唯一编码，不可修改
group_id|组别|string|设备对应组别
type|设备类型|int64|1：人证一体机，2：门禁机，3：智能网关
ip|设备IP地址|string|
apk_version|apk版本|string|安卓apk版本号
device_version|设备版本号|string|设备版本号

#### 1.3.2 设备添加
添加设备，使用对应组别默认参数赋值，只能添加后台未知设备，即使用设备发现查询的设备，当设备同步完所有参数（4s超时），则添加完成

__URL:__

url|method|
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/add| POST 

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|是|设备对应的唯一编码
group_id|string|否|组别，默认使用"default"分组

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_results|设备执行结果|[]object|如：[{"device_code":"kkk","result":true,"info":"&lt;结果说明&gt;"}]，result表示设备运行任务是否成功


#### 1.3.3 设备设置
设置设备参数，只能对已知设备进行设置

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/set| POST 

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|是|设备对应的唯一编码
group_id|string|否|设备组别
device_params|object|否|设备参数，参考[5.1 设备参数](### 5.1设备参数) 


__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.3.4 设备删除
删除设备，从已知设备回到未知设备，设备清空数据

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/delete| POST 

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|是|设备对应的唯一编码，不可修改

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_results|设备执行结果|[]object|如：[{"device_code":"kkk","result":true,"info":"&lt;结果说明&gt;"}]，result表示设备运行任务是否成功

#### 1.3.5 设备数据重新下发
删除设备的人员数据，然后设备重新向平台拉取人员数据

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/reconstruct| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|是|设备对应的唯一编码，不可修改

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.3.6 获取设备信息
获取已知设备所有信息

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/list| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|否|设备编码，不填则查询所有设备，设备按组别排序，否则查询该设备
status|int64|否|设备在线状态，1：在线，2：离线，默认所有
ip|string|否|设备ip，模糊查询
skip|int|否|开始位置
limit|int|否|返回数量

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
total|总数|int|数据库总数
devices|已知设备|[]object|object信息见下表

__devices的object:__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
device_code|设备编码|string|设备对应的唯一编码，不可修改
type|设备类型|string|1：人证一体机，2：门禁机，3：智能网关,4：pc智能网关
ip|设备IP地址|string|
apk_version|apk版本|string|apk版本号
device_version|设备版本|string|设备版本号
device_params|object|否|设备参数，见[4.1 设备参数](###4.1 设备参数) 
camera_params|[]object|否|所有流参数，见[4.2 流参数](###4.2 流参数)
camera_status|bool|是|摄像头状态，true:正常 false:异常
display_status|bool|是|应用显示状态，true:正常 false:异常
alive|设备状态|int64|1：在线，2：断线

#### 1.3.7 设备应答
设备进行应答（亮闪光灯和发出声音），平台同步等待设备执行结果（4s超时）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/test| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备对应的唯一编码
camera_id|string|否|不填则为使用自身的webcam，当type包含3时有效
sound|string|否|填写则播报该信息，否则播报"test"，当type包含2时有效
display|string|否|填写则显示该信息，否则显示"test",当type包含4时有效
types|[]int64|是|测试类型，1：闪光灯 2：声音 3：摄像头，4：显示，当为照片时返回照片base64，如：[1,2,3]

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_result|设备执行结果|bool|true：成功，false：失败
capture_image|设备抓拍照片|string|抓拍到的照片base64编码，当测试为摄像头时存在

#### 1.3.8 设备开门
设备远程开门，平台同步等待设备执行结果（4s超时）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/open| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备对应的唯一编码
camera_id|string|是|开门对应的摄像头id

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_result|设备执行结果|bool|true：成功，false：失败

#### 1.3.9 人员通行
设备远程开门并显示人员信息（显示人员头像，通行信息和播放语音），平台同步等待设备执行结果（4s超时）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/pass| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备对应的唯一编码
person_id|string|是|人员的唯一id
camera_id|string|是|开门对应的摄像头id

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_result|设备执行结果|bool|true：成功，false：失败

#### 1.3.10 设置样式
设置设备样式，全量设置（修改、删除、添加需传递所有样式）


__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/set_style| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|否|设备编码，默认为所有设备，否则为该设备
styles|[]object|否|设备样式数组，object结构见下表

__styles的object:__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
type|int64|是|样式类型，1：屏保图，2：皮肤，3：logo图，4：跑马灯，5：头像框
info|string|否|type为1，2，3，5时，是图片url，当type为4时，如："欢迎到来"
info_base64|string|否|type为1，2，3，5时，是图片无换行base64编码
info_path|string|否|type为1，2，3，5时，是图片本地全局路径

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
failed_styles|设置失败的样式|[]object|object为上传的样式的object，见上表styles的object，该字段当且仅当res==0时存在，res!=0为样式全设置失败

#### 1.3.11 重置样式
重置样式，清空设备的样式，设备使用默认样式

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/reset_style| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|否| 设备编码，默认为所有设备，否则为该设备
style_types|[]string|否| 如果不存在，则重置所有样式，存在则重置指定类型样式，如：[1,2,3]

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.3.12 设备状态变更回调
后台提供回调接口，当设备状态改变或出现异常则进行上报

__URL:__

url|method
:---:|:---:
系统参数中设置| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备对应的唯一编码，不可修改
camera_status|bool|否|摄像头状态，true:正常 false:异常
display_status|bool|否|应用显示状态，true:正常 false:异常
alive|bool|否|设备在线状态，true:在线 false:离线
timestamp|int64|是|状态改变时的秒级时间戳

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.3.13 设备app升级
设备升级apk

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/update| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
group_ids|[]string|否|设备组别
device_codes|[]string|否|设备，都填则进行并集下发，如果都不填则使用"default"分组下的所有设备
apk_url|string|否|apk的url，用于下载apk
apk_path|string|否|apk的本地完整路径，用于下载apk
etag|string|是|apk包的md5值


__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.3.14 设备错误回调
设备出现错误之后进行回调

__URL:__

url|method
:---:|:---:
设备错误回调地址| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备编码
timestamp|int64|是|设备出现错误时间戳，时间戳为秒级
level|int64|是|日志错误等级
log|string|是|错误信息

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.3.15 流参数增加
设备增加流参数，同步等待设备响应（4s超时）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/camera/add| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备编码
camera_params|[]object|是|数组类型的设备流参数，见设备 [4.2流参数](#4.2 流参数) ,"id"参数必须存在，用于标识流，如果"id"参数值不填，则为默认id

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_result|设备执行结果|bool|true：成功，false：失败

#### 1.3.16 流参数修改
设备修改流参数，同步等待设备响应（4s超时）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/camera/edit| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备编码
camera_params|[]object|是|设备流参数，见 [4.1流参数](#4.1 流参数) ,"id"参数必须存在，用于标识流，如果"id"参数值不填，则为默认id

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_result|设备执行结果|bool|true：成功，false：失败

#### 1.3.17 流参数删除
设备删除流参数，同步等待设备响应（4s超时）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/camera/delete| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备编码
camera_ids|[]string|是|流参数中的"id"参数，默认

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_result|设备执行结果|bool|true：成功，false：失败

### 3.18 设备关门
设备远程关门，平台同步等待设备执行结果（16s超时）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/relay_close| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备对应的唯一编码
camera_id|string|是|开门对应的摄像头id

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_result|设备执行结果|bool|true：成功，false：失败

### 3.19 设备常开
设备常开，平台同步等待设备执行结果（16s超时）

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/device/relay_open| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备对应的唯一编码
camera_id|string|是|开门对应的摄像头id

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
device_result|设备执行结果|bool|true：成功，false：失败

### 1.4、人员信息

#### 1.4.1 添加人员
添加人员

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/person/add| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
person_id|string|是|人员对应的唯一id
group_ids|[]string|否|设备组
device_codes|[]string|否|设备编码，当设备组与设备列表都存在时，进行并集下发，如果都不填则使用"default"分组下的所有设备
ic_card|string|否|ic卡号，用于刷卡通行
id_card|string|否|身份证号码，用于1:1比对
wechat_user_id|string|否|微信id
date_begin|int64|否|鉴权开始秒级时间戳
date_end|int64|否|鉴权结束秒级时间戳
box_image|string|否|自定义头像框url
box_image_base64|string|否|自定义头像框base64无换行编码
box_image_path|string|否|自定义头像框本地完整路径
portrait_image|string|否|识别展示照片url
portrait_image_base64|string|否|识别展示照片base64无换行编码
portrait_image_path|string|否|识别展示照片本地完整路径
subtitle_pattern|[]string|否|人员展示信息，每个字符串一行，字符串如："你好，&lt;name&gt;"，每个尖括号中的值为属性名称
voice_pattern|string|否|人员特定语音播报信息，"你好，&lt;name&gt;"，每个尖括号中的值为属性名称
attributes|object|否|人员属性，属性内容由业务系统定义，如{"姓名":"张三"}

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.4.2 添加人员照片
添加人员照片，系统参数设置seeta cloud url时进行人脸检测，否则不进行人脸检测

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/person/add_image| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
person_id|string|是|人员对应的唯一id
image_url|string|否|人员照片url，如：`http://tt.com/kk.jpg`
image_base64|string|否|人员照片base64无换行编码
image_path|string|否|人员照片本地完整路径
feature|[]float64|否|特征值，如果带有该值，则不请求seetacloud
feature_version|string|否|特征值的特征版本号

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
image_id|人员照片id|string|平台生成的照片唯一id
rect|人脸位置|object|上传图片的人脸位置，object信息见下表

__object:__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
x|横坐标|int64|人脸横坐标
y|纵坐标|int64|人脸纵坐标
height|高度|int64|人脸图片高度
width|宽度|int64|人脸图片宽度

#### 1.4.3 删除
删除该人员

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/person/delete| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
person_id|string|是|人员对应的唯一id

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.4.4 删除人员照片
删除人员的一张照片

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/person/delete_image| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
person_id|string|是|人员对应的唯一id
image_id|string|是|照片的唯一id

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.4.5 编辑人员
编辑人员下发的设备组别

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/person/edit| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
person_id|string|是|人员对应的唯一id
group_ids|[]string|否|设备组别
device_codes|[]string|否|设备编码，当设备组与设备列表都存在时，进行并集下发
ic_card|string|否|ic卡号
id_card|string|否|身份证号码
wechat_user_id|string|否|微信id
date_begin|int64|否|鉴权开始秒级时间戳
date_end|int64|否|鉴权结束秒级时间戳
box_image|string|否|自定义头像框url
box_image_base64|string|否|自定义头像框base64无换行编码
box_image_path|string|否|自定义头像框本地完整路径
portrait_image|string|否|识别展示照片url
portrait_image_base64|string|否|识别展示照片base64无换行编码
portrait_image_path|string|否|识别展示照片本地完整路径
subtitle_pattern|[]string|否|界面提示，每个字符串一行，字符串如："你好，&lt;name&gt;"，每个尖括号中的值为属性名称
voice_pattern|string|否|语音展示，"你好，&lt;name&gt;"，每个尖括号中的值为属性名称
attributes|object|否|人员属性，属性内容由业务系统定义，如{"姓名":"张三"}

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

#### 1.4.6 获取人员
获取人员信息

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/person/list| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
person_ids|[]string|否|后台对应人员id，不填写则获取所有人员，否则为对应人员信息
ic_card|string|否|人员IC卡，模糊查询
id_card|string|否|人员ID卡，模糊查询
wechat_user_id|string|否|人员微信id，模糊查询
skip|int|否|开始个数
limit|int|否|返回个数

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
persons|人员信息|[]object|object见下表

__persons的object：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
person_id|后台对应人员id|string|人员对应的唯一id
ic_card|ic卡号|string|ic卡号
id_card|身份证号|string|身份证号码
wechat_user_id|微信id|string|微信id
date_begin|鉴权开始时间|int64|秒级时间戳
date_end|鉴权结束时间|int64|秒级时间戳
box_image|头像框|string|自定义头像框url
portrait_image|头像照片|string|识别展示照片url
subtitle_pattern|界面提示|[]string|每个属性一行， 如：["姓名"]
voice_pattern|语音展示|string|字符串格式为如"你好，&lt;name&gt;"，每个尖括号中的值为属性名称
attributes|人员属性|object|如{"姓名":"张三"}
images|人员照片|[]object|如：[{"image_id":"fsfs","image_feature":[11.11,12.12],"model_version":"dfsfs"}]

#### 1.4.7 照片注册错误上报
上报照片错误信息

__URL:__

url|method
:---:|:---:
照片注册错误回调地址| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_code|string|是|设备编码
image_id|string|是|图片id
timestamp|int64|是|秒级错误时时间戳
data|string|是|错误说明

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误

### 1.5.日志系统

#### 1.5.1 设备日志查询

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/log/list| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|否|设备编码数组
level|int64|否|日志等级，如："DEBUG":1,"INFO":2,"WARN":3,"ERROR":4 返回大于等于等级的日志
begin_date|int64|否|秒级开始时间戳
end_date|int64|否|秒级结束时间戳
start_index|int|否|开始条数，默认为0
limit|int|否|返回条数，默认为10

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
total|总共条数|int|查询结果条数
logs|日志|[]object|object见下表

__object:__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
device_code|设备编码|string|设备对应的唯一编码
timestamp|日志时间戳|int64|秒级时间戳
level|日志标签|string|如："DEBUG","INFO","WARN","ERROR"
data|日志说明|string|出现日志情况描述

#### 1.5.2 请求日志查询

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/request_log/list| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
ip|string|否|请求ip
router|string|否|请求uri
begin_date|int64|否|秒级开始时间戳
end_date|int64|否|秒级结束时间戳
start_index|int|否|开始条数，默认为0
limit|int|否|返回条数，默认为10

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
total|总共条数|int|查询结果条数
logs|日志|[]object|object见下表

__object:__

参数|类型|说明
:---:|:---:|:---:
ip|string|请求的ip
router|string|请求uri
timestamp|int64|日志秒级时间戳
data|string|出现日志情况描述

### 1.6.照片同步
#### 1.6.1 照片同步查询

__URL:__

url|method
:---:|:---:
http://ip:port/seetadevice/v1/platform/image/list| POST

__参数：__

参数|类型|必填|描述
:---:|:---:|:---:|:---:
device_codes|[]string|否|设备编码数组
image_ids|[]string|否|照片对应的id
begin_date|int64|否|秒级开始时间戳
end_date|int64|否|秒级结束时间戳
start_index|int|否|开始条数，默认为0
limit|int|否|返回条数，默认为10

__返回结果：__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
res|返回码|int|成功为0，其余错误
total|总共条数|int|查询结果条数
sync_images|注册照片信息|[]object|object见下表

__object:__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
device_code|设备编码|string|设备对应的唯一编码
image_id|图片id|string|
sync_result|图片获取结果|int64|1：成功 2：失败 3：未下发 4：下发但是未返回结果
timestamp|日志时间戳|int64|秒级时间戳
data|日志说明|string|出现日志情况描述

## 2 终端识别上报数据和鉴权数据格式说明

### 2.1. 通行记录上报接口

#### 请求参数
|参数 | 类型 |必填|描述|
|----|----|----|----|
|device_code|string|是|设备编码|
|capture_image|base64|是|人脸现场抓拍照片（base64)|
|person_id| string |是|识别人员id|
|id_card| string |否|身份证|
|ic_card| string |否|ic卡号|
|qr_code| string |否|二维码|
|card_info| object|否|身份证信息|
|matches| array of object|否|人脸识别信息|
|recognize_type|int64|是|识别类型，1:1-n 2:1-1 3:1-x|
|recognize_type_backup|int64|否|备选识别类型,0:无 2:1-1 3:1-x|
|feature_comparison|int64|否|底库比对，1-x时使用，1:开启 2:关闭|
|is_exist|int64|否|存在性验证，1-1时使用，1:开启 2:关闭|
|recognize_info|object|是|识别参数，见下表|
|timestamp|int64|是|毫秒级时间戳|
|camera_id|string|是|流id|
|score|float|是|对比最高分数|
|is_pass|int|是|是否通过，1通过；2不通过|

**字段 recognize_info 说明**

|参数 | 类型 |必填|描述|
|----|----|----|----|
|clarity|float|否|清晰度|
|pose|float[]|否|姿态|
|faceRect|object|否|人脸框|

**字段 card_info 说明**

|参数 | 类型 |必填|描述|
|----|----|----|----|
|name|string|否|姓名|
|gender|string|否|性别|
|address|string|否|地址|
|folk|string|否|民族|
|card_image|string(b64)|否|身份证照片|

**字段 matches 说明：**

|参数 | 类型 |必填|描述|
|----|----|----|----|
|person_id|string|是|人员ID，识别类型为1时,人员注册ID；识别类型为2时，身份证ID号|
|image_id|string|是|相似照片ID，当识别类型为1时，照片ID;识别类型为2时，为空；|
|score|float|是|比对得分|

#### 返回参数（json形式）
|字段 | 类型 |解释|eg|
|----|----|----|----|
|res|int|状态码||是|

### 2.3.二次鉴权上报接口

#### 请求参数

|参数 | 类型 |必填|描述|
|----|----|----|----|
|device_code|string|是|设备编码|
|person_id|string|是|人员id|
|id_card| string |是|身份证|
|ic_card| string |是|ic卡号|
|qr_code| string |是|二维码|
|card_info| object|是|身份证信息|
|matches| array of object|是|人脸识别信息|
|recognize_type|int64|是|识别类型，1:1-n 2:1-1 3:1-x|
|recognize_type_backup|int64|否|备选识别类型,0:无 2:1-1 3:1-x|
|feature_comparison|int64|否|底库比对，1-x时使用，1:开启 2:关闭|
|is_exist|int64|否|存在性验证，1-1时使用，1:开启 2:关闭|
|recognize_info|object|是|识别参数，见下表|
|timestamp|int64|是|毫秒级时间戳|
|camera_id|string|是|流id|
|score|float|是|对比最高分数|

#### 返回参数（json形式）
|字段 | 类型 |解释|eg|
|----|----|----|----|
|res|int|状态码|0，为成功，其他为失败|

## 3、设备参数说明

### 3.1 设备参数
 
参数|名称|类型|说明
:---:|:---:|:---:|:---:
voice_switch|声音开关|int64|1：开启，2：关闭
volume|音量|int64|0<=值<=100,
password|开门密码|string|设备开门密码
relay_signal_alignment|继电器信号是否对调|int|1:是，2：否
relay_hold_time|继电器保持时长(s)|int64|
log_level|上报日志等级|int|日志包含（1-4，分别对应debug,info,warn,error）等级，大于该设置等级的才上报
is_light|是否开启闪光灯|int64|1：开启，2：关闭
recognize_type|识别类型|int64|1表示1:n，2表示1:1，3表示1:x；
feature_comparison|底库比对|int64|1-x时使用，1:开启 2:关闭
is_exist|存在性验证|int64|1-1时使用，1:开启 2:关闭
external_devices|外接设备|[]int64|1:二维码，2：身份证，3：IC卡

### 3.2 流参数
参数|名称|类型|说明
:---:|:---:|:---:|:---:
id|摄像头id|string|
type|摄像头类型|int64|
url|摄像头参数|string|json字符串，内容必须与camera_type匹配
threshold_11|1:1验证阈值|float64|0<=值<=1
confidence|1:n验证阈值|float64|0<=值<=1
unsure|1:n不可信阈值|float64|小于等于confidence
min_clarity|最小清晰度|float64|0<=值<=1,默认0.35
recognition_mode|识别模式|int64|识别模式(1:最大人脸识别,2:多人脸识别),默认最大人脸
min_face|最小人脸宽度|int64|抓拍时人脸宽度
max_angle|最大人脸角度|float64|人脸最大角度(建议取值范围10-50)
crop_ratio|截取比例|float64|人脸截取比例
detect_box|人脸检测框|int64|是否开启人脸检测框，1：开启，2：关闭，默认2：关闭
time_slots|可用时间段|[]time_slot|time_slot见下表
subtitle_pattern|显示信息|[]String|展示信息,人员展示信息，每个字符串一行，字符串如："你好，&lt;name&gt;"，每个尖括号中的值为属性名称
sound_pattern|语音播报|String|语音播报信息，人员特定语音播报信息，"你好，&lt;name&gt;"，每个尖括号中的值为属性名称
report_11_url|通行记录1:1上报接口|string|上报1:1通行记录
report_1n_url|通行记录1:n上报接口|string|上报1:n通行记录
auth_url|二次鉴权接口|string|进行二次鉴权接口
capture_max_interval|最大尝试人脸抓拍时长(ms)|int64|值大于等于1
is_living_detect|是否开启活体检测|int|1：开启，2：关闭
control_signal_out|控制信号输出|int64|1:不输出，2:本机高电平，3：本机低电平，4：本机485信号输出
top_n|返回的照片数目|int64|1:n上报时返回的图片数量
not_pass_report|是否上报未识别人员|int|1:是，2：否
is_working|是否启用(默认为启用)|int|1:是，2：否

__参数“time_slot”说明:__

参数|名称|类型|说明
:---:|:---:|:---:|:---:
date|日期|string|如:"2006-01-02"
slots|时间段|[]object|object为{"begin":"15:04:05","end":"23.59:59"}

### 3.3 人证一体机参数设置说明

#### 3.3.1 设备参数：
参数|名称|类型|说明
:---:|:---:|:---:|:---:
voice_switch|声音开关|int64|1：开启，2：关闭，默认开启
volume|音量|int64|0<=值<=100，默认60
log_level|上报日志等级|int|日志包含（1-4，分别对应debug,info,warn,error）等级，大于该设置等级的才上报；默认1.debug
is_light|是否开启闪光灯|int64|1：开启，2：关闭，默认开启
recognize_type|识别类型|int64|1表示1:n，2表示1:1,3表示1:x；人证一体机应选择2
feature_comparison|底库比对|int64|1-x时使用，1:开启 2:关闭
is_exist|存在性验证|int64|1-1时使用，1:开启 2:关闭
external_devices|外接设备|[]int64|1:二维码，2：身份证，3：IC卡

#### 3.3.2 流参数：
参数|名称|类型|说明
:---:|:---:|:---:|:---:
threshold_11|1:1验证阈值|float64|0<=值<=1，默认0.7
min_clarity|最小清晰度|float64|0<=值<=1，默认0.35
recognition_mode|识别模式|int64|识别模式(1:最大人脸识别,2:多人脸识别)，默认最大人脸
min_face|最小人脸宽度|int64|抓拍时人脸宽度，默认80
max_angle|最大人脸角度|float64|人脸最大角度(建议取值范围10-50)，默认20
crop_ratio|截取比例|float64|人脸截取比例，默认0.5
detect_box|人脸检测框|int64|是否开启人脸检测框，1：开启，2：关闭，默认2：关闭
subtitle_pattern|显示信息|[]String|展示信息,人员展示信息，每个字符串一行，字符串如："你好，&lt;name&gt;"，每个尖括号中的值为属性名称
sound_pattern|语音播报|String|语音播报信息，人员特定语音播报信息，"你好，&lt;name&gt;"，每个尖括号中的值为属性名称
report_11_url|通行记录1:1上报接口|string|上报1:1通行记录，填写url会自动上报数据，数据格式参考[1：1上传接口格式](### 4.2 人证搜索识别数据上报格式（1:1）)
auth_url|二次鉴权接口|string|进行二次鉴权接口
capture_max_interval|最大尝试人脸抓拍时长(ms)|int64|值大于等于1
top_n|返回的照片数目|int64|1:n上报时返回的图片数量；默认1，即返回一张照片
not_pass_report|是否上报未识别人员|int|1:是，2：否，默认不上报
is_working|是否启用(默认为启用)|int|1:是，2：否，默认启用


## 4.错误码说明

 设备管理平台错误码

res|msg
:---:|:---:
0|成功
1002|参数错误
1003|鉴权失败
1006|数据库操作异常
1007|mqtt连接异常
1009|mqtt发送失败
1010|json解析失败
1011|数据解析失败
1012|app_id不同错误
1013|apk不存在
1014|apk已存在
1015|topic不存在
2201|未找到匹配设备
2202|设备已存在
2203|设备类型不同
2204|设备不在线
2206|设备图片已存在
2207|设备图片不存在
2208|设备摄像头参数超出限制
2209|设备皮肤数量错误
2210|设备组已存在
2211|设备组不存在
2212|组别包含设备
2213|默认组别不可删除
2214|默认摄像头已存在
2215|该摄像头不存在
2216|该摄像头已存在
2301|未找到匹配的人员
2302|人员已存在
2303|人脸检测错误
2304|人员照片存在
2305|人员照片不存在
2306|人员有效期错误
2401|文件下载失败
2402|文件打开错误
2403|文件写入错误
2404|文件保存错误
2405|文件未找到
2406|文件MD5值不同
2901|任务不存在
2902|任务已完成
2903|安卓执行任务错误
2904|执行任务出错
3001|SeetaCloud服务错误
3002|c++ 解析错误
3003|人脸数目错误
3004|无人脸错误
3005|多人脸错误
3006|清晰度低
3007|人脸宽度过小
3008|人脸角度过大
3009|人脸检测错误
3010|SeetaCloud地址未填写
3011|特征值不存在

## 5. 备注
<font color=red>系统暂不支持语音合成相关功能</font>
