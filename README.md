# DesignApi
## The Exercise

We are building a neighbourhood collaboration site and we want to make a system to keep track of people, houses, and addresses of those houses.

- Each person has a name, age and number of people in their household
- Each house has an address and an owner
- Each address has a postcode and street address

The REST API will need to enable users to:

- Store people, houses and addresses
- Look up a house, its address and owner
- Look up people in our neighbourhood within certain age brackets and with specific household sizes


### Task 1

Consider the type of data we will be storing and therefore the type of database we should implement (SQL vs NoSQL)

#### Our Considerations

Because the data we are handling is structured and will involve joining database tables together, we elected to go for a relational database. This will allows us to link each residents to a household which will be linked to an address. See our proposed schema below.


### Task 2

Create a schema for this database

#### Our Schema

![DesignAPISchema](https://user-images.githubusercontent.com/81855619/170238488-eef95606-396a-45c6-8190-93ff82b1575e.png)

 **Please note the arrows are meant to be double headed**


**Possible updates**
- Could add “isDestroyed” to household -- Land to build a household on
- Could add “maxHouseholdCapacity” to household -- to check wether we can authorise additonal resident in an household


### Task 3

Consider the requests our API should be capable of handling

#### Requests our API should handle

REST API needs to…

- Get household
- Get address
- Get owner
- Get people in our neighborhood within age brackets and house size (??)

- Post person/:id to house 
- Post house (multiple houses?)

- Delete owner (change of ownership)
- Delete person from household (braving the outside world)
- Delete household (natural disaster, demolished, act of god)

- Patch/put address (move house?)
- Patch/put owner
- Patch/put person (change name or pronouns)

### Task 4

List the routes you will need with their HTTP verb and path

#### Our Route Table

 Our steps:
 
- Step 1: Write query e.g., 
`SELECT residents.name, residents.age, address.street_address
FROM residents, households, addresses
WHERE residents.age > 20 AND
residents.age < 40 AND
count(households.resident_id) < 10`

 - Step 2: create route to fire the query
 
 - Step 3: http formatted with params
 
`/cats?age=:ageNumber&temperement=:cuddlyNumber`



|Route                                       |         Verb                   |           Action                    |
| --- | --- | --- |
|/households/                                |          GET                   | Information about all households    |
|/households/id:                             |          GET                   | Information about specific house    |
|/people/                                    |          GET                   | Information about all people        |
|/people/id                                  |          GET                   | Information about individual        |
|/people?age=ageBrackets&householdSize=size  |          GET                   | List with age+size confinements     |
|/people/id                                  |          POST                  | Create a new person                 |
|/households/                                |          POST                  | Create a new house                  |
|/owner/:id                                  |          DELETE                | Remove owner_id, isOwner= false     |
|/people/:id                                 |          DELETE                | Remove resident record, resident_ID |
|/household/:id                              |          DELETE                | Remove id from address table + ID   |
|/address/:id/edit                           |          PATCH/PUT             | Update address                      |
|/Owner/:id/edit                             |          PATCH/PUT             | update owner                        |
|/person/:id/edit                            |          PATCH/PUT             | update person                       |

### Task 5 

Determine the responses that should be returned and the content types of these requests and responses

#### Responses

|           Get             | No Content | Work |
| --- | --- | --- |
|Get Household              | 404        | 200  |
|Get address                | 404        | 200  |
|Get Owner                  | 404        | 200  |
|get people within age+size | 404        | 200  |


|          Post            | No Content | Work |
| --- | --- | --- |
|Post person                | 404        | 201  |
|Post house                 | 404        | 201  |


|         Delete           | No Content | Unauthorized | Bad Credentials | Work |
| --- | --- | --- | --- | --- |
|Delete Owner               | 404        | 200          | 403             | 200  |
|Delete Person              | 404        | 200          | 403             | 200  |
|Delete household           | 404        | 200          | 403             | 202  |


|          Patch           | No Content | Work |
| --- | --- | --- |
|Patch/Put address          | 404        | 200  |
|Patch/Put owner            | 404        | 200  |
|Patch/Put person           | 404        | 202  |
