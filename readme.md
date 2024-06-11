# Digital Banking with Microservices API Documentation

## Introduction
This API is the result of the first version of migrating an existing banking system from a monolithic architecture to a microservice architecture. It does not include all features; it mainly focuses on the non-functional aspects of banking and also integrates with the existing system.

# Table of Contents
- [Digital Banking with Microservices API Documentation](#digital-banking-with-microservices-api-documentation)
  - [Introduction](#introduction)
- [Services](#services)
  - [API Gateway Endpoints](#api-gateway-endpoints)
    - [Auth](#auth)
      - [Login](#login)
      - [Refresh Token](#refresh-token)
      - [Retrieve Current Authentication Info](#retrieve-current-authentication-info)
    - [Users](#users)
      - [List Users](#list-users)
      - [Create a User](#create-a-user)
      - [Retrieve a User](#retrieve-a-user)
      - [Enable/Disable a User](#enabledisable-a-user)
      - [Update a User](#update-a-user)
      - [Delete a User](#delete-a-user)
    - [Roles](#roles)
      - [List Roles](#list-roles)
      - [Create a Role](#create-a-role)
      - [Retrieve a Role](#retrieve-a-role)
      - [Update a Role](#update-a-role)
      - [Delete a Role](#delete-a-role)
    - [Permissions](#permissions)
      - [List Application Permissions](#list-application-permissions)
  - [Client Service Endpoints](#client-service-endpoints)
    - [Clients](#clients)
      - [List Clients](#list-clients)
      - [Create a Client](#create-a-client)
      - [Activate/Reject/Close a Client](#activaterejectclose-a-client)
      - [Assign a User to Client](#assign-a-user-to-client)
      - [Update a Client](#update-a-client)
      - [Delete a Client](#delete-a-client)
  - [Account Service Endpoints](#account-service-endpoints)
    - [Saving Accounts](#saving-accounts)
      - [List Saving Accounts](#list-saving-accounts)
      - [Retrieve a Saving Account](#retrieve-a-saving-account)
      - [Create a Saving Account](#create-a-saving-account)
      - [Update a Saving Account](#update-a-saving-account)
      - [Approve/Activate/Reject/Close a Saving Account](#approveactivaterejectclose-a-saving-account)
    - [Transactions](#transactions)
      - [List Saving Account Transactions](#list-saving-account-transactions)
      - [Deposit Balance](#deposit-balance)
      - [Withdraw Balance](#withdraw-balance)
      - [Generate Account Statement](#generate-account-statement)
  - [Fund Transfer Service Endpoints](#fund-transfer-service-endpoints)
    - [Fund Transfer](#fund-transfer)
      - [Internal Transfer](#internal-transfer)

## Services
The system mainly consists of 4 microservices to handle and support the banking operations:

### API Gateway Endpoints
#### Auth
- ##### Login
  #### Mandatory fields
  `username`, `password`
  #### Method example
  `POST /auth/login`
  #### Method request
  ```
  POST /auth/login
  Content-type: application/json
  Request body:

  {
    "username":"root",
    "password":"123456"
  }
  ```
  #### Method response
  ```
  {
    "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiIyIiwic3ViIjoicm9vdDIiLCJpYXQiOjE3MTYxMDEwNDcsImV4cCI6MTcxNjE4NzQ0NywiaXNBY2Nlc3NUb2tlbiI6dHJ1ZX0.ZdxXrteBEjGmLLwPErg4aK0R8_hGwvAoqd0KqaGoclo",
    "token_type": "Bearer",
    "refresh_token": "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiIyIiwic3ViIjoicm9vdDIiLCJpYXQiOjE3MTYxMDEwNDcsImV4cCI6MTcxNjE2MTA0NywiaXNBY2Nlc3NUb2tlbiI6ZmFsc2V9.cL34eX4k0mKnXvd-jYDsonrN84DvypdXHj92y5iK7EQ",
    "expire_in": 86400
  }
  ```
- ##### Refresh Token
  #### Mandatory fields
  `token`
  #### Method example
  `POST /auth/refresh?token={tokenValue}`
  #### Method request
  ```
  POST /auth/refresh?token=eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiIyIiwic3ViIjoicm9vdDIiLCJpYXQiOjE3MTYxMDEwNDcsImV4cCI6MTcxNjE2MTA0NywiaXNBY2Nlc3NUb2tlbiI6ZmFsc2V9.cL34eX4k0mKnXvd-jYDsonrN84DvypdXHj92y5iK7EQ
  Content-Type: application/json
  No Request Body
  ```
  #### Method response
  ```
  {
    "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiIyIiwic3ViIjoicm9vdDIiLCJpYXQiOjE3MTYxMDEwNDcsImV4cCI6MTcxNjE4NzQ0NywiaXNBY2Nlc3NUb2tlbiI6dHJ1ZX0.ZdxXrteBEjGmLLwPErg4aK0R8_hGwvAoqd0KqaGoclo",
    "token_type": "Bearer",
    "refresh_token": "eyJhbGciOiJIUzI1NiJ9.eyJqdGkiOiIyIiwic3ViIjoicm9vdDIiLCJpYXQiOjE3MTYxMDEwNDcsImV4cCI6MTcxNjE2MTA0NywiaXNBY2Nlc3NUb2tlbiI6ZmFsc2V9.cL34eX4k0mKnXvd-jYDsonrN84DvypdXHj92y5iK7EQ",
    "expire_in": 86400
  }
  ```
- ##### Retrieve Current Authentication Info
  #### Method example
  `/auth/me`
  #### Method response
  ```
  {
    "id": 2,
    "username": "root",
    "firstName": "root",
    "lastName": "user",
    "email": "root@rootroot.com",
    "isEnabled": true,
    "isSystemUser": false,
    "mobile": "1234567890",
    "roles": [
        {
            "id": 1,
            "name": "SuperUser",
            "description": "Super User Role"
        }
    ],
    "permissions": [
        {
            "id": 1,
            "name": "ALL",
            "displayName": "Super user lmao",
            "description": "Allows do everything"
        }
    ],
    "createdBy": 1,
    "updatedBy": 1,
    "createdAt": "2024-05-15T17:14:47.637712",
    "updatedAt": "2024-05-15T17:14:47.637823",
    "deleted": false
  }
  ```
#### Users
- ##### List Users
  ##### Optional arguments
  `page` - integer optional, defaults to 0.  
  - Indicates the result from which pagination starts.

  `size` - integer optional, defaults to 10.  
  - Limits the size of results returned. To override the default and return all entries you must explicitly pass a non-positive integer value for limit e.g. size=0, or size=1  
  
  `orderBy` - string optional  
  - one of id. Orders results by the indicated field.  
  
  `sortDirection` - string optional, one of ASC, DESC.  
  - Indicates what way to order results if orderBy is used.  
  ##### Mehtod example 
  `
  GET /users
  `  
  `  
  GET /users?page=1&size=10$orderBy=id&sortDirection=DESC  
  `
  ##### Mehtod response 
  ```
  {
    "size": 10,
    "page": 0,
    "totalItems": 2,
    "totalPages": 1,
    "data": [
        {
            "id": 2,
            "username": "root2",
            "firstName": "John",
            "lastName": "Doe",
            "email": "john.doe@example.com",
            "isEnabled": true,
            "isSystemUser": false,
            "mobile": "1234567890",
            "roles": [
                {
                    "id": 1,
                    "name": "SuperUser",
                    "description": "Super User Role"
                }
            ],
            "permissions": [
                {
                    "id": 1,
                    "name": "ALL",
                    "displayName": "Super user lmao",
                    "description": "Allows do everything",
                    "httpMethod": null
                }
            ],
            "createdBy": 1,
            "updatedBy": 1,
            "createdAt": "2024-05-15T17:14:47.637712",
            "updatedAt": "2024-05-15T17:14:47.637823",
            "deleted": false
        },
        {
            "id": 1,
            "username": "root",
            "firstName": "Super",
            "lastName": "User",
            "email": "user1@example.com",
            "isEnabled": true,
            "isSystemUser": false,
            "mobile": "123456789",
            "roles": [
                {
                    "id": 1,
                    "name": "SuperUser",
                    "description": "Super User Role"
                }
            ],
            "permissions": [
                {
                    "id": 1,
                    "name": "ALL",
                    "displayName": "Super user lmao",
                    "description": "Allows do everything",
                    "httpMethod": null
                }
            ],
            "createdBy": null,
            "updatedBy": null,
            "createdAt": "2024-04-03T00:00:00",
            "updatedAt": "2024-04-03T00:00:00",
            "deleted": false
        }
    ]
  }
  ```

- ##### Create a User
    Adds new application user.
  #### Mandatory fields
  `username`, `firstname`, `lastname`, `email`, `roles`
  #### Method example
  `POST /users`
  #### Method Request
  ```
  POST /users
  Content-Type: application/json
  Request body:

  {
    "username": "newuser",
    "firstname": "Test",
    "lastname": "User",
    "email": "whatever@domain-name.org",
    "roles": [1],
    "password": "password",
  }
  ```
  #### Method Reponse 
  ```
  {
      "id": 3
  }
  ```

- ##### Retrieve a User
  ##### Method example
  `/users/{userId}`
  ##### Method reponse 
  ```
  {
      "id": 3,
      "username": "User1234",
      "firstName": "User",
      "lastName": "User",
      "email": "user.doe@example.com",
      "isEnabled": true,
      "isSystemUser": false,
      "mobile": "1234567890",
      "roles": [
          {
              "id": 1,
              "name": "SuperUser",
              "description": "Super User Role"
          }
      ],
      "permissions": [
          {
              "id": 1,
              "name": "ALL",
              "displayName": "Super user lmao",
              "description": "Allows do everything",
              "httpMethod": null
          }
      ],
      "createdBy": 2,
      "updatedBy": 2,
      "createdAt": "2024-05-19T13:55:35.075702",
      "updatedAt": "2024-05-19T13:55:35.075879",
      "deleted": false
  }
  ```
- ##### Enable/Disable a User
  ##### Method example
  `POST /users/{userId}?command=enable`  
  `POST /users/{userId}?command=disable` 
  ##### Method request 
  ```
  POST /users/3?command=enable
  Content-Type: application/json
  No Request Body

  POST /users/3?command=disable
  Content-Type: application/json
  No Request Body
  ``` 
- ##### Update a User
  ##### Method example
  `PUT /users/{userId}`
  ##### Method request
  ```
  PUT /users/3
  Content-Type: application/json
  Request body:

  {
    "firstName": "suepr",
    "roleIds": [2]
  }
  ```
  ##### Method reponse
  ```
  {
      "id": 3
  }
  ```
- ##### Delete a User
  ##### Method example
  `DELETE /users/{userId}`
  ##### Method request 
  ```
  DELETE /users/3
  Content-Type: application/json
  No Request Body
  ```
  ##### Method reponse
  ```
  {
      "id": 3
  }
  ```
#### Roles
An API capability to support management of application roles for user administration.
- ##### List Roles
  ##### Mehtod Example 
  `
  GET /roles
  `  
  ##### Method response 
  ```
  [
    {
        "id": 4,
        "name": "Test role1234",
        "permissions": [
            {
                "id": 1,
                "name": "ALL",
                "displayName": "Super user lmao",
                "description": "Allows do everything"
            },
            {
                "id": 2,
                "name": "User Deletion",
                "displayName": "Delete any user",
                "description": "allow to delete any user",
                "httpMethod": "DELETE"
            }
        ]
    },
    {
        "id": 1,
        "name": "SuperUser",
        "description": "Super User Role",
        "permissions": [
            {
                "id": 1,
                "name": "ALL",
                "displayName": "Super user lmao",
                "description": "Allows do everything"
            }
        ]
    },
    {
        "id": 2,
        "name": "Super User1 1",
        "permissions": []
    },
    {
        "id": 3,
        "name": "test rol",
        "permissions": [
            {
                "id": 1,
                "name": "ALL",
                "displayName": "Super user lmao",
                "description": "Allows do everything"
            },
            {
                "id": 2,
                "name": "User Deletion",
                "displayName": "Delete any user",
                "description": "allow to delete any user",
                "httpMethod": "DELETE"
            }
        ]
    }
  ]
  ```

- ##### Create a Role
  ##### Mandatory fields
  `name`, `permissionIds`

  ##### Mehtod example
  `POST /roles`
  #### Mehtod request 
  ```
  POST /roles
  Content-Type: application/json
  Request body:
  
  {
    "name":"test rol",
    "permissions":[1,2]
  }
  ```
  ##### Mehtod reponse 
  ```
  {
    "id":2
  }
  ```
- ##### Retrieve a Role
  #### Method example
  `/roles/{roleId}`
  #### Method response
  ```
  {
    "id": 3,
    "name": "test rol",
    "permissions": [
        {
            "id": 2,
            "name": "User Deletion",
            "displayName": "Delete any user",
            "description": "allow to delete any user",
            "httpMethod": "DELETE"
        },
        {
            "id": 1,
            "name": "ALL",
            "displayName": "Super user lmao",
            "description": "Allows do everything"
        }
    ]
  }
  ```
  - ##### Update a Role
    #### Method example
    `PUT /roles/{roleId}`
    #### Method request
    ```
    PUT /roles/3
    Content-Type: application/json
    Request body:

    {
      "name":"Test role1234",
     "permissionIds":[2]
    }
    ```
    #### Method response
    ```
    {
      "id":3
    }
    ```

- ##### Delete a Role
#### Method example
`DELETE /roles/{roleId}`
#### Method request
```
  DELETE /roles/3?command=enable
  Content-Type: application/json
  No Request Body
```
#### Method response
```
{
  "id":3
}
```
#### Permissions
- ##### List Application Permissions
#### Method example
`GET /permissions`
#### Method response 
```
[
    {
        "id": 3,
        "name": "User Update info",
        "displayName": "update any user",
        "description": "allow to update any user",
        "httpMethod": "PUT"
    },
    {
        "id": 1,
        "name": "ALL",
        "displayName": "Super user lmao",
        "description": "Allows do everything"
    },
    {
        "id": 2,
        "name": "User Deletion",
        "displayName": "Delete any user",
        "description": "allow to delete any user",
        "httpMethod": "DELETE"
    }
]
```
### Client Service Endpoints
An API capable the management of client
#### Clients
- #### List Clients
  #### Optional arguments
  `page` - integer optional, defaults to 0.  
  - Indicates the result from which pagination starts.

  `size` - integer optional, defaults to 10.  
  - Limits the size of results returned. To override the default and return all entries you must explicitly pass a non-positive integer value for limit e.g. size=0, or size=1  
  
  `orderBy` - string optional  
  - one of id. Orders results by the indicated field.  
  
  `sortDirection` - string optional, one of ASC, DESC.  
  - Indicates what way to order results if orderBy is used.  
  #### Method example
  `GET /clients`  
  `GET /clients?page=1&size=10$orderBy=id&sortDirection=DESC`  
  #### Method response
  ```
  {
    "size": 10,
    "page": 0,
    "totalItems": 2,
    "totalPages": 1,
    "data": [
        {
            "id": 20,
            "accountNo": "000000020",
            "firstName": "Lord",
            "lastName": "Megatron",
            "emailAddress": "moopsy.bone@eater.com",
            "mobileCountryCode": "+112",
            "dateOfBirth": "1990-01-10",
            "submittedOnDate": "2024-03-21",
            "officeId": 1,
            "officeName": "Head Office",
            "legalForm": {
                "id": 100,
                "value": "Person",
                "code": "LegalFormType.person",
                "position": 0
            },
            "status": {
                "id": 202,
                "value": "Pending",
                "code": "ClientStatusType.pending",
                "position": 0
            },
            "gender": {
                "id": 1,
                "value": "Male",
                "code": "GenderType.male",
                "position": 0
            },
            "createdBy": 2,
            "createdAt": "2024-05-19T15:28:10.809789",
            "updatedBy": 2,
            "updatedAt": "2024-05-19T15:28:10.907699"
        },
        {
            "id": 19,
            "accountNo": "000000019",
            "firstName": "Lord",
            "lastName": "Megatron",
            "emailAddress": "client.bone@wild.com",
            "mobileCountryCode": "+112",
            "dateOfBirth": "1990-01-10",
            "submittedOnDate": "2024-05-19",
            "officeId": 1,
            "officeName": "Head Office",
            "legalForm": {
                "id": 100,
                "value": "Person",
                "code": "LegalFormType.person",
                "position": 0
            },
            "status": {
                "id": 202,
                "value": "Pending",
                "code": "ClientStatusType.pending",
                "position": 0
            },
            "gender": {
                "id": 1,
                "value": "Male",
                "code": "GenderType.male",
                "position": 0
            },
            "createdBy": 2,
            "createdAt": "2024-05-19T15:24:14.745032",
            "updatedBy": 2,
            "updatedAt": "2024-05-19T15:24:14.872257"
        }
      ]
    }  


  ```
- ##### Create a Client
  #### Mandatory fields
  `firstName`, `lastName`, `officeId`, `legalformId`, `genderId`
  #### Method example
  `POST /clients`
  #### Method request
  ```
  POST /clients
  Content-Type: application/json
  Request body:
  
  {
    "officeId": 1,
    "dateOfBirth": "1990-01-10",
    "displayName": "Random Client",
    "emailAddress": "client.testing@wild.com",
    "firstName": "Lord",
    "lastName": "Megatron",
    "legalFormId": 100,
    "genderId": 1
    }
  ```
  #### Method response
  ```
  {
    "id":10
  }
  ```
- ##### Activate/Reject/Close a Client
  #### Method example
  `POST /clients/{clientId}?command=activate`  
  `POST /clients/{clientId}?command=close`  
  `POST /clients/{clientId}?command=reject`  
  #### Method response
  ```  
  {
    "id":10
  }
  ```
- ##### Assign a User to Client
  #### Method example
  `POST /clients/clientauthorizations/{clientId}`
  #### Method request
  ```
  POST /clients/clientauthorizations/10
  Content-Type: application/json
  Request body:

  {
    "authorizations":[1,2]
  }
  ```
  #### Method response
  ```
  {
    "id":10
  }
  ```
- ##### Update a Client
  #### Method example
  `PUT /clients/{clientId}`
  #### Method request
  ```
  PUT /clients/10
  Content-Type: application/json
  Request body:
  
  {
    "lastName": "Optimus"
  }
  ```
  #### Method response
  ```
  {
    "id":10
  }
  ```
- ##### Delete a Client
  #### Method example
  `DELETE /client/{clientId}
  #### Method request
  ```
  DELETE /clients/10
  Content-Type: application/json
  No Request Body
  ```
  #### Method response
  ```
  {
    "id":10
  }
  ```
### Account Service Endpoints
An API capable the management of account
#### Saving Accounts
- #### List Saving Accounts
  ##### Optional arguments
  `page` - integer optional, defaults to 0.  
  - Indicates the result from which pagination starts.

  `size` - integer optional, defaults to 10.  
  - Limits the size of results returned. To override the default and return all entries you must explicitly pass a non-positive integer value for limit e.g. size=0, or size=1  
  
  `sortDirection` - string optional, one of ASC, DESC.  
  - Indicates what way to order results if orderBy is used.  
  #### Method example
  `GET /savingsaccounts` 
  `GET /savingsaccounts?page=0&size=10&sordDirection=DESC` 
  #### Method response
  ```
  {
    "size": 10,
    "page": 0,
    "totalItems": 3,
    "totalPages": 1,
    "data": [
        {
            "id": 20,
            "parentId": 0,
            "productId": 1,
            "nickname": "Master Kai",
            "lockinPeriodFrequency": 0,
            "allowOverdraft": false,
            "overdraftLimit": 0.00,
            "minOverdraftForInterestCalculation": 0.00,
            "enforceMinRequiredBalance": false,
            "minRequiredBalance": 0.00,
            "lockinPeriodFrequencyType": 0,
            "status": {
                "id": 200,
                "value": "Approved",
                "code": "savingsAccountStatusType.approved"
            },
            "subStatus": {
                "id": 0,
                "value": "SavingsAccountSubStatus.none",
                "code": "None"
            },
            "accountBalance": 0.00,
            "accountType": {
                "id": 0,
                "value": "AccountType.saving",
                "code": "Saving"
            },
            "currency": {
                "currencyCode": "USD",
                "currencyDigit": 2,
                "inMultiplesOf": 1
            },
            "approveDate": "2024-05-19",
            "clientId": 20
        },
        {
            "id": 19,
            "parentId": 0,
            "productId": 1,
            "nickname": "Master Kai",
            "lockinPeriodFrequency": 0,
            "allowOverdraft": false,
            "overdraftLimit": 0.00,
            "minOverdraftForInterestCalculation": 0.00,
            "enforceMinRequiredBalance": false,
            "minRequiredBalance": 0.00,
            "lockinPeriodFrequencyType": 0,
            "status": {
                "id": 100,
                "value": "Submitted and pending approval",
                "code": "savingsAccountStatusType.submitted.and.pending.approval"
            },
            "subStatus": {
                "id": 0,
                "value": "SavingsAccountSubStatus.none",
                "code": "None"
            },
            "accountBalance": 0.00,
            "accountType": {
                "id": 0,
                "value": "AccountType.saving",
                "code": "Saving"
            },
            "currency": {
                "currencyCode": "USD",
                "currencyDigit": 2,
                "inMultiplesOf": 1
            },
            "clientId": 20
        },
        {
            "id": 18,
            "parentId": 0,
            "productId": 1,
            "nickname": "Master Ougway",
            "lockinPeriodFrequency": 0,
            "allowOverdraft": false,
            "overdraftLimit": 0.00,
            "minOverdraftForInterestCalculation": 0.00,
            "enforceMinRequiredBalance": false,
            "minRequiredBalance": 0.00,
            "lockinPeriodFrequencyType": 0,
            "status": {
                "id": 100,
                "value": "Submitted and pending approval",
                "code": "savingsAccountStatusType.submitted.and.pending.approval"
            },
            "subStatus": {
                "id": 0,
                "value": "SavingsAccountSubStatus.none",
                "code": "None"
            },
            "accountBalance": 0.00,
            "accountType": {
                "id": 0,
                "value": "AccountType.saving",
                "code": "Saving"
            },
            "currency": {
                "currencyCode": "USD",
                "currencyDigit": 2,
                "inMultiplesOf": 1
            },
            "clientId": 20
        }
    ]
  }
  ```

- ##### Retrieve a Saving Account

  #### Method example
  `GET /savingsaccounts/{savingAccountId}` 
  #### Method response
    `GET /savingsaccounts/20` 
  ```
  {
    "id": 20,
    "parentId": 0,
    "productId": 1,
    "nickname": "Master Kai",
    "lockinPeriodFrequency": 0,
    "allowOverdraft": false,
    "overdraftLimit": 0.00,
    "minOverdraftForInterestCalculation": 0.00,
    "enforceMinRequiredBalance": false,
    "minRequiredBalance": 0.00,
    "lockinPeriodFrequencyType": 0,
    "status": {
        "id": 200,
        "value": "Approved",
        "code": "savingsAccountStatusType.approved"
    },
    "subStatus": {
        "id": 0,
        "value": "SavingsAccountSubStatus.none",
        "code": "None"
    },
    "accountBalance": 0.00,
    "accountType": {
        "id": 0,
        "value": "AccountType.saving",
        "code": "Saving"
    },
    "currency": {
        "currencyCode": "USD",
        "currencyDigit": 2,
        "inMultiplesOf": 1
    },
    "approveDate": "2024-05-19",
    "clientId": 20
  }
  ```
- ##### Create a Saving Account
  #### Mandatory fields
  `productId`, `nickname`, `locale`, `dateFormat`, `submittedOnDate`, `clientId`
  #### Method example
  `POST /savingsaccounts`
  #### Method request
  ```
  POST /savingsaccounts
  Content-Type: application/json
  Request body:
  {
    "productId": 1,
    "nickname": "Master Kai",
    "locale": "en",
    "dateFormat": "dd MMMM yyyy",
    "submittedOnDate": "19 May 2024",
    "clientId": 20
  }
  ```
  #### Method response
  ```
  {
    "id":20
  }
  ```
- ##### Update a Saving Account

  #### Method example
  `PUT /savingsaccounts/{savingAccountId}`
  #### Method request
  ```
  PUT /savingsaccounts/20
  Content-Type: application/json
  Request body:

  {
    "nickname": "General Kai",
  }
  ```
  #### Method response
  ```
  {
    "id":20
  }
  ```
- ##### Approve/Activate/Reject/Close a Saving Account
  #### Method example
  `POST /savingsaccounts/2?command=approve`  
  `POST /savingsaccounts/2?command=activate`  
  `POST /savingsaccounts/2?command=reject`  
  #### Method request
  ```
  POST /savingsaccounts/2?command=approve
  Content-Type: application/json
  Request body: 

  {
    "approvedOnDate": "19 May 2024",
    "locale": "en",
    "dateFormat": "dd MMMM yyyy" 
  }

  ```
  ```
  POST /savingsaccounts/2?command=activate
  Content-Type: application/json
  Request body: 
  {
    "activateOnDate": "19 May 2024",
    "locale": "en",
    "dateFormat": "dd MMMM yyyy" 
  }

  ```
  ```
  POST /savingsaccounts/2?command=reject
  Content-Type: application/json
  Request body: 
  {
    "rejectOnDate": "19 May 2024",
    "locale": "en",
    "dateFormat": "dd MMMM yyyy" 
  }

  ```
  #### Method response
  ```
  {
    "id":20
  }
  ```
#### Transactions
- ##### List Saving Account Transactions
  #### Optional arguments
  `page` - integer optional, defaults to 0.  
  - Indicates the result from which pagination starts.

  `size` - integer optional, defaults to 10.  
  - Limits the size of results returned. To override the default and return all entries you must explicitly pass a non-positive integer value for limit e.g. size=0, or size=1  
  
  `orderBy` - string optional  
  - one of id. Orders results by the indicated field.  
  
  `sortDirection` - string optional, one of ASC, DESC.  
  - Indicates what way to order results if orderBy is used.  

  `endDate` date, optional: 
  - Filters transactions with an end date. 

  `startDate` date, optional
  - Filters transactions with a start date.  

  `transactionType` integer, optional
  - Filters transactions by type.  

  `subTransactionType` integer, optional
  - Filters transactions by sub-type.  
  #### Method example
  `clients/1/transactions?size=3&page=0&sortDirection=DESC&endDate=2024-05-19&startDate=2024-01-01&transactionType=1&subTransactionType=2`  
  #### Method response
  ```
  {
    "size": 3,
    "page": 0,
    "totalItems": 6,
    "totalPages": 2,
    "data": [
        {
            "id": 6,
            "amount": 100.00,
            "accountId": 20,
            "accountNo": "000000020",
            "currency": {
                "currencyCode": "USD",
                "currencyDigit": 2,
                "inMultiplesOf": 1
            },
            "transactionDate": "2024-05-19",
            "transactionType": {
                "id": 2,
                "value": "Withdrawal",
                "code": "savingsAccountTransactionType.withdrawal"
            },
            "transactionSubtype": "NONE",
            "status": "PROCESSED",
            "availableBalanceDerived": 1100.00,
            "reversed": false,
            "runningBalance": 1000.00,
            "bookingDate": "2024-05-19",
            "createdBy": null,
            "initiatedAt": "2024-05-19T17:09:51.998077",
            "transferData": null
        },
        {
            "id": 5,
            "amount": 300.00,
            "accountId": 20,
            "accountNo": "000000020",
            "currency": {
                "currencyCode": "USD",
                "currencyDigit": 2,
                "inMultiplesOf": 1
            },
            "transactionDate": "2024-05-19",
            "transactionType": {
                "id": 1,
                "value": "Deposit",
                "code": "savingsAccountTransactionType.deposit"
            },
            "transactionSubtype": "NONE",
            "status": "PROCESSED",
            "availableBalanceDerived": 800.00,
            "reversed": false,
            "runningBalance": 1100.00,
            "bookingDate": "2024-05-19",
            "createdBy": null,
            "initiatedAt": "2024-05-19T17:09:44.093994",
            "transferData": null
        },
        {
            "id": 4,
            "amount": 200.00,
            "accountId": 20,
            "accountNo": "000000020",
            "currency": {
                "currencyCode": "USD",
                "currencyDigit": 2,
                "inMultiplesOf": 1
            },
            "transactionDate": "2024-05-19",
            "transactionType": {
                "id": 1,
                "value": "Deposit",
                "code": "savingsAccountTransactionType.deposit"
            },
            "transactionSubtype": "NONE",
            "status": "PROCESSED",
            "availableBalanceDerived": 600.00,
            "reversed": false,
            "runningBalance": 800.00,
            "bookingDate": "2024-05-19",
            "createdBy": null,
            "initiatedAt": "2024-05-19T17:09:40.496437",
            "transferData": null
        }
    ]
  }
  ```
- ##### Deposit Balance
  #### Method example
  `POST /savingsaccounts/20/transactions?command=deposit`
  #### Method request
  ```
  POST /savingsaccounts/20/transactions?command=deposit
  Content-Type: application/json
  Request body:
  
  {
    "dateFormat": "dd MMMM yyyy",
    "locale": "en",
    "paymentTypeId": 6,
    "transactionDate": "19 May 2024",
    "transactionAmount": "300",
    "reference": "just need sample data"
  }
  ```
  #### Method response
  ```
  {
    "officeId": 1,
    "clientId": 10,
    "savingsId": 20,
    "resourceId": 232242,
    "changes": {
        "reference": "Hello"
    }
  }
  ```
- ##### Withdraw Balance
  #### Method example
  `POST /savingsaccounts/20/transactions?command=withdrawal`
  #### Method request
  ```
  POST /savingsaccounts/20/transactions?command=withdrawal
  Content-Type: application/json
  Request body:

  {
    "dateFormat": "dd MMMM yyyy",
    "locale": "en",
    "paymentTypeId": 6,
    "transactionDate": "19 May 2024",
    "transactionAmount": "200",
    "reference": "just need sample data"
  }
  ```
  #### Method response
  ```
  {
    "officeId": 1,
    "clientId": 10,
    "savingsId": 20,
    "resourceId": 232242,
    "changes": {
        "reference": "Hello world"
    }
  }
  ```

- ##### Generate Account Statement
  There are 2 output type of account statement, PDF and CSV
  #### Method example
  `/savingsaccounts/reports?output-type=PDF&savingAccountId=17&period=ThisMonth`  
  `/savingsaccounts/reports?output-type=PDF&savingAccountId=17&period=Last3Month`  
  `/savingsaccounts/reports?output-type=PDF&savingAccountId=17&period=Last6Month`  

  `/savingsaccounts/reports?output-type=CSV&savingAccountId=17&period=Last3Month`  
### Fund Transfer Service Endpoints
#### Fund Transfer
- ##### Internal Transfer
  #### Mandatory fields
  `type`, `paymentType`, `amount`, `debtor`, `credtor`, `reference`
  #### Method example
  `POST /transfers`
  #### Method request
  ```
  POST /transfers
  Content-Type: application/json
  Request body:
  
  {
    "type": "CREDIT",
    "paymentType": "INTERNAL",
    "amount": 200,
    "debtor": {
        "identifier": "id:10" //sender
    },
    "creditor": {
        "identifier": "id:8" //receiver
    },
    "reference": [
        "test"
    ]
  }
  ```
  #### Method response
  ```
  {
    "clientId": 20,
    "savingsId": 10,
    "resourceId": 64,
    "resourceIdentifier": "1714578714172vN",
    "data": {
        "amount": 200
    }
  }
  ```
<div style="text-align: right;">
  Last Updated: 19/5/2024, 2:13:40 PM
</div>


  <!-- #### Optional arguments
  #### Mandatory fields
  #### Method example
  #### Method request
  #### Method response


  POST /users/3?command=enable
  Content-Type: application/json
  Request body:

  POST /users/3?command=enable
  Content-Type: application/json
  No Request Body

 -->

