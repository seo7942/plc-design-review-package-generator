# AGI40 XGT Logic Pattern Draft

본 문서는 자동 생성된 검토용 Logic Draft입니다.

- 실제 PLC 현장 투입 전 XG5000 빌드/컴파일 검수 필요
- ESTOP/안전문/안전릴레이/과부하 접점 방향 확인 필요
- 출력 명령은 Safety/Ready/No Fault/Mode 조건 이후 허용
- Fault Latch는 원인 제거 + Reset 조건 분리 필요

---


### MASTER SAFETY CHAIN

**N001 - Master Safety Chain 후보**

```ladder
|--[ ESTOP_OK(X000) AND T1_PUMP_1_OVERLOAD_TRIP(X020) AND T1_PUMP_2_OVERLOAD_TRIP(X027) AND T1_PUMP_3_OVERLOAD_TRIP(X034) AND T2_PUMP_1_OVERLOAD_TRIP(X041) AND T2_PUMP_2_OVERLOAD_TRIP(X048) AND HMI_RESUME_CONFIRM_PB(X003) AND POWER_RECOVERY_DETECTED(X006) AND MCC_MAIN_POWER_OK(X008) AND UPS_POWER_OK(X009) AND T1_LEVEL_LOW_SW(X010) AND T1_LEVEL_HIGH_HIGH_SW(X013) AND T2_LEVEL_LOW_SW(X014) AND T2_LEVEL_HIGH_HIGH_SW(X017) AND T1_PUMP_1_FAULT_FB(X019) AND T1_PUMP_1_MCC_CB_OK(X021) AND T1_PUMP_1_MAINT_KEY_ON(X024) AND T1_PUMP_2_FAULT_FB(X026) AND T1_PUMP_2_MCC_CB_OK(X028) AND T1_PUMP_2_MAINT_KEY_ON(X031) AND T1_PUMP_3_FAULT_FB(X033) AND T1_PUMP_3_MCC_CB_OK(X035) AND T1_PUMP_3_MAINT_KEY_ON(X038) AND T2_PUMP_1_FAULT_FB(X040) AND T2_PUMP_1_MCC_CB_OK(X042) AND T2_PUMP_1_MAINT_KEY_ON(X045) AND T2_PUMP_2_FAULT_FB(X047) AND T2_PUMP_2_MCC_CB_OK(X049) AND T2_PUMP_2_MAINT_KEY_ON(X052) AND T1_DRAIN_VALVE_FAULT_FB(X055) AND T1_ISOLATION_VALVE_FAULT_FB(X058) AND T1_BYPASS_VALVE_FAULT_FB(X061) AND T2_TRANSFER_VALVE_FAULT_FB(X064) AND T2_ISOLATION_VALVE_FAULT_FB(X067) AND WWTP_DISCHARGE_VALVE_FAULT_FB(X070) AND LEAK_T1_AREA_DETECTED(X071) AND LEAK_T2_AREA_DETECTED(X072) AND LEAK_T1_TRENCH_DETECTED(X073) AND LEAK_T2_TRENCH_DETECTED(X074) AND LEAK_MCC_ROOM_DETECTED(X075) AND LEAK_VALVE_PIT_DETECTED(X076) AND T1_PUMP_1_FLOW_OK(X077) AND T1_PUMP_2_FLOW_OK(X078) AND T1_PUMP_3_FLOW_OK(X079) AND T2_PUMP_1_FLOW_OK(X080) AND T2_PUMP_2_FLOW_OK(X081) AND T1_LEVEL_TRANSMITTER_FAULT(X082) AND T2_LEVEL_TRANSMITTER_FAULT(X083) ]--[/ M_AGI40_SAFETY_FAULT_LATCH ]----------------( M_AGI40_MASTER_SAFETY_OK )
```

**N002 - Safety Fault Latch 후보**

```ladder
|--[/ ESTOP_OK_ANY ]-----------------------------------( SET M_AGI40_SAFETY_FAULT_LATCH )
|--[/ DOOR_CLOSED_ANY ]--------------------------------( SET M_AGI40_SAFETY_FAULT_LATCH )
|--[/ SAFETY_RELAY_OK ]--------------------------------( SET M_AGI40_SAFETY_FAULT_LATCH )
|--[ OVERLOAD_ANY ]------------------------------------( SET M_AGI40_SAFETY_FAULT_LATCH )
```

검수 포인트:
- ESTOP/안전문/안전릴레이/과부하 실제 접점 방향(NO/NC)을 XG5000에서 확인해야 한다.
- 안전 관련 신호는 출력 명령보다 우선한다.
- Safety Fault는 자동 복구가 아니라 원인 제거 + 수동 Reset 검토가 필요하다.


### FAULT LATCH AND MANUAL RESET

**N010 - Fault Latch 후보**

```ladder
|--[ ALARM_RESET_PB(X004) AND ALARM_ACK_PB(X005) AND T1_PUMP_1_FAULT_FB(X019) AND T1_PUMP_1_OVERLOAD_TRIP(X020) AND T1_PUMP_2_FAULT_FB(X026) AND T1_PUMP_2_OVERLOAD_TRIP(X027) AND T1_PUMP_3_FAULT_FB(X033) AND T1_PUMP_3_OVERLOAD_TRIP(X034) ]------------------------------------( SET M_AGI40_FAULT_LATCH )
```

**N011 - Manual Reset 후보**

```ladder
|--[ ALARM_RESET_PB(X004) ]--[/ FAULT_CONDITION_ANY ]-----------( M_AGI40_RESET_OK )
|--[ M_AGI40_RESET_OK ]--------------------------------------( RST M_AGI40_FAULT_LATCH )
```

검수 포인트:
- Reset은 원인 제거 후에만 허용해야 한다.
- Fault 조건과 Alarm 표시 조건을 분리 검토해야 한다.


### SAFETY / PROCESS FAULT SPLIT

**N012 - Safety Fault Any 후보**

```ladder
|--[ ESTOP_OK(X000) AND T1_PUMP_1_OVERLOAD_TRIP(X020) AND T1_PUMP_2_OVERLOAD_TRIP(X027) AND T1_PUMP_3_OVERLOAD_TRIP(X034) AND T2_PUMP_1_OVERLOAD_TRIP(X041) AND T2_PUMP_2_OVERLOAD_TRIP(X048) AND HMI_RESUME_CONFIRM_PB(X003) AND POWER_RECOVERY_DETECTED(X006) AND MCC_MAIN_POWER_OK(X008) AND UPS_POWER_OK(X009) AND T1_LEVEL_LOW_SW(X010) AND T1_LEVEL_HIGH_HIGH_SW(X013) AND T2_LEVEL_LOW_SW(X014) AND T2_LEVEL_HIGH_HIGH_SW(X017) AND T1_PUMP_1_FAULT_FB(X019) AND T1_PUMP_1_MCC_CB_OK(X021) AND T1_PUMP_1_MAINT_KEY_ON(X024) AND T1_PUMP_2_FAULT_FB(X026) AND T1_PUMP_2_MCC_CB_OK(X028) AND T1_PUMP_2_MAINT_KEY_ON(X031) AND T1_PUMP_3_FAULT_FB(X033) AND T1_PUMP_3_MCC_CB_OK(X035) AND T1_PUMP_3_MAINT_KEY_ON(X038) AND T2_PUMP_1_FAULT_FB(X040) AND T2_PUMP_1_MCC_CB_OK(X042) AND T2_PUMP_1_MAINT_KEY_ON(X045) AND T2_PUMP_2_FAULT_FB(X047) AND T2_PUMP_2_MCC_CB_OK(X049) AND T2_PUMP_2_MAINT_KEY_ON(X052) AND T1_DRAIN_VALVE_FAULT_FB(X055) AND T1_ISOLATION_VALVE_FAULT_FB(X058) AND T1_BYPASS_VALVE_FAULT_FB(X061) AND T2_TRANSFER_VALVE_FAULT_FB(X064) AND T2_ISOLATION_VALVE_FAULT_FB(X067) AND WWTP_DISCHARGE_VALVE_FAULT_FB(X070) AND LEAK_T1_AREA_DETECTED(X071) AND LEAK_T2_AREA_DETECTED(X072) AND LEAK_T1_TRENCH_DETECTED(X073) AND LEAK_T2_TRENCH_DETECTED(X074) AND LEAK_MCC_ROOM_DETECTED(X075) AND LEAK_VALVE_PIT_DETECTED(X076) AND T1_PUMP_1_FLOW_OK(X077) AND T1_PUMP_2_FLOW_OK(X078) AND T1_PUMP_3_FLOW_OK(X079) AND T2_PUMP_1_FLOW_OK(X080) AND T2_PUMP_2_FLOW_OK(X081) AND T1_LEVEL_TRANSMITTER_FAULT(X082) AND T2_LEVEL_TRANSMITTER_FAULT(X083) ]---------------------------------( SET M_AGI40_SAFETY_FAULT_LATCH )
```

**N013 - Process Fault Any 후보**

```ladder
|--[ ALARM_RESET_PB(X004) AND ALARM_ACK_PB(X005) AND T1_LEVEL_MID_SW(X011) AND T1_LEVEL_HIGH_SW(X012) AND T2_LEVEL_MID_SW(X015) AND T2_LEVEL_HIGH_SW(X016) AND T1_PUMP_1_RUN_FB(X018) AND T1_PUMP_2_RUN_FB(X025) AND T1_PUMP_3_RUN_FB(X032) AND T2_PUMP_1_RUN_FB(X039) AND T2_PUMP_2_RUN_FB(X046) AND T1_DRAIN_VALVE_OPEN_FB(X053) ]--------------------------------( SET M_AGI40_PROCESS_FAULT_LATCH )
```

**N014 - Safety Reset OK 후보**

```ladder
|--[ ALARM_RESET_PB(X004) ]--[/ SAFETY_FAULT_ANY ]--------------( M_AGI40_SAFETY_RESET_OK )
|--[ M_AGI40_SAFETY_RESET_OK ]-------------------------------( RST M_AGI40_SAFETY_FAULT_LATCH )
```

**N015 - Process Reset OK 후보**

```ladder
|--[ ALARM_RESET_PB(X004) ]--[/ PROCESS_FAULT_ANY ]--[/ M_AGI40_SAFETY_FAULT_LATCH ]----( M_AGI40_PROCESS_RESET_OK )
|--[ M_AGI40_PROCESS_RESET_OK ]-----------------------------------------------( RST M_AGI40_PROCESS_FAULT_LATCH )
```

**N016 - Reset Blocked Reason 후보**

```ladder
|--[ ALARM_RESET_PB(X004) ]--[ M_AGI40_SAFETY_FAULT_LATCH ]-----------( M_AGI40_RESET_BLOCKED_REASON_SAFETY_ACTIVE )
|--[ ALARM_RESET_PB(X004) ]--[ PROCESS_FAULT_ANY ]--------------( M_AGI40_RESET_BLOCKED_REASON_PROCESS_ACTIVE )
```

검수 포인트:
- Safety Fault는 Process Fault보다 우선한다.
- Safety Fault 해제는 실제 원인 제거 + 현장 Reset 조건 확인 후 허용한다.
- Process Fault Reset은 Safety Fault가 해제된 상태에서만 허용한다.


### AUTO / MANUAL MODE GATE

**N070 - Auto/Manual Mutual Exclusion 후보**

```ladder
|--[ PANEL_AUTO_SEL(X001) ]--[ PANEL_MANUAL_SEL(X002) ]----( MODE_CONFLICT_ALARM )
```

**N071 - Manual Permissive 후보**

```ladder
|--[ PANEL_MANUAL_SEL(X002) ]--[ M_AGI40_MASTER_SAFETY_OK ]----( M_AGI40_MANUAL_PERMISSIVE )
```

검수 포인트:
- 자동/수동 모드 동시 ON 방지.
- 수동 모드에서도 Safety OK 조건은 유지한다.


### MAINTENANCE / OVERRIDE MODE GATE

**N072 - Maintenance Active 후보**

```ladder
|--[ T1_PUMP_1_MAINT_KEY_ON(X024) ]-------------------------------------( M_AGI40_MAINTENANCE_ACTIVE )
```

**N073 - Override Active / Permissive 후보**

```ladder
|--[ T1_BYPASS_VALVE_OPEN_FB(X059) ]----------------------------------( M_AGI40_OVERRIDE_ACTIVE )
|--[ M_AGI40_OVERRIDE_ACTIVE ]--[ M_AGI40_MASTER_SAFETY_OK ]--[/ M_AGI40_SAFETY_FAULT_LATCH ]----( M_AGI40_OVERRIDE_PERMISSIVE )
```

**N074 - Output Force Block 후보**

```ladder
|--[ M_AGI40_MAINTENANCE_ACTIVE ]--[/ M_AGI40_OVERRIDE_PERMISSIVE ]-----------------( M_AGI40_OUTPUT_FORCE_BLOCK )
|--[ M_AGI40_SAFETY_FAULT_LATCH ]---------------------------------------------( M_AGI40_OUTPUT_FORCE_BLOCK )
```

검수 포인트:
- Override/Maintenance 모드에서도 Safety Fault는 절대 우회하지 않는다.
- Maintenance 상태의 출력 강제 허용 범위는 설비별 승인 조건이 필요하다.
- 유지보수 모드는 HMI 표시와 이력 기록 연동이 필요하다.


### SEQUENCE STEP MANAGER

**N080 - IDLE → READY 후보**

```ladder
|--[ M_AGI40_STEP_IDLE ]--[ M_AGI40_MASTER_SAFETY_OK ]--[ READY_OK ]--[/ M_AGI40_SAFETY_FAULT_LATCH ]----( SET M_AGI40_STEP_READY )
|--[ M_AGI40_STEP_READY ]----------------------------------------------------------( RST M_AGI40_STEP_IDLE )
```

**N081 - READY → RUN 후보**

```ladder
|--[ M_AGI40_STEP_READY ]--[ START_PB ]--[/ M_AGI40_PROCESS_FAULT_LATCH ]-------------( SET M_AGI40_STEP_RUN )
|--[ M_AGI40_STEP_RUN ]----------------------------------------------------------------( RST M_AGI40_STEP_READY )
```

**N082 - RUN → COMPLETE 후보**

```ladder
|--[ M_AGI40_STEP_RUN ]--[ M_AGI40_STEP_DONE ]----------------------------------------------------( SET M_AGI40_STEP_COMPLETE )
|--[ M_AGI40_STEP_COMPLETE ]-----------------------------------------------------------( RST M_AGI40_STEP_RUN )
```

**N083 - RUN Timeout 후보**

```ladder
|--[ M_AGI40_STEP_RUN ]--[/ M_AGI40_STEP_DONE ]---------------------------------------------------( TON T_STEP_TIMEOUT, PT=10s )
|--[ T_STEP_TIMEOUT.Q ]-------------------------------------------------------( SET M_AGI40_STEP_TIMEOUT_FAULT )
```

**N084 - FAULT 전이 후보**

```ladder
|--[ M_AGI40_SAFETY_FAULT_LATCH ]-------------------------------------------------( SET M_AGI40_STEP_FAULT )
|--[ M_AGI40_PROCESS_FAULT_LATCH ]------------------------------------------------( SET M_AGI40_STEP_FAULT )
|--[ M_AGI40_STEP_TIMEOUT_FAULT ]-------------------------------------------------( SET M_AGI40_STEP_FAULT )
|--[ M_AGI40_STEP_FAULT ]--------------------------------------------------------------( RST M_AGI40_STEP_RUN )
```

**N085 - STOP / RESET 복귀 후보**

```ladder
|--[ STOP_PB ]-----------------------------------------------------------( RST M_AGI40_STEP_RUN )
|--[ ALARM_RESET_PB(X004) ]--[/ M_AGI40_SAFETY_FAULT_LATCH ]--[/ M_AGI40_PROCESS_FAULT_LATCH ]----( SET M_AGI40_STEP_IDLE )
```

검수 포인트:
- 실제 공정 완료 조건은 M_AGI40_STEP_DONE 후보를 실제 완료 피드백으로 치환해야 한다.
- Fault 상태에서 Reset 복귀 조건은 Safety/Process Fault 해제 후 허용해야 한다.
- STOP은 RUN보다 우선 처리한다.


### MOTOR OUTPUT PERMISSIVE

**N020 - Auto/Manual Mode Valid 후보**

```ladder
|--[ PANEL_AUTO_SEL(X001) ]--[/ PANEL_MANUAL_SEL(X002) ]----( M_AGI40_MODE_VALID )
|--[ PANEL_MANUAL_SEL(X002) ]--[/ PANEL_AUTO_SEL(X001) ]----( M_AGI40_MODE_VALID )
```

**N021 - Motor/Fan/Pump Output Permissive 후보**

```ladder
|--[ M_AGI40_MASTER_SAFETY_OK ]--[ READY_OK ]--[/ M_AGI40_FAULT_LATCH ]--[/ M_AGI40_PROCESS_FAULT_LATCH ]--[ M_AGI40_MODE_VALID ]----( MOTOR_OUTPUT_PERMISSIVE )
```

대상 출력 후보:
- T1_PUMP_1_RUN_CMD(Y000), T1_PUMP_2_RUN_CMD(Y001), T1_PUMP_3_RUN_CMD(Y002), T2_PUMP_1_RUN_CMD(Y003), T2_PUMP_2_RUN_CMD(Y004)

검수 포인트:
- 모터/팬/펌프 출력은 Safety OK, Ready, No Fault, Mode Valid 조건을 모두 거친 뒤 허용한다.
- 수동 모드에서도 안전 인터락은 우회하면 안 된다.


### MOTOR FEEDBACK MONITORING

**N030 - Motor Start Feedback Timeout 후보**

```ladder
|--[ T1_PUMP_1_RUN_CMD(Y000) ]--[/ T1_PUMP_1_RUN_FB(X018) ]-------------------------------( TON T_MOTOR_START_TIMEOUT, PT=3s )
|--[ T_MOTOR_START_TIMEOUT.Q ]------------------------------( SET M_AGI40_MOTOR_START_TIMEOUT_FAULT )
```

**N031 - Motor Stop Feedback Timeout 후보**

```ladder
|--[/ T1_PUMP_1_RUN_CMD(Y000) ]--[ T1_PUMP_1_RUN_FB(X018) ]-------------------------------( TON T_MOTOR_STOP_TIMEOUT, PT=3s )
|--[ T_MOTOR_STOP_TIMEOUT.Q ]-------------------------------( SET M_AGI40_MOTOR_STOP_TIMEOUT_FAULT )
```

**N032 - Command/Feedback Mismatch 통합 Fault 후보**

```ladder
|--[ M_AGI40_MOTOR_START_TIMEOUT_FAULT ]---------------------------( SET M_AGI40_MOTOR_FEEDBACK_FAULT )
|--[ M_AGI40_MOTOR_STOP_TIMEOUT_FAULT ]----------------------------( SET M_AGI40_MOTOR_FEEDBACK_FAULT )
|--[ M_AGI40_MOTOR_FEEDBACK_FAULT ]--------------------------------( SET M_AGI40_MOTOR_FAULT )
|--[ M_AGI40_MOTOR_FAULT ]-----------------------------------( SET M_AGI40_PROCESS_FAULT_LATCH )
```

**N033 - Stop Command 우선 후보**

```ladder
|--[ STOP_PB ]--------------------------------------( RST MOTOR_OUTPUT_PERMISSIVE )
```

검수 포인트:
- 실제 장비별 기동/정지 시간에 따라 PT 값을 조정해야 한다.
- 출력 명령과 피드백 불일치 시 Process Fault 계열로 래치해야 한다.
- STOP 명령은 운전 허가보다 우선한다.


### VALVE OPEN/CLOSE CONTROL

**N040 - Valve Open/Close Mutual Exclusion 후보**

```ladder
|--[ T1_DRAIN_VALVE_OPEN_CMD(Y005) ]--[ T1_DRAIN_VALVE_CLOSE_CMD(Y006) ]-------------( SET M_AGI40_VALVE_FAULT )
```

**N041 - Valve Open Feedback Timeout 후보**

```ladder
|--[ T1_DRAIN_VALVE_OPEN_CMD(Y005) ]--[/ T1_DRAIN_VALVE_OPEN_FB(X053) ]--------------( TON T_VALVE_OPEN_TIMEOUT, PT=5s )
|--[ T_VALVE_OPEN_TIMEOUT.Q ]----------------------------------( SET M_AGI40_VALVE_OPEN_TIMEOUT_FAULT )
```

**N042 - Valve Close Feedback Timeout 후보**

```ladder
|--[ T1_DRAIN_VALVE_CLOSE_CMD(Y006) ]--[/ T1_DRAIN_VALVE_CLOSE_FB(X054) ]------------( TON T_VALVE_CLOSE_TIMEOUT, PT=5s )
|--[ T_VALVE_CLOSE_TIMEOUT.Q ]---------------------------------( SET M_AGI40_VALVE_CLOSE_TIMEOUT_FAULT )
```

**N043 - Valve Position Conflict 후보**

```ladder
|--[ T1_DRAIN_VALVE_OPEN_FB(X053) ]--[ T1_DRAIN_VALVE_CLOSE_FB(X054) ]---------------( SET M_AGI40_VALVE_POSITION_CONFLICT_FAULT )
```

**N044 - Valve Fault 통합 후보**

```ladder
|--[ M_AGI40_VALVE_OPEN_TIMEOUT_FAULT ]----------------------------( SET M_AGI40_VALVE_FAULT )
|--[ M_AGI40_VALVE_CLOSE_TIMEOUT_FAULT ]---------------------------( SET M_AGI40_VALVE_FAULT )
|--[ M_AGI40_VALVE_POSITION_CONFLICT_FAULT ]-----------------------( SET M_AGI40_VALVE_FAULT )
|--[ M_AGI40_VALVE_FAULT ]-----------------------------------( SET M_AGI40_PROCESS_FAULT_LATCH )
```

검수 포인트:
- Open/Close 동시 출력 방지.
- Open/Close 피드백 동시 ON은 결선 또는 센서 이상 후보.
- Open/Close 타임아웃은 설비별 동작 시간 기준으로 조정해야 한다.


### ANALOG RANGE / DEVIATION / RAMP CHECK

**N050 - Analog Low/High Range Alarm 후보**

```ladder
|--[ T1_LEVEL_AI(D100) < D_ANALOG_LOW_LIMIT ]----------------------( SET M_AGI40_ANALOG_RANGE_ALARM )
|--[ T1_LEVEL_AI(D100) > D_ANALOG_HIGH_LIMIT ]---------------------( SET M_AGI40_ANALOG_RANGE_ALARM )
```

**N051 - Analog Deviation Alarm 후보**

```ladder
|--[ ABS(T1_LEVEL_AI(D100) - D_ANALOG_TARGET_VALUE) > D_ANALOG_DEVIATION_LIMIT ]----( SET M_AGI40_ANALOG_DEVIATION_ALARM )
```

**N052 - Analog Ramp Rate Alarm 후보**

```ladder
|--[ ABS(T1_LEVEL_AI(D100) - D_ANALOG_PREVIOUS_VALUE) > D_ANALOG_RAMP_RATE_LIMIT ]--( SET M_AGI40_ANALOG_RAMP_RATE_ALARM )
```

**N053 - Analog Sensor Fault 후보**

```ladder
|--[ T1_LEVEL_AI(D100) <= D_ANALOG_RAW_MIN ]-----------------------( SET M_AGI40_ANALOG_SENSOR_FAULT )
|--[ T1_LEVEL_AI(D100) >= D_ANALOG_RAW_MAX ]-----------------------( SET M_AGI40_ANALOG_SENSOR_FAULT )
```

**N054 - Analog Fault 통합 후보**

```ladder
|--[ M_AGI40_ANALOG_RANGE_ALARM ]-----------------------------------( SET M_AGI40_ANALOG_FAULT )
|--[ M_AGI40_ANALOG_DEVIATION_ALARM ]-------------------------------( SET M_AGI40_ANALOG_FAULT )
|--[ M_AGI40_ANALOG_RAMP_RATE_ALARM ]------------------------------------( SET M_AGI40_ANALOG_FAULT )
|--[ M_AGI40_ANALOG_SENSOR_FAULT ]----------------------------------( SET M_AGI40_ANALOG_FAULT )
|--[ M_AGI40_ANALOG_FAULT ]----------------------------------( SET M_AGI40_PROCESS_FAULT_LATCH )
```

검수 포인트:
- AI/AO 스케일링 원시값/공학값 변환 기준 확인 필요.
- 센서 단선/포화값/상한/하한/목표값 이탈/급변 기준은 설비별로 확정해야 한다.
- Analog Fault는 Process Fault 계열로 연동 검토한다.


### TIMER TIMEOUT FAULT

**N060 - Step Timeout Fault 후보**

```ladder
|--[ M_AGI40_STEP_ACTIVE ]--[/ STEP_DONE ]-------------------( TON T_STEP_TIMEOUT, PT=10s )
|--[ T_STEP_TIMEOUT.Q ]---------------------------------------( SET M_AGI40_STEP_TIMEOUT_FAULT )
```

검수 포인트:
- STEP_DONE 조건은 실제 공정 완료 신호 또는 피드백으로 치환해야 한다.
- PT=10s는 후보값이며 공정별로 확정해야 한다.

