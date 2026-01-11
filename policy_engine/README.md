Adaptive Aadhaar authentication decision rules that select
the safest authentication method based on BRS.
The adaptive authentication policy engine selects the safest
authentication method based on Biometric Reliability Score (BRS).

Decision Logic:
- Low BRS: QR / OTP / Attribute-only authentication
- Medium BRS: Biometric + OTP
- High BRS: Biometric authentication

This approach reduces authentication failures
and minimizes unnecessary biometric usage.
