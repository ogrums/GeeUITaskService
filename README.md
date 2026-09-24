# GeeUITaskService

System app that decides what the robot does with a command. The old README says "Control Robot". The process that does the work is `DispatchService`, not the empty launcher activity.

## Package

- Foreground service action: `android.intent.action.LTPCMD`
- Application class `TaskServiceApplication` only starts xlog
- `MainActivity` is a launcher stub

Submodules: `GeeUIBase` and `GeeUIComponets`. `settings.gradle` spells the components path `GeeUIComponents`.

## What it does

`DispatchService.onCreate` binds `com.renhejia.robot.letianpaiservice` (`android.intent.action.LETIANPAI`) and registers callbacks for long-connect, speech, app command, sensor, robot status, Xiaomi, identify, and BLE.

It then drives mode, gestures, speech, app commands, BLE, Mi IoT, identity, battery, temperature, sleep, the status-bar overlay (`FloatingViewService`), alarms, and notices. The manifest comment "添加浮窗" is that overlay. A marked block is the temperature check.

Outbound AIDL from this class: `setAppCmd`, `setRobotStatusCmd`, `setMcuCommand`, `setIdentifyCmd`, `setExpression`. Servo power and sensor switches are posted to `ControlSteeringEngineHandler` (`FOOT_POWER`, `FOOT_SENSOR`).

One comment says a block looks unused and should be deleted later ("此代码查看应该是没有用到，后期删除"). Another says the hardware code is fetched here ("获取硬件码").

## Cloud calls

Local `GeeUiNetManager`: `getDeviceInfo`, `getTimeStamp`, and unused helpers for calendar, weather, and the full config. The shared `com.letianpai.robot.components.network.nets.GeeUiNetManager` is used for `getAllConfig`, `getReChargeConfig`, and enter or exit auto-charging uploads.

`GeeUINetworkConsts.java` in this repo is fully commented. Its strings are the placeholder `"your interface url"`.

It also queries `com.letianpai.emqxservice`, `com.letianpai.robot.audioservice`, `com.letianpai.robot.mcuservice`, `com.renhejia.robot.launcher`, and `com.geeui.face`.

## Comment glossary

| Where | Chinese | English |
|---|---|---|
| `AndroidManifest` | 添加浮窗 | Add a floating window |
| `AndroidManifest` | 温度校验 | Temperature check |
| `DispatchService` | 获取硬件码 | Fetch the hardware code |
| `DispatchService` | 乐天派 ControlService 完成AIDLService服务 | ControlService finished binding AIDL |
| `DispatchService` | 乐天派 ControlService 无法绑定aidlserver的AIDLService服务 | Could not bind the AIDL server |
| `DispatchService` | 此代码查看应该是没有用到，后期删除 | Looks unused; delete later |
| `GeeUiNetManager` | 获取日历列表 / 获取天气信息 / 获取闹钟列表 | Calendar / weather / alarm lists |
| `GeeUiNetManager` | 获取机器人全部配置 / 更新机器人状态 | Full robot config / update robot status |
