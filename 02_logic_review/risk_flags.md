# AGI40 Logic Pattern Risk Flags

본 문서는 최신 패턴 라이브러리 기반 자동 생성 검수 플래그입니다.

- 실제 PLC 현장 투입 전 XG5000 빌드/컴파일 검수 필요
- ESTOP/안전문/안전릴레이/과부하 접점 방향 확인 필요
- Safety Fault / Process Fault 분리 검수 필요
- Fault Latch는 원인 제거 + Reset 조건 분리 필요
- 모든 항목은 검토용 후보이며 확정 로직이 아닙니다.

## 전체 Risk Flags

- ANALOG_DEVIATION_REVIEW_REQUIRED
- ANALOG_RAMP_RATE_REVIEW_REQUIRED
- ANALOG_SCALE_LIMIT_REVIEW_REQUIRED
- ANALOG_SENSOR_FAULT_PROCESS_FAULT_REVIEW_REQUIRED
- AUTO_MANUAL_MODE_REVIEW_REQUIRED
- FAULT_RESET_CONDITION_REVIEW_REQUIRED
- MAINTENANCE_OVERRIDE_SAFETY_REVIEW_REQUIRED
- MOTOR_FEEDBACK_MISMATCH_PROCESS_FAULT_REVIEW_REQUIRED
- MOTOR_OUTPUT_PERMISSIVE_REVIEW_REQUIRED
- MOTOR_START_STOP_TIMEOUT_REVIEW_REQUIRED
- OUTPUT_FORCE_BLOCK_REVIEW_REQUIRED
- PROCESS_RESET_BLOCKED_BY_SAFETY_REVIEW_REQUIRED
- SAFETY_CONTACT_DIRECTION_REVIEW_REQUIRED
- SAFETY_LATCH_RESET_REQUIRED
- SAFETY_PROCESS_FAULT_SPLIT_REVIEW_REQUIRED
- SAFETY_RESET_PRIORITY_REVIEW_REQUIRED
- SEQUENCE_RESET_CONDITION_REVIEW_REQUIRED
- SEQUENCE_STEP_MANAGER_REVIEW_REQUIRED
- STEP_DONE_FEEDBACK_MAPPING_REVIEW_REQUIRED
- STEP_TIMEOUT_REVIEW_REQUIRED
- VALVE_OPEN_CLOSE_TIMEOUT_REVIEW_REQUIRED
- VALVE_POSITION_CONFLICT_PROCESS_FAULT_REVIEW_REQUIRED

## 패턴별 검수 메모

### MASTER SAFETY CHAIN

- pattern_key: `safety_master_chain`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `50`

검수 메모:
- ESTOP/안전문/안전릴레이 접점 방향 확인 필요
- Safety Fault 자동 해제 금지, Reset 조건 분리 필요

### FAULT LATCH AND MANUAL RESET

- pattern_key: `fault_latch_reset`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `11`

검수 메모:
- Reset 조건은 원인 제거 조건과 함께 검수 필요
- Fault Latch와 Alarm Message 매핑 확인 필요

### SAFETY / PROCESS FAULT SPLIT

- pattern_key: `safety_process_fault_split`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `32`

검수 메모:
- Safety Fault와 Process Fault를 분리해 검수 필요
- Safety Fault는 Process Fault보다 우선 처리 필요
- Process Reset은 Safety Fault 해제 후 허용하는지 검수 필요
- Reset Blocked Reason 표시는 HMI/알람 메시지와 연동 검토 필요

### AUTO / MANUAL MODE GATE

- pattern_key: `auto_manual_mode`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `5`

검수 메모:
- 자동/수동 모드 배타 확인 필요
- 수동 모드 안전 인터락 유지 필요

### MAINTENANCE / OVERRIDE MODE GATE

- pattern_key: `maintenance_override_mode`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `9`

검수 메모:
- Override/Maintenance 모드에서도 Safety Fault 우회 금지
- 유지보수 출력 허용 범위는 설비별 승인 조건 필요
- HMI 표시 및 이력 기록 연동 검토 필요

### SEQUENCE STEP MANAGER

- pattern_key: `sequence_step`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `15`

검수 메모:
- 상태전이는 실제 공정 Step 정의에 맞춰 확장 필요
- STEP_DONE 조건은 실제 완료 피드백으로 치환 필요
- STOP 우선순위와 Reset 복귀 조건 검수 필요
- Fault 상태 전이와 HMI 상태 표시 연동 검토 필요

### MOTOR OUTPUT PERMISSIVE

- pattern_key: `motor_permissive`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `10`

검수 메모:
- 출력 허가 조건에 Safety/Ready/No Fault/Mode Valid 포함 필요
- 수동 운전 시에도 안전 인터락 유지 필요

### MOTOR FEEDBACK MONITORING

- pattern_key: `motor_feedback_monitoring`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `10`

검수 메모:
- 모터 기동/정지 피드백 타임아웃 시간 현장 기준 검수 필요
- 명령/피드백 불일치 Fault를 Process Fault 계열로 연동 필요
- STOP 명령 우선순위 검수 필요

### VALVE OPEN/CLOSE CONTROL

- pattern_key: `valve_open_close`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `11`

검수 메모:
- 밸브 Open/Close 상호배제 조건 확인 필요
- 밸브 Open/Close 피드백 접점 방향 및 타임아웃 기준 검수 필요
- 밸브 Fault는 Process Fault 계열 연동 검토 필요

### ANALOG RANGE / DEVIATION / RAMP CHECK

- pattern_key: `analog_range_check`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `15`

검수 메모:
- AI/AO 스케일링 기준 검수 필요
- 아날로그 하한/상한/목표값 이탈/램프율 급변 기준 확인 필요
- 센서 단선 또는 포화값 판단 기준 확인 필요
- Analog Fault는 Process Fault 계열 연동 검토 필요

### TIMER TIMEOUT FAULT

- pattern_key: `timer_timeout`
- review_required: `True`
- confirmed_for_plc_run: `False`
- variable_count: `4`

검수 메모:
- STEP_DONE 조건을 실제 완료 신호로 치환 필요
- 타임아웃 시간은 공정별 검수 필요

