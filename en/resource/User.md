# User Management



Tuya Cloud supports multiple user systems: mobile phone, email, and UID. The mobile phone supports two methods of verification code login and password login. UID login is mainly used when you already have your own account system. Specific content can refer to the document [user management](https://tuyainc.github.io/tuyasmart_home_android_sdk_doc/en/resource/User.html).



## Mini SDK solution

If you use the **Mini SDK**, the accessor already has its own account system, and the cloud development of the accessor needs to map its own account system to Tuya's account system. Each account registers a UID in Tuya Cloud, and the app uses this. UID logs in to Tuya Cloud to get Tuya device management permissions. For details, please refer to the document [**Mini SDK**-User Management](https://docs.tuya.com/en/iot/open-api/api-list/api/user-management).