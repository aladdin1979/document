# BestWay Project

## 更新日誌

| 版本           | 日期       | 更新內容 |
| -------------- | ---------- | -------- |
| V1.1.2024.10.9 | 2024-10-09 | 初版     |

## API URL 說明

基礎網址 http://localhost:8100/api/ <br/>

共用相關 http://localhost:8100/api/universal/ <br/>
管理端 http://localhost:8100/api/admin/ <br/>
總代理端 http://localhost:8100/api/mainAgent/ <br/>
代理端 http://localhost:8100/api/agent/ <br/>
會員端 http://localhost:8100/api/member/ <br/>

### 基本參數

| ParameterName | DataType             | Description                |
|---------------|----------------------|----------------------------|
| s             | string (Required)    | 簽章                         |
| d             | int array (Required) | 加密後內容                      |
| t             | int (Required)       | 時間搓計(10 碼)                 |
| tk            | string (Required)    | token<br/>除了登入以外，其他都必須帶入   |
| sp            | int                  | (正式環境無效) 是否回傳輸入的參數 1:是     |
| dm            | int                  | (正式環境無效) 是否顯示 debug 訊息 1:是 |
| dc            | int                  | (正式環境無效) d 是否不加密 1:是       |
| tc            | int                  | (正式環境無效) 忽略時間搓計檢查 1:是      |

### 回傳說明

```json
{
  "cmdNo": 100,
  "status": 1, //除了 1 以外都是錯誤
  "message": "" //status !=1 時，會帶入錯誤訊息
}
```

status 代碼表<br/> https://docs.google.com/spreadsheets/d/1ldp7npnMXjC20SYNDnKv7KaAF9dAVweQRwAMuVy5bDg/edit?gid=0#gid=0

## API cmdNo 範圍

| 分類     | cmdNo 開始 | cmdNo 結束 |
| -------- | ---------- | ---------- |
| 管理端   | 1          | 499        |
| 總代理端 | 500        | 699        |
| 代理端   | 700        | 799        |
| 會員端   | 800        | 999        |
| 其他     | 1000       | 1100       |

## 管理端

### 管理員-登入 cmdNo=100

| ParameterName | DataType          | Description |
|---------------|-------------------|-------------|
| account       | string (Required) | 帳號          |
| password      | string (Required) | 密碼          |
| lang          | string            | 語系，預設 en    |

```json
{
  "cmdNo": 100,
  "status": 1,
  "message": "",
  "data": {
    "token": "WzEzLDE3LDM0LDAsOSw2Myw1NywxLDM5LDIsMzMsMj" //Token
  }
}
```

### 註冊 cmdNo=1

| ParameterName | DataType            | Description |
|---------------|---------------------|-------------|
| account       | string (Required)   | 帳號          |
| password      | string (Required)   | 密碼          |
| rid           | integral (Required) | 角色代碼        |
| name          | string              | 名稱          |

```json
{
  "cmdNo": 1,
  "status": 1,
  "message": ""
}
```

### 管理員-列表 cmdNo=2

| ParameterName | DataType | Description  |
|---------------|----------|--------------|
| aid           | integral | 管理員代碼        |
| rid           | integral | 角色代碼         |
| account       | string   | 帳號           |
| pageNum       | integral | 頁數，0 開始      |
| pageLimit     | integral | 每頁筆數，預設 10 筆 |

```json
{
  "cmdNo": 2,
  "status": 1,
  "message": "",
  "data": {
    "total": 14,
    "totalPage": 2,
    "list": [
      {
        "aid": 2, //管理員代碼
        "rid": 1, //角色代碼
        "name": "23434", //名稱
        "status": 1, //狀態 0:停用 1:啟用 9:觀看
        "account": "admin", //管理員帳號
        "updated_at": "2024-10-14 07:22:54"
      }
    ]
  }
}
```

### 管理員-修改 cmdNo=3

| ParameterName | DataType           | Description       |
|---------------|--------------------|-------------------|
| aid           | integral(Required) | 管理員代碼             |
| rid           | integral           | 角色代碼              |
| password      | string             | 密碼                |
| status        | integral           | 狀態 0:停用 1:啟用 9:觀看 |
| name          | string             | 名稱                |

```json
{
  "cmdNo": 1,
  "status": 1,
  "message": ""
}
```

### 角色-列表 cmdNo=10

| ParameterName | DataType | Description         |
|---------------|----------|---------------------|
| type          | integral | 類別 1:管理員 2:總代理 3:代理 |
| pageNum       | integral | 頁數，0 開始             |
| pageLimit     | integral | 每頁筆數，預設 10 筆        |

```json
{
  "cmdNo": 10,
  "status": 1,
  "message": "",
  "data": {
    "total": 3,
    "totalPage": 1,
    "list": [
      {
        "rid": 3, //角色代碼
        "type": 3, //類別 1:管理員 2:總代理 3:代理
        "updated_at": "2024-11-20 11:38:11" //最後更新時間
      },
      {
        "rid": 4,
        "type": 3,
        "updated_at": "2024-11-20 11:37:29"
      }
    ]
  }
}
```

### 角色-新增 cmdNo=11

| ParameterName | DataType               | Description                                                             |
|---------------|------------------------|-------------------------------------------------------------------------|
| type          | integral (Required)    | 類別 <br/> 1:管理員 2:總代理 3:代理                                               |
| permissions   | stringArray (Required) | 權限陣列 <br/> ex:{"aaa":1,"bbb":3,"ccc":31} <br/>詳情看下方「permissions 參數說明」說明 |

```json
{
  "cmdNo": 11,
  "status": 1,
  "message": ""
}
```

### 角色-修改 cmdNo=12

| ParameterName | DataType            | Description                                                             |
|---------------|---------------------|-------------------------------------------------------------------------|
| rid           | integral (Required) | 角色代碼                                                                    |
| type          | integral            | 類別 <br/> 1:管理員 2:總代理 3:代理                                               |
| Permissions   | stringArray         | 權限陣列 <br/> ex:{"aaa":1,"bbb":3,"ccc":31} <br/>詳情看下方「permissions 參數說明」說明 |

```json
{
  "cmdNo": 12,
  "status": 1,
  "message": ""
}
```

### 角色-查詢權限內容 cmdNo=13

| ParameterName | DataType            | Description |
|---------------|---------------------|-------------|
| rid           | integral (Required) | 角色代碼        |

```json
{
  "cmdNo": 13,
  "status": 1,
  "message": "",
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "rid": 1, //角色代碼
        "name": "最高管理員", //角色名稱
        "permissions": {
          //權限陣列
          "aaa": 1,
          "bbb": 3,
          "ccc": 31
        },
        "created_at": "2024-10-14 08:19:12", //建立時間
        "updated_at": "2024-10-17 06:35:06" //更新時間
      }
    ]
  }
}
```

#### permissions 參數說明

key 值為自定義 （aaa、bbb、ccc）<br/>
value 值為二進制 (新增:1, 刪除:2, 更新:4, 查詢:8, 下載:16)<br/>
ex:
aaa: 1,bbb: 3,ccc: 31 <br/>
aaa 有新增權限 <br/>
bbb 有新增、刪除權限 <br/>
ccc 有全部的權限

### 管理員-遊戲列表 cmdNo=14

| ParameterName | DataType | Description |
|---------------|----------|-------------|
| gameProvider  | string   | 遊戲提供商       |

```json
{
  "cmdNo": 15,
  "status": 1,
  "message": "",
  "data": {
    "we": [
      //遊戲廠商代碼
      {
        "c": "BAC", //遊戲代碼
        "n": "經典百家樂" //遊戲名稱
      }
    ],
    "sa": [
      {
        "c": "bac",
        "n": "百家樂"
      }
    ]
  }
}
```

### 管理端-查詢投注拆帳明細 cmdNo=15

| ParameterName | DataType            | Description  |
|---------------|---------------------|--------------|
| said          | integral            | 上層代理代碼       |
| maid          | integral            | 代理代碼 0:管理端   |
| debtType      | integral (Required) | 拆帳類別 1:投注記錄  |
| debtSerial    | integral            | 對應的拆帳對象訂單流水號 |
| pageNum       | integral            | 頁數，0 開始      |
| pageLimit     | integral            | 每頁筆數，預設 10 筆 |

```json
{
  "cmdNo": 15,
  "status": 1,
  "message": "",
  "data": {
    "list": [
      {
        "debt_type": 1, //拆帳類別 1:投注記錄
        "debt_serial": 117, //對應的拆帳對象訂單流水號
        "maid": 0, //代理代碼
        "said": 0, //上層代理代碼
        "commission_pct": 100, //設定的佔成值
        "actual_commission_pct": 20, //實際整條線所佔的佔成%數 20=20%
        "debt_amount": 10000, //該代理拆分後的金額 10000:1
        "paid_amount": 42000, //上繳金額 10000:1
        "member_amount": 0, //直屬會員上繳金額 10000:1
        "total_amount": 50000, //整線總共金額 10000:1
        "commission_pct_detail": "[[0,100,20],[1,80,20],[30,60,60]]", //整線的佔成計算內容 [代理代碼,設定的佔成值,實際佔成%數]
        "created_at": "2024-11-19 14:13:44"
      }
    ]
  }
}
```

### 管理端-系統-查詢告警紀錄 cmdNo=16

| ParameterName | DataType | Description  |
|---------------|----------|--------------|
| stime         | string   | 開始時間(+8 時區)  |
| etime         | string   | 結束時間(+8 時區)  |
| pageNum       | integral | 頁數，0 開始      |
| pageLimit     | integral | 每頁筆數，預設 10 筆 |

```json
{
  "cmdNo": 16,
  "status": 1,
  "message": "",
  "data": {
    "total": 809,
    "totalPage": 81,
    "list": [
      {
        "type": "info", //分類 info:訊息 warm:警告 error:錯誤
        "text": "執行API: 會員端-FastPay金流入款查詢 [ 851 ] 秒數:5.169072請注意", //告警內容
        "created_at": "2024-12-09 10:30:05" //產生時間
      }
    ]
  }
}
```

### 管理端-會員-查詢報表 cmdNo=17

| ParameterName | DataType | Description                                |
|---------------|----------|--------------------------------------------|
| said          | integral | 上層代理代碼                                     |
| uuid          | integral | 會員代碼                                       |
| gameProvider  | string   | 遊戲提供商                                      |
| gameCategory  | string   | 遊戲分類代碼 1:真人 2:彩票 3:電子 4:體育 5:棋牌 10:區塊鏈     |
| gameType      | string   | 遊戲類別(代碼)                                   |
| reportType    | integral | 報表類別 1:日(預設) 2:週 3:月                       |
| reportSubType | integral | 報表子類別 1:一般報表(預設) 2:遊戲商報表 3:遊戲分類報表 4:遊戲類別報表 |
| stime         | string   | 開始時間(+8 時區)                                |
| etime         | string   | 結束時間(+8 時區)                                |
| pageNum       | integral | 頁數，0 開始                                    |
| pageLimit     | integral | 每頁筆數，預設 10 筆                               |

```json
{
  "cmdNo": 17,
  "status": 1,
  "message": "",
  "data": {
    "total": 20,
    "totalPage": 2,
    "list": [
      {
        "game_provider": "we", //遊戲廠商代碼( 遊戲提供商代碼有值時才會有 )
        "game_category": "真人[1]", //遊戲分類( 遊戲分類代碼有值時才會有 )
        "game_type": "獵龍高手[jdb7006]", //遊戲類別( 遊戲類別代碼有值時才會有 )
        "uuid": 50, //會員代碼
        "said": 30, //上層代理代碼
        "report_date": "2024-12-06", //報表日期
        "bet_amount": 1200000, //投注金額 10000:1
        "valid_bet_amount": 1200000, //有效投注额 10000:1
        "winloss_amount": -750000, //赢输金额 10000:1
        "deposit": 0, //入金點數 10000:1
        "withdraw": 0, //出金點數 10000:1
        "bet_total": 4, //總投注次數
        "deposit_total": 0, //入金訂單筆數
        "withdraw_total": 0, //出金訂單筆數
        "updated_at": "2024-12-06 11:57:09" //最後更新時間
      }
    ]
  }
}
```

### 管理端-會員-查詢餘額異動紀錄 cmdNo=18

| ParameterName | DataType | Description  |
|---------------|----------|--------------|
| aid           | integral | 管理員代碼(執行者)   |
| said          | integral | 上層代理代碼       |
| maid          | integral | 代理代碼(執行者)    |
| uuid          | integral | 會員代碼         |
| ip            | string   | IP           |
| wdCode        | integral | 金流類別代碼(下方定義) |
| stime         | string   | 開始時間(+8 時區)  |
| etime         | string   | 結束時間(+8 時區)  |
| pageNum       | integral | 頁數，0 開始      |
| pageLimit     | integral | 每頁筆數，預設 10 筆 |

```json
{
  "cmdNo": 18,
  "status": 1,
  "message": "",
  "data": {
    "total": 62,
    "totalPage": 7,
    "list": [
      {
        "macl_id": 62, //流水號
        "uuid": 58, //會員代碼
        "aid": 2, //管理端代碼
        "maid": 0, //代理代碼
        "said": 30, //上層代理代碼
        "transaction_id": "", //交易編號，不一定會有
        "currency": "VND", //幣別
        "wd_code": 1000, //金流類別代碼
        "wd_type": 1, //增減類別 0:減少 1:增加
        "c_amount": 100000, //此次異動金額 10000:1
        "b_amount": 0, //用戶異動前金額 10000:1
        "a_amount": 100000, //用戶異動後金額 10000:1
        "ip": "192.168.65.1",
        "created_at": "2024-11-14 17:21:23" //交易時間
      }
    ]
  }
}
```

### 管理端-全部-查詢操作日誌 cmdNo=20

| ParameterName | DataType | Description                           |
| ------------- | -------- | ------------------------------------- |
| cmd           | integral | 指令代碼                              |
| userType      | integral | 對象類別 1:管理員(預設) 2:代理 3:會員 |
| type          | integral | 類別 1:正常日誌(預設) 2:錯誤日誌      |
| aid           | integral | 管理員代碼，對象類別:1 使用           |
| maid          | integral | 代理代碼，對象類別:2 使用             |
| said          | integral | 上層代理代碼，對象類別:2,3 使用       |
| uuid          | integral | 會員代碼，對象類別:3 使用             |
| taid          | integral | 對象代碼                              |
| ip            | string   | IP                                    |
| stime         | string   | 開始時間(+8 時區)                     |
| etime         | string   | 結束時間(+8 時區)                     |
| pageNum       | integral | 頁數，0 開始                          |
| pageLimit     | integral | 每頁筆數，預設 10 筆                  |

```json
{
  "cmdNo": 20,
  "status": 1,
  "message": "",
  "data": {
    "total": 1628,
    "totalPage": 163,
    "list": [
      {
        "serial": 1,
        "aid": 2, //管理員代碼
        "a_name": "23434", //管理員名稱
        "a_account": "admin", //管理員帳號
        "cmd_no": 100, //指令代碼
        "taid": 0, //對象代碼
        "t_name": null, //對象名稱
        "t_account": null, //對象帳號
        "request": {
          //呼叫API時的參數
          "account": "admin",
          "password": "********",
          "lang": "zh_tw"
        },
        "ip": "192.168.65.1", //呼叫API時IP
        "updated_at": "2024-10-22 01:26:20", //呼叫API時間
        "cmdNoName": "管理端-管理員-登入" //API說明
      }
    ]
  }
}
```

### 查詢代理端操作日誌 cmdNo=21

| ParameterName | DataType | Description                      |
| ------------- | -------- | -------------------------------- |
| cmd           | integral | 指令代碼                         |
| type          | integral | 類別 0:正常日誌(預設) 1:錯誤日誌 |
| maid          | integral | 代理代碼                         |
| said          | integral | 上層代理代碼                     |
| taid          | integral | 對象代理代碼                     |
| ip            | string   | IP                               |
| stime         | string   | 開始時間(+8 時區)                |
| etime         | string   | 結束時間(+8 時區)                |
| pageNum       | integral | 頁數，0 開始                     |
| pageLimit     | integral | 每頁筆數，預設 10 筆             |

```json
{
  "cmdNo": 21,
  "status": 1,
  "message": "",
  "data": {
    "total": 1856,
    "totalPage": 186,
    "list": [
      {
        "serial": 1,
        "maid": 1, //代理代碼
        "m_name": "", //代理名稱
        "m_account": "agent", //代理帳號
        "said": 0, //上層代理代碼，0代表該代理是總代理
        "cmd_no": 500, //指令代碼
        "taid": 0, //對象代理代碼
        "t_name": null, //對象代理名稱
        "t_account": null, //對象代理帳號
        "request": {
          //呼叫API時的參數
          "account": "agent",
          "password": "********",
          "lang": "zh_tw"
        },
        "ip": "192.168.65.1", //呼叫API時IP
        "updated_at": "2024-10-22 02:13:33", //呼叫API時間
        "cmdNoName": "總代理端-總代理-登入" //API說明
      }
    ]
  }
}
```

### 查詢投注記錄 cmdNo=22

| ParameterName | DataType | Description                                               |
| ------------- | -------- | --------------------------------------------------------- |
| said          | integral | 上層代理代碼                                              |
| uuid          | integral | 會員代碼                                                  |
| gameProvider  | string   | 遊戲提供商                                                |
| gameCategory  | string   | 遊戲分類代碼 1:真人 2:彩票 3:電子 4:體育 5:棋牌 10:區塊鏈 |
| gameType      | string   | 遊戲類別(代碼)                                            |
| ip            | string   | IP                                                        |
| stime         | string   | 開始時間(+8 時區)                                         |
| etime         | string   | 結束時間(+8 時區)                                         |
| pageNum       | integral | 頁數，0 開始                                              |
| pageLimit     | integral | 每頁筆數，預設 10 筆                                      |

```json
{
  "cmdNo": 22,
  "status": 1,
  "message": "",
  "data": {
    "total": 2,
    "totalPage": 1,
    "list": [
      {
        "serial": 12,
        "uuid": 2, //用戶代碼
        "said": 4, //上層代理代碼
        "game_provider": "we", //遊戲提供商
        "bet_date_time": 1731426320, //下注時間
        "settlement_time": 1731426333, //結算時間
        "table_id": "STUDIO-DT-12", //桌號
        "game_round_id": "FDTC01241112F16R0014", //局號
        "transaction_id": "d8662386-8f68-4b8a-b93d-637f842f1de8", //交易編號
        "bet_amount": 60000, //投注金額
        "valid_bet_amount": 60000, //有效投注金額
        "winloss_amount": 60000, //輸贏金額
        "game_category": "真人[1]", //遊戲分類，括弧後面是遊戲分類代碼
        "game_type": "百家樂[sabac]", //遊戲名稱，括弧後面是遊戲代碼
        "ip": "18.140.73.9", //投注時IP，不一定會有值
        "created_at": "2024-11-13 17:00:56"
      }
    ]
  }
}
```

### 支付類型列表 cmdNo=23

| ParameterName  | DataType | Description              |
| -------------- | -------- | ------------------------ |
| methodLangCode | string   | 多語系支付代碼(下方說明) |
| action         | string   | 出入金 (D:入, W:出)      |
| pageNum        | integral | 頁數，0 開始             |
| pageLimit      | integral | 每頁筆數，預設 10 筆     |

```json
{
  "cmdNo": 23,
  "status": 1,
  "message": "",
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "mtid": 1,
        "method_lang_code": "001", //多語系支付代碼
        "action": "入款", //出入金類別
        "created_at": "2024-11-14 13:15:56",
        "updated_at": "2024-11-14 13:15:56",
        "method_name": "Bank transfer"
      }
    ]
  }
}
```

#### 多語系支付代碼

詳請見表<br/>
https://docs.google.com/spreadsheets/d/1ldp7npnMXjC20SYNDnKv7KaAF9dAVweQRwAMuVy5bDg/edit?gid=1894553015#gid=1894553015 <br/>
請移除前面關鍵字`PaymentMethod`後，才是 API 使用代碼<br/>
例：`PaymentMethod001` 應為 `001`

### 新增支付類型 cmdNo=24

| ParameterName  | DataType          | Description         |
| -------------- | ----------------- | ------------------- |
| methodLangCode | string (Required) | 多語系支付代碼      |
| action         | string (Required) | 出入金 (D:入, W:出) |

```json
{
  "cmdNo": 24,
  "status": 1,
  "message": ""
}
```

### 刪除支付類型 cmdNo=25

| ParameterName | DataType            | Description |
| ------------- | ------------------- | ----------- |
| mtid          | integral (Required) | 支付類型 id |

```json
{
  "cmdNo": 25,
  "status": 1,
  "message": ""
}
```

### 遊戲權限設定 cmdNo=26

| ParameterName        | DataType            | Description                              |
| -------------------- | ------------------- | ---------------------------------------- |
| maid                 | integral (Required) | 代理代碼 (與會員代碼擇一必填)            |
| uuid                 | integral (Required) | 會員代碼 (與會員代碼擇一必填)            |
| gameProvider         | string (Required)   | 遊戲商代碼                               |
| gameProviderStatus   | integral (Required) | 遊戲商狀態                               |
| liveStatus           | integral (Required) | 真人狀態                                 |
| lotteryStatus        | integral (Required) | 彩票狀態                                 |
| slotStatus           | integral (Required) | 電子狀態                                 |
| sportStatus          | integral (Required) | 體育狀態                                 |
| chessStatus          | integral (Required) | 棋牌狀態                                 |
| blockchainStatus     | integral (Required) | 區塊鏈狀態                               |
| liveGameStatus       | json                | 真人遊戲狀態 json 格式 {"BAC":0, "DI":0} |
| lotteryGameStatus    | json                | 彩票遊戲狀態 json 格式                   |
| slotGameStatus       | json                | 電子遊戲狀態 json 格式                   |
| sportGameStatus      | json                | 體育遊戲狀態 json 格式                   |
| chessGameStatus      | json                | 棋牌遊戲狀態 json 格式                   |
| blockchainGameStatus | json                | 區塊鏈遊戲狀態 json 格式                 |

狀態不管 0 還是 1 都傳

遊戲狀態定義檔請洽 PM

代理代碼 與 會員代碼 擇一傳入，另一方帶 0

status: 1 為成功

### 取得遊戲權限 cmdNo=27

| ParameterName               | DataType            | Description                                 |
| --------------------------- | ------------------- | ------------------------------------------- |
| maid                        | integral (Required) | 代理代碼 (代理代碼 或 會員代碼只能擇一抓取) |
| uuid                        | integral (Required) | 會員代碼                                    |
| gameProvider                | string (Required)   | 遊戲商代碼 (若查無資料則必填)               |
| isGetIndividualGameSettings | integral            | 是否只取得個別遊戲設定 0:否 1:是            |

代理代碼 與 會員代碼 擇一傳入，另一方帶 0

```json
{
  "cmdNo": 27,
  "status": 1,
  "message": "",
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "maid": 1,
        "said": 0,
        "uuid": 0,
        "agent_tree": null,
        "game_provider": "we",
        "game_provider_status": 1,
        "live_status": 1,
        "live_game_status": {
          "BAC": 0,
          "FAN": 0,
          "CG": 0
        },
        "lottery_status": 1,
        "lottery_game_status": [],
        "slot_status": 1,
        "slot_game_status": [],
        "sport_status": 1,
        "sport_game_status": [],
        "chess_status": 1,
        "chess_game_status": [],
        "blockchain_status": 1,
        "blockchain_game_status": []
      }
    ]
  }
}
```

若沒設定過權限

```json
{
  "cmdNo": 27,
  "status": 1,
  "message": "",
  "data": {
    "total": 0,
    "totalPage": 0,
    "list": {
      "game_provider": "jdb",
      "game_provider_status": 1,
      "live_status": 1,
      "live_game_status": [],
      "lottery_status": 1,
      "lottery_game_status": [],
      "slot_status": 1,
      "slot_game_status": [],
      "sport_status": 1,
      "sport_game_status": [],
      "chess_status": 1,
      "chess_game_status": [],
      "blockchain_status": 1,
      "blockchain_game_status": []
    }
  }
}
```

取得遊戲大類 (isGetIndividualGameSettings = 0)

```json
{
  "cmdNo": 27,
  "status": 1,
  "message": "",
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "maid": 0,
        "uuid": 1,
        "game_provider": "we",
        "game_provider_status": 1,
        "live_status": 1,
        "lottery_status": 1,
        "slot_status": 1,
        "sport_status": 0,
        "chess_status": 1,
        "blockchain_status": 1
      }
    ]
  }
}
```

取得遊戲大類預設值 (isGetIndividualGameSettings = 0)

```json
{
  "cmdNo": 27,
  "status": 1,
  "message": "",
  "data": {
    "total": 0,
    "totalPage": 0,
    "list": {
      "game_provider": "jdb",
      "game_provider_status": 1,
      "live_status": 1,
      "lottery_status": 1,
      "slot_status": 1,
      "sport_status": 1,
      "chess_status": 1,
      "blockchain_status": 1
    }
  }
}
```

### 公告列表 cmdNo=29

| ParameterName         | DataType | Description                         |
| --------------------- | -------- | ----------------------------------- |
| acid                  | integral | 公告 id                             |
| state                 | string   | 狀態                                |
| identifyTitle         | string   | 識別標題 (模糊搜尋)                 |
| displayMode           | string   | 顯示方式 1:跑馬燈 2:彈跳視窗 3:全部 |
| announcementType      | string   | 公告類別 1:每日 2:每月 3:時間區間   |
| announcementStartDate | string   | 公告開始日期 (+8 時區)              |
| announcementEndDate   | string   | 公告結束日期 (+8 時區)              |
| pageNum               | string   | 頁數，0 開始                        |
| pageLimit             | string   | 每頁筆數，預設 10 筆                |

```json
{
  "cmdNo": 29,
  "status": 1,
  "message": "",
  "data": {
    "total": 8,
    "totalPage": 1,
    "list": [
      {
        "acid": 1,
        "state": "1",
        "identify_title": "測試公告1",
        "display_mode": "1",
        "announcement_type": "0",
        "announcement_start_date": "2024-12-14 11:00:00",
        "announcement_end_date": "2024-12-31 11:00:00",
        "daily_announcement_start_date": "",
        "daily_announcement_end_date": "",
        "monthly_announcement_start_date": "",
        "monthly_announcement_end_date": "",
        "marquee_sort": "0",
        "marquee_shows_seconds": 0,
        "time_interval_next_marquee": 0,
        "pop_up_window_sort": "0",
        "time_interval_next_pop_up": 0,
        "creator": 2,
        "updater": 2,
        "created_at": "2024-12-06 17:08:24",
        "updated_at": "2024-12-06 17:15:48",
        // stateName 狀態判斷 即將到來（半個月內）、逾期、進行中、關閉中（停用）、時間未到
        "stateName": "即將到來",
        "data": [
          // 實際多語系內容
          {
            "serial": 2,
            "acid": "1",
            "language": "en-us",
            "title": "test111",
            "content": "ddddd",
            "creator": 2,
            "updater": null,
            "created_at": "2024-12-06 17:18:52",
            "updated_at": "2024-12-06 17:18:52"
          },
          {
            "serial": 1,
            "acid": "1",
            "language": "zh-tw",
            "title": "測試111",
            "content": "asdfasdfaf",
            "creator": 2,
            "updater": 2,
            "created_at": "2024-12-06 17:17:20",
            "updated_at": "2024-12-06 17:18:20"
          }
        ]
      },
      {
        "acid": 2,
        "state": "1",
        "identify_title": "測試公告2",
        "display_mode": "2",
        "announcement_type": "1",
        "announcement_start_date": "2024-12-05 11:00:00",
        "announcement_end_date": "2024-12-08 11:12:00",
        "daily_announcement_start_date": "11:00",
        "daily_announcement_end_date": "11:05",
        "monthly_announcement_start_date": "",
        "monthly_announcement_end_date": "",
        "marquee_sort": "0",
        "marquee_shows_seconds": 0,
        "time_interval_next_marquee": 0,
        "pop_up_window_sort": "0",
        "time_interval_next_pop_up": 0,
        "creator": 2,
        "updater": null,
        "created_at": "2024-12-06 17:09:40",
        "updated_at": "2024-12-06 17:09:40",
        "stateName": "逾期",
        "data": [
          {
            "serial": 3,
            "acid": "2",
            "language": "en-us",
            "title": "test111",
            "content": "ddddd",
            "creator": 2,
            "updater": null,
            "created_at": "2024-12-06 17:19:10",
            "updated_at": "2024-12-06 17:19:10"
          }
        ]
      }
    ]
  }
}
```

### 新增公告 cmdNo=30

| ParameterName                | DataType          | Description                                                                   |
| ---------------------------- | ----------------- | ----------------------------------------------------------------------------- |
| identifyTitle                | string (Required) | 識別標題                                                                      |
| state                        | string (Required) | 狀態                                                                          |
| displayMode                  | string (Required) | 顯示方式 1:跑馬燈 2:彈跳視窗 3:全部                                           |
| announcementType             | string (Required) | 公告類別 1:每日 2:每月 3:時間區間                                             |
| announcementStartDate        | string (Required) | 公告開始日期 (+8 時區) 年月日時分 yyyy-mm-dd hh:mm:ss                         |
| announcementEndDate          | string (Required) | 公告結束日期 (+8 時區) 年月日時分 yyyy-mm-dd hh:mm:ss                         |
| dailyAnnouncementStartDate   | string            | 每日公告開始日期 (+8 時區) 時分 (填了開始日期，結束日期就必填) hh:mm          |
| dailyAnnouncementEndDate     | string            | 每日公告結束日期 (+8 時區) 時分 (填了開始日期，結束日期就必填) hh:mm          |
| monthlyAnnouncementStartDate | string            | 每月幾號公告開始日期 (+8 時區) 日時分 (填了開始日期，結束日期就必填) dd hh:mm |
| monthlyAnnouncementEndDate   | string            | 每月幾號公告結束日期 (+8 時區) 日時分 (填了開始日期，結束日期就必填) dd hh:mm |
| marqueeSort                  | integral          | 跑馬燈排序                                                                    |
| marqueeShowsSeconds          | integral          | 跑馬燈一輪顯示的秒數 0:無限制                                                 |
| timeIntervalNextMarquee      | integral          | 與下一個跑馬燈間隔時間 0:不會自動更換 單位:秒                                 |
| popUpWindowSort              | integral          | 彈跳視窗排序                                                                  |
| timeIntervalNextPopUp        | integral          | 與下一個彈跳視窗間隔時間 0:不會自動更換 單位:秒                               |

公告類別為每日的話，每日公告開始與結束時間必填
公告類別為每月的話，每月幾號公告開始與結束時間必填

此表只更新基本公告設定，內容與多語系需使用 CmdNo34 編輯多語系公告 API

status: 1 為成功

### 編輯公告 cmdNo=31

| ParameterName                | DataType            | Description                                           |
| ---------------------------- | ------------------- | ----------------------------------------------------- |
| acid                         | integral (Required) | 公告 id                                               |
| state                        | string              | 狀態                                                  |
| displayMode                  | string              | 顯示方式 1:跑馬燈 2:彈跳視窗 3:全部                   |
| announcementType             | string              | 公告類別 1:每日 2:每月 3:時間區間                     |
| announcementStartDate        | string              | 公告開始日期 (+8 時區) 年月日時分 yyyy-mm-dd hh:mm:ss |
| announcementEndDate          | string              | 公告結束日期 (+8 時區) 年月日時分 yyyy-mm-dd hh:mm:ss |
| dailyAnnouncementStartDate   | string              | 每日公告開始日期 (+8 時區) 時分 hh:mm                 |
| dailyAnnouncementEndDate     | string              | 每日公告結束日期 (+8 時區) 時分 hh:mm                 |
| monthlyAnnouncementStartDate | string              | 每月幾號公告開始日期 (+8 時區) 日時分 dd hh:mm        |
| monthlyAnnouncementEndDate   | string              | 每月幾號公告結束日期 (+8 時區) 日時分 dd hh:mm        |
| marqueeSort                  | integral            | 跑馬燈排序                                            |
| marqueeShowsSeconds          | integral            | 跑馬燈一輪顯示的秒數 0:無限制                         |
| timeIntervalNextMarquee      | integral            | 與下一個跑馬燈間隔時間 0:不會自動更換 單位:秒         |
| popUpWindowSort              | integral            | 彈跳視窗排序                                          |
| timeIntervalNextPopUp        | integral            | 與下一個彈跳視窗間隔時間 0:不會自動更換 單位:秒       |

status: 1 為成功

### 刪除公告 cmdNo=32

| ParameterName | DataType            | Description |
| ------------- | ------------------- | ----------- |
| acid          | integral (Required) | 公告 id     |

status: 1 為成功

### 複製公告 cmdNo=33

| ParameterName | DataType           | Description |
| ------------- | ------------------ | ----------- |
| acid          | integral(Required) | 公告 id     |
| identifyTitle | string (Required)  | 識別標題    |

status: 1 為成功

### 設定多語系公告 cmdNo=34

| ParameterName | DataType           | Description |
| ------------- | ------------------ | ----------- |
| acid          | integral(Required) | 公告 id     |
| language      | string             | 多語系      |
| title         | string             | 標題        |
| content       | string             | 內容        |

公告 id 若已存在，則編輯已存在的公告 id
不存在則新增

status: 1 為成功

### 查詢拆帳報表 cmdNo=40

| ParameterName | DataType | Description                                                            |
| ------------- | -------- | ---------------------------------------------------------------------- |
| maid          | integral | 0:代表公司端                                                           |
| said          | integral | 上層代理代碼                                                           |
| gameProvider  | string   | 遊戲提供商                                                             |
| gameCategory  | string   | 遊戲分類代碼 1:真人 2:彩票 3:電子 4:體育 5:棋牌 10:區塊鏈              |
| gameType      | string   | 遊戲類別(代碼)                                                         |
| reportType    | integral | 報表類別 1:日 2:週 3:月                                                |
| reportSubType | integral | 報表子類別 1:一般報表(預設) 2:遊戲商報表 3:遊戲分類報表 4:遊戲類別報表 |
| stime         | string   | 開始時間(+8 時區)                                                      |
| etime         | string   | 結束時間(+8 時區)                                                      |
| pageNum       | integral | 頁數，0 開始                                                           |
| pageLimit     | integral | 每頁筆數，預設 10 筆                                                   |

```json
{
  "cmdNo": 40,
  "status": 1,
  "message": "",
  "data": {
    "total": 147,
    "totalPage": 15,
    "list": [
      {
        "report_date": "2024-12-06", //拆帳日期
        "maid": 7, //代理代碼
        "said": 1, //上層代理代碼，0代表總代理
        "game_provider": "sp", //遊戲商代碼 reportSubType=2才有
        "game_category": "電子[3]", //遊戲分類[遊戲分類代碼] reportSubType=3才有
        "game_type": "比基尼狂熱[spEG-SLOT-A013]", //遊戲名稱[遊戲代碼] reportSubType=4才有
        "debt_amount": 0, //拆帳金額(實拿金額) 10000:1
        "paid_amount": 0, //上繳金額 10000:1
        "member_amount": 0, //[直屬]會員上繳金額 10000:1
        "dir_distinct_bet_total": 0, //[直屬]不重複會員投注次數
        "dir_bet_total": 0, //[直屬]總投注次數
        "dir_register_agent_total": 0, //[直屬]總註冊代理數量
        "dir_agent_total": 1, //[直屬]總代理數量
        "dir_register_member_total": 0, //[直屬]總註冊會員數量
        "dir_member_total": 8, //[直屬]總會員數量
        "dir_bet_amount": 0, //[直屬]投注金額 10000:1
        "dir_valid_bet_amount": 0, //[直屬]有效投注额 10000:1
        "dir_winloss_amount": 0, //[直屬]赢输金额，如果是負數代表代理賠錢 10000:1
        "dir_rebate_amount": 0, //[直屬]返水金額 10000:1
        "dir_deposit": 0, //[直屬]入金點數 10000:1
        "dir_withdraw": 0, //[直屬]出金點數 10000:1
        "dir_deposit_total": 0, //[直屬]入金訂單筆數
        "dir_withdraw_total": 0, //[直屬]出金訂單筆數
        "distinct_bet_total": 0, //[全線]不重複會員投注次數
        "bet_total": 0, //[全線]總投注次數
        "register_agent_total": 0, //[全線]總註冊代理數量
        "agent_total": 2, //[全線]總代理數量
        "register_member_total": 0, //[全線]總註冊會員數量
        "member_total": 10, //[全線]總會員數量
        "bet_amount": 0, //[全線]投注金額 10000:1
        "valid_bet_amount": 0, //[全線]有效投注额 10000:1
        "winloss_amount": 0, //[全線]赢输金额 10000:1
        "rebate_amount": 0, //[全線]返水金額 10000:1
        "deposit": 0, //[全線]入金點數 10000:1
        "withdraw": 0, //[全線]出金點數 10000:1
        "deposit_total": 0, //[全線]入金訂單筆數
        "withdraw_total": 0, //[全線]出金訂單筆數
        "updated_at": "2024-12-06 11:37:15" //最後更新時間
      }
    ]
  }
}
```

### 代理-列表 cmdNo=50

| ParameterName | DataType | Description                   |
| ------------- | -------- | ----------------------------- |
| account       | string   | 帳號                          |
| said          | integral | 上層代理代碼，0 代表總代理    |
| rid           | integral | 角色代碼                      |
| currency      | string   | 幣值代碼                      |
| name          | string   | 名稱                          |
| parentStatus  | integral | 上層狀態 0:停用 1:啟用 9:觀看 |
| status        | integral | 狀態 0:停用 1:啟用 9:觀看     |
| pageNum       | integral | 頁數，0 開始                  |
| pageLimit     | integral | 每頁筆數，預設 10 筆          |

### 代理-新增 cmdNo=51

| ParameterName | DataType            | Description                                 |
| ------------- | ------------------- | ------------------------------------------- |
| account       | string (Required)   | 帳號                                        |
| password      | string (Required)   | 密碼                                        |
| said          | integral (Required) | 上層代理代碼，0 代表總代理                  |
| rid           | integral (Required) | 角色代碼                                    |
| currency      | string (Required)   | 幣值代碼，只有 1 級代理需要                 |
| debtIdentity  | integral (Required) | 用戶型別，只有 1 級代理需要， 1:信用 2:現金 |
| name          | string              | 名稱                                        |

### 代理-修改 cmdNo=52

| ParameterName | DataType           | Description                        |
| ------------- | ------------------ | ---------------------------------- |
| maid          | integral(Required) | 代理代碼                           |
| rid           | integral           | 角色代碼                           |
| password      | string             | 密碼                               |
| name          | string             | 名稱                               |
| status        | integral           | 狀態 0:停用 1:啟用 9:觀看          |
| syncSubAgent  | integral           | 狀態同步下線(代理，會員) 1:是 0:否 |

### 代理-取得不返水遊戲 cmdNo=56

| ParameterName | DataType | Description |
| ------------- | -------- | ----------- |
| maid          | integral | 代理代碼    |
| gameProvider  | string   | 遊戲商代碼  |

```json
{
  "cmdNo": 56,
  "status": 1,
  "message": "",
  "data": {
    "spEG-SLOT-A051": 1, //key 遊戲代碼
    "spEG-SLOT-A043": 1
  }
}
```

### 代理-設定不返水遊戲 cmdNo=57

| ParameterName  | DataType            | Description                                     |
| -------------- | ------------------- | ----------------------------------------------- |
| maid           | integral (Required) | 代理代碼 (管理員必填，非管理員直接帶入當下代理) |
| gameProvider   | string (Required)   | 遊戲商代碼                                      |
| liveGame       | json                | 真人遊戲 json 格式 "["BAC","LO"]"               |
| lotteryGame    | json                | 彩票遊戲 json 格式 "["BAC","LO"]"               |
| slotGame       | json                | 電子遊戲 json 格式 "["BAC","LO"]"               |
| sportGame      | json                | 體育遊戲 json 格式 "["BAC","LO"]"               |
| chessGame      | json                | 棋牌遊戲 json 格式 "["BAC","LO"]"               |
| blockchainGame | json                | 區塊鏈遊戲 json 格式 "["BAC","LO"]"             |

至少需填一個遊戲設定
status: 1 為成功

### 會員-列表 cmdNo=60

| ParameterName | DataType            | Description                   |
| ------------- | ------------------- | ----------------------------- |
| account       | string              | 帳號                          |
| said          | integral (Required) | 上層代理代碼                  |
| name          | string              | 名稱                          |
| email         | string              | Email                         |
| countryCodes  | string              | 國碼(手機)                    |
| phoneNumber   | string              | 手機號碼，不含國碼            |
| parentStatus  | integral            | 上層狀態 0:停用 1:啟用 9:觀看 |
| status        | integral            | 狀態 0:停用 1:啟用 9:觀看     |
| pageNum       | integral            | 頁數，0 開始                  |
| pageLimit     | integral            | 每頁筆數，預設 10 筆          |

```json
{
  cmdNo: 760,
  status: 1,
  message: "",
  data: {
    total: 40,
    totalPage: 4,
    list: [
      {
        uuid: 27,         //會員代碼
        said: 30,         //上層代理代碼
        debt_identity: 1, //用戶型別 1:信用 2:現金
        high_risk: 0,     //是否為高風險用戶 1:是 0:否 禁止投注/禁止出款/禁止入款	
        able_withdraw: 1, //是否可以出款 1:是 0:否
        able_deposit: 1,  //是否可以入款 1:是 0:否
        name: null,       //名稱
        agent_tree: "|1|30|", //代理階層圖
        tag_tree: "|1|2|3|4|",  //標籤ID陣列
        tag_name_tree: [        //標籤說明陣列(有多語系)
          "禁止投注",
          "禁止出款",
          "禁止入款",
          "標A",
          "標B"
        ],
        currency: "VND",      //幣值
        p_status: 1,          //上層狀態 0:停用 1:啟用 9:觀看
        status: 1,            //狀態 0:停用 1:啟用 9:觀看
        amount: 0,            //用戶餘額 10000:1
        rebate_total_amount: 0,       //累積總返水金額 10000:1
        rebate_daily_total_amount: 0, //本日總返水金額 10000:1
        account: "T1731384894_1",     //帳號
        email: "abc@gmail.com",       //Email
        country_codes: "886",         //國碼(手機)
        phone: "911111111",           //手機號碼(不含國碼)
        updated_at: "2024-11-25 12:51:48" //最後更新時間
      }
    ]
  }
}
```

### 會員-新增 cmdNo=61

| ParameterName | DataType            | Description |
|---------------|---------------------|-------------|
| account       | string (Required)   | 帳號          |
| password      | string (Required)   | 密碼          |
| said          | integral (Required) | 上層代理代碼      |
| name          | string              | 名稱          |
| email         | string              | Email       |
| countryCodes  | string              | 國碼(手機)      |
| phoneNumber   | string              | 手機號碼，不含國碼   |

### 會員-修改 cmdNo=62

| ParameterName | DataType           | Description       |
|---------------|--------------------|-------------------|
| uuid          | integral(Required) | 會員代碼              |
| password      | string             | 密碼                |
| name          | string             | 名稱                |
| email         | string             | Email             |
| countryCodes  | string             | 國碼(手機)            |
| phoneNumber   | string             | 手機號碼，不含國碼         |
| status        | integral           | 狀態 0:停用 1:啟用 9:觀看 |
| highRisk      | integral           | 是否為高風險用戶 1:是 0:否  |
| ableWithdraw  | integral           | 是否可以出款 1:是 0:否    |
| ableDeposit   | integral           | 是否可以入款 1:是 0:否    |

[回傳說明](#60 response)

### 會員-返水累積值歸零 cmdNo=63

| ParameterName | DataType           | Description |
|---------------|--------------------|-------------|
| uuid          | integral(Required) | 會員代碼        |

### 會員-金流明細列表 cmdNo=70

| ParameterName | DataType           | Description     |
|---------------|--------------------|-----------------|
| uuid          | integral(Required) | 會員代碼            |
| pid           | string             | 訂單號             |
| agent         | string             | 代理代碼            |
| payment       | string             | 金流商代碼 例：FastPay |
| acton         | string             | 出入金 D:入, W:出    |
| pageNum       | integral           | 頁數，0 開始         |
| pageLimit     | integral           | 每頁筆數，預設 10 筆    |

```json
{
  "cmdNo": 70,
  "status": 1,
  "message": "",
  "data": {
    "total": 33,
    "totalPage": 4,
    "list": [
      {
        "pid": "98967f-20241108152910-24481",
        "payment": "FastPay",
        "action": "D",
        "merchant": "fast203676",
        "method": "001", // 支付類型多語系代碼
        "amount": 600000000,
        "timestamp": "1731050950",
        "currency": "VND",
        "extra": "",
        "state": "未付款",
        "uuid": 1,
        "said": 4,
        "agent_tree": "|3|4|",
        "trade_at": "2024-11-08 15:29:10",
        "created_at": "2024-11-08 15:29:11",
        "updated_at": "2024-11-14 14:44:41",
        "method_name": "Bank transfer" // 支付類型名稱
      },
      {
        "pid": "98967f-20241112131226-45360",
        "payment": "FastPay",
        "action": "D",
        "merchant": "fast203676",
        "method": "002",
        "amount": 900000000,
        "timestamp": "1731388346",
        "currency": "VND",
        "extra": "",
        "state": "未付款",
        "uuid": 1,
        "said": 4,
        "agent_tree": "|3|4|",
        "trade_at": "2024-11-12 13:12:26",
        "created_at": "2024-11-12 13:12:27",
        "updated_at": "2024-11-14 14:44:50",
        "method_name": "Momo QR"
      }
    ]
  }
}
```

### 會員-銀行卡列表 cmdNo=71

| ParameterName | DataType | Description                   |
|---------------|----------|-------------------------------|
| uuid          | string   | 會員代碼                          |
| bankLangCode  | string   | 銀行代碼 <br/> ex: 002，詳情代碼表請洽 PM |
| bankAccount   | string   | 銀行帳號                          |
| realName      | string   | 真實姓名                          |
| currency      | string   | 幣別                            |
| pageNum       | integral | 頁數，0 開始                       |
| pageLimit     | integral | 每頁筆數，預設 10 筆                  |

```json
{
  "cmdNo": 71,
  "status": 1,
  "message": "",
  "data": {
    "total": 6,
    "totalPage": 1,
    "list": [
      {
        "baid": 7,
        "bank_lang_code": "022",
        "bank_account": "1234968504",
        "currency": "VND",
        "real_name": "NNTEST",
        "alias": "",
        "created_at": "2024-11-12 13:28:46",
        "updated_at": "2024-11-12 13:28:46",
        "bank_name": "NGAN HANG TMCP A CHAU (ACB)" // 會轉成多語系銀行名稱
      },
      {
        "baid": 8,
        "bank_lang_code": "002",
        "bank_account": "12395759333",
        "currency": "VND",
        "real_name": "TEST222",
        "alias": "",
        "created_at": "2024-11-12 15:11:18",
        "updated_at": "2024-11-12 15:11:18",
        "bank_name": "ACB BANK"
      }
    ]
  }
}
```

### 會員-增減點數 cmdNo=80

| ParameterName | DataType           | Description                                          |
|---------------|--------------------|------------------------------------------------------|
| uuid          | integral(Required) | 會員代碼                                                 |
| amount        | integral(Required) | 點數 1=10000<br/>例 1TWD = 10000 點                      |
| wdCode        | integral           | 金流類別代碼(下方定義) <br/>原則上 1000~1999 加點<br/> 2000~2999 減點 |

## 總代理端

### 總代理-登入 cmdNo=500

| ParameterName | DataType          | Description |
|---------------|-------------------|-------------|
| account       | string (Required) | 帳號          |
| password      | string (Required) | 密碼          |
| lang          | string            | 語系，預設 en-us |

### 遊戲列表 cmdNo=501

無需任何參數

```json
{
  "cmdNo": 15,
  "status": 1,
  "message": "",
  "data": {
    "we": [
      //遊戲廠商代碼
      {
        "c": "BAC", //遊戲代碼
        "n": "經典百家樂" //遊戲名稱
      }
    ],
    "sa": [
      {
        "c": "bac",
        "n": "百家樂"
      }
    ]
  }
}
```

### 會員-金流明細列表 cmdNo=505

| ParameterName | DataType           | Description            |
| ------------- | ------------------ | ---------------------- |
| uuid          | integral(Required) | 會員代碼               |
| pid           | string             | 訂單號                 |
| payment       | string             | 金流商代碼 例：FastPay |
| acton         | string             | 出入金 D:入, W:出      |
| pageNum       | integral           | 頁數，0 開始           |
| pageLimit     | integral           | 每頁筆數，預設 10 筆   |

```json
{
  "cmdNo": 505,
  "status": 1,
  "message": "",
  "input": {
    "cmdNo": 505,
    "uuid": "49",
    "pid": "DP-98964F-673D3A35-152D95370",
    "payment": "FastPay",
    "action": ""
  },
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "pid": "DP-98964F-673D3A35-152D95370",
        "payment": "FastPay",
        "action": "D",
        "merchant": "fast203676",
        "method": "001",
        "amount": 600000000,
        "currency": "VND",
        "extra": "",
        "state": "未付款",
        "uuid": 49,
        "said": 30,
        "agent_tree": "|1|30|",
        "trade_at": 1732065845,
        "created_at": "2024-11-20 09:24:05",
        "updated_at": "2024-11-25 10:48:27",
        "method_name": "Bank transfer"
      }
    ]
  }
}
```

### 會員-銀行卡列表 cmdNo=506

| ParameterName | DataType | Description                               |
| ------------- | -------- | ----------------------------------------- |
| uuid          | string   | 會員代碼                                  |
| bankLangCode  | string   | 銀行代碼 <br/> ex: 002，詳情代碼表請洽 PM |
| bankAccount   | string   | 銀行帳號                                  |
| realName      | string   | 真實姓名                                  |
| currency      | string   | 幣別                                      |
| pageNum       | integral | 頁數，0 開始                              |
| pageLimit     | integral | 每頁筆數，預設 10 筆                      |

只看得到自己底下會員的銀行卡

```json
{
  "cmdNo": 506,
  "status": 1,
  "message": "",
  "input": {
    "cmdNo": 506,
    "uuid": "",
    "bankLangCode": "",
    "bankAccount": "",
    "realName": "",
    "currency": ""
  },
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "baid": 11,
        "bank_lang_code": "001",
        "bank_account": "test55647888",
        "currency": "VND",
        "real_name": "aaa",
        "alias": "測試用",
        "created_at": "2024-12-02 09:46:48",
        "updated_at": "2024-12-02 09:46:48",
        "bank_name": "VP BANK"
      }
    ]
  }
}
```

### 總代端-全部-查詢操作日誌 cmdNo=521

| ParameterName | DataType | Description                      |
| ------------- | -------- | -------------------------------- |
| cmd           | integral | 指令代碼                         |
| userType      | integral | 對象類別 2:代理(預設) 3:會員     |
| type          | integral | 類別 1:正常日誌(預設) 2:錯誤日誌 |
| maid          | integral | 代理代碼，對象類別:2 使用        |
| said          | integral | 上層代理代碼，對象類別:2,3 使用  |
| uuid          | integral | 會員代碼，對象類別:3 使用        |
| taid          | integral | 對象代碼                         |
| ip            | string   | IP                               |
| stime         | string   | 開始時間(+8 時區)                |
| etime         | string   | 結束時間(+8 時區)                |
| pageNum       | integral | 頁數，0 開始                     |
| pageLimit     | integral | 每頁筆數，預設 10 筆             |

```json
{
  "cmdNo": 521,
  "status": 1,
  "message": "",
  "data": {
    "total": 1628,
    "totalPage": 163,
    "list": [
      {
        "serial": 1,
        "maid": 2, //代理代碼
        "m_name": "23434", //代理名稱
        "m_account": "admin", //代理帳號
        "cmd_no": 100, //指令代碼
        "taid": 0, //對象代碼
        "t_name": null, //對象名稱
        "t_account": null, //對象帳號
        "request": {
          //呼叫API時的參數
          "account": "admin",
          "password": "********",
          "lang": "zh_tw"
        },
        "ip": "192.168.65.1", //呼叫API時IP
        "updated_at": "2024-10-22 01:26:20", //呼叫API時間
        "cmdNoName": "管理端-管理員-登入" //API說明
      }
    ]
  }
}
```

### 查詢投注記錄 cmdNo=522

| ParameterName | DataType | Description          |
| ------------- | -------- | -------------------- |
| said          | integral | 代理代碼             |
| uuid          | integral | 會員代碼             |
| gameProvider  | string   | 遊戲提供商           |
| ip            | string   | IP                   |
| stime         | string   | 開始時間(+8 時區)    |
| etime         | string   | 結束時間(+8 時區)    |
| pageNum       | integral | 頁數，0 開始         |
| pageLimit     | integral | 每頁筆數，預設 10 筆 |

### 查詢會員報表 cmdNo=523

| ParameterName | DataType | Description             |
| ------------- | -------- | ----------------------- |
| said          | integral | 上層代理代碼            |
| uuid          | integral | 會員代碼                |
| reportType    | integral | 報表類別 1:日 2:週 3:月 |
| ip            | string   | IP                      |
| sDate         | string   | 開始時間(+8 時區)       |
| eDate         | string   | 結束時間(+8 時區)       |
| pageNum       | integral | 頁數，0 開始            |
| pageLimit     | integral | 每頁筆數，預設 10 筆    |

### 總代端-會員-查詢餘額異動紀錄 cmdNo=524

| ParameterName | DataType | Description            |
| ------------- | -------- | ---------------------- |
| maid          | integral | 代理代碼(執行者)       |
| said          | integral | 上層代理代碼           |
| uuid          | integral | 會員代碼               |
| ip            | string   | IP                     |
| wdCode        | integral | 金流類別代碼(下方定義) |
| stime         | string   | 開始時間(+8 時區)      |
| etime         | string   | 結束時間(+8 時區)      |
| pageNum       | integral | 頁數，0 開始           |
| pageLimit     | integral | 每頁筆數，預設 10 筆   |

```json
{
  "cmdNo": 724,
  "status": 1,
  "message": "",
  "data": {
    "total": 262,
    "totalPage": 27,
    "list": [
      {
        "macl_id": 336,
        "uuid": 49, //會員代碼
        "aid": 0, //管理者代碼
        "maid": 0, //代理代碼
        "said": 30, //上層代理代碼
        "transaction_id": "XXXXXX", //交易編號
        "currency": "VND", //幣值
        "wd_code": "FastPay充值[1200]", //金流類別說明[金流類別代碼]
        "wd_type": 1, //增減類別 0:減少 1:增加
        "c_amount": 800000000, //此次異動金額 10000:1
        "b_amount": 1982006018180, //用戶異動前金額 10000:1
        "a_amount": 1982806018180, //用戶異動後金額 10000:1
        "ip": "172.18.0.1", //IP
        "created_at": "2024-12-06 16:19:55" //記錄產生時間
      }
    ]
  }
}
```

### 代理-遊戲權限設定 cmdNo=525

| ParameterName        | DataType            | Description                              |
| -------------------- | ------------------- | ---------------------------------------- |
| maid                 | integral (Required) | 代理代碼 (與會員代碼擇一必填)            |
| uuid                 | integral (Required) | 會員代碼 (與會員代碼擇一必填)            |
| gameProvider         | string (Required)   | 遊戲商代碼                               |
| gameProviderStatus   | integral (Required) | 遊戲商狀態                               |
| liveStatus           | integral (Required) | 真人狀態                                 |
| lotteryStatus        | integral (Required) | 彩票狀態                                 |
| slotStatus           | integral (Required) | 電子狀態                                 |
| sportStatus          | integral (Required) | 體育狀態                                 |
| chessStatus          | integral (Required) | 棋牌狀態                                 |
| blockchainStatus     | integral (Required) | 區塊鏈狀態                               |
| liveGameStatus       | json                | 真人遊戲狀態 json 格式 {"BAC":0, "DI":0} |
| lotteryGameStatus    | json                | 彩票遊戲狀態 json 格式                   |
| slotGameStatus       | json                | 電子遊戲狀態 json 格式                   |
| sportGameStatus      | json                | 體育遊戲狀態 json 格式                   |
| chessGameStatus      | json                | 棋牌遊戲狀態 json 格式                   |
| blockchainGameStatus | json                | 區塊鏈遊戲狀態 json 格式                 |

狀態不管 0 還是 1 都傳

遊戲狀態定義檔請洽 PM

代理代碼 與 會員代碼 擇一傳入，另一方帶 0

status: 1 為成功

### 代理-取得遊戲權限 cmdNo=526

| ParameterName               | DataType            | Description                                 |
| --------------------------- | ------------------- | ------------------------------------------- |
| maid                        | integral (Required) | 代理代碼 (代理代碼 或 會員代碼只能擇一抓取) |
| uuid                        | integral (Required) | 會員代碼                                    |
| gameProvider                | string (Required)   | 遊戲商代碼 (若查無資料則必填)               |
| isGetIndividualGameSettings | integral            | 是否只取得個別遊戲設定 0:否 1:是            |

代理代碼 與 會員代碼 擇一傳入，另一方帶 0
只能取自己底下代理的遊戲權限

```json
{
  "cmdNo": 526,
  "status": 1,
  "message": "",
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "maid": 30,
        "said": 1,
        "uuid": 0,
        "agent_tree": "|1|30|",
        "game_provider": "we",
        "game_provider_status": 1,
        "live_status": 1,
        "live_game_status": {
          "BAC": 0
        },
        "lottery_status": 1,
        "lottery_game_status": [],
        "slot_status": 1,
        "slot_game_status": [],
        "sport_status": 1,
        "sport_game_status": [],
        "chess_status": 1,
        "chess_game_status": [],
        "blockchain_status": 1,
        "blockchain_game_status": []
      }
    ]
  }
}
```

若沒設定過權限

```json
{
  "cmdNo": 526,
  "status": 1,
  "message": "",
  "data": {
    "total": 0,
    "totalPage": 0,
    "list": {
      "game_provider": "jdb",
      "game_provider_status": 1,
      "live_status": 1,
      "live_game_status": [],
      "lottery_status": 1,
      "lottery_game_status": [],
      "slot_status": 1,
      "slot_game_status": [],
      "sport_status": 1,
      "sport_game_status": [],
      "chess_status": 1,
      "chess_game_status": [],
      "blockchain_status": 1,
      "blockchain_game_status": []
    },
  },
}
```

### 總代端-代理-拆帳報表 cmdNo=528

| ParameterName | DataType | Description                                                            |
| ------------- | -------- | ---------------------------------------------------------------------- |
| maid          | integral | 0:代表公司端                                                           |
| said          | integral | 上層代理代碼                                                           |
| gameProvider  | string   | 遊戲提供商                                                             |
| gameCategory  | string   | 遊戲分類代碼 1:真人 2:彩票 3:電子 4:體育 5:棋牌 10:區塊鏈              |
| gameType      | string   | 遊戲類別(代碼)                                                         |
| reportType    | integral | 報表類別 1:日 2:週 3:月                                                |
| reportSubType | integral | 報表子類別 1:一般報表(預設) 2:遊戲商報表 3:遊戲分類報表 4:遊戲類別報表 |
| stime         | string   | 開始時間(+8 時區)                                                      |
| etime         | string   | 結束時間(+8 時區)                                                      |
| pageNum       | integral | 頁數，0 開始                                                           |
| pageLimit     | integral | 每頁筆數，預設 10 筆                                                   |

```json
{
  "cmdNo": 528,
  "status": 1,
  "message": "",
  "data": {
    "total": 147,
    "totalPage": 15,
    "list": [
      {
        "report_date": "2024-12-06", //拆帳日期
        "maid": 7, //代理代碼
        "said": 1, //上層代理代碼，0代表總代理
        "game_provider": "sp", //遊戲商代碼 reportSubType=2才有
        "game_category": "電子[3]", //遊戲分類[遊戲分類代碼] reportSubType=3才有
        "game_type": "比基尼狂熱[spEG-SLOT-A013]", //遊戲名稱[遊戲代碼] reportSubType=4才有
        "debt_amount": 0, //拆帳金額(實拿金額) 10000:1
        "paid_amount": 0, //上繳金額 10000:1
        "member_amount": 0, //[直屬]會員上繳金額 10000:1
        "dir_distinct_bet_total": 0, //[直屬]不重複會員投注次數
        "dir_bet_total": 0, //[直屬]總投注次數
        "dir_register_agent_total": 0, //[直屬]總註冊代理數量
        "dir_agent_total": 1, //[直屬]總代理數量
        "dir_register_member_total": 0, //[直屬]總註冊會員數量
        "dir_member_total": 8, //[直屬]總會員數量
        "dir_bet_amount": 0, //[直屬]投注金額 10000:1
        "dir_valid_bet_amount": 0, //[直屬]有效投注额 10000:1
        "dir_winloss_amount": 0, //[直屬]赢输金额，如果是負數代表代理賠錢 10000:1
        "dir_rebate_amount": 0, //[直屬]返水金額 10000:1
        "dir_deposit": 0, //[直屬]入金點數 10000:1
        "dir_withdraw": 0, //[直屬]出金點數 10000:1
        "dir_deposit_total": 0, //[直屬]入金訂單筆數
        "dir_withdraw_total": 0, //[直屬]出金訂單筆數
        "distinct_bet_total": 0, //[全線]不重複會員投注次數
        "bet_total": 0, //[全線]總投注次數
        "register_agent_total": 0, //[全線]總註冊代理數量
        "agent_total": 2, //[全線]總代理數量
        "register_member_total": 0, //[全線]總註冊會員數量
        "member_total": 10, //[全線]總會員數量
        "bet_amount": 0, //[全線]投注金額 10000:1
        "valid_bet_amount": 0, //[全線]有效投注额 10000:1
        "winloss_amount": 0, //[全線]赢输金额 10000:1
        "rebate_amount": 0, //[全線]返水金額 10000:1
        "deposit": 0, //[全線]入金點數 10000:1
        "withdraw": 0, //[全線]出金點數 10000:1
        "deposit_total": 0, //[全線]入金訂單筆數
        "withdraw_total": 0, //[全線]出金訂單筆數
        "updated_at": "2024-12-06 11:37:15" //最後更新時間
      }
    ]
  }
}
```

### 代理-列表 cmdNo=550

| ParameterName | DataType | Description                   |
| ------------- | -------- | ----------------------------- |
| account       | string   | 帳號                          |
| said          | integral | 上層代理代碼，0 代表總代理    |
| rid           | integral | 角色代碼                      |
| currency      | string   | 幣值代碼                      |
| name          | string   | 名稱                          |
| parentStatus  | integral | 上層狀態 0:停用 1:啟用 9:觀看 |
| status        | integral | 狀態 0:停用 1:啟用 9:觀看     |
| pageNum       | integral | 頁數，0 開始                  |
| pageLimit     | integral | 每頁筆數，預設 10 筆          |

### 代理-新增 cmdNo=551

| ParameterName | DataType            | Description                                  |
| ------------- | ------------------- | -------------------------------------------- |
| account       | string (Required)   | 帳號                                         |
| password      | string (Required)   | 密碼                                         |
| said          | integral (Required) | 上層代理代碼，0 或空值代表新增者為該代理上層 |
| rid           | integral (Required) | 角色代碼                                     |
| currency      | string (Required)   | 幣值代碼，只有 1 級代理需要                  |
| name          | string              | 名稱                                         |

### 代理-修改 cmdNo=552

| ParameterName | DataType           | Description            |
| ------------- | ------------------ | ---------------------- |
| maid          | integral(Required) | 代理代碼               |
| rid           | integral           | 角色代碼               |
| password      | string             | 密碼                   |
| name          | string             | 名稱                   |
| syncSubAgent  | integral           | 狀態同步下線 1:是 0:否 |

### 代理-取得不返水遊戲 cmdNo=553

| ParameterName | DataType | Description |
| ------------- | -------- | ----------- |
| maid          | integral | 代理代碼    |
| gameProvider  | string   | 遊戲商代碼  |

```json
{
    "cmdNo": 553,
    "status": 1,
    "message": "",
    "input": {
        "cmdNo": 553,
        "maid": "8",
        "gameProvider": "sp",
        "pageNum": "0",
        "pageLimit": "10"
    },
    "data": {
        "key": "{"sp_EG-SLOT-A033":1}"
    }
}
```

### 代理-設定不返水遊戲 cmdNo=554

| ParameterName  | DataType            | Description                                     |
| -------------- | ------------------- | ----------------------------------------------- |
| maid           | integral (Required) | 代理代碼 (管理員必填，非管理員直接帶入當下代理) |
| gameProvider   | string (Required)   | 遊戲商代碼                                      |
| liveGame       | json                | 真人遊戲 json 格式 "["BAC","LO"]"               |
| lotteryGame    | json                | 彩票遊戲 json 格式 "["BAC","LO"]"               |
| slotGame       | json                | 電子遊戲 json 格式 "["BAC","LO"]"               |
| sportGame      | json                | 體育遊戲 json 格式 "["BAC","LO"]"               |
| chessGame      | json                | 棋牌遊戲 json 格式 "["BAC","LO"]"               |
| blockchainGame | json                | 區塊鏈遊戲 json 格式 "["BAC","LO"]"             |

至少需填一個遊戲設定
status: 1 為成功

### 會員-列表 cmdNo=560

| ParameterName | DataType            | Description                   |
| ------------- | ------------------- | ----------------------------- |
| account       | string              | 帳號                          |
| said          | integral (Required) | 上層代理代碼                  |
| name          | string              | 名稱                          |
| email         | string              | Email                         |
| countryCodes  | string              | 國碼(手機)                    |
| phoneNumber   | string              | 手機號碼，不含國碼            |
| parentStatus  | integral            | 上層狀態 0:停用 1:啟用 9:觀看 |
| status        | integral            | 狀態 0:停用 1:啟用 9:觀看     |
| pageNum       | integral            | 頁數，0 開始                  |
| pageLimit     | integral            | 每頁筆數，預設 10 筆          |

回覆說明 cmdNo=60

### 會員-新增 cmdNo=561

| ParameterName | DataType            | Description        |
| ------------- | ------------------- | ------------------ |
| account       | string (Required)   | 帳號               |
| password      | string (Required)   | 密碼               |
| said          | integral (Required) | 上層代理代碼       |
| name          | string              | 名稱               |
| email         | string              | Email              |
| countryCodes  | string              | 國碼(手機)         |
| phoneNumber   | string              | 手機號碼，不含國碼 |

### 會員-修改 cmdNo=562

| ParameterName | DataType           | Description       |
|---------------|--------------------|-------------------|
| uuid          | integral(Required) | 會員代碼              |
| password      | string             | 密碼                |
| name          | string             | 名稱                |
| email         | string             | Email             |
| countryCodes  | string             | 國碼(手機)            |
| phoneNumber   | string             | 手機號碼，不含國碼         |
| status        | integral           | 狀態 0:停用 1:啟用 9:觀看 |
| highRisk      | integral           | 是否為高風險用戶 1:是 0:否  |
| ableWithdraw  | integral           | 是否可以出款 1:是 0:否    |
| ableDeposit   | integral           | 是否可以入款 1:是 0:否    |

## 代理端

### 代理-登入 cmdNo=700

| ParameterName | DataType          | Description   |
| ------------- | ----------------- | ------------- |
| account       | string (Required) | 帳號          |
| password      | string (Required) | 密碼          |
| lang          | string            | 語系，預設 en |

### 遊戲列表 cmdNo=701

無需任何參數

```json
{
  "cmdNo": 15,
  "status": 1,
  "message": "",
  "data": {
    "we": [
      //遊戲廠商代碼
      {
        "c": "BAC", //遊戲代碼
        "n": "經典百家樂" //遊戲名稱
      }
    ],
    "sa": [
      {
        "c": "bac",
        "n": "百家樂"
      }
    ]
  }
}
```

### 代理端-全部-查詢操作日誌 cmdNo=721

| ParameterName | DataType | Description                      |
| ------------- | -------- | -------------------------------- |
| cmd           | integral | 指令代碼                         |
| userType      | integral | 對象類別 2:代理(預設) 3:會員     |
| type          | integral | 類別 1:正常日誌(預設) 2:錯誤日誌 |
| maid          | integral | 代理代碼，對象類別:2 使用        |
| said          | integral | 上層代理代碼，對象類別:2,3 使用  |
| uuid          | integral | 會員代碼，對象類別:3 使用        |
| taid          | integral | 對象代碼                         |
| ip            | string   | IP                               |
| stime         | string   | 開始時間(+8 時區)                |
| etime         | string   | 結束時間(+8 時區)                |
| pageNum       | integral | 頁數，0 開始                     |
| pageLimit     | integral | 每頁筆數，預設 10 筆             |

```json
{
  "cmdNo": 721,
  "status": 1,
  "message": "",
  "data": {
    "total": 1628,
    "totalPage": 163,
    "list": [
      {
        "serial": 1,
        "maid": 2, //代理代碼
        "m_name": "23434", //代理名稱
        "m_account": "admin", //代理帳號
        "cmd_no": 100, //指令代碼
        "taid": 0, //對象代碼
        "t_name": null, //對象名稱
        "t_account": null, //對象帳號
        "request": {
          //呼叫API時的參數
          "account": "admin",
          "password": "********",
          "lang": "zh_tw"
        },
        "ip": "192.168.65.1", //呼叫API時IP
        "updated_at": "2024-10-22 01:26:20", //呼叫API時間
        "cmdNoName": "管理端-管理員-登入" //API說明
      }
    ]
  }
}
```

### 代理端-會員-查詢投注記錄 cmdNo=722

| ParameterName | DataType | Description                                               |
| ------------- | -------- | --------------------------------------------------------- |
| said          | integral | 代理代碼                                                  |
| uuid          | integral | 會員代碼                                                  |
| gameProvider  | string   | 遊戲提供商                                                |
| gameCategory  | string   | 遊戲分類代碼 1:真人 2:彩票 3:電子 4:體育 5:棋牌 10:區塊鏈 |
| gameType      | string   | 遊戲類別(代碼)                                            |
| ip            | string   | IP                                                        |
| stime         | string   | 開始時間(+8 時區)                                         |
| etime         | string   | 結束時間(+8 時區)                                         |
| pageNum       | integral | 頁數，0 開始                                              |
| pageLimit     | integral | 每頁筆數，預設 10 筆                                      |

```json
{
  "cmdNo": 722,
  "status": 1,
  "message": "",
  "data": {
    "total": 4,
    "totalPage": 1,
    "list": [
      {
        "serial": 837,
        "uuid": 50, //會員代碼
        "said": 30, //上層代理代碼
        "game_provider": "sp", //遊戲提供商代碼
        "bet_date_time": 1733457195, //投注时间(UNIX)
        "settlement_time": 1733457198, //结算时间(UNIX)
        "table_id": "EG-SLOT-A013", //桌號代碼
        "game_round_id": "14601966108688", //遊戲局號
        "transaction_id": "EG-SLOT-A013-14601966108688-77118974", //遊戲訂單編號(唯一值)
        "bet_amount": 300000, //投注金額 10000:1
        "valid_bet_amount": 300000, //有效投注额 10000:1
        "winloss_amount": -300000, //赢输金额 10000:1
        "game_category": "電子[3]", //遊戲分類名稱[遊戲分類代碼]
        "game_type": "比基尼狂熱[spEG-SLOT-A013]", //遊戲名稱[遊戲代碼]
        "ip": null, //投注時IP位置，不一定會有值
        "created_at": "2024-12-06 11:54:07" //建立記錄時間
      }
    ]
  }
}
```

### 代理端-會員-查詢投注記錄 cmdNo=724

| ParameterName | DataType | Description            |
| ------------- | -------- | ---------------------- |
| maid          | integral | 代理代碼(執行者)       |
| said          | integral | 上層代理代碼           |
| uuid          | integral | 會員代碼               |
| ip            | string   | IP                     |
| wdCode        | integral | 金流類別代碼(下方定義) |
| stime         | string   | 開始時間(+8 時區)      |
| etime         | string   | 結束時間(+8 時區)      |
| pageNum       | integral | 頁數，0 開始           |
| pageLimit     | integral | 每頁筆數，預設 10 筆   |

```json
{
  "cmdNo": 724,
  "status": 1,
  "message": "",
  "data": {
    "total": 262,
    "totalPage": 27,
    "list": [
      {
        "macl_id": 336,
        "uuid": 49, //會員代碼
        "aid": 0, //管理者代碼
        "maid": 0, //代理代碼
        "said": 30, //上層代理代碼
        "transaction_id": "XXXXXX", //交易編號
        "currency": "VND", //幣值
        "wd_code": "FastPay充值[1200]", //金流類別說明[金流類別代碼]
        "wd_type": 1, //增減類別 0:減少 1:增加
        "c_amount": 800000000, //此次異動金額 10000:1
        "b_amount": 1982006018180, //用戶異動前金額 10000:1
        "a_amount": 1982806018180, //用戶異動後金額 10000:1
        "ip": "172.18.0.1", //IP
        "created_at": "2024-12-06 16:19:55" //記錄產生時間
      }
    ]
  }
}
```

### 代理端-代理-拆帳報表 cmdNo=726

| ParameterName | DataType | Description                                                            |
| ------------- | -------- | ---------------------------------------------------------------------- |
| maid          | integral | 0:代表公司端                                                           |
| said          | integral | 上層代理代碼                                                           |
| gameProvider  | string   | 遊戲提供商                                                             |
| gameCategory  | string   | 遊戲分類代碼 1:真人 2:彩票 3:電子 4:體育 5:棋牌 10:區塊鏈              |
| gameType      | string   | 遊戲類別(代碼)                                                         |
| reportType    | integral | 報表類別 1:日 2:週 3:月                                                |
| reportSubType | integral | 報表子類別 1:一般報表(預設) 2:遊戲商報表 3:遊戲分類報表 4:遊戲類別報表 |
| stime         | string   | 開始時間(+8 時區)                                                      |
| etime         | string   | 結束時間(+8 時區)                                                      |
| pageNum       | integral | 頁數，0 開始                                                           |
| pageLimit     | integral | 每頁筆數，預設 10 筆                                                   |

```json
{
  "cmdNo": 726,
  "status": 1,
  "message": "",
  "data": {
    "total": 147,
    "totalPage": 15,
    "list": [
      {
        "report_date": "2024-12-06", //拆帳日期
        "maid": 7, //代理代碼
        "said": 1, //上層代理代碼，0代表總代理
        "game_provider": "sp", //遊戲商代碼 reportSubType=2才有
        "game_category": "電子[3]", //遊戲分類[遊戲分類代碼] reportSubType=3才有
        "game_type": "比基尼狂熱[spEG-SLOT-A013]", //遊戲名稱[遊戲代碼] reportSubType=4才有
        "debt_amount": 0, //拆帳金額(實拿金額) 10000:1
        "paid_amount": 0, //上繳金額 10000:1
        "member_amount": 0, //[直屬]會員上繳金額 10000:1
        "dir_distinct_bet_total": 0, //[直屬]不重複會員投注次數
        "dir_bet_total": 0, //[直屬]總投注次數
        "dir_register_agent_total": 0, //[直屬]總註冊代理數量
        "dir_agent_total": 1, //[直屬]總代理數量
        "dir_register_member_total": 0, //[直屬]總註冊會員數量
        "dir_member_total": 8, //[直屬]總會員數量
        "dir_bet_amount": 0, //[直屬]投注金額 10000:1
        "dir_valid_bet_amount": 0, //[直屬]有效投注额 10000:1
        "dir_winloss_amount": 0, //[直屬]赢输金额，如果是負數代表代理賠錢 10000:1
        "dir_rebate_amount": 0, //[直屬]返水金額 10000:1
        "dir_deposit": 0, //[直屬]入金點數 10000:1
        "dir_withdraw": 0, //[直屬]出金點數 10000:1
        "dir_deposit_total": 0, //[直屬]入金訂單筆數
        "dir_withdraw_total": 0, //[直屬]出金訂單筆數
        "distinct_bet_total": 0, //[全線]不重複會員投注次數
        "bet_total": 0, //[全線]總投注次數
        "register_agent_total": 0, //[全線]總註冊代理數量
        "agent_total": 2, //[全線]總代理數量
        "register_member_total": 0, //[全線]總註冊會員數量
        "member_total": 10, //[全線]總會員數量
        "bet_amount": 0, //[全線]投注金額 10000:1
        "valid_bet_amount": 0, //[全線]有效投注额 10000:1
        "winloss_amount": 0, //[全線]赢输金额 10000:1
        "rebate_amount": 0, //[全線]返水金額 10000:1
        "deposit": 0, //[全線]入金點數 10000:1
        "withdraw": 0, //[全線]出金點數 10000:1
        "deposit_total": 0, //[全線]入金訂單筆數
        "withdraw_total": 0, //[全線]出金訂單筆數
        "updated_at": "2024-12-06 11:37:15" //最後更新時間
      }
    ]
  }
}
```

### 代理端-標籤-列表 cmdNo=740

| ParameterName | DataType | Description  |
|---------------|----------|--------------|
| pageNum       | integral | 頁數，0 開始      |
| pageLimit     | integral | 每頁筆數，預設 10 筆 |

```json
{
  "cmdNo": 740,
  "status": 1,
  "message": "",
  "data": {
    "total": 2,
    "totalPage": 1,
    "list": [
      {
        "tag_id": 1,                        //標籤代碼
        "maid": 30,                         //代理代碼
        "tag_name": "ererer",               //標籤名稱
        "description": "",                  //標籤說明
        "member_total": 0,                  //在此標簽下的會員數量
        "updated_at": "2024-12-11 08:46:08" //最後更新時間
      }
    ]
  }
}
```

### 代理端-標籤-新增 cmdNo=741

| ParameterName | DataType | Description |
|---------------|----------|-------------|
| tagName       | string   | 標籤名稱        |
| description   | string   | 標籤說明        |
```json
{
  "cmdNo": 741,
  "status": 1,
  "message": ""
}
```

### 代理端-標籤-修改 cmdNo=742

| ParameterName | DataType | Description |
|---------------|----------|-------------|
| tagId         | integral | 標籤代碼        |
| tagName       | string   | 標籤名稱        |
| description   | string   | 標籤說明        |
```json
{
  "cmdNo": 742,
  "status": 1,
  "message": ""
}
```

### 代理-列表 cmdNo=750

| ParameterName | DataType | Description         |
|---------------|----------|---------------------|
| account       | string   | 帳號                  |
| said          | integral | 上層代理代碼，0 代表總代理      |
| rid           | integral | 角色代碼                |
| currency      | string   | 幣值代碼                |
| name          | string   | 名稱                  |
| parentStatus  | integral | 上層狀態 0:停用 1:啟用 9:觀看 |
| status        | integral | 狀態 0:停用 1:啟用 9:觀看   |
| getDetail     | integral | 詳細資料 0:否(預設) 1:是    |
| pageNum       | integral | 頁數，0 開始             |
| pageLimit     | integral | 每頁筆數，預設 10 筆        |
```json
{
  cmdNo: 750,
  status: 1,
  message: "",
  data: {
    total: 3,
    totalPage: 1,
    list: [
      {
        maid: 30,             //代理代碼
        said: 1,              //上層代理代碼
        rid: 0,               //角色代碼
        debt_identity: 1,     //用戶型別 1:信用 2:現金
        name: "WE1級代理",     //名稱
        agent_tree: "|1|30|", //代理階層圖
        agent_level: 1,       //代理層級 0:總代 1:一級代理 以此類推
        status: 1,            //狀態 0:停用 1:啟用 9:觀看
        commission_pct: 60,   //代理佔成數 0~100
        currency: "VND",      //幣別 總代不會有幣別
        p_status: 1,          //上層狀態 0:停用 1:啟用 9:觀看
        account: "we1",       //帳號
        email: null,          //Email
        country_codes: null,  //國碼(手機)
        phone: null,          //手機號碼(不含國碼)
        updated_at: "2024-11-29 13:44:40" //最後更新時間
      },
    ],
  }
}
```


### 代理-新增 cmdNo=751

| ParameterName | DataType            | Description             |
|---------------|---------------------|-------------------------|
| account       | string (Required)   | 帳號                      |
| password      | string (Required)   | 密碼                      |
| said          | integral (Required) | 上層代理代碼，0 或空值代表新增者為該代理上層 |
| rid           | integral (Required) | 角色代碼                    |
| currency      | string (Required)   | 幣值代碼，只有 1 級代理需要         |
| name          | string              | 名稱                      |

```json
{
  "cmdNo": 751,
  "status": 1,
  "message": ""
}
```

### 代理-修改 cmdNo=752

| ParameterName | DataType           | Description       |
|---------------|--------------------|-------------------|
| maid          | integral(Required) | 代理代碼              |
| rid           | integral           | 角色代碼              |
| password      | string             | 密碼                |
| status        | integral           | 狀態 0:停用 1:啟用 9:觀看 |
| name          | string             | 名稱                |
| syncSubAgent  | integral           | 狀態同步下線 1:是 0:否    |

```json
{
  "cmdNo": 752,
  "status": 1,
  "message": ""
}
```

### 代理-事件數量通知 cmdNo=753

無需任何參數

```json
{
  "serial": 1,
  "maid": 4,
  "said": 3,
  "agent_tree": "|3|4|",
  "withdraw_review": 22, // 出款待審核數量
  "created_at": "2024-11-19 16:40:05",
  "updated_at": "2024-11-19 16:47:24",
  "infoType": "redis"
}
```

### 代理-遊戲權限設定 cmdNo=754

| ParameterName        | DataType            | Description                              |
| -------------------- | ------------------- | ---------------------------------------- |
| uuid                 | integral (Required) | 會員代碼                                 |
| gameProvider         | string (Required)   | 遊戲商代碼                               |
| gameProviderStatus   | integral (Required) | 遊戲商狀態                               |
| liveStatus           | integral (Required) | 真人狀態                                 |
| lotteryStatus        | integral (Required) | 彩票狀態                                 |
| slotStatus           | integral (Required) | 電子狀態                                 |
| sportStatus          | integral (Required) | 體育狀態                                 |
| chessStatus          | integral (Required) | 棋牌狀態                                 |
| blockchainStatus     | integral (Required) | 區塊鏈狀態                               |
| liveGameStatus       | json                | 真人遊戲狀態 json 格式 {"BAC":0, "DI":0} |
| lotteryGameStatus    | json                | 彩票遊戲狀態 json 格式                   |
| slotGameStatus       | json                | 電子遊戲狀態 json 格式                   |
| sportGameStatus      | json                | 體育遊戲狀態 json 格式                   |
| chessGameStatus      | json                | 棋牌遊戲狀態 json 格式                   |
| blockchainGameStatus | json                | 區塊鏈遊戲狀態 json 格式                 |

狀態不管 0 還是 1 都傳

遊戲狀態定義檔請洽 PM

status: 1 為成功

### 代理-取得遊戲權限 cmdNo=755

| ParameterName               | DataType            | Description                      |
| --------------------------- | ------------------- | -------------------------------- |
| maid                        | integral (Required) | 代理代碼                         |
| uuid                        | integral (Required) | 會員代碼                         |
| gameProvider                | string (Required)   | 遊戲商代碼 (若查無資料則必填)    |
| isGetIndividualGameSettings | integral            | 是否只取得個別遊戲設定 0:否 1:是 |

只能取自己底下代理或會員的遊戲權限

```json
{
  "cmdNo": 755,
  "status": 1,
  "message": "",
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "maid": 0,
        "said": 4,
        "uuid": 6,
        "agent_tree": "|3|4|",
        "game_provider": "we",
        "game_provider_status": 1,
        "live_status": 1,
        "live_game_status": {
          "BAC": 0,
          "DI": 0
        },
        "lottery_status": 1,
        "lottery_game_status": [],
        "slot_status": 1,
        "slot_game_status": [],
        "sport_status": 0,
        "sport_game_status": [],
        "chess_status": 1,
        "chess_game_status": [],
        "blockchain_status": 1,
        "blockchain_game_status": []
      }
    ]
  }
}
```

若沒設定過權限

```json
{
  "cmdNo": 755,
  "status": 1,
  "message": "",
  "data": {
    "total": 0,
    "totalPage": 0,
    "list": {
      "game_provider": "we",
      "game_provider_status": 1,
      "live_status": 1,
      "live_game_status": "",
      "lottery_status": 1,
      "lottery_game_status": "",
      "slot_status": 1,
      "slot_game_status": "",
      "sport_status": 1,
      "sport_game_status": "",
      "chess_status": 1,
      "chess_game_status": "",
      "blockchain_status": 1,
      "blockchain_game_status": ""
    }
  }
}
```

### 代理-取得不返水遊戲 cmdNo=756

| ParameterName | DataType | Description |
| ------------- | -------- | ----------- |
| gameProvider  | string   | 遊戲商代碼  |

```json
{
    "cmdNo": 756,
    "status": 1,
    "message": "",
    "input": {
        "cmdNo": 756,
        "gameProvider": "we",
        "pageNum": "0",
        "pageLimit": "10"
    },
    "data": {
        "key": "{"we_CGM":1}"
    }
}
```

### 代理-設定不返水遊戲 cmdNo=757

| ParameterName  | DataType          | Description                         |
| -------------- | ----------------- | ----------------------------------- |
| gameProvider   | string (Required) | 遊戲商代碼                          |
| liveGame       | json              | 真人遊戲 json 格式 "["BAC","LO"]"   |
| lotteryGame    | json              | 彩票遊戲 json 格式 "["BAC","LO"]"   |
| slotGame       | json              | 電子遊戲 json 格式 "["BAC","LO"]"   |
| sportGame      | json              | 體育遊戲 json 格式 "["BAC","LO"]"   |
| chessGame      | json              | 棋牌遊戲 json 格式 "["BAC","LO"]"   |
| blockchainGame | json              | 區塊鏈遊戲 json 格式 "["BAC","LO"]" |

至少需填一個遊戲設定
status: 1 為成功

### 會員-列表 cmdNo=760

| ParameterName | DataType            | Description         |
|---------------|---------------------|---------------------|
| account       | string              | 帳號                  |
| said          | integral (Required) | 上層代理代碼              |
| name          | string              | 名稱                  |
| email         | string              | Email               |
| countryCodes  | string              | 國碼(手機)              |
| phoneNumber   | string              | 手機號碼，不含國碼           |
| parentStatus  | integral            | 上層狀態 0:停用 1:啟用 9:觀看 |
| status        | integral            | 狀態 0:停用 1:啟用 9:觀看   |
| pageNum       | integral            | 頁數，0 開始             |
| pageLimit     | integral            | 每頁筆數，預設 10 筆        |

回覆說明 cmdNo=60

### 會員-新增 cmdNo=761

| ParameterName | DataType            | Description |
|---------------|---------------------|-------------|
| account       | string (Required)   | 帳號          |
| password      | string (Required)   | 密碼          |
| said          | integral (Required) | 上層代理代碼      |
| name          | string              | 名稱          |
| email         | string              | Email       |
| countryCodes  | string              | 國碼(手機)      |
| phoneNumber   | string              | 手機號碼，不含國碼   |

```json
{
  "cmdNo": 761,
  "status": 1,
  "message": ""
}
```

### 會員-修改 cmdNo=762

| ParameterName | DataType           | Description       |
|---------------|--------------------|-------------------|
| uuid          | integral(Required) | 會員代碼              |
| password      | string             | 密碼                |
| name          | string             | 名稱                |
| email         | string             | Email             |
| countryCodes  | string             | 國碼(手機)            |
| phoneNumber   | string             | 手機號碼，不含國碼         |
| status        | integral           | 狀態 0:停用 1:啟用 9:觀看 |
| highRisk      | integral           | 是否為高風險用戶 1:是 0:否  |
| ableWithdraw  | integral           | 是否可以出款 1:是 0:否    |
| ableDeposit   | integral           | 是否可以入款 1:是 0:否    |

```json
{
  "cmdNo": 762,
  "status": 1,
  "message": ""
}
```

### 會員-金流明細列表 cmdNo=763

| ParameterName | DataType           | Description            |
| ------------- | ------------------ | ---------------------- |
| uuid          | integral(Required) | 會員代碼               |
| pid           | string             | 訂單號                 |
| payment       | string             | 金流商代碼 例：FastPay |
| acton         | string             | 出入金 D:入, W:出      |
| pageNum       | integral           | 頁數，0 開始           |
| pageLimit     | integral           | 每頁筆數，預設 10 筆   |

```json
{
  "cmdNo": 763,
  "status": 1,
  "message": "",
  "input": {
    "cmdNo": 763,
    "uuid": "50",
    "pid": "",
    "payment": "FastPay",
    "action": ""
  },
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "pid": "DPX98964EX674D48DFX8E88E82C",
        "payment": "FastPay",
        "action": "D",
        "merchant": "fast203676",
        "method": "001",
        "amount": 600000000,
        "currency": "VND",
        "extra": "",
        "state": "已下單",
        "uuid": 50,
        "said": 30,
        "agent_tree": "|1|30|",
        "trade_at": 1733118175,
        "created_at": "2024-12-02 13:42:56",
        "updated_at": "2024-12-02 13:42:56",
        "method_name": "Bank transfer"
      }
    ]
  }
}
```

### 會員-銀行卡列表 cmdNo=764

| ParameterName | DataType | Description                               |
| ------------- | -------- | ----------------------------------------- |
| uuid          | string   | 會員代碼                                  |
| bankLangCode  | string   | 銀行代碼 <br/> ex: 002，詳情代碼表請洽 PM |
| bankAccount   | string   | 銀行帳號                                  |
| realName      | string   | 真實姓名                                  |
| currency      | string   | 幣別                                      |
| pageNum       | integral | 頁數，0 開始                              |
| pageLimit     | integral | 每頁筆數，預設 10 筆                      |

```json
{
  "cmdNo": 764,
  "status": 1,
  "message": "",
  "input": {
    "cmdNo": 764,
    "uuid": "",
    "bankLangCode": "",
    "bankAccount": "",
    "realName": "",
    "currency": ""
  },
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "baid": 11,
        "bank_lang_code": "001",
        "bank_account": "test55647888",
        "currency": "VND",
        "real_name": "aaa",
        "alias": "測試用",
        "created_at": "2024-12-02 09:46:48",
        "updated_at": "2024-12-02 09:46:48",
        "bank_name": "VP BANK"
      }
    ]
  }
}
```

### 會員-出款審核 cmdNo=765

| ParameterName | DataType          | Description                    |
| ------------- | ----------------- | ------------------------------ |
| pid           | string (Required) | 訂單號                         |
| state         | string (Required) | 審核狀態 <br/> 1:已審核 2:駁回 |

status: 1 為成功

## 會員端

### 會員-登入 cmdNo=800

| ParameterName | DataType          | Description   |
| ------------- | ----------------- | ------------- |
| account       | string (Required) | 帳號          |
| password      | string (Required) | 密碼          |
| lang          | string            | 語系，預設 en |

```json
{
  "cmdNo": 800,
  "status": 1,
  "message": "",
  "data": {
    "uuid": 50, //會員代碼
    "token": "WzEzLDE3LDM0LDAsOSw2Myw1NywxLDM5LDIsMzMsMj" //Token
  }
}
```

### 會員-修改密碼 cmdNo=801

| ParameterName | DataType          | Description |
| ------------- | ----------------- | ----------- |
| password      | string (Required) | 密碼        |

### 查詢投注記錄 cmdNo=802

| ParameterName | DataType | Description       |
| ------------- | -------- | ----------------- |
| gameProvider  | string   | 遊戲提供商        |
| ip            | string   | IP                |
| stime         | string   | 開始時間(+8 時區) |
| etime         | string   | 結束時間(+8 時區) |

### 查詢會員報表 cmdNo=803

| ParameterName | DataType | Description             |
| ------------- | -------- | ----------------------- |
| reportType    | integral | 報表類別 1:日 2:週 3:月 |
| ip            | string   | IP                      |
| sDate         | string   | 開始時間(+8 時區)       |
| eDate         | string   | 結束時間(+8 時區)       |
| pageNum       | integral | 頁數，0 開始            |
| pageLimit     | integral | 每頁筆數，預設 10 筆    |

### 會員端-查詢餘額異動紀錄 cmdNo=804

| ParameterName | DataType | Description            |
| ------------- | -------- | ---------------------- |
| ip            | string   | IP                     |
| wdCode        | integral | 金流類別代碼(下方定義) |
| stime         | string   | 開始時間(+8 時區)      |
| etime         | string   | 結束時間(+8 時區)      |
| pageNum       | integral | 頁數，0 開始           |
| pageLimit     | integral | 每頁筆數，預設 10 筆   |

```json
{
  "cmdNo": 804,
  "status": 1,
  "message": "",
  "data": {
    "total": 262,
    "totalPage": 27,
    "list": [
      {
        "macl_id": 336,
        "uuid": 49, //會員代碼
        "aid": 0, //管理者代碼
        "maid": 0, //代理代碼
        "said": 30, //上層代理代碼
        "transaction_id": "XXXXXX", //交易編號
        "currency": "VND", //幣值
        "wd_code": "FastPay充值[1200]", //金流類別說明[金流類別代碼]
        "wd_type": 1, //增減類別 0:減少 1:增加
        "c_amount": 800000000, //此次異動金額 10000:1
        "b_amount": 1982006018180, //用戶異動前金額 10000:1
        "a_amount": 1982806018180, //用戶異動後金額 10000:1
        "ip": "172.18.0.1", //IP
        "created_at": "2024-12-06 16:19:55" //記錄產生時間
      }
    ]
  }
}
```

### 會員-跑馬燈列表 cmdNo=806

無需任何參數

目前規則固定會輸出 5 筆，除非滿足條件的筆數不足

```json
{
  "0": {
    "acid": 3,
    "state": "1",
    "identify_title": "測試公告3",
    "announcement_type": "1",
    "announcement_start_date": "2024-12-08 11:00:00",
    "announcement_end_date": "2025-01-24 11:12:00",
    "daily_announcement_start_date": "13:00",
    "daily_announcement_end_date": "19:00",
    "monthly_announcement_start_date": "14 11:00",
    "monthly_announcement_end_date": "17 11:00",
    "marquee_sort": "1",
    "marquee_shows_seconds": 10,
    "time_interval_next_marquee": 3,
    "creator": 2,
    "updater": 2,
    "created_at": "2024-12-06 17:14:18",
    "updated_at": "2024-12-10 13:35:23",
    "data": []
  },
  "1": {
    "acid": 19,
    "state": "1",
    "identify_title": "新增公告fghdfgh",
    "announcement_type": "0",
    "announcement_start_date": "2024-12-10 12:00:00",
    "announcement_end_date": "2024-12-24 11:00:00",
    "daily_announcement_start_date": "",
    "daily_announcement_end_date": "",
    "monthly_announcement_start_date": "",
    "monthly_announcement_end_date": "",
    "marquee_sort": "0",
    "marquee_shows_seconds": 0,
    "time_interval_next_marquee": 0,
    "creator": 2,
    "updater": null,
    "created_at": "2024-12-10 13:44:09",
    "updated_at": "2024-12-10 13:44:09",
    "data": []
  },
  "2": {
    "acid": 1,
    "state": "1",
    "identify_title": "測試公告1",
    "announcement_type": "0",
    "announcement_start_date": "2024-12-10 11:00:00",
    "announcement_end_date": "2024-12-31 11:00:00",
    "daily_announcement_start_date": "",
    "daily_announcement_end_date": "",
    "monthly_announcement_start_date": "",
    "monthly_announcement_end_date": "",
    "marquee_sort": "0",
    "marquee_shows_seconds": 0,
    "time_interval_next_marquee": 0,
    "creator": 2,
    "updater": 2,
    "created_at": "2024-12-06 17:08:24",
    "updated_at": "2024-12-06 17:15:48",
    "data": [
      {
        "serial": 2,
        "acid": 1,
        "language": "en-us",
        "title": "test111",
        "content": "ddddd",
        "creator": 2,
        "updater": null,
        "created_at": "2024-12-06 17:18:52",
        "updated_at": "2024-12-06 17:18:52"
      },
      {
        "serial": 1,
        "acid": 1,
        "language": "zh-tw",
        "title": "測試111",
        "content": "asdfasdfaf",
        "creator": 2,
        "updater": 2,
        "created_at": "2024-12-06 17:17:20",
        "updated_at": "2024-12-06 17:18:20"
      }
    ]
  },
  "3": {
    "acid": 13,
    "state": "1",
    "identify_title": "複製112",
    "announcement_type": "0",
    "announcement_start_date": "2024-12-10 11:00:00",
    "announcement_end_date": "2024-12-24 11:00:00",
    "daily_announcement_start_date": "",
    "daily_announcement_end_date": "",
    "monthly_announcement_start_date": "",
    "monthly_announcement_end_date": "",
    "marquee_sort": "0",
    "marquee_shows_seconds": 0,
    "time_interval_next_marquee": 0,
    "creator": 2,
    "updater": null,
    "created_at": "2024-12-09 16:49:33",
    "updated_at": "2024-12-09 16:49:33",
    "data": [
      {
        "serial": 11,
        "acid": 13,
        "language": "vi-vn",
        "title": "aaa",
        "content": "bbbb",
        "creator": 2,
        "updater": null,
        "created_at": "2024-12-10 10:02:40",
        "updated_at": "2024-12-10 10:02:40"
      }
    ]
  },
  "4": {
    "acid": 9,
    "state": "1",
    "identify_title": "測試公告6",
    "announcement_type": "0",
    "announcement_start_date": "2024-12-09 11:00:00",
    "announcement_end_date": "2025-12-31 12:00:00",
    "daily_announcement_start_date": "",
    "daily_announcement_end_date": "",
    "monthly_announcement_start_date": "10 11:00",
    "monthly_announcement_end_date": "10 19:00",
    "marquee_sort": "0",
    "marquee_shows_seconds": 0,
    "time_interval_next_marquee": 0,
    "creator": 2,
    "updater": null,
    "created_at": "2024-12-09 14:18:51",
    "updated_at": "2024-12-09 14:18:51",
    "data": [
      {
        "serial": 9,
        "acid": 9,
        "language": "en-us",
        "title": "aaa",
        "content": "bbbb",
        "creator": 2,
        "updater": null,
        "created_at": "2024-12-10 10:01:53",
        "updated_at": "2024-12-10 10:01:53"
      },
      {
        "serial": 10,
        "acid": 9,
        "language": "vi-vn",
        "title": "aaa",
        "content": "bbbb",
        "creator": 2,
        "updater": null,
        "created_at": "2024-12-10 10:02:04",
        "updated_at": "2024-12-10 10:02:04"
      }
    ]
  },
  "infoType": "redis"
}
```

### 會員-彈跳視窗列表 cmdNo=807

無需任何參數

目前規則固定會輸出 5 筆，除非滿足條件的筆數不足

```json
{
  "0": {
    "acid": 17,
    "state": "1",
    "identify_title": "新增公告1fadf",
    "announcement_type": "1",
    "announcement_start_date": "2024-12-09 11:00:00",
    "announcement_end_date": "2024-12-24 11:00:00",
    "daily_announcement_start_date": "13:00",
    "daily_announcement_end_date": "19:00",
    "monthly_announcement_start_date": "",
    "monthly_announcement_end_date": "",
    "pop_up_window_sort": "50",
    "time_interval_next_pop_up": 0,
    "creator": 2,
    "updater": null,
    "created_at": "2024-12-10 10:19:19",
    "updated_at": "2024-12-10 10:19:19",
    "data": []
  },
  "1": {
    "acid": 3,
    "state": "1",
    "identify_title": "測試公告3",
    "announcement_type": "1",
    "announcement_start_date": "2024-12-08 11:00:00",
    "announcement_end_date": "2025-01-24 11:12:00",
    "daily_announcement_start_date": "13:00",
    "daily_announcement_end_date": "19:00",
    "monthly_announcement_start_date": "14 11:00",
    "monthly_announcement_end_date": "17 11:00",
    "pop_up_window_sort": "1",
    "time_interval_next_pop_up": 2,
    "creator": 2,
    "updater": 2,
    "created_at": "2024-12-06 17:14:18",
    "updated_at": "2024-12-10 13:35:23",
    "data": []
  },
  "2": {
    "acid": 20,
    "state": "1",
    "identify_title": "ffasdfajsdf",
    "announcement_type": "2",
    "announcement_start_date": "2024-12-10 12:00:00",
    "announcement_end_date": "2024-12-24 11:00:00",
    "daily_announcement_start_date": "",
    "daily_announcement_end_date": "",
    "monthly_announcement_start_date": "9 11:00",
    "monthly_announcement_end_date": "20 11:00",
    "pop_up_window_sort": "7",
    "time_interval_next_pop_up": 0,
    "creator": 2,
    "updater": null,
    "created_at": "2024-12-10 14:09:24",
    "updated_at": "2024-12-10 14:09:24",
    "data": []
  },
  "3": {
    "acid": 16,
    "state": "1",
    "identify_title": "新增公告99",
    "announcement_type": "2",
    "announcement_start_date": "2024-12-09 11:00:00",
    "announcement_end_date": "2024-12-24 11:00:00",
    "daily_announcement_start_date": "",
    "daily_announcement_end_date": "",
    "monthly_announcement_start_date": "8 11:00",
    "monthly_announcement_end_date": "19 11:00",
    "pop_up_window_sort": "2",
    "time_interval_next_pop_up": 0,
    "creator": 2,
    "updater": null,
    "created_at": "2024-12-10 10:18:16",
    "updated_at": "2024-12-10 10:18:16",
    "data": [
      {
        "serial": 13,
        "acid": 16,
        "language": "vi-vn",
        "title": "aaa",
        "content": "bbbb",
        "creator": 2,
        "updater": null,
        "created_at": "2024-12-10 10:18:31",
        "updated_at": "2024-12-10 10:18:31"
      }
    ]
  },
  "4": {
    "acid": 15,
    "state": "1",
    "identify_title": "新增公告888",
    "announcement_type": "0",
    "announcement_start_date": "2024-12-09 11:00:00",
    "announcement_end_date": "2024-12-24 11:00:00",
    "daily_announcement_start_date": "",
    "daily_announcement_end_date": "",
    "monthly_announcement_start_date": "",
    "monthly_announcement_end_date": "",
    "pop_up_window_sort": "1",
    "time_interval_next_pop_up": 0,
    "creator": 2,
    "updater": null,
    "created_at": "2024-12-10 10:17:49",
    "updated_at": "2024-12-10 10:17:49",
    "data": [
      {
        "serial": 12,
        "acid": 15,
        "language": "vi-vn",
        "title": "aaa",
        "content": "bbbb",
        "creator": 2,
        "updater": null,
        "created_at": "2024-12-10 10:18:27",
        "updated_at": "2024-12-10 10:18:27"
      }
    ]
  },
  "infoType": "db"
}
```

### 會員-銀行卡列表 cmdNo=810

| ParameterName | DataType | Description                               |
| ------------- | -------- | ----------------------------------------- |
| bankLangCode  | string   | 銀行代碼 <br/> ex: 002，詳情代碼表請洽 PM |
| bankAccount   | string   | 銀行帳號                                  |
| realName      | string   | 真實姓名                                  |
| currency      | string   | 幣別                                      |
| pageNum       | integral | 頁數，0 開始                              |
| pageLimit     | integral | 每頁筆數，預設 10 筆                      |

```json
{
  "cmdNo": 810,
  "status": 1,
  "message": "",
  "input": {
    "cmdNo": 810,
    "bankLangCode": "",
    "bankAccount": "",
    "realName": "",
    "currency": ""
  },
  "data": {
    "total": 1,
    "totalPage": 1,
    "list": [
      {
        "baid": 11,
        "bank_lang_code": "001",
        "bank_account": "test55647888",
        "currency": "VND",
        "real_name": "aaa",
        "alias": "測試用",
        "created_at": "2024-12-02 09:46:48",
        "updated_at": "2024-12-02 09:46:48",
        "bank_name": "VP BANK"
      }
    ]
  }
}
```

### 會員-新增銀行卡 cmdNo=811

| ParameterName | DataType          | Description                  |
| ------------- | ----------------- | ---------------------------- |
| bankLangCode  | string (Required) | 多語系銀行名稱 <br/> ex: 002 |
| bankAccount   | string (Required) | 銀行帳號                     |
| realName      | string (Required) | 真實姓名                     |
| currency      | string (Required) | 幣別                         |
| alias         | string            | 別名                         |

status: 1 為成功

### 會員-刪除銀行卡 cmdNo=812

| ParameterName | DataType          | Description |
| ------------- | ----------------- | ----------- |
| baid          | string (Required) | 銀行 id     |

status: 1 為成功

### 會員-FastPay 金流入款 cmdNo=850

| ParameterName  | DataType           | Description                    |
| -------------- | ------------------ | ------------------------------ |
| methodLangCode | string (Required)  | 多語系支付代碼                 |
| baid           | string             | 銀行 id （Bank transfer 必填） |
| amount         | integral(Required) | 金額                           |
| notifyUrl      | string (Required)  | 異步回調網址                   |
| extra          | string             | 備註                           |

```json
{
    "cmdNo:" 850,
    "status": 1,
    "message": "",
    "input": {
        "cmdNo": 850,
        "methodLangCode": "001",
        "baid": "8",
        "amount": "600000000",
        "notifyUrl": "https://test.com",
        "extra": ""
    },
    "data": "https://trade.fastpayvnm.com/bank_transfer.html?cc0aa391688f8bb6a2613845a26" // 金流商回傳的支付連結
}
```

### 會員-FastPay 金流入款查詢 cmdNo=851

| ParameterName | DataType          | Description |
| ------------- | ----------------- | ----------- |
| pid           | string (Required) | 訂單號      |

```json
{
  "cmdNo": 851,
  "status": 1,
  "message": "",
  "data": {
    "orderNo": "uvudq1854394883695063041", // 交易編號
    "outTradeNo": "000001-20241107132607-30219", // 訂單號
    "merchantNo": "fast203676", // 商號
    "type": "viettel_qr", // 支付類型
    "code": "", // 支付銀行代碼
    "notifyUrl": null,
    "extra": null,
    "amount": "60000.00", // 金額
    "rechargeAmount": 600000000, // 實付金額，已轉內部格式
    "message": null,
    "currency": null,
    "timestamp": 1730958793,
    "userIP": null,
    "sign": "5d083ddf452ccbe505eecc255bcfbe82",
    "redirect": null,
    "status": 1,
    "username": null,
    "password": null,
    "bankName": null,
    "bankAccount": "0336085435",
    "payName": null,
    "userAmount": 59520,
    "companyNumber": null,
    "companyPayName": null,
    "statusName": "已付款" // 支付狀態
  }
}
```

### 會員-FastPay 金流出款 cmdNo=852

| ParameterName | DataType            | Description  |
| ------------- | ------------------- | ------------ |
| baid          | string (Required)   | 銀行 id      |
| amount        | integral (Required) | 金額         |
| notifyUrl     | string (Required)   | 異步回調網址 |

status: 1 為成功

### 會員-FastPay 金流出款查詢 cmdNo=853

| ParameterName | DataType          | Description |
| ------------- | ----------------- | ----------- |
| pid           | string (Required) | 訂單號      |

```json
{
  "cmdNo": 853,
  "status": 1,
  "message": "",
  "data": {
    "orderNo": "P11141856980871642431488", // 交易編號
    "outTradeNo": "wd_98967f-20241114164154-39741",
    "merchantNo": "fast203676",
    "bankCode": null,
    "bankCardNumber": null,
    "notifyUrl": null,
    "amount": "200000.00",
    "rechargeAmount": null,
    "timestamp": 1731573759,
    "sign": "091b05802c55211e8d496e0fd670d9fc",
    "status": 0,
    "username": null,
    "message": null,
    "payAccountNumber": null,
    "payAccountName": null,
    "payBankCode": null,
    "callToken": null,
    "withdrawQueryUrl": null,
    "statusName": "出款處理中"
  }
}
```

### 會員-金流明細列表 cmdNo=854

| ParameterName | DataType | Description            |
| ------------- | -------- | ---------------------- |
| pid           | string   | 訂單號                 |
| payment       | string   | 金流商代碼 例：FastPay |
| acton         | string   | 出入金 D:入, W:出      |
| pageNum       | integral | 頁數，0 開始           |
| pageLimit     | integral | 每頁筆數，預設 10 筆   |

```json
{
  "cmdNo": 854,
  "status": 1,
  "message": "",
  "input": {
    "cmdNo": 854,
    "pid": "",
    "payment": "FastPay",
    "action": "D"
  },
  "data": {
    "total": 2,
    "totalPage": 1,
    "list": [
      {
        "pid": "DP-98964F-673D3A35-152D95370",
        "payment": "FastPay",
        "action": "D",
        "merchant": "fast203676",
        "method": "001",
        "amount": 600000000,
        "currency": "VND",
        "extra": "",
        "state": "未付款",
        "uuid": 49,
        "said": 30,
        "agent_tree": "|1|30|",
        "trade_at": 1732065845,
        "created_at": "2024-11-20 09:24:05",
        "updated_at": "2024-11-25 10:48:27",
        "method_name": "Bank transfer"
      },
      {
        "pid": "DP-98964F-673D795A-182FC417B",
        "payment": "FastPay",
        "action": "D",
        "merchant": "fast203676",
        "method": "001",
        "amount": 600000000,
        "currency": "VND",
        "extra": "",
        "state": "未付款",
        "uuid": 49,
        "said": 30,
        "agent_tree": "|1|30|",
        "trade_at": 1732082010,
        "created_at": "2024-11-20 13:53:31",
        "updated_at": "2024-11-25 10:48:28",
        "method_name": "Bank transfer"
      }
    ]
  }
}
```

### 會員-登入遊戲 cmdNo=860

| ParameterName  | DataType | Description                 |
| -------------- | -------- | --------------------------- |
| transferPoints | integral | 轉點所有點數到 WE 1:是 0:否 |

```json
{
  "cmdNo": 860,
  "status": 1,
  "message": "",
  "data": {
    //登入網址
    "url": "https://uat-weg-game-client.ufweg.com?lang=en&timezone=UTC-4&token=ugs.vnd_1019iprv.01jchyr8gf5d6t4pk09gt5ng4z"
  }
}
```

### 會員-登出遊戲(轉回所有點數) cmdNo=861

無需任何參數

## 共用

### 讀取系統參數 cmdNo=1000

| ParameterName | DataType          | Description          |
| ------------- | ----------------- | -------------------- |
| type          | string (Required) | 讀取參數類別下方說明 |

`currency`：幣別 <br/>
`lang`:語系<br/>
`gameProvider`:遊戲商代碼<br/>

### 測試排程是否有正常啟用 cmdNo=1001

無需任何參數

## 語系代碼

對照表 <br/>
https://www.science.co.il/language/Locale-codes.php#google_vignette <br/>
公司翻譯文件 <br/>
https://docs.google.com/spreadsheets/d/1ldp7npnMXjC20SYNDnKv7KaAF9dAVweQRwAMuVy5bDg/edit?gid=0#gid=0 <br/>

| Code  | Description |
| ----- | ----------- |
| en-us | 英文        |
| zh-tw | 繁體中文    |
| zh-cn | 簡體中文    |
| vi-vn | 越南        |

## 幣別代碼

對照表 <br/>
https://www.ifcmarkets.tw/about-forex/currencies-and-abbreviations <br/>

| Code | Description |
| ---- | ----------- |
| USD  | 美金        |
| TWD  | 台幣        |
| VND  | 越南盾      |
| CNY  | 人民幣      |
| PHP  | 菲律賓比索  |

## 金流類別定義

去除 `WdCode` 字串取後面數字 <br/>
例：`WdCode1000` ， 真實代碼應為 `1000` <br/>
https://docs.google.com/spreadsheets/d/1ldp7npnMXjC20SYNDnKv7KaAF9dAVweQRwAMuVy5bDg/edit?gid=951569945#gid=951569945 <br/>
