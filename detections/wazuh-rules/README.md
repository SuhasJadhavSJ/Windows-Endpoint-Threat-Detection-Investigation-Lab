# Wazuh Rules

This directory contains custom Wazuh detection rules developed after the underlying Windows/Sysmon behavior has been investigated.

## Rule Development Process

```text
Observed Behavior
      |
      v
Raw Telemetry
      |
      v
Investigation
      |
      v
Detection Logic
      |
      v
Wazuh Rule
      |
      v
Alert Validation
      |
      v
Tuning
```

Rules should be added only after their underlying behavior and evidence have been understood.

## Status

- [ ] First custom rule
- [ ] Rule validation
- [ ] False-positive testing
- [ ] Rule tuning
