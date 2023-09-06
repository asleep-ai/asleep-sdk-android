[1.0.8(10008)](https://drive.google.com/uc?export=download&id=1w86Y1LG5MJeSbbufRj-Q3ViYmUpYeNFD) 8/29/2023
- Modified internal logic  
---

[1.0.7](https://drive.google.com/uc?export=download&id=1paWZC6R3DiuQcct1oS-562lNr6xThLyu) 8/17/2023
- Changed 'estimatedAh' to 'estimatedAhi' in response of Report
---

[1.0.6](https://drive.google.com/uc?export=download&id=1OLU4O5EyMYpJnSUPkGEsse1wLM8QiOSh)	8/9/2023
- If the session status is OPEN, remove callbackUrl of closeSession
---

[1.0.5](https://drive.google.com/uc?export=download&id=166ePZYVUy9jB-D-_AcgwRP6hj6Iu_slm)	8/7/2023
- If the session status is OPEN, add callbackUrl of closeSession
---

[1.0.4](https://drive.google.com/uc?export=download&id=19nSpCkZKk3UnPRsjnhqBfv7vSzJuC0e7)	8/2/2023
- Fixed bug that onFail() was continuously called when a network error occurred in data upload after startTracking()
- Added exception handling (errorCode: ERR_INVALID_URL) about baseUrl
---

[1.0.3](https://drive.google.com/uc?export=download&id=1r77yjdBIkvPnvnsusUpcTrRvZ4jtU5f_)	8/1/2023
- Fixed bug that userId was not deleted when deleteUser
- Added errorCode 11404 when requesting a deleted userId
---

[1.0.2](https://drive.google.com/uc?export=download&id=1YVOibtf6TWFI8IaCuroMckU4YUetAU-1)	7/24/2023
- getReports() response change from existing List<Report> to List<SleepSession>
---

[1.0.1](https://drive.google.com/uc?export=download&id=1bbmAFVxi-JrL4FNP1FtBi_FAGamAQdNT)	7/18/2023
- Fixed bug in stopSleepTracking()
- Changed default 'limit' of getReports() to 20
---

[1.0.0](https://drive.google.com/uc?export=download&id=1MWo2sCCdRL5oo8Mbbtypscj1Ek-DdsDB)	7/3/2023
- Initial Release AsleepSDK.aar
