[2.0.0] 9/7/2023
- Changed interface of Report class
---

[1.0.8] 8/29/2023
- Modified internal logic  
---

[1.0.8] 8/29/2023
- Modified internal logic  
---

[1.0.7] 8/17/2023
- Changed 'estimatedAh' to 'estimatedAhi' in response of Report
---

[1.0.6]	8/9/2023
- If the session status is OPEN, remove callbackUrl of closeSession
---

[1.0.5]	8/7/2023
- If the session status is OPEN, add callbackUrl of closeSession
---

[1.0.4]	8/2/2023
- Fixed bug that onFail() was continuously called when a network error occurred in data upload after startTracking()
- Added exception handling (errorCode: ERR_INVALID_URL) about baseUrl
---

[1.0.3]	8/1/2023
- Fixed bug that userId was not deleted when deleteUser
- Added errorCode 11404 when requesting a deleted userId
---

[1.0.2]	7/24/2023
- getReports() response change from existing List<Report> to List<SleepSession>
---

[1.0.1]	7/18/2023
- Fixed bug in stopSleepTracking()
- Changed default 'limit' of getReports() to 20
---

[1.0.0]	7/3/2023
- Initial Release AsleepSDK.aar
