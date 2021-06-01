# JWTAuthentication.WebApi
Security is a vital part of any kind of application. Since APIs can expose highly sensitive data like user details, email adressses , it is highly critical that these API endpoints are secured.In this Guide let's build a Secure ASP.NET Core API with JWT Authentication. Read my detailed blog post for implementation.

mssql database 접속 가능한 연결자를 appsettings.json 수정합니다. 저는 미리 데이터베이스를 생성하고, database 를 접근 가능한 사용자의 아이디와 
패스워드를 추가했습니다. 그리고 나서 아래 패키지 관리자 콘솔을 통해 실행하였으며, 생성된 데이터베이스 안에 자동적으로 테이블이 생성됩니다.

PM> Remove-Migration
Build started...
Build succeeded.
Removing model snapshot.
Done.

PM> Add-Migration CreateIdentitySchema
Build started...
Build succeeded.
To undo this action, use Remove-Migration.

PM> Update-Database
Build started...
Build succeeded.
Done.
PM> 



1> 토큰 가져오기

https://localhost:44346/api/user/token

body 에 row 로 선택한 다음, json 형태를 선택하고 아래 구문을 입력합니다.

{
  "email": "user@secureapi.com",
  "password": "Pa$$w0rd."
}

결과값

{
    "message": null,
    "isAuthenticated": true,
    "userName": "luckshim",
    "email": "luckshim@univ.me",
    "roles": [
        "Administrator",
        "User"
    ],
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJsdWNrc2hpbSIsImp0aSI6IjAwOWRkMmMzLWVhY2MtNDNmMS1hODNjLWNkZGI5NzA2NWUyZSIsImVtYWlsIjoibHVja3NoaW1AdW5pdi5tZSIsInVpZCI6Ijg2MDVkZjFjLTBhNGUtNGJlYS1iOGJhLWYxODFiM2I4YzU1MSIsInJvbGVzIjpbIkFkbWluaXN0cmF0b3IiLCJVc2VyIl0sImV4cCI6MTYyMjUyMzUyNSwiaXNzIjoiU2VjdXJlQXBpIiwiYXVkIjoiU2VjdXJlQXBpVXNlciJ9.7lW72KlmpRWZBRqOrwu36HhEueLeAoofOkAlz1HjRbE",
    "refreshTokenExpiration": "2021-06-08T04:57:45.5841355Z"
}


2> 사용자 생성

https://localhost:44346/api/User/register

post 로 지정하고, Authorizaiton 에서 type 을 Bearer Token 을 선택한 다음, 위의 token 값을 넣습니다.

그 다음 body 에서 row 로 선택한 다음, json 형태를 선택하고 아래 구문을 입력합니다.

{
    "FirstName" : "shim",
    "LastName" : "jaewoon",
    "Username" : "luckshim",
    "Email" : "luckshim@univ.me",
    "Password" : "Password1!"
}

3> 사용자 권한 추가

설명은 2번과 동일하게 설정한 다음, 경로와 json 값만 바꾸고 실행하면 됩니다.

https://localhost:44346/api/User/addrole

{
    "Email" : "luckshim@univ.me",
    "Password" : "Eogkrsodlf1!",
    "Role" : "Administrator"
}

4> refresh token 으로 신규 token 재발급하기

https://localhost:44346/api/user/refresh-token

post 로 지정하고, body 값은 없으므로 none 을 선택합니다.

이전에 token 을 생성했다면 그 시점에 refresh token 값이 readonly 로 cookie 값에 자동 셋팅되어 있습니다.
그래서 body 에 값을 전송하지 않고 백엔드에서 쿠키값을 읽어서 refresh token 값을 읽어서 판단하여 갱신된 token 값을
제공해 줍니다.


{
    "message": null,
    "isAuthenticated": true,
    "userName": "user",
    "email": "user@secureapi.com",
    "roles": [
        "User"
    ],
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJ1c2VyIiwianRpIjoiMjllYjYwYWUtZTZmNC00ZDk1LTgyNDktZGM3ZmIxYmI2OWNiIiwiZW1haWwiOiJ1c2VyQHNlY3VyZWFwaS5jb20iLCJ1aWQiOiIwMjZjMmEzNC1lNjgyLTRhZGQtODBlNC01NzZiZWZkN2QxNTMiLCJyb2xlcyI6IlVzZXIiLCJleHAiOjE2MjI1MjM0NDAsImlzcyI6IlNlY3VyZUFwaSIsImF1ZCI6IlNlY3VyZUFwaVVzZXIifQ.KNRVyiibkaXVL4n0m_MyTtXBU0DmmbqdKG6Nx0Xxz-4",
    "refreshTokenExpiration": "2021-06-08T04:56:20.5332852Z"
}









