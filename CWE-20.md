
---

# Vulnerability Report: Full Dialer ver. 1.0.1

## Overview

This report details a  vulnerability within the "Full Dialer" Version 1.0.1 for Android application and pertains to the lack of input validation for received intents.

## Details

### Description

The DialerActivity within the com.full.dialer.top.secure.encrypted package of the "Full Dialer" app is marked as exported. This configuration allows third-party applications without any permissions on the same device to invoke it and initiate phone calls without user interaction.

### Code Reference:

**Manifest**:
```xml
<activity android:theme="@style/Theme.Transparent" android:label="@string/dialer" 
    android:name="com.full.dialer.top.secure.encrypted.activities.DialerActivity" android:exported="true">
    <intent-filter>
        <action android:name="android.intent.action.CALL"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:scheme="tel"/>
    </intent-filter>
</activity>
```

**DialerActivity's onCreate method**:
```java
public final void onCreate(Bundle bundle) {
    super.onCreate(bundle);
    if (a0.e(getIntent().getAction(), "android.intent.action.CALL") && getIntent().getData() != null) {
        this.B = getIntent().getData();
        if (!r.u(this)) {
            H();
        } else {
            P();
        }
    } else {
        r.B(this, R.string.unknown_error_occurred, 0);
        finish();
    }
}
```

![dial](https://github.com/actuator/com.full.dialer.top.secure.encrypted/assets/78701239/a3765442-98a2-4c79-a01b-3430c120f1da)


### CWE:

- CWE-284: Improper Access Control: This weakness occurs when the software does not adequately enforce appropriate authorization on an actor's access requests.
  
- CWE-20: Improper Input Validation: Using unverified or improperly validated input could lead to unintended behaviors within the application.

### Impacts:

1. Unintended Behaviors: A malicious third-party application can craft and send intents to the DialerActivity, possibly triggering unintended dialing operations or other actions within the "Full Dialer" app.
2. Resource Consumption: Malicious intent requests to the DialerActivity could lead to undue resource consumption on the device.

## Recommendations:

1. Restrict exported components: If the DialerActivity should not be accessed by third-party apps and is not part of a public API, set android:exported="false".
2. Implement Input Validation: Add checks to validate the integrity and authenticity of received intent data before processing it.
3. Permission Checks: Implement custom permissions to safeguard exported activities. Only apps granted the proper permissions should be able to initiate the exported activity.

---
