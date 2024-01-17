
| Column    | Type     | Defalut     | Method                                         |
| --------- | -------- | ----------- | ---------------------------------------------- |
| UID | Int | Primary Key | token + Cookie + Base64 Encode |
| Id | Varchar  | | Default ID = UID, Changeable, Validation Check |
| Email | Varchar  | Not Null    | Changeable, Validation Check                   |
| Password  | Varchar | Not Null | Bcrypt(Hash + Salt) |
| JoinDate | Date | Now() | |
| Name | Varchar | | |
| LastUpate | Date | Now() | Password LastUpdate Check                      |
| ==Profile== | | | Profile Image, DB Or LocalStorage |
> User Table
---

| Column | Type    | Defalut     | Method               |
| ------ | ------- | ----------- | ---------------------------------------------- |
| UID    | Int     | Foreign Key | FK : User|
| Age    | Int     |             |                      |
| Gender | Varchar |             |                      |
| Height | float   |             |                      |
| Weight | float   |             |                      |
| SKM    |         |             | Skaletal Muscle Mess |
| BFM    |         |             | Body Fat Mess        |
| ProgramName | Foreign Key |  | FK : Program |
> Profile
---

| Column      | Type    | Defalut     | Method |
| ----------- | ------- | ----------- | ------ |
| ProgramName | Varchar | Primary Key |        |
| text        |         |             |        |
> Program Table
> 	ex > 1행 : 5x5 : text)
---

| Column   | Type    | Defalut | Method |
| -------- | ------- | ------- | ------ |
| UID      | Varchar |         |        |
| 시작일   |         |         |        |
| 최초무게 |         |         |        |
| 등등 잡다하고 필요한 컬럼들 |         |         |        |
> 5x5 Table






==DB 구현 여부 미정==